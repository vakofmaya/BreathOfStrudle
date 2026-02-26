# 🌬️ Breath of Strudel — Beginner Cheat Sheet

> **Everything you need memorized to start performing live.**
> Open [strudel.cc](https://strudel.cc/), keep this sheet visible, and go.

---

## ⌨️ Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+Enter` | **Evaluate** all code (play / update) |
| `Ctrl+.` | **Hush** — stop all sound NOW |

> **No block evaluation.** Prefix `_$:` to mute a single pattern.

---

## 🎚️ Tempo Setup

```js
setcpm(128/4)   // 128 BPM, 4 beats/cycle, 1 cycle = 1 bar
```

| BPM | Code |
|-----|------|
| 120 | `setcpm(120/4)` |
| **128** | **`setcpm(128/4)`** |
| 130 | `setcpm(130/4)` |
| 140 | `setcpm(140/4)` |

---

## ✏️ Mini-Notation — The Pattern Language

| Syntax | Name | What It Does | Example |
|--------|------|-------------|---------|
| `a b c` | **Sequence** | Divide cycle equally | `s("bd sd hh cp")` |
| `~` | **Rest** | Silence | `s("bd ~ sd ~")` |
| `[a b]` | **Sub-group** | Subdivide a slot | `s("bd [sd sd] hh cp")` |
| `a*N` | **Multiply** | Repeat N× (faster) | `s("hh*8")` |
| `<a b c>` | **Slow seq** | One per cycle, rotate | `note("<c2 eb2 f2>")` |
| `a,b` | **Parallel** | Play simultaneously | `s("bd*4, hh*8")` |
| `a:N` | **Select** | Pick Nth sample | `s("hh:2")` |
| `a!N` | **Replicate** | Repeat N× (same speed) | `s("bd!4")` |
| `a(N,M)` | **Euclidean** | N beats over M steps | `s("bd(3,8)")` |
| `a?` | **Degrade** | 50% chance of silence | `s("hh*8?")` |
| `a\|b` | **Random pick** | Choose one randomly | `s("bd\|sd")` |
| `[a b]/N` | **Divide** | Play over N cycles | `note("[c d e f]/4")` |
| `a@N` | **Elongate** | Give N units weight | `s("bd@2 sd")` |

---

## 🔊 Sound Sources

### Triggering Sounds

| Code | What It Does |
|------|-------------|
| `s("bd")` | Play a sample by name |
| `note("c3")` | Play a note (default: triangle) |
| `note("c3").s("sawtooth")` | Play a note with a specific waveform |
| `n("0 2 4").scale("C3:minor")` | Play scale degrees |

### The 4 Waveforms (ordered by use)

| Waveform | Code | Character | Best For |
|----------|------|-----------|----------|
| **Sawtooth** | `.s("sawtooth")` | Buzzy, bright, rich | Bass, pads, leads |
| **Sine** | `.s("sine")` | Pure, clean | Sub-bass, kicks |
| **Triangle** | `.s("triangle")` | Soft, mellow (default) | Leads, melodies |
| **Square** | `.s("square")` | Hollow, nasal | Stabs, retro |

### Noise Sources

| Type | Code | Use |
|------|------|-----|
| White | `s("white")` | Hi-hats |
| Pink | `s("pink")` | Snare body |
| Brown | `s("brown")` | Rumble, texture |
| Crackle | `s("crackle")` | Vinyl texture |

---

## 🥁 Drum Samples & Banks

### Essential Drum Hits

| Name | Sound |
|------|-------|
| `bd` | Bass drum / kick |
| `sd` | Snare |
| `hh` | Closed hi-hat |
| `oh` | Open hi-hat |
| `cp` | Clap |
| `rim` | Rimshot |
| `cr` | Crash |

### Drum Machine Banks

| Bank | Character | Code |
|------|-----------|------|
| **RolandTR909** | Punchy, standard techno | `.bank("RolandTR909")` |
| **RolandTR808** | Boomy, deep | `.bank("RolandTR808")` |
| **RolandCR78** | Vintage, lo-fi | `.bank("RolandCR78")` |
| **LinnDrum** | Clean, 80s | `.bank("LinnDrum")` |

### Sample Variants & Cut Groups

| Code | What It Does |
|------|-------------|
| `s("hh:0 hh:1 hh:2")` | Select variants (0-indexed) |
| `.n("0 1 2 3")` | Same thing as `:N` |
| `.cut(1)` | Cut group — `oh` gets muted by `hh` |

---

## 🎛️ Filters (Most Used)

| Filter | Code | Removes |
|--------|------|---------|
| **Low-pass** | `.lpf(400)` | Highs → darker |
| **High-pass** | `.hpf(6000)` | Lows → thinner |
| **Band-pass** | `.bpf(2000)` | Highs & lows → telephone |
| **Vowel** | `.vowel("a")` | Formant shaping |

### Resonance & Filter Types

| Code | What It Does |
|------|-------------|
| `.lpq(12)` | Resonance boost at cutoff (0–30+) |
| `.ftype("ladder")` | Aggressive Moog-style (acid bass) |
| `.ftype("24db")` | Steep cutoff |
| `.ftype("12db")` | Gentle, subtle |

---

## 📈 Envelopes — Shaping Sound Over Time

### Amplitude Envelope (ADSR)

| Parameter | Code | What It Controls |
|-----------|------|-----------------|
| Attack | `.attack(0.01)` | Rise time (0 = instant) |
| Decay | `.decay(0.2)` | Fall to sustain level |
| Sustain | `.sustain(0)` | Hold level (0–1) |
| Release | `.release(0.1)` | Fade after note off |

**Shorthand:** `.adsr("0.01:0.2:0:0.1")`

### Quick Envelope Recipes

| Sound Type | Code |
|-----------|------|
| **Percussive pluck** | `.decay(0.15).sustain(0)` |
| **Pad / string** | `.attack(0.5).sustain(0.7).release(1.5)` |
| **Hi-hat / click** | `.decay(0.03).sustain(0)` |

### Pitch Envelope (Kick Design!)

| Parameter | Code | What It Does |
|-----------|------|-------------|
| Depth | `.penv(24)` | Semitones of pitch sweep |
| Decay | `.pdecay(0.06)` | Speed of pitch drop |

**The synthesized kick:**
```js
note("c1!4").s("sine").penv(24).pdecay(.06).decay(.4).sustain(0)
```

---

## 🔉 Volume & Dynamics

| Code | What It Does |
|------|-------------|
| `.gain(0.8)` | Volume per event (default ~0.8) |
| `.gain(".5 .3 .8 .3")` | Accent pattern |
| `.velocity(0.5)` | Multiplied with gain |

### Gain Structure (Mix Reference)

| Element | Gain |
|---------|------|
| Kick | 0.80–0.90 |
| Clap | 0.60–0.70 |
| Bass | 0.50–0.60 |
| Hi-hats | 0.30–0.50 |
| Pad | 0.20–0.30 |
| Lead | 0.20–0.30 |
| Texture | 0.05–0.10 |

---

## 🌊 Essential Effects

| Effect | Code | Key Params |
|--------|------|-----------|
| **Delay** | `.delay(0.5)` | `.delaytime(0.1875)`, `.delayfeedback(0.5)` |
| **Reverb** | `.room(0.4)` | `.roomsize(0.7)`, `.roomlp(3000)` |
| **Distortion** | `.distort("1.5:.6")` | `amount:asymmetry` |
| **Pan** | `.pan(0.5)` | 0=left, 0.5=center, 1=right |

### Delay Times at 128 BPM

| Subdivision | `.delaytime()` |
|-------------|----------------|
| Dotted eighth (most used) | `0.3515625` or `3/16` ≈ `0.1875` |
| Eighth note | `0.234375` |
| Sixteenth | `0.1171875` |
| Quarter | `0.46875` |

> **Tip:** `.delaytime(0.1875)` is the go-to techno delay time.

---

## 🎹 Chords & Scales

### Quick Chord Progressions

```js
chord("<Cm Fm Ab Bb>").voicing().s("sawtooth").lpf(2000)
```

| Progression | Code | Mood |
|-------------|------|------|
| Dark minor | `<Cm Bb Ab G>` | Classic techno |
| Deep | `<Cm Fm Bb Eb>` | Deep house/techno |
| Minimal | `<Cm Fm>` | Two-chord loop |

### Essential Scales

| Scale | Code | When |
|-------|------|------|
| **Minor Pentatonic** | `"C:minor:pentatonic"` | Melodies — can't go wrong |
| **Minor** | `"C:minor"` | Dark techno standard |
| **Dorian** | `"C:dorian"` | Groovier, warmer |
| **Phrygian** | `"C:phrygian"` | Eastern, tense |

---

## 🏗️ Pattern Structure — `$:` System

```js
setcpm(128/4)
$: s("bd*4").bank("RolandTR909")           // Kick
$: s("hh*16").bank("RolandTR909").gain(.4) // Hi-hats
$: s("~ cp ~ cp").bank("RolandTR909")      // Clap
$: note("c2 ~ <eb2 f2> ~").s("sine")      // Bass
```

| Technique | Code |
|-----------|------|
| Independent patterns | `$: ...` per line |
| Mute a pattern | `_$: ...` or `// $: ...` |
| Named patterns | `$kick: ...`, `$bass: ...` |
| Stack (parallel) | `stack(patternA, patternB)` |
| Sequence (one/cycle) | `cat(patternA, patternB)` |
| Stop everything | `hush` or `Ctrl+.` |

---

## 🎤 Live Performance Quick-Ref

### The Workflow

```
1. Start kick → wait 4–8 bars
2. Add hats → wait
3. Add clap → wait
4. Add bass → wait
5. Add pad → wait
6. MODIFY (filter sweeps, new notes, add .sometimes())
7. BREAKDOWN: comment out kick + bass
8. DROP: bring back kick + bass + extras
9. OUTRO: comment out bottom→top, fade kick
10. hush
```

### Common Euclidean Rhythms

| Pattern | Feel |
|---------|------|
| `(3,8)` | Cuban tresillo |
| `(5,8)` | Cinquillo |
| `(5,16)` | Good for rimshots |
| `(7,16)` | Complex shuffle |

---

## 🧱 Sound Design Recipes — Copy & Paste

### Kick
```js
$: note("c1!4").s("sine").penv(24).pdecay(.06).decay(.4).sustain(0).gain(0.85)
```

### Hi-Hats (16ths with accents)
```js
$: s("hh*16").bank("RolandTR909").gain(".5 .3 .8 .3").hpf(6000)
```

### Clap
```js
$: s("~ cp ~ cp").bank("RolandTR909").room(0.15).gain(0.65)
```

### Acid Bass
```js
$: note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
  .lpf(400).lpq(12).lpenv(4).lpd(.12).lps(0)
  .ftype("ladder").decay(.2).sustain(0).gain(0.55)
```

### Dark Pad
```js
$: note("<[c4,eb4,g4] [bb3,d4,f4]>").s("sawtooth")
  .lpf(2000).attack(.3).release(1).room(.4).gain(0.25).orbit(2)
```

### Sparse Lead
```js
$: n("<0 ~ 3 ~ 5 ~ 7 ~>*2").scale("C4:minor:pentatonic").s("triangle")
  .decay(.15).sustain(0).delay(.4).delaytime(.1875).delayfeedback(.5).gain(0.2)
```

### Sidechain Duck (put pad on orbit 2)
```js
$: s("bd*4").bank("RolandTR909").duckorbit(2).duckattack(.2).duckdepth(1).gain(0)
```

---

## 🚨 Common Mistakes

| Mistake | Fix |
|---------|-----|
| No sound playing | Press `Ctrl+Enter` to evaluate |
| Two patterns without `$:` | Only last one plays — add `$:` |
| `Bd` instead of `bd` | Case-sensitive — use lowercase |
| Missing closing `"` | Always close strings |
| Audio won't start | Click Play button first |
| Filter on sine = no change | Sine has no harmonics — use sawtooth |

---

*Keep this open. Memorize the top half. You'll be performing in no time.* 🌬️
