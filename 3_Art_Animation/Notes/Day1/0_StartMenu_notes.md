# Start Menu: full instructions (640×320)

## Part A: Project Settings (Project → Project Settings, Advanced Settings ON)

| Setting                                           | Value          |
| ---------------------------------------------------| ----------------|
| Display → Window → Size → Viewport Width / Height | **640 / 320**  |
| Window Width / Height Override                    | **1280 / 640** |
| Stretch → Mode                                    | **viewport**   |
| Stretch → Aspect                                  | **keep**       |
| Stretch → Scale Mode                              | **integer**    |
| Rendering → Textures → Default Texture Filter     | **Nearest**    |

---

## Part B: Aseprite (3 files)

Open each file with **File → New**: Color Mode **RGBA**, Background **Transparent**. Then load your `.gpl` palette (palette menu → Load Palette). Turn on **View → Show → Pixel Grid**.

### File 1: `art_source/ui/shared/ui_buttons.aseprite`
Canvas **96×24**, **3 frames**. The pixel coordinates are (x, y) from the top-left.

**Frame 0 (normal):**
1. Pick palette #1 (dark). Draw a 1 px outline around the whole canvas (x 0–95, y 0–23).
2. Erase these 8 pixels for rounded corners: (0,0), (95,0), (0,23), (95,23), (1,1), (94,1), (1,22), (94,22).
3. Bucket-fill the inside with #7 (sand mid).
4. Draw a line in #6 (sand light) on y = 2, from x = 2 to x = 93. This is the top highlight.
5. Draw a line in #8 (sand dark) on y = 21, from x = 2 to x = 93. This is the bottom shade.

**Frame 1 (hover):** Right-click frame 0 → **Duplicate Frame**.
1. Recolor the outline from #1 to #5 (orange). Use the Bucket with **Contiguous off** or **Replace Color** mode.
2. Recolor the fill #7 to #6.
3. The highlight on y = 2 is now invisible, so leave it.

**Frame 2 (pressed):** Duplicate frame 0 again.
1. Fill = #8. Repaint the y = 2 and y = 21 lines in #8 as well.
2. Box-select everything **inside** the outline (x 1–94, y 1–22) and nudge it down 1 px with the arrow keys. The button now looks pushed in.

**Tags:** select frame 0 → **Frame → Tags → New Tag** → `normal`. Repeat for frame 1 (`hover`) and frame 2 (`pressed`).

**Export:** File → Export As → `assets/art/ui/shared/ui_btn_normal.png`, scale 1×. If Aseprite saves only one frame, click each frame and export it by hand as `ui_btn_normal.png`, `ui_btn_hover.png`, `ui_btn_pressed.png`. Check each PNG is 96×24.

### File 2: `art_source/ui/shared/ui_panel.aseprite`
Canvas **24×24**, 1 frame. You don't need it for Start, but Pause and Game Over reuse it.
1. 1 px #1 outline around the canvas. Erase the 4 corner pixels (0,0), (23,0), (0,23), (23,23).
2. Fill the inside with #6.
3. Keep the 8 px edge strips plain, because this is a 9-patch.
4. Export to `assets/art/ui/shared/ui_panel.png`.

### File 3: `art_source/ui/start/ui_title.aseprite`
Canvas **288×48**, 1 frame. "KEEP-OSTRICHING" is 15 characters × 19 px (15 wide + 4 gap) = 285 px.
1. Set **View → Grid → Grid Settings**: width 19, height 48. Turn on the grid and snap, so each character gets one column.
2. Draw each letter inside its cell, about 15 px wide and 14 px tall, at y 14–27, in #6 or #12.
3. Add a 1 px #1 outline around every letter.
4. Add a 2 px drop shadow down-right in #1.
5. Export to `assets/art/ui/start/ui_title.png`.

**Don't let this block you.** Use a Label with your pixel font as the title until this is finished.

### Background
Skip the image for now. A ColorRect will do. When you draw the real one, make it **640×320**.

---

## Part C: Godot import check
Click each PNG in FileSystem → Import dock → Preset **2D Pixel** → **Reimport**. If you set the default preset in Import Defaults, this is already done.

---

## Part D: Build the scene (`scenes/ui/StartMenu.tscn`)

**Target tree:**
```
StartMenu (CanvasLayer)        ← script: start_menu.gd
└─ Root (Control)              ← Full Rect
   ├─ Background (ColorRect)   ← Full Rect, first child
   ├─ Title (TextureRect)
   └─ Buttons (VBoxContainer)
      ├─ StartButton (TextureButton)
      │  └─ Label "START"
      └─ QuitButton (TextureButton)
         └─ Label "QUIT"
```

1. **Root:** right-click `Root` → **Change Type** → `Control`. Then toolbar Layout → **Anchors Preset → Full Rect**.
2. **Background:** right-click Root → Add Child Node → `ColorRect`. Rename it `Background`. Layout → Full Rect. In the Inspector set **Color** = `#e2582d`. Drag it so it's the **first** child of Root, because order = draw order.
3. **Title:**
   - Delete the old `TitleLabel` (or keep it until your image is ready).
   - Add a `TextureRect` named `Title` and drag `ui_title.png` into **Texture**.
   - Layout → Anchors Preset → **Center Top**. Then set **Position Y = 40**. The image is 288 wide, so it centers on its own. If you set position by hand instead: **(176, 40)**.
4. **Buttons container:**
   - Rename your existing VBoxContainer to `Buttons`. It must be a child of Root.
   - Inspector → **Theme Overrides → Constants → Separation = 8**.
   - Layout → Anchors Preset → **Center**. Then set **Grow Horizontal = Both** and **Grow Vertical = Both**. The stack is 96×56, so it sits dead center. If you set position by hand instead: **(272, 190)**.
5. **StartButton:**
   - Delete the old `Button`. Right-click `Buttons` → Add Child Node → `TextureButton`, named `StartButton`.
   - Inspector → **Textures** group:
     - Normal = `ui_btn_normal.png`
     - Pressed = `ui_btn_pressed.png`
     - Hover = `ui_btn_hover.png`
     - **Focused = `ui_btn_hover.png`** (this is what makes keyboard and gamepad navigation visible)
   - Set **Stretch Mode = Keep**. Leave **Ignore Texture Size** off, so the button is 96×24.
6. **Label child:** right-click StartButton → Add Child Node → `Label`.
   - Text = `START`.
   - Layout → Full Rect.
   - Horizontal Alignment and Vertical Alignment = **Center**.
   - Control → Mouse → **Mouse Filter = Ignore**. Without this the label eats clicks and hover breaks.
7. **QuitButton:** select StartButton → **Ctrl+D** (duplicate). Rename the copy to `QuitButton` and change its label to `QUIT`.
8. **Pixel font:** if you haven't done step 0.6 yet (Theme resource with an imported pixel font, set under GUI → Theme → Custom), do it now. Otherwise the labels look smooth against your pixel art.

---

## Part E: Script (`start_menu.gd`)

1. Select `StartButton` → **Node dock → Signals → `pressed()`** → double-click → choose the `StartMenu` root node → **Connect**. Godot reuses `_on_start_button_pressed` if it still exists. Make sure it still calls `change_scene_to_file` with your Main scene path.
2. Do the same for `QuitButton`. In its function, call `get_tree().quit()`.
3. In `_ready()`, call `grab_focus()` on StartButton. Get the node path by dragging StartButton from the Scene dock into the script editor while holding **Ctrl**. It will be `$Root/Buttons/StartButton`.

---

## Part F: Checks
- Hover and press each show the right button image.
- Arrow keys or d-pad move the highlight between START and QUIT, and Enter activates.
- Nothing is blurry at 1280×640. Every pixel should be a clean square.
- START loads Main. QUIT closes the game.
- Make the window larger. The menu should stay centered.

If something breaks, tell me the step number and paste the Debugger error.

📖 Keep reading your game mechanics/engine book. The UI and input-focus chapters match exactly what you just built.