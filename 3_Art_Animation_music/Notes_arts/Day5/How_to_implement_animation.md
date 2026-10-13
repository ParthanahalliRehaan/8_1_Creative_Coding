# **how can I implement gif/sprite sheet in the GoDot ?, What if I have more than 2 png files then how ?**
## Step 0: Project setup (do this once)

1. Go to **Project → Project Settings → Rendering → Textures**.
2. Set **Canvas Textures → Default Texture Filter** to **Nearest**. Without this, pixel art looks blurry.

## Step 1: Export from Aseprite

**If you have a GIF or an Aseprite file:**
1. Open it in Aseprite and go to **File → Export Sprite Sheet**.
2. Layout tab: set **Sheet Type** to **Horizontal Strip**. This gives you a 128×32 PNG (4 frames × 32 px wide).
3. Output tab: tick **Output File** and save it into `res://art/ostrich/ostrich_walk.png`.

**If you want separate PNGs:** use **File → Export As…**, name it `ostrich_walk.png`, and Aseprite writes `ostrich_walk_0.png`, `ostrich_walk_1.png`, and so on.

## Method A: One sprite sheet → Sprite2D + AnimationPlayer

This is the "manual" way, and it shows you how frames really work.

1. Open your `Player.tscn`. Under the root node, add a child **Sprite2D** and rename it `Sprite`.
2. Drag `ostrich_walk.png` from the FileSystem dock into the **Texture** property in the Inspector.
3. In the Inspector, open the **Animation** section:
   - **Hframes = 4** (columns)
   - **Vframes = 1** (rows)
   - **Frame = 0**

   Change **Frame** to 1, 2 and 3 to see each frame appear.
4. Add another child node under the root: **AnimationPlayer**.
5. In the bottom panel, click **Animation → New**, name it `walk`, and set the length to `0.5` s.
6. Click the **Loop** icon in the animation panel so it repeats.
7. Click the **Sprite** node, then click the **key icon** next to **Frame** in the Inspector. Choose to create a new track.
8. Move the timeline to 0.0 s and set Frame = 0 and key it. Repeat at 0.125 s (Frame 1), 0.25 s (Frame 2) and 0.375 s (Frame 3).
9. Press **Play** in the animation panel to preview it.

## Method B: One sprite sheet → AnimatedSprite2D

This is the faster way, and the best one for a simple loop.

1. Under the root, add a child **AnimatedSprite2D** and rename it `Anim`.
2. In the Inspector, click **Sprite Frames → New SpriteFrames**, then click the SpriteFrames resource to open the bottom panel.
3. Rename `default` to `walk`.
4. Click the **Add Frames from Sprite Sheet** icon (grid icon).
5. Pick `ostrich_walk.png`, set **Horizontal = 4** and **Vertical = 1**, click each of the 4 frames to select it, then click **Add 4 Frame(s)**.
6. Set **FPS = 8** and turn on the **Loop** icon.
7. Make a second animation with the **Create Animation** icon, called `idle`, and repeat steps 4-6 with its own sheet.

## Method C: Several separate PNGs (your "more than 2 PNGs" case)

Use this when each frame is its own file, like `ostrich_walk_0.png` to `ostrich_walk_3.png`.

1. Add an **AnimatedSprite2D** and create a **New SpriteFrames**, same as in Method B steps 1-3.
2. Click the **Add frames from file** icon (folder icon) in the bottom panel.
3. Select all 4 PNGs at once with **Shift+click**. Godot adds them in filename order, so number them with the same digits (`_0`, `_1`, `_2`, `_3`, or `_00`, `_01` if you pass 9 frames).
4. Set **FPS = 8** and enable **Loop**.

If you ever get the order wrong, drag frames in the panel to reorder them.

## Step 2: Play it from code

Add this to your `player.gd` (adjust the node name if yours differs):

```gdscript
@onready var anim: AnimatedSprite2D = $Anim

func _physics_process(delta):
    # ...your movement code...
    if velocity.length() > 0:
        anim.play("walk")
    else:
        anim.play("idle")
    if velocity.x != 0:
        anim.flip_h = velocity.x < 0   # mirrors the sprite instead of drawing a left-facing version
```

For Method A, replace the node with `$AnimationPlayer` and call `$AnimationPlayer.play("walk")`.

## Math corner

- Loop length = frames ÷ FPS, so 4 ÷ 8 = **0.5 s per loop**.
- Each frame shows for 1 ÷ FPS = **0.125 s**.
- A sheet is `frames × frame_width` wide, so 4 × 32 = **128 px**. If the width doesn't divide evenly, Hframes will slice the frames wrong.

## Common problems

- **Blurry sprites**: you skipped the Nearest filter in Step 0.
- **Frames look cut off**: your Hframes/Vframes don't match the sheet's real grid.
- **Animation doesn't loop**: the Loop toggle is off in the SpriteFrames panel (or in the AnimationPlayer).
- **Collision shape is off**: the Sprite and its CollisionShape2D must both be children of the same body, and the shape shouldn't be scaled by the sprite.

## Which one to choose?

- **AnimatedSprite2D (B/C)** is best for simple loops like walk, idle and snakes.
- **Sprite2D + AnimationPlayer (A)** is best when you want to animate more than frames, such as hitboxes, sounds or the position of a kick, at specific frames.

## Mentor's reminder

Don't skip your reading homework. Pick up *Game Programming Patterns* by Robert Nystrom (it's free online) to understand how game loops, state machines and updates work inside an engine. I can't search or check a library from here, so double-check the title and author yourself.