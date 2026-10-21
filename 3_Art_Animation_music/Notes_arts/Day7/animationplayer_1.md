# Notes: AnimationPlayer (Godot 4)

> **Official docs (best source of Godot images):**
> - Intro to animation features: https://docs.godotengine.org/en/stable/tutorials/animation/introduction_to_the_animation_features.html
> - AnimationPlayer class reference: https://docs.godotengine.org/en/stable/classes/class_animationplayer.html
>
> Each section below has a **📷 Screenshot slot**. Take the screenshot from your own editor (Win+Shift+S), save it in an `images/` folder next to this file, and replace the placeholder line.

---

## 1. What it is
- **AnimationPlayer** records property changes over time and plays them back.
- It can animate almost any property: position, rotation, scale, modulate (color/alpha), visibility, sprite frame, sound volume.
- One AnimationPlayer holds many animations (`idle`, `walk`, `hit`, `die`), each with its own timeline.

📷 **Screenshot slot:** the Animation panel at the bottom of the editor with an AnimationPlayer selected.
`![AnimationPlayer panel](images/animation_panel.png)`
Docs reference: see the "Creating an animation" section of the intro page linked above.

---

## 2. Keyframe
- A keyframe is a saved value of a property at a specific moment in time.
- Godot fills in the values between two keyframes (**interpolation**).
- Common keyed properties:
  - **Position**: where the node is.
  - **Rotation**: the angle.
  - **Frame (index)**: which picture of a sprite sheet is shown. It doesn't interpolate, it just switches.
- You don't need a keyframe every second. Place keyframes only where the value changes. For a 4-frame walk at 0.1s per frame: keys at 0.0, 0.1, 0.2, 0.3.

📷 **Screenshot slot:** the key icon next to the `Frame` property in the Inspector, plus a track with 4 key diamonds on the timeline.
`![Keyframes](images/keyframes.png)`

---

## 3. Onion skin
- Shows faint ghost copies of the previous and next frames over the current one.
- Lets you see how far a sprite moved or rotated between frames.
- Best for hand-made frame animation and checking smoothness.
- Toggle it from the animation panel toolbar (onion skinning button).

📷 **Screenshot slot:** the timeline toolbar with the onion skin button, and a sprite showing ghost frames.
`![Onion skin](images/onion_skin.png)`

---

## 4. Snap
- Snap forces keyframes onto fixed **time** steps (e.g. 0.1s, 0.5s).
- Snap off: you can place a key at any time, like 0.13s.
- Use snap for sprite-frame animation; turn it off to fine-tune feel.

📷 **Screenshot slot:** the Snap toggle and the Step value field in the animation toolbar.
`![Snap](images/snap.png)`

---

## 5. Key animation settings

| Setting | Meaning |
|---|---|
| **Length** | Total duration in seconds |
| **Loop** | Repeats forever (idle, walk) |
| **Step** | The snap interval |
| **Autoplay** | Plays when the scene starts |

📷 **Screenshot slot:** the animation toolbar showing Length, Loop and Step.
`![Animation settings](images/settings.png)`

---

## 6. Build it: 4-frame walk
1. Scene with root `CharacterBody2D` and child `Sprite2D`.
2. Sprite2D: `Hframes` = 4, `Vframes` = 1 (4 images in one row).
3. Add an `AnimationPlayer` as a child of the root.
4. Bottom panel: **Animation → New**, name it `walk`.
5. **Length** = 0.4, **Loop** on, **Step** = 0.1, snap on.
6. Select Sprite2D, set `Frame` = 0, click the key icon, choose **Create**.
7. Move the timeline to 0.1 and set `Frame` = 1, key it. Repeat for 0.2 (frame 2) and 0.3 (frame 3).
8. Press play.

No art yet? Use 4 colored rectangles of different shades in one row. Same workflow.

📷 **Screenshot slot:** your finished scene tree + the `walk` timeline.
`![Walk animation](images/walk_result.png)`

---

## 7. Playing from code
```gdscript
@onready var anim: AnimationPlayer = $AnimationPlayer

func _physics_process(delta):
    if velocity.length() > 0:
        anim.play("walk")
    else:
        anim.play("idle")
```
Calling `play()` with the animation already playing doesn't restart it, so it's safe every frame.

---

## 8. Quick tips
- Don't keyframe properties you aren't changing.
- Name animations in lowercase.
- The **Call Method** track triggers code at an exact time, e.g. enabling a hitbox during an attack.

---

**Mentor reminder:** keep reading your game-mechanics/engine book alongside these notes, even a few pages a day.