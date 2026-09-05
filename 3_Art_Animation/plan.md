  ── PIXEL ART & ANIMATION MASTERY (20 days, self-paced)
/
  ├── Phase 1: Pixel Art Foundations (5 days)
  │   ├── Day 1 — Aseprite Basics
  │   │   ├── Interface: canvas, layers, timeline, color palette
  │   │   ├── Tools: pencil, bucket, line, rectangle, eraser
  │   │   ├── Resolution: 16x16, 32x32, 64x64 canvas sizes
  │   │   ├── Color theory: limited palettes (Pico-8, DawnBringer)
  │   │   └── SKIP: Advanced brush engines, custom scripts
  │   │
  │   ├── Day 2 — Shapes, Shading & Form
  │   │   ├── Drawing basic shapes: circle, cylinder, cube in pixel
  │   │   ├── Anti-aliasing vs hard edges
  │   │   ├── Light source consistency
  │   │   ├── Dithering techniques
  │   │   └── SKIP: Complex perspective, isometric drawing
  │   │
  │   ├── Day 3 — Character Design
  │   │   ├── Silhouette readability
  │   │   ├── Chibi proportions (big head, small body)
  │   │   ├── Color blocking: base → shadow → highlight
  │   │   ├── Turnaround: front, side, back, 3/4 view
  │   │   └── SKIP: Complex anatomy, realistic proportions
  │   │
  │   ├── Day 4 — Environment & Props
  │   │   ├── Tileable textures: grass, dirt, stone
  │   │   ├── Props: crates, barrels, trees, rocks
  │   │   ├── Background layers: parallax-ready elements
  │   │   ├── Consistent scale across all assets
  │   │   └── SKIP: Complex architectural perspective
  │   │
  │   └── Day 5 — UI & Effects
  │       ├── Buttons, panels, health bars
  │       ├── Icons: coins, hearts, stars
  │       ├── Particle sprites: sparks, dust, blood
  │       ├── Font pixel art (optional)
  │       └── SKIP: Complex UI animations
  │
  ├── Phase 2: Animation Principles (5 days)
  │   ├── Day 6 — Animation Basics
  │   │   ├── Keyframes vs in-betweens
  │   │   ├── Onion skinning in Aseprite
  │   │   ├── Frame duration: hold frames, timing
  │   │   ├── Looping vs one-shot animations
  │   │   └── SKIP: Traditional 12 principles deep dive
  │   │
  │   ├── Day 7 — Character Idle & Walk
  │   │   ├── Idle: subtle breathing, weight shift (2-4 frames)
  │   │   ├── Walk cycle: contact, down, pass, up poses
  │   │   ├── 3-frame vs 6-frame vs 8-frame walk cycles
  │   │   ├── Export: PNG sequence for Godot
  │   │   └── SKIP: Run cycles (similar, just faster)
  │   │
  │   ├── Day 8 — Action Animations
  │   │   ├── Jump: anticipation, apex, landing
  │   │   ├── Attack: windup, active frame, recovery
  │   │   ├── Hurt/take damage: flash, knockback frame
  │   │   ├── Death: collapse, poof, fade
  │   │   └── SKIP: Complex combo animations
  │   │
  │   ├── Day 9 — Effects Animation
  │   │   ├── Explosion: flash, expand, dissipate
  │   │   ├── Projectile: spawn, travel, impact
  │   │   ├── Dust clouds on land/jump
  │   │   ├── UI animations: button press, menu slide
  │   │   └── SKIP: Complex fluid simulations
  │   │
  │   └── Day 10 — Creature & Enemy Design
  │       ├── Silhouette variation for enemy types
  │       ├── Boss design: readable attack tells
  │       ├── Color coding: weak = green, strong = red
  │       ├── Animation states: idle, aggro, attack, death
  │       └── SKIP: Complex multi-part creatures
  │
  ├── Phase 3: Godot Integration (5 days)
  │   ├── Day 11 — Importing to Godot
  │   │   ├── PNG sequence → AnimatedSprite2D
  │   │   ├── Sprite sheet slicing in Godot
  │   │   ├── Setting FPS per animation
  │   │   ├── Loop vs one-shot settings
  │   │   └── SKIP: Custom importers, editor plugins
  │   │
  │   ├── Day 12 — Animation States in Code
  │   │   ├── $AnimatedSprite2D.play("run")
  │   │   ├── Animation finished signals
  │   │   ├── Blending: walk → run transition
  │   │   ├── Flip_h for direction change
  │   │   └── SKIP: AnimationTree (overkill for pixel art)
  │   │
  │   ├── Day 13 — Hitbox Sync with Animation
  │   │   ├── Attack frame = active hitbox
  │   │   ├── Using frame_changed signal
  │   │   ├── Enabling/disabling CollisionShape2D per frame
  │   │   ├── Visual hitbox debug (modulate color)
  │   │   └── SKIP: Complex frame data tables
  │   │
  │   ├── Day 14 — Juice: Code + Art Together
  │   │   ├── Screen shake on heavy attack
  │   │   ├── Frame freeze (hitstop) on impact
  │   │   ├── Particle burst at impact point
  │   │   ├── Camera zoom on critical hit
  │   │   └── SKIP: Shader-based post-processing
  │   │
  │   └── Day 15 — Tilemap Art Pipeline
  │       ├── Designing 16x16 or 32x32 tiles
  │       ├── Autotile rules in Godot TileSet
  │       ├── Terrain sets for auto-painting
  │       ├── Decorative tiles: grass variants, rocks
  │       └── SKIP: Complex procedural tile generation
  │
  └── Phase 4: Polish & Style (5 days)
      ├── Day 16 — Consistent Style Guide
      │   ├── Define your palette (max 32 colors)
      │   ├── Outline rules: always/never/selective
      │   ├── Shadow direction consistency
      │   ├── Scale rules: player = 32px, enemy = 48px, etc.
      │   └── SKIP: Complex style bibles
      │
      ├── Day 17 — Background & Parallax
      │   ├── Layered backgrounds: sky, mountains, trees
      │   ├── Parallax speed multipliers in Godot
      │   ├── Atmospheric perspective (distant = lighter/bluer)
      │   ├── Seamless looping backgrounds
      │   └── SKIP: Complex shader backgrounds
      │
      ├── Day 18 — Lighting in Pixel Art
      │   ├── Rim lighting for characters
      │   ├── Torch/candle flicker effects
      │   ├── Day/night cycle (palette shifting)
      │   ├── Godot Light2D + normal maps (optional)
      │   └── SKIP: Real-time global illumination
      │
      ├── Day 19 — Full Asset Pack Creation
      │   ├── Character: idle, walk, jump, attack, hurt, death
      │   ├── Environment: 20+ tiles, 10+ props
      │   ├── Enemies: 2-3 types with full animation sets
      │   ├── UI: full HUD, menus, buttons
      │   └── SKIP: Massive 100+ asset packs
      │
      └── Day 20 — Portfolio Piece
          ├── Create one complete game mockup screenshot
          ├── Animated GIF showcase of all animations
          ├── Itch.io page with asset pack
          ├── Consistent style across every pixel
          └── SKIP: Commercial asset store submission