# NPC Water Orb + Player Inventory (Keep-Ostriching, Godot 4.7)

## What we build

- An NPC stands in the World. A player presses **R** (P2: **O**) near it and gets a water orb in a 3-slot inventory.
- The player presses **Q** (P2: **P**) to drink an orb, which gives **+30 water**.
- The server decides everything, the same pattern as your Cactus.

> Not run in Godot by me. If a red error appears, paste it to me.

---

## Step 1: Export the sprite sheet as PNG

Your sheet is 96×32, which is 3 frames of 32×32. Speed is 4 FPS.

1. Open `assets/art/characters/NPC/NPC-Animation-Sheet.aseprite` in Aseprite.
2. Go to **File → Export As…** and save it as `NPC-Animation-Sheet.png` in the same folder.
3. In Godot, check that the PNG imported (96×32).

---

## Step 2: Add input actions

Go to **Project → Project Settings → Input Map**. For each row: type the name in "Add New Action", click **Add**, click **+**, press the key, click **OK**.

| Action | Key |
|---|---|
| `p1_interact` | R |
| `p1_drink` | Q |
| `p2_interact` | O |
| `p2_drink` | P |

---

## Step 3: Edit `scripts/game_state.gd`

**3a.** Under your other signals add:

```gdscript
signal orbs_changed(player_id, count)
```

**3b.** Under `const HALLUCINATION_DURATION := 10` add:

```gdscript
const MAX_ORBS := 3
const ORB_WATER_GAIN := 30.0
```

**3c.** In `register_player`, replace the dictionary with:

```gdscript
	players[player_id] = {
		"water": MAX_WATER,
		"health": MAX_HEALTH,
		"hallucinating": false,
		"dead": false,
		"orbs": 0
	}
```

**3d.** Add these two functions at the bottom of the file:

```gdscript
func add_orb(player_id: int) -> bool:
	if not players.has(player_id):
		return false
	var p = players[player_id]
	if p.orbs >= MAX_ORBS:
		return false          # inventory full
	p.orbs += 1
	emit_signal("orbs_changed", player_id, p.orbs)
	return true


func use_orb(player_id: int) -> bool:
	if not players.has(player_id):
		return false
	var p = players[player_id]
	if p.orbs <= 0 or p.water >= MAX_WATER:
		return false          # nothing to drink, or no need to
	p.orbs -= 1
	emit_signal("orbs_changed", player_id, p.orbs)
	add_water(player_id, ORB_WATER_GAIN)
	return true
```

---

## Step 4: Create the Inventory scene

1. In the FileSystem dock, right-click the folder `scenes/ui` → **Create New → Scene**.
2. Choose **Other Node → HBoxContainer** and name it `Inventory`. Save as `res://scenes/ui/Inventory.tscn`.
3. In the Inspector, go to **Theme Overrides → Constants → Separation** and set it to `2`.
4. Add exactly **3 ColorRect** children named `Slot1`, `Slot2`, `Slot3`.
5. Select **each** slot and set **Layout → Custom Minimum Size** to x = `8`, y = `8`.
6. Right-click `Inventory` → **Attach Script**. Path: `res://scenes/ui/inventory.gd`. Paste:

```gdscript
extends HBoxContainer

const EMPTY_COLOR := Color(0.2, 0.2, 0.2, 0.8)
const FULL_COLOR := Color(0.2, 0.6, 1.0, 1.0)

# Colors the first `count` slots blue, the rest dark gray.
func set_orbs(count: int) -> void:
	for i in get_child_count():
		var slot := get_child(i) as ColorRect
		slot.color = FULL_COLOR if i < count else EMPTY_COLOR


func _ready() -> void:
	set_orbs(0)
```

> The number of slots (3) must match `GameState.MAX_ORBS`.

---

## Step 5: Put the Inventory on the Player

### In `Player.tscn`

1. Right-click the root `Player` → **Instantiate Child Scene…** → `Inventory.tscn`.
2. Select it → **Layout → Transform → Position**: x = `-14`, y = `-66`. Nudge it if it overlaps your bars.

### In `scenes/player/player.gd`

**5a.** Under `@onready var camera: Camera2D = $Camera` add:

```gdscript
@onready var inventory: HBoxContainer = $Inventory
var nearby_npc: Node = null   # NPC.gd sets this while the player is in its zone
```

**5b.** Replace the `ONLINE_INPUT` constant with:

```gdscript
const ONLINE_INPUT := {
	"left": "p1_left", "right": "p1_right", "jump": "p1_jump",
	"plunge": "p1_plunge", "kick": "p1_kick", "slide": "p1_slide",
	"interact": "p1_interact", "drink": "p1_drink"
}
```

**5c.** Replace the `net_input` line with:

```gdscript
var net_input := {"dir": 0.0, "jump": false, "kick": false, "plunge": false, "slide": false, "interact": false, "drink": false}
```

**5d.** In `_ready()`, after the line `GameState.water_changed.connect(...)` add:

```gdscript
	GameState.orbs_changed.connect(_on_orbs_changed)
```

At the very end of `_ready()`, after `water_bar.value = ...`, add:

```gdscript
	inventory.set_orbs(GameState.players[player_id].orbs)
```

**5e.** Replace `_client_stats` and add the new handler under it:

```gdscript
@rpc("authority", "call_remote", "unreliable_ordered")
func _client_stats(h: float, w: float, orbs: int) -> void:
	health_bar.value = h
	water_bar.value = w
	inventory.set_orbs(orbs)


func _on_orbs_changed(id: int, count: int) -> void:
	if id != player_id:
		return
	inventory.set_orbs(count)
```

**5f.** In `_read_local_input`, the returned dictionary must end like this:

```gdscript
		"slide": Input.is_action_pressed(input_source["slide"]),
		"interact": Input.is_action_just_pressed(input_source["interact"]),
		"drink": Input.is_action_just_pressed(input_source["drink"])
```

**5g.** In `_apply_input`, add at the end:

```gdscript
	net_input["interact"] = net_input["interact"] or inp["interact"]
	net_input["drink"] = net_input["drink"] or inp["drink"]
```

**5h.** In `_physics_process`, change the stats RPC line to:

```gdscript
			_client_stats.rpc_id(pid, health_bar.value, water_bar.value, GameState.players[player_id].orbs)
```

**5i.** In `_physics_process`, paste this just **above** the line `# --- State machine ---`:

```gdscript
	# --- Take an orb from the NPC / drink an orb ---
	if net_input["interact"] and nearby_npc and nearby_npc.has_method("interact"):
		nearby_npc.interact(player_id)
	net_input["interact"] = false

	if net_input["drink"]:
		GameState.use_orb(player_id)
	net_input["drink"] = false
```

---

## Step 6: Edit `main_single.gd` and `main_double.gd`

Their `input_source` dictionaries need the new keys, or the game crashes with a "key not found" error.

**`main_single.gd`**

```gdscript
	player1.input_source = {
		"left": "p1_left", "right": "p1_right", "jump": "p1_jump",
		"plunge": "p1_plunge", "kick": "p1_kick", "slide": "p1_slide",
		"interact": "p1_interact", "drink": "p1_drink"
	}
```

**`main_double.gd`**: do the same for `player1`, and for `player2` add these two entries:

```gdscript
		"interact": "p2_interact", "drink": "p2_drink"
```

`main_online.gd` needs **no change** (it uses `ONLINE_INPUT`).

---

## Step 7: Build the NPC scene (`scenes/NPC/NPC.tscn`)

1. Open `NPC.tscn`. Select the root, press **F2**, and rename it `NPC`.
2. In the Inspector → **Collision**: **uncheck Layer 1** (so kicks ignore the NPC). Keep **Mask 1** checked (that is how it sees players).
3. Add a child **AnimatedSprite2D**:
   - Inspector → **Sprite Frames → New SpriteFrames**, then click it to open the bottom panel.
   - Rename `default` to `idle`.
   - Click the **Add Frames from Sprite Sheet** icon → pick `NPC-Animation-Sheet.png` → **Horizontal = 3**, **Vertical = 1** → select all 3 frames → **Add 3 Frame(s)**.
   - Set **FPS = 4** and turn **Loop** on.
4. Add a child **CollisionShape2D**: **Shape → New RectangleShape2D**, **Size** x = `72`, y = `40`.
5. Add a child **Label** and name it `Prompt`:
   - **Text**: `R / O : take orb`
   - **Horizontal Alignment**: Center
   - **Layout → Transform**: Size x = `80`, y = `12`; Position x = `-40`, y = `-38`
6. Right-click `NPC` → **Attach Script**. Path: `res://scenes/NPC/npc.gd`. Paste:

```gdscript
extends Area2D

@export var orb_cooldown: float = 5.0   # seconds between orbs
var cooldown_left: float = 0.0

@onready var anim: AnimatedSprite2D = $AnimatedSprite2D
@onready var prompt: Label = $Prompt


func _ready() -> void:
	anim.play("idle")
	prompt.visible = false


func _physics_process(delta: float) -> void:
	if cooldown_left > 0.0:
		cooldown_left = max(cooldown_left - delta, 0.0)

	# Show the prompt whenever at least one Player overlaps the zone.
	# Re-checking every frame means a freed player can never leave it stuck on.
	var someone_here := false
	for b in get_overlapping_bodies():
		if is_instance_valid(b) and b is Player:
			someone_here = true
			break
	prompt.visible = someone_here


func _on_body_entered(body: Node2D) -> void:
	if is_instance_valid(body) and body is Player:
		body.nearby_npc = self


func _on_body_exited(body: Node2D) -> void:
	# The body may already be freed (player died or fell), so check first.
	if is_instance_valid(body) and body is Player and body.nearby_npc == self:
		body.nearby_npc = null


# Called by player.gd on the server only.
func interact(player_id: int) -> void:
	if not multiplayer.is_server():
		return
	if cooldown_left > 0.0:
		return
	if GameState.add_orb(player_id):
		cooldown_left = orb_cooldown
		print("NPC gave a water orb to player ", player_id)
```

7. Select the root `NPC` → **Node** tab (next to Inspector) → **Signals** → double-click `body_entered` → **Connect**. Do the same for `body_exited`. The method names `_on_body_entered` / `_on_body_exited` fill in automatically.

---

## Step 8: Place the NPC in the World

1. Open `scenes/levels/World.tscn`.
2. Drag `NPC.tscn` from the FileSystem dock onto the root `World` node.
3. Set **Position** to x = `500`, y = `154` (ground top is y = 170 and the sprite is 32 px tall, so its center is 154).
4. Run **MainSingle**. Walk to the NPC, the prompt shows, press **R** and one slot turns blue. Once the Water bar drops below max, press **Q** to drink.

Online needs no extra spawner: the NPC lives inside `World`, so it exists on every peer with the same path.

---

## Math corner

- Animation loop = frames ÷ FPS = 3 ÷ 4 = **0.75 s**
- One orb = +30 water. Drain is 1/s, so that is **30 seconds of life** per orb.
- Full inventory = 3 × 30 = **90 seconds** of reserve.
- A 5 s cooldown allows at most 60 ÷ 5 = **12 orbs per minute** (raise `orb_cooldown` if it feels too easy).
- Zone width 72 px → 72 ÷ 2 = **36 px** each side of the NPC's center.

---

## If something breaks

| Error / symptom | Fix |
|---|---|
| `Invalid access to property 'orbs'` | Check Step 3c. |
| `Invalid get index 'interact'` | An `input_source` dictionary (Step 6) misses the new keys. |
| Prompt never appears | NPC Mask 1 must be on and both signals connected (Step 7.7). |
| Orb doesn't show online | Step 5h must pass all 3 values. |

---

## Mentor's reminder

Read one chapter of *Game Programming Patterns* by Robert Nystrom this week: the **Observer** chapter explains signals like `orbs_changed`. I can't search or check a library from here, so double-check the title and chapter name yourself.
