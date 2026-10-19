# Implemented All remaining features!
## Step 0: One helper in `scripts/net_manager.gd`

Add this function anywhere in the file (for example above `close()`):

```gdscript
# Reveal a freshly spawned node (bullet, eagle...) to every peer that already loaded the world.
# Same job as _share() in world.gd. Offline, ready_peers is empty, so this does nothing.
func share(node: Node) -> void:
	var sync := node.get_node_or_null("MultiplayerSynchronizer") as MultiplayerSynchronizer
	if sync == null:
		return
	for pid in ready_peers:
		sync.set_visibility_for(pid, true)
```

---

# A. Ghost on death

**Idea:** a dead Player is no longer `queue_free()`'d. It stays alive in the tree, turns semi-transparent, loses all collisions, and flies in 4 directions. Because it stays in the tree, camera and online sync keep working.

### A1. Edit `scenes/player/player.gd`

**1) Add these variables** right after the `var stats_timer := 0.0` line:

```gdscript
@export var ghost_speed: float = 160.0

# Replicated by the MultiplayerSynchronizer (set up in A2). The setter runs on every
# peer when the value changes, so the look of the ghost is the same on every screen.
var is_ghost := false:
	set(value):
		is_ghost = value
		_apply_ghost_visuals()
```

**2) Add `"up"` to `net_input`.** Change the dictionary to:

```gdscript
var net_input := {"dir": 0.0, "jump": false, "kick": false, "plunge": false, "slide": false, "interact": false, "drink": false, "give": false, "up": false}
```

**3) In `_read_local_input()`**, add one line inside the returned dictionary (the ghost flies up with your jump key, down with your plunge key):

```gdscript
		"up": Input.is_action_pressed(input_source["jump"]),
```

**4) In `_apply_input()`**, add at the end:

```gdscript
	net_input["up"] = inp["up"]
```

**5) At the very end of `_ready()`**, add one line (for a ghost that arrives through the spawn packet):

```gdscript
	_apply_ghost_visuals()
```

**6) Change the first statement of `_physics_process`** (the `give_hint.visible = (...)` block) to:

```gdscript
	give_hint.visible = (
		not is_ghost
		and not input_source.is_empty()
		and orb_count > 0
		and _nearest_friend() != null
	)
```

**7) Replace the death block.** Find:

```gdscript
	if GameState.players[player_id].dead or abs(position.y) > 320:
		print("Ostrich died!")
		queue_free()
		return
```

Replace it with:

```gdscript
	if not is_ghost and (GameState.players[player_id].dead or abs(position.y) > 320):
		_become_ghost()
	if is_ghost:
		_ghost_move()
		return
```

**8) Paste these 3 new functions** at the bottom of the file:

```gdscript
# Server only. Runs once, when this Ostrich dies (health 0 or fell in the void).
func _become_ghost() -> void:
	print("Ostrich died! Now a ghost")
	# Void deaths never touched GameState.health, so mark dead here too.
	GameState.players[player_id].dead = true
	current_state = State.IDLE
	plunge_timer = 0.0
	position.y = min(position.y, 100.0)   # came back from the void: lift the ghost above the ground
	is_ghost = true                        # setter -> visuals here, synchronizer -> every client
	GameState._check_all_dead()            # all dead = game over, same as before


# Runs on EVERY peer (called by the is_ghost setter).
func _apply_ghost_visuals() -> void:
	if not is_node_ready():
		return                             # value arrived before _ready, _ready calls us again
	if not is_ghost:
		return
	modulate.a = 0.4                       # whole Player + children semi-transparent
	# set_deferred: this can be called while physics is busy
	set_deferred("collision_layer", 0)     # nothing can touch me (cactus, NPC, snakes, bullets)
	set_deferred("collision_mask", 0)      # I touch nothing (ground, snakes, eagle)
	hitbox.set_deferred("collision_layer", 0)
	hitbox.set_deferred("monitoring", false)
	remove_from_group("players")           # snakes stop chasing me, friends can't give me orbs
	health_bar.hide()
	water_bar.hide()
	inventory.hide()
	give_hint.hide()


# Server only. Free flight: no gravity, no kick/slide/orbs.
func _ghost_move() -> void:
	var dir := Vector2(
		net_input["dir"],
		float(net_input["plunge"]) - float(net_input["up"])
	)
	velocity = dir.normalized() * ghost_speed
	move_and_slide()
	# Throw away presses made while dead so they don't fire later.
	net_input["jump"] = false
	net_input["kick"] = false
	net_input["slide"] = false
	net_input["interact"] = false
	net_input["drink"] = false
	net_input["give"] = false
```

### A2. Replicate `is_ghost` (editor, no code)

1. Open `scenes/player/Player.tscn`.
2. Click the **MultiplayerSynchronizer** node.
3. Bottom panel → **Replication** tab → **Add property to sync** → pick the **Player** root node (`.`) → choose `is_ghost` → OK.
4. On the new `is_ghost` row: tick **Spawn**, and set the Replicate dropdown to **On Change**.

### A3. Edit `scenes/net/main_online.gd`

Players are no longer freed, so the "alive" count must look at the dead flag. In `_physics_process`, replace the loop:

```gdscript
	var alive := 0
	for p in players.get_children():
		if not p.is_queued_for_deletion():
			alive += 1
```

with:

```gdscript
	var alive := 0
	for p in players.get_children():
		var st = GameState.players.get(p.player_id)
		if not p.is_queued_for_deletion() and st != null and not st.dead:
			alive += 1
```

### A4. Edit `scenes/main/main_double.gd`

In `_process`, **delete** these two blocks (player.gd handles void deaths now, and `main_single` already has this commented out):

```gdscript
	if player1 and abs(player1.position.y) > 320:
		print("Player 1 jumped and died")
		player1.queue_free()
		player1 = null
	if player2 and abs(player2.position.y) > 320:
		print("Player 2 jumped and died")
		player2.queue_free()
		player2 = null
```

### Test A

Run `MainSingle`, jump off the world (or lose health to 0). The ostrich should become see-through, lose its bars, and fly with **W / S / A / D**. Walk it through a snake and the cactus. Nothing should happen. When every player is a ghost, the Game Over menu should appear.

---

# B. Saharan viper shoots, no biting

Rattlesnake keeps biting. Viper only shoots.

### B1. Create `scenes/enemy/Bullet.tscn`

1. **Scene → New Scene → Other Node →** `Area2D`. Rename it **Bullet**.
2. Inspector → **Collision**: Layer: untick everything (0). Mask: tick **only layer 1**. (Mask 1 sees the player Hitbox and the ground.)
3. Add child **CollisionShape2D** → Shape: **New CircleShape2D**, Radius `4`.
4. Add child **ColorRect** → Size `8 x 8`, Position `-4, -4`, Color: bright yellow.
5. Add child **MultiplayerSynchronizer** → in the Inspector set **Public Visibility = off**. Replication tab → add `.:position`, tick **Spawn**, mode **Always**. (Same setup as the Enemy.)
6. Save as `res://scenes/enemy/Bullet.tscn`.
7. Right-click **Bullet** → **Attach Script** → path `res://scenes/enemy/bullet.gd` → paste:

```gdscript
extends Area2D

@export var speed: float = 260.0
@export var damage: int = 10
@export var lifetime: float = 2.0

# Set by enemy.gd BEFORE add_child. Only the server needs it:
# clients just display the position the synchronizer sends.
var direction: Vector2 = Vector2.RIGHT


func _ready() -> void:
	area_entered.connect(_on_area_entered)
	body_entered.connect(_on_body_entered)


func _physics_process(delta: float) -> void:
	if not multiplayer.is_server():
		return
	position += direction * speed * delta
	lifetime -= delta
	if lifetime <= 0.0:
		queue_free()


# The Player's Hitbox (Area2D, layer 1) touched us.
func _on_area_entered(area: Area2D) -> void:
	if not multiplayer.is_server():
		return
	var player := area.get_parent() as Player
	if player == null:
		return
	GameState.change_health(player.player_id, -damage)
	queue_free()


# Ground (StaticBody2D) stops the bullet. The player's body also triggers
# this signal, so we ignore everything that isn't a StaticBody2D.
func _on_body_entered(body: Node2D) -> void:
	if not multiplayer.is_server():
		return
	if body is StaticBody2D:
		queue_free()
```

### B2. Edit `scenes/enemy/enemy.gd`

**1) Add near the top**, under the `enum`:

```gdscript
const BULLET_SCENE: PackedScene = preload("res://scenes/enemy/Bullet.tscn")
@export var shoot_range: float = 260.0
@export var keep_distance: float = 120.0   # viper stops this far from the target
var chase_range: float = 100.0             # rattlesnake: old behaviour (100)
```

**2) In `_ready()`**, extend the `match` and add the group line:

```gdscript
	add_to_group("enemies")   # the eagle looks for snakes in this group
	match enemy_type:
		EnemyType.RATTLESNAKE:
			speed = 120.0
			chase_range = 100.0
			$ColorRect.modulate = Color(0.6, 0.8, 0.3)
		EnemyType.SAHARAN_VIPER:
			speed = 160.0
			chase_range = 260.0
			modulate = Color(0.9, 0.7, 0.2)
```

(Keep your `health_bar.max_value...` lines after the match.)

**3) Replace the whole `_physics_process`** with:

```gdscript
func _physics_process(delta: float) -> void:
	health_bar.value = current_health
	if not multiplayer.is_server():
		return

	attack_timer = max(attack_timer - delta, 0.0)
	var target: CharacterBody2D = _get_nearest_player()

	# Rattlesnake bites, viper shoots. Never both.
	if enemy_type == EnemyType.RATTLESNAKE:
		_try_bite()
	elif target != null:
		_try_shoot(target)

	if not is_on_floor():
		velocity.y += gravity * delta

	if target == null:
		move_and_slide()
		return

	var direction: Vector2 = (target.global_position - global_position).normalized()
	velocity.x = direction.x * speed
	var dx: float = abs(target.global_position.x - global_position.x)

	# Viper keeps its distance so it can shoot instead of walking into you.
	if enemy_type == EnemyType.SAHARAN_VIPER and dx < keep_distance:
		velocity.x = 0

	if dx >= chase_range:
		velocity.x = 0
		idle_timer += delta
		if idle_timer >= idle_lifetime:
			idle_timer = 0
			queue_free()
			print("Snake vaporized!")
			return
	else:
		idle_timer = 0
	move_and_slide()
```

**4) Add the shoot function** at the bottom:

```gdscript
func _try_shoot(target: CharacterBody2D) -> void:
	if attack_timer > 0.0:
		return
	if global_position.distance_to(target.global_position) > shoot_range:
		return

	var bullet: Area2D = BULLET_SCENE.instantiate()
	bullet.name = "Bullet" + str(Time.get_ticks_usec())   # unique names are required online
	# Everything is set BEFORE add_child, so the spawn packet carries the position.
	bullet.position = global_position
	bullet.direction = (target.global_position - global_position).normalized()

	# Online: the "bullet_root" node (made in B3) holds bullets. Offline: put it next to the snake.
	var root: Node = get_tree().get_first_node_in_group("bullet_root")
	if root == null:
		root = get_parent()
	root.add_child(bullet)
	NetManager.share(bullet)

	attack_timer = attack_cooldown
	print("Viper shot a bullet!")
```

### B3. Online scene setup, `scenes/net/MainOnline.tscn`

1. Under the root **MainOnline**, add a **Node2D**, rename it **Bullets**.
2. Select **Bullets** → Inspector → Node tab → **Groups** → type `bullet_root` → Add.
3. Add a **MultiplayerSpawner**, rename it **BulletSpawner**: **Spawn Path** = `../Bullets`; under **Auto Spawn List** add `res://scenes/enemy/Bullet.tscn`; **Spawn Limit = 0** (a limit of 2 would cap your bullets).

### Test B

Offline: let a viper spawn (the world timer makes one every 12 s, type is random). It should stop about 120 px away and fire a yellow square every 2 s. Rattlesnakes should still bite and never shoot.

---

# C. Eagle that kills snakes

**Idea:** every Player gets one eagle pet that hovers above them. When a snake is within range of the Player, the eagle dives at it, one-shots it (`_take_damage(9999)`), then flies back and waits 3 s. If the owner becomes a ghost, the eagle disappears. Only the server moves the eagle, and clients just see the synced position.

### C1. Create `scenes/eagle/Eagle.tscn`

1. **New Scene → Other Node →** `Node2D`. Rename it **Eagle**.
2. Add child **ColorRect** → Size `24 x 12`, Position `-12, -6`, Color: brown.
3. Add child **Area2D**, rename it **Talons** → Inspector → **Collision**: Layer: untick all. Mask: tick **only layer 2** (the snake's Hitbox sits on layer 2). Under Talons, add **CollisionShape2D** → **New RectangleShape2D**, Size `24 x 20`.
4. Add child **MultiplayerSynchronizer** → **Public Visibility = off** → Replication tab → add `.:position`, tick **Spawn**, mode **Always**.
5. Save as `res://scenes/eagle/Eagle.tscn`.
6. Right-click **Eagle** → **Attach Script** → `res://scenes/eagle/eagle.gd` → paste:

```gdscript
class_name Eagle
extends Node2D

enum State { FOLLOW, ATTACK }
var state: State = State.FOLLOW

# Set by player.gd on the server before add_child. (Not called "owner":
# every Node already has a built-in `owner` property.)
var leader: Player = null
var target: Enemy = null

@export var follow_speed: float = 220.0
@export var attack_speed: float = 380.0
@export var detect_range: float = 260.0            # snake must be this close to the LEADER
@export var hover_offset: Vector2 = Vector2(0, -60)
@export var attack_cooldown: float = 3.0
var cooldown_timer: float = 0.0

@onready var talons: Area2D = $Talons


func _physics_process(delta: float) -> void:
	# Clients only display. The synchronizer delivers the position.
	if not multiplayer.is_server():
		return

	# Owner gone or turned ghost -> eagle leaves.
	if leader == null or not is_instance_valid(leader) or leader.is_ghost:
		queue_free()
		return

	cooldown_timer = max(cooldown_timer - delta, 0.0)

	match state:
		State.FOLLOW:
			_fly_towards(leader.global_position + hover_offset, follow_speed, delta)
			if cooldown_timer == 0.0:
				target = _find_target()
				if target != null:
					state = State.ATTACK
		State.ATTACK:
			if target == null or not is_instance_valid(target) or target.is_queued_for_deletion():
				_end_attack()
				return
			_fly_towards(target.global_position, attack_speed, delta)
			# Talons touching a snake's Hitbox -> one-shot kill.
			for area in talons.get_overlapping_areas():
				var snake := area.get_parent() as Enemy
				if snake:
					snake._take_damage(9999)
					print("Eagle killed a snake!")
					_end_attack()
					return


func _fly_towards(dest: Vector2, speed: float, delta: float) -> void:
	global_position = global_position.move_toward(dest, speed * delta)


func _end_attack() -> void:
	target = null
	state = State.FOLLOW
	cooldown_timer = attack_cooldown


# Nearest snake within detect_range of the leader (not of the eagle).
func _find_target() -> Enemy:
	var best: Enemy = null
	var best_d: float = detect_range * detect_range
	for n in get_tree().get_nodes_in_group("enemies"):
		var e := n as Enemy
		if e == null or e.is_queued_for_deletion():
			continue
		var d: float = leader.global_position.distance_squared_to(e.global_position)
		if d < best_d:
			best_d = d
			best = e
	return best
```

### C2. Edit `scenes/player/player.gd`, spawn the eagle

**1) Add at the top**, under the other `@onready` lines:

```gdscript
const EAGLE_SCENE: PackedScene = preload("res://scenes/eagle/Eagle.tscn")
```

**2) At the end of `_ready()`** (after the `_apply_ghost_visuals()` line from A1) add:

```gdscript
	if multiplayer.is_server():
		_spawn_eagle()
```

**3) Paste at the bottom of the file:**

```gdscript
# Server only (offline counts as server). Online, clients get the eagle from the EagleSpawner.
func _spawn_eagle() -> void:
	var eagle: Eagle = EAGLE_SCENE.instantiate()
	eagle.name = "Eagle" + str(player_id)    # unique per player (online names must be unique)
	eagle.leader = self
	eagle.position = global_position + Vector2(0, -60)

	var root: Node = get_tree().get_first_node_in_group("eagle_root")
	if root == null:
		root = get_parent()                  # offline: next to the player
	root.add_child(eagle)
	NetManager.share(eagle)
```

### C3. Online scene setup, `scenes/net/MainOnline.tscn`

1. Add a **Node2D** under **MainOnline**, rename it **Eagles**, and add it to group `eagle_root` (Node tab → Groups).
2. Add a **MultiplayerSpawner**, rename it **EagleSpawner**: **Spawn Path** = `../Eagles`; **Auto Spawn List** → add `res://scenes/eagle/Eagle.tscn`; **Spawn Limit = 0**.

### C4. Edit `scenes/net/main_online.gd`

Add `eagles` to the "newcomer must see everything" list, and the node ref:

```gdscript
@onready var eagles: Node2D = $Eagles
```

and in `_on_player_ready` change the loop line to:

```gdscript
	for container in [players, enemies, props, eagles]:
```

### Test C

Run `MainSingle`: a brown bar should hover above your ostrich. When a snake spawns within 260 px of you, the eagle should dive, the snake should vanish ("Eagle killed a snake!" in Output), then the eagle returns. Die, and the eagle should disappear.

---

## Final online check

Host in one window, join in another. Verify, **on the client's screen**: the client's ghost is semi-transparent, the viper's bullets move, and the eagle flies. If something is invisible only on the client, the usual cause is a missed step in the **MultiplayerSynchronizer** setup (Spawn ticked? Public Visibility off? Property added?) or a forgotten **Spawner** in MainOnline.

A caveat: I could not run Godot here, so I haven't tested any of this. If something errors, paste the Output panel message and I'll fix it.

One thing to know: `world.gd` never assigns `min_pos` / `max_pos`, so every snake currently spawns at `(0, 0)`. That is not from my changes, but you'll notice vipers appearing in the same spot.Checking how the cactus and NPC detect players, so ghosts don't trigger them, then I'll write the steps.

I read your whole project (player, enemy, world, main_online, net_manager, game_state), so everything below is written against your actual code. Three parts, in this order: **A. Ghost → B. Viper bullets → C. Eagle**. Do them in order, because the Bullet and Eagle code reuses a helper from Step 0.

---

## Step 0: One helper in `scripts/net_manager.gd`

Add this function anywhere in the file (for example above `close()`):

```gdscript
# Reveal a freshly spawned node (bullet, eagle...) to every peer that already loaded the world.
# Same job as _share() in world.gd. Offline, ready_peers is empty, so this does nothing.
func share(node: Node) -> void:
	var sync := node.get_node_or_null("MultiplayerSynchronizer") as MultiplayerSynchronizer
	if sync == null:
		return
	for pid in ready_peers:
		sync.set_visibility_for(pid, true)
```

---

# A. Ghost on death

**Idea:** a dead Player is no longer `queue_free()`'d. It stays alive in the tree, turns semi-transparent, loses all collisions, and flies in 4 directions. Because it stays in the tree, camera and online sync keep working.

### A1. Edit `scenes/player/player.gd`

**1) Add these variables** right after the `var stats_timer := 0.0` line:

```gdscript
@export var ghost_speed: float = 160.0

# Replicated by the MultiplayerSynchronizer (set up in A2). The setter runs on every
# peer when the value changes, so the look of the ghost is the same on every screen.
var is_ghost := false:
	set(value):
		is_ghost = value
		_apply_ghost_visuals()
```

**2) Add `"up"` to `net_input`.** Change the dictionary to:

```gdscript
var net_input := {"dir": 0.0, "jump": false, "kick": false, "plunge": false, "slide": false, "interact": false, "drink": false, "give": false, "up": false}
```

**3) In `_read_local_input()`**, add one line inside the returned dictionary (the ghost flies up with your jump key, down with your plunge key):

```gdscript
		"up": Input.is_action_pressed(input_source["jump"]),
```

**4) In `_apply_input()`**, add at the end:

```gdscript
	net_input["up"] = inp["up"]
```

**5) At the very end of `_ready()`**, add one line (for a ghost that arrives through the spawn packet):

```gdscript
	_apply_ghost_visuals()
```

**6) Change the first statement of `_physics_process`** (the `give_hint.visible = (...)` block) to:

```gdscript
	give_hint.visible = (
		not is_ghost
		and not input_source.is_empty()
		and orb_count > 0
		and _nearest_friend() != null
	)
```

**7) Replace the death block.** Find:

```gdscript
	if GameState.players[player_id].dead or abs(position.y) > 320:
		print("Ostrich died!")
		queue_free()
		return
```

Replace it with:

```gdscript
	if not is_ghost and (GameState.players[player_id].dead or abs(position.y) > 320):
		_become_ghost()
	if is_ghost:
		_ghost_move()
		return
```

**8) Paste these 3 new functions** at the bottom of the file:

```gdscript
# Server only. Runs once, when this Ostrich dies (health 0 or fell in the void).
func _become_ghost() -> void:
	print("Ostrich died! Now a ghost")
	# Void deaths never touched GameState.health, so mark dead here too.
	GameState.players[player_id].dead = true
	current_state = State.IDLE
	plunge_timer = 0.0
	position.y = min(position.y, 100.0)   # came back from the void: lift the ghost above the ground
	is_ghost = true                        # setter -> visuals here, synchronizer -> every client
	GameState._check_all_dead()            # all dead = game over, same as before


# Runs on EVERY peer (called by the is_ghost setter).
func _apply_ghost_visuals() -> void:
	if not is_node_ready():
		return                             # value arrived before _ready, _ready calls us again
	if not is_ghost:
		return
	modulate.a = 0.4                       # whole Player + children semi-transparent
	# set_deferred: this can be called while physics is busy
	set_deferred("collision_layer", 0)     # nothing can touch me (cactus, NPC, snakes, bullets)
	set_deferred("collision_mask", 0)      # I touch nothing (ground, snakes, eagle)
	hitbox.set_deferred("collision_layer", 0)
	hitbox.set_deferred("monitoring", false)
	remove_from_group("players")           # snakes stop chasing me, friends can't give me orbs
	health_bar.hide()
	water_bar.hide()
	inventory.hide()
	give_hint.hide()


# Server only. Free flight: no gravity, no kick/slide/orbs.
func _ghost_move() -> void:
	var dir := Vector2(
		net_input["dir"],
		float(net_input["plunge"]) - float(net_input["up"])
	)
	velocity = dir.normalized() * ghost_speed
	move_and_slide()
	# Throw away presses made while dead so they don't fire later.
	net_input["jump"] = false
	net_input["kick"] = false
	net_input["slide"] = false
	net_input["interact"] = false
	net_input["drink"] = false
	net_input["give"] = false
```

### A2. Replicate `is_ghost` (editor, no code)

1. Open `scenes/player/Player.tscn`.
2. Click the **MultiplayerSynchronizer** node.
3. Bottom panel → **Replication** tab → **Add property to sync** → pick the **Player** root node (`.`) → choose `is_ghost` → OK.
4. On the new `is_ghost` row: tick **Spawn**, and set the Replicate dropdown to **On Change**.

### A3. Edit `scenes/net/main_online.gd`

Players are no longer freed, so the "alive" count must look at the dead flag. In `_physics_process`, replace the loop:

```gdscript
	var alive := 0
	for p in players.get_children():
		if not p.is_queued_for_deletion():
			alive += 1
```

with:

```gdscript
	var alive := 0
	for p in players.get_children():
		var st = GameState.players.get(p.player_id)
		if not p.is_queued_for_deletion() and st != null and not st.dead:
			alive += 1
```

### A4. Edit `scenes/main/main_double.gd`

In `_process`, **delete** these two blocks (player.gd handles void deaths now, and `main_single` already has this commented out):

```gdscript
	if player1 and abs(player1.position.y) > 320:
		print("Player 1 jumped and died")
		player1.queue_free()
		player1 = null
	if player2 and abs(player2.position.y) > 320:
		print("Player 2 jumped and died")
		player2.queue_free()
		player2 = null
```

### Test A

Run `MainSingle`, jump off the world (or lose health to 0). The ostrich should become see-through, lose its bars, and fly with **W / S / A / D**. Walk it through a snake and the cactus. Nothing should happen. When every player is a ghost, the Game Over menu should appear.

---

# B. Saharan viper shoots, no biting

Rattlesnake keeps biting. Viper only shoots.

### B1. Create `scenes/enemy/Bullet.tscn`

1. **Scene → New Scene → Other Node →** `Area2D`. Rename it **Bullet**.
2. Inspector → **Collision**: Layer: untick everything (0). Mask: tick **only layer 1**. (Mask 1 sees the player Hitbox and the ground.)
3. Add child **CollisionShape2D** → Shape: **New CircleShape2D**, Radius `4`.
4. Add child **ColorRect** → Size `8 x 8`, Position `-4, -4`, Color: bright yellow.
5. Add child **MultiplayerSynchronizer** → in the Inspector set **Public Visibility = off**. Replication tab → add `.:position`, tick **Spawn**, mode **Always**. (Same setup as the Enemy.)
6. Save as `res://scenes/enemy/Bullet.tscn`.
7. Right-click **Bullet** → **Attach Script** → path `res://scenes/enemy/bullet.gd` → paste:

```gdscript
extends Area2D

@export var speed: float = 260.0
@export var damage: int = 10
@export var lifetime: float = 2.0

# Set by enemy.gd BEFORE add_child. Only the server needs it:
# clients just display the position the synchronizer sends.
var direction: Vector2 = Vector2.RIGHT


func _ready() -> void:
	area_entered.connect(_on_area_entered)
	body_entered.connect(_on_body_entered)


func _physics_process(delta: float) -> void:
	if not multiplayer.is_server():
		return
	position += direction * speed * delta
	lifetime -= delta
	if lifetime <= 0.0:
		queue_free()


# The Player's Hitbox (Area2D, layer 1) touched us.
func _on_area_entered(area: Area2D) -> void:
	if not multiplayer.is_server():
		return
	var player := area.get_parent() as Player
	if player == null:
		return
	GameState.change_health(player.player_id, -damage)
	queue_free()


# Ground (StaticBody2D) stops the bullet. The player's body also triggers
# this signal, so we ignore everything that isn't a StaticBody2D.
func _on_body_entered(body: Node2D) -> void:
	if not multiplayer.is_server():
		return
	if body is StaticBody2D:
		queue_free()
```

### B2. Edit `scenes/enemy/enemy.gd`

**1) Add near the top**, under the `enum`:

```gdscript
const BULLET_SCENE: PackedScene = preload("res://scenes/enemy/Bullet.tscn")
@export var shoot_range: float = 260.0
@export var keep_distance: float = 120.0   # viper stops this far from the target
var chase_range: float = 100.0             # rattlesnake: old behaviour (100)
```

**2) In `_ready()`**, extend the `match` and add the group line:

```gdscript
	add_to_group("enemies")   # the eagle looks for snakes in this group
	match enemy_type:
		EnemyType.RATTLESNAKE:
			speed = 120.0
			chase_range = 100.0
			$ColorRect.modulate = Color(0.6, 0.8, 0.3)
		EnemyType.SAHARAN_VIPER:
			speed = 160.0
			chase_range = 260.0
			modulate = Color(0.9, 0.7, 0.2)
```

(Keep your `health_bar.max_value...` lines after the match.)

**3) Replace the whole `_physics_process`** with:

```gdscript
func _physics_process(delta: float) -> void:
	health_bar.value = current_health
	if not multiplayer.is_server():
		return

	attack_timer = max(attack_timer - delta, 0.0)
	var target: CharacterBody2D = _get_nearest_player()

	# Rattlesnake bites, viper shoots. Never both.
	if enemy_type == EnemyType.RATTLESNAKE:
		_try_bite()
	elif target != null:
		_try_shoot(target)

	if not is_on_floor():
		velocity.y += gravity * delta

	if target == null:
		move_and_slide()
		return

	var direction: Vector2 = (target.global_position - global_position).normalized()
	velocity.x = direction.x * speed
	var dx: float = abs(target.global_position.x - global_position.x)

	# Viper keeps its distance so it can shoot instead of walking into you.
	if enemy_type == EnemyType.SAHARAN_VIPER and dx < keep_distance:
		velocity.x = 0

	if dx >= chase_range:
		velocity.x = 0
		idle_timer += delta
		if idle_timer >= idle_lifetime:
			idle_timer = 0
			queue_free()
			print("Snake vaporized!")
			return
	else:
		idle_timer = 0
	move_and_slide()
```

**4) Add the shoot function** at the bottom:

```gdscript
func _try_shoot(target: CharacterBody2D) -> void:
	if attack_timer > 0.0:
		return
	if global_position.distance_to(target.global_position) > shoot_range:
		return

	var bullet: Area2D = BULLET_SCENE.instantiate()
	bullet.name = "Bullet" + str(Time.get_ticks_usec())   # unique names are required online
	# Everything is set BEFORE add_child, so the spawn packet carries the position.
	bullet.position = global_position
	bullet.direction = (target.global_position - global_position).normalized()

	# Online: the "bullet_root" node (made in B3) holds bullets. Offline: put it next to the snake.
	var root: Node = get_tree().get_first_node_in_group("bullet_root")
	if root == null:
		root = get_parent()
	root.add_child(bullet)
	NetManager.share(bullet)

	attack_timer = attack_cooldown
	print("Viper shot a bullet!")
```

### B3. Online scene setup, `scenes/net/MainOnline.tscn`

1. Under the root **MainOnline**, add a **Node2D**, rename it **Bullets**.
2. Select **Bullets** → Inspector → Node tab → **Groups** → type `bullet_root` → Add.
3. Add a **MultiplayerSpawner**, rename it **BulletSpawner**: **Spawn Path** = `../Bullets`; under **Auto Spawn List** add `res://scenes/enemy/Bullet.tscn`; **Spawn Limit = 0** (a limit of 2 would cap your bullets).

### Test B

Offline: let a viper spawn (the world timer makes one every 12 s, type is random). It should stop about 120 px away and fire a yellow square every 2 s. Rattlesnakes should still bite and never shoot.

---

# C. Eagle that kills snakes

**Idea:** every Player gets one eagle pet that hovers above them. When a snake is within range of the Player, the eagle dives at it, one-shots it (`_take_damage(9999)`), then flies back and waits 3 s. If the owner becomes a ghost, the eagle disappears. Only the server moves the eagle, and clients just see the synced position.

### C1. Create `scenes/eagle/Eagle.tscn`

1. **New Scene → Other Node →** `Node2D`. Rename it **Eagle**.
2. Add child **ColorRect** → Size `24 x 12`, Position `-12, -6`, Color: brown.
3. Add child **Area2D**, rename it **Talons** → Inspector → **Collision**: Layer: untick all. Mask: tick **only layer 2** (the snake's Hitbox sits on layer 2). Under Talons, add **CollisionShape2D** → **New RectangleShape2D**, Size `24 x 20`.
4. Add child **MultiplayerSynchronizer** → **Public Visibility = off** → Replication tab → add `.:position`, tick **Spawn**, mode **Always**.
5. Save as `res://scenes/eagle/Eagle.tscn`.
6. Right-click **Eagle** → **Attach Script** → `res://scenes/eagle/eagle.gd` → paste:

```gdscript
class_name Eagle
extends Node2D

enum State { FOLLOW, ATTACK }
var state: State = State.FOLLOW

# Set by player.gd on the server before add_child. (Not called "owner":
# every Node already has a built-in `owner` property.)
var leader: Player = null
var target: Enemy = null

@export var follow_speed: float = 220.0
@export var attack_speed: float = 380.0
@export var detect_range: float = 260.0            # snake must be this close to the LEADER
@export var hover_offset: Vector2 = Vector2(0, -60)
@export var attack_cooldown: float = 3.0
var cooldown_timer: float = 0.0

@onready var talons: Area2D = $Talons


func _physics_process(delta: float) -> void:
	# Clients only display. The synchronizer delivers the position.
	if not multiplayer.is_server():
		return

	# Owner gone or turned ghost -> eagle leaves.
	if leader == null or not is_instance_valid(leader) or leader.is_ghost:
		queue_free()
		return

	cooldown_timer = max(cooldown_timer - delta, 0.0)

	match state:
		State.FOLLOW:
			_fly_towards(leader.global_position + hover_offset, follow_speed, delta)
			if cooldown_timer == 0.0:
				target = _find_target()
				if target != null:
					state = State.ATTACK
		State.ATTACK:
			if target == null or not is_instance_valid(target) or target.is_queued_for_deletion():
				_end_attack()
				return
			_fly_towards(target.global_position, attack_speed, delta)
			# Talons touching a snake's Hitbox -> one-shot kill.
			for area in talons.get_overlapping_areas():
				var snake := area.get_parent() as Enemy
				if snake:
					snake._take_damage(9999)
					print("Eagle killed a snake!")
					_end_attack()
					return


func _fly_towards(dest: Vector2, speed: float, delta: float) -> void:
	global_position = global_position.move_toward(dest, speed * delta)


func _end_attack() -> void:
	target = null
	state = State.FOLLOW
	cooldown_timer = attack_cooldown


# Nearest snake within detect_range of the leader (not of the eagle).
func _find_target() -> Enemy:
	var best: Enemy = null
	var best_d: float = detect_range * detect_range
	for n in get_tree().get_nodes_in_group("enemies"):
		var e := n as Enemy
		if e == null or e.is_queued_for_deletion():
			continue
		var d: float = leader.global_position.distance_squared_to(e.global_position)
		if d < best_d:
			best_d = d
			best = e
	return best
```

### C2. Edit `scenes/player/player.gd`, spawn the eagle

**1) Add at the top**, under the other `@onready` lines:

```gdscript
const EAGLE_SCENE: PackedScene = preload("res://scenes/eagle/Eagle.tscn")
```

**2) At the end of `_ready()`** (after the `_apply_ghost_visuals()` line from A1) add:

```gdscript
	if multiplayer.is_server():
		_spawn_eagle()
```

**3) Paste at the bottom of the file:**

```gdscript
# Server only (offline counts as server). Online, clients get the eagle from the EagleSpawner.
func _spawn_eagle() -> void:
	var eagle: Eagle = EAGLE_SCENE.instantiate()
	eagle.name = "Eagle" + str(player_id)    # unique per player (online names must be unique)
	eagle.leader = self
	eagle.position = global_position + Vector2(0, -60)

	var root: Node = get_tree().get_first_node_in_group("eagle_root")
	if root == null:
		root = get_parent()                  # offline: next to the player
	root.add_child(eagle)
	NetManager.share(eagle)
```

### C3. Online scene setup, `scenes/net/MainOnline.tscn`

1. Add a **Node2D** under **MainOnline**, rename it **Eagles**, and add it to group `eagle_root` (Node tab → Groups).
2. Add a **MultiplayerSpawner**, rename it **EagleSpawner**: **Spawn Path** = `../Eagles`; **Auto Spawn List** → add `res://scenes/eagle/Eagle.tscn`; **Spawn Limit = 0**.

### C4. Edit `scenes/net/main_online.gd`

Add `eagles` to the "newcomer must see everything" list, and the node ref:

```gdscript
@onready var eagles: Node2D = $Eagles
```

and in `_on_player_ready` change the loop line to:

```gdscript
	for container in [players, enemies, props, eagles]:
```

### Test C

Run `MainSingle`: a brown bar should hover above your ostrich. When a snake spawns within 260 px of you, the eagle should dive, the snake should vanish ("Eagle killed a snake!" in Output), then the eagle returns. Die, and the eagle should disappear.

---

## Final online check

Host in one window, join in another. Verify, **on the client's screen**: the client's ghost is semi-transparent, the viper's bullets move, and the eagle flies. If something is invisible only on the client, the usual cause is a missed step in the **MultiplayerSynchronizer** setup (Spawn ticked? Public Visibility off? Property added?) or a forgotten **Spawner** in MainOnline.

A caveat: I could not run Godot here, so I haven't tested any of this. If something errors, paste the Output panel message and I'll fix it.

One thing to know: `world.gd` never assigns `min_pos` / `max_pos`, so every snake currently spawns at `(0, 0)`. That is not from my changes, but you'll notice vipers appearing in the same spot.