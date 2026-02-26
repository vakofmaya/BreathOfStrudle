# Module 11 — Structuring a Full Track

> **Goal:** Learn how to organize your sounds and patterns into a complete Minimal Techno arrangement — from a sparse intro through builds, drops, breakdowns, and outro. This is where you go from making loops to making **music**.

---

## 11.1 Minimal Techno Track Structure

A typical Minimal Techno track (6–8 minutes) follows a macro structure designed for DJ mixing and dancefloor energy:

```
 INTRO    │  BUILD   │   MAIN    │ BREAKDOWN │   DROP    │  OUTRO
 16-32 bars  8-16 bars  32-64 bars  8-16 bars  32-64 bars  16-32 bars
```

### The Sections

| Section | Duration | Energy | Techniques |
|---------|----------|--------|------------|
| **Intro** | 16–32 bars | Low | Kick only → add hats, atmosphere |
| **Build** | 8–16 bars | Rising | Layer elements, filter sweeps up, risers |
| **Main Loop** | 32–64 bars | Full | Full groove: kick + hats + clap + bass + pad + melody |
| **Breakdown** | 8–16 bars | Drops down | Strip elements, reverb wash, atmospheric |
| **Drop** | 32–64 bars | Peak | Full energy return, all elements, peak sidechain |
| **Outro** | 16–32 bars | Falling | Fade out elements, return to kicks only |

> **Key principle for DJs:** Intros and outros should have just the kick (or kick + minimal elements) so DJs can mix your track in and out smoothly.
>
> **Key principle for dancers:** A breakdown creates anticipation. The drop releases it. Minimal Techno does this subtly — don't overdo it.

### In Strudel's Cycle-Based System

At 128 BPM with 4 beats per cycle, **1 cycle = 1 bar**:

```js
setcpm(128/4)
// 1 cycle = 4 beats = 1 bar at 128 BPM
// 16 bars = 16 cycles
// 32 bars = 32 cycles
```

---

## 11.2 Setting Up: Tempo & Key

Start every track setup with tempo and your key:

```js
// 128 BPM, 4 beats/cycle
setcpm(128/4)

// Our key: C minor
// Bass notes: c, eb, f, g, ab, bb
// Chord options: Cm, Fm, Ab, Bb, Eb, G
```

---

## 11.3 Method 1: `arrange()` — The Scripted Arrangement

`arrange()` lets you specify **exactly** how many cycles each section plays and what patterns are active:

### Step 1: Define Your Parts

```js
setcpm(128/4)

// Individual elements
let kick   = s("bd*4").bank("RolandTR909")
let hats   = s("hh*16").bank("RolandTR909")
                .gain(".5 .3 .8 .3").swingBy(1/6, 4).hpf(6000)
let clap   = s("~ cp ~ cp").bank("RolandTR909").room(0.15)
let rim    = s("rim(5,16)").bank("RolandTR909").gain(0.3).pan(rand)
let bass   = note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
                .lpf(400).lpq(12).lpenv(4).lpd(0.12).lps(0)
                .ftype("ladder").decay(0.2).sustain(0).gain(0.55)
let pad    = note("<[c4,eb4,g4] [bb3,d4,f4]>")
                .s("sawtooth").lpf(2000).attack(0.3).release(1)
                .room(0.4).gain(0.25).orbit(2)
let melody = n("<0 ~ 3 ~ 5 ~ 7 ~>*2").scale("C4:minor:pentatonic")
                .s("triangle").decay(0.15).sustain(0)
                .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
                .gain(0.2).orbit(2)
let duck   = s("bd*4").bank("RolandTR909")
                .duckorbit(2).duckattack(0.2).duckdepth(1).gain(0)
```

### Step 2: Arrange Into Sections

```js
arrange(
  // INTRO: just kick (16 bars)
  [16, kick],

  // BUILD: add elements (8 bars)
  [4, stack(kick, hats)],
  [4, stack(kick, hats, clap)],

  // MAIN LOOP (32 bars)
  [32, stack(kick, hats, clap, rim, bass, pad, duck)],

  // BREAKDOWN: strip back (8 bars)
  [8, stack(
    hats.lpf(sine.range(500, 8000).slow(8)),
    pad.room(0.8).roomsize(0.9),
    melody
  )],

  // DROP: full energy return (32 bars)
  [32, stack(kick, hats, clap, rim, bass, pad, melody, duck)],

  // OUTRO: fade out (16 bars)
  [8, stack(kick, hats.gain(saw.range(0.5, 0).slow(8)))],
  [8, kick.gain(saw.range(0.8, 0).slow(8))]
)
```

### Pros and Cons of `arrange()`

| ✅ Pros | ❌ Cons |
|---------|---------|
| Full track plays automatically | Not interactive — you can't change it live |
| Precise section timing | Changes require re-evaluation of everything |
| Repeatable | Variables can't be changed per-section easily |

---

## 11.4 Method 2: `cat()` — Sequential Patterns

`cat()` plays one pattern per cycle, rotating through them:

```js
setcpm(128/4)

// Simple buildup: one more element each cycle
cat(
  s("bd*4"),
  s("bd*4, hh*8"),
  s("bd*4, hh*8, ~ cp ~ cp"),
  s("bd*4, hh*8, ~ cp ~ cp, rim(3,8)")
).bank("RolandTR909")
```

Use `.slow(N)` to stretch each step over multiple cycles:

```js
// Each step lasts 8 cycles
cat(
  s("bd*4"),
  s("bd*4, hh*8"),
  s("bd*4, hh*8, ~ cp ~ cp")
).bank("RolandTR909").slow(8)
```

---

## 11.5 Method 3: `$:` Additive Arrangement (Live Coding)

This is the most flexible approach and the one used for live performance. You build the track by **evaluating patterns one at a time**:

```js
setcpm(128/4)

// START with just the kick (Ctrl+Enter to evaluate)
$: s("bd*4").bank("RolandTR909")

// UNCOMMENT and evaluate to add hi-hats
// $: s("hh*16").bank("RolandTR909").gain(".5 .3 .8 .3").hpf(6000)

// UNCOMMENT to add clap
// $: s("~ cp ~ cp").bank("RolandTR909").room(0.15)

// UNCOMMENT to add bass
// $: note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
//   .lpf(400).lpq(12).lpenv(4).lpd(0.12).lps(0)
//   .ftype("ladder").decay(0.2).sustain(0).gain(0.55)

// UNCOMMENT to add pad
// $: note("<[c4,eb4,g4] [bb3,d4,f4]>")
//   .s("sawtooth").lpf(2000).attack(0.3).release(1)
//   .room(0.4).gain(0.25).orbit(2)

// UNCOMMENT for sidechain
// $: s("bd*4").bank("RolandTR909")
//   .duckorbit(2).duckattack(0.2).duckdepth(1).gain(0)

// UNCOMMENT for melody
// $: n("<0 ~ 3 ~ 5 ~ 7 ~>*2").scale("C4:minor:pentatonic")
//   .s("triangle").decay(0.15).sustain(0)
//   .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
//   .gain(0.2)
```

**Workflow:**
1. Evaluate the kick line → listen
2. Uncomment the hats line → evaluate again (Ctrl+Enter) → listen
3. Uncomment the clap → evaluate → listen
4. Continue adding elements...
5. To remove an element: comment it out or change `$:` to `// $:`
6. To break down: comment out several elements and re-evaluate

### Pros and Cons of Additive Arrangement

| ✅ Pros | ❌ Cons |
|---------|---------|
| Fully interactive | No fixed timing — section lengths are up to you |
| Can modify any element in real time | Requires re-evaluation to add/remove |
| Natural for live coding | Harder to reproduce exact same arrangement |

---

## 11.6 Transition Techniques

### Filter Sweeps

```js
// HPF sweep up = buildup (strip low end)
.hpf(saw.range(50, 5000).slow(8))

// LPF sweep down = breakdown (darken everything)
.lpf(saw.range(8000, 200).slow(8))

// LPF sweep up = drop anticipation (brighten)
.lpf(saw.range(200, 8000).slow(8))
```

### Reverb Wash

```js
// Reverb grows during breakdown
.room(saw.range(0, 0.9).slow(16))
.roomsize(sine.range(0.3, 0.9).slow(8))
```

### Degradation (Pattern Thinning)

```js
// Pattern gets sparser over time
.degradeBy(saw.range(0, 0.8).slow(8))
```

### Gain Fades

```js
// Fade out
.gain(saw.range(0.8, 0).slow(16))

// Fade in
.gain(saw.range(0, 0.8).slow(16))
```

### Pitch Risers

```js
// Rising tone for buildup anticipation
note(saw.range(40, 80).segment(64).slow(16))
  .s("sine").gain(0.2)
```

### Snare/Noise Builds

```js
// Snare roll builds (faster and louder)
s("sd").fast(saw.range(1, 16).slow(4))
  .bank("RolandTR909")
  .gain(saw.range(0.1, 0.8).slow(4))
```

### Combining Transition Techniques

```js
setcpm(128/4)

// 8-cycle buildup
$: s("bd*4").bank("RolandTR909")
$: s("hh*16").bank("RolandTR909")
  .hpf(saw.range(2000, 10000).slow(8))
  .gain(saw.range(0.3, 0.8).slow(8))
$: s("sd").fast(saw.range(1, 16).slow(8))
  .bank("RolandTR909")
  .gain(saw.range(0, 0.6).slow(8))
$: note(saw.range(48, 72).segment(32).slow(8)).s("sine")
  .gain(saw.range(0, 0.25).slow(8))
```

---

## 11.7 Creating Energy Dynamics

### Additive Energy (Build)

Each new element = more energy:

```
Kick → +Hats → +Clap → +Percussion → +Bass → +Pad → +Melody = PEAK
```

### Subtractive Energy (Breakdown)

Remove elements one by one:

```
PEAK → -Kick → -Bass → -Clap → -Percussion → Just pad + melody = LOW
```

### Parameter-Based Energy

Without adding/removing elements, change parameters:

| Parameter | Lower Energy | Higher Energy |
|-----------|-------------|---------------|
| Filter cutoff | Low (dark) | High (bright) |
| Distortion | None | High |
| Hi-hat density | `hh*4` | `hh*16` |
| Reverb amount | High (washy) | Low (dry, direct) |
| Degradation | High (sparse) | None (full) |
| Delay feedback | High (spacious) | Low (tight) |

---

## 11.8 Complete Track: Scripted Arrangement

Here's a full 4-minute Minimal Techno track using `arrange()`:

```js
setcpm(128/4)

// === SOUND DEFINITIONS ===
let kick = note("c1!4").s("sine")
  .penv(24).pdecay(.06).decay(.4).sustain(0)
  .distort("1.5:.6").gain(0.8)

let hats = s("hh*16").bank("RolandTR909")
  .gain(".5 .3 .8 .3").swingBy(1/6, 4).hpf(6000)

let clap = s("~ cp ~ cp").bank("RolandTR909")
  .room(0.15).gain(0.65)

let rim = s("rim(5,16)").bank("RolandTR909")
  .gain(0.3).pan(rand).delay(0.2).delaytime(0.1875)

let bass = note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
  .lpf(400).lpq(12).lpenv(4).lpa(.005).lpd(.12).lps(0)
  .ftype("ladder").decay(0.2).sustain(0).release(.05).gain(0.55)

let pad = note("<[c4,eb4,g4] [bb3,d4,f4] [ab3,c4,eb4] [g3,bb3,d4]>")
  .s("sawtooth")
  .lpf(sine.range(1200, 2500).slow(16)).lpq(2)
  .attack(0.4).sustain(0.5).release(1)
  .room(0.5).roomsize(0.7).gain(0.25).orbit(2)

let melody = n("<0 ~ 3 ~ 5 ~ 7 ~>*2")
  .scale("C4:minor:pentatonic").s("triangle")
  .decay(0.15).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .degradeBy(0.3).gain(0.2).orbit(2)

let duck = s("bd*4").bank("RolandTR909")
  .duckorbit(2).duckattack(0.2).duckdepth(1).gain(0)

let texture = s("crackle*2").density(0.03).gain(0.1)

let riser = note(saw.range(40, 72).segment(32).slow(8))
  .s("sine").gain(0.15)

// === ARRANGEMENT ===
arrange(
  // INTRO (16 bars) — kick emerges
  [4,  kick.gain(saw.range(0, 0.8).slow(4))],
  [12, kick],

  // BUILD (12 bars) — elements enter
  [4,  stack(kick, hats.gain(saw.range(0, 0.5).slow(4)), texture)],
  [4,  stack(kick, hats, clap, texture)],
  [4,  stack(kick, hats, clap, rim, texture)],

  // MAIN LOOP A (32 bars) — full groove
  [32, stack(kick, hats, clap, rim, bass, pad, duck, texture)],

  // BREAKDOWN (8 bars) — atmospheric
  [8,  stack(
    hats.lpf(sine.range(500, 8000).slow(8)).gain(0.25),
    pad.room(0.8).roomsize(0.9),
    melody,
    texture
  )],

  // BUILD 2 (8 bars) — riser + elements return
  [4,  stack(riser, pad, melody, texture)],
  [4,  stack(kick, hats, riser, texture)],

  // DROP / MAIN LOOP B (32 bars) — full energy with melody
  [32, stack(kick, hats, clap, rim, bass, pad, melody, duck, texture)],

  // OUTRO (16 bars) — elements fade
  [4,  stack(kick, hats, clap, bass.gain(saw.range(0.55, 0).slow(4)))],
  [4,  stack(kick, hats.gain(saw.range(0.5, 0).slow(4)))],
  [8,  kick.gain(saw.range(0.8, 0).slow(8))]
)
```

> **Total: ~128 bars = ~4 minutes at 128 BPM**

---

## 11.9 Practice Challenges

### Challenge 1: 32-Bar Mini-Track ⭐⭐
Using `arrange()`, create a 32-bar track with:
- 8 bars intro (kick only)  
- 8 bars full groove
- 8 bars breakdown (no kick)
- 8 bars full groove return

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
let kick = s("bd*4").bank("RolandTR909")
let hats = s("hh*8").bank("RolandTR909").gain(0.4)
let clap = s("~ cp ~ cp").bank("RolandTR909")

arrange(
  [8, kick],
  [8, stack(kick, hats, clap)],
  [8, stack(hats.lpf(sine.range(500,6000).slow(8)),
            s("~ cp ~ ~").bank("RolandTR909").room(0.6))],
  [8, stack(kick, hats, clap)]
)
```
</details>

### Challenge 2: Build a Transition ⭐⭐⭐
Create an 8-bar buildup using filter sweeps, a snare roll, and a riser:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
stack(
  s("bd*4").bank("RolandTR909"),
  s("hh*16").bank("RolandTR909")
    .hpf(saw.range(2000, 12000).slow(8))
    .gain(saw.range(0.3, 0.8).slow(8)),
  s("sd").fast(saw.range(1, 16).slow(8))
    .bank("RolandTR909").gain(saw.range(0, 0.5).slow(8)),
  note(saw.range(48, 72).segment(32).slow(8))
    .s("sine").gain(saw.range(0, 0.2).slow(8))
)
```
</details>

### Challenge 3: Additive Live Arrangement ⭐⭐
Write a complete track template using `$:` patterns with comments indicating when to add each element:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)

// === PHASE 1: Start with kick ===
$: s("bd*4").bank("RolandTR909")

// === PHASE 2: Add hats ===
$: s("hh*16").bank("RolandTR909").gain(".5 .3 .8 .3").hpf(6000)

// === PHASE 3: Add clap ===
$: s("~ cp ~ cp").bank("RolandTR909").room(0.15)

// === PHASE 4: Add bass ===
$: note("c2 ~ <eb2 f2> ~").s("sine").lpf(200).decay(.3).sustain(0)

// === PHASE 5: Add pad + sidechain ===
$: note("<[c4,eb4,g4] [bb3,d4,f4]>").s("sawtooth")
  .lpf(2000).attack(.3).release(1).room(.4).gain(.25).orbit(2)
$: s("bd*4").bank("RolandTR909")
  .duckorbit(2).duckattack(.2).duckdepth(1).gain(0)

// === PHASE 6: Add melody ===
$: n("<0 ~ 3 5>*2").scale("C4:minor:pentatonic").s("triangle")
  .decay(.15).sustain(0).delay(.4).delaytime(.1875).gain(.2)
```
</details>

---

## 11.10 Key Takeaways

1. Minimal Techno structure: **Intro → Build → Main → Breakdown → Drop → Outro**
2. At 128 BPM with `setcpm(128/4)`: **1 cycle = 1 bar = 4 beats**
3. **`arrange()`** = scripted, precise, reproducible arrangements
4. **`$:` patterns** = live, interactive, additive arrangements
5. **Transitions** use filter sweeps, reverb washes, degradation, gain fades, and risers
6. Energy comes from **adding/removing elements** AND **changing parameters**
7. **Intro/outro = just kick** (for DJ mixing)
8. **Breakdowns strip back** to atmospheric elements → anticipation
9. **Drops bring everything back** → release
10. A 4-minute track ≈ **128 bars** at 128 BPM

---

**Previous:** [← Module 10 — Pattern Manipulation & Variation](./Module_10_Pattern_Manipulation.md)
**Next:** [Module 12 — Live Performance Techniques →](./Module_12_Live_Performance.md)
