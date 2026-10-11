# How to Make UI (Menus & Buttons) — Godot + Aseprite Workflow

## Step 0: Plan before you draw

1. Decide your **game resolution** first (e.g. 1152x648, 1280x720, or 320x180 if you upscale pixels).
2. Decide a **UI style guide**: 1 accent color, 1 base color, 1 font, 1 border style. Stick to it everywhere.
3. Sketch the menu on paper: what buttons exist, in what order, on which screens (main menu, pause, settings, game over).
4. Every interactive element needs a **normal**, **hover**, and **pressed** look. Plan for 3 states from the start — retrofitting them later is painful.

## Step 1: Make the art in Aseprite

1. **File → New**, set the canvas to match your button size, e.g. **48x16 px**.
2. Turn on the **pixel grid** (View → Pixel Grid) and **snap to grid**.
3. Draw the button at the **smallest size first** — it's easier to scale pixel art up than to shrink it.
4. Keep a **1px empty margin** inside the border so text doesn't touch the edges.
5. Use **limited colors**: 2 shades for the base (light top, dark bottom), 1 border color, 1 accent.
6. For each button:
   - **Normal**: base colors.
   - **Hover**: lighten the base or swap the accent color.
   - **Pressed**: darker base, and draw the border "sunken" (shadow at top, highlight at bottom).
7. Use **layers**: base shape on one layer, border on another, highlight on a third. Then save a copy, hide the highlight layer, save again — quick variants without redrawing.
8. Optional: draw a **disabled** state (greyed out) and a **focus** state (brighter outline) — Godot supports both.
9. **File → Export** each state as a separate PNG: `btn_normal.png`, `btn_hover.png`, `btn_pressed.png`.
10. Also export a **panel/background** PNG (e.g. 96x64 with a border) for menus, and optionally a **slider track/knob**, **checkbox**, and **scrollbar** if your settings menu needs them.

### Aseprite tips for UI work

- Make a **Sprite Sheet** (File → Export Sprite Sheet) if you want all button states in one image, then slice it in Godot — but separate PNGs are simpler for beginners.
- Keep a master `.ase` file with all UI elements on separate layers/frames; export PNGs from it. Never edit the PNGs directly — always edit the `.ase` and re-export.
- If your game uses an upscale (e.g. 320x180 rendered at 4x), draw UI at the **native small resolution**, not the upscaled one.

## Step 2: Set up the Godot menu scene

1. **Scene → New Scene → Other Node → Control**. Name it `MainMenu` (or `PauseMenu`).
2. Set the root Control's **Layout → Full Rect** so it always fills the window.
3. Add a **CenterContainer** (or `VBoxContainer`) as a child — containers auto-align their children.
4. Inside it, add your **Buttons**: Start, Settings, Quit.
5. Select each Button → Layout → and give it a **minimum size** (e.g. 120x32) so the container spaces them evenly.
6. Add a **TextureRect** as the *first* child (so it draws behind) for the menu background: set **Full Rect**, Stretch Mode → **Keep Aspect Covered**, Expand Mode → **Ignore Size**.

```
MainMenu (Control, Full Rect)
├── TextureRect (background)
└── CenterContainer
    └── VBoxContainer
        ├── Label (title)
        ├── Button "START"
        ├── Button "SETTINGS"
        └── Button "QUIT"
```

## Step 3: Wire the theme

1. Create a **Theme** resource: right-click in FileSystem → New Resource → `Theme`. Name it `ui_theme.tres`.
2. Open it, click the "+" next to **Type Variation** or select the **Button** base type.
3. Under **Styles**, assign your PNGs:
   - `normal` → StyleBoxTexture → `btn_normal.png`
   - `hover` → StyleBoxTexture → `btn_hover.png`
   - `pressed` → StyleBoxTexture → `btn_pressed.png`
4. In each **StyleBoxTexture**, set **Texture Margins** (Content Margins) to your border thickness, e.g. **4px on all sides**. This makes it a 9-patch so buttons stretch cleanly to any size.
5. Set the theme on the root Control node (Inspector → Theme → drag `ui_theme.tres`).
6. If you use a custom font: set it under the theme's **Default Font**, with font size 8/16/24 for pixel fonts.

## Step 4: Connect button signals

Don't poll in `_process`. Use signals:

```gdscript
extends Control

@onready var start_button: Button = $CenterContainer/VBoxContainer/StartButton

func _ready():
    start_button.pressed.connect(_on_start_pressed)
    $CenterContainer/VBoxContainer/QuitButton.pressed.connect(get_tree().quit)

func _on_start_pressed():
    get_tree().change_scene_to_file("res://scenes/game.tscn")
```

Or connect in the editor: select the button → **Node** tab → double-click `pressed` → pick the method.

## Step 5: Pause menu specifics

1. Set the pause menu's root Control **Process Mode → Always** (so it works while the tree is paused).
2. In your pause script:

```gdscript
func _unhandled_input(event):
    if event.is_action_pressed("ui_cancel"):
        get_tree().paused = not get_tree().paused
        visible = get_tree().paused
```

3. On the **Resume** button: `get_tree().paused = false; visible = false`.
4. Style a **dim overlay**: add a `ColorRect` (black, alpha 0.5) behind the pause panel.

## Step 6: Keyboard/gamepad navigation (often skipped — don't skip it)

1. Select the root Control → Inspector → **Focus → Mode** on each button should be **All**.
2. Set **Neighbour** properties (top/bottom/left/right focus targets) or let containers handle it automatically with a VBox.
3. Give the focused state a distinct look: in the Button theme, set `focus` style to a **StyleBoxFlat** with a bright outline. Players using keyboard/gamepad need to see where they are.
4. Test with arrow keys and Enter — it should work with zero mouse input.

## Quick checklist before calling it done

- [ ] Buttons visibly change on hover and press
- [ ] Works fully with keyboard/gamepad (focus outline visible)
- [ ] Buttons scale cleanly at different window sizes (9-patch margins set)
- [ ] Pause menu actually pauses the game tree
- [ ] All PNGs exported from the master `.ase` file at the right resolution
- [ ] Settings menu widgets (sliders, checkboxes) match the theme

## Mentor's reminder

State changes (normal/hover/pressed) belong in the **Theme** as styleboxes; smooth motion (a button growing on hover) belongs in a **Tween**. Keep the two separate and your UI code stays tiny. *Game Programming Patterns* by Robert Nystrom hto a downloadable `.md` file, or add a section on animated menus (Tween/AnimationPlayer transitions between screens)?