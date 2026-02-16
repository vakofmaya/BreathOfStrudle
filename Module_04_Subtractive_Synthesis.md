# Module 4 — Subtractive Synthesis

> **Goal:** Learn to sculpt raw oscillator sound into musical timbres using Strudel's filters, filter envelopes, and signal chain. This is the most important sound design technique in electronic music — and the foundation of nearly every bass, pad, and lead sound in Minimal Techno.

---

## 4.1 What is Subtractive Synthesis?

The idea is simple:

1. **Start with something harmonically rich** (a sawtooth wave, for example — full of overtones)
2. **Remove frequencies you don't want** using a **filter**

That's it. You're "subtracting" frequencies from a raw waveform to sculpt the sound you hear in your head.

> Think of it like a sculptor: the sawtooth wave is the raw marble, the filter is the chisel.

```
Oscillator (sawtooth)  ──→  Filter (remove highs)  ──→  Your ear
   [BRIGHT, BUZZY]            [WARM, ROUNDED]           [NICE!]
```

Let's hear the raw material first:

```js
// Raw sawtooth — lots of harmonics, very buzzy
note("c2 c2 c2 c2").s("sawtooth")._scope()
```

Now let's filter it:

```js
// Same sawtooth through a low-pass filter at 400 Hz
note("c2 c2 c2 c2").s("sawtooth").lpf(400)._scope()
```

Hear the difference? The buzzy high frequencies are gone. The sound is warmer, rounder, more "musical." **That's subtractive synthesis.**

---

## 4.2 The Signal Chain

Before diving into filters, it helps to know **where** effects sit in Strudel's audio pipeline. The order is fixed — you can't rearrange it:

```
Sound Source (oscillator or sample)
  │
  ├── Pitch effects (detune, penv)
  │
  ├── Gain + ADSR amplitude envelope
  │
  ├── Low-Pass Filter (lpf)
  ├── High-Pass Filter (hpf)
  ├── Band-Pass Filter (bpf)
  ├── Vowel Filter (vowel)
  │
  ├── Coarse (sample rate reduction)
  ├── Crush (bit crushing)
  ├── Distort (waveshaping)
  ├── Tremolo
  ├── Compressor
  ├── Pan
  ├── Phaser
  ├── Postgain
  │
  └── Sends ──→ Delay / Reverb (global, per orbit)
```

> **Important:** Each effect is **single-use**. You can't do `.lpf(200).distort(3).lpf(800)` hoping for two filters. The second `.lpf()` just overrides the first. The filter always comes before distortion in the chain.

---

## 4.3 Low-Pass Filter (LPF)

The **low-pass filter** is the most important tool in subtractive synthesis. It allows frequencies **below** the cutoff point to pass through, and attenuates (reduces) frequencies **above** it.

### Cutoff Frequency

The cutoff frequency (in Hz) determines where the filter starts working:

```js
// Full brightness — cutoff above audible range
note("c2").s("sawtooth").lpf(20000)._scope()

// Progressively darker
note("c2").s("sawtooth").lpf(8000)._scope()
note("c2").s("sawtooth").lpf(4000)._scope()
note("c2").s("sawtooth").lpf(2000)._scope()
note("c2").s("sawtooth").lpf(1000)._scope()
note("c2").s("sawtooth").lpf(500)._scope()
note("c2").s("sawtooth").lpf(200)._scope()
```

**Aliases for `.lpf()`:** `.cutoff()`, `.ctf()`, `.lp()`

### Animated Cutoff

Make the filter move over time to create sweeps:

```js
// Cutoff changes each cycle via angle brackets
s("bd sd [~ bd] sd, hh*6").lpf("<4000 2000 1000 500 200 100>")
```

```js
// Continuous sweep using a sine signal
note("c2").s("sawtooth")
  .lpf(sine.range(200, 4000).slow(4))
  ._scope()
```

> 💡 **Exercise:** Play `note("c2").s("sawtooth").lpf(2000)._scope()` and gradually change the cutoff value. Go from 200 to 10000. Listen to how the character changes from "dark" to "bright."

---

### Resonance (Q)

Resonance (`.lpq()`) boosts frequencies **right at the cutoff point**, creating a peak that adds character:

```js
// No resonance (flat response)
note("c2").s("sawtooth").lpf(1000).lpq(0)._scope()

// Mild resonance — slight emphasis
note("c2").s("sawtooth").lpf(1000).lpq(5)._scope()

// High resonance — pronounced peak, "acid" sound
note("c2").s("sawtooth").lpf(1000).lpq(15)._scope()

// Very high resonance — filter starts to "sing"
note("c2").s("sawtooth").lpf(1000).lpq(30)._scope()
```

**Alias for `.lpq()`:** `.resonance()`

### Resonance + Moving Cutoff = The "Acid" Sound

This is the classic sound of the Roland TB-303 and acid techno:

```js
setcpm(128/4)
note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
  .lpf(sine.range(300, 3000).slow(4))
  .lpq(15)
  ._scope()
```

> 💡 **Exercise:** Start with `.lpq(0)` and increase to 5, 10, 15, 20, 30. Listen to how resonance transforms the sound from flat to squelchy to screaming.

---

### Cutoff + Resonance in Mini-Notation

You can combine cutoff and resonance inline with `:` separator:

```js
// cutoff:resonance
s("bd*16").lpf("1000:0 1000:10 1000:20 1000:30")
```

---

## 4.4 High-Pass Filter (HPF)

The high-pass filter does the opposite of LPF — it removes frequencies **below** the cutoff and lets high frequencies pass through.

### Use Cases
- **Remove muddiness** from hi-hats, claps, and percussion
- **Thin out** a pad to make room for the bass
- **Buildups and transitions** — sweeping HPF up creates tension

```js
// Raw drums — full frequency
s("bd sd [~ bd] sd, hh*8")

// Remove low end — only high frequencies remain
s("bd sd [~ bd] sd, hh*8").hpf(2000)

// Gradually sweep HPF up for a buildup effect
s("bd sd [~ bd] sd, hh*8").hpf("<100 200 500 1000 2000 4000>")
```

### HPF Resonance

```js
s("bd sd [~ bd] sd, hh*8").hpf(2000).hpq("<0 10 20 30>")
```

**Aliases for `.hpf()`:** `.hp()`, `.hcutoff()`
**Alias for `.hpq()`:** `.hresonance()`

### HPF + Resonance in Mini-Notation

```js
s("bd sd [~ bd] sd, hh*8").hpf("<2000 2000:25>")
```

> 💡 **Exercise:** Take a full drum beat and apply `.hpf()`. Start at 100 and gradually increase. Notice how the kick disappears first (it's the lowest frequency), then the snare body, until only the sizzle of the hats remains.

---

## 4.5 Band-Pass Filter (BPF)

The band-pass filter lets through only a **narrow band** of frequencies around the cutoff, cutting both the highs and lows. It creates a "telephone" or "radio" effect.

```js
// Isolate different frequency bands
s("bd sd [~ bd] sd, hh*6").bpf("<1000 2000 4000 8000>")
```

### BPF Resonance

Narrower band = more extreme effect:

```js
s("bd sd [~ bd] sd").bpf(500).bpq("<0 1 2 3>")
```

**Aliases:** `.bandf()`, `.bp()` for frequency; `.bandq()` for resonance.

> **Techno tip:** Band-pass a noise source to create tuned percussion, or use it on a pad for a "trapped in a box" effect.

---

## 4.6 Vowel Filter

The vowel filter is a special **formant filter** that makes sounds "speak" vowel sounds. Formants are the resonant frequencies of the human vocal tract.

```js
// Sawtooth bass "speaking" vowels
note("[c2 <eb2 <g2 g1>>]*2").s("sawtooth")
  .vowel("<a e i <o u>>")
```

### Available Vowels

| Symbol | Sound | IPA |
|--------|-------|-----|
| `a` | "ah" | [a] |
| `e` | "eh" | [e] |
| `i` | "ee" | [i] |
| `o` | "oh" | [o] |
| `u` | "oo" | [u] |
| `ae` | "æ" | [æ] |
| `aa` | "å" | [ɑ] |
| `oe` | "ö" | [ø] |
| `y` | "ı" | [y] |
| `uh` | "uh" | [ɯ] |

```js
// On drums for a weird vocal effect
s("bd sd mt ht bd [~ cp] ht lt").vowel("[a|e|i|o|u]")
```

> 💡 **Exercise:** Play `note("c2").s("sawtooth").vowel("<a e i o u>")._scope()` and listen to the "talking bass."

---

## 4.7 Filter Types — `ftype()`

Strudel offers three different filter circuit emulations. The choice affects how sharply the filter cuts off frequencies:

| `ftype()` | Name | Slope | Character |
|-----------|------|-------|-----------|
| `0` or `"12db"` | 12dB/oct | Gentle | Subtle, polite, plenty of harmonics still audible |
| `1` or `"ladder"` | Ladder | Aggressive | Emulates analog Moog-style filter, self-resonance possible |
| `2` or `"24db"` | 24dB/oct | Steep | Very sharp cutoff, dramatic effect |

```js
// Compare all three on the same material
note("c2 f2 g2 c3").s("sawtooth")
  .lpf(500).lpq(5)
  .ftype("<0 1 2>")
  ._scope()
```

```js
// Ladder filter with resonance — the classic acid sound
note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
  .lpf(600).lpq(15)
  .ftype("ladder")
  ._scope()
```

> **For Minimal Techno:**
> - **Ladder** for acid bass lines (aggressive, resonant)
> - **24dB** for deep sub-bass (steep rolloff keeps it clean)
> - **12dB** for pads and gentle filtering (lets some brightness through)

> 💡 **Exercise:** Play the same bass pattern with each filter type. Which one sounds fattest? Which one has the most "squelch" with high resonance?

---

## 4.8 Filter Envelopes — Making Filters Move Over Time

A **filter envelope** is an ADSR shape (Attack, Decay, Sustain, Release) that controls the filter cutoff **automatically over the lifetime of each note**. This is what makes subtractive synthesis come alive — static filters sound flat; moving filters sound musical.

### The Parameters

| Parameter | Alias | What it does |
|-----------|-------|-------------|
| `lpf` | `cutoff` | Base cutoff frequency (starting/resting position) |
| `lpenv` | `lpe` | **Envelope depth** — how many semitones above the base cutoff the envelope sweeps to |
| `lpa` | `lpattack` | **Attack** — time to reach peak cutoff |
| `lpd` | `lpdecay` | **Decay** — time to fall from peak to sustain level |
| `lps` | `lpsustain` | **Sustain** — cutoff level held during note |
| `lpr` | `lprelease` | **Release** — cutoff fade after note ends |

### How It Works

```
Cutoff
  ▲
  │     ╱╲
  │    ╱  ╲
  │   ╱    ╲─────── sustain level
  │  ╱              ╲
  │ ╱                ╲
  │╱                  ╲
  ├─────────────────────→ Time
  │  A    D     S     R
  │
  └── base cutoff (lpf value)
```

The envelope **adds** to the base cutoff. So if `lpf` = 300 and `lpenv` = 6:
- At the **peak** (end of attack), the cutoff jumps up by 6 modulatory units above 300
- During **decay**, it falls back toward the sustain level
- A higher `lpenv` = more dramatic sweep

### Basic Filter Envelope

```js
// Simple filter pluck — starts bright, decays to dark
note("c2 c2 c2 c2").s("sawtooth")
  .lpf(300)          // base cutoff: quite dark
  .lpenv(4)          // envelope sweeps up 4 units
  .lpa(0.005)        // nearly instant attack
  .lpd(0.2)          // decay over 200ms
  .lps(0)            // sustain cutoff at base level
  ._scope()
```

### Varying Envelope Depth

```js
// Low depth: subtle
note("c2").s("sawtooth").lpf(300).lpenv(1).lpd(0.2).lps(0)._scope()

// Medium depth: clear sweep
note("c2").s("sawtooth").lpf(300).lpenv(4).lpd(0.2).lps(0)._scope()

// High depth: dramatic sweep
note("c2").s("sawtooth").lpf(300).lpenv(8).lpd(0.2).lps(0)._scope()
```

### Varying Decay Time

```js
// Short decay: percussive pluck
note("c2").s("sawtooth").lpf(300).lpenv(6).lpd(0.05).lps(0)._scope()

// Medium decay: bass-like
note("c2").s("sawtooth").lpf(300).lpenv(6).lpd(0.2).lps(0)._scope()

// Long decay: pad-like opening
note("c2").s("sawtooth").lpf(300).lpenv(6).lpd(0.8).lps(0)._scope()
```

### Using Signals for Organic Movement

Instead of fixed values, use continuous signals for parameters:

```js
note("[c eb g <f bb>](3,8,<0 1>)".sub(12))
  .s("sawtooth")
  .lpf(sine.range(300, 2000).slow(16))       // base cutoff sweeps
  .lpa(0.005)
  .lpd(perlin.range(.02, .2))                // random decay times
  .lps(perlin.range(0, .5).slow(3))          // random sustain levels
  .lpq(sine.range(2, 10).slow(32))           // resonance sweeps
  .release(.5)
  .lpenv(perlin.range(1, 8).slow(2))         // random env depth
  .ftype("24db")
  .room(1)
  ._scope()
```

> 💡 **Exercise:**
> 1. Start: `note("c2").s("sawtooth").lpf(300).lpenv(4).lpd(0.2).lps(0)._scope()`
> 2. Change `lpenv` from 1 to 8 — hear the sweep get more dramatic
> 3. Change `lpd` from 0.05 to 1.0 — hear the speed of the sweep change
> 4. Try adding `.lpq(10)` for resonance during the sweep

---

### HPF and BPF Envelopes

The same envelope system works for high-pass and band-pass filters, just with different prefixes:

**High-pass envelope:**
```js
note("c2").s("sawtooth")
  .hpf(100)
  .hpenv(4)
  .hpa(0.01).hpd(0.3).hps(0)
```

**Band-pass envelope:**
```js
note("c3").s("sawtooth")
  .bpf(1000)
  .bpenv(4)
  .bpa(0.01).bpd(0.5).bps(0.3)
```

---

## 4.9 Putting It All Together — Subtractive Sound Recipes

### Recipe 1: Classic Techno Bass

```js
setcpm(128/4)
note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
  .lpf(400).lpq(12)
  .lpenv(4)
  .lpa(0.005).lpd(0.15).lps(0)
  .ftype("ladder")
  .decay(0.2).sustain(0).release(0.05)
  ._scope()
```

**What's happening:**
- `sawtooth` = harmonically rich starting point
- `lpf(400)` = dark base cutoff
- `lpq(12)` = resonant, "squelchy"
- `lpenv(4)` = filter opens on each note attack
- `lpa(0.005)` = instant attack
- `lpd(0.15)` = quick fade back to dark
- `lps(0)` = no sustain (fully closed after decay)
- `ftype("ladder")` = aggressive, analog-style filter
- `decay/sustain/release` = short note (amplitude envelope)

### Recipe 2: Dark Pad

```js
setcpm(128/4)
note("<[c4,eb4,g4] [bb3,d4,f4] [ab3,c4,eb4] [g3,bb3,d4]>")
  .s("sawtooth")
  .lpf(1500).lpq(2)
  .lpenv(2)
  .lpa(0.3).lpd(0.8).lps(0.5)
  .attack(0.5).release(1.5)
  .room(0.5).roomsize(0.7)
  ._scope()
```

**What's happening:**
- Chord (3 notes stacked) as source
- `lpf(1500)` = moderate brightness
- Low resonance (2) = smooth, not harsh
- Filter envelope slowly opens and partially sustains
- Long amplitude attack + release = gentle, sweeping pad
- Reverb for space

### Recipe 3: Filtered Stab

```js
setcpm(128/4)
note("[c3 ~ eb3 ~]*2").s("square")
  .lpf(800).lpq(8)
  .lpenv(5)
  .lpa(0.001).lpd(0.08).lps(0)
  .ftype("24db")
  .decay(0.1).sustain(0).release(0.02)
  .delay(0.3).delaytime(0.1875)
  ._scope()
```

**What's happening:**
- `square` wave = hollow, punchy
- Very short filter + amplitude envelope = percussive stab
- `24db` filter = steep, dramatic cutoff
- Delay adds rhythmic echo

### Recipe 4: Evolving Sweep

```js
setcpm(128/4)
note("c2").s("sawtooth")
  .lpf(sine.range(100, 6000).slow(16))  // 16-cycle sweep
  .lpq(sine.range(0, 15).slow(8))       // resonance breathes
  .ftype("ladder")
  ._scope()
```

**What's happening:**
- Fixed note, but the filter moves continuously
- Sine signal sweeps cutoff up and down over 16 cycles
- Resonance also moves independently
- Creates a hypnotic, evolving texture

### Recipe 5: Complete Subtractive Drum + Bass Sketch

```js
setcpm(128/4)

// Kick (sine + pitch envelope)
$: note("c1!4").s("sine")
  .penv(24).pdecay(.06)
  .decay(.45).sustain(0)
  .gain(0.85)

// Hi-hats (noise + HPF)
$: s("white*16")
  .hpf(8000)
  .decay(.03).sustain(0)
  .gain(".5 .3 .8 .3")

// Clap (noise + BPF)
$: s("~ pink ~ pink")
  .bpf(2000).bpq(2)
  .decay(.12).sustain(0)
  .gain(0.5)
  .room(0.15)

// Acid bass (saw + ladder filter + filter envelope)
$: note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
  .lpf(400).lpq(12)
  .lpenv(4).lpa(0.005).lpd(0.15).lps(0)
  .ftype("ladder")
  .decay(0.2).sustain(0).release(0.05)
  .gain(0.6)

// Pad (saw + gentle filter + reverb)
$: note("<[c4,eb4,g4] [bb3,d4,f4]>")
  .s("sawtooth")
  .lpf(2000).lpq(2)
  .attack(0.4).release(1)
  .room(0.4).roomsize(0.6)
  .gain(0.25)
```

> 💡 **Exercise:** Copy the complete sketch above and modify:
> 1. Change the bass filter type from `"ladder"` to `"24db"` — hear how it changes
> 2. Increase the pad's `.lpq()` to 10 — it becomes harsher
> 3. Add `.lpf(sine.range(500,4000).slow(8))` to the pad for a filter sweep
> 4. Try changing the bass notes

---

## 4.10 Practice Challenges

### Challenge 1: Filter Sweep
Create a sawtooth drone on C2 with a filter that sweeps from 200 Hz to 5000 Hz over 8 cycles:

<details>
<summary>Solution</summary>

```js
note("c2").s("sawtooth")
  .lpf(sine.range(200, 5000).slow(8))
  ._scope()
```
</details>

### Challenge 2: Resonant Bass Line
Write a 4-note bass pattern using sawtooth with:
- Cutoff at 500 Hz
- Resonance at 15
- Ladder filter type
- Filter envelope: fast attack, 100ms decay, no sustain

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
note("c2 eb2 f2 g2").s("sawtooth")
  .lpf(500).lpq(15)
  .lpenv(4)
  .lpa(0.005).lpd(0.1).lps(0)
  .ftype("ladder")
  .decay(0.2).sustain(0)
  ._scope()
```
</details>

### Challenge 3: HPF Buildup
Create a full drum beat that uses a high-pass filter sweep to create a buildup effect. The HPF should sweep from 50 Hz to 5000 Hz over 8 cycles:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
s("bd*4, hh*8, ~ cp ~ cp").bank("RolandTR909")
  .hpf(saw.range(50, 5000).slow(8))
```
</details>

### Challenge 4: Three-Filter Combo
Layer three patterns: one using LPF, one using HPF, and one using BPF, playing at the same time:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
// Bass: low-passed
$: note("c2 ~ <eb2 f2> ~").s("sawtooth")
  .lpf(600).lpq(8).ftype("ladder")
  .decay(0.2).sustain(0)
  .gain(0.5)

// Pad: high-passed (only brightness)
$: note("[c4,eb4,g4]").s("sawtooth")
  .hpf(2000)
  .attack(0.3).release(0.8)
  .room(0.4)
  .gain(0.3)

// Percussion: band-passed (telephone effect)
$: s("rim(5,8)").bank("RolandTR909")
  .bpf(3000).bpq(3)
  .gain(0.4)
```
</details>

### Challenge 5: Vowel Bass
Create a bass line that "talks" using the vowel filter, cycling through all 5 vowels:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
  .vowel("<a e i o u>")
  .gain(0.6)
  ._scope()
```
</details>

---

## 4.11 Key Takeaways

1. **Subtractive synthesis** = start bright (sawtooth), filter to taste
2. **LPF** removes highs → the most important filter for bass, pads, warmth
3. **HPF** removes lows → essential for cleaning up mixes, creating buildups
4. **BPF** isolates a band → special effects, tuned resonance
5. **Vowel filter** = instant vocal-like formants
6. **Resonance (Q)** boosts frequencies at the cutoff → more character, more "acid"
7. **Filter types:**
   - `12db` = gentle, subtle
   - `ladder` = aggressive, analog (best for acid bass)
   - `24db` = steep, dramatic
8. **Filter envelopes** make filters move over time → this is what makes sounds *alive*
   - `lpenv` = depth, `lpa/lpd/lps/lpr` = ADSR shape
9. **Continuous signals** (`sine`, `saw`, `perlin`, `rand`) can modulate any parameter for evolving sounds
10. Same system works for HPF (`hp` prefix) and BPF (`bp` prefix)

---

## What's Next?

You now have the three core pillars:
- **Module 2:** How to write rhythms and set tempo (Mini-Notation)
- **Module 3:** How to generate raw sound (Oscillators, FM, Wavetables)
- **Module 4:** How to shape that sound (Filters + Envelopes)

In the upcoming modules you'll learn:
- **Module 5:** Samples & drum machines (working with recordings instead of synthesis)
- **Module 6:** Amplitude envelopes & dynamics (shaping volume over time)
- **Module 7:** Building complete sounds (kick, bass, pad, lead) from scratch

---

**Previous:** [← Module 3 — Synths & Oscillators](./Module_03_Synths_Oscillators.md)
**Next:** [Module 5 — Samples & Drum Machines →](./Module_05_Samples_DrumMachines.md)
