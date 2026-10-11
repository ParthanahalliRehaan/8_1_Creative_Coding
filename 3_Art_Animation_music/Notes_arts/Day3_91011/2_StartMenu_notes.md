# Start Menu: full instructions v2 (640×320, Godot 4.7)

**Watch first (optional):** https://www.youtube.com/watch?v=-qJo5AfnB0g and https://www.youtube.com/watch?v=J5HlXFguaX0

---

## 0. Pick your path (this was the confusion in v1)

v1 mixed two different ways of building UI. Both are valid. They differ in **where the look comes from**.

|                    | Path 1: Theme-only                        | Path 2: Art-first                   | Path 3: Hybrid (recommended)                                       |
| --------------------| -------------------------------------------| -------------------------------------| --------------------------------------------------------------------|
| Look comes from    | `StyleBoxFlat` + colors + font in a Theme | PNGs drawn in Aseprite              | PNGs, but plugged into the Theme                                   |
| Button node        | `Button`                                  | `TextureButton`                     | `Button`                                                           |
| Text               | Button's own `Text`                       | separate `Label` child              | Button's own `Text`                                                |
| Panel/background   | `PanelContainer`, `ColorRect`             | `TextureRect`, `NinePatchRect`      | `PanelContainer` with `StyleBoxTexture`, `TextureRect` for scenery |
| Resize a button    | free                                      | art is fixed 96×24                  | free (9-patch stretches)                                           |
| Restyle everything | edit Theme once                           | re-export PNGs, re-wire each button | edit Theme once                                                    |
| Needs Aseprite     | no                                        | yes                                 | yes (small)                                                        |

**What you built (Control → PanelContainer → VBoxContainer → RichTextLabel + Button + Theme + Theme Variation + color palette) is Path 1.** It is correct and it is the right choice for the BlackOut (rectangles) phase. When the Art phase starts, you upgrade to Path 3 without rebuilding the scene: only the Theme changes.

Rule of thumb:
- Menu buttons, panels, labels → **Theme** (Path 1 or 3).
- Scenery and one-off pictures (logo, backdrop, icons) → **TextureRect**.
- `TextureButton` only for odd-shaped buttons (round icon, gear, speaker) or when one image per state is simpler than a Theme.

---

## Part A: Project Settings (Project → Project Settings, Advanced Settings ON)

| Setting | Value |
|---|---|
| Display → Window → Size → Viewport Width / Height | **640 / 320** |
| Display → Window → Size → Window Width / Height Override | **1280 / 640** |
| Display → Window → Stretch → Mode | **viewport** |
| Display → Window → Stretch → Aspect | **keep** |
| Display → Window → Stretch → Scale Mode | **integer** |
| Rendering → Textures → **Canvas Textures** → Default Texture Filter | **Nearest** |
| GUI → Theme → Custom | `res://resources/menu_theme.tres` (set after Part B) |

Why it matters: with `viewport` stretch the whole game renders at 640×320 and is then enlarged 2×. So **UI sizes and font sizes are in 640×320 pixels**, not 1280×640. A 16 px font is already big.

(v1 listed the texture filter without the "Canvas Textures" sub-group. That is where it actually lives.)

---

## Part B: Shared setup (all paths)

### B1. Folders
```
assets/art/ui/shared/    ui_btn_*.png, ui_panel.png
assets/art/ui/start/     ui_title.png
assets/fonts/            fonts.ttf (or my_font.png)
resources/               menu_theme.tres, menu_color_palette.tres
scenes/ui/               StartMenu.tscn
art_source/ui/...        .aseprite files
```

### B2. Palette (write the hex values down once)
Open your `.gpl` and fill this table. Every color in the UI comes from here.

| # | Name | Hex |
|---|---|---|
| 1 | dark (outlines) | #______ |
| 5 | orange (hover/accent) | #______ |
| 6 | sand light | #______ |
| 7 | sand mid | #______ |
| 8 | sand dark | #______ |
| bg | background | #e2582d |

### B3. Font
1. FileSystem → click `fonts.ttf` → **Import** tab.
2. Set **Antialiasing = Disabled**, **Hinting = None**, **Subpixel Positioning = Disabled**, **Generate Mipmaps = Off** → **Reimport**.
3. Double-click `menu_theme.tres` → Inspector: **Default Font** = `fonts.ttf`, **Default Font Size** = the font's native pixel size, or a multiple of it (e.g. 8, 16).

### B4. Theme Type Variations (do this once)
In the Theme editor, **+ Add Type**:
- `TitleLabel`, Base Type `Label`, Font Size 32, Font Color = palette 6.
- `DangerButton`, Base Type `Button`, for Quit later.

Apply with Inspector → **Theme → Theme Type Variation** on the node.

### B5. Target scene tree (same for every path)

```
StartMenu (CanvasLayer)            ← start_menu.gd
└─ Root (Control)                  ← Full Rect
   ├─ Background (ColorRect)       ← Full Rect, FIRST child (order = draw order)
   └─ Center (CenterContainer)     ← Full Rect
      └─ Menu (VBoxContainer)      ← Separation 24
         ├─ Title (Label)          ← variation TitleLabel
         └─ Buttons (VBoxContainer)← Separation 8
            ├─ StartButton (Button)
            └─ QuitButton (Button)
```

Why `CenterContainer` instead of typing pixel positions: v1 used hand-typed positions like (176, 40) and (272, 190). Those break the moment you change a size. Also v1's (272, 190) was not "dead center" (center y would be 132). A container centers everything automatically, at any window size.

Build it:
1. Select `Root` → toolbar **Layout → Anchors Preset → Full Rect**.
2. Add `ColorRect` named `Background` → Full Rect → Color `#e2582d`.
3. Add `CenterContainer` named `Center` → Full Rect.
4. Inside it add `VBoxContainer` named `Menu` → Theme Overrides → Constants → Separation = 24.
5. Add `Label` `Title` (text `KEEP-OSTRICHING`, Theme Type Variation = `TitleLabel`) and `VBoxContainer` `Buttons` (Separation = 8).
6. Under `Buttons` add two `Button` nodes: `StartButton` (Text `START`) and `QuitButton` (Text `QUIT`).
7. Right-click `StartButton` and `QuitButton` → **Access as Unique Name** (a `%` appears). Do the same for `Title` if you want.
8. Select `Buttons` → Inspector → Layout → **Custom Minimum Size X = 96** so both buttons share one width.

> `RichTextLabel` note: you used it for the title. It works, but inside a container it collapses to zero height unless you turn on **Fit Content** (and give it a Custom Minimum Size X). Also its Mouse Filter defaults to **Stop**, so it can block clicks; set it to **Ignore** if it sits on top of a button. A plain `Label` has neither problem. Use `RichTextLabel` only when you need BBCode (colors, wave, shake).

---

## Part C: Path 1, Theme-only (no art)

Open `menu_theme.tres`. In the Theme editor select the **Button** type (add it with **+ Add Type** if missing) → **Styles** tab.

**normal** → New `StyleBoxFlat`:
1. Bg Color = palette 7.
2. Border Width: all four = 1. Border Color = palette 1.
3. Corner Radius: all four = 2, **Corner Detail = 1**. This gives blocky pixel corners.
4. **Anti Aliasing = Off** (otherwise edges blur).
5. Content Margin: Left/Right = 8, Top/Bottom = 4.

**hover** → copy the normal style, then: Bg = palette 6, Border Color = palette 5.

**pressed** → copy normal, then: Bg = palette 8, Content Margin Top = 5, Bottom = 3 (text sinks 1 px, button looks pushed).

**focus** → New `StyleBoxFlat`: **Draw Center = Off**, Border width 1, Border Color = palette 5. This is what shows keyboard/gamepad selection.

**disabled** → copy normal, Bg = a gray from your palette.

Then in the **Colors** tab for Button: Font Color = palette 1, Font Hover Color = palette 1, Font Pressed Color = palette 1.

Panel behind the menu (optional): wrap `Menu` in a `PanelContainer`, then in the Theme select type **PanelContainer** → Styles → **panel** → `StyleBoxFlat` with Bg = palette 6, border 1 = palette 1, content margins 12.

Done. No images needed.

---

## Part D: Path 2, Art-first (TextureButton)

### D1. `art_source/ui/shared/ui_buttons.aseprite`
Canvas **96×24**, **4 frames**. File → New: Color Mode **RGBA**, Background **Transparent**. Load your `.gpl` (palette menu → Load Palette). View → Show → **Pixel Grid**. Coordinates are (x, y) from the top-left.

**Frame 0 (normal):**
1. Palette 1: draw a 1 px outline around the whole canvas (x 0–95, y 0–23).
2. Erase 8 pixels for rounded corners: (0,0), (95,0), (0,23), (95,23), (1,1), (94,1), (1,22), (94,22).
3. Bucket-fill the inside with 7.
4. Line in 6 on y = 2, x = 2 to 93 (top highlight).
5. Line in 8 on y = 21, x = 2 to 93 (bottom shade).

**Frame 1 (hover):** right-click frame 0 → **Duplicate Frame**.
1. Recolor outline 1 → 5 (Bucket with **Contiguous off**, or Replace Color).
2. Recolor fill 7 → 6. The highlight line becomes invisible. Leave it.

**Frame 2 (pressed):** duplicate frame 0 again.
1. Fill = 8, and repaint the y = 2 and y = 21 lines in 8.
2. Box-select everything inside the outline (x 1–94, y 1–22) and nudge it down 1 px.

**Frame 3 (disabled):** duplicate frame 0, recolor fill and outline to grays/darker sand.

**Tags:** Frame → Tags → New Tag: `normal` (0), `hover` (1), `pressed` (2), `disabled` (3).

**Export:** File → Export As, name it `ui_btn_{tag}.png`, Frames = **All frames**, scale 1×. If your Aseprite version does not split by tag, export each frame by hand as `ui_btn_normal.png`, `ui_btn_hover.png`, `ui_btn_pressed.png`, `ui_btn_disabled.png` into `assets/art/ui/shared/`. Check each PNG is **96×24**.

### D2. `ui_panel.aseprite`
Canvas **24×24**, 1 frame.
1. 1 px outline in 1, erase the 4 corner pixels (0,0), (23,0), (0,23), (23,23).
2. Fill the inside with 6.
3. Keep the 8 px edge strips plain (this is a 9-patch with 8 px margins).
4. Export to `assets/art/ui/shared/ui_panel.png`.

### D3. `ui_title.aseprite`
Canvas **288×48**, 1 frame. "KEEP-OSTRICHING" = 15 characters × 19 px (15 wide + 4 gap) = 285 px.
1. View → Grid → Grid Settings: width 19, height 48; turn on grid and snap.
2. Draw each letter in its cell (about 15×14 px, y 14–27) in 6 or 12.
3. 1 px outline in 1 around each letter, 2 px drop shadow down-right in 1.
4. Export to `assets/art/ui/start/ui_title.png`.

Not blocking: keep the `Label` title until this is done.

### D4. Godot import check
Click each PNG → Import dock → Preset **2D Pixel** → **Reimport** (or set it once in Project → Project Settings → Import Defaults).

### D5. Swap the scene nodes
1. Replace `StartButton`/`QuitButton` with `TextureButton` (right-click → Change Type).
2. Inspector → **Textures**: Normal = `ui_btn_normal.png`, Pressed = `ui_btn_pressed.png`, Hover = `ui_btn_hover.png`, Disabled = `ui_btn_disabled.png`, **Focused = `ui_btn_hover.png`**.
3. **Stretch Mode = Keep**, leave **Ignore Texture Size** off (button stays 96×24).
4. Add a child `Label` with the text → Layout Full Rect → Alignment Center/Center → **Mouse Filter = Ignore** (Label defaults to Ignore already, RichTextLabel does not).
5. Title: swap `Title` to a `TextureRect`, Texture = `ui_title.png`.
6. Optional picture backdrop: replace `Background` with `TextureRect` → Full Rect → **Expand Mode = Ignore Size**, **Stretch Mode = Keep Aspect Covered**, image **640×320**.

Cost of this path: you maintain 4 PNGs per button style, the label does not scale with the art, and every new button needs the same wiring.

---

## Part E: Path 3, Hybrid (recommended once art exists)

Keep the Path 1 scene exactly as it is. Only edit the Theme:

1. Export the PNGs from D1 and D2.
2. `menu_theme.tres` → Button type → Styles → `normal`: replace the `StyleBoxFlat` with a new **StyleBoxTexture**, Texture = `ui_btn_normal.png`.
3. In that StyleBoxTexture: **Texture Margins** Left/Right/Top/Bottom = 4 (your border thickness). That makes it a 9-patch. Content Margin L/R = 8, T/B = 4 so text does not touch the border.
4. Repeat for `hover`, `pressed`, `focus` (use the hover PNG), `disabled`.
5. PanelContainer type → `panel` → StyleBoxTexture with `ui_panel.png`, Texture Margins = 8.
6. Run. Nothing in the scene changed, but every button and panel now uses your art, and they resize freely.

Use `TextureRect` only for the title logo and scenery.

---

## Part F: Script (`start_menu.gd`)

```gdscript
extends CanvasLayer

@export_file("*.tscn") var main_scene: String  # pick the scene in the Inspector

@onready var start_button: Button = %StartButton
@onready var quit_button: Button = %QuitButton

func _ready() -> void:
    start_button.pressed.connect(_on_start_pressed)
    quit_button.pressed.connect(_on_quit_pressed)
    start_button.grab_focus()  # keyboard/gamepad can navigate immediately
    if OS.has_feature("web"):
        quit_button.hide()  # quitting does nothing in a browser

func _on_start_pressed() -> void:
    get_tree().change_scene_to_file(main_scene)

func _on_quit_pressed() -> void:
    get_tree().quit()
```

Steps:
1. Select the `StartMenu` root → attach this script.
2. In the Inspector, set **Main Scene** to your Main scene file.
3. `%StartButton` works because of **Access as Unique Name** (Part B5). It keeps working if you move nodes around, unlike `$Root/Center/Menu/Buttons/StartButton`.

(If you prefer the editor way: select the button → Node dock → Signals → `pressed()` → Connect. The code way above does the same and is easier to read later.)

If `Button` is `TextureButton` (Path 2), change the type hints to `TextureButton`.

---

## Part G: Checks

- Hover, press, focus and disabled each look different.
- Arrow keys or d-pad move between START and QUIT, Enter activates.
- Nothing is blurry at 1280×640. Every pixel is a clean square (if not: re-check font import, Anti Aliasing Off, texture filter Nearest).
- START loads Main. QUIT closes the game.
- Resize the window. The menu stays centered, and you only see black bars on the sides (integer scaling).
- Change one color in the Theme. Every button and panel updates.

## Troubleshooting

| Symptom | Likely cause |
|---|---|
| Text is smooth/blurry | Font import settings, or font size is not a multiple of the pixel size |
| Button edges blurry | `StyleBoxFlat` Anti Aliasing on, or Default Texture Filter not Nearest |
| RichTextLabel disappears in a container | Fit Content off, no Custom Minimum Size |
| Clicks do nothing | Something on top has Mouse Filter = Stop |
| No focus highlight | `focus` style empty, or Focused texture not set |
| Theme not applied to a new scene | Theme is not on the root Control, or not set in GUI → Theme → Custom |
| Title off-center | Used manual position instead of `CenterContainer` |

## Polish later (not in BlackOut)
- Hover grow: Tween on `scale` with `pivot_offset = size / 2` (update it on the `resized` signal).
- Click sound: an `AudioStreamPlayer` called from `pressed`.
- Fade-in: Tween `Root.modulate:a` from 0 to 1 in `_ready()`.

If something breaks, tell me the step number and paste the Debugger error.

📖 Keep reading your game mechanics/engine book. The UI and input-focus parts match what you just built.