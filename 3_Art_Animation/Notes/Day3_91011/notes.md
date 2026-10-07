# DAY 9 — Networking Foundations I: Host, Join & the Multiplayer Peer

## NOTES

### 🔤 GDScript
- **`-> Error` return type**: functions can return the engine's error enum (`OK`, `ERR_*`). Lets the UI show "port busy?" instead of silently failing.
- **`signal join_failed` / `signal player_ready(id)`**: custom signals. `join_failed` tells the menu to re-enable its buttons; `player_ready` bridges "new player's screen is loaded" → "spawn their Ostrich."
- **`IP.get_local_addresses()`**: all network addresses of this machine; filtering for `192.168.` / `10.` finds your LAN IP to give a friend.
- **`:=` type inference** and one-line ifs — small syntax wins today.

### 🎮 Godot
- **`ENetMultiplayerPeer`**: the transport (UDP socket wrapper). `create_server(port, max_clients)` listens; `create_client(ip, port)` dials. Returns an error if it fails.
- **`multiplayer.multiplayer_peer = peer`**: THE plug-in moment. `multiplayer` exists on every node always (in singleplayer it holds an `OfflineMultiplayerPeer`). This one line connects your whole tree to the network.
- **Peer IDs**: server is always **1**; clients get 2, 3, 4... assigned by the engine on connect.
- **Multiplayer signals on `multiplayer`**: `peer_connected(id)`, `peer_disconnected(id)`, `connection_failed`, `server_disconnected` — hooked up once in the Autoload.
- **`multiplayer.is_server()`**: almost every networked decision checks this.
- **Autoload**: `NetManager` is created before any scene and never freed — the connection must survive menu → world → game over → menu.
- **`%IpInput`**: unique-name access to a node marked with the "unique name" icon.

### 📐 Math
Optional: latency = round-trip time in ms; packets arrive at a rate (events/sec). Godot syncs ~20×/sec by default — foreshadows Day 10's interpolation.

## DOUBTS
1. If `NetManager` were a child of the menu scene instead of an Autoload, what happens the moment `change_scene_to_file` runs — exactly, and why?
2. Why must the host check `is_server()` inside `_on_peer_connected`?
3. `create_server()` erroring is a different failure from a client connecting later and failing. Which signal fires for each, and why can't they be handled the same way?
4. Your friend types your IP but the port is firewalled. Which signal fires on THEIR machine? On yours?

## CODE, IN THE ORDER YOU WROTE IT

**1) `project.godot` — Autoload registration**
```ini
[autoload]

GameState="*uid://bfyjr1o4k3fa"
NetManager="*uid://b00aqb8qxktep"
```

**2) `scripts/net_manager.gd`**
```gdscript
extends Node

signal join_failed
signal player_ready(id: int)

const PORT := 7777
const MAX_CLIENTS := 4  # host + 4 = 5 players
const ONLINE_SCENE := "res://scenes/net/MainOnline.tscn"
const MENU_SCENE := "res://scenes/ui/StartMenu.tscn"

var ready_peers: Array[int] = []


func _ready() -> void:
	multiplayer.peer_connected.connect(_on_peer_connected)
	multiplayer.peer_disconnected.connect(_on_peer_disconnected)
	multiplayer.connection_failed.connect(_on_connection_failed)
	multiplayer.server_disconnected.connect(_on_server_disconnected)


func host() -> Error:
	close()
	var peer := ENetMultiplayerPeer.new()
	var err := peer.create_server(PORT, MAX_CLIENTS)
	if err != OK:
		return err
	multiplayer.multiplayer_peer = peer
	get_tree().change_scene_to_file(ONLINE_SCENE)  # host enters the world immediately
	return OK


func join(ip: String) -> Error:
	close()
	var peer := ENetMultiplayerPeer.new()
	var err := peer.create_client(ip, PORT)
	if err != OK:
		return err
	multiplayer.multiplayer_peer = peer
	return OK


func close() -> void:
	ready_peers.clear()
	multiplayer.multiplayer_peer = null


func _on_peer_connected(id: int) -> void:
	# Host only: tell ONLY the newcomer to load the world.
	if multiplayer.is_server():
		start_online_game.rpc_id(id)


func _on_peer_disconnected(id: int) -> void:
	ready_peers.erase(id)


@rpc("authority", "reliable")
func start_online_game() -> void:
	get_tree().change_scene_to_file(ONLINE_SCENE)


@rpc("any_peer", "reliable")
func client_ready() -> void:
	if not multiplayer.is_server():
		return
	var id := multiplayer.get_remote_sender_id()
	if id not in ready_peers:
		ready_peers.append(id)
	player_ready.emit(id)


func _on_connection_failed() -> void:
	close()
	join_failed.emit()


func _on_server_disconnected() -> void:
	close()
	get_tree().change_scene_to_file(MENU_SCENE)
```

**3) `scenes/ui/online/online_menu.gd`** (scene: CanvasLayer + `%IpInput` LineEdit, `%HostButton`/`%JoinButton`/`%BackButton`, `%StatusLabel`)
```gdscript
extends CanvasLayer

@export var menu_scene: PackedScene = preload("res://scenes/ui/StartMenu.tscn")

@onready var ip_input: LineEdit = %IpInput
@onready var host_button: Button = %HostButton
@onready var join_button: Button = %JoinButton
@onready var back_button: Button = %BackButton
@onready var status_label: Label = %StatusLabel


func _ready() -> void:
	host_button.pressed.connect(_on_host_pressed)
	join_button.pressed.connect(_on_join_pressed)
	back_button.pressed.connect(_on_back_pressed)
	NetManager.join_failed.connect(_on_join_failed)
	status_label.text = ""

func _on_host_pressed() -> void:
	_set_buttons_disabled(true)
	var err := NetManager.host()
	if err != OK:
		status_label.text = "Could not host (port busy?)"
		_set_buttons_disabled(false)

func _on_join_pressed() -> void:
	var ip := ip_input.text.strip_edges()
	if ip.is_empty():
		ip = "127.0.0.1"
	_set_buttons_disabled(true)
	var err := NetManager.join(ip)
	if err != OK:
		status_label.text = "Invalid address"
		_set_buttons_disabled(false)
		return
	status_label.text = "Connecting..."


func _on_join_failed() -> void:
	status_label.text = "Could not connect"
	_set_buttons_disabled(false)


func _on_back_pressed() -> void:
	NetManager.close()
	get_tree().change_scene_to_packed(menu_scene)


func _set_buttons_disabled(value: bool) -> void:
	host_button.disabled = value
	join_button.disabled = value


func _get_lan_ip() -> String:
	for addr in IP.get_local_addresses():
		if addr.begins_with("192.168.") or addr.begins_with("10."):
			return addr
	return "127.0.0.1"
```

---

# DAY 10 — Networking Foundations II: Spawning Players & Syncing Movement

## NOTES

### 🔤 GDScript
- **`@rpc("authority", "reliable")` etc.**: declares a function callable over the network. The annotation is a contract — who may call it, whether the caller also runs it, delivery guarantees.
- **`rpc_id(target_id, args)`**: targeted RPC — runs ONLY on that peer. Plain `rpc()` runs on all remote peers.
- **`inp != last_sent`**: send input only when it changes, not every physics tick.
- **`name.is_valid_int()` / `name.to_int()`**: every Node has a built-in `name`. You store the peer ID in it at spawn (`p.name = str(id)`), so the Ostrich recovers its own ID — no editor export needed online.
- **Input latching with `or`**: `net_input["jump"] = net_input["jump"] or inp["jump"]` — one-shot presses survive until the server consumes them; a press between ticks can't be lost.
- **`as MultiplayerSynchronizer`**: cast so the engine knows which class you're calling methods on.

### 🎮 Godot
- **Transfer modes**: `reliable` (guaranteed, ordered — join/kick/game-over) vs `unreliable_ordered` (may drop, order kept — fine for per-frame state). Never `reliable` for 60Hz position.
- **`multiplayer.get_unique_id()`**: my own peer ID → "is this Ostrich mine?" → enable camera, read my keyboard.
- **`multiplayer.get_remote_sender_id()`**: inside an `any_peer` RPC, who actually called. Security check — `_send_input` rejects anyone controlling someone else's Ostrich.
- **Server-authoritative**: clients NEVER move bodies. They send input; only the server's `_physics_process` runs gravity/state/`move_and_slide()`; results flow back via replication.
- **`MultiplayerSynchronizer` + `SceneReplicationConfig`**: automatic property replication. List properties; owner broadcasts changes at a set rate; receivers write them into their copy. No manual RPC.
- **`public_visibility = false` + `set_visibility_for(id, true)`**: stop broadcast; only listed peers get updates. Critical for late joiners.
- **The handshake**: `peer_connected` → host `rpc_id(newcomer, start_online_game)` → newcomer's world `_ready` → `rpc_id(1, client_ready)` → host records in `ready_peers` → spawns Player named `"<id>"` → enables visibility for all ready peers.

### 📐 Math
- **Interpolation (`Vector2.lerp`)**: sync arrives ~20×/sec, rendering at 60fps → remote Ostriches "step." Smooth with `visual = visual.lerp(target, 1 - exp(-delta * rate))`. If remote players look choppy, this is your missing piece.

## DOUBTS
1. A jump press lands exactly between two physics ticks and the dictionary hasn't changed — walk through why latching is needed. What breaks with plain assignment?
2. The synchronizer could sync `velocity` directly. Why is syncing `position` from the server MORE trustworthy?
3. `_spawn_player` sets `p.name = str(id)` BEFORE `add_child`. Why does order matter?
4. A late joiner sees no enemies. Name three places the visibility loop could be failing.

## CODE, IN THE ORDER YOU WROTE IT

**1) `scenes/net/main_online.gd`**
```gdscript
extends Node

const PLAYER_SCENE := preload("res://scenes/player/Player.tscn")
const MAX_PLAYERS := 5
const SPAWN_POSITION := Vector2(320, 80)

@onready var players: Node2D = $Players
@onready var world: Node2D = $World
@onready var enemies: Node2D = $Enemies
@onready var props: Node2D = $Props

var game_started := false
var game_over_shown := false


func _ready() -> void:
	world.enemy_parent = enemies
	world.prop_parent = props

	if multiplayer.is_server():
		NetManager.player_ready.connect(_on_player_ready)
		multiplayer.peer_disconnected.connect(_on_peer_disconnected)
		_spawn_player(1)
		game_started = true   # only count deaths once the host's Ostrich exists
	else:
		NetManager.client_ready.rpc_id(1)


func _physics_process(_delta: float) -> void:
	# Only the server decides that the game is over.
	if not multiplayer.is_server() or not game_started or game_over_shown:
		return
	var alive := 0
	for p in players.get_children():
		if not p.is_queued_for_deletion():
			alive += 1
	if alive == 0:
		game_over_shown = true
		show_game_over.rpc()


# Runs on the server AND on every client.
@rpc("authority", "call_local", "reliable")
func show_game_over() -> void:
	game_over_shown = true
	var menu := world.get_node_or_null("GameOverMenu")
	if menu == null:
		push_warning("GameOverMenu not found under World")
		return
	get_tree().paused = true
	menu.visible = true


func _on_player_ready(id: int) -> void:
	if players.get_child_count() >= MAX_PLAYERS:
		return
	# The newcomer must see everything that already exists.
	for container in [players, enemies, props]:
		for node in container.get_children():
			var sync := node.get_node_or_null("MultiplayerSynchronizer") as MultiplayerSynchronizer
			if sync:
				sync.set_visibility_for(id, true)
	_spawn_player(id)


func _spawn_player(id: int) -> void:
	if players.has_node(str(id)):
		return
	var p := PLAYER_SCENE.instantiate()
	p.name = str(id)
	p.position = SPAWN_POSITION
	var sync := p.get_node("MultiplayerSynchronizer") as MultiplayerSynchronizer
	sync.public_visibility = false
	players.add_child(p)
	sync.set_visibility_for(1, true)
	for pid in NetManager.ready_peers:
		sync.set_visibility_for(pid, true)


func _on_peer_disconnected(id: int) -> void:
	var p := players.get_node_or_null(str(id))
	if p:
		p.queue_free()
```

**2) `scenes/player/player.gd`** (the whole file — Day 2's `input_source` pays off here)
```gdscript
class_name Player
extends CharacterBody2D

@onready var hitbox = $Hitbox
@onready var health_bar: ProgressBar = $HealthBar
@onready var water_bar: ProgressBar = $WaterBar
@onready var camera: Camera2D = $Camera

enum State { IDLE, JUMP, PLUNGE, KICK }
var current_state: State = State.IDLE

@export var player_id: int = 1

@export var speed: float = 200.0
@export var gravity: float = 1200.0
@export var jump_velocity: float = -400.0
@export var max_fall_speed: float = 800.0

@export var plunge_explode_threshold: float = 6.0
var plunge_timer: float = 0.0

var ostrich_exploded: bool = false

var hallucination_check_timer := 0.0

@export var kick_duration: float = 0.2
@export var kick_damage: int = 20
@export var kick_friend: int = 5
var kick_timer: float = 0.0

var stats_timer := 0.0

# Online: every window controls ONLY its own Ostrich with these actions.
const ONLINE_INPUT := {
	"left": "p1_left", "right": "p1_right", "jump": "p1_jump",
	"plunge": "p1_plunge", "kick": "p1_kick"
}
# Offline: main_single / main_double fill it. Online: _ready doesn't fills it.
var input_source: Dictionary = {}

# What the server currently believes this player is pressing.
var net_input := {"dir": 0.0, "jump": false, "kick": false, "plunge": false}
var last_sent := {}

func _ready():
	if name.is_valid_int():
		# Online: the node name is the peer id (host = 1).
		player_id = name.to_int()
		var mine := player_id == multiplayer.get_unique_id()
		camera.enabled = mine
		if mine:
			input_source = ONLINE_INPUT.duplicate()
			camera.make_current()
	add_to_group("players")
	GameState.register_player(player_id)
	GameState.hallucination_started.connect(_on_hallucination_started)
	GameState.health_changed.connect(_on_health_changed)
	GameState.water_changed.connect(_on_water_changed)
	health_bar.max_value = GameState.MAX_HEALTH
	health_bar.value = GameState.players[player_id].health
	water_bar.max_value = GameState.MAX_WATER
	water_bar.value = GameState.players[player_id].water


# Server -> one specific client. Updates the mirrored values.
@rpc("authority", "call_remote", "unreliable_ordered")
func _client_stats(h: float, w: float) -> void:
	health_bar.value = h
	water_bar.value = w

func _on_hallucination_started(id: int, duration: float) -> void:
	if id != player_id:
		return
	await get_tree().create_timer(duration).timeout
	GameState.end_hallucination(player_id)

func _on_health_changed(id: int, new_health: float, max_health: float) -> void:
	if id != player_id:
		return
	health_bar.value = new_health
	if new_health <= 0.0:
		ostrich_exploded = true

func _on_water_changed(id: int, new_water: float, max_water: float) -> void:
	if id != player_id:
		return
	water_bar.value = new_water

# ---------- INPUT ----------
func _read_local_input() -> Dictionary:
	return {
		"dir": Input.get_axis(input_source["left"], input_source["right"]),
		"jump": Input.is_action_just_pressed(input_source["jump"]),
		"kick": Input.is_action_just_pressed(input_source["kick"]),
		"plunge": Input.is_action_pressed(input_source["plunge"]),
	}

# One-shot presses (jump/kick) are latched until the server consumes them,
# so a press can't be lost between two physics ticks.
func _apply_input(inp: Dictionary) -> void:
	net_input["dir"] = inp["dir"]
	net_input["plunge"] = inp["plunge"]
	net_input["jump"] = net_input["jump"] or inp["jump"]
	net_input["kick"] = net_input["kick"] or inp["kick"]

# Client -> server. Only the owner of this Ostrich may control it.
@rpc("any_peer", "call_remote", "reliable")
func _send_input(inp: Dictionary) -> void:
	if not multiplayer.is_server():
		return
	if multiplayer.get_remote_sender_id() != player_id:
		return
	_apply_input(inp)

# True only when a real network connection exists (host or client).
# Offline play uses Godot's default OfflineMultiplayerPeer.
func _is_online() -> bool:
	return not (multiplayer.multiplayer_peer is OfflineMultiplayerPeer)

# ---------- SIMULATION ----------
func _physics_process(delta: float) -> void:
	# 1) Whoever owns this Ostrich reads their own keyboard.
	if not input_source.is_empty():
		var inp := _read_local_input()
		if multiplayer.is_server():
			_apply_input(inp)
		elif inp != last_sent:
			last_sent = inp
			_send_input.rpc_id(1, inp)

	# 2) Only the server simulates. Clients just display synced positions.
	if not multiplayer.is_server():
		return

	stats_timer += delta
	if stats_timer >= 0.1 and _is_online():
		stats_timer = 0.0
		for pid in NetManager.ready_peers:
			_client_stats.rpc_id(pid, health_bar.value, water_bar.value)
	if ostrich_exploded or abs(position.y) > 320:
		print("Ostrich died!")
		queue_free()
		return
	GameState.drain_water(player_id, delta)

	# --- Horizontal ---
	var raw_dir: float = net_input["dir"]
	var horizontal_input := raw_dir
	if GameState.players[player_id].hallucinating:
		horizontal_input = -horizontal_input   # flip direction while hallucinating
	velocity.x = horizontal_input * speed

	# --- Vertical: gravity ---
	velocity.y += gravity * delta
	velocity.y = min(velocity.y, max_fall_speed)

	# --- Jump ---
	if net_input["jump"] and is_on_floor():
		velocity.y = jump_velocity
		current_state = State.JUMP
	net_input["jump"] = false

	# --- Kick ---
	if net_input["kick"] and current_state != State.KICK:
		current_state = State.KICK
		kick_timer = 0.0
		hitbox.monitoring = true
	net_input["kick"] = false

	# --- State machine ---
	match current_state:
		State.PLUNGE:
			if not net_input["plunge"]:
				current_state = State.IDLE
			else:
				if raw_dir != 0.0:
					current_state = State.IDLE
				plunge_timer += delta
				if plunge_timer >= plunge_explode_threshold:
					GameState.change_health(player_id, -GameState.MAX_HEALTH)
					print("Ostrich exploded!")
					plunge_timer = 0.0
					current_state = State.IDLE
		State.KICK:
			kick_timer += delta
			if kick_timer >= kick_duration:
				print("Kicked!")
				hitbox.monitoring = false
				current_state = State.IDLE
		_:
			if net_input["plunge"] and is_on_floor():
				current_state = State.PLUNGE

	if is_on_floor() and current_state != State.PLUNGE and current_state != State.KICK:
		current_state = State.IDLE

	move_and_slide()

func get_player_id() -> int:
	return player_id

func _on_hitbox_area_entered(area: Area2D) -> void:
	var enemy := area.get_parent() as Enemy
	if enemy and current_state == State.KICK:
		print(area.name + " Was hit by Ostrichs kick!")
		enemy._take_damage(kick_damage)

	var player := area.get_parent() as Player
	if player and GameState.players[player_id].hallucinating and current_state == State.KICK:
		print(player.name + " has been kicked!")
		GameState.change_health(player.player_id, -kick_friend)
```

**3) `Player.tscn` — the added MultiplayerSynchronizer block**
```ini
[sub_resource type="SceneReplicationConfig" id="SceneReplicationConfig_wr5hl"]
properties/0/path = NodePath(".:position")
properties/0/spawn = true
properties/0/replication_mode = 1
properties/1/path = NodePath("HealthBar:value")
properties/1/spawn = true
properties/1/replication_mode = 1
properties/2/path = NodePath("WaterBar:value")
properties/2/spawn = true
properties/2/replication_mode = 1

[node name="MultiplayerSynchronizer" type="MultiplayerSynchronizer" parent="."]
replication_config = SubResource("SceneReplicationConfig_wr5hl")
public_visibility = false
```

---

# DAY 11 — Merge Local + Network Co-op, Full Playtest

## NOTES

### 🔤 GDScript
- **`is OfflineMultiplayerPeer` check** (`_is_online()`): the cleanest line of the arc. Singleplayer uses Godot's default offline peer — same code runs until you must behave differently (the stats RPC loop).
- **`queue_free()` on disconnect**: server removes the Ostrich; `peer_disconnected` handles cleanup centrally in the world scene.
- **`get_tree().paused = true`**: freezes physics everywhere; combined with `call_local` RPCs, all peers freeze consistently at game over.

### 🎮 Godot
- **One architecture, three modes**: singleplayer, local, online all run the same Player scene — "a player = ID + input source" (Day 2). Online just swaps the input source for a network pipe.
- **Desync diagnosis**: position drift = someone simulating who shouldn't; double events = `call_local` missing/duplicated; missing entities = synchronizer visibility.
- **RPC ordering**: reliable RPCs arrive in order, but sync packets and RPCs use different channels — never assume "kick" and "position" arrive in lockstep.
- **Out of scope for v1 (write down, don't build)**: graceful mid-run disconnect, reconnection with state restore, anti-cheat beyond sender checks, lag compensation, >5 players.

### 📐 Math
None — integration day. Revisit Day 2–7 math if playtesting surfaces weird movement.

## DOUBTS
1. Host pulls their ethernet cable mid-run. What does each client experience, and which signal fires where?
2. Both players kick the same snake on the same frame; the server processes one `_send_input` first. Is that "fair"? Does your code need it to be?
3. Why does the game-over check count alive Ostriches instead of listening for a `died` signal?
4. `show_game_over` uses `call_local`. With `call_remote`, the server's screen never shows the menu — but does the game still end correctly? Explain "simulation ends" vs "UI shows."

## CODE / TASKS, IN ORDER

**1) Additions inside your Day-10 files (already shown above)**
- `player.gd`: `_client_stats` RPC + the `stats_timer` block — server pushes health/water bar values to each client 10×/sec.
- `main_online.gd`: alive-count check in `_physics_process` → `show_game_over.rpc()`, and `_on_peer_disconnected` cleanup.

**2) `scenes/ui/game_over_menu.gd`** (GameOverMenu.tscn: CanvasLayer, `visible = false` by default)
```gdscript
extends CanvasLayer

func _ready() -> void:
	visible = false

func _on_game_over_button_pressed() -> void:
	get_tree().paused = false
	NetManager.close()
	get_tree().change_scene_to_file("res://scenes/ui/StartMenu.tscn")
```

**3) The playtest checklist (no code — run all three modes)**
- [ ] Singleplayer: full loop — move, jump, plunge-explode, kick snake, cactus trade, hallucination flip, water → 0, death → Game Over → restart.
- [ ] Local co-op: both ostriches move independently, camera frames both, kick-each-other works, one dies / both die → correct lose behavior per GDD.
- [ ] Online host: own ostrich spawns; enemies/water behave like singleplayer.
- [ ] Online client: second instance joins via LAN IP; sees host's ostrich + world; controls ONLY their own; host sees theirs.
- [ ] Edge: client joins mid-run — late joiner sees everything, no crash.
- [ ] Edge: client disconnects mid-run — their ostrich vanishes on host, no errors.
- [ ] Edge: host disconnects — clients return to menu via `server_disconnected`.
- [ ] Edge: two kicks same frame; both plunge simultaneously; water hits 0 mid-plunge.
- [ ] `NetManager.close()` on every menu exit path — no zombie servers.

---

📖 Keep reading your game mechanics/engine design book alongside this plan — the client-server authority ideas will click even harder after Day 12's animation sync.