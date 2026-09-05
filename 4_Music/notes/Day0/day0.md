# 🎵 Music Day 1 — Tools & DAW Setup

**DAW -  Digital Audio Workstation / Main Music Production:**

* **LMMS** — Free, open-source, cross-platform

  * Windows
  * macOS
  * Linux
  * Piano Roll
  * Beat/Bassline Editor
  * MIDI
  * Synthesizers
  * Samples
  * Effects
  * Arrangement
  * Mixing
* Download from [LMMS official website](https://lmms.io/?utm_source=chatgpt.com)
* Use this as the **main DAW for the zero-cost workflow**. LMMS is explicitly free and open source. ([LMMS][2])

**Audio Editing / SFX:**

* **Audacity** — Free, open-source audio editor

  * Record
  * Cut
  * Trim
  * Normalize
  * Noise reduction
  * Pitch changing
  * Speed changing
  * Compression
  * Basic effects
  * WAV / OGG / MP3 / FLAC export
* Download from [Audacity official website](https://www.audacityteam.org/?utm_source=chatgpt.com)
* Use mainly for:

  * SFX editing
  * Foley editing
  * Cleaning recordings
  * Cutting samples
  * Quick audio manipulation
* Audacity is free and open source. ([Audacity][3])

---

## 🎹 Plugins / Instruments

**Do not start by downloading 200 VSTs.**

Start with the instruments and effects already included with your DAW.

Learn these categories first:

* Piano
* Synth
* Bass
* Pad
* Pluck
* Strings
* Percussion
* Drum kit
* Reverb
* Delay
* EQ
* Compressor
* Limiter

**Optional later:**

* Free synthesizer plugins
* Free sampled instruments
* Free drum libraries
* Free effects

The important thing is understanding **why a sound is being used**, not accumulating plugins like digital Pokémon.

---

## 🎼 MIDI

**MIDI:**

* MIDI is **performance/control data**, not recorded audio
* Contains information such as:

  * Which note
  * When the note starts
  * How long it lasts
  * Velocity
  * Pitch
  * Controller information
* MIDI can control:

  * Piano
  * Synth
  * Bass
  * Strings
  * Drums
  * Other virtual instruments

**Remember:**

```text
MIDI
  ↓
Instrument
  ↓
Audio
```

Example:

```text
C4 → MIDI note
     ↓
Piano instrument
     ↓
Actual piano sound
```

---

## 🔊 Audio

**Audio:**

* Actual recorded/generated sound
* Examples:

  * Piano recording
  * Explosion
  * Footstep
  * Voice
  * Synthesized bass
  * Music track

**Basic formats:**

* **WAV**

  * Uncompressed production audio
  * Large file
  * Good for editing/master files
* **OGG**

  * Compressed
  * Smaller file
  * Useful for game assets
* **MP3**

  * Compressed
  * General-purpose distribution
  * Avoid using it as your main production format

---

## 📁 Music Project Structure

Create:

```text
GameAudio/
├── Projects/
│   └── Music/
├── MIDI/
├── Samples/
├── SFX/
├── Instruments/
├── Exports/
│   ├── WAV/
│   └── OGG/
└── References/
```

**File Naming:**

```text
menu_theme_v01
menu_theme_v02
menu_theme_v03
battle_theme_v01
player_jump_v01
player_jump_v02
```

**Avoid:**

```text
final.wav
final2.wav
final_final.wav
final_final_REAL.wav
final_final_REAL_2.wav
```

Versioning is cheaper than archaeology.

---

## 🎛️ DAW Interface

Learn these areas first:

**Arrangement / Playlist:**

* Arrange music over time
* Intro
* Sections
* Loops
* Transitions
* Ending

**Piano Roll:**

* Place notes
* Move notes
* Resize notes
* Delete notes
* Change velocity
* Quantize
* Create melodies
* Create chords

**Mixer:**

* Volume
* Pan
* Mute
* Solo
* Metering
* Effects

**Browser:**

* Instruments
* Samples
* Presets
* Plugins

**Transport:**

* Play
* Stop
* Record
* Loop
* BPM
* Metronome
* Time signature

---

## ⏱️ Tempo

**BPM = Beats Per Minute**

* Slow → ~60–80 BPM
* Moderate → ~80–120 BPM
* Fast → ~120–160 BPM
* Very fast → 160+ BPM

For Day 1:

```text
BPM: 100
Time Signature: 4/4
Length: 4 bars
```

Change the BPM after creating the loop.

Listen to how the **same musical material** changes when the tempo changes.

---

## 🥁 First Rhythm

Start with **4/4**.

Count:

```text
1   2   3   4
```

Basic exercise:

```text
Kick:     X   X   X   X
Hi-hat:   - X - X - X - X
```

Then experiment.

Do not worry about:

* Polyrhythms
* Odd meters
* Complex drum programming
* Swing theory
* Advanced percussion

Those are future problems.

---

## 🎹 First Music Exercise

Create a **4-bar loop**.

Use:

* 1 drum instrument
* 1 bass instrument
* 1 melodic instrument

Structure:

```text
Drums
   ↓
Bass
   ↓
Chords / Harmony
   ↓
Melody
```

Keep it extremely simple.

The objective is **not to make a good song**.

The objective is to understand:

```text
Idea
 ↓
MIDI
 ↓
Instrument
 ↓
Audio
 ↓
Mixer
 ↓
Export
```

---

## 🎧 Listening

While making the loop, continuously switch between:

```text
ZOOM IN
   ↓
Inspect individual notes
   ↓
ZOOM OUT
   ↓
Listen to the musical phrase
```

Similar principle to your pixel-art workflow.

**Don't only inspect the waveform or piano roll.**

Listen to the result.

---

## 🎯 Day 1 Goal

By the end of Day 1 you should be able to:

* Open your DAW
* Understand the basic interface
* Create an instrument track
* Open the Piano Roll
* Create MIDI notes
* Create a basic drum pattern
* Create a simple melody
* Change BPM
* Change volume
* Pan a track
* Use mute/solo
* Export WAV
* Find the exported file
* Re-import the WAV
* Explain the difference between MIDI and audio

### Day 1 output

```text
01_Day1_TestLoop/
├── day1_test_loop.lmms
├── day1_test_loop.wav
└── notes.txt
```

**Do not move to Day 2 until you can explain the entire signal path yourself:**

```text
MIDI
 ↓
Virtual Instrument
 ↓
Audio
 ↓
Track
 ↓
Mixer
 ↓
Master
 ↓
WAV Export
```

That chain is the foundation. Everything more complicated in game audio eventually becomes a more elaborate version of this.

[1]: https://www.reaper.fm/download.php?utm_source=chatgpt.com "REAPER | Download"
[2]: https://lmms.io/download?utm_source=chatgpt.com "LMMS | Download"
[3]: https://www.audacityteam.org/download/?utm_source=chatgpt.com "Audacity ® | Downloads"
