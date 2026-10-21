# SpriteFrames (Godot) vs Layers / Frames / Tags (Aseprite)

> **Correction applied:** a SpriteFrames resource stores **animations**, and each animation holds frames. In Aseprite, a **tag** marks an **animation** (a range of frames). Frames are the individual pictures, not the animation itself.
>
> Docs (links from memory, please check they open):
> - Godot AnimatedSprite2D: https://docs.godotengine.org/en/stable/classes/class_animatedsprite2d.html
> - Godot SpriteFrames: https://docs.godotengine.org/en/stable/classes/class_spriteframes.html
> - Aseprite docs: https://www.aseprite.org/docs/
>
> 📷 Screenshot slots: capture from your own editor, save into `images/`, and replace the placeholder lines.

---

## Big picture

```
ASEPRITE (making art)                      GODOT (playing art)
Layers + Groups  ──flatten on export──▶   (Godot never sees layers)
Frames           ──────────────────────▶  Frames inside an animation
Tags (= animations) ───────────────────▶  Animations inside SpriteFrames
Frame duration (ms) ───────────────────▶  FPS (set per animation)
```

**Key idea:** layers exist only while drawing. Once exported, everything is flattened into a sprite sheet. Godot only knows frames and animations.

---

## Part 1: Godot

### Node vs resource
| Thing | What it is | Analogy |
|---|---|---|
| **AnimatedSprite2D** | Node that plays an animation | The player |
| **SpriteFrames** | Resource holding named animations | The playlist |
| **Animation** (`idle`, `walk`) | One named entry in SpriteFrames: a list of frames + FPS + loop | One song |
| **Frame** | One image inside an animation | One second of the song |

### Build steps
1. Create a scene with root `CharacterBody2D`, child `AnimatedSprite2D`.
2. Select `AnimatedSprite2D` → Inspector → **Sprite Frames** → `<empty>` → **New SpriteFrames**.
3. Click the resource to open the **SpriteFrames panel** at the bottom.
4. Rename `default` to `idle` (double-click).
5. Add frames: drag images in, or use **Add frames from sprite sheet**.
6. Set **FPS** = 5 for a simple 2-frame idle.
7. Click **Add Animation**, name it `walk`, add 4 frames, FPS = 8.
8. Loop **on** for `idle` and `walk`. Loop **off** for one-shots (attack, death).
9. Optional: **Autoplay** on `idle`.

📷 `![SpriteFrames panel](images/spriteframes_panel.png)`

No art yet? Make 2 rectangle PNGs (e.g. 32x32, red and dark red) for `idle`, 4 for `walk`.

### Play from code
```gdscript
@onready var sprite: AnimatedSprite2D = $AnimatedSprite2D

func _physics_process(delta):
    if velocity.length() > 0:
        sprite.play("walk")
    else:
        sprite.play("idle")
```
The string must match the animation name exactly. Calling `play()` on the already-playing animation doesn't restart it.

### AnimatedSprite2D vs AnimationPlayer
| | AnimatedSprite2D + SpriteFrames | AnimationPlayer |
|---|---|---|
| Best for | Simple frame-by-frame sprite animation | Animating many properties (position, rotation, color, sound, code calls) |
| Setup | Quick | More flexible |
| Use together? | Yes: AnimationPlayer can also trigger things around a sprite's animation (hitboxes, sounds) |

---

## Part 2: Aseprite

### Vocabulary
| Term | Meaning | Example |
|---|---|---|
| **Frame** | One picture in the sequence (time) | walk1, walk2, walk3, walk4 |
| **Layer** | A transparent sheet stacked in each frame (depth) | `outline`, `body`, `shadow` |
| **Group** | A folder holding several layers | `character` = body + outline |
| **Cel** | One layer's drawing in one frame (row × column in the timeline) | `body` in frame 3 |
| **Tag** | A named range of frames = **an animation** | `idle` = frames 1-2 |

**Rule of thumb:** changes over time → new frame. Separate part of the picture → new layer. Many layers to manage → group.

### Why use groups
- Hide, show or move many layers at once.
- Keeps the timeline tidy past ~8 layers.
- Hidden layers and groups are skipped in export, so reference/guide layers are safe to keep. Check export settings if something unexpected appears.
- Create: **Layer → New → New Group**, then drag layers in.

📷 `![Aseprite timeline](images/aseprite_timeline.png)`

### Practice build (32x32 character)
1. **File → New**: 32x32, RGBA.
2. Create layers `body`, `outline`, `shadow`. Group `body` + `outline` into `character`.
3. Make **6 frames** (Alt+N adds a frame).
4. Tag frames 1-2 as `idle`, frames 3-6 as `walk` (tags must not overlap).
5. Idle: on frame 2, move `body` up 1px.
6. Walk: bounce `body` by 1px on each of frames 3-6.
7. Set frame durations: idle ~200 ms, walk ~100 ms (Godot FPS = 1000 ÷ ms, so 100 ms = 10 FPS).
8. **File → Export Sprite Sheet**: Layout = *Horizontal Strip*, tick *Output File*.

### Import into Godot
1. Save the PNG into your project's folder.
2. In the SpriteFrames panel, choose the `idle` animation → **Add frames from sprite sheet** → pick the PNG.
3. Set Horizontal = 6, Vertical = 1.
4. Select frames 1-2 → **Add Frames**. Switch to `walk`, repeat for frames 3-6.
5. Set FPS: `idle` = 5, `walk` = 10.

📷 `![Sprite sheet import dialog](images/sheet_import.png)`

---

## Common mistakes
- Expecting layers to appear in Godot. They're flattened on export.
- Mismatched names between code (`"walk"`) and the SpriteFrames panel.
- Overlapping tags in Aseprite.
- Forgetting to turn Loop off for attack/death animations.
- Setting FPS too high; start low (5-10) and adjust by eye.

---

**Mentor reminder:** this week, read a chapter on the **game loop** in your game-engine book. It explains why frames and FPS matter: your game updates and draws many times per second, and animation FPS is independent of that.