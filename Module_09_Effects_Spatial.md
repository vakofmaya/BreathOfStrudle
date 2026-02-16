# Module 9 — Effects & Spatial Processing

> **Goal:** Learn to place your sounds in space and add character using Strudel's effects: delay, reverb, distortion, bit crushing, phaser, panning, stereo widening, and — most importantly for Minimal Techno — **sidechain ducking** (the pumping effect).

---

## 9.1 The Effects Signal Chain

Recall from Module 4 where effects sit in the pipeline:

```
Source → Gain/ADSR → Filters → Coarse → Crush → Distort
→ Tremolo → Compressor → Pan → Phaser → Postgain
→ [Sends] → Delay / Reverb (per orbit) → Master
```

Effects are **single-use**: `.delay(0.5).delay(0.3)` — the second overrides the first. You can't chain two instances of the same effect.

---

## 9.2 Delay

Delay repeats a sound after a set time, creating an echo. It's the most important effect for Minimal Techno melodies — a few notes + delay = a full melodic texture.

### Basic Delay

```js
// delay(level) — 0 = no delay, 1 = full send
s("cp").delay(0.5)
```

### Full Control — Three Parameters

```js
// delay("level:time:feedback")
// level: how loud the echo (0–1)
// time: delay time in seconds (or fractions of a cycle)
// feedback: how many repeats (0 = one echo, 0.9 = many)

s("cp").delay("0.5:0.125:0.6")
```

### Individual Parameters

```js
s("cp")
  .delay(0.5)              // send level
  .delaytime(0.125)        // echo time in seconds
  .delayfeedback(0.6)      // repeat intensity
```

### Syncing Delay to Tempo

For techno at 128 BPM:
- One beat = 60/128 = **0.46875 seconds**
- Eighth note = **0.234375 seconds**
- Sixteenth note = **0.1171875 seconds**
- Dotted eighth = **0.3515625 seconds**

But since Strudel works in cycles and we set `setcpm(128/4)`:
- One cycle = 4 beats
- One beat = 1/4 cycle
- Eighth note = 1/8 cycle
- Sixteenth = 1/16 cycle

When using `delaytime`, the value is in **seconds**, so calculate:

```js
setcpm(128/4)
// At 128 BPM, one beat = 0.46875s
// 1/8 note = 0.234375s
// 3/16 note (dotted 8th) = 0.3515625s

// Common delay times for 128 BPM:
let beat = 60/128
let eighth = beat/2
let sixteenth = beat/4
let dotted8th = beat * 0.75

note("c4 ~ ~ ~").s("triangle")
  .decay(0.15).sustain(0)
  .delay(0.5)
  .delaytime(dotted8th)    // classic dotted-eighth delay
  .delayfeedback(0.5)
```

**Pre-calculated values at 128 BPM:**

| Subdivision | Seconds | Strudel |
|-------------|---------|---------|
| Quarter (1 beat) | 0.46875 | `.delaytime(0.46875)` |
| Eighth | 0.234375 | `.delaytime(0.234375)` |
| Dotted Eighth | 0.3515625 | `.delaytime(0.3515625)` |
| Sixteenth | 0.1171875 | `.delaytime(0.1171875)` |
| Triplet Eighth | 0.15625 | `.delaytime(0.15625)` |

### Using Simple Fractions

Alternatively, use fractional values which map to cycle fractions:

```js
// These are easier to remember
s("cp").delay(0.5).delaytime(1/8).delayfeedback(0.5)
s("cp").delay(0.5).delaytime(3/16).delayfeedback(0.5)
```

### Delay Patterns

Pattern the delay parameters for variation:

```js
setcpm(128/4)
note("c4 eb4 g4 c5").s("triangle")
  .decay(0.15).sustain(0)
  .delay("<0 0.3 0.5 0.7>")             // delay builds up
  .delaytime(0.1875)
  .delayfeedback("<0.3 0.5 0.7>")       // feedback changes
```

### Ping-Pong Effect with `jux`

`jux` applies a function to the right channel only — combined with delay, it creates a ping-pong effect:

```js
setcpm(128/4)
note("c4 ~ eb4 ~").s("triangle")
  .decay(0.15).sustain(0)
  .delay(0.4).delaytime(0.1875)
  .jux(rev)
```

> 💡 **Exercise:** Play a sparse 4-note melody. Add `.delay(0.5).delaytime(0.1875).delayfeedback(0.5)`. Listen to how the delay fills the space between notes. Try different delaytime values.

---

## 9.3 Reverb

Reverb simulates the reflections of sound in a physical space. It adds depth, width, and atmosphere.

### Basic Reverb

```js
// room(level) — send amount (0 = dry, 1 = full reverb)
note("c4 e4 g4 c5").s("triangle").room(0.5)
```

### Reverb Size

```js
// roomsize — how "big" the space sounds (0 = small, ~1 = huge)
note("c4 e4 g4 c5").s("triangle")
  .room(0.5)
  .roomsize(0.2)     // small room
  
note("c4 e4 g4 c5").s("triangle")
  .room(0.5)
  .roomsize(0.9)     // cathedral
```

### Reverb Tone Shaping

```js
// roomlp — low-pass filter on reverb (darken the tail)
// roomdim — high-frequency damping (simulate air absorption)

note("c3").s("sawtooth")
  .room(0.7).roomsize(0.9)
  .roomlp(3000)       // dark reverb
  .roomdim(4000)       // high frequencies die faster
```

### Reverb Recipes for Techno

```js
// Tight room — for claps/snares  
.room(0.15).roomsize(0.3)

// Medium hall — for pads
.room(0.4).roomsize(0.6).roomlp(4000)

// Cathedral — for breakdowns/transitions
.room(0.8).roomsize(0.95).roomlp(2000).roomdim(3000)

// Dark, atmospheric — for ambient layers
.room(0.6).roomsize(0.85).roomlp(1500).roomdim(2000)
```

### Patterning Reverb

```js
// Reverb builds up during a breakdown
note("[c4,eb4,g4]").s("sawtooth")
  .attack(0.3).release(1)
  .room(saw.range(0, 0.8).slow(16))      // reverb grows
  .roomsize(sine.range(0.3, 0.9).slow(8))
  .lpf(2000)
```

> 💡 **Exercise:** Take a clap pattern `s("~ cp ~ cp")` and try different reverb settings. Find a reverb that adds space without washing the clap out.

---

## 9.4 Orbits — Independent Effect Buses

**Orbits** are separate audio channels, each with their own delay and reverb settings. By default, everything goes to orbit 0.

Why does this matter? Because delay and reverb are **global per orbit**. If you put everything on orbit 0, the kick's reverb setting will affect the hi-hat's reverb too (last write wins per cycle).

### Using Orbits

```js
setcpm(128/4)

// Drums on orbit 0 (dry)
$: s("bd*4").bank("RolandTR909").orbit(0)
$: s("hh*16").bank("RolandTR909").orbit(0).gain(.4)

// Clap on orbit 1 (short reverb)
$: s("~ cp ~ cp").bank("RolandTR909")
  .room(0.2).roomsize(0.3)
  .orbit(1)

// Pad on orbit 2 (big reverb + delay)
$: note("<[c4,eb4,g4] [bb3,d4,f4]>")
  .s("sawtooth").lpf(2000)
  .attack(0.3).release(1)
  .room(0.5).roomsize(0.7)
  .delay(0.3).delaytime(0.1875)
  .orbit(2)
```

> **Rule of thumb:** Keep kicks and bass dry (orbit 0). Give percussion some room (orbit 1). Let pads and melodies swim in reverb (orbit 2).

---

## 9.5 Distortion & Waveshaping

Distortion adds harmonics, warmth, and aggression. Essential for giving synthesized kicks weight and making bass lines cut through.

### Soft Distortion — `distort()`

```js
// distort("amount:asymmetry")
// amount: intensity (1 = mild, 3+ = aggressive)
// asymmetry: 0-1 (0.5 = symmetric, 0 or 1 = asymmetric)

note("c1!4").s("sine").penv(24).pdecay(.06)
  .decay(.4).sustain(0)
  .distort("1.5:.5")       // mild, symmetric
```

```js
// Increasing distortion
note("c2").s("sawtooth")
  .distort("<0 1 2 4 8>")
```

### Bit Crushing — `crush()`

Reduces the bit depth of the audio. Lower values = more crunchy:

```js
// 16 = no effect (full quality)
// 8  = noticeable crunch
// 4  = heavy lo-fi
// 1  = extreme destruction

s("hh*8").crush("<16 8 6 4 2>")
```

### Sample Rate Reduction — `coarse()`

Reduces the sample rate, creating aliasing artifacts:

```js
// 1 = no effect
// Higher = more extreme reduction

s("bd sd, hh*8").coarse("<1 4 8 16 32>")
```

### Distortion Recipes

```js
// Warm kick — subtle saturation
note("c1!4").s("sine").penv(24).pdecay(.06)
  .decay(.4).sustain(0)
  .distort("1.5:.6")

// Gritty bass — medium drive
note("c2!4").s("sawtooth").lpf(600)
  .decay(0.2).sustain(0)
  .distort("3:.5")

// Lo-fi hats — bit crush
s("hh*16").bank("RolandTR909")
  .crush(8).gain(0.4)

// Industrial percussion — extreme
s("rim(5,8)").bank("RolandTR909")
  .crush(4).coarse(8)
  .gain(0.3)
```

> 💡 **Exercise:** Take a clean kick `note("c1!4").s("sine").penv(24).pdecay(.06).decay(.4).sustain(0)` and add `.distort()` with increasing values: 0, 1, 2, 4, 8. At what point does it start sounding "too much"?

---

## 9.6 Phaser

The phaser creates a sweeping, jet-engine-like effect by summing the signal with a phase-shifted copy of itself:

```js
// phaser(speed) — rate in Hz
s("bd sd, hh*8")
  .phaser(2)

// With depth and center frequency
s("bd sd, hh*8")
  .phaser(0.5)             // slow sweep
  .phaserdepth(0.8)        // deep modulation
  .phasercenter(1000)      // center frequency
  .phasersweep(2000)       // sweep range
```

### Phaser on Pads

```js
setcpm(128/4)
note("<[c4,eb4,g4] [bb3,d4,f4]>")
  .s("sawtooth").lpf(2500)
  .attack(0.3).release(1)
  .phaser(0.3).phaserdepth(0.6)
  .room(0.3)
```

---

## 9.7 Tremolo

Tremolo rapidly modulates the volume, creating a pulsing effect:

```js
// tremolo("speed:depth")
note("c3").s("sawtooth").lpf(2000)
  .tremolo("4:0.5")         // 4 Hz, 50% depth

// Faster tremolo for choppy effect
note("c3").s("sawtooth").lpf(2000)
  .tremolo("8:1")            // 8 Hz, full depth
```

---

## 9.8 Sidechain Ducking ★

This is **the** defining effect of Minimal Techno. The "pumping" feeling where everything ducks when the kick hits — pads breathe in and out with the kick rhythm.

### How It Works

1. You have a **source** pattern (e.g., a pad) on a specific orbit
2. A **trigger** pattern (usually the kick) ducks that orbit
3. When the trigger fires, the source's volume drops and then recovers

### Implementation

```js
setcpm(128/4)

// PAD — on orbit 2
$: note("<[c4,eb4,g4] [bb3,d4,f4]>")
  .s("sawtooth").lpf(2000)
  .attack(0.3).release(1)
  .room(0.4)
  .gain(0.3)
  .orbit(2)                     // ← target orbit

// KICK — normal audible kick
$: s("bd*4").bank("RolandTR909")
  .gain(0.85)

// SIDECHAIN TRIGGER — ducks orbit 2
$: s("bd*4").bank("RolandTR909")
  .duckorbit(2)                  // which orbit to duck
  .duckattack(0.2)               // recovery time (seconds)
  .duckdepth(1)                  // ducking amount (0-1, 1 = full)
  .gain(0)                       // silent — only triggers the duck
```

### Sidechain Parameters

| Parameter | What it does | Typical values |
|-----------|-------------|----------------|
| `duckorbit(N)` | Which orbit to duck | `2` (keep drums on 0, pad on 2) |
| `duckattack(t)` | Recovery time in seconds | `0.15` (snappy) – `0.3` (pumpy) |
| `duckdepth(d)` | How much volume drops (0–1) | `0.8` (subtle) – `1` (full duck) |

### Varying the Pump

```js
// Short recovery = snappy, punchy
.duckattack(0.1)

// Medium recovery = standard techno pump
.duckattack(0.2)

// Long recovery = deep, breathing pump
.duckattack(0.35)

// Partial ducking (more subtle)
.duckdepth(0.6)

// Full ducking (dramatic)
.duckdepth(1)
```

### Sidechain on Multiple Elements

You can duck multiple orbits, or put multiple elements on the same ducked orbit:

```js
setcpm(128/4)

// Pad + Melody both on orbit 2 → both get pumped
$: note("<[c4,eb4,g4] [bb3,d4,f4]>")
  .s("sawtooth").lpf(2000)
  .attack(0.3).release(1)
  .gain(0.25).orbit(2)

$: n("<0 ~ 3 ~ 5 ~ 7 ~>*2")
  .scale("C4:minor:pentatonic")
  .s("triangle")
  .decay(0.15).sustain(0)
  .delay(0.4).delaytime(0.1875)
  .gain(0.2).orbit(2)

// Kick
$: s("bd*4").bank("RolandTR909").gain(0.85)

// Duck trigger
$: s("bd*4").bank("RolandTR909")
  .duckorbit(2).duckattack(0.25).duckdepth(1)
  .gain(0)
```

### Half-Time Sidechain

Use a different pattern for the duck trigger than the actual kick — e.g., duck on every other beat for a half-time pump:

```js
$: s("bd ~ bd ~")
  .duckorbit(2).duckattack(0.3).duckdepth(1)
  .gain(0)
```

> 💡 **Exercise:**
> 1. Create a pad on orbit 2
> 2. Add a four-on-the-floor kick
> 3. Add a duck trigger with `.duckattack(0.2).duckdepth(1)`
> 4. Experiment: change `duckattack` from 0.1 to 0.4, hear the pump change

---

## 9.9 Panning & Stereo

### Pan — Left/Right Placement

```js
// pan: 0 = hard left, 0.5 = center, 1 = hard right
s("hh*8").pan("0 0.25 0.5 0.75 1 0.75 0.5 0.25")
```

```js
// Random panning — each event in a different position
s("rim(5,16)").bank("RolandTR909")
  .pan(rand)
  .gain(0.3)
```

```js
// Sine-driven auto-pan
note("c4 eb4 g4 c5").s("triangle")
  .pan(sine.range(0.2, 0.8).slow(4))
```

### `jux` — Stereo Function Application

`jux` applies a function to the **right channel only**, creating stereo width:

```js
// Right channel is reversed
s("bd sd [~ bd] sd, hh*8").jux(rev)
```

```js
// Right channel plays at double speed
s("hh*8").jux(x => x.fast(2))
```

### `juxBy` — Partial Stereo Spread

```js
// juxBy(amount, function) — amount controls stereo width (0-1)
s("hh*8").juxBy(0.5, x => x.speed(1.5))
```

### Stereo Tips for Minimal Techno

1. **Keep kicks and bass in the center** — `pan(0.5)` or don't pan at all
2. **Hi-hats can have subtle panning** — `pan(sine.range(0.4, 0.6))` 
3. **Percussion benefits from random panning** — `pan(rand)`
4. **Pads can be widened** with `jux` or subtle pan movement
5. **Melodies: use pan for movement** — `pan(sine.range(0.3, 0.7).slow(4))`

```js
setcpm(128/4)
// Example: well-panned mix
$: s("bd*4").bank("RolandTR909")                                    // center
$: s("hh*16").bank("RolandTR909").gain(.4).pan(sine.range(.4,.6))   // subtle
$: s("~ cp ~ cp").bank("RolandTR909").room(0.15)                    // center
$: s("rim(5,16)").bank("RolandTR909").gain(0.3).pan(rand)           // wide
$: note("c2 ~ <eb2 f2> ~").s("sine").lpf(200)                      // center
$: note("<[c4,eb4,g4] [bb3,d4,f4]>").s("sawtooth")
  .lpf(2000).attack(0.3).release(1)
  .juxBy(0.3, rev).room(0.4).gain(0.2)                              // wide pad
```

> 💡 **Exercise:** Take a drum pattern and apply different panning strategies. Pan the rimshot with `rand`, keep the kick centered, and slowly auto-pan the hi-hats.

---

## 9.10 Complete Effects Reference

| Effect | Function | Key Parameters | Character |
|--------|----------|---------------|-----------|
| **Delay** | `.delay()` | `.delaytime()`, `.delayfeedback()` | Echo, rhythm |
| **Reverb** | `.room()` | `.roomsize()`, `.roomlp()`, `.roomdim()` | Space, depth |
| **Distortion** | `.distort()` | amount:asymmetry | Warmth, grit |
| **Bit Crush** | `.crush()` | 1–16 (16=clean) | Lo-fi, crunchy |
| **Coarse** | `.coarse()` | 1+ (1=clean) | Aliasing, digital |
| **Phaser** | `.phaser()` | `.phaserdepth()`, `.phasercenter()` | Sweeping, jet |
| **Tremolo** | `.tremolo()` | speed:depth | Pulsing, choppy |
| **Sidechain** | `.duckorbit()` | `.duckattack()`, `.duckdepth()` | Pumping |
| **Pan** | `.pan()` | 0–1 | Stereo placement |
| **Jux** | `.jux()` | function | Stereo widening |
| **Compressor** | `.compressor()` | threshold:ratio:knee:attack:release | Dynamics |

---

## 9.11 Practice Challenges

### Challenge 1: Delay Echo
Create a sparse 2-note melody where delay fills the gaps. The result should feel like a full melody even though only 2 notes are played per cycle:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
note("c4 ~ ~ ~, ~ ~ g4 ~").s("triangle")
  .decay(0.15).sustain(0)
  .delay(0.6).delaytime(0.1875).delayfeedback(0.6)
```
</details>

### Challenge 2: Reverb Wash
Create a pad that transitions from dry to fully wet reverb over 8 cycles:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
note("<[c4,eb4,g4]>").s("sawtooth")
  .attack(0.3).release(1)
  .lpf(2000)
  .room(saw.range(0, 0.9).slow(8))
  .roomsize(sine.range(0.3, 0.9).slow(8))
```
</details>

### Challenge 3: Sidechain Pump
Build a pad with a strong sidechain pump from the kick. The pump should be clearly audible:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
$: note("[c4,eb4,g4]").s("sawtooth")
  .lpf(2000).attack(0.2).release(1)
  .room(0.3).gain(0.3).orbit(2)

$: s("bd*4").bank("RolandTR909").gain(0.85)

$: s("bd*4").bank("RolandTR909")
  .duckorbit(2).duckattack(0.25).duckdepth(1).gain(0)
```
</details>

### Challenge 4: Stereo Mix
Create a multi-element pattern where each element has appropriate panning:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
$: s("bd*4").bank("RolandTR909")                  // center
$: s("hh*16").bank("RolandTR909").gain(.4)
  .pan(sine.range(0.35, 0.65).slow(2))            // subtle sway
$: s("~ cp ~ cp").bank("RolandTR909").room(0.15)  // center
$: s("rim(5,16)").bank("RolandTR909")
  .gain(0.25).pan(rand)                            // wide
  .delay(0.2).delaytime(0.1875)
```
</details>

### Challenge 5: Effects Chain
Design a bass sound that uses distortion, filter, AND delay together:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
  .lpf(500).lpq(10).ftype("ladder")
  .lpenv(4).lpd(0.12).lps(0)
  .decay(0.2).sustain(0)
  .distort("2:.5")
  .delay(0.15).delaytime(0.1875).delayfeedback(0.3)
```
</details>

---

## 9.12 Key Takeaways

1. **Delay** is essential for Minimal Techno — a few notes + delay = full texture
2. Sync delay to tempo: at 128 BPM, dotted eighth ≈ 0.35s, eighth ≈ 0.23s
3. **Reverb** adds space — use `.room()` + `.roomsize()`, shape with `.roomlp()`
4. Use **orbits** to give different elements their own reverb/delay settings
5. **Distortion** warms up kicks and basses — `.distort("1.5:.5")` is a good starting point
6. **Bit crush** (`crush`) and **sample rate reduction** (`coarse`) for lo-fi character
7. **Sidechain ducking** = the Minimal Techno pump effect
   - Put pad on orbit 2 → `.duckorbit(2).duckattack(0.2).duckdepth(1)`
8. **Keep low-end centered** — pan only mid/high frequency elements
9. `jux(fn)` creates stereo width by applying a function to the right channel only
10. All effect parameters can be **patterned** with signals for evolving textures

---

**Previous:** [← Module 8 — Writing Chords & Melodies](./Module_08_Chords_Melodies.md)
**Next:** [Module 10 — Pattern Manipulation & Variation →](./Module_10_Pattern_Manipulation.md)
