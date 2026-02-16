# Module 6 — Envelopes & Dynamics

> **Goal:** Learn how to shape a sound's volume, filter, and pitch over time using ADSR envelopes. These are what turn a raw tone into a musical sound — a pluck, a pad, a kick, a stab. Master dynamics control with gain, velocity, and compression.

---

## 6.1 What is an Envelope?

An envelope describes **how a parameter changes over the lifetime of a single note**. The most common type is the **ADSR envelope** (Attack, Decay, Sustain, Release):

```
  Level
   ▲
   │     Peak
   │     ╱╲
   │    ╱  ╲
   │   ╱    ╲──────── Sustain Level
   │  ╱                ╲
   │ ╱     Decay        ╲  Release
   │╱                    ╲
   ├──┬────┬──────────┬───╲──→ Time
   │  A    D    S      │
   │              note off
   │
   └── silence
```

| Phase | What Happens |
|-------|-------------|
| **Attack** (A) | Sound rises from silence to peak level |
| **Decay** (D) | Sound falls from peak to sustain level |
| **Sustain** (S) | Sound holds at this level while the note is active |
| **Release** (R) | Sound fades to silence after the note ends |

> Every sound you hear in music has an envelope, whether designed explicitly or not. A piano note has a fast attack, quick decay, moderate sustain, and long release. A pad has a slow attack, slow everything. A hi-hat has a near-instant attack and decay with zero sustain.

---

## 6.2 Amplitude Envelope — Shaping Volume

The amplitude envelope controls the **volume** of your sound over time.

### Individual Parameters

```js
// Attack: time (in seconds) to reach peak
note("c3 e3 g3 c4").attack(0)     // instant (default)
note("c3 e3 g3 c4").attack(0.01)  // very fast (percussive)
note("c3 e3 g3 c4").attack(0.1)   // moderate (string-like)
note("c3 e3 g3 c4").attack(0.5)   // slow (pad-like)
note("c3 e3 g3 c4").attack(1.0)   // very slow (ambient swell)
```

```js
// Decay: time (in seconds) to fall from peak to sustain level
// Only audible when sustain < 1
note("c3 e3 g3 c4").decay("<.1 .2 .3 .4>").sustain(0)
```

```js
// Sustain: level held during the note (0 = silent, 1 = full volume)
note("c3 e3 g3 c4").decay(.2).sustain("<0 .25 .5 .75 1>")
```

```js
// Release: time (in seconds) to fade after note ends
note("c3 e3 g3 c4").release("<0 .1 .4 .6 1>/2")
```

### Aliases

| Full Name | Alias |
|-----------|-------|
| `.attack()` | `.att()` |
| `.decay()` | `.dec()` |
| `.sustain()` | `.sus()` |
| `.release()` | `.rel()` |

### ADSR Shorthand

Combine all four in one call using the `.adsr()` function with `:` separators:

```js
// .adsr("attack:decay:sustain:release")
note("[c3 bb2 f3 eb3]*2").sound("sawtooth")
  .lpf(600)
  .adsr(".1:.1:.5:.2")
```

---

## 6.3 Envelope Shapes for Different Sounds

The envelope you choose **defines the character** of the sound. Here's a cheat sheet:

### Percussive Pluck (kick, stab, synth hit)

```
Fast attack → Short decay → No sustain → No release
```

```js
note("c3 e3 g3 c4").s("sawtooth")
  .attack(0.001)
  .decay(0.15)
  .sustain(0)
  .release(0.01)
  .lpf(2000)
```

### Piano / Keys

```
Instant attack → Medium decay → Moderate sustain → Medium release
```

```js
note("c3 e3 g3 c4").s("triangle")
  .attack(0.005)
  .decay(0.3)
  .sustain(0.4)
  .release(0.3)
```

### Pad / String

```
Slow attack → Long decay → High sustain → Long release
```

```js
note("[c4,eb4,g4]").s("sawtooth")
  .attack(0.5)
  .decay(0.8)
  .sustain(0.7)
  .release(1.5)
  .lpf(2000)
```

### Kick Drum Body

```
Instant attack → Medium decay → Zero sustain → No release
```

```js
note("c1!4").s("sine")
  .attack(0)
  .decay(0.4)
  .sustain(0)
  .release(0.01)
```

### Hi-Hat / Click

```
Instant attack → Very short decay → Zero sustain
```

```js
s("white*8")
  .attack(0)
  .decay(0.03)
  .sustain(0)
```

### Brass Stab

```
Moderate attack → No decay → Full sustain → Quick release
```

```js
note("c3 eb3 g3 c4").s("sawtooth")
  .attack(0.05)
  .decay(0)
  .sustain(1)
  .release(0.1)
  .lpf(3000)
```

> 💡 **Exercise:** Pick any waveform and a note. Try each of the six envelope shapes above. Listen to how the same raw sound completely transforms.

---

## 6.4 Patterning Envelope Parameters

Like everything in Strudel, envelope parameters can be **patterns**:

```js
// Different decay each cycle
note("c3 e3 g3 c4").s("sawtooth")
  .decay("<.05 .1 .2 .5>")
  .sustain(0)
  .lpf(2000)
```

```js
// Attack swells over 4 cycles
note("[c4,eb4,g4]").s("sawtooth")
  .attack("<0.01 0.1 0.3 0.8>")
  .release(0.5)
  .lpf(2000)
```

```js
// Random decay per event
note("c3 e3 g3 c4").s("triangle")
  .decay(rand.range(0.05, 0.5))
  .sustain(0)
```

---

## 6.5 Pitch Envelope — Sound Design Power Tool

The **pitch envelope** changes the **pitch** of a sound over time. This is the secret weapon for:
- **Kick drum design** — pitch drops from high to low = "punch"
- **Laser / FX sounds** — pitch sweeps
- **Tom / percussion tuning** — subtle pitch bend adds life

### Parameters

| Parameter | Alias | What it does |
|-----------|-------|-------------|
| `.penv()` | — | **Depth** — how many semitones the pitch sweeps |
| `.pdecay()` | — | **Decay time** — how fast the pitch drops |
| `.prelease()` | — | **Release** — pitch behavior after note off |
| `.pattack()` | — | **Attack** — time for pitch to reach peak |
| `.panchor()` | — | **Anchor** — which frequency is the "base" (0 = note, 1 = note+penv) |
| `.pcurve()` | — | **Curve** — shape of the pitch decay |

### The Synthesized Kick Drum

This is the most important use of pitch envelopes in techno:

```js
// The Classic: sine + pitch envelope
note("c1!4").s("sine")
  .penv(24)          // pitch starts 24 semitones (2 octaves) above
  .pdecay(0.06)      // drops to base note in 60ms
  .decay(0.4)        // amplitude holds for 400ms
  .sustain(0)        // then silence
```

**How it works:**
1. The note is C1 (very low)
2. `penv(24)` makes the pitch START at C3 (24 semitones above C1)
3. `pdecay(0.06)` makes the pitch drop from C3 → C1 in 60 milliseconds
4. Your ear hears: a high "click" that sweeps down into a low "thud" = **kick drum**

### Varying Pitch Envelope Depth

```js
// More penv = more "click" / punch at the top
note("c1").s("sine").penv(12).pdecay(.06).decay(.4).sustain(0)  // 1 octave
note("c1").s("sine").penv(24).pdecay(.06).decay(.4).sustain(0)  // 2 octaves
note("c1").s("sine").penv(36).pdecay(.06).decay(.4).sustain(0)  // 3 octaves
note("c1").s("sine").penv(48).pdecay(.06).decay(.4).sustain(0)  // 4 octaves!
```

### Varying Pitch Decay Time

```js
// Faster decay = tighter, more percussive
note("c1").s("sine").penv(24).pdecay(.02).decay(.4).sustain(0)  // very tight
note("c1").s("sine").penv(24).pdecay(.06).decay(.4).sustain(0)  // standard
note("c1").s("sine").penv(24).pdecay(.15).decay(.4).sustain(0)  // tonal sweep
note("c1").s("sine").penv(24).pdecay(.4).decay(.4).sustain(0)   // obvious sweep
```

### Kick Drum Variations

```js
setcpm(128/4)

// Tight, punchy kick (for fast techno)
$: note("c1!4").s("sine")
  .penv(36).pdecay(.04)
  .decay(.25).sustain(0)

// Deep, round kick (for deep/minimal techno)
$: note("c1!4").s("sine")
  .penv(18).pdecay(.08)
  .decay(.5).sustain(0)

// Distorted kick (for industrial/hard techno)
$: note("d1!4").s("sine")
  .penv(36).pdecay(.05)
  .decay(.35).sustain(0)
  .distort("3:.5")
```

### Laser / FX Sounds

```js
// Laser pew
note("c4!8").s("sine")
  .penv(48).pdecay(0.1)
  .decay(0.2).sustain(0)

// Rising pitch (negative penv = pitch goes UP)
note("c3").s("sine")
  .penv(-24).pdecay(0.5)
  .decay(1).sustain(0)
```

### Pitch Envelope on Toms

```js
setcpm(128/4)
note("~ g2 ~ <e2 d2>").s("triangle")
  .penv(12).pdecay(0.05)
  .decay(0.2).sustain(0)
```

> 💡 **Exercise:** Build three different kicks by only changing `penv` and `pdecay`:
> 1. A tight punchy kick (high penv, short pdecay)
> 2. A deep boomy kick (low penv, longer pdecay)
> 3. A tonal, zappy kick (high penv, long pdecay)

---

## 6.6 Filter Envelope — Recap & Deep Dive

We covered filter envelopes in Module 4 (Section 4.8), but here's a deeper look at how they interact with the amplitude envelope.

### Amplitude + Filter Envelope Together

The magic of subtractive synthesis comes from using **both** envelopes:

```js
setcpm(128/4)
note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
  // Amplitude envelope: plucky
  .attack(0.001)
  .decay(0.2)
  .sustain(0)
  .release(0.05)
  // Filter envelope: bright attack, dark sustain
  .lpf(300)
  .lpenv(6)
  .lpa(0.005)
  .lpd(0.15)
  .lps(0)
  .lpq(12)
  .ftype("ladder")
```

**What happens:** On each note:
1. **Amplitude** spikes instantly and fades over 200ms
2. **Filter** opens briefly (bright "pluck") then closes back to 300 Hz

The two envelopes working together create a sound much more interesting than either alone.

### Independent Timing

The filter envelope doesn't have to match the amplitude envelope:

```js
// Fast amp, slow filter → "wah" effect
note("c2!4").s("sawtooth")
  .decay(0.8).sustain(0.5)       // long amp
  .lpf(200).lpenv(6)
  .lpd(0.5).lps(0.2)             // slow filter
```

```js
// Slow amp, fast filter → "synth breath"
note("[c4,eb4,g4]").s("sawtooth")
  .attack(0.5).release(1)        // slow amp (pad)
  .lpf(500).lpenv(4)
  .lpa(0.3).lpd(0.1).lps(0.8)   // fast filter opening
```

---

## 6.7 Dynamics — Gain, Velocity & Compression

### `gain` — Master Volume Control

`.gain()` controls the volume of each event. Default is ~0.8. Values are exponential:

```js
// Accent pattern — louder on beats 1 and 3
s("hh*8").gain(".8 .4 .6 .4 .8 .4 .6 .4")
```

```js
// Crescendo using a signal
s("hh*16").gain(saw.range(0.2, 1).slow(4))
```

### `velocity` — Secondary Volume

`.velocity()` is **multiplied** with gain. Useful for layering volume controls:

```js
// gain controls base level, velocity controls accent
s("hh*8")
  .gain(".4!2 1 .4!2 1 .4 1")
  .velocity(".4 1")
```

### Compressor

The compressor reduces the dynamic range — it makes loud parts quieter and can make the overall sound punchier:

```js
// compressor("threshold:ratio:knee:attack:release")
s("bd sd [~ bd] sd, hh*8")
  .compressor("-20:20:10:.002:.02")
```

| Parameter | What it does | Typical range |
|-----------|-------------|---------------|
| threshold | Level (dB) above which compression starts | -40 to 0 |
| ratio | Compression ratio (20 = hard limiting) | 1 to 20 |
| knee | How gradually compression kicks in | 0 to 40 |
| attack | How fast compressor reacts (seconds) | 0.001 to 0.1 |
| release | How fast compressor lets go (seconds) | 0.01 to 0.5 |

### Postgain

Gain applied **after** all effects (including compression):

```js
s("bd sd [~ bd] sd, hh*8")
  .compressor("-20:20:10:.002:.02")
  .postgain(1.5)  // boost after compression
```

> 💡 **Exercise:** Create a hi-hat pattern with a clear accent pattern:
> 1. Start: `s("hh*16")`
> 2. Add accents: `.gain(".8 .3 .5 .3")`
> 3. Add compression: `.compressor("-15:8:10:.002:.02")`
> 4. Listen to how compression evens out the dynamics

---

## 6.8 Combining Envelopes — Complete Sound Recipes

### Recipe 1: Techno Kick (Amplitude + Pitch)

```js
setcpm(128/4)
note("c1!4").s("sine")
  .penv(24).pdecay(.06)           // pitch: high click → low thud
  .attack(0.001).decay(.45)       // amp: instant hit, medium body
  .sustain(0).release(0.01)
  .distort("1.5:.6")              // add grit
```

### Recipe 2: Acid Bass (Amplitude + Filter)

```js
setcpm(128/4)
note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
  .attack(0.001).decay(0.2)      // amp: percussive
  .sustain(0).release(0.05)
  .lpf(400).lpq(15)              // filter: dark + resonant
  .lpenv(5).lpa(0.005)           // filter env: bright pluck
  .lpd(0.12).lps(0)
  .ftype("ladder")
```

### Recipe 3: Dark Pad (Slow Amplitude + Gentle Filter)

```js
setcpm(128/4)
note("<[c4,eb4,g4] [bb3,d4,f4] [ab3,c4,eb4] [g3,bb3,d4]>")
  .s("sawtooth")
  .attack(0.6).decay(1)          // amp: slow swell
  .sustain(0.6).release(1.5)
  .lpf(1200).lpq(2)              // filter: moderate brightness
  .lpenv(2).lpa(0.4)             // filter env: slow opening
  .lpd(0.5).lps(0.6)
  .room(0.5).roomsize(0.7)
```

### Recipe 4: Synth Stab (Fast Everything)

```js
setcpm(128/4)
note("[c3 ~ eb3 ~]*2").s("square")
  .attack(0.001).decay(0.1)      // amp: very short
  .sustain(0).release(0.02)
  .lpf(800).lpq(8)               // filter: moderate
  .lpenv(5).lpd(0.06).lps(0)     // filter env: fast pluck
  .ftype("24db")
  .delay(0.3).delaytime(0.1875)
```

### Recipe 5: Snappy Tom (Pitch + Amplitude)

```js
setcpm(128/4)
note("~ g2 ~ <e2 d2>").s("triangle")
  .penv(12).pdecay(0.04)         // pitch: slight drop
  .attack(0.001).decay(0.15)     // amp: short
  .sustain(0).release(0.02)
  .lpf(3000)
```

### Recipe 6: Layered Kick (sine body + noise click)

```js
setcpm(128/4)
stack(
  // Sub body
  note("c1!4").s("sine")
    .penv(18).pdecay(.08)
    .decay(.5).sustain(0)
    .gain(0.7),
  // Click transient
  s("white!4")
    .hpf(4000)
    .decay(.008).sustain(0)
    .gain(0.2)
)
```

---

## 6.9 Practice Challenges

### Challenge 1: Envelope Ear Training
Play `note("c3").s("sawtooth")` with the following envelopes and describe what each sounds like (pluck? pad? stab? breath?):
1. `.attack(0).decay(0.1).sustain(0)`
2. `.attack(0.5).decay(0).sustain(1).release(0.5)`
3. `.attack(0).decay(0.5).sustain(0.3).release(1)`
4. `.attack(0.01).decay(0).sustain(1).release(0.01)`

<details>
<summary>Answers</summary>

1. **Pluck** — instant hit, quick fade
2. **Pad / Swell** — slow rise, holds, slow fade
3. **Piano-like** — instant hit, fades to moderate hold, long tail
4. **Organ** — instant on, instant off, full volume while held
</details>

### Challenge 2: Design a Kick
Create a kick drum using only `sine` + `penv` + amplitude envelope. Goal: a kick that has a clear "click" attack and a deep "boom" body lasting about 300ms.

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
note("c1!4").s("sine")
  .penv(30).pdecay(.05)
  .decay(.3).sustain(0)
```
</details>

### Challenge 3: Two-Envelope Bass
Create a bass using BOTH amplitude and filter envelopes. The filter should open on each note attack and close quickly.

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
note("c2 c2 <eb2 f2> c2").s("sawtooth")
  .decay(0.25).sustain(0)            // amp envelope
  .lpf(300).lpenv(5)                 // filter base + depth
  .lpd(0.1).lps(0).lpq(10)          // filter ADSR + resonance
  .ftype("ladder")
```
</details>

### Challenge 4: Dynamic Drums
Create a drum pattern with a clear volume accent pattern where beat 1 and beat 3 are louder:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
s("bd*4").bank("RolandTR909")
  .gain("1 .5 .8 .5")
s("hh*16").bank("RolandTR909")
  .gain(".8 .3 .5 .3")
```
</details>

### Challenge 5: Complete Sound Palette
Using envelopes, create all four of these from a single sawtooth wave:
1. A short pluck
2. A long pad
3. A percussive stab
4. A filtered bass with envelope

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
// 1. Pluck
$: note("c4 e4 g4 c5").s("sawtooth")
  .decay(0.1).sustain(0).lpf(3000).gain(0.3)

// 2. Pad
$: note("[c4,eb4,g4]").s("sawtooth")
  .attack(0.5).sustain(0.7).release(1.5).lpf(2000).gain(0.2)

// 3. Stab
$: note("[c3 ~ eb3 ~]*2").s("sawtooth")
  .decay(0.08).sustain(0).lpf(4000).gain(0.3)

// 4. Filtered bass
$: note("c2 c2 <eb2 f2> c2").s("sawtooth")
  .decay(0.2).sustain(0)
  .lpf(400).lpenv(4).lpd(0.1).lps(0).lpq(10)
  .ftype("ladder").gain(0.5)
```
</details>

---

## 6.10 Key Takeaways

1. **ADSR** controls how volume evolves: Attack, Decay, Sustain, Release
2. Short envelopes = percussive (kicks, stabs, plucks)
3. Long envelopes = sustained (pads, strings, ambient)
4. **Pitch envelope** (`penv` + `pdecay`) = the secret to kick drum design
5. **Filter envelope** (`lpenv` + `lpd`) = the secret to bass and lead design
6. Combining amplitude + filter + pitch envelopes = complete sound design
7. `gain` controls per-event volume, `velocity` multiplies with gain
8. `compressor` evens out dynamics — essential for polished mixes
9. `.adsr("a:d:s:r")` is a convenient shorthand
10. All envelope parameters can be patterned for variation

---

**Previous:** [← Module 5 — Samples & Drum Machines](./Module_05_Samples_DrumMachines.md)
**Next:** [Module 7 — Building Core Sounds →](./Module_07_Building_Core_Sounds.md)
