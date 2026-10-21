# Notes: AnimationTree (Godot 4)

> Docs (link from memory, please check it opens):
> - https://docs.godotengine.org/en/stable/tutorials/animation/animation_tree.html
> - Class reference: https://docs.godotengine.org/en/stable/classes/class_animationtree.html
>
> 📷 Screenshot slots: capture from your own editor, save into `images/`, and replace the placeholder lines.

---

## 1. What it is
- **AnimationTree** doesn't store animations. It **decides which animations from an AnimationPlayer play, and how they blend or switch**.
- Think: **AnimationPlayer = the clips, AnimationTree = the director.**
- It works only with **AnimationPlayer**, not AnimatedSprite2D / SpriteFrames.

| Node | Job |
|---|---|
| AnimatedSprite2D + SpriteFrames | Simple frame-by-frame sprite playback |
| AnimationPlayer | Stores and plays animations (any property) |
| **AnimationTree** | Switches/blends between AnimationPlayer animations using rules |

### When do you need it?
- Not needed for 2-3 animations: `anim.play("walk")` is enough.
- Use it when you have many states (idle, walk, run, jump, attack, hurt) with transitions, or direction-based animation (top-down 4/8 directions), or blending.

---

## 2. Setup (always the same 4 steps)
1. Make an **AnimationPlayer** with your animations first.
2. Add an **AnimationTree** node (sibling of the AnimationPlayer).
3. In the Inspector: **Anim Player** → assign your AnimationPlayer.
4. **Tree Root** → New **AnimationNodeStateMachine** (or another root type), then turn **Active** on.

Forgetting **Active** is the most common reason nothing plays.

📷 `![AnimationTree inspector](images/animtree_inspector.png)`

---

## 3. Node types you can use inside the tree
| Type | Purpose | Example |
|---|---|---|
| **Animation** | Plays one animation from the AnimationPlayer | `walk` |
| **StateMachine** | States + transitions between them | idle ⇄ walk → attack |
| **BlendSpace1D** | Blend along one value | speed: idle → walk → run |
| **BlendSpace2D** | Blend along a Vector2 | 4-direction top-down walking |
| **BlendTree** | Combine/mix animations with math nodes | walk + upper-body aim |

Most 2D games use: **StateMachine** (main) + **BlendSpace2D** (directions) inside it.

---

## 4. Build: StateMachine with idle, walk, attack
**Animations to make first in AnimationPlayer:**
- `idle`: length 0.4, loop on
- `walk`: length 0.4, 4 frames at 0.1, 0.2, 0.3 (+0.0), loop on
- `attack`: length 0.3, 3 frames, **loop off**

**In the AnimationTree:**
1. Tree Root = **AnimationNodeStateMachine**, Active on.
2. Open the **AnimationTree** panel at the bottom.
3. Right-click empty space → **Add Animation** → `idle`. Repeat for `walk` and `attack` (3 nodes).
4. Right-click `idle` → **Set as Start** (Godot marks the start state).
5. Use the **connect (line) tool** to draw transitions: `idle ⇄ walk` (both directions), `idle → attack`, `walk → attack`, and `attack → idle`.
6. Click the `attack → idle` arrow and set **Switch Mode = At End** and **Advance Mode = Auto**. It returns to idle automatically after the attack finishes.

📷 `![State machine graph](images/state_machine.png)`

### Code
```gdscript
@onready var tree: AnimationTree = $AnimationTree
@onready var playback: AnimationNodeStateMachinePlayback = tree.get("parameters/playback")

func _physics_process(delta):
    if Input.is_action_just_pressed("attack"):
        playback.travel("attack")
    elif playback.get_current_node() != "attack":
        if velocity.length() > 0:
            playback.travel("walk")
        else:
            playback.travel("idle")
```
- `travel("walk")` follows the transitions you drew to reach `walk`.
- The `!= "attack"` check stops walk/idle from interrupting the attack.
- `travel()` only goes where a transition exists, so draw the lines.

---

## 5. Build: 4-direction top-down walk (BlendSpace2D)
Good fit for a top-down game like your Haunted Shift.

**Animations needed in AnimationPlayer:** `walk_up`, `walk_down`, `walk_left`, `walk_right`, plus `idle_up/down/left/right` (8 total).

1. In the StateMachine, add a **BlendSpace2D** node named `walk`.
2. Open it and add 4 points (Create Points tool):
   - (0, -1) → `walk_up`
   - (0, 1) → `walk_down`
   - (-1, 0) → `walk_left`
   - (1, 0) → `walk_right`
3. Set the BlendSpace2D **Blend Mode = Discrete** (important for frame animation, you want a switch, not a blend of two pictures).
4. Repeat for an `idle` BlendSpace2D with the idle animations.

📷 `![BlendSpace2D](images/blendspace2d.png)`

### Code
```gdscript
func _physics_process(delta):
    var dir := Input.get_vector("left", "right", "up", "down")
    velocity = dir * speed
    move_and_slide()

    if dir != Vector2.ZERO:
        tree.set("parameters/walk/blend_position", dir)
        tree.set("parameters/idle/blend_position", dir)  # idle remembers last facing
        playback.travel("walk")
    else:
        playback.travel("idle")
```
The parameter path is `parameters/<state name>/blend_position`, so names must match exactly.

---

## 6. Parameters cheat sheet
| Path | Meaning |
|---|---|
| `parameters/playback` | StateMachine controller (`travel`, `start`, `get_current_node`) |
| `parameters/<state>/blend_position` | BlendSpace1D/2D input |
| `parameters/<state>/...` | Other nodes expose their values here; see the Parameters section in the Inspector |

Tip: select the AnimationTree and expand **Parameters** in the Inspector to see exact paths. Copy them instead of typing.

---

## 7. Common mistakes
- **Active** is off → nothing plays.
- Calling `anim_player.play()` while the tree is active → they fight.
- Trying to use it with AnimatedSprite2D (not supported).
- Typos in state or parameter names.
- Blend Mode left on *Interpolated* for sprite-frame animations → odd mixed frames.
- Forgetting to draw a transition → `travel()` can't reach the state.
- Not setting a **Start** state.

---

## 8. Quick rule
- 1-3 animations → `AnimationPlayer.play()`
- Many states / directions → **AnimationTree**
- Just a sprite flipbook → AnimatedSprite2D

---

**Mentor reminder:** a state machine is a core game-engine idea (the same pattern drives enemy AI and game states). Read about it in your game-mechanics/engine book alongside these notes.