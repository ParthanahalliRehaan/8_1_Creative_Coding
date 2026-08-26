# Art, Animation & Music!
```
🎨 Pixel Art, Animation & Music Mastery (20 Days)
│
├── 🖌️ Phase 1: Pixel Art Foundations (Days 1–5)
│   │
│   ├── Day 1 — Aseprite Setup & Basic Shapes
│   │   ├── Install Aseprite, set 16×16 canvas
│   │   ├── Tools: pencil, eraser, line, bucket
│   │   ├── Load DawnBringer 16 palette
│   │   └── Draw: 16×16 crate, 16×16 pot
│   │
│   ├── Day 2 — Shading & Light Direction
│   │   ├── 3-shade rule: base, shadow, highlight
│   │   ├── Light source: top-left convention
│   │   └── Draw: 16×16 sword, 24×24 rock
│   │
│   ├── Day 3 — Sprite Sheet Basics
│   │   ├── Canvas size: 32×32 for characters
│   │   ├── Export as sprite sheet (File → Export Sprite Sheet)
│   │   └── Draw: 32×32 character idle (1 frame)
│   │
│   ├── Day 4 — Simple Animation
│   │   ├── Onion skinning (Alt+O)
│   │   ├── Frame timing: 100ms = 10 FPS
│   │   └── Animate: 6-frame coin spin
│   │
│   └── Day 5 — Tileset Creation
│       ├── Seamless tiling: edges must match
│       ├── 4-tile rule: corner, edge, fill, variation
│       └── Draw: 4 grass tiles + 2 dirt tiles
│
│   🎯 Week 1 Deliverable: Crate, pot, sword, rock, character idle, coin spin, grass tileset
│
├── 🏃 Phase 2: Character Animation (Days 6–10)
│   │
│   ├── Day 6 — Character Design (4 Directions)
│   │   ├── Front, back, left, right poses
│   │   ├── Silhouette readability
│   │   └── Draw: 32×32 player in 4 directions
│   │
│   ├── Day 7 — Walk Cycle
│   │   ├── 4-frame walk: contact, down, pass, up
│   │   ├── Tag animations in Aseprite timeline
│   │   └── Animate: walk cycle for one direction
│   │
│   ├── Day 8 — Jump & Fall
│   │   ├── Anticipation frame (squash before jump)
│   │   ├── Mid-air pose, landing pose
│   │   └── Animate: jump up + fall down
│   │
│   ├── Day 9 — Attack Animation
│   │   ├── 3 frames: windup, strike, recovery
│   │   ├── Impact frame: white flash or shake
│   │   └── Animate: sword slash
│   │
│   └── Day 10 — Enemy Design
│       ├── 2 enemy types: slime + bat
│       ├── Idle + hurt animations each
│       └── Draw: slime idle (2 frames), bat fly (4 frames)
│
│   🎯 Week 2 Deliverable: Player sprite sheet (idle, walk 4-dir, jump, fall, attack) + 2 enemies
│
├── ✨ Phase 3: Effects, UI & Environment (Days 11–15)
│   │
│   ├── Day 11 — Particle Sprites
│   │   ├── 8×8 size: spark, dust puff, blood drop
│   │   ├── Fade to transparent over frames
│   │   └── Draw: 3 particle types, 4 frames each
│   │
│   ├── Day 12 — Background Parallax
│   │   ├── 3 layers: sky, midground, foreground
│   │   ├── Different pixel scales per layer
│   │   └── Draw: 320×180 parallax set
│   │
│   ├── Day 13 — UI / HUD Elements
│   │   ├── Health bar: frame + fill + segments
│   │   ├── Button states: normal, hover, pressed
│   │   └── Draw: full UI kit in 16×16 and 32×32
│   │
│   ├── Day 14 — Animation Principles
│   │   ├── Squash & stretch: heavy attack
│   │   ├── Follow-through: cape/hair movement
│   │   └── Animate: complex heavy attack with juice
│   │
│   └── Day 15 — Full Scene Mockup
│       ├── Color scripting: warm = safe, cool = danger
│       ├── Apply full palette to one screen
│       └── Draw: one complete game screen
│
│   🎯 Week 3 Deliverable: Particles, parallax layers, UI kit, polished attack, scene mockup
│
└── 🎵 Phase 4: Music, SFX & Godot Integration (Days 16–20)
    │
    ├── Day 16 — LMMS Setup & First Loop
    │   ├── Install LMMS
    │   ├── Beat+Bassline editor: 4-bar drum loop
    │   ├── Piano roll: simple melody
    │   └── Export: 1 chiptune loop as OGG
    │
    ├── Day 17 — Music Structure
    │   ├── Intro → Loop → Outro arrangement
    │   ├── Add bassline (root notes)
    │   └── Export: 1-minute track
    │
    ├── Day 18 — SFX with sfxr
    │   ├── Browser: sfxr.me or drpetter.se/project_sfxr
    │   ├── Presets: jump, coin, explosion, hit
    │   └── Export: 4 WAV files
    │
    ├── Day 19 — Godot Import Pipeline
    │   ├── Pixel art: 2D Pixel preset, nearest filter, no mipmaps
    │   ├── Sprite sheet: slice in AnimationPlayer
    │   ├── Music: OGG loop import
    │   └── SFX: WAV, low latency
    │
    └── Day 20 — Vertical Slice Polish
        ├── One playable level
        ├── 100% your art + animation + music + SFX
        ├── No placeholders
        └── Record GIF, write devlog
    │
    🎯 Week 4 Deliverable: Playable vertical slice with original everything
```

---

## 🛠️ Tools (One Each)

| Need | Tool | Why | Cost |
|------|------|-----|------|
| **Pixel Art + Animation** | Aseprite | You have it. Best for pixel art, sprite sheets, animation | $20 |
| **Music** | LMMS | Full DAW, chiptune-friendly, exports OGG | Free |
| **SFX** | sfxr (browser) | Instant retro sounds, zero learning curve | Free |

---

## 📅 Daily Time Split

```
Each Day:
├── Pixel Art + Animation: 1.5 hours
└── Music / SFX: 30 minutes
    (Flip in Week 4: more music integration)
```

---

## 🎯 Final Deliverable

```
YourGame (Vertical Slice)
├── Player
│   ├── Idle, walk (4-dir), jump, fall, attack
│   └── Hit-flash + particle effects
├── Enemy: slime + bat
│   └── Idle, hurt, death animations
├── World
│   ├── Tileset: grass, dirt, platforms
│   ├── Parallax background
│   └── Particles: dust, spark, coin burst
├── UI
│   ├── Health bar, buttons, inventory slots
│   └── Hover + press states
├── Audio
│   ├── 1 background music track (looping)
│   └── SFX: jump, coin, hit, explosion, UI click
└── All imported and working in Godot
```