# GODOT Animation

**Note** Day1 contains the instructions to complete the art/animation but first learn from this video tutorial: https://www.youtube.com/watch?v=XbDh2GAshBA

## 1. Two Systems, Know the Difference

| Tool | Use for | Complexity |
|---|---|---|
| **AnimatedSprite2D** | Simple frame-by-frame (walk, idle, attack) | Easy |
| **AnimationPlayer** | Animating *any property* + cutscenes + logic | Medium |
| **AnimationTree** | Blending between animations (character controllers) | Hard |
| **Tween** | Quick code-driven motion | Easy |

---

## 2. AnimatedSprite2D (start here)

**Setup workflow:**
1. Create `AnimatedSprite2D` node
2. In Inspector → Sprite Frames → click `[empty]` → New SpriteFrames
3. Click the SpriteFrames resource to open the editor (bottom panel)
4. Add animations (rename them: `idle`, `run`, `attack`...)
5. Drag your Aseprite sprite sheet in → **slice it** in the Import dock:
   - Select the PNG in FileSystem → Import tab
   - Set `hframes` (columns) and `vframes` (rows) → Reimport
6. Drag frames into each animation

**Key properties:**
- `Speed (FPS)` — playback speed. 8–12 FPS for pixel art feels good; don't match Aseprite's timing, tune in Godot
- `Loop` — on/off per animation
- `Autoplay` — animation that plays on scene start

**In code:**
```gdscript
@onready var sprite: AnimatedSprite2D = $AnimatedSprite2D

func _ready():
    sprite.play("idle")

# React when an animation finishes
func _on_sprite_animation_finished():
    if sprite.animation == "attack":
        sprite.play("idle")
```

**Important signals:** `animation_finished`, `frame_changed` (useful for hitboxes that activate on a specific frame!)

---

## 3. AnimationPlayer (the real power)

Animates **any property** of **any node** — position, rotation, scale, modulate, even calling functions.

**Creating an animation:**
1. Add `AnimationPlayer` node → click it → Animation panel opens (bottom)
2. "Animation" button → New → name it
3. Click the ⏱ record button (key icon) → modify any property in the Inspector → keyframe is auto-added
4. Or manually: click the key icon next to any property

**Track types (right-click in track panel → Insert Track):**
| Track | What it does |
|---|---|
| **Property** | Animate position, scale, rotation, modulate, visible... |
| **Method** | Call a function at a keyframe (e.g., spawn hitbox, play sound) |
| **Bezier** | Smooth curves — for camera movement, UI |
| **Audio** | Play audio at keyframes |
| **Animation** | Play another animation from here |

**Key concepts:**
- **Length** — animation duration (set in the panel)
- **Update mode** per track: `Discrete` (snap, for frame changes) / `Continuous` (smooth) / `Trigger` / `Capture`
- **Loop** — set via the animation's settings
- **Easing** — click a key → bottom-left curve editor → choose ease in/out. This is what makes motion feel juicy

**Playing in code:**
```gdscript
@onready var anim: AnimationPlayer = $AnimationPlayer

anim.play("open_door")
anim.play_backwards("open_door")   # reverse!
anim.stop()
anim.current_animation            # what's playing
```

**The killer feature — Method tracks:** sync logic to animation without frame math:
- Hitbox activates on frame 3 of attack → method track calls `enable_hitbox()`
- Footstep sound on frame where foot touches ground

---

## 4. AnimationTree (character controller blending)

Sits on top of AnimationPlayer. You don't create animations here — you **organize** them.

**Nodes you'll use:**
| Node | Purpose |
|---|---|
| **StateMachine** | "idle / run / attack / hurt" — exactly one state at a time |
| **BlendSpace1D/2D** | Blend animations along speed (1D) or direction (2D, like walk direction) |
| **OneShot** | Play something once, then return (attack, hurt flash) |
| **Blend2 / Add2** | Mix two animations (walk + aim) |

**Workflow:**
1. AnimationPlayer with all your animations
2. Add `AnimationTree` → assign the AnimationPlayer in `Anim Player` property
3. Set `Tree Root` → New `AnimationNodeStateMachine`
4. Double-click the tree → right-click → Add states → drag animations in
5. Connect states with **travel transitions** (choose via code: `travel("attack")`)

**In code:**
```gdscript
@onready var tree: AnimationTree = $AnimationTree

func _ready():
    tree.active = true

func attack():
    tree.get("parameters/playback").travel("attack")
```

**Travel vs travel-less:** `travel()` respects transition rules; use it for player states.

---

## 5. Tween (juice, code-only)

No editor needed. One-liners for snappy motion:

```gdscript
# Squash and stretch on jump
var tween = create_tween()
tween.tween_property(self, "scale", Vector2(1.2, 0.8), 0.1)
tween.tween_property(self, "scale", Vector2.ONE, 0.2)

# Damage flash red
var t = create_tween()
t.tween_property(sprite, "modulate", Color.RED, 0.05)
t.tween_property(sprite, "modulate", Color.WHITE, 0.2)

# Easing styles
tween.set_ease(Tween.EASE_OUT).set_trans(Tween.TRANS_BACK)  # snappy overshoot
```

**Common TRANS types:** `TRANS_LINEAR` (boring), `TRANS_SINE` (smooth), `TRANS_QUAD` (default feel), `TRANS_BACK` (overshoot — great for UI pop), `TRANS_BOUNCE`, `TRANS_ELASTIC`.

---

## 6. Import Rules (from Aseprite)

- **Sprite sheet PNG** → Import dock → set `hframes`/`vframes` → Reimport
- Filter: **disable** → `Import` → `Filter` OFF, `Mipmaps` OFF (pixel art must stay crisp)
- **Pivot consistency:** decide origin (usually bottom-center for characters) and draw every sprite on the same canvas size

---

## 7. Game Feel Checklist (where animation meets code)

- [ ] Squash & stretch on jump/land
- [ ] Brief pause/hitstop on hits (Godot: `Engine.time_scale = 0.05` then back)
- [ ] Screen shake on impact (`Camera2D` offset with decaying random values)
- [ ] Attack: windup → active → recovery frames (different speeds per phase)
- [ ] Use `frame_changed` signal for hitbox timing, not timers
- [ ] `modulate` flash for damage/invincibility

---

# 🎨 ASEPRITE — MASTERY NOTES (the "little bit" you need)

## 1. Interface (10 seconds)
Left = toolbox | Right = layers & color | Top = timeline (appears with tags) | Middle = canvas.

## 2. Tools you actually need
| Tool | Notes |
|---|---|
| **Pencil (B)** | Pixel workhorse. 1px size, `Perfect Pixel` ON in context bar |
| **Eraser (E)** | Same settings as pencil |
| **Line (L)** | Hold Shift for straight lines with any tool |
| **Rectangle/Ellipse (U/R)** | Hold Shift = perfect shape |
| **Eyedropper (Alt+click)** | Fastest way to grab colors |
| **Paint Bucket (G)** | `Contiguous` checkbox = flood fill vs fill-all |
| **Selection (M)** | Ctrl+C/V moves chunks; nudge with arrows |
| **Move (V)** | Move selection or layers |

**Learn 5 shortcuts and you're 80% done:** B, E, G, M, Alt+click.

## 3. Layers (like Godot nodes for art)
- New layer per: outline, base color, shading, highlights, effects
- Shortcut: `Ctrl+Shift+N`
- **Why it matters:** you can edit shading without touching outline, and export only certain layers (e.g., separate weapon layer for swappable sprites in Godot!)

## 4. Tagging & the Timeline
- `Alt+N` → new tag (name it `run`, `attack`...)
- Tags = animations. Without tags, exports become a mess
- **Onion skinning:** button at timeline bottom — shows previous/next frames ghosted. Essential for smooth motion

## 5. Animation principles (this is 90% of "being good")
- **Silhouette first** — if the shape isn't readable in black, colors won't save it
- **Squash & stretch** — impacts compress, jumps stretch
- **Anticipation** — before a punch, character pulls back slightly
- **Follow-through** — cape/hair moves after the body stops
- **4–8 frames per cycle** for run cycles; fewer frames ≠ worse, often snappier
- **Hold keyframes** — don't redraw every frame; hold 2–3 frames, move only what's needed

## 6. Color & Shading (the part beginners get wrong)
- **Never use black for shadows or white for highlights.** Shift the hue: shadow = base color shifted darker + toward blue/purple; highlight = lighter + toward yellow
- **Hue shifting** is what separates amateur from pro pixel art
- **Palette discipline:** build a palette (`Windows → Color Palette` → save as .gpl) and reuse it across all sprites — keeps your game visually consistent
- Shading technique: **2-tone cel shading** first (base + shadow). Add a 3rd highlight tone only when needed

## 7. Canvas discipline (critical for Godot workflow)
- Pick ONE sprite size per game: 16×16, 32×32, or 48×48, etc.
- **Every frame = same canvas size.** Animation looks broken in Godot if frames have different sizes
- Keep the character's **feet at the same pixel row** in every frame of a walk/run cycle (pivot consistency)

## 8. Exporting to Godot
- `File → Export Sprite Sheet`
- Settings:
  - **Sheet type:** Horizontal Strip or Packed (packed = smaller file, but frames have offsets — fine for AnimatedSprite2D)
  - **Border padding: 1px** (prevents bleeding between frames)
  - **Trim sprite / Merge duplicates:** ON to save space
  - **Split layers/tags:** ON if you want per-animation sheets
- Export as **PNG** (lossless — never JPEG for pixel art)

## 9. Pro tricks worth learning
- **Mirror mode (symmetry):** `View → Show → Symmetry` — huge for characters
- **Tilemap mode:** for level tiles
- **Linked cels:** copy a frame forward so edits propagate
- `Ctrl+Z` history is deep — experiment fearlessly

---

# 📚 Suggested learning order

1. Aseprite: draw a 32×32 idle (2–3 frames) → export sprite sheet
2. Godot: slice it, play it with AnimatedSprite2D
3. Godot: build a walk cycle + attack with frame timing
4. Godot: AnimationPlayer method track → hitbox on frame 3
5. Godot: AnimationTree state machine (idle/run/attack)
6. Juice: tweens, hitstop, screenshake
7. Aseprite: shading, palettes, squash & stretch polish
