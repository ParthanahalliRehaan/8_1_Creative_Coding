
# 🎮 60-Dawn Godot 4 Mastery Plan — Complete Instruction Manual

> **For:** Rehaan  
> **Goal:** Master Godot 4, Pixel Art, Music Composition, and Game Design in 60 focused dawns  
> **Philosophy:** No tutorials. Only docs, articles, and "build this" instructions. Struggle = learning.

---

## The Philosophy

Instead of building 60 disconnected things, you'll build **12 Core Vertical Slices** (one every 5 dawns). Each slice is a complete, playable micro-game with its own mechanics, art, animation, music, and UI. By the end, you'll have a modular toolkit — 12 "Lego sets" you can snap together into any advanced game.

**No code given. Only docs, articles, and "build this" instructions.**

---

## 📊 The 12 Vertical Slices (Progression: Simple → Intermediate → Advanced)

| Dawn Block | Micro-Game / Vertical Slice | Core Mechanics                                                  | Art Focus                                                                    | Music/SFX Focus                                   | UI Focus                                                      | Level Design Focus                    |
| ------------| -----------------------------| -----------------------------------------------------------------| ------------------------------------------------------------------------------| ---------------------------------------------------| ---------------------------------------------------------------| ---------------------------------------|
| 1–5        | 🏃 The Runner　　　　　　　 | Side-scrolling auto-runner, jump, collect coins                 | Pixel art basics, tileset, simple character sprite                           | First chiptune loop, 3 SFX                        | Main menu, HUD (score, lives)                                 | Single-screen linear level            |
| 6–10       | ⚔️ The Fighter　　　　　　　 | Melee combat, hitbox/hurtbox, combos, enemy AI                  | Character animations (idle, walk, attack, hurt), enemy sprites               | Combat music, hit SFX, UI SFX                     | Health bar, combo counter, pause menu                         | Arena-style combat room               |
| 11–15      | 🧙 The Caster　　　　　　　 | Ranged combat, projectile physics, mana system, spell casting   | Projectile sprites, particle effects (sparks, explosions), magic UI          | Magical SFX, ambient music                        | Mana bar, spell hotbar, cooldown indicators                   | Vertical tower climb                  |
| 16–20      | 🏹 The Hunter　　　　　　　 | Stealth mechanics, line of sight, AI patrol/chase, traps        | Stealth UI icons, trap sprites, vision cone visualization                    | Tension music, footstep SFX, alarm SFX            | Stealth meter, minimap, objective markers                     | Multi-room stealth layout             |
| 21–25      | 🚗 The Racer　　　　　　　　 | Top-down driving, drifting physics, boost system, lap timing    | Vehicle sprite, track tiles, skid marks, speed lines                         | Engine rev SFX, racing music, crash SFX           | Lap timer, position tracker, speedometer                      | Curved track with shortcuts           |
| 26–30      | 🏗️ The Builder　　　　　　　| Inventory grid, crafting recipes, resource gathering, placement | Item sprites, crafting UI art, building tiles                                | Building SFX, peaceful music                      | Inventory grid, crafting menu, tooltip system                 | Sandbox building area                 |
| 31–35      | 🌊 The Diver　　　　　　　　| Underwater physics, oxygen meter, buoyancy, treasure hunt       | Underwater parallax, bubble particles, fish sprites                          | Underwater ambience, bubble SFX, sonar ping       | Oxygen bar, depth meter, treasure log                         | Underwater cave system                |
| 36–40      | 🧩 The Puzzle　　　　　　　 | Switch/button logic, pressure plates, sliding blocks, key-door  | Puzzle element sprites, animated doors, pressure plates                      | Puzzle SFX, mysterious ambient music              | Hint system, level select screen, star rating                 | Puzzle room with multiple solutions   |
| 41–45      | 🌌 The Flyer　　　　　　　　| Space shooter, bullet patterns, power-ups, boss fight           | Spaceship sprites, bullet patterns, explosion animations, boss art           | Synthwave music, laser SFX, explosion SFX         | Power-up HUD, boss health bar, score multiplier               | Scrolling space level with boss arena |
| 46–50      | 🗺️ The Explorer　　　　　　 | Procedural generation, fog of war, map discovery, biome system  | Biome tilesets, fog overlay, map icons, discovery animations                 | Biome music transitions, discovery SFX            | World map, legend, discovery log                              | Procedurally generated overworld      |
| 51–55      | 🏰 The Siege　　　　　　　　| Tower defense, pathfinding, tower upgrades, wave system         | Tower sprites, enemy variety (10+ types), projectile variety                 | Intense battle music, tower SFX, wave alert SFX   | Tower upgrade UI, wave info, resource tracker                 | Multi-lane tower defense map          |
| 56–60      | 🎭 The Finale　　　　　　　 | Everything combined. Metroidvania with ALL previous mechanics   | Full character sheet (20+ animations), complete tileset library, full UI kit | Full soundtrack (5+ tracks), complete SFX library | Full game UI: title, settings, inventory, map, dialogue, shop | Multi-biome interconnected world      |

---

## 🗂️ Folder Structure Per Dawn Block

```
Dawn_XX-XX_[SliceName]/
├── 📁 mechanics/
│   ├── docs/              # Links to Godot docs you read
│   ├── notes.md           # YOUR notes after reading
│   └── design.md          # YOUR design decisions
├── 📁 art/
│   ├── aseprite_files/    # .ase files
│   ├── exported/            # .png sprite sheets
│   └── palette.ase          # Your palette file
├── 📁 animation/
│   ├── aseprite_animations/ # Tagged animations
│   └── godot_imports/       # How you imported into Godot
├── 📁 music/
│   ├── lmms_projects/     # .mmpz files
│   ├── exported/            # .ogg loops
│   └── SFX/                 # .wav files from sfxr
├── 📁 UI/
│   ├── design_mockups/    # Reference images / sketches
│   ├── aseprite_UI/         # UI element sprites
│   └── godot_scenes/        # .tscn UI scenes
├── 📁 level_design/
│   ├── sketches/            # Paper sketches / digital layouts
│   ├── tilemap_layers/      # Godot TileMap data
│   └── playtest_notes.md    # What worked, what didn't
└── 📁 vertical_slice/
    ├── project/             # Full Godot project
    ├── builds/              # Exported playable builds
    └── devlog.md            # What you learned, what to reuse
```

---

## 🎯 The "Lego Piece" Reusability System

After each block, tag and catalog reusable assets:

| Catalog File | What You Track |
|---|---|
| mechanics_catalog.md | Each mechanic → which slice it came from → how to reuse |
| art_catalog.md | Each sprite/animation → dimensions → style notes → reuse instructions |
| music_catalog.md | Each track → BPM → mood → loop points → reuse instructions |
| UI_catalog.md | Each UI component → Godot scene path → how to instance |
| level_patterns.md | Each layout pattern → screenshot → when to use |

---

## 📅 Detailed Dawn-by-Dawn Breakdown

---

# 🏃 BLOCK 1: The Runner (Dawns 1–5)
**Goal:** Learn Godot basics + pixel art fundamentals + first music loop + basic UI

---

## DAWN 1: Setup & Core Movement

**Game Mechanic:** Side-Scrolling Auto-Runner Base  
**Origin:** Canabalt (2009) pioneered the infinite runner genre. Temple Run and Subway Surfers popularized it.

**Read:**
- [Godot CharacterBody2D](https://docs.godotengine.org/en/stable/classes/class_characterbody2d.html) — Official docs for 2D character physics body
- [The History of Endless Runners](https://www.gamedeveloper.com/design/the-history-of-endless-runners) — Genre origins and design principles

**Build:** Character that auto-runs right. Space to jump. Gravity. Floor collision.

**Art: First Pixel Art**

**Read:**
- [Aseprite Docs](https://www.aseprite.org/docs/) — Official Aseprite documentation, quick start, workspace, workflow
- [Pixel Art Tutorial for Beginners](https://www.youtube.com/results?search_query=pixel+art+tutorial+beginners) — YouTube search for beginner pixel art tutorials

**Build:** 16×16 player sprite (3 colors). 32×32 floor tile (2 variations). DawnBringer 16 palette.

**Music: LMMS Setup**

**Read:**
- [LMMS Wiki - Getting Started](https://github.com/LMMS/lmms/wiki) — Official LMMS wiki with getting started guide
- [Chiptune Basics](https://www.youtube.com/results?search_query=chiptune+basics) — YouTube search for chiptune fundamentals

**Build:** Install LMMS. Create 4-bar drum pattern. Export as OGG.

**Level Design: Single Screen**

**Read:** [Level Design for 2D Games](https://www.gamedeveloper.com/design/level-design-for-2d-games) — Core principles of 2D level layout

**Build:** One 640×360 screen. Flat ground. One pit to jump over.

**UI: Main Menu**

**Read:** [Godot UI Basics](https://docs.godotengine.org/en/stable/tutorials/ui/index.html) — Official Godot UI tutorial covering Control nodes, anchors, containers

**Build:** Title text, "Start" button, "Quit" button.

---

## DAWN 2: Jump Feel & Animation

**Game Mechanic:** Variable Jump Height + Coyote Time

**Origin:** Super Mario Bros. (1985) invented variable jump height. Celeste perfected coyote time.

**Read:**
- [Celeste's Coyote Time & Forgiveness](https://www.maddymakesgames.com/articles/celeste_and_forgiveness/index.html) — Maddy Thorson (Celeste creator) explains coyote time, jump buffering, and forgiveness mechanics
- [Godot Input Handling](https://docs.godotengine.org/en/stable/tutorials/inputs/input_examples.html) — Official Godot input system docs

**Build:** Hold jump = higher. Release = cut gravity. 5-frame coyote time after leaving platform.

**Art: Player Animation (Idle + Run)**

**Read:**
- [Sprite Sheet Basics](https://docs.godotengine.org/en/stable/tutorials/2d/2d_sprite_animation.html) — Godot 2D sprite animation docs
- [Aseprite Animation](https://www.aseprite.org/docs/animation/) — Official Aseprite animation docs

**Build:** 4-frame run cycle. 2-frame idle bounce. Export as sprite sheet.

**Music: First Melody**

**Read:** [Music Theory for Game Composers](https://www.youtube.com/results?search_query=music+theory+for+game+composers) — YouTube search for game music theory

**Build:** Add simple melody over drum loop. 4 bars. C major pentatonic.

**Level Design: Pacing & Rhythm**

**Read:** [The Rhythm of Level Design](https://www.gamedeveloper.com/design/the-rhythm-of-level-design) — How spacing creates rhythm in levels

**Build:** Space obstacles at intervals matching jump timing.

**UI: HUD (Score + Lives)**

**Read:** [Godot Control Nodes](https://docs.godotengine.org/en/stable/tutorials/ui/index.html) — Godot UI control nodes reference

**Build:** Score counter (top-left). 3 heart lives (top-right).

---

## DAWN 3: Collectibles & Particles

**Game Mechanic:** Coin Collection + Score System

**Origin:** Super Mario Bros. coins. Sonic rings.

**Read:**
- [Godot Area2D](https://docs.godotengine.org/en/stable/classes/class_area2d.html) — Official Area2D docs for collision detection
- [Game Feel - Juiciness](https://www.youtube.com/results?search_query=game+feel+juiciness) — YouTube search for game feel and juiciness

**Build:** Coin Area2D. On collect: +10 score, particle burst, coin disappears.

**Art: Coin Animation + Particles**

**Read:** [Aseprite Onion Skinning](https://www.aseprite.org/docs/onion-skinning/) — Official Aseprite onion skinning docs for animation

**Build:** 6-frame coin spin (8×8). 4-frame sparkle particle (4×4).

**Music: SFX with sfxr**

**Read:**
- [sfxr Documentation](https://sfxr.me/) — Online sfxr tool for retro SFX
- [Retro SFX Design](https://www.youtube.com/results?search_query=retro+sfx+design) — YouTube search for retro sound design

**Build:** Coin collect SFX, jump SFX, game over SFX. Export as WAV.

**Level Design: Risk/Reward Placement**

**Read:** [Risk vs Reward in Level Design](https://www.gamedeveloper.com/design/risk-vs-reward-in-level-design) — Core level design principle

**Build:** Coins placed over pits (risk) vs safe ground (reward).

**UI: Game Over Screen**

**Read:** [Godot Scene Transitions](https://docs.godotengine.org/en/stable/tutorials/scripting/scene_tree.html) — Godot scene management docs

**Build:** "Game Over" text. Final score. "Retry" and "Main Menu" buttons.

---

## DAWN 4: Parallax Backgrounds

**Game Mechanic:** Infinite Scrolling Background

**Origin:** Moon Patrol (1982) first parallax scrolling. Standard in all runners.

**Read:**
- [Godot ParallaxBackground](https://docs.godotengine.org/en/stable/classes/class_parallaxbackground.html) — Official ParallaxBackground docs
- [Godot 2D Parallax Tutorial](https://docs.godotengine.org/en/stable/tutorials/2d/2d_parallax.html) — Official 2D parallax tutorial with Parallax2D node

**Build:** 3-layer parallax (sky, mountains, trees). Auto-scroll. Speed increases over time.

**Art: Background Layers**

**Read:** [Parallax Scrolling in Pixel Art](https://www.youtube.com/results?search_query=parallax+scrolling+pixel+art) — YouTube search for parallax pixel art techniques

**Build:** 320×180 sky (gradient). 320×180 mountains (silhouette). 320×180 trees (detailed).

**Music: Looping Seamlessly**

**Read:** [Seamless Audio Looping](https://www.youtube.com/results?search_query=seamless+audio+looping+lmms) — YouTube search for seamless looping techniques

**Build:** Extend track to 16 bars. Ensure loop point is seamless.

**Level Design: Vertical Layering**

**Read:** [Layering in Level Design](https://www.gamedeveloper.com/design/layering-in-level-design) — Vertical path design principles

**Build:** Upper path (harder, more coins). Lower path (easier, fewer coins).

**UI: Pause Menu**

**Read:** [Godot Pause Mode](https://docs.godotengine.org/en/stable/tutorials/scripting/pausing_games.html) — Official Godot pause system docs

**Build:** Pause with Escape. Resume, Restart, Quit buttons. Time freezes.

---

## DAWN 5: Vertical Slice Polish

**Game Mechanic:** Speed Increase + Difficulty Curve

**Read:** [Difficulty Curves in Game Design](https://www.gamedeveloper.com/design/difficulty-curves-in-game-design) — How to design satisfying difficulty progression

**Build:** Speed increases every 10 seconds. Obstacle frequency increases.

**Art: Full Sprite Sheet Export**

**Read:** [Aseprite Export Sprite Sheet](https://www.aseprite.org/docs/exporting/) — Official Aseprite export docs

**Build:** Export all art. Import into Godot with 2D Pixel preset, nearest filter, no mipmaps.

**Music: Full Track Integration**

**Read:** [Godot AudioStreamPlayer](https://docs.godotengine.org/en/stable/classes/class_audiostreamplayer.html) — Official audio player docs

**Build:** Loop music. SFX on correct buses. Volume mixing.

**Level Design: Complete Level**

**Read:** [Flow in Level Design](https://www.gamedeveloper.com/design/flow-in-level-design) — Mihaly Csikszentmihalyi's flow applied to games

**Build:** 2-minute playable level. Intro → build-up → climax → resolution.

**UI: Polish**

**Read:** [Godot Theme System](https://docs.godotengine.org/en/stable/tutorials/ui/gui_theme_editor.html) — Official Godot theme editor docs

**Build:** Consistent button theme. Hover/pressed states. Screen transitions.

**BLOCK 1 DELIVERABLE:** Playable auto-runner .exe. Catalog all reusable pieces.

---

# ⚔️ BLOCK 2: The Fighter (Dawns 6–10)
**Goal:** Combat systems, state machines, hit systems, enemy AI

---

## DAWN 6: State Machine & Character Design

**Game Mechanic:** Player State Machine

**Origin:** Street Fighter II (1991) codified fighting game states. Hollow Knight uses elegant state machines.

**Read:**
- [Godot State Machine Pattern](https://shaggydev.com/2023/10/08/godot-4-state-machines/) — Node-based state machines in Godot 4
- [Godot Enum State Machine](https://forum.godotengine.org/t/enum-state-machine/100095) — Simple enum-based state machine example
- [State Machines in Games](https://www.gamedeveloper.com/design/state-machines-in-games) — Game design theory on state machines

**Build:** States: Idle, Walk, Attack, Hurt, Die. Enum-based or Node-based.

**Art: 4-Directional Character**

**Read:** [Character Design for Games](https://www.youtube.com/results?search_query=character+design+for+games+pixel+art) — YouTube search for game character design

**Build:** 32×32 character. Front, back, left, right idle poses.

**Music: Combat Music Structure**

**Read:** [Music Structure for Games](https://www.gamedeveloper.com/design/music-structure-for-games) — How to structure combat music

**Build:** 8-bar combat loop. Tension building. Bassline added.

**Level Design: Arena Layout**

**Read:** [Arena Design in Fighting Games](https://www.gamedeveloper.com/design/arena-design-in-fighting-games) — Fighting game arena principles

**Build:** Flat arena. Boundaries. Simple background.

**UI: Health Bar**

**Read:** [Godot ProgressBar](https://docs.godotengine.org/en/stable/classes/class_progressbar.html) — Official ProgressBar docs

**Build:** Health bar frame + fill. Damage = bar decreases with lerp animation.

---

## DAWN 7: Hitbox/Hurtbox & Combat Feel

**Game Mechanic:** Hitbox/Hurtbox System

**Origin:** Street Fighter hitboxes. Dark Souls refined the concept.

**Read:**
- [Hitboxes and Hurtboxes in Games](https://www.gdquest.com/library/hitbox_hurtbox_godot4/) — GDQuest's comprehensive hitbox/hurtbox tutorial for Godot 4
- [Godot Collision Layers](https://docs.godotengine.org/en/stable/tutorials/physics/physics_introduction.html) — Official Godot physics layers docs

**Build:** Attack Area2D (hitbox). Body Area2D (hurtbox). On overlap = damage.

**Art: Attack Animation**

**Read:** [Animation Principles - Anticipation](https://www.youtube.com/results?search_query=animation+principles+anticipation) — YouTube search for animation anticipation principle

**Build:** 3-frame attack: windup (2f), strike (1f), recovery (2f). Add white flash frame on strike.

**Music: Impact SFX**

**Read:** [Designing Impact Sounds](https://www.youtube.com/results?search_query=designing+impact+sounds+sfxr) — YouTube search for impact sound design

**Build:** Sword swing SFX (sfxr). Hit SFX. Block SFX.

**Level Design: Combat Space**

**Read:** [Combat Encounter Design](https://www.gamedeveloper.com/design/combat-encounter-design) — Designing readable combat spaces

**Build:** Space for 1v1. No obstacles. Focus on combat readability.

**UI: Combo Counter**

**Read:** [Godot Label](https://docs.godotengine.org/en/stable/classes/class_label.html) — Official Label docs

**Build:** "Hits: X" counter. Resets after 2 seconds of no hit.

---

## DAWN 8: Enemy AI & Knockback

**Game Mechanic:** Simple Enemy AI + Knockback

**Origin:** Zelda enemies. Castlevania knockback on hit.

**Read:**
- [Godot Navigation2D / NavigationAgent2D](https://docs.godotengine.org/en/stable/classes/class_navigationagent2d.html) — Official NavigationAgent2D docs for pathfinding
- [2D Pathfinding in Godot 4](https://vav-labs.com/blog/2d-pathfinding-in-godot/) — Comprehensive guide to AStarGrid2D, AStar2D, and NavigationServer2D
- [Knockback in Games](https://www.gamedeveloper.com/design/knockback-in-games) — Knockback design principles

**Build:** Enemy patrols between two points. On player in range, chases. On hit, knockback applied.

**Art: Enemy Sprite (Slime)**

**Read:** [Enemy Design Principles](https://www.youtube.com/results?search_query=enemy+design+principles+pixel+art) — YouTube search for enemy design

**Build:** 32×32 slime. Idle (2 frames). Hurt (1 frame). Death (4 frames).

**Music: Enemy SFX**

**Read:** [Creature Sound Design](https://www.youtube.com/results?search_query=creature+sound+design+sfxr) — YouTube search for creature SFX design

**Build:** Slime move SFX. Slime hurt SFX. Slime death SFX.

**Level Design: Enemy Placement**

**Read:** [Enemy Placement in Level Design](https://www.gamedeveloper.com/design/enemy-placement-in-level-design) — Strategic enemy positioning

**Build:** One slime in arena. Learn its pattern. Then add second slime.

**UI: Damage Numbers**

**Read:** [Floating Damage Numbers](https://www.youtube.com/results?search_query=floating+damage+numbers+godot) — YouTube search for damage number implementation

**Build:** Numbers pop up on hit. Float up and fade out.

---

## DAWN 9: Invincibility Frames & Polish

**Game Mechanic:** i-Frames + Hit Pause

**Origin:** Street Fighter invincibility. Celeste hit pause for juice.

**Read:**
- [Invincibility Frames in Games](https://www.gamedeveloper.com/design/invincibility-frames-in-games) — i-Frame design theory
- [Game Feel - Hit Pause](https://www.youtube.com/results?search_query=game+feel+hit+pause+screenshake) — YouTube search for hit pause techniques

**Build:** 1-second i-frames after hit (sprite flashes). 3-frame hit pause on impact.

**Art: Hit Effects**

**Read:** [Impact Effects in Pixel Art](https://www.youtube.com/results?search_query=impact+effects+pixel+art) — YouTube search for pixel art impact effects

**Build:** 4-frame hit spark (8×8). Screen shake on heavy hit.

**Music: Dynamic Audio**

**Read:** [Godot Audio Buses](https://docs.godotengine.org/en/stable/tutorials/audio/audio_buses.html) — Official Godot audio bus docs

**Build:** Low-pass filter when paused. Reverb in "cave" area.

**Level Design: Multiple Encounters**

**Read:** [Pacing in Combat Design](https://www.gamedeveloper.com/design/pacing-in-combat-design) — Combat pacing principles

**Build:** 3 rooms. Room 1: 1 slime. Room 2: 2 slimes. Room 3: 3 slimes + health pickup.

**UI: Pause Menu (Combat)**

**Read:** [Godot Popup](https://docs.godotengine.org/en/stable/classes/class_popup.html) — Official Popup docs

**Build:** Pause with stats. Resume, Restart, Quit.

---

## DAWN 10: Vertical Slice Polish

**Game Mechanic:** Full Combat System

**Read:** [Combat System Design](https://www.gamedeveloper.com/design/combat-system-design) — Comprehensive combat system design

**Build:** Light attack, heavy attack (hold), combo system (light → light → heavy).

**Art: Full Player Sprite Sheet**

**Read:** [Sprite Sheet Organization](https://www.aseprite.org/docs/sprite-sheet/) — Official Aseprite sprite sheet docs

**Build:** 20+ frames. All states. Proper tagging in Aseprite.

**Music: Full Combat Track**

**Read:** [Layering Music in Games](https://www.gamedeveloper.com/design/layering-music-in-games) — Music stem layering techniques

**Build:** 1-minute track. Intro → loop. Stem mixing capability.

**Level Design: Complete Arena**

**Read:** [Boss Arena Design](https://www.gamedeveloper.com/design/boss-arena-design) — Boss arena layout principles

**Build:** 3-room dungeon. Final room = mini-boss (larger slime).

**UI: Full UI Kit**

**Read:** [Godot Theme Editor](https://docs.godotengine.org/en/stable/tutorials/ui/gui_theme_editor.html) — Official Godot theme editor docs

**Build:** Health bar, combo counter, pause menu, game over, victory screen.

**BLOCK 2 DELIVERABLE:** Playable combat demo. Catalog all reusable pieces.
"""

with open('/mnt/agents/output/instruction.md', 'w', encoding='utf-8') as f:
    f.write(content_part1)

print("Part 1 written successfully")

content_part2 = """

---

# 🧙 BLOCK 3: The Caster (Dawns 11–15)
**Goal:** Ranged combat, projectile physics, particle systems, mana/spell systems

---

## DAWN 11: Projectile Physics

**Game Mechanic:** Projectile System

**Origin:** Mega Man (1987) projectiles. Magicka spell combining.

**Read:**
- [Godot RigidBody2D](https://docs.godotengine.org/en/stable/classes/class_rigidbody2d.html) — Official RigidBody2D docs for physics-based projectiles
- [Projectile Motion in Games](https://www.gamedeveloper.com/design/projectile-motion-in-games) — Projectile physics design

**Build:** Fireball projectile. Gravity arc. Collision with enemies/walls. Destroy on hit.

**Art: Projectile Sprites**

**Read:** [Projectile Design in Pixel Art](https://www.youtube.com/results?search_query=projectile+design+pixel+art) — YouTube search for projectile pixel art

**Build:** 8×8 fireball (4 frames). 8×8 ice shard (4 frames). Trail particles.

**Music: Magic SFX**

**Read:** [Designing Magic Sounds](https://www.youtube.com/results?search_query=designing+magic+sounds+sfxr) — YouTube search for magic SFX design

**Build:** Fire cast SFX. Ice cast SFX. Projectile hit SFX.

**Level Design: Ranged Combat Space**

**Read:** [Ranged Combat Level Design](https://www.gamedeveloper.com/design/ranged-combat-level-design) — Designing for ranged combat

**Build:** Wide arena. Platforms at different heights. Cover points.

**UI: Mana Bar**

**Read:** [Godot TextureProgressBar](https://docs.godotengine.org/en/stable/classes/class_textureprogressbar.html) — Official TextureProgressBar docs

**Build:** Mana bar (blue). Cast = mana drain. Regeneration over time.

---

## DAWN 12: Particle Systems

**Game Mechanic:** Particle Effects on Cast/Hit

**Origin:** Diablo spell effects. Path of Exile particle overload.

**Read:**
- [Godot CPUParticles2D](https://docs.godotengine.org/en/stable/classes/class_cpuparticles2d.html) — Official CPUParticles2D docs
- [Godot GPUParticles2D Tutorial](https://christinec-dev.medium.com/learn-godot-4-by-making-a-2d-platformer-part-23-particle-effects-10473e650aa7) — Comprehensive particle effects tutorial
- [Particle Systems in Games](https://www.gamedeveloper.com/design/particle-systems-in-games) — Particle system design theory

**Build:** Cast particles (burst from player). Hit particles (burst at collision point). Death particles.

**Art: Particle Sprites**

**Read:** [Pixel Art Particles](https://www.youtube.com/results?search_query=pixel+art+particles) — YouTube search for pixel art particles

**Build:** Spark (4×4, 3 frames). Dust puff (6×6, 4 frames). Blood/essence drop (4×4, 3 frames).

**Music: Ambient Layer**

**Read:** [Ambient Music in Games](https://www.gamedeveloper.com/design/ambient-music-in-games) — Ambient music composition

**Build:** Add ambient pad to combat track. Crossfade between combat and ambient.

**Level Design: Verticality**

**Read:** [Vertical Level Design](https://www.gamedeveloper.com/design/vertical-level-design) — Vertical space design principles

**Build:** Tower with 3 floors. Each floor = different enemy type.

**UI: Spell Hotbar**

**Read:** [Godot GridContainer](https://docs.godotengine.org/en/stable/classes/class_gridcontainer.html) — Official GridContainer docs

**Build:** 4-slot hotbar. Q/W/E/R to cast. Cooldown indicators.

---

## DAWN 13: Spell Combinations

**Game Mechanic:** Spell Combining System

**Origin:** Magicka (2011) spell combining. Baba Is You rule combining.

**Read:**
- [Combo Systems in Games](https://www.gamedeveloper.com/design/combo-systems-in-games) — Combo system design
- [Godot Dictionary](https://docs.godotengine.org/en/stable/classes/class_dictionary.html) — Official Dictionary docs for recipe data

**Build:** Fire + Fire = Big Fireball. Fire + Ice = Steam (AoE). Ice + Ice = Ice Wall.

**Art: Combined Spell Sprites**

**Read:** [Visual Effects for Combos](https://www.youtube.com/results?search_query=visual+effects+for+combos+pixel+art) — YouTube search for combo VFX

**Build:** Big fireball (16×16). Steam cloud (16×16). Ice wall tile (32×32).

**Music: Combo SFX**

**Read:** [Audio Feedback for Combos](https://www.youtube.com/results?search_query=audio+feedback+for+combos) — YouTube search for combo audio

**Build:** Combo success SFX. Combo fail SFX. Element mixing SFX.

**Level Design: Puzzle Combat**

**Read:** [Puzzle Combat Design](https://www.gamedeveloper.com/design/puzzle-combat-design) — Combining puzzles with combat

**Build:** Enemies with elemental weaknesses. Fire enemy weak to ice. Ice enemy weak to fire.

**UI: Combo Display**

**Read:** [Godot RichTextLabel](https://docs.godotengine.org/en/stable/classes/class_richtextlabel.html) — Official RichTextLabel docs

**Build:** "Fire + Ice = Steam!" text popup. Element icons.

---

## DAWN 14: AoE & Status Effects

**Game Mechanic:** Area of Effect + Status Effects

**Origin:** World of Warcraft AoE. Final Fantasy status effects.

**Read:**
- [Status Effects in Games](https://www.gamedeveloper.com/design/status-effects-in-games) — Status effect design
- [Godot Timer](https://docs.godotengine.org/en/stable/classes/class_timer.html) — Official Timer docs for effect duration

**Build:** Fireball = burn (damage over time). Ice shard = freeze (stun). Steam = blind (miss chance).

**Art: Status Effect Icons**

**Read:** [UI Icon Design](https://www.youtube.com/results?search_query=ui+icon+design+pixel+art) — YouTube search for UI icon design

**Build:** Burn icon (flame). Freeze icon (snowflake). Blind icon (eye with X).

**Music: Status SFX**

**Read:** [Status Effect Audio](https://www.youtube.com/results?search_query=status+effect+audio+design) — YouTube search for status audio

**Build:** Burn tick SFX. Freeze shatter SFX. Blind SFX.

**Level Design: Status Puzzle**

**Read:** [Status-Based Level Design](https://www.gamedeveloper.com/design/status-based-level-design) — Designing levels around status effects

**Build:** Room requires freeze to cross water. Room requires burn to melt ice wall.

**UI: Buff/Debuff Bar**

**Read:** [Godot HBoxContainer](https://docs.godotengine.org/en/stable/classes/class_hboxcontainer.html) — Official HBoxContainer docs

**Build:** Icon bar above health. Timers on icons. Tooltip on hover.

---

## DAWN 15: Vertical Slice Polish

**Game Mechanic:** Full Spell System

**Read:** [Magic System Design](https://www.gamedeveloper.com/design/magic-system-design) — Comprehensive magic system design

**Build:** 8 base spells. 12 combinations. Mana costs. Cooldowns.

**Art: Full Spell VFX Library**

**Read:** [VFX Library Organization](https://www.youtube.com/results?search_query=vfx+library+organization+godot) — YouTube search for VFX organization

**Build:** All spell sprites, particles, impact effects. Organized in Godot.

**Music: Full Magic Track**

**Read:** [Thematic Music in Games](https://www.gamedeveloper.com/design/thematic-music-in-games) — Thematic composition

**Build:** 1.5-minute track. Mystical theme. Dynamic layers.

**Level Design: Magic Tower**

**Read:** [Tower Level Design](https://www.gamedeveloper.com/design/tower-level-design) — Vertical tower design

**Build:** 5-floor tower. Each floor teaches new spell. Top floor = boss.

**UI: Full Magic UI**

**Read:** [Godot TabContainer](https://docs.godotengine.org/en/stable/classes/class_tabcontainer.html) — Official TabContainer docs

**Build:** Spell book (all spells). Hotbar. Mana bar. Status bar. Pause menu.

**BLOCK 3 DELIVERABLE:** Playable spell-casting demo. Catalog all reusable pieces.

---

# 🏹 BLOCK 4: The Hunter (Dawns 16–20)
**Goal:** Stealth, AI vision, line of sight, trap systems, minimap

---

## DAWN 16: Stealth Mechanics

**Game Mechanic:** Stealth System (Detection Meter)

**Origin:** Metal Gear Solid (1998) detection meter. Mark of the Ninja 2D stealth.

**Read:**
- [Stealth Game Design](https://www.gamedeveloper.com/design/stealth-game-design) — Stealth game design principles
- [Godot VisibilityNotifier2D / VisibleOnScreenNotifier2D](https://docs.godotengine.org/en/stable/classes/class_visibleonscreennotifier2d.html) — Official visibility notifier docs

**Build:** Detection meter fills when in enemy vision. Full = alert. Empty = hidden.

**Art: Stealth UI + Vision Cone**

**Read:** [Stealth Game Art](https://www.youtube.com/results?search_query=stealth+game+art+pixel) — YouTube search for stealth game art

**Build:** Vision cone sprite (semi-transparent). Stealth meter frame + fill. Crouch sprite.

**Music: Tension Music**

**Read:** [Tension in Game Music](https://www.gamedeveloper.com/design/tension-in-game-music) — Building musical tension

**Build:** Low tension ambient. High tension when detected. Crossfade between.

**Level Design: Cover System**

**Read:** [Cover in Level Design](https://www.gamedeveloper.com/design/cover-in-level-design) — Cover system design

**Build:** Crates/walls that block vision. Shadows that hide player.

**UI: Stealth Meter**

**Read:** [Godot TextureProgressBar](https://docs.godotengine.org/en/stable/classes/class_textureprogressbar.html) — Official TextureProgressBar docs

**Build:** Eye icon + meter. Color changes: green (hidden) → yellow (suspicious) → red (alert).

---

## DAWN 17: AI Vision & Patrol

**Game Mechanic:** AI Vision Cone + Patrol Paths

**Origin:** Splinter Cell vision cones. Dishonored patrol patterns.

**Read:**
- [AI Vision in Games](https://www.gamedeveloper.com/design/ai-vision-in-games) — AI vision system design
- [Godot RayCast2D](https://docs.godotengine.org/en/stable/classes/class_raycast2d.html) — Official RayCast2D docs for vision rays

**Build:** Enemy has vision cone (RayCast2D array). Patrols path. On alert, searches last known position.

**Art: Enemy Alert States**

**Read:** [AI State Visualization](https://www.youtube.com/results?search_query=ai+state+visualization+pixel+art) — YouTube search for AI state visuals

**Build:** Enemy sprites: patrol (calm), alert (question mark), chase (exclamation). Color-coded.

**Music: Alert SFX**

**Read:** [Alert Sounds in Games](https://www.youtube.com/results?search_query=alert+sounds+in+games) — YouTube search for alert audio

**Build:** Alert ping SFX. Search SFX. Return to patrol SFX.

**Level Design: Patrol Routes**

**Read:** [Patrol Design in Stealth Games](https://www.gamedeveloper.com/design/patrol-design-in-stealth-games) — Patrol route design

**Build:** 2 enemies with overlapping patrols. Player must time movement.

**UI: Minimap**

**Read:** [Godot SubViewport](https://docs.godotengine.org/en/stable/classes/class_subviewport.html) — Official SubViewport docs for minimap

**Build:** Minimap (top-right). Shows player, enemies (dots), vision cones (wedges).

---

## DAWN 18: Traps & Environmental Hazards

**Game Mechanic:** Trap System (Pressure Plates, Tripwires)

**Origin:** Tomb Raider traps. Dark Souls environmental hazards.

**Read:**
- [Trap Design in Games](https://www.gamedeveloper.com/design/trap-design-in-games) — Trap design principles
- [Godot Area2D Signals](https://docs.godotengine.org/en/stable/classes/class_area2d.html) — Official Area2D signal docs

**Build:** Pressure plate (step = trigger). Tripwire (cross = trigger). Spike trap (timed).

**Art: Trap Sprites + Animations**

**Read:** [Trap Design in Pixel Art](https://www.youtube.com/results?search_query=trap+design+pixel+art) — YouTube search for trap pixel art

**Build:** Pressure plate (2 states). Tripwire (taut/broken). Spikes (retract/extend animation).

**Music: Trap SFX**

**Read:** [Trap Audio Design](https://www.youtube.com/results?search_query=trap+audio+design+games) — YouTube search for trap audio

**Build:** Pressure plate click. Tripwire snap. Spike extend. Spike hit.

**Level Design: Trap Placement**

**Read:** [Trap Placement in Level Design](https://www.gamedeveloper.com/design/trap-placement-in-level-design) — Strategic trap placement

**Build:** Traps that can be used against enemies. Lure enemy into spike trap.

**UI: Interaction Prompt**

**Read:** [Godot Label](https://docs.godotengine.org/en/stable/classes/class_label.html) — Official Label docs

**Build:** "Press E to disarm" prompt. Progress bar for disarming.

---

## DAWN 19: Line of Sight & Sound

**Game Mechanic:** Sound Propagation (Noise System)

**Origin:** Thief sound propagation. Alien: Isolation sound design.

**Read:**
- [Sound in Stealth Games](https://www.gamedeveloper.com/design/sound-in-stealth-games) — Sound design for stealth
- [Godot AudioStreamPlayer2D](https://docs.godotengine.org/en/stable/classes/class_audiostreamplayer2d.html) — Official positional audio docs

**Build:** Running = noise radius. Breaking crate = noise radius. Enemies investigate noise.

**Art: Noise Visualization**

**Read:** [Visualizing Sound in Games](https://www.youtube.com/results?search_query=visualizing+sound+in+games) — YouTube search for sound visualization

**Build:** Noise ripple effect (expanding circle). Visible only in "detective vision" mode.

**Music: Footstep System**

**Read:** [Footstep Audio in Games](https://www.youtube.com/results?search_query=footstep+audio+in+games) — YouTube search for footstep systems

**Build:** Different footstep SFX for wood, metal, grass. Volume based on speed.

**Level Design: Sound Puzzle**

**Read:** [Sound-Based Level Design](https://www.gamedeveloper.com/design/sound-based-level-design) — Designing sound puzzles

**Build:** Room where player must throw object to distract enemy. Silent takedown room.

**UI: Sound Meter**

**Read:** [Godot VBoxContainer](https://docs.godotengine.org/en/stable/classes/class_vboxcontainer.html) — Official VBoxContainer docs

**Build:** Noise meter (bottom). Shows current noise level. Footstep icons.

---

## DAWN 20: Vertical Slice Polish

**Game Mechanic:** Full Stealth System

**Read:** [Stealth System Design](https://www.gamedeveloper.com/design/stealth-system-design) — Comprehensive stealth system design

**Build:** Crouch, run, silent takedown, body hiding, alarm system, escape route.

**Art: Full Stealth Sprite Sheet**

**Read:** [Stealth Character Animation](https://www.youtube.com/results?search_query=stealth+character+animation+pixel+art) — YouTube search for stealth animations

**Build:** Crouch walk, silent takedown, body drag, hiding. Enemy all states.

**Music: Full Stealth Track**

**Read:** [Stealth Music Composition](https://www.gamedeveloper.com/design/stealth-music-composition) — Composing stealth music

**Build:** 2-minute track. 3 layers (ambient, tension, chase). Stem mixing.

**Level Design: Full Stealth Mission**

**Read:** [Stealth Mission Design](https://www.gamedeveloper.com/design/stealth-mission-design) — Designing complete stealth missions

**Build:** Infiltration → objective → extraction. 5 rooms. Multiple paths.

**UI: Full Stealth UI**

**Read:** [Godot MarginContainer](https://docs.godotengine.org/en/stable/classes/class_margincontainer.html) — Official MarginContainer docs

**Build:** Stealth meter, minimap, noise meter, interaction prompt, objective list, pause menu.

**BLOCK 4 DELIVERABLE:** Playable stealth demo. Catalog all reusable pieces.

---

# 🚗 BLOCK 5: The Racer (Dawns 21–25)
**Goal:** Top-down physics, drifting, boost, lap timing, track design

---

## DAWN 21: Top-Down Vehicle Physics

**Game Mechanic:** Top-Down Car Movement

**Origin:** Micro Machines (1991). GTA 1/2 top-down driving.

**Read:**
- [Top-Down Car Physics](https://www.gamedeveloper.com/design/top-down-car-physics) — Top-down vehicle physics design
- [Godot RigidBody2D](https://docs.godotengine.org/en/stable/classes/class_rigidbody2d.html) — Official RigidBody2D docs for car physics

**Build:** Car with acceleration, max speed, friction, rotation. Not tank controls — car turns and moves forward.

**Art: Car Sprite + Skid Marks**

**Read:** [Vehicle Sprite Design](https://www.youtube.com/results?search_query=vehicle+sprite+design+pixel+art) — YouTube search for vehicle sprites

**Build:** 32×32 car (4 angles). Skid mark sprite (Line2D trail). Tire smoke particles.

**Music: Engine SFX**

**Read:** [Engine Sound Design](https://www.youtube.com/results?search_query=engine+sound+design+games) — YouTube search for engine audio

**Build:** Engine loop (pitch changes with speed). Brake SFX. Crash SFX.

**Level Design: Test Track**

**Read:** [Racing Track Design Basics](https://www.gamedeveloper.com/design/racing-track-design-basics) — Track design fundamentals

**Build:** Oval track. Learn handling. No obstacles.

**UI: Speedometer**

**Read:** [Godot TextureProgressBar](https://docs.godotengine.org/en/stable/classes/class_textureprogressbar.html) — Official TextureProgressBar docs

**Build:** Circular speedometer. Needle rotates with speed. Digital speed readout.

---

## DAWN 22: Drifting & Boost

**Game Mechanic:** Drift Mechanics + Boost System

**Origin:** Mario Kart drifting. Burnout boost.

**Read:**
- [Drift Mechanics in Racing Games](https://www.gamedeveloper.com/design/drift-mechanics-in-racing-games) — Drift system design
- [Godot Particles](https://docs.godotengine.org/en/stable/classes/class_cpuparticles2d.html) — Official particle docs for drift effects

**Build:** Hold brake while turning = drift. Drift meter fills. Release = boost.

**Art: Drift Effects**

**Read:** [Drift Effects in Pixel Art](https://www.youtube.com/results?search_query=drift+effects+pixel+art) — YouTube search for drift VFX

**Build:** Tire smoke (more during drift). Spark particles. Boost flame (behind car).

**Music: Boost SFX + Music Intensity**

**Read:** [Dynamic Music in Racing Games](https://www.gamedeveloper.com/design/dynamic-music-in-racing-games) — Dynamic music for racing

**Build:** Boost SFX (sfxr). Music intensity increases with speed.

**Level Design: Curves & Straights**

**Read:** [Racing Line Design](https://www.gamedeveloper.com/design/racing-line-design) — Racing line and track flow

**Build:** Track with curves (drift opportunities) and straights (boost opportunities).

**UI: Drift Meter + Boost Meter**

**Read:** [Godot ProgressBar](https://docs.godotengine.org/en/stable/classes/class_progressbar.html) — Official ProgressBar docs

**Build:** Drift meter (fills while drifting). Boost meter (spend for speed burst).

---

## DAWN 23: Lap Timing & Checkpoints

**Game Mechanic:** Lap System + Checkpoints

**Origin:** Gran Turismo lap timing. Mario Kart checkpoint system.

**Read:**
- [Checkpoint System Design](https://www.gamedeveloper.com/design/checkpoint-system-design) — Checkpoint system design
- [Godot Timer](https://docs.godotengine.org/en/stable/classes/class_timer.html) — Official Timer docs

**Build:** Start/finish line. Checkpoints (must pass in order). Lap timer. Best lap tracking.

**Art: Track Elements**

**Read:** [Racing Track Pixel Art](https://www.youtube.com/results?search_query=racing+track+pixel+art) — YouTube search for track pixel art

**Build:** Track tiles (straight, curve, start line, checkpoint flags). Barrier sprites.

**Music: Racing Music**

**Read:** [Racing Music Composition](https://www.gamedeveloper.com/design/racing-music-composition) — Composing racing music

**Build:** High-energy 2-minute loop. BPM increases with lap number.

**Level Design: Multi-Lap Track**

**Read:** [Lap-Based Level Design](https://www.gamedeveloper.com/design/lap-based-level-design) — Multi-lap track design

**Build:** 3-lap race. Track with shortcuts (risk/reward).

**UI: Lap Display + Leaderboard**

**Read:** [Godot HSplitContainer](https://docs.godotengine.org/en/stable/classes/class_hsplitcontainer.html) — Official HSplitContainer docs

**Build:** "Lap 1/3" display. Current lap time. Best lap time. Position (1st, 2nd, etc.).

---

## DAWN 24: AI Opponents

**Game Mechanic:** Racing AI (Rubber Banding)

**Origin:** Mario Kart rubber banding. Forza Drivatar system.

**Read:**
- [Racing AI Design](https://www.gamedeveloper.com/design/racing-ai-design) — Racing AI design principles
- [Godot Navigation2D](https://docs.godotengine.org/en/stable/classes/class_navigationagent2d.html) — Official NavigationAgent2D docs for AI pathing

**Build:** 3 AI opponents. Follow racing line. Rubber banding (slow down if too far ahead, speed up if behind).

**Art: AI Car Variants**

**Read:** [Car Variation in Sprite Design](https://www.youtube.com/results?search_query=car+variation+sprite+design) — YouTube search for car variants

**Build:** 3 car colors. Different stats (speed, handling, acceleration).

**Music: Position-Based Audio**

**Read:** [Positional Audio in Racing](https://www.youtube.com/results?search_query=positional+audio+in+racing+games) — YouTube search for positional racing audio

**Build:** Engine sounds positional. Leader has louder engine. Overtake SFX.

**Level Design: Competitive Track**

**Read:** [Overtake Opportunities in Track Design](https://www.gamedeveloper.com/design/overtake-opportunities-in-track-design) — Designing for overtaking

**Build:** Track with wide sections (overtake) and narrow sections (defend position).

**UI: Mini-Map + Race Standings**

**Read:** [Godot SubViewport](https://docs.godotengine.org/en/stable/classes/class_subviewport.html) — Official SubViewport docs

**Build:** Track minimap with all cars. Real-time position list. Gap to next car.

---

## DAWN 25: Vertical Slice Polish

**Game Mechanic:** Full Racing System

**Read:** [Racing Game Design](https://www.gamedeveloper.com/design/racing-game-design) — Comprehensive racing game design

**Build:** 3 tracks. 4 cars. Championship mode (points system).

**Art: Full Racing Asset Pack**

**Read:** [Racing Game Asset Creation](https://www.youtube.com/results?search_query=racing+game+asset+creation) — YouTube search for racing assets

**Build:** 3 track tilesets. 4 cars. All UI elements. Particle effects.

**Music: Full Racing Soundtrack**

**Read:** [Racing Soundtrack Composition](https://www.gamedeveloper.com/design/racing-soundtrack-composition) — Racing soundtrack design

**Build:** 3 tracks (menu, race, victory). All SFX. Engine audio system.

**Level Design: Championship Tracks**

**Read:** [Championship Track Design](https://www.gamedeveloper.com/design/championship-track-design) — Championship progression design

**Build:** 3 distinct tracks. Easy → medium → hard. Shortcut on each.

**UI: Full Racing UI**

**Read:** [Godot MarginContainer](https://docs.godotengine.org/en/stable/classes/class_margincontainer.html) — Official MarginContainer docs

**Build:** Title screen, car select, track select, race HUD, pause, results, championship standings.

**BLOCK 5 DELIVERABLE:** Playable racing demo. Catalog all reusable pieces.

---

# 🏗️ BLOCK 6: The Builder (Dawns 26–30)
**Goal:** Inventory grid, crafting, resource gathering, placement system, tooltips

---

## DAWN 26: Inventory Grid System

**Game Mechanic:** Grid Inventory (Drag & Drop)

**Origin:** Diablo (1996) grid inventory. Resident Evil tetris inventory.

**Read:**
- [Grid Inventory Systems](https://www.gamedeveloper.com/design/grid-inventory-systems) — Grid inventory design patterns
- [Godot GridContainer](https://docs.godotengine.org/en/stable/classes/class_gridcontainer.html) — Official GridContainer docs

**Build:** 8×5 inventory grid. Drag items between slots. Stackable items (max 99).

**Art: Item Sprites + UI Frame**

**Read:** [Item Icon Design](https://www.youtube.com/results?search_query=item+icon+design+pixel+art) — YouTube search for item icons

**Build:** 10 item sprites (16×16). Inventory frame (9-patch). Slot highlight.

**Music: Inventory SFX**

**Read:** [UI Audio Design](https://www.youtube.com/results?search_query=ui+audio+design+games) — YouTube search for UI audio

**Build:** Pick up SFX. Drop SFX. Equip SFX. Error SFX (inventory full).

**Level Design: Resource Nodes**

**Read:** [Resource Node Placement](https://www.gamedeveloper.com/design/resource-node-placement) — Resource distribution design

**Build:** Trees, rocks, ore deposits. Click to gather. Respawn timer.

**UI: Inventory Screen**

**Read:** [Godot PanelContainer](https://docs.godotengine.org/en/stable/classes/class_panelcontainer.html) — Official PanelContainer docs

**Build:** Full inventory UI. Grid slots. Item count labels. Gold display.

---

## DAWN 27: Crafting System

**Game Mechanic:** Crafting Recipes

**Origin:** Minecraft (2009) crafting. Terraria recipe system.

**Read:**
- [Crafting System Design](https://www.gamedeveloper.com/design/crafting-system-design) — Crafting system design
- [Godot Dictionary](https://docs.godotengine.org/en/stable/classes/class_dictionary.html) — Official Dictionary docs for recipe database

**Build:** Recipe database. 2 wood + 1 stone = wooden sword. Crafting UI with recipe list.

**Art: Crafting UI + Recipe Icons**

**Read:** [Crafting UI Design](https://www.youtube.com/results?search_query=crafting+ui+design+pixel+art) — YouTube search for crafting UI

**Build:** Crafting station sprite. Recipe list icons. Result preview.

**Music: Crafting SFX**

**Read:** [Crafting Audio Design](https://www.youtube.com/results?search_query=crafting+audio+design) — YouTube search for crafting audio

**Build:** Craft start SFX. Craft complete SFX. Craft fail SFX.

**Level Design: Crafting Stations**

**Read:** [Crafting Station Placement](https://www.gamedeveloper.com/design/crafting-station-placement) — Crafting station layout

**Build:** Workbench, furnace, anvil. Each has different recipes. Must be near to craft.

**UI: Recipe Book**

**Read:** [Godot ItemList](https://docs.godotengine.org/en/stable/classes/class_itemlist.html) — Official ItemList docs

**Build:** Recipe list (discovered/undiscovered). Ingredient requirements. Craft button.

---

## DAWN 28: Resource Gathering

**Game Mechanic:** Gathering Tools + Durability

**Origin:** Harvest Moon tools. Stardew Valley tool progression.

**Read:**
- [Tool System Design](https://www.gamedeveloper.com/design/tool-system-design) — Tool system design
- [Godot AnimationPlayer](https://docs.godotengine.org/en/stable/classes/class_animationplayer.html) — Official AnimationPlayer docs

**Build:** Axe (trees), Pickaxe (rocks), Hoe (dirt). Each has durability. Upgradable.

**Art: Tool Sprites + Animations**

**Read:** [Tool Animation in Pixel Art](https://www.youtube.com/results?search_query=tool+animation+pixel+art) — YouTube search for tool animations

**Build:** 3 tool sprites (swing animation). Resource break animations (tree falls, rock cracks).

**Music: Gathering SFX**

**Read:** [Gathering Audio Design](https://www.youtube.com/results?search_query=gathering+audio+design+games) — YouTube search for gathering audio

**Build:** Chop SFX. Mine SFX. Dig SFX. Tool break SFX.

**Level Design: Resource Distribution**

**Read:** [Resource Distribution in Open Worlds](https://www.gamedeveloper.com/design/resource-distribution-in-open-worlds) — Open world resource layout

**Build:** Forest (wood), mountain (stone), cave (ore). Rare resources in dangerous areas.

**UI: Tool Durability Bar**

**Read:** [Godot TextureProgressBar](https://docs.godotengine.org/en/stable/classes/class_textureprogressbar.html) — Official TextureProgressBar docs

**Build:** Durability bar on tool icon. Color changes (green → yellow → red).

---

## DAWN 29: Building Placement

**Game Mechanic:** Building Placement System

**Origin:** Minecraft block placement. Terraria tile placement.

**Read:**
- [Building System Design](https://www.gamedeveloper.com/design/building-system-design) — Building system design
- [Godot TileMap](https://docs.godotengine.org/en/stable/classes/class_tilemap.html) — Official TileMap docs for placement
- [Grid-Based Placement in Godot](https://medium.com/godot-dev-digest/grid-based-placement-in-godot-be2231554d09) — Grid-based placement tutorial

**Build:** Select building from inventory. Ghost preview. Click to place. Collision check.

**Art: Building Tiles + Preview**

**Read:** [Building Tile Design](https://www.youtube.com/results?search_query=building+tile+design+pixel+art) — YouTube search for building tiles

**Build:** Wall tiles, floor tiles, door tiles. Ghost preview (semi-transparent). Invalid placement (red).

**Music: Building SFX**

**Read:** [Construction Audio Design](https://www.youtube.com/results?search_query=construction+audio+design+games) — YouTube search for construction audio

**Build:** Place SFX. Remove SFX. Invalid placement SFX.

**Level Design: Building Area**

**Read:** [Base Building Level Design](https://www.gamedeveloper.com/design/base-building-level-design) — Base building area design

**Build:** Flat area for building. Pre-built example house. Player builds their own.

**UI: Building Hotbar**

**Read:** [Godot HBoxContainer](https://docs.godotengine.org/en/stable/classes/class_hboxcontainer.html) — Official HBoxContainer docs

**Build:** Hotbar with building categories. Rotate building (R key). Remove mode.

---

## DAWN 30: Vertical Slice Polish

**Game Mechanic:** Full Builder System

**Read:** [Builder Game Design](https://www.gamedeveloper.com/design/builder-game-design) — Builder game design principles

**Build:** Gather → Craft → Build loop. 20+ recipes. 10+ building types. Save/load builds.

**Art: Full Builder Asset Pack**

**Read:** [Builder Game Asset Creation](https://www.youtube.com/results?search_query=builder+game+asset+creation) — YouTube search for builder assets

**Build:** All resources, tools, buildings, UI. Consistent 16×16 style.

**Music: Full Builder Soundtrack**

**Read:** [Peaceful Game Music](https://www.youtube.com/results?search_query=peaceful+game+music+composition) — YouTube search for peaceful music

**Build:** 2-minute peaceful loop. All SFX. Ambient nature sounds.

**Level Design: Sandbox World**

**Read:** [Sandbox Level Design](https://www.gamedeveloper.com/design/sandbox-level-design) — Sandbox world design

**Build:** Medium-sized world. Biomes. Resource distribution. Building area.

**UI: Full Builder UI**

**Read:** [Godot TabContainer](https://docs.godotengine.org/en/stable/classes/class_tabcontainer.html) — Official TabContainer docs

**Build:** Inventory, crafting, building, map, settings. Consistent theme.

**BLOCK 6 DELIVERABLE:** Playable builder demo. Catalog all reusable pieces.

---

# 🌊 BLOCK 7: The Diver (Dawns 31–35)
**Goal:** Underwater physics, oxygen system, buoyancy, treasure hunt, parallax

---

## DAWN 31: Underwater Physics

**Game Mechanic:** Underwater Movement (Buoyancy, Drag)

**Origin:** Ecco the Dolphin (1992). Subnautica underwater physics.

**Read:**
- [Underwater Physics in Games](https://www.gamedeveloper.com/design/underwater-physics-in-games) — Underwater physics design
- [Godot Area2D Gravity](https://docs.godotengine.org/en/stable/classes/class_area2d.html) — Official Area2D docs for gravity override

**Build:** Player in water. Buoyancy pushes up. Drag slows movement. Swim up/down.

**Art: Underwater Player + Bubble Particles**

**Read:** [Underwater Pixel Art](https://www.youtube.com/results?search_query=underwater+pixel+art) — YouTube search for underwater art

**Build:** Swimming player sprite (4 directions). Bubble trail particles. Water overlay effect.

**Music: Underwater Ambience**

**Read:** [Underwater Audio Design](https://www.youtube.com/results?search_query=underwater+audio+design+games) — YouTube search for underwater audio

**Build:** Underwater ambience (low-pass filter). Bubble SFX. Swim SFX.

**Level Design: Underwater Cave**

**Read:** [Underwater Level Design](https://www.gamedeveloper.com/design/underwater-level-design) — Underwater cave design

**Build:** Cave system with air pockets. Vertical shafts. Hidden passages.

**UI: Oxygen Bar**

**Read:** [Godot TextureProgressBar](https://docs.godotengine.org/en/stable/classes/class_textureprogressbar.html) — Official TextureProgressBar docs

**Build:** Oxygen meter (depletes underwater). Refills in air pockets. Warning flash below 25%.

---

## DAWN 32: Oxygen & Pressure

**Game Mechanic:** Oxygen System + Depth Pressure

**Origin:** Subnautica oxygen. Barotrauma pressure system.

**Read:**
- [Oxygen System Design](https://www.gamedeveloper.com/design/oxygen-system-design) — Oxygen mechanic design
- [Godot Timer](https://docs.godotengine.org/en/stable/classes/class_timer.html) — Official Timer docs

**Build:** Oxygen tank (limited). Refill at surface/air pockets. Deeper = more oxygen drain.

**Art: Oxygen Tank + Air Pocket**

**Read:** [Underwater UI Design](https://www.youtube.com/results?search_query=underwater+ui+design) — YouTube search for underwater UI

**Build:** Oxygen tank sprite (inventory). Air pocket bubble animation. Depth indicator art.

**Music: Pressure SFX**

**Read:** [Pressure Audio in Games](https://www.youtube.com/results?search_query=pressure+audio+in+games) — YouTube search for pressure audio

**Build:** Low oxygen warning beep. Air refill SFX. Crushing pressure SFX (deep areas).

**Level Design: Depth Layers**

**Read:** [Depth-Based Level Design](https://www.gamedeveloper.com/design/depth-based-level-design) — Designing by depth

**Build:** Shallow (safe, lots of air). Mid (moderate danger). Deep (high danger, best loot).

**UI: Depth Meter**

**Read:** [Godot Label](https://docs.godotengine.org/en/stable/classes/class_label.html) — Official Label docs

**Build:** Depth display (meters). Pressure warning. Oxygen timer countdown.

---

## DAWN 33: Treasure & Collectibles

**Game Mechanic:** Treasure Hunt (Hidden Items)

**Origin:** Zelda underwater treasures. Sea of Thieves treasure hunting.

**Read:**
- [Treasure Hunt Design](https://www.gamedeveloper.com/design/treasure-hunt-design) — Treasure hunt design
- [Godot RayCast2D](https://docs.godotengine.org/en/stable/classes/class_raycast2d.html) — Official RayCast2D docs for detection

**Build:** Treasure chests hidden in caves. Sonar pulse reveals nearby treasure. Rare artifacts.

**Art: Treasure Sprites + Glow Effects**

**Read:** [Treasure Design in Pixel Art](https://www.youtube.com/results?search_query=treasure+design+pixel+art) — YouTube search for treasure art

**Build:** Chest sprites (closed/open). Gem sprites (glow shader). Artifact sprites.

**Music: Discovery SFX**

**Read:** [Discovery Audio in Games](https://www.youtube.com/results?search_query=discovery+audio+in+games) — YouTube search for discovery audio

**Build:** Sonar ping SFX. Chest open SFX. Rare find SFX (fanfare).

**Level Design: Treasure Distribution**

**Read:** [Loot Distribution in Level Design](https://www.gamedeveloper.com/design/loot-distribution-in-level-design) — Loot placement principles

**Build:** Common treasure (easy to find). Rare treasure (hidden). Legendary (puzzle-locked).

**UI: Treasure Log**

**Read:** [Godot ItemList](https://docs.godotengine.org/en/stable/classes/class_itemlist.html) — Official ItemList docs

**Build:** Collected treasures list. Rarity indicators. Lore descriptions.

---

## DAWN 34: Underwater Enemies

**Game Mechanic:** Underwater Enemies (Jellyfish, Sharks)

**Origin:** Ecco enemies. Subnautica creature AI.

**Read:**
- [Underwater AI Design](https://www.gamedeveloper.com/design/underwater-ai-design) — Underwater enemy AI
- [Godot PathFollow2D](https://docs.godotengine.org/en/stable/classes/class_pathfollow2d.html) — Official PathFollow2D docs

**Build:** Jellyfish (float up/down, electric touch). Shark (patrol, charge attack). Fish school (flocking).

**Art: Underwater Creatures**

**Read:** [Creature Design for Underwater Games](https://www.youtube.com/results?search_query=creature+design+underwater+pixel+art) — YouTube search for underwater creatures

**Build:** Jellyfish (pulse animation). Shark (swim cycle). Small fish (school sprite).

**Music: Creature SFX**

**Read:** [Creature Audio Underwater](https://www.youtube.com/results?search_query=creature+audio+underwater+games) — YouTube search for creature audio

**Build:** Jellyfish zap SFX. Shark approach SFX (Jaws-style). Fish scatter SFX.

**Level Design: Danger Zones**

**Read:** [Danger Zone Design](https://www.gamedeveloper.com/design/danger-zone-design) — Danger zone layout

**Build:** Safe zones (no enemies). Danger zones (jellyfish fields). Dead zones (shark territory).

**UI: Danger Indicator**

**Read:** [Godot ColorRect](https://docs.godotengine.org/en/stable/classes/class_colorrect.html) — Official ColorRect docs

**Build:** Screen edge glow (red = danger). Heartbeat sound increases near sharks.

---

## DAWN 35: Vertical Slice Polish

**Game Mechanic:** Full Underwater System

**Read:** [Underwater Game Design](https://www.gamedeveloper.com/design/underwater-game-design) — Underwater game design

**Build:** Full dive mechanics. 10+ treasures. 5+ enemy types. Boss (giant squid).

**Art: Full Underwater Asset Pack**

**Read:** [Underwater Game Asset Creation](https://www.youtube.com/results?search_query=underwater+game+asset+creation) — YouTube search for underwater assets

**Build:** Player, enemies, treasures, environment, particles, UI. Parallax layers.

**Music: Full Underwater Soundtrack**

**Read:** [Ambient Underwater Music](https://www.youtube.com/results?search_query=ambient+underwater+music+composition) — YouTube search for underwater music

**Build:** 3 tracks (surface, shallow, deep). All SFX. Dynamic mixing.

**Level Design: Full Underwater World**

**Read:** [Underwater World Design](https://www.gamedeveloper.com/design/underwater-world-design) — Underwater world layout

**Build:** Connected cave system. 5 areas. Surface base. Deep treasure vault.

**UI: Full Underwater UI**

**Read:** [Godot MarginContainer](https://docs.godotengine.org/en/stable/classes/class_margincontainer.html) — Official MarginContainer docs

**Build:** Oxygen, depth, treasure log, map, danger indicator, pause menu.

**BLOCK 7 DELIVERABLE:** Playable underwater demo. Catalog all reusable pieces.
"""

with open('/mnt/agents/output/instruction.md', 'a', encoding='utf-8') as f:
    f.write(content_part2)

print("Part 2 written successfully")

content_part3 = """

---

# 🧩 BLOCK 8: The Puzzle (Dawns 36–40)
**Goal:** Switch logic, pressure plates, sliding blocks, key-door, puzzle UI

---

## DAWN 36: Switch & Button Logic

**Game Mechanic:** Switch/Button System

**Origin:** Zelda dungeon puzzles. Portal test chambers.

**Read:**
- [Switch Puzzle Design](https://www.gamedeveloper.com/design/switch-puzzle-design) — Switch puzzle design
- [Godot Signals](https://docs.godotengine.org/en/stable/tutorials/scripting/signals.html) — Official Godot signals docs

**Build:** Button (step to activate). Toggle switch (flip on/off). Timer switch (deactivates after delay).

**Art: Switch Sprites + Animations**

**Read:** [Switch Design in Pixel Art](https://www.youtube.com/results?search_query=switch+design+pixel+art) — YouTube search for switch pixel art

**Build:** Button (up/down states). Lever (left/right). Timer switch (countdown animation).

**Music: Puzzle SFX**

**Read:** [Puzzle Audio Design](https://www.youtube.com/results?search_query=puzzle+audio+design+games) — YouTube search for puzzle audio

**Build:** Button press SFX. Switch flip SFX. Timer tick SFX. Door open SFX.

**Level Design: Single-Switch Room**

**Read:** [Tutorial Puzzle Design](https://www.gamedeveloper.com/design/tutorial-puzzle-design) — Teaching puzzle mechanics

**Build:** One button. One door. Player learns cause → effect.

**UI: Puzzle Hint System**

**Read:** [Godot RichTextLabel](https://docs.godotengine.org/en/stable/classes/class_richtextlabel.html) — Official RichTextLabel docs

**Build:** Contextual hints ("Press the button to open the door"). Toggle hints on/off.

---

## DAWN 37: Pressure Plates & Weight

**Game Mechanic:** Pressure Plate + Weight System

**Origin:** Zelda block puzzles. Baba Is You rule manipulation.

**Read:**
- [Weight Puzzle Design](https://www.gamedeveloper.com/design/weight-puzzle-design) — Weight-based puzzle design
- [Godot RigidBody2D](https://docs.godotengine.org/en/stable/classes/class_rigidbody2d.html) — Official RigidBody2D docs for physics objects

**Build:** Pressure plate (needs weight). Player stands = light. Push block = heavy. Both = different result.

**Art: Block Sprites + Weight Indicators**

**Read:** [Block Puzzle Art](https://www.youtube.com/results?search_query=block+puzzle+art+pixel) — YouTube search for block puzzle art

**Build:** Small block (light). Large block (heavy). Weight indicator (scale icon).

**Music: Weight SFX**

**Read:** [Physics Audio Design](https://www.youtube.com/results?search_query=physics+audio+design+games) — YouTube search for physics audio

**Build:** Block push SFX. Pressure plate creak SFX. Weight match SFX.

**Level Design: Weight Puzzle Room**

**Read:** [Multi-Step Puzzle Design](https://www.gamedeveloper.com/design/multi-step-puzzle-design) — Multi-step puzzle principles

**Build:** Room with 2 pressure plates. Need to arrange blocks to open door.

**UI: Weight Display**

**Read:** [Godot Label](https://docs.godotengine.org/en/stable/classes/class_label.html) — Official Label docs

**Build:** Current weight on plate. Target weight. Visual indicator.

---

## DAWN 38: Sliding Blocks & Ice

**Game Mechanic:** Sliding Block + Ice Physics

**Origin:** Pokemon ice puzzles. Zelda ice caves.

**Read:**
- [Sliding Puzzle Design](https://www.gamedeveloper.com/design/sliding-puzzle-design) — Sliding puzzle design
- [Godot Physics Material](https://docs.godotengine.org/en/stable/classes/class_physicsmaterial.html) — Official PhysicsMaterial docs for friction

**Build:** Ice floor (slide until hit wall). Push blocks on ice. Momentum puzzles.

**Art: Ice Tiles + Slide Effects**

**Read:** [Ice Level Pixel Art](https://www.youtube.com/results?search_query=ice+level+pixel+art) — YouTube search for ice level art

**Build:** Ice floor tiles. Slide dust particles. Block slide animation.

**Music: Ice SFX**

**Read:** [Ice Audio Design](https://www.youtube.com/results?search_query=ice+audio+design+games) — YouTube search for ice audio

**Build:** Slide SFX. Ice crack SFX. Block hit wall SFX.

**Level Design: Ice Puzzle Room**

**Read:** [Ice Puzzle Level Design](https://www.gamedeveloper.com/design/ice-puzzle-level-design) — Ice puzzle room design

**Build:** Room where player must slide blocks into correct positions. Multiple solutions.

**UI: Move Counter**

**Read:** [Godot Label](https://docs.godotengine.org/en/stable/classes/class_label.html) — Official Label docs

**Build:** Move counter. Par display. Star rating (3 stars for par or below).

---

## DAWN 39: Key-Door & Multi-Step Puzzles

**Game Mechanic:** Key-Door + Sequence Puzzles

**Origin:** Zelda dungeon keys. Resident Evil key items.

**Read:**
- [Key-Door Design](https://www.gamedeveloper.com/design/key-door-design) — Key-door puzzle design
- [Godot AnimationPlayer](https://docs.godotengine.org/en/stable/classes/class_animationplayer.html) — Official AnimationPlayer docs

**Build:** Colored keys for colored doors. Multi-step: get key A → open door A → get key B → open door B.

**Art: Key & Door Sprites**

**Read:** [Key Design in Games](https://www.youtube.com/results?search_query=key+design+in+games+pixel+art) — YouTube search for key design

**Build:** 4 key colors. 4 door colors. Key pickup animation. Door open animation.

**Music: Key SFX**

**Read:** [Key Audio Design](https://www.youtube.com/results?search_query=key+audio+design+games) — YouTube search for key audio

**Build:** Key pickup SFX. Door unlock SFX. Door open SFX. Wrong key SFX.

**Level Design: Key Dungeon**

**Read:** [Dungeon Puzzle Design](https://www.gamedeveloper.com/design/dungeon-puzzle-design) — Dungeon puzzle layout

**Build:** 5-room dungeon. Each room has one puzzle. Final room = reward.

**UI: Key Inventory**

**Read:** [Godot HBoxContainer](https://docs.godotengine.org/en/stable/classes/class_hboxcontainer.html) — Official HBoxContainer docs

**Build:** Key display (icons). Collected keys highlighted. Current objective.

---

## DAWN 40: Vertical Slice Polish

**Game Mechanic:** Full Puzzle System

**Read:** [Puzzle Game Design](https://www.gamedeveloper.com/design/puzzle-game-design) — Comprehensive puzzle game design

**Build:** 10+ puzzle types. Hint system. Undo system. Skip option (costs points).

**Art: Full Puzzle Asset Pack**

**Read:** [Puzzle Game Asset Creation](https://www.youtube.com/results?search_query=puzzle+game+asset+creation) — YouTube search for puzzle assets

**Build:** All puzzle elements, environment, player, UI. Consistent style.

**Music: Full Puzzle Soundtrack**

**Read:** [Puzzle Music Composition](https://www.gamedeveloper.com/design/puzzle-music-composition) — Puzzle music composition

**Build:** 2 tracks (calm, thinking). All SFX. Dynamic intensity.

**Level Design: Full Puzzle Dungeon**

**Read:** [Puzzle Dungeon Design](https://www.gamedeveloper.com/design/puzzle-dungeon-design) — Puzzle dungeon layout

**Build:** 10-room dungeon. Difficulty curve. Secret rooms. Multiple solutions.

**UI: Full Puzzle UI**

**Read:** [Godot MarginContainer](https://docs.godotengine.org/en/stable/classes/class_margincontainer.html) — Official MarginContainer docs

**Build:** Hint button, undo button, move counter, star rating, pause menu, level select.

**BLOCK 8 DELIVERABLE:** Playable puzzle demo. Catalog all reusable pieces.

---

# 🌌 BLOCK 9: The Flyer (Dawns 41–45)
**Goal:** Space shooter, bullet patterns, power-ups, boss fight, screen shake

---

## DAWN 41: Space Shooter Basics

**Game Mechanic:** Space Shooter Movement + Shooting

**Origin:** Space Invaders (1978). Gradius (1985). Ikaruga (2001).

**Read:**
- [Shoot 'Em Up Design](https://www.gamedeveloper.com/design/shoot-em-up-design) — Shmup design principles
- [Godot CharacterBody2D](https://docs.godotengine.org/en/stable/classes/class_characterbody2d.html) — Official CharacterBody2D docs

**Build:** Ship moves in 8 directions. Shoot button. Bullet pool. Enemy spawns.

**Art: Ship + Bullet Sprites**

**Read:** [Space Shooter Sprite Design](https://www.youtube.com/results?search_query=space+shooter+sprite+design+pixel+art) — YouTube search for space shooter sprites

**Build:** Player ship (16×16). Enemy ships (2 types). Bullet sprites (player + enemy).

**Music: Shooter Music**

**Read:** [Shooter Music Composition](https://www.gamedeveloper.com/design/shooter-music-composition) — Shmup music composition

**Build:** Fast-paced 2-minute loop. Synthwave style. High energy.

**Level Design: Wave System**

**Read:** [Wave Design in Shooters](https://www.gamedeveloper.com/design/wave-design-in-shooters) — Wave system design

**Build:** 5 waves. Each wave has pattern. Difficulty increases.

**UI: Score + Lives**

**Read:** [Godot Label](https://docs.godotengine.org/en/stable/classes/class_label.html) — Official Label docs

**Build:** Score (top). Lives (ship icons). High score.

---

## DAWN 42: Bullet Patterns

**Game Mechanic:** Bullet Hell Patterns

**Origin:** Touhou Project bullet patterns. Enter the Gungeon patterns.

**Read:**
- [Bullet Hell Design](https://www.gamedeveloper.com/design/bullet-hell-design) — Bullet hell pattern design
- [Godot Timer](https://docs.godotengine.org/en/stable/classes/class_timer.html) — Official Timer docs for pattern timing

**Build:** Spiral pattern. Spread pattern. Aimed pattern. Wave pattern. Bullet pooling.

**Art: Bullet Pattern Visuals**

**Read:** [Bullet Pattern Art](https://www.youtube.com/results?search_query=bullet+pattern+art+pixel) — YouTube search for bullet pattern art

**Build:** Different bullet colors per pattern. Trail effects. Warning indicators.

**Music: Bullet SFX**

**Read:** [Bullet Audio Design](https://www.youtube.com/results?search_query=bullet+audio+design+games) — YouTube search for bullet audio

**Build:** Player shoot SFX. Enemy shoot SFX. Bullet hit SFX. Pattern change SFX.

**Level Design: Pattern Recognition**

**Read:** [Pattern-Based Level Design](https://www.gamedeveloper.com/design/pattern-based-level-design) — Pattern recognition in levels

**Build:** Each wave teaches one pattern. Final wave combines patterns.

**UI: Pattern Warning**

**Read:** [Godot ColorRect](https://docs.godotengine.org/en/stable/classes/class_colorrect.html) — Official ColorRect docs

**Build:** Screen edge flash before pattern. Boss pattern name display.

---

## DAWN 43: Power-Up System

**Game Mechanic:** Power-Ups (Spread, Speed, Shield)

**Origin:** Gradius power-up bar. R-Type force pod.

**Read:**
- [Power-Up System Design](https://www.gamedeveloper.com/design/power-up-system-design) — Power-up system design
- [Godot Area2D](https://docs.godotengine.org/en/stable/classes/class_area2d.html) — Official Area2D docs for pickup detection

**Build:** Power-up drops from enemies. Spread shot (3 bullets). Speed up. Shield (1 hit).

**Art: Power-Up Sprites**

**Read:** [Power-Up Design in Pixel Art](https://www.youtube.com/results?search_query=power+up+design+pixel+art) — YouTube search for power-up art

**Build:** 3 power-up icons. Glow effect. Collect animation. Active indicator.

**Music: Power-Up SFX**

**Read:** [Power-Up Audio Design](https://www.youtube.com/results?search_query=power+up+audio+design+games) — YouTube search for power-up audio

**Build:** Power-up spawn SFX. Collect SFX. Active SFX. Lose power-up SFX.

**Level Design: Power-Up Placement**

**Read:** [Power-Up Placement in Shooters](https://www.gamedeveloper.com/design/power-up-placement-in-shooters) — Strategic power-up placement

**Build:** Power-ups before hard sections. Risk/reward (power-up in enemy dense area).

**UI: Power-Up Display**

**Read:** [Godot HBoxContainer](https://docs.godotengine.org/en/stable/classes/class_hboxcontainer.html) — Official HBoxContainer docs

**Build:** Active power-up icons. Duration timer. Stacked power-ups display.

---

## DAWN 44: Boss Fight

**Game Mechanic:** Boss Fight (Phases, Weak Points)

**Origin:** Contra boss fights. Dark Souls phase transitions. Hollow Knight's multi-phase battles codified the modern indie standard.

**Read:**
- [Boss Battle Design and Structure](https://www.gamedeveloper.com/design/boss-battle-design-and-structure) — Boss battle design theory
- [Hollow Knight Inspired Boss Fight in Godot 4](https://ludonauta.itch.io/platformer-essentials/devlog/1089921/hollow-knight-inspired-boss-fight-in-godot-4) — Hollow Knight style boss in Godot 4
- [Boss Design: How to Make an Unforgettable Boss Battle](https://gamedesignskills.com/game-design/game-boss-design/) — Boss design principles
- [Godot AnimationTree](https://docs.godotengine.org/en/stable/classes/class_animationtree.html) — Official AnimationTree docs for phase transitions
- [Godot AnimationNodeStateMachine](https://docs.godotengine.org/en/stable/tutorials/animation/animation_tree.html) — Animation state machine tutorial

**Build:** Boss with 3 phases. Phase 1: simple patterns. Phase 2: faster + new patterns. Phase 3: desperation. Use `AnimationTree` with `AnimationNodeStateMachine` for phase transitions. Weak points that must be hit to deal damage. The boss should have defensive capabilities — force fields, teleportation, or knockback attacks — so the player cannot simply stun-lock it.

**Art: Boss Sprite + Phases**

**Read:**
- [Pixelblog - Top Down Character Animation](https://www.slynyrd.com/blog/2025/3/24/pixelblog-55-top-down-character-animation) — Slynyrd pixel art tutorial
- [Pixel Art Boss Design Principles](https://www.youtube.com/results?search_query=pixel+art+boss+design) — YouTube search for boss pixel art

**Build:** 64×64 boss sprite. Phase 1 (calm color palette). Phase 2 (damaged, angrier expression). Phase 3 (desperate, glowing red, cracked). Weak point indicator (glowing spot that changes position). Phase transition animation (screen shake + transform flash + sprite swap).

**Music: Boss Music**

**Read:** [Boss Music Composition](https://www.gamedeveloper.com/design/boss-music-composition) — Boss music composition

**Build:** 3-layer boss track. Phase 1 = calm tension (strings + low drums). Phase 2 = intense drums + brass. Phase 3 = full orchestra + choir + synth. Use `AudioStreamInteractive` for seamless crossfade between phases.

**Level Design: Boss Arena**

**Read:** [Boss Arena Design](https://www.gamedeveloper.com/design/boss-arena-design) — Boss arena layout principles

**Build:** Large arena with environmental hazards (spikes, lava pools). Safe zones behind cover. Arena changes between phases — platforms break, new hazards appear, layout shifts.

**UI: Boss Health Bar**

**Read:**
- [Godot ProgressBar](https://docs.godotengine.org/en/stable/classes/class_progressbar.html) — Official ProgressBar docs
- [Godot TextureProgressBar](https://docs.godotengine.org/en/stable/classes/class_textureprogressbar.html) — Official TextureProgressBar docs

**Build:** Large boss health bar (top-center of screen). Phase indicator icon. Weak point marker (flashes when vulnerable). Damage numbers on hit. Boss name display.

---

## DAWN 45: Vertical Slice Polish

**Game Mechanic:** Full Space Shooter System

**Read:**
- [Shoot 'Em Up Design](https://www.gamedeveloper.com/design/shoot-em-up-design) — Shmup design principles
- [Bullet Hell Design Patterns](https://www.gamedeveloper.com/design/bullet-hell-design-patterns) — Bullet hell pattern design

**Build:** Complete space shooter. Player ship with 8-direction movement. 5 bullet patterns (spiral, spread, aimed, wave, circle). 3 power-up types (spread shot, speed boost, shield). 3-phase boss. Wave system with 5 waves. Score + lives system. Bullet pooling for performance.

**Art: Full Space Shooter Asset Pack**

**Read:**
- [Space Shooter Sprite Design](https://www.slynyrd.com) — Slynyrd pixel art tutorials
- [Sprite Sheet Organization](https://www.aseprite.org/docs/sprite-sheet/) — Official Aseprite sprite sheet docs

**Build:** Player ship (16×16, 8 orientations). 3 enemy types (basic, fast, tank). Boss (64×64, 3 phases). All bullet sprites (different colors per pattern). Power-up icons. Background stars (parallax). Explosion animations (8 frames). Shield effect.

**Music: Full Space Shooter Soundtrack**

**Read:**
- [Shooter Music Composition](https://www.gamedeveloper.com/design/shooter-music-composition) — Shmup music composition
- [Dynamic Music in Games](https://www.gamedeveloper.com/design/dynamic-music-in-games) — Dynamic music systems

**Build:** 3 tracks (menu, gameplay, boss). All SFX (shoot, hit, explosion, power-up, boss phase change). Stem mixing for intensity layers. Loop points verified.

**Level Design: Complete Space Level**

**Read:**
- [Scrolling Level Design](https://www.gamedeveloper.com/design/scrolling-level-design) — Scrolling level design
- [Wave Design in Shooters](https://www.gamedeveloper.com/design/wave-design-in-shooters) — Wave system design

**Build:** 3-minute level. Intro (easy waves) → Build-up (more enemies, mixed types) → Climax (bullet hell patterns) → Boss arena. Parallax starfield background (3 layers). Asteroid obstacles in later waves.

**UI: Full Space Shooter UI**

**Read:**
- [Godot UI System](https://docs.godotengine.org/en/stable/tutorials/ui/index.html) — Official Godot UI docs
- [Godot Theme Editor](https://docs.godotengine.org/en/stable/tutorials/ui/gui_theme_editor.html) — Official Godot theme editor docs

**Build:** Title screen (animated logo), gameplay HUD (score, lives, power-ups), pause menu, game over, high score table, victory screen. Sci-fi pixel art theme. Neon color palette.

**BLOCK 9 DELIVERABLE:** Playable space shooter demo. Catalog all reusable pieces.

---

# 🗺️ BLOCK 10: The Explorer (Dawns 46–50)
**Goal:** Procedural generation, fog of war, map discovery, biome system

---

## DAWN 46: Procedural Generation Basics

**Game Mechanic:** Procedural Map Generation

**Origin:** Minecraft (2009) infinite worlds. No Man's Sky (2016) procedural universe.

**Read:**
- [Procedural Map Generation with Godot](https://medium.com/pumpkinbox-blog/procedural-map-generation-with-godot-part-1-1b4e78191e90) — Procedural generation tutorial series
- [Procedural Generation in Godot 4: 5 Patterns](https://ziva.sh/blogs/godot-procedural-generation) — 5 procedural patterns from real indie games
- [Godot FastNoiseLite](https://docs.godotengine.org/en/stable/classes/class_fastnoiselite.html) — Official FastNoiseLite docs

**Build:** Use `FastNoiseLite` to generate terrain heightmaps. Map noise values to tile types: water (< -0.3), sand (-0.3 to 0), grass (0 to 0.3), forest (0.3 to 0.6), mountain (> 0.6). Random seed for different worlds each time. Use 2D array to store map data.

**Art: Biome Tilesets**

**Read:**
- [Biome Tile Design](https://www.slynyrd.com) — Slynyrd pixel art tutorials
- [Tileset Creation for Procedural Maps](https://www.youtube.com/results?search_query=tileset+creation+procedural+maps) — YouTube search for procedural tilesets

**Build:** 5 biome tilesets: water (animated), beach, grassland, forest, mountain. Each with 4-6 tiles for variation. Consistent 16×16 style. Use DawnBringer 16 palette.

**Music: Ambient Biome Music**

**Read:**
- [Ambient Music in Games](https://www.gamedeveloper.com/design/ambient-music-in-games) — Ambient music composition
- [Procedural Music Basics](https://www.youtube.com/results?search_query=procedural+music+basics) — YouTube search for procedural music

**Build:** 5 short ambient loops (one per biome). Soft, atmospheric, no percussion. Crossfade between biomes as player moves.

**Level Design: Procedural World Layout**

**Read:** [Procedural World Design](https://medium.com/pumpkinbox-blog/procedural-map-generation-with-godot-part-1-1b4e78191e90) — Procedural world layout

**Build:** 100×100 tile world. Biomes distributed by noise layers (temperature + moisture). Rivers using random walker algorithm. Ensure all land is connected (flood fill check).

**UI: World Map**

**Read:**
- [Godot SubViewport](https://docs.godotengine.org/en/stable/classes/class_subviewport.html) — Official SubViewport docs
- [Minimap Implementation](https://docs.godotengine.org/en/stable/tutorials/rendering/viewports.html) — Godot viewport tutorial

**Build:** Full-screen world map (press M). Shows discovered areas only. Player position marker. Biome legend. Zoom in/out with mouse wheel.

---

## DAWN 47: Fog of War

**Game Mechanic:** Fog of War + Map Discovery

**Origin:** Warcraft (1994) RTS fog. Civilization map discovery.

**Read:**
- [Godot Fog of War 2D](https://github.com/AntonWedemeier/Godot-FogOfWar) — Fog of war implementation for Godot
- [Fog of War Shader](https://godotforums.org/d/27770-fog-of-war-2d-simple-implementation-demo) — Simple fog of war shader demo
- [Godot Viewport Texture](https://docs.godotengine.org/en/stable/tutorials/rendering/viewports.html) — Godot viewport texture tutorial

**Build:** Fog overlay using `Viewport` + custom Shader. Player vision radius reveals fog. Discovered areas stay semi-visible (gray). Undiscovered = black. Dynamic updates as player moves. Use `ViewportTexture` for fog texture.

**Art: Fog & Discovery Visuals**

**Read:** [Fog of War Visual Design](https://www.gamedeveloper.com/design/fog-of-war-visual-design) — Fog of war visual design

**Build:** Fog texture (semi-transparent black). Discovered area tint (dark gray). Vision radius gradient (soft edges). Discovery particle effect (fog clearing burst).

**Music: Discovery SFX**

**Read:** [Discovery Audio in Games](https://www.gamedeveloper.com/design/discovery-audio-in-games) — Discovery audio design

**Build:** Area discovery chime (magical bell). New biome discovery fanfare. Map open SFX (unfolding paper). Map close SFX.

**Level Design: Exploration Pacing**

**Read:** [Exploration Game Design](https://www.gamedeveloper.com/design/exploration-game-design) — Exploration game design

**Build:** Points of interest distributed across map. Some hidden behind fog. Rewards for exploration (treasure, landmarks, lore). Natural landmarks guide player direction.

**UI: Discovery Log**

**Read:**
- [Godot ItemList](https://docs.godotengine.org/en/stable/classes/class_itemlist.html) — Official ItemList docs
- [Godot RichTextLabel](https://docs.godotengine.org/en/stable/classes/class_richtextlabel.html) — Official RichTextLabel docs

**Build:** Discovery log (discovered landmarks, biomes, creatures). Percentage discovered display. Lore entries unlocked by discovery. Scrollable list with icons.

---

## DAWN 48: Biome System

**Game Mechanic:** Biome Transitions + Environmental Effects

**Origin:** Terraria biome system. Minecraft biomes with unique mobs/resources.

**Read:**
- [Biome System Design](https://www.gamedeveloper.com/design/biome-system-design) — Biome system design
- [Godot Area2D Environment Detection](https://docs.godotengine.org/en/stable/classes/class_area2d.html) — Official Area2D docs for biome zones

**Build:** Biome detection using `Area2D` zones. Each biome has: unique enemies, resources, weather, music. Smooth transitions between biomes (gradual color shift via `CanvasModulate`, music crossfade).

**Art: Biome-Specific Assets**

**Read:** [Biome Visual Design](https://www.slynyrd.com) — Slynyrd pixel art tutorials

**Build:** Biome-specific enemies (5 types total). Biome resources (trees, rocks, flowers). Weather effects (rain particles in forest, sandstorm in desert). Biome color grading.

**Music: Biome Music Transitions**

**Read:**
- [Adaptive Music in Games](https://www.gamedeveloper.com/design/adaptive-music-in-games) — Adaptive music systems
- [Godot Audio Buses](https://docs.godotengine.org/en/stable/tutorials/audio/audio_buses.html) — Official Godot audio bus docs

**Build:** 5 biome tracks that share a musical motif (leitmotif). Seamless crossfade using `AudioStreamInteractive`. Dynamic intensity based on biome danger level.

**Level Design: Biome Distribution**

**Read:** [Biome Placement in Open Worlds](https://www.gamedeveloper.com/design/biome-placement-in-open-worlds) — Biome placement principles

**Build:** Biomes arranged logically: water → beach → grass → forest → mountain. Transition zones between biomes (2-3 tiles). Rare biomes in hard-to-reach areas.

**UI: Biome Indicator**

**Read:**
- [Godot Label](https://docs.godotengine.org/en/stable/classes/class_label.html) — Official Label docs
- [Godot TextureRect](https://docs.godotengine.org/en/stable/classes/class_texturerect.html) — Official TextureRect docs

**Build:** Current biome name display (bottom-left). Biome icon. Environmental hazard warnings. Resource availability indicator.

---

## DAWN 49: Map Icons & Fast Travel

**Game Mechanic:** Map Discovery + Fast Travel System

**Origin:** The Legend of Zelda: Breath of the Wild (2017) tower discovery. Elden Ring (2022) Sites of Grace.

**Read:**
- [Fast Travel System Design](https://www.gamedeveloper.com/design/fast-travel-system-design) — Fast travel system design
- [Godot Button Signals](https://docs.godotengine.org/en/stable/tutorials/scripting/signals.html) — Official Godot signals docs

**Build:** Discoverable landmarks (towers, camps, caves). Map icons appear on discovery. Fast travel unlocked at landmarks. Fast travel menu with world map and list view.

**Art: Map Icons + Landmarks**

**Read:**
- [Map Icon Design](https://www.slynyrd.com) — Slynyrd pixel art tutorials
- [Pixel Art Icon Design](https://www.youtube.com/results?search_query=pixel+art+icon+design) — YouTube search for icon design

**Build:** 10 map icons (tower, camp, cave, boss, treasure, village, etc.). Landmark sprites (tower, campfire, cave entrance). Fast travel portal effect (swirling particles).

**Music: Landmark Discovery**

**Read:** [Landmark Audio in Open Worlds](https://www.gamedeveloper.com/design/landmark-audio-in-open-worlds) — Landmark audio design

**Build:** Landmark discovery fanfare. Fast travel SFX (warp sound). Landmark ambient sounds (wind at tower, fire crackle at camp).

**Level Design: Landmark Placement**

**Read:** [Landmark Placement in Open Worlds](https://www.gamedeveloper.com/design/landmark-placement-in-open-worlds) — Landmark placement principles

**Build:** Landmarks distributed for optimal fast travel coverage. Each landmark has unique view. Some landmarks require puzzle/challenge to unlock.

**UI: Fast Travel Menu**

**Read:**
- [Godot GridContainer](https://docs.godotengine.org/en/stable/classes/class_gridcontainer.html) — Official GridContainer docs
- [Godot ScrollContainer](https://docs.godotengine.org/en/stable/classes/class_scrollcontainer.html) — Official ScrollContainer docs

**Build:** Fast travel menu with map + list. Landmark thumbnails. Travel cost indicator (if any). Locked/unlocked status. Confirmation dialog.

---

## DAWN 50: Vertical Slice Polish

**Game Mechanic:** Full Exploration System

**Read:**
- [Open World Game Design](https://www.gamedeveloper.com/design/open-world-game-design) — Open world design principles
- [Procedural Generation Best Practices](https://ziva.sh/blogs/godot-procedural-generation) — Procedural generation best practices

**Build:** Complete exploration demo. Procedural 100×100 world. 5 biomes. Fog of war. Map discovery. 10 landmarks. Fast travel. Biome-specific enemies and resources.

**Art: Full Explorer Asset Pack**

**Read:**
- [Open World Asset Creation](https://www.slynyrd.com) — Slynyrd pixel art tutorials
- [Consistent Art Style Guide](https://www.youtube.com/results?search_query=consistent+art+style+guide+pixel+art) — YouTube search for art style consistency

**Build:** All biome tilesets. 10+ enemy types. 15+ resources. Landmark sprites. Weather effects. Map icons. Player sprites for all biomes. Parallax backgrounds.

**Music: Full Explorer Soundtrack**

**Read:**
- [Open World Music Composition](https://www.gamedeveloper.com/design/open-world-music-composition) — Open world music composition
- [Adaptive Music Systems](https://www.gamedeveloper.com/design/adaptive-music-systems) — Adaptive music systems

**Build:** 5 biome tracks + 1 exploration theme. All SFX. Dynamic music system with crossfades. Ambient soundscapes per biome.

**Level Design: Complete Open World**

**Read:** [Open World Level Design](https://www.gamedeveloper.com/design/open-world-level-design) — Open world level design

**Build:** Fully explorable world with points of interest. Hidden secrets. Difficulty curve: safe center → dangerous edges. Natural player guidance through landmark visibility.

**UI: Full Explorer UI**

**Read:** [Godot UI Best Practices](https://docs.godotengine.org/en/stable/tutorials/ui/index.html) — Official Godot UI best practices

**Build:** World map, discovery log, biome indicator, fast travel menu, inventory, settings. Consistent fantasy exploration theme.

**BLOCK 10 DELIVERABLE:** Playable exploration demo. Catalog all reusable pieces.

---

# 🏰 BLOCK 11: The Siege (Dawns 51–55)
**Goal:** Tower defense, pathfinding, tower upgrades, wave system

---

## DAWN 51: Tower Defense Core

**Game Mechanic:** Tower Defense Base System

**Origin:** Warcraft III Tower Defense maps (2002). Bloons TD (2007). Kingdom Rush (2011).

**Read:**
- [Tower Defense Core Gameplay - GDQuest](https://school.gdquest.com/courses/learn_2d_gamedev_godot_4/tower_defense_core_gameplay/overview) — GDQuest tower defense course
- [Dynamic Pathing Algorithm for Tower Defense](https://gamedev.stackexchange.com/questions/1003/dynamic-pathing-algorithm-for-tower-defense-game) — Dynamic pathing for TD
- [Godot Tower Defense Patterns](https://lobehub.com/skills/thedivergentai-gd-agentic-skills-godot-genre-tower-defense) — Tower defense patterns

**Build:** Grid-based placement. Enemy path (`Path2D` + `PathFollow2D`). Basic tower (shoots at nearest enemy). Enemy waves with `Timer`. Lives system. Currency system (gold per kill).

**Art: Tower Defense Base Assets**

**Read:**
- [Tower Defense Sprite Design](https://www.slynyrd.com) — Slynyrd pixel art tutorials
- [Tower Sprite Design](https://www.gamedeveloper.com/design/tower-sprite-design) — Tower sprite design

**Build:** 3 basic towers: Arrow (wooden), Cannon (stone), Magic (crystal). 2 enemy types: Goblin (fast, low HP), Orc (slow, high HP). Path tiles (dirt road). Base/castle sprite. Projectile sprites.

**Music: Tower Defense Music**

**Read:** [Strategy Game Music](https://www.gamedeveloper.com/design/strategy-game-music) — Strategy game music composition

**Build:** Calm building phase music. Intense wave music. Victory/defeat stingers. Tower build SFX (hammer). Enemy death SFX.

**Level Design: First TD Map**

**Read:**
- [Tower Defense Map Design](https://www.gamedeveloper.com/design/tower-defense-map-design) — TD map design
- [TD Path Design](https://gamedev.stackexchange.com/questions/1003/dynamic-pathing-algorithm-for-tower-defense-game) — TD path design

**Build:** Simple winding path. Buildable areas on sides. Single entrance/exit. Tutorial layout (teaches placement, upgrading, wave system).

**UI: TD HUD**

**Read:** [Godot UI for Strategy Games](https://docs.godotengine.org/en/stable/tutorials/ui/index.html) — Official Godot UI docs

**Build:** Lives display (hearts). Currency display (gold coin icon). Wave counter. Tower selection panel (3 towers). Build mode indicator. Speed up button (2x).

---

## DAWN 52: Tower Variety & Upgrades

**Game Mechanic:** Tower Types + Upgrade System

**Origin:** Kingdom Rush tower branches. Bloons TD upgrade paths.

**Read:**
- [Tower Upgrade System Design](https://school.gdquest.com/courses/learn_2d_gamedev_godot_4/tower_defense_core_gameplay/overview) — Tower upgrade system design
- [Godot Resource-Based Upgrades](https://docs.godotengine.org/en/stable/tutorials/scripting/resources.html) — Official Godot resources docs

**Build:** 5 tower types with distinct roles: Arrow (fast, single target), Cannon (slow, AoE), Magic (armor piercing), Ice (slows enemies), Sniper (long range, high damage). Upgrade paths: each tower has 2 upgrade branches (e.g., Arrow → Rapid Fire OR Poison Arrows).

**Art: Tower Upgrade Visuals**

**Read:** [Upgrade Visual Design](https://www.slynyrd.com) — Slynyrd pixel art tutorials

**Build:** Tower sprites for all 5 types + 2 upgrade levels each. Upgrade effect particles (glow burst). Range indicator (circle on hover). Attack visual effects (different per tower).

**Music: Upgrade SFX**

**Read:** [Upgrade Audio Design](https://www.gamedeveloper.com/design/upgrade-audio-design) — Upgrade audio design

**Build:** Tower build SFX (construction). Upgrade SFX (level up chime). Sell SFX (coins). Tower attack SFX (5 distinct types). Upgrade complete fanfare.

**Level Design: Multi-Path Map**

**Read:** [Multi-Path TD Design](https://www.gamedeveloper.com/design/multi-path-td-design) — Multi-path TD map design

**Build:** Map with 2 paths converging at the base. Requires strategic tower placement. Some build spots cover both paths. Forces player to choose: cover both weakly OR cover one strongly.

**UI: Tower Info Panel**

**Read:**
- [Godot PanelContainer](https://docs.godotengine.org/en/stable/classes/class_panelcontainer.html) — Official PanelContainer docs
- [Godot Tooltip System](https://docs.godotengine.org/en/stable/tutorials/ui/index.html) — Official Godot UI docs

**Build:** Tower info panel (click tower to see stats: damage, range, speed). Upgrade buttons with costs. Sell button (returns 70% gold). Range highlight on hover. Damage/attack speed display.

---

## DAWN 53: Enemy Variety & Wave System

**Game Mechanic:** Enemy Types + Wave Manager

**Origin:** Plants vs Zombies enemy variety. Kingdom Rush wave system.

**Read:**
- [Wave System Design](https://lobehub.com/skills/thedivergentai-gd-agentic-skills-godot-genre-tower-defense) — Wave system design
- [Godot Timer Wave Spawning](https://docs.godotengine.org/en/stable/classes/class_timer.html) — Official Timer docs

**Build:** 5 enemy types: Basic (balanced), Fast (low HP, high speed), Tank (high HP, slow), Flying (ignores path, straight to base), Boss (high HP, special abilities). Wave manager with groups, delays, and intervals. Wave preview (shows incoming enemies).

**Art: Enemy Variety Sprites**

**Read:** [Enemy Design for TD Games](https://www.slynyrd.com) — Slynyrd pixel art tutorials

**Build:** 5 enemy sprites + walk animations. Flying enemy (bat/dragon with wing flap). Boss enemy (large, detailed, 2-frame idle). Enemy status effects (frozen = blue tint, poisoned = green tint, burning = orange tint).

**Music: Enemy SFX + Wave Alerts**

**Read:** [Wave Audio Design](https://www.gamedeveloper.com/design/wave-audio-design) — Wave audio design

**Build:** Enemy spawn SFX (per type). Wave start alert (horn). Wave complete fanfare. Boss spawn warning (dramatic sting). Enemy ability SFX.

**Level Design: Difficulty Scaling**

**Read:** [TD Difficulty Curves](https://www.gamedeveloper.com/design/td-difficulty-curves) — TD difficulty curve design

**Build:** 10 waves with escalating difficulty. Early waves teach enemy types (1 type per wave). Mid waves mix types. Late waves introduce combinations. Boss wave every 5 waves.

**UI: Wave Info + Enemy Preview**

**Read:**
- [Godot HBoxContainer](https://docs.godotengine.org/en/stable/classes/class_hboxcontainer.html) — Official HBoxContainer docs
- [Godot TextureRect](https://docs.godotengine.org/en/stable/classes/class_texturerect.html) — Official TextureRect docs

**Build:** Wave preview bar (shows next 5 waves with icons). Enemy type icons with counts. Wave timer countdown. Wave progress bar. Boss warning indicator (flashing red).

---

## DAWN 54: Advanced TD Mechanics

**Game Mechanic:** Special Abilities + Tower Synergies

**Origin:** Kingdom Rush hero abilities. Bloons TD special agents.

**Read:**
- [TD Special Mechanics](https://www.gamedeveloper.com/design/td-special-mechanics) — TD special mechanics design
- [Godot Ability System](https://docs.godotengine.org/en/stable/tutorials/scripting/signals.html) — Official Godot signals docs for abilities

**Build:** Player abilities: Meteor Strike (AoE damage), Heal Base (restore lives), Freeze All (stun all enemies for 3s). Tower synergies: Ice + Cannon = Shatter (frozen enemies take 2x damage from Cannon). Combo system: kill 5 enemies in 3 seconds = bonus gold. Mazing validation (ensure path always exists after tower placement).

**Art: Ability Effects**

**Read:** [Ability VFX in Pixel Art](https://www.slynyrd.com) — Slynyrd pixel art tutorials

**Build:** Meteor strike animation (falling rock + explosion burst). Heal effect (green aura + particles). Freeze effect (blue wave expanding). Synergy indicator (glowing chain between Ice and Cannon towers).

**Music: Ability SFX**

**Read:** [Ability Audio Design](https://www.gamedeveloper.com/design/ability-audio-design) — Ability audio design

**Build:** Meteor SFX (rumble + impact). Heal SFX (magical chime). Freeze SFX (ice crack). Combo SFX (rising pitch chime). Synergy activation SFX (power chord).

**Level Design: Advanced Maps**

**Read:** [Advanced TD Map Design](https://www.gamedeveloper.com/design/advanced-td-map-design) — Advanced TD map design

**Build:** 3 maps: Easy (single path, wide build areas), Medium (split paths, limited build spots), Hard (mazing allowed, tight spaces, flying enemies on separate aerial path).

**UI: Ability Bar + Combo Display**

**Read:**
- [Godot ProgressBar](https://docs.godotengine.org/en/stable/classes/class_progressbar.html) — Official ProgressBar docs
- [Godot AnimationPlayer](https://docs.godotengine.org/en/stable/classes/class_animationplayer.html) — Official AnimationPlayer docs

**Build:** Ability hotbar (3 abilities, cooldown indicators). Combo counter (resets after 3s). Synergy active indicator. Mazing path validation feedback (green = valid placement, red = blocks path).

---

## DAWN 55: Vertical Slice Polish

**Game Mechanic:** Full Tower Defense System

**Read:**
- [Tower Defense Game Design](https://www.gamedeveloper.com/design/tower-defense-game-design) — Comprehensive TD design
- [TD Best Practices](https://lobehub.com/skills/thedivergentai-gd-agentic-skills-godot-genre-tower-defense) — TD best practices

**Build:** Complete TD game. 5 tower types with 2 upgrade branches each. 5 enemy types. 10+ waves across 3 maps. Special abilities. Tower synergies. Combo system. Mazing validation. Save/load progression.

**Art: Full TD Asset Pack**

**Read:** [TD Asset Creation Guide](https://www.slynyrd.com) — Slynyrd pixel art tutorials

**Build:** All towers (base + 2 upgrades × 5 types = 15 sprites). All enemies (5 types × animations). Projectiles (5 types). Effects (build, upgrade, ability, death). Map tiles (3 themes: grass, desert, snow). Backgrounds (3 themes).

**Music: Full TD Soundtrack**

**Read:** [TD Music Composition](https://www.gamedeveloper.com/design/td-music-composition) — TD music composition

**Build:** 3 map themes (grass, desert, snow). Battle music (3 intensity layers: calm, medium, intense). All SFX. Victory/defeat music. Menu music.

**Level Design: Complete TD Campaign**

**Read:** [TD Campaign Design](https://www.gamedeveloper.com/design/td-campaign-design) — TD campaign design

**Build:** 3-map campaign with difficulty progression. Map 1: single path, tutorial. Map 2: split paths, flying enemies. Map 3: mazing, all enemy types, final boss. Star rating per map (3 stars = no lives lost).

**UI: Full TD UI**

**Read:** [Godot UI System](https://docs.godotengine.org/en/stable/tutorials/ui/index.html) — Official Godot UI docs

**Build:** Title screen, map select (3 maps with star ratings), gameplay HUD, tower info panel, upgrade panel, ability bar, wave info, pause menu, victory/defeat screens, settings, credits.

**BLOCK 11 DELIVERABLE:** Playable tower defense demo. Catalog all reusable pieces.

---

# 🎭 BLOCK 12: The Finale (Dawns 56–60)
**Goal:** Everything combined. Metroidvania with ALL previous mechanics

---

## DAWN 56: Metroidvania Core — Movement + Combat

**Game Mechanic:** Full Player Controller (All Previous Movement + Combat)

**Origin:** Hollow Knight (2017) — the gold standard for indie metroidvanias. Castlevania: Symphony of the Night (1997) — the genre origin.

**Read:**
- [Make a Metroidvania in Godot 4](https://academy.zenva.com/product/godot-metroidvania/) — Zenva metroidvania course
- [Hollow Knight Design Analysis](https://www.gamedeveloper.com/design/hollow-knight-design-analysis) — Hollow Knight design analysis
- [Godot State Machine for Metroidvania](https://ludonauta.itch.io/platformer-essentials/devlog/1089921/hollow-knight-inspired-boss-fight-in-godot-4) — Hollow Knight inspired state machine

**Build:** Combine ALL movement from previous blocks: walk, run, jump, double jump, wall jump, wall slide, dash, crouch, climb (from Block 2/3). Combine ALL combat: light attack combo, heavy attack, ranged spells, blocking, parry (from Block 2/3/9). State machine with 20+ states. `AnimationTree` integration for smooth transitions.

**Art: Full Player Sprite Sheet (20+ Animations)**

**Read:**
- [Metroidvania Character Design](https://www.slynyrd.com) — Slynyrd pixel art tutorials
- [Complex Sprite Sheet Organization](https://www.gamedeveloper.com/design/complex-sprite-sheet-organization) — Complex sprite sheet organization

**Build:** 32×32 player with 20+ animations: idle, run, jump, fall, double jump, wall slide, wall jump, dash, crouch, crouch walk, climb, attack light 1/2/3, attack heavy, attack air, block, parry, hurt, die, respawn, interact, spell cast. Proper Aseprite tags for each animation.

**Music: Metroidvania Theme**

**Read:**
- [Metroidvania Music Composition](https://www.gamedeveloper.com/design/metroidvania-music-composition) — Metroidvania music composition
- [Atmospheric Game Music](https://www.gamedeveloper.com/design/atmospheric-game-music) — Atmospheric music composition

**Build:** Main character theme (heroic yet melancholic). 2-minute loop. Leitmotif (recurring musical phrase) that appears in multiple tracks. Combat variation (adds drums). Ambient variation (removes melody).

**Level Design: Hub World Layout**

**Read:**
- [Metroidvania World Design](https://academy.zenva.com/product/godot-metroidvania/) — Metroidvania world design
- [Interconnected Level Design](https://www.gamedeveloper.com/design/interconnected-level-design) — Interconnected level design

**Build:** Central hub area connecting to 4 biomes. Each biome has unique mechanic from previous blocks. Hub has: save point (bench), shop, fast travel shrine. Locked doors requiring abilities from other biomes (ability gating).

**UI: Full Metroidvania HUD**

**Read:** [Godot UI Best Practices](https://docs.godotengine.org/en/stable/tutorials/ui/index.html) — Official Godot UI best practices

**Build:** Health bar (Hollow Knight souls-style, masks). Mana bar (for spells). Mini-map (top-right, discovered areas). Ability icons (bottom, unlocked abilities). Currency display (geo/gold). Key items. Status effects (poison, burn). Boss health bar (when in combat).

---

## DAWN 57: Biome Integration — All Previous Worlds

**Game Mechanic:** Multi-Biome World with All Mechanics

**Origin:** Hollow Knight's diverse areas (Greenpath, City of Tears, Deepnest). Ori and the Blind Forest (2015) biome variety.

**Read:**
- [Biome Integration in Metroidvanias](https://www.gamedeveloper.com/design/biome-integration-in-metroidvanias) — Biome integration design
- [Godot Scene Transition](https://docs.godotengine.org/en/stable/tutorials/scripting/change_scenes_manually.html) — Official Godot scene transition docs

**Build:** 4 biomes from previous blocks integrated into one world: Forest (Block 1 runner mechanics — auto-scroll sections), Cave (Block 2 combat — tight corridors, melee focus), Tower (Block 3 spell casting — verticality, puzzles), Underwater (Block 7 diving — swimming, oxygen). Seamless transitions between biomes (no loading screens).

**Art: Complete Biome Tileset Library**

**Read:**
- [Consistent Art Across Biomes](https://www.slynyrd.com) — Slynyrd pixel art tutorials
- [Metroidvania Environment Art](https://www.gamedeveloper.com/design/metroidvania-environment-art) — Metroidvania environment art

**Build:** All biome tilesets from previous blocks, unified under consistent palette (DawnBringer 16 with modifications). Transition tiles between biomes. Biome-specific decorations. Parallax backgrounds for each biome (3 layers each).

**Music: Biome Music Integration**

**Read:**
- [Adaptive Music Across Biomes](https://www.gamedeveloper.com/design/adaptive-music-across-biomes) — Adaptive music across biomes
- [Godot AudioStreamInteractive](https://docs.godotengine.org/en/stable/classes/class_audiostreaminteractive.html) — Official AudioStreamInteractive docs

**Build:** 4 biome tracks that share musical motifs (same key, related melodies). Seamless transitions at biome borders. Dynamic intensity (calm exploration → combat). Reuse and remix previous block music into cohesive soundtrack.

**Level Design: Interconnected Biome Layout**

**Read:** [Metroidvania Interconnection Design](https://www.gamedeveloper.com/design/metroidvania-interconnection-design) — Interconnection design principles

**Build:** Biomes connected by passages requiring abilities: Forest → Cave (need dash), Cave → Tower (need double jump), Tower → Underwater (need spell), Underwater → Final Dungeon (need all abilities). Shortcuts unlock as player gains powers. Secret paths between biomes.

**UI: World Map + Fast Travel**

**Read:**
- [Godot SubViewport Minimap](https://docs.godotengine.org/en/stable/tutorials/rendering/viewports.html) — Godot viewport tutorial
- [Godot ScrollContainer](https://docs.godotengine.org/en/stable/classes/class_scrollcontainer.html) — Official ScrollContainer docs

**Build:** Full world map (discovered areas only, fog of war for undiscovered). Fast travel to unlocked checkpoints (shrines). Biome color-coding on map. Objective markers. Legend for map icons.

---

## DAWN 58: Systems Integration — Inventory, Shop, Dialogue, Quests

**Game Mechanic:** All Support Systems Combined

**Origin:** Hollow Knight (charms, geo, nail upgrades). Stardew Valley (comprehensive systems).

**Read:**
- [Godot Inventory System Complete Guide](https://www.strayspark.studio/blog/godot-4-inventory-crafting-system-complete-guide) — Complete inventory system guide
- [Godot Dialogue and Quest Systems](https://www.strayspark.studio/blog/godot-4-dialogue-quest-systems-signals-resources) — Dialogue and quest system guide
- [Godot Shop System](https://forum.godotengine.org/t/suggestions-on-how-to-make-a-shop-system-using-resources/79416) — Shop system using resources

**Build:** Inventory system (grid-based, drag-drop, from Block 6). Shop system (buy/sell, currency, from Block 6). Dialogue system (typing effect, branching, portraits, from Block 12 research). Quest system (objectives, rewards, tracking, from Block 12 research). All systems connected via `EventBus` autoload.

**Art: Full UI Kit**

**Read:**
- [Metroidvania UI Design](https://www.slynyrd.com) — Slynyrd pixel art tutorials
- [Consistent UI Style](https://www.gamedeveloper.com/design/consistent-ui-style) — UI style consistency

**Build:** Complete UI kit: inventory screen, shop screen, dialogue box (with portrait + text + choices), quest log, settings menu, pause menu, title screen, game over, victory. All using consistent 16×16 pixel art style. Button states: normal, hover, pressed, disabled.

**Music: UI SFX + Shop Music**

**Read:**
- [UI Audio Design](https://www.gamedeveloper.com/design/ui-audio-design) — UI audio design
- [Shop Music Composition](https://www.gamedeveloper.com/design/shop-music-composition) — Shop music composition

**Build:** Complete UI SFX library: hover (soft tick), click (confirm), back (cancel), error (buzz), equip (shing), sell (coin clink), buy (coin clink + chime). Shop music (calm, inviting, 1-minute loop). Dialogue SFX (text blip per character, pitch varies by speaker). Quest complete fanfare.

**Level Design: Town/NPC Hub**

**Read:** [Hub Area Design](https://www.gamedeveloper.com/design/hub-area-design) — Hub area design

**Build:** Town area with 4 NPCs: Shopkeeper (sells items/upgrades), Quest Giver (main + side quests), Blacksmith (upgrades weapon damage), Healer (restores health). Each NPC has dialogue + function. Shop has rotating stock.

**UI: Full System UIs**

**Read:**
- [Godot TabContainer](https://docs.godotengine.org/en/stable/classes/class_tabcontainer.html) — Official TabContainer docs
- [Godot ItemList](https://docs.godotengine.org/en/stable/classes/class_itemlist.html) — Official ItemList docs
- [Godot RichTextLabel](https://docs.godotengine.org/en/stable/classes/class_richtextlabel.html) — Official RichTextLabel docs

**Build:** Inventory (grid + equipment slots + tooltip). Shop (buy/sell tabs, item list, price display, currency). Dialogue (portrait left, text right, choice buttons bottom). Quest log (active/completed tabs, objectives, rewards). All with keyboard + gamepad navigation support.

---

## DAWN 59: Boss Rush + Final Polish

**Game Mechanic:** Boss Rush + Final Game Polish

**Origin:** Hollow Knight Godmaster DLC (boss rush). Cuphead (boss-focused game).

**Read:**
- [Boss Rush Design](https://www.gamedeveloper.com/design/boss-rush-design) — Boss rush design
- [Game Polish Techniques](https://www.gamedeveloper.com/design/game-polish-techniques) — Game polish techniques
- [Godot Performance Optimization](https://docs.godotengine.org/en/stable/tutorials/performance/index.html) — Official Godot performance docs

**Build:** Boss rush mode (fight all 5 previous mini-bosses in sequence, no healing between). Final boss (uses ALL mechanics: 4 phases, bullet patterns, enemy summons, environment changes). Hit pause (freeze 3 frames on impact). Screen shake (trauma-based). Particle effects on every significant action. Shader effects: flash white on hit, dissolve on death, glow for magic.

**Art: Final Boss + Polish Effects**

**Read:**
- [Final Boss Design](https://www.gamedeveloper.com/design/final-boss-design) — Final boss design
- [Game Juice Visual Effects](https://www.slynyrd.com) — Slynyrd pixel art tutorials

**Build:** Final boss (128×128, 4 phases, transforms between phases). All boss sprites from previous blocks refined and unified. Hit effects (sparks, blood splatter, white flash). Death dissolve shader (pixel-by-pixel fade). Glow effects for magic abilities. Screen-wide effects (darken during boss phase 3, vignette during low health).

**Music: Final Boss Theme + Soundtrack Assembly**

**Read:**
- [Final Boss Music Composition](https://www.gamedeveloper.com/design/final-boss-music-composition) — Final boss music composition
- [Game Soundtrack Integration](https://www.gamedeveloper.com/design/game-soundtrack-integration) — Soundtrack integration

**Build:** Epic final boss theme (5+ minutes, 4 phases matching boss phases). Assemble all previous music into cohesive soundtrack (15+ tracks total). Main theme (title screen). 4 biome ambient tracks. 4 combat tracks. 5 boss tracks. Victory theme. Game over theme. Credits music.

**Level Design: Final Dungeon + Boss Arena**

**Read:**
- [Final Dungeon Design](https://www.gamedeveloper.com/design/final-dungeon-design) — Final dungeon design
- [Climax Level Design](https://www.gamedeveloper.com/design/climax-level-design) — Climax level design

**Build:** Final dungeon (hardest area, combines all mechanics: platforming, combat, spells, swimming). Final boss arena (multi-phase, environment changes: platforms break, lava rises, gravity shifts). Victory room (credits, reward chest, completion stats). Secret ending conditions (100% completion).

**UI: Final Polish + Settings Menu**

**Read:**
- [Godot Settings Menu](https://docs.godotengine.org/en/stable/tutorials/ui/index.html) — Official Godot UI docs
- [Godot Project Settings](https://docs.godotengine.org/en/stable/tutorials/editor/project_settings.html) — Official Godot project settings docs

**Build:** Settings menu: Graphics (resolution, fullscreen, VSync, pixel scale), Audio (master, music, SFX, UI volume sliders), Controls (rebindable keys, gamepad support), Accessibility (colorblind mode, text size, screenshake intensity). Final UI polish: smooth animations, screen transitions (fade), consistent spacing.

---

## DAWN 60: Export, Optimization & Devlog

**Game Mechanic:** Final Integration + Export

**Origin:** Indie game release best practices. Godot export pipeline.

**Read:**
- [Godot Exporting Basics](https://docs.godotengine.org/en/stable/tutorials/export/exporting_basics.html) — Official Godot exporting docs
- [Godot Performance Optimization](https://docs.godotengine.org/en/stable/tutorials/performance/index.html) — Official Godot performance docs
- [Godot Web Export](https://docs.godotengine.org/en/stable/tutorials/export/exporting_for_web.html) — Official Godot web export docs

**Build:** Final integration of ALL systems. Bug fixing pass. Performance optimization: object pooling (bullets, enemies, particles), off-screen culling (`set_process(false)`), texture atlases (`AtlasTexture`), draw call batching. Export to: Windows (.exe), Web (HTML5/WASM), Linux. Build pipeline setup. `.gitignore` for `.godot/`, `*.tmp`, `*.import`.

**Art: Final Asset Cleanup + Atlas**

**Read:**
- [Texture Atlas Creation](https://docs.godotengine.org/en/stable/classes/class_atlastexture.html) — Official AtlasTexture docs
- [Godot Import Pipeline](https://docs.godotengine.org/en/stable/tutorials/assets_pipeline/import_process.html) — Official Godot import pipeline docs

**Build:** Texture atlas for all sprites (reduces draw calls from 200+ to ~20). Final sprite cleanup (remove unused frames, consistent sizing). Consistent palette across all assets (final pass). Title screen art (hero standing before dark castle). Credits background art.

**Music: Final Mix + Master**

**Read:**
- [Game Audio Mixing](https://www.gamedeveloper.com/design/game-audio-mixing) — Game audio mixing
- [Godot Audio Buses](https://docs.godotengine.org/en/stable/tutorials/audio/audio_buses.html) — Official Godot audio bus docs

**Build:** Final mix of all tracks (consistent levels, no clipping). Master bus processing (limiter, gentle EQ). Audio bus organization: Music, SFX, UI, Ambient. Volume presets (Low, Medium, High). Loop point verification for all music tracks.

**Level Design: Complete Game World**

**Read:** [Final Game World Assembly](https://www.gamedeveloper.com/design/final-game-world-assembly) — Final world assembly

**Build:** Complete interconnected world: Hub → 4 Biomes → Final Dungeon → Boss Arena. All shortcuts unlocked. All secrets placed (20+ hidden rooms). Difficulty balanced (playtest data). Estimated playtime: 30-45 minutes for full completion.

**UI: Complete Game UI**

**Read:** [Godot UI Final Polish](https://docs.godotengine.org/en/stable/tutorials/ui/index.html) — Official Godot UI docs

**Build:** Title screen (animated logo, press start). Main menu (continue, new game, settings, credits, quit). In-game HUD (health, mana, mini-map, abilities, currency). Pause menu (resume, settings, save, quit). Game over (retry, load last save, quit). Victory screen (completion time, % discovered, death count). Credits roll (scrollable, skippable).

**BLOCK 12 DELIVERABLE:** Complete Metroidvania vertical slice. Full game with all systems. Exportable builds for Windows + Web. Complete devlog. Master catalog of ALL reusable pieces.

---

# 🎯 THE MASTER CATALOG SYSTEM

After Dawn 60, create these catalog files in your project root:

```
📁 MasterCatalog/
├── mechanics_catalog.md      # Every mechanic → which block → how to reuse
├── art_catalog.md            # Every sprite/animation → dimensions → style → reuse
├── music_catalog.md          # Every track → BPM → mood → loop points → reuse
├── UI_catalog.md             # Every UI component → scene path → how to instance
├── level_patterns.md         # Every layout pattern → screenshot → when to use
└── sfx_catalog.md            # Every SFX → category → trigger → reuse
```

## mechanics_catalog.md (Excerpt)

```markdown
# Mechanics Catalog

## Movement
| Mechanic | Block | File | Reuse Notes |
|----------|-------|------|-------------|
| Auto-runner | Block 1 | player_runner.gd | Base for any runner game |
| Platformer (coyote, buffer) | Block 1 | player_platformer.gd | Standard platformer controller |
| Wall jump/slide | Block 2 | player_wall.gd | Add to platformer for advanced movement |
| Dash | Block 2 | player_dash.gd | Reusable dash with i-frames |
| Top-down 8-dir | Block 5 | player_topdown.gd | Base for RPG/top-down games |
| Underwater swim | Block 7 | player_swim.gd | Buoyancy + drag system |
| Space shooter | Block 9 | player_ship.gd | 8-direction + shooting |

## Combat
| Mechanic | Block | File | Reuse Notes |
|----------|-------|------|-------------|
| Hitbox/Hurtbox | Block 2 | combat_hitbox.gd | Base for all combat |
| Combo system | Block 2 | combat_combo.gd | Light/heavy attack chains |
| i-Frames | Block 2 | combat_iframes.gd | Flash + invincibility |
| Knockback | Block 2 | combat_knockback.gd | Vector-based pushback |
| Projectile system | Block 3 | combat_projectile.gd | Fireball + physics arc |
| Spell combining | Block 3 | combat_spells.gd | Element mixing system |
| Status effects | Block 3 | combat_status.gd | Burn/freeze/poison |
| Bullet patterns | Block 9 | combat_bullets.gd | Spiral, spread, aimed |
| Boss phases | Block 9 | combat_boss.gd | AnimationTree state machine |

## Systems
| Mechanic | Block | File | Reuse Notes |
|----------|-------|------|-------------|
| State machine | Block 2 | system_statemachine.gd | Enum + Node-based |
| Object pooling | Block 3 | system_pool.gd | Bullets, enemies, particles |
| A* Pathfinding | Block 2/11 | system_pathfinding.gd | NavigationAgent2D wrapper |
| Save/Load JSON | Block 3 | system_save.gd | FileAccess + JSON |
| Inventory grid | Block 6 | system_inventory.gd | Drag-drop + stacking |
| Crafting | Block 6 | system_crafting.gd | Recipe database |
| Shop | Block 6/12 | system_shop.gd | Buy/sell + currency |
| Dialogue | Block 12 | system_dialogue.gd | Typing + branching |
| Quest | Block 12 | system_quest.gd | Objectives + rewards |
| Wave manager | Block 9/11 | system_waves.gd | Timer-based spawning |
| Procedural gen | Block 10 | system_procgen.gd | FastNoiseLite wrapper |
| Fog of war | Block 10 | system_fog.gd | Viewport + shader |
| Fast travel | Block 10 | system_fasttravel.gd | Landmark + map system |

## AI
| Mechanic | Block | File | Reuse Notes |
|----------|-------|------|-------------|
| Patrol | Block 2 | ai_patrol.gd | Waypoint loop |
| Chase | Block 2 | ai_chase.gd | Vision + pathfinding |
| Vision cone | Block 4 | ai_vision.gd | RayCast2D array |
| Stealth AI | Block 4 | ai_stealth.gd | Alert states + search |
| Flocking | Block 7 | ai_flock.gd | Group movement |
| Racing AI | Block 5 | ai_racer.gd | Rubber banding |
| TD enemy path | Block 11 | ai_tdpath.gd | Path2D follower |

## Effects
| Mechanic | Block | File | Reuse Notes |
|----------|-------|------|-------------|
| Screen shake | Block 2/9 | fx_shake.gd | Trauma-based + noise |
| Particles (CPU) | Block 3 | fx_particles_cpu.gd | Burst effects |
| Particles (GPU) | Block 3 | fx_particles_gpu.gd | Complex effects |
| Hit flash | Block 2 | fx_hitflash.gd | White flash shader |
| Dissolve | Block 9 | fx_dissolve.gd | Death effect shader |
| Trail | Block 2 | fx_trail.gd | Line2D follow |
| Camera rooms | Block 2 | fx_cameraroom.gd | Boundary-based camera |

| Footstep surfaces | Block 4 | audio_footsteps.gd | Surface-based SFX (grass/stone/water/metal) |

## SFX (sfx_catalog.md)
| SFX | Category | Trigger | Block | Reuse Notes |
|-----|----------|---------|-------|-------------|
| Jump | Movement | On jump input | Block 1 | Pitch-varied per character weight |
| Coin/pickup | Feedback | On item collect | Block 1 | Reused across all collectible types |
| Hit/impact | Combat | On hitbox connect | Block 2 | Layered with hit-flash shader |
| UI select/confirm | UI | On menu navigation | Block 1 | Shared across all menus |
| Spell cast | Magic | On ability use | Block 3 | Pitch shifts with spell tier |
| Alarm/detected | Stealth | On AI spotting player | Block 4 | Stinger + music layer swap |
| Engine rev | Vehicle | On throttle input | Block 5 | Pitch tied to speed value |
| Craft complete | Systems | On recipe finish | Block 6 | Shared with build-placement SFX |
| Bubble/dive | Environment | Underwater movement | Block 7 | Randomized pitch per bubble |
| Door/switch | Puzzle | On interact | Block 8 | Reused for all puzzle elements |
| Explosion | Combat/FX | On death/boss hit | Block 9 | Paired with screen shake |
| Discovery chime | Exploration | On map reveal | Block 10 | Layered with fog-of-war shader |
| Tower fire | TD | On tower attack | Block 11 | Variant per tower type |
| Wave alert | TD | On wave start | Block 11 | Shared stinger with boss intro |

---

**MASTER CATALOG COMPLETE.** By Dawn 60 you should have 6 populated catalog files covering every mechanic, sprite, track, UI component, level pattern, and SFX built across all 12 blocks — a full reusable toolkit for any future Godot project.

## What You've Built

60 dawns. 12 vertical slices. One finale that combines them all. You didn't follow a single tutorial — you read docs, made decisions, and debugged your own mistakes. That's the actual skill. The catalogs are your proof of work and your toolbox going forward: next project, you're not starting from zero, you're snapping together Lego pieces you already built and understand.