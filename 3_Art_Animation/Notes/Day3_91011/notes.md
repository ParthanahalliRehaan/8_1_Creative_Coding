# Keep-Ostriching — Days 9–11: Become a Godot Networking Expert
**Format:** NOTES → DOUBTS → INSTRUCTIONS (no code — you write every line). One day at a time.

**The one idea behind ALL of networking:**
Your game is not one program. It's two (or more) copies of the same program running on different machines, and they can only communicate by sending **messages**. Everything in these three days — peers, RPCs, synchronizers, servers — is just a cleaner way to send and answer messages. Once you see it this way, nothing in Godot networking will ever feel magical again.

---

# DAY 9 — Two Copies of Your Game, Talking

## NOTES

### The Multiplayer API lives in the engine, not in a node
Here's the first thing that confuses everyone: **there is no "networking node" you add to your scene.** Networking in Godot lives inside a global engine object called `multiplayer`, which every node can reach. You don't build networking by placing special objects — you build it by calling normal functions on your normal nodes, *triggered by network events*.

`multiplayer` has three jobs:
1. Hold the connection (the "peer").
2. Tell you when peers connect/disconnect (signals).
3. Route function calls across machines (RPCs — tomorrow).

### What a peer actually is
A **peer** is one copy of the game. When you play singleplayer, you're a peer with no one to talk to. To go online, you create an `ENetMultiplayerPeer` — think of it as your game's walkie-talkie — and plug it into `multiplayer.multiplayer_peer`. The moment you do that, your island becomes a network.

`ENetMultiplayerPeer` has two modes:
- **`create_server(port)`** — your copy starts *listening* on a port for anyone who wants to join. A port is just a numbered door on your machine (7777 is the convention). That's why it needs no IP — it doesn't go anywhere; it waits.
- **`create_client(ip, port)`** — your copy *knocks on someone's door*: it goes to a specific IP address and port. That's why it needs both.

Under the hood ENet uses **UDP** — the internet's "shout and hope" protocol. UDP packets can be lost, duplicated, or arrive out of order. ENet wraps UDP in a reliability layer: it resends what got lost and fixes the order, so *your* code can pretend the network just works. LAN latency is 1–5ms; internet is 20–150ms+. That's why we test on LAN first.

### Every machine gets an ID. The server is always 1
When copies connect, each one receives a unique integer ID. **The server is always ID 1.** Clients get 2, 3, 4... assigned by the server — the client never picks its own ID, which guarantees no two machines ever share one. An ID is simply how your game says "that copy over there." You'll use IDs everywhere: "send this to peer 2," "is this player node mine or his?"

Two signals start firing once you're connected:
- `peer_connected(id)` — someone joined (the *server* hears this; clients don't hear about each other automatically).
- `peer_disconnected(id)` — someone left.
- Clients also get `connected_to_server` and `connection_failed` — their personal result of knocking on the door.

### Listen server: your host IS the server
There are two server shapes:
- A **dedicated server** is a headless copy of the game that only hosts — no player of its own.
- A **listen server** is what you're building: the host's own game copy *is* the server AND plays in it.

This is the standard choice for small co-op games: one less build to maintain, zero extra cost, and perfectly fine for 2 players.

The question "is this machine the server?" has an answer: `multiplayer.is_server()`. Get used to it — a shocking amount of networked code is just `if multiplayer.is_server(): do the authoritative thing`. Why the server gets special treatment is Day 10's story; for now, accept the rule: **the server is the referee, everyone else plays.**

### Why an Autoload holds your connection
Your connection must exist before any scene loads and must survive scene changes (menu → desert). An **Autoload** is a script Godot instantiates once at startup and never frees — the perfect home for your peer. You'll make `net_manager.gd` an Autoload with two jobs: `host_game(port)` and `join_game(ip, port)`, plus connection logging. Menus call it; scenes trust it.

### Latency is just a rate (the only math today)
When a client presses jump, the message takes ~latency/2 ms to reach the server, the server simulates, and the result takes another ~latency/2 ms to come back. At 100ms round-trip, you see your own jump ~50ms late. Keep this number in your head — Day 10's interpolation and Day 11's "why does remote movement feel delayed" are both this same number wearing different clothes.

## DOUBTS
1. If the server is ID 1, who decides the client's ID — the client or the server? What would break if clients picked their own IDs?
2. `create_server(port)` takes no IP; `create_client(ip, port)` takes both. Explain the asymmetry in your own words — what is "listening" vs "connecting"?
3. UDP can lose and reorder packets. Your "print player connected" test doesn't care — but sort every future message you'll send into two buckets: "if this is lost, interpolation covers it" vs "if this is lost, a real event is missed." What belongs in each bucket?
4. You're on a listen server. When the host presses Kick, no network travel is needed. When a client presses Kick, travel *is* needed. Look at your Day 2–8 input code — where exactly would those two cases first diverge?
5. Two editor instances on one machine — what resource are they both trying to use, and how would you test on one PC vs two real machines?

## INSTRUCTIONS (no code)
1. Write `scripts/net_manager.gd`. Register it as an Autoload named `NetManager`. Give it `host_game(port)` and `join_game(ip, port)` that create an `ENetMultiplayerPeer` and assign it to `multiplayer.multiplayer_peer`.
2. Connect `peer_connected` and `peer_disconnected` handlers that print the ID and whether this machine `is_server()`.
3. Build `scenes/net/HostJoinMenu.tscn`: a `Control` with Host and Join `Button`s, a `LineEdit` for IP, and a port field defaulting to 7777. Wire the buttons to NetManager.
4. On success (server: `peer_connected`; client: `connected_to_server`), switch to your game scene with `change_scene_to_file()`. Verify the connection survives the scene swap.
5. Run two instances (Debug → Run Multiple Instances → 2, or one export + one editor). Host on one, join with `127.0.0.1` on the other. Win condition for today: **both copies print that the other exists.** Two programs acknowledging each other is the entire foundation.
6. Write three lines in your notes: what a peer is, what a listen server is, why the server is ID 1.

**Report back:** your NetManager structure (functions/vars, not code), what each instance printed, answers to doubts 1–3. I review before you touch Day 10.

---

# DAY 10 — Who Owns the World: Spawning, Syncing, RPCs

## NOTES

### The central law: the server owns reality
Yesterday the server was "the referee." Today, precision. In your architecture, **only the server runs physics and decides what happened.** Clients never move bodies, never score kicks, never drain water. Clients only do one thing: **send their input to the server.** The server simulates, then **broadcasts the results** to everyone.

Why? If clients moved their own bodies and told the server, two clients could disagree (one says "I kicked the snake," the other says "you missed") and the game world would fork into two realities. One referee = one reality. The cost is input delay (~half your round-trip latency) — for a co-op game with friends, that's a fine trade, and it's the simplest correct model that exists. Pros call this **server-authoritative**. You're building it.

### Node 1: MultiplayerSpawner — the automatic birth machine
Add a `MultiplayerSpawner` node to your game scene. Configure it with a scene to spawn (`Player.tscn`) and a parent to spawn under. From then on, every time a peer connects, **every machine automatically instantiates one Player node** — and frees it when the peer disconnects.

Beneath the surface there's no magic: it's listening to `peer_connected` and calling `instantiate()` on all copies at the same network moment. Understand what it does NOT do: it does not position the player, configure it, or give it controls. It just births identical nodes everywhere. Configuration is your job — which brings us to...

### How each Player knows who it belongs to
Each spawned player node must answer: "am I the local human's body, or a remote friend's ghost?" You'll store the owning peer ID on the node (from the spawn event) and compare it against `multiplayer.get_unique_id()`. This is where your Day 2 design gets its exam: `player.gd` was written to read from an `input_source` instead of hardcoded keys. Today:
- The **local** player's `input_source` = the keyboard/gamepad (local co-op) or the network inbox (online).
- The movement/jump/plunge/kick code **does not change.** That's the whole point of the abstraction. If you find yourself editing movement code today, stop — the architecture leaked.

### Node 2: MultiplayerSynchronizer — the property broadcaster
Add a `MultiplayerSynchronizer` as a child of Player. Configure it with a property list — `position`, your state enum — and a send rate (default 20 updates/sec, not 60). What it does beneath: on a timer, it reads those properties on the machine that **owns** the synchronizer, packs them into packets, sends them; receiving machines write the values into their copy of the same node.

Two things to internalize:
- **It broadcasts whatever the owner's values are.** It contains zero game logic. If the owner's position is wrong, everyone's position is wrong.
- **Ownership is a per-node decision** (`set_multiplayer_authority(id)`). By default the server owns everything — which is exactly what you want: the server's copy of every player body is the real one, and clients receive its positions.

### RPCs: calling functions on other machines
The final tool. Tag any function with `@rpc("any_peer", "reliable")` and it becomes callable across the network. The two knobs:

- **Who may call it:** `authority` = only the server can call it (clients just receive). Use for "server announces: a kick happened." `any_peer` = any client may call it — used when a client wants to *request* something from the server.
- **Delivery promise:** `reliable` = resend until it arrives — for events that must not vanish (kicks, deaths, "I drank from the cactus"). `unreliable` = faster, may drop — for continuous data that updates constantly (positions, which your synchronizer already handles better).

Your input pipeline becomes beautifully small:
1. Local machine reads `Input` (as it always did).
2. Instead of moving the body, it calls an `any_peer` RPC on the server: "player [id] pressed right / released jump / kicked."
3. The server receives it and writes it into an **input state** (a small variable block) for that player — this IS the network `input_source`.
4. The server's `player.gd` runs its normal movement code from that input state, inside `_physics_process`, **gated by `is_multiplayer_authority()`** so only the real body simulates and ghosts don't.

### Interpolation: the math that makes it look alive
Synchronizers send at 20Hz; your screen refreshes 60+ times a second. So on your machine, a remote friend's position is **stale for about 3 frames** between updates. Copy received positions directly and the remote ostrich moves in visible steps — a slideshow.

The fix: keep a `target_position` (last value received from the network) and each frame move the *visual* toward it with `lerp`: go a fixed **fraction of the remaining distance** per unit of time, e.g. weight proportional to `delta` (like `10 * delta`). Big jumps close fast, small errors close slowly, and the eye reads "gliding ostrich" instead of "laggy rectangle."

Why must the weight scale with `delta`? If you lerp a flat 0.1 *per frame*, a 60fps machine applies it 60 times/sec and a 30fps machine 30 times/sec — different smoothing for different players. Tie the weight to `delta` (time, not frames) and both see identical motion. (This is the same delta-time lesson as Day 2's movement — network code is not a new math universe.)

Pattern to hold: the `CharacterBody2D` is the authoritative physics body (server-side truth); what your eye sees is an interpolated visual chasing the synced position. Truth teleports, visuals glide.

## DOUBTS
1. When you press jump on your own ostrich, who runs the physics first, and how many milliseconds until *you* see yourself leave the ground? Is that acceptable in co-op? Why did big competitive games invent "client prediction" to hide it — and why don't you need it for ostriches?
2. Trace one position packet: which machine *sends* player 2's position, and why? What would happen if player 2's own client also tried to send his position?
3. Two clients press Kick 50ms apart. Both `any_peer` RPCs head to the server. What guarantees exist about arrival order? Does kick order matter in your game? What if it did (think of two players grabbing the same cactus)?
4. Why does the position sync get to be continuous/loss-tolerant while the Kick RPC must be `reliable`? What does each lost packet cost in each case?
5. Write the lerp weight as a function of `delta` so that a 30fps and 60fps player see identical smoothing. (You want a fixed fraction per *second*, not per *frame*.)
6. The Spawner creates a node per peer on *every* machine. On the host's screen during online 2P: how many Player nodes exist? On the client's? Which of them run `move_and_slide()`?

## INSTRUCTIONS (no code)
1. Add a `MultiplayerSpawner` to your game scene; configure scene + parent. Confirm each instance spawns one player per peer.
2. On spawn, stamp each Player with its owning peer ID; compute `is_local` by comparing to `get_unique_id()`.
3. Build the RPC input pipeline described in NOTES: local input → `any_peer` RPC → server writes input state → authority-gated physics reads that state. Define your RPC list (names, modes) and write it down.
4. Add a `MultiplayerSynchronizer` to Player syncing `position` + state enum. Document which machine owns each synchronizer and why.
5. Add interpolated visuals: lerp toward synced position with a `delta`-based weight every frame.
6. Two-instance test: both move on both screens, smooth not steppy, kicks register everywhere, host sees both bodies obey physics.
7. Draw (boxes + arrows) the journey of "client presses right": RPC → server input state → physics → synchronizer → client's interpolated visual.

**Report back:** your RPC contract list, your authority decisions, your lerp formula, and the honest answer: did movement code stay untouched? I review before Day 11.

---

# DAY 11 — Merge Everything, Playtest Like a Pro

## NOTES

### Your Day 2 bet pays out (verify it)
Walk all three modes through the same `player.gd`:
- **Solo:** one body, local input, your machine is authority.
- **Local co-op:** two bodies, two local `input_source`s, same machine is authority for both.
- **Online:** server's copy of your body has `input_source` = "RPC inbox"; your client feeds it.

If movement needed zero edits to absorb mode three, you genuinely decoupled input from simulation — that's the skill real engineers get paid for, and you'll know you have it. If you sprinkled `if online:` everywhere, that's the honest review I want to hear.

### The four bug species (classify before you fix)
Every networking bug you'll meet is one of these:
1. **Authority leak** — the wrong machine ran physics or made a decision. Symptom: copies of the game disagree about reality. Detector: `print()` position + frame on both machines for the same player and diff the logs. Cure: gate simulation behind `is_multiplayer_authority()`.
2. **Sync gap** — game state changed on one machine with no server decision and no broadcast (a cactus drained water via a local signal; only that machine knows). Cure: every gameplay *event* is decided on the server and broadcast; never decided locally and silently.
3. **Timing race** — things happened in a bad order: a client joined before the game scene (with its Spawner) existed, so its player spawned into the void. Cure: spawn under nodes that definitely exist; use a custom spawn callable if placement matters.
4. **Interpolation/visual** — truth is fine, but the glide looks wrong (jitter when both players approach each other). Cure: revisit the lerp weight and synchronizer rate.

### Two functions that look alike but are not (the #1 beginner desync)
- `multiplayer.is_server()` — about the **connection**: "am I the host machine?"
- `is_multiplayer_authority()` — about a **specific node**: "do I control THIS body?"
Gameplay decisions → authority check on the node. Session decisions (spawners, timers, game over) → `is_server()`. Confusing them is how you get the host's snakes chasing ghosts.

### What happens to every system when a player vanishes
On `peer_disconnected`, the Spawner frees the node — but the camera still has him in the midpoint, snakes still target his last position, the HUD still shows his water, win/lose still counts him. V1 rule: pick the simplest sane behavior per system (usually: remove from player lists), write it in your GDD, and explicitly defer reconnect to "never in v1."

## DOUBTS
1. Tabulate online 2P: how many Player nodes on the host? On the client? Which run physics? Where does each one's input come from?
2. Snakes target the nearest player (Day 6). In online mode, on which machine does that math run, and what player list does it see? What breaks if a client runs it against ghost copies?
3. Water is [shared/per your GDD]. A cactus interaction on a client — how does the server (and the other client) learn about it? Design the minimal correct path.
4. Remote jitter appears only when both players move toward each other. Design one experiment to separate: interpolation weight vs synchronizer rate vs collision feedback.
5. Build the disconnect risk table: for camera, HUD, snakes, water, win/lose — crash, silently wrong, or fine when a player vanishes mid-run?

## INSTRUCTIONS (no code)
1. Playtest matrix — solo / local 2P / online 2P (two instances, then two LAN machines if you can). Each cell: movement, jump, plunge, kick, snake targeting, water, hallucination flip, camera containment, HUD, win/lose.
2. Log every bug with its species (authority leak / sync gap / timing race / interpolation) before fixing.
3. Enforce the two golden rules and grep your own code for violations: (a) only authority bodies simulate; (b) every event is server-decided and broadcast.
4. Write your "out of scope for v1" list with one-line justifications: graceful disconnect, reconnect, >2 network players, client prediction, cheat validation.
5. Add a Networking section to `GDD.md`: architecture (listen server, server-authoritative, 20Hz), your input-pipeline diagram, the out-of-scope list.

**Report back:** matrix results, classified bug log, answers to doubts 1–3. If movement code truly survived all three modes untouched — you've finished the arc, and you understand Godot networking at a level most tutorials never reach: from *how to press the buttons* down to *what the packets are doing*.

---

## 📖 Reminder
Keep reading your game mechanics/engine design book alongside — the chapters on client-server models and latency will make Day 10's interpolation feel obvious instead of magical.
