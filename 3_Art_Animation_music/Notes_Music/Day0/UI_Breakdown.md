# 🎵 Music Day 1 — LMMS UI

Your screen can be divided into **5 important areas**:

```text
┌──────────────────────────────────────────────────────────────┐
│  1. TOOLBAR / TRANSPORT                                     │
├──────────────────────────────────────────────────────────────┤
│  2. SONG EDITOR / ARRANGEMENT                                │
│                                                              │
├──────────────────────────────────────────────────────────────┤
│  3. FX-MIXER                     │ 4. CONTROLLER RACK       │
│                                  │                          │
├──────────────────────────────────────────────────────────────┤
│  5. LEFT SIDEBAR / TOOL WINDOWS                             │
└──────────────────────────────────────────────────────────────┘
```

## 1. 🎛️ Top Toolbar

This is your **control center**.

* **Play ▶** → plays the project
* **Stop ■** → stops playback
* **Tempo/BPM** → controls playback speed
* **Time Signature** → currently `4/4`
* **Metronome** → keeps a timing reference
* **CPU meter** → shows how much processing LMMS is using
* **Save** → saves your project

### Remember

```text
BPM → How fast
4/4 → How beats are grouped
Play/Stop → Transport
CPU → Processing load
```

---

# 2. 🎼 Song Editor

This is the **main arrangement/timeline**.

You currently have:

* TripleOscillator
* Sample Track
* Beat/Bassline
* Automation Track

The numbers across the top:

```text
1   5   9   13   17   21 ...
```

represent **bars/measures**.

This is where you'll eventually build:

```text
Intro → A → B → A → Ending
```

### Important distinction

**Song Editor = arrangement**

It answers:

> "When does each musical element happen?"

---

# 3. 🎚️ FX-Mixer

Your mixer is currently at the bottom.

It handles the **audio signal after an instrument generates sound**.

You will use it for:

* Volume
* Panning
* Effects
* Balancing instruments
* Routing

Think:

```text
Instrument
    ↓
Audio
    ↓
Mixer
    ↓
Master
```

Your **Master** channel is visible on the left.

### Don't touch advanced effects yet.

For Day 1, understand:

**Volume + Pan + Master**

That's enough.

---

# 4. 🎮 Controller Rack

The panel on the right.

It allows controllers/automation to control parameters.

For example:

```text
Controller
    ↓
Volume
    ↓
Volume changes over time
```

Later this becomes useful for:

* Volume automation
* Filter sweeps
* Pitch changes
* Effects
* Dynamic music

**For now: ignore it.**

---

# 5. 🧰 Left Sidebar

The vertical icons on the far left open different LMMS tools/resources.

You'll eventually use these for things like:

* Instruments
* Samples
* Presets
* Project resources
* Plugins

The important concept is:

```text
Sidebar
   ↓
Find/create musical resources
   ↓
Put them into Song Editor
```

---

# 🧠 The Most Important Mental Model

Don't memorize the UI individually.

Understand the **flow**:

```text
             YOU
              ↓
       ┌─────────────┐
       │ Song Editor │
       └──────┬──────┘
              ↓
       ┌─────────────┐
       │ Instrument  │
       └──────┬──────┘
              ↓
            AUDIO
              ↓
       ┌─────────────┐
       │    Mixer    │
       └──────┬──────┘
              ↓
           MASTER
              ↓
          EXPORT
              ↓
            WAV
```

And separately:

```text
Piano Roll
    ↓
MIDI notes
    ↓
Instrument
    ↓
Audio
```

### Your Day 1 UI checklist

Learn only these today:

* [ ] Play / Stop
* [ ] BPM
* [ ] Time Signature
* [ ] Song Editor
* [ ] Piano Roll
* [ ] Instrument
* [ ] Mixer
* [ ] Volume
* [ ] Pan
* [ ] Master
* [ ] Save project

**Don't touch the Controller Rack, automation, advanced effects, or mastering yet.** We're building understanding first, then complexity.
