### 1. Add these variables
Put them under `var cooldown_timer: float = 0.0`:

```gdscript
@export var low_health_threshold: float = 30.0   # eagle only helps at or below this health
@export var max_kills: int = 2                   # kills before the eagle is tired
@export var tired_window: float = 600.0          # seconds (10 minutes)

var game_time: float = 0.0                       # only counts while the game is running (not paused)
var kill_times: Array[float] = []                # game_time of each recent kill
```

I used `game_time` instead of the real clock so the 10 minutes don't tick while the game is paused.

### 2. Count time
In `_physics_process`, right after the line `cooldown_timer = max(cooldown_timer - delta, 0.0)`, add:

```gdscript
	game_time += delta
```

### 3. Only look for a target when the owner is in danger and the eagle is rested
In the `State.FOLLOW:` branch, change this line:

```gdscript
			if cooldown_timer == 0.0:
```

to:

```gdscript
			if cooldown_timer == 0.0 and _leader_in_danger() and not _is_tired():
```

### 4. Remember each kill
In the `State.ATTACK:` branch, add one line right after `print("Eagle killed a snake!")`:

```gdscript
					kill_times.append(game_time)
```

### 5. Paste these two functions at the bottom of the file

```gdscript
# True when the owner's health is at or below the threshold.
func _leader_in_danger() -> bool:
	var st = GameState.players.get(leader.player_id)
	return st != null and st.health <= low_health_threshold


# Tired = already made max_kills kills within the last tired_window seconds.
func _is_tired() -> bool:
	# Forget kills older than 10 minutes. The oldest one is always first in the array.
	while not kill_times.is_empty() and game_time - kill_times[0] >= tired_window:
		kill_times.pop_front()
	return kill_times.size() >= max_kills
```

### How it behaves
- The eagle still follows you all the time, but it only dives when your health is at or below 30.
- After 2 kills it ignores snakes until the older kill is 10 minutes old, then it can attack again.
- If you heal above 30 while it is already diving, it finishes that attack.
- Each player has their own eagle with its own tiredness.
- The tired state is not visible to the player yet. Clients only receive the eagle's position, so a "tired" look would need one more replicated property. Say so if you want that.

### Test it quickly
In the Inspector (select the Eagle scene root) set these temporary values:
- `low_health_threshold = 100`: the eagle attacks right away, because your health is always ≤ 100.
- `tired_window = 30`: you only wait 30 seconds to see it recover.

Let 3 snakes come near. The eagle should kill 2, then ignore the third until 30 seconds after the first kill. Set the values back to `30` and `600` when you're done.