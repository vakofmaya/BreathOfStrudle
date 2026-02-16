# Module 10 — Pattern Manipulation & Variation

> **Goal:** Learn to transform, modulate, and evolve your patterns using Strudel's powerful pattern functions — time modifiers, random/conditional modifiers, continuous signals for modulation, and layering constructs. These techniques turn static loops into living, breathing music.

---

## 10.1 Why Patterns Need Variation

A static loop gets boring after 8 bars. Minimal Techno thrives on **subtle, evolving change** — small modifications that keep the listener engaged without disrupting the groove. Strudel gives you dozens of tools for this.

The categories:

| Category | What it does | Example |
|----------|-------------|---------|
| **Time modifiers** | Change speed, direction, rotation | `slow`, `fast`, `rev`, `iter` |
| **Random modifiers** | Add unpredictability | `sometimes`, `degrade`, `chooseCycles` |
| **Conditional modifiers** | Apply changes on specific cycles | `lastOf`, `firstOf`, `when` |
| **Continuous signals** | Smoothly modulate parameters | `sine`, `saw`, `rand`, `perlin` |
| **Layering & sequencing** | Combine patterns over time | `stack`, `cat`, `arrange` |

---

## 10.2 Time Modifiers

### `slow(n)` — Stretch Over Multiple Cycles

```js
// Normal: plays every cycle
note("c3 eb3 g3 bb3")

// slow(2): takes 2 cycles to complete
note("c3 eb3 g3 bb3").slow(2)

// slow(4): takes 4 cycles
note("c3 eb3 g3 bb3").slow(4)
```

**Use for:** Longer chord progressions, evolving filter sweeps, macro-level changes.

### `fast(n)` — Speed Up Within a Cycle

```js
// Normal
s("bd sd bd sd")

// fast(2): double speed — 8 events in one cycle
s("bd sd bd sd").fast(2)

// fast(0.5): half speed — 2 events in one cycle
s("bd sd bd sd").fast(0.5)
```

### `rev()` — Reverse Pattern Order

```js
s("bd sd hh cp").rev()
// becomes: cp hh sd bd
```

### `palindrome()` — Forward Then Backward

```js
// Plays forward one cycle, backward the next
s("bd sd hh cp").palindrome()
// Cycle 1: bd sd hh cp
// Cycle 2: cp hh sd bd
// Cycle 3: bd sd hh cp
// ...
```

### `iter(n)` — Rotate Pattern Each Cycle

Shifts the starting point of the pattern by one position each cycle:

```js
s("bd hh sd oh").iter(4)
// Cycle 1: bd hh sd oh
// Cycle 2: hh sd oh bd
// Cycle 3: sd oh bd hh
// Cycle 4: oh bd hh sd
```

This creates subtle variation from a simple pattern — the events stay the same but their position shifts.

```js
// Great for minimal techno percussion
setcpm(128/4)
s("bd [~ sd] hh [sd cp]").iter(4)
  .bank("RolandTR909")
```

### `ply(n)` — Repeat Each Event

```js
// Each event plays n times
s("bd sd hh cp").ply(2)
// becomes: bd bd sd sd hh hh cp cp (all in same cycle)

// Pattern the ply amount
s("bd sd hh cp").ply("<1 2 1 3>")
```

### `early(t)` / `late(t)` — Nudge Timing

Shift events earlier or later by a fraction of a cycle:

```js
// Nudge events early by 1/8 cycle
s("hh*4").early(0.125)

// Humanize: tiny random lateness
s("hh*16").late(0.02)
```

### `segment(n)` — Quantize to N Steps

Forces a continuous signal into N discrete steps per cycle:

```js
// Turn a continuous sine into 8 steps per cycle
note(sine.segment(8).range(48, 72))
```

> 💡 **Exercise:** Take `s("bd sd hh cp")` and apply each modifier one at a time: `.rev()`, `.palindrome()`, `.iter(4)`, `.ply(2)`. Listen to what each one does.

---

## 10.3 Euclidean Rhythms (Revisited)

We covered Euclidean basics in Module 2. Here's how to use them as a pattern modifier:

### `.euclid(pulses, steps, offset)`

```js
// Apply Euclidean distribution to existing pattern
note("c2").euclid(3, 8)

// With rotation
note("c2").euclid(3, 8, 1)
```

### `.struct()` — Binary Rhythm Mask

Apply a rhythmic pattern to any sound:

```js
// x = play, ~ = rest
note("c2").struct("x ~ x ~ x ~ ~ x")

// Or with Euclidean
note("c2").struct("[x(3,8)]")
```

---

## 10.4 Swing

Swing shifts every other subdivision slightly late, creating a shuffle/groove:

```js
// swingBy(amount, subdivision)
// amount: 0 = straight, 1/3 = medium swing, 1/2 = extreme
// subdivision: which grid to swing (typically 4 for 16th notes)

setcpm(128/4)
s("hh*16").bank("RolandTR909")
  .swingBy(1/6, 4)         // subtle shuffle
```

```js
// More swing
s("hh*16").bank("RolandTR909").swingBy(1/3, 4)

// Less swing
s("hh*16").bank("RolandTR909").swingBy(1/8, 4)
```

> **For Minimal Techno:** Use swing values between `1/8` and `1/4` for subtle groove. Higher values make it too "jazzy."

---

## 10.5 Random Modifiers

### `degrade()` — Random Event Removal

```js
// Remove ~50% of events randomly
s("hh*16").degrade()
```

### `degradeBy(amount)` — Controlled Removal

```js
s("hh*16").degradeBy(0.3)    // remove 30%
s("hh*16").degradeBy(0.7)    // remove 70% — very sparse
```

### `sometimes(fn)` — 50% Chance Per Event

```js
// Sometimes play an octave higher
note("c3 eb3 g3 bb3").sometimes(x => x.add(note(12)))

// Sometimes speed up
s("hh*8").sometimes(x => x.speed(1.5))

// Sometimes apply distortion
s("bd*4").sometimes(x => x.distort(3))
```

### Probability Variants

```js
.almostAlways(fn)    // ~90% chance
.often(fn)           // ~75% chance
.sometimes(fn)       // ~50% chance
.rarely(fn)          // ~25% chance
.almostNever(fn)     // ~10% chance
```

```js
// Rarely reverse the pattern
s("bd sd [~ bd] sd").rarely(x => x.rev())

// Often crush hi-hats
s("hh*8").often(x => x.crush(4))
```

### `chooseCycles(...)` — Random Per Cycle

```js
// Each cycle randomly picks one of these patterns
chooseCycles(
  s("bd sd bd sd"),
  s("bd [~ sd] bd [sd cp]"),
  s("bd bd [~ bd] sd")
).bank("RolandTR909")
```

> 💡 **Exercise:** Take a steady hi-hat `s("hh*16")` and gradually add randomness:
> 1. `.degradeBy(0.1)` — very subtle
> 2. `.degradeBy(0.3)` — noticeable gaps
> 3. Add `.sometimes(x => x.speed(1.5))` — pitch variation
> 4. Add `.rarely(x => x.crush(4))` — occasional lo-fi

---

## 10.6 Conditional Modifiers

These apply a transformation only on **specific cycles**, creating structured variation.

### `lastOf(n, fn)` — Apply on the Last Cycle of N

```js
// Every 4th cycle, reverse the pattern (fills!)
s("hh*8").lastOf(4, x => x.rev())

// Every 8th cycle, double the speed
s("bd sd, hh*8").lastOf(8, x => x.fast(2))
```

### `firstOf(n, fn)` — Apply on the First Cycle of N

```js
// Every 3rd cycle, play at double speed
s("bd sd cp hh").firstOf(3, x => x.fast(2))
```

### `every(n, fn)` — Same as `lastOf`

```js
// Alias for lastOf
s("hh*8").every(4, x => x.ply(2))
```

### `when(condition, fn)` — Apply When Condition is True

```js
// Apply on alternating cycles using a pattern
note("c3 eb3 g3").when("<0 1>/2", x => x.transpose(7))

// Apply based on a signal
s("hh*8").when("<1 0 0 1>", x => x.speed(2))
```

### `chunk(n, fn)` — Rotate Function Through Pattern Sections

```js
// Divide pattern into 4 chunks, apply fn to one chunk at a time
n("0 1 2 3 4 5 6 7")
  .chunk(4, x => x.add(7))   // shift one quarter up by 7 each cycle
  .scale("A:minor")
  .s("triangle")
```

### Building Fills and Breaks

```js
setcpm(128/4)
// Every 4th cycle: hihat fill
$: s("hh*8").bank("RolandTR909")
  .gain(".5 .3 .8 .3")
  .lastOf(4, x => x.ply(2).gain(0.8))         // fill: 32nd notes

// Every 8th cycle: ride cymbal replaces hihats
$: s("hh*8").bank("RolandTR909")
  .lastOf(8, x => x.s("rd"))                   // swap sound

// Every 4th cycle: reverse the bass
$: note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
  .lpf(500).decay(0.2).sustain(0)
  .lastOf(4, x => x.rev())
```

> 💡 **Exercise:** Create a drum beat that has a fill every 4th cycle using `.lastOf(4, fn)`. Try different fill techniques: `.ply(2)`, `.fast(2)`, `.rev()`, `.speed(2)`.

---

## 10.7 Continuous Signals for Modulation

Signals are **continuous values** that change over time. Use them to modulate any parameter for organic, evolving textures.

### Available Signals

| Signal | Shape | Character |
|--------|-------|-----------|
| `sine` | Smooth wave | Gentle, predictable oscillation |
| `cosine` | Shifted sine | Same as sine but offset by 90° |
| `saw` | Ramp up | Gradual build, sudden reset |
| `tri` | Triangle | Linear up-down |
| `square` | On/off | Binary switching |
| `rand` | Random | New random value each event |
| `irand(n)` | Integer random | Random integer 0 to n-1 |
| `perlin` | Perlin noise | Smooth, organic random |

### `.range(min, max)` — Map to Useful Values

Signals output 0–1 by default. Use `.range()` to map to any range:

```js
// Filter cutoff sweeps from 200 to 4000 Hz
note("c2").s("sawtooth")
  .lpf(sine.range(200, 4000).slow(8))

// Gain ramps from 0 to 1
s("hh*16").gain(saw.range(0, 1).slow(4))
```

### `.slow(n)` / `.fast(n)` — Signal Speed

```js
// Slow LFO: completes one cycle over 16 pattern cycles
note("c2").s("sawtooth")
  .lpf(sine.range(200, 4000).slow(16))

// Fast LFO: 4 oscillations per cycle
note("c2").s("sawtooth")
  .lpf(sine.range(200, 4000).fast(4))
```

### Signal by Signal

**Sine — Smooth Modulation:**
```js
setcpm(128/4)
note("c2").s("sawtooth")
  .lpf(sine.range(200, 4000).slow(8))
  .lpq(sine.range(0, 15).slow(16))
  ._scope()
```

**Saw — Buildups and Drops:**
```js
// Filter builds up then resets
note("c2!4").s("sawtooth")
  .lpf(saw.range(200, 4000).slow(4))
```

**Rand — Per-Event Randomness:**
```js
// Random cutoff each hihat
s("hh*16").bank("RolandTR909")
  .lpf(rand.range(2000, 10000))
  .gain(rand.range(0.2, 0.8))
```

**irand — Integer Randomness:**
```js
// Random scale degree
n(irand(8).segment(8)).scale("C4:minor:pentatonic")
  .s("triangle")
```

**Perlin — Organic Random:**
```js
// Smooth, natural-sounding filter movement
note("c2").s("sawtooth")
  .lpf(perlin.range(300, 3000).slow(4))
  ._scope()
```

### Common Modulation Targets

```js
// Filter cutoff
.lpf(sine.range(200, 4000).slow(8))

// Resonance
.lpq(sine.range(0, 15).slow(16))

// Gain (volume swell)
.gain(saw.range(0.2, 1).slow(4))

// Pan (auto-pan)
.pan(sine.range(0.3, 0.7).slow(4))

// Delay send (delay builds up)
.delay(saw.range(0, 0.6).slow(8))

// Reverb send
.room(sine.range(0, 0.8).slow(16))

// Speed / pitch
.speed(sine.range(0.8, 1.2).slow(8))

// FM amount
.fm(sine.range(0, 8).slow(8))

// Degradation (pattern gets sparser)
.degradeBy(saw.range(0, 0.9).slow(16))
```

> 💡 **Exercise:** Take a pad sound and modulate THREE parameters simultaneously with different signals at different speeds. For example: filter with sine, reverb with saw, pan with perlin.

---

## 10.8 Layering Constructs

### `stack()` — Play Simultaneously

```js
stack(
  s("bd*4"),
  s("hh*8").gain(0.4),
  s("~ cp ~ cp")
).bank("RolandTR909")
```

### `cat()` — Play One Per Cycle

```js
// Cycle 1: just kick, Cycle 2: kick+hat, etc.
cat(
  s("bd*4"),
  s("bd*4, hh*8"),
  s("bd*4, hh*8, ~ cp ~ cp")
).bank("RolandTR909")
```

### `seq()` — Same as `cat`

```js
seq(
  s("bd*4"),
  s("bd*4, hh*8")
)
```

### `arrange()` — Multi-Cycle Sequencing

The most powerful tool for structuring a track:

```js
setcpm(128/4)
arrange(
  [4, s("bd*4")],                           // 4 cycles: kick only
  [4, s("bd*4, hh*8")],                     // 4 cycles: + hats
  [4, s("bd*4, hh*8, ~ cp ~ cp")],          // 4 cycles: + clap
  [8, stack(                                 // 8 cycles: full groove
    s("bd*4"),
    s("hh*8").gain(0.4),
    s("~ cp ~ cp"),
    note("c2 ~ <eb2 f2> ~").s("sine")
  )]
).bank("RolandTR909")
```

### `$:` — Labeled Patterns (Live Coding)

For live coding, use `$:` labels instead of `stack/arrange`:

```js
setcpm(128/4)
$: s("bd*4").bank("RolandTR909")
$: s("hh*16").bank("RolandTR909").gain(0.4)
$: s("~ cp ~ cp").bank("RolandTR909")
$: note("c2 ~ <eb2 f2> ~").s("sine").lpf(200)
```

Each `$:` line runs independently. You can modify and re-evaluate any single line without affecting others.

---

## 10.9 Combining Techniques — Real-World Examples

### Example 1: Evolving Hi-Hat

```js
setcpm(128/4)
s("hh*16").bank("RolandTR909")
  .gain(".5 .3 .8 .3")
  .swingBy(1/6, 4)
  .sometimes(x => x.speed(1.5))
  .lastOf(4, x => x.ply(2))
  .degradeBy(perlin.range(0, 0.3).slow(8))
  .hpf(sine.range(4000, 10000).slow(16))
```

### Example 2: Breathing Bass

```js
setcpm(128/4)
note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
  .lpf(sine.range(300, 1500).slow(8))
  .lpq(sine.range(5, 15).slow(12))
  .ftype("ladder")
  .decay(0.2).sustain(0)
  .lastOf(4, x => x.rev())
  .rarely(x => x.add(note(12)))
```

### Example 3: Generative Percussion

```js
setcpm(128/4)
s("rim(5,16)").bank("RolandTR909")
  .iter(4)
  .gain(rand.range(0.2, 0.5))
  .pan(rand)
  .delay(0.2).delaytime(0.1875)
  .sometimes(x => x.speed(1.5))
  .lastOf(8, x => x.fast(2))
```

### Example 4: Building Tension

```js
setcpm(128/4)
// 16-cycle tension builder
note("c2!4").s("sawtooth")
  .lpf(saw.range(100, 6000).slow(16))        // filter opens over 16 cycles
  .lpq(saw.range(0, 15).slow(16))            // resonance builds
  .decay(0.2).sustain(0)
  .distort(saw.range(0, 4).slow(16))         // distortion builds
  .gain(saw.range(0.3, 0.8).slow(16))        // volume rises
```

---

## 10.10 Practice Challenges

### Challenge 1: Evolving Loop
Create a 4-beat drum loop that uses `.iter(4)` so the starting point shifts each cycle:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
s("bd [~ hh] sd [hh oh]").iter(4)
  .bank("RolandTR909").cut(1)
```
</details>

### Challenge 2: Conditional Fill
Build a hi-hat pattern that plays a fill (double speed) every 4th cycle:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
s("hh*8").bank("RolandTR909")
  .gain(".5 .3 .7 .3")
  .lastOf(4, x => x.ply(2).gain(0.8))
```
</details>

### Challenge 3: Signal Modulation
Create a pad where the filter cutoff, reverb amount, and pan all move with different signals:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
note("[c4,eb4,g4]").s("sawtooth")
  .lpf(sine.range(800, 3000).slow(8))
  .room(saw.range(0.1, 0.6).slow(16))
  .pan(perlin.range(0.3, 0.7).slow(4))
  .attack(0.3).release(1)
  .gain(0.25)
```
</details>

### Challenge 4: Probability Chain
Apply a chain of probability modifiers to create an evolving pattern:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
s("hh*16").bank("RolandTR909")
  .gain(".6 .3 .8 .3")
  .sometimes(x => x.speed(1.5))
  .rarely(x => x.crush(4))
  .degradeBy(0.15)
  .lastOf(4, x => x.rev())
```
</details>

### Challenge 5: 16-Cycle Build
Create a pattern that builds in intensity over 16 cycles using saw signals:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
s("hh*16").bank("RolandTR909")
  .gain(saw.range(0.1, 0.8).slow(16))
  .hpf(saw.range(10000, 2000).slow(16))    // opens up
  .degradeBy(saw.range(0.8, 0).slow(16))   // fills in
```
</details>

---

## 10.11 Key Takeaways

1. **Time modifiers** change how patterns flow: `slow`, `fast`, `rev`, `iter`, `ply`, `palindrome`
2. **`iter(n)`** is a Minimal Techno secret weapon — subtle rotation creates variation from nothing
3. **Swing** (`swingBy(1/6, 4)`) adds human groove to mechanical patterns
4. **Random modifiers** (`sometimes`, `degradeBy`, `rarely`) add unpredictability
5. **Conditional modifiers** (`lastOf`, `firstOf`, `when`) create structured fills and breaks
6. **Continuous signals** (`sine`, `saw`, `perlin`, `rand`) modulate any parameter for organic evolution
7. **`.range(min, max)`** maps 0–1 signals to useful parameter ranges
8. **`.slow(n)`** on signals controls modulation speed (higher = slower)
9. **`stack`** = parallel, **`cat`** = sequential, **`arrange`** = multi-cycle structure
10. **`$:` patterns** are the live coding equivalent — free-form, independently modifiable

---

**Previous:** [← Module 9 — Effects & Spatial Processing](./Module_09_Effects_Spatial.md)
**Next:** [Module 11 — Structuring a Full Track →](./Module_11_Track_Structure.md)
