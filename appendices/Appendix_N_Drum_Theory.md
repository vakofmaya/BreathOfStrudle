# Appendix N — The Science of Rhythm

> **Everything you need to know about drums and rhythm — from the physics of why beats make you move, to Euclidean algorithms, ghost notes, polyrhythms, and genre-specific drum recipes you can paste into Strudel.**

This appendix goes beyond Module 5's practical introduction to drum machines and samples. Here, we explore *why* certain rhythmic patterns feel good, how to build sophisticated grooves from first principles, the mathematics behind Euclidean rhythms, and how every electronic genre treats its drums differently.

> [!NOTE]
> **This appendix is reference material, not a prerequisite.** You can build great beats with just Module 5's knowledge. Come here when you want to understand the *why* behind rhythm, or when you need drum pattern ideas for a specific genre.

---

## N.1 Why Rhythm Works — The Physics of Groove

Before we talk about drum *patterns*, let's understand why rhythm feels good at all. The answer isn't cultural preference — it's neuroscience.

### Entrainment — Your Brain Locks to the Beat

When you hear a steady pulse, your brain's motor cortex begins to **entrain** — it synchronizes neural oscillations to the beat. This is why you tap your foot without thinking. Your brain is literally *predicting* the next beat and firing neurons in advance.

```
What your brain does with rhythm:

Sound:  💥 ─── ─── 💥 ─── ─── 💥 ─── ─── 💥
Brain:  ⚡ ─── ─── ⚡ ─── ─── ⚡ ─── ─── ⚡     ← locked on
Body:   🦶 ─── ─── 🦶 ─── ─── 🦶 ─── ─── 🦶     ← tapping

After a few beats, your brain PREDICTS the next hit
before it happens. This prediction = groove.
```

### The Tempo Sweet Spot

Different tempos activate different neural responses:

| BPM Range | Feel | Why | Genre |
|-----------|------|-----|-------|
| 60–80 | Relaxed, meditative | Close to resting heart rate (~72 BPM) | Ambient, lo-fi |
| 90–110 | Walking, groovy | Matches natural walking cadence | Hip-hop, downtempo |
| 118–128 | Dancing, entraining | Optimal for sustained movement — fast enough to engage, slow enough to sustain | **House, Techno** |
| 130–145 | Driving, energetic | Pushes the body into higher intensity | Hard techno, trance |
| 170–180 | Frenetic, half-time feel | Too fast to feel every beat — brain "halves" it | Drum & Bass |

> **Why 120–128 BPM dominates dance music:** This range maps almost perfectly to the natural frequency of a relaxed, rhythmic body movement (stepping, swaying). The brain entrains most easily here. It's not an accident that techno, house, and most dance music lives in this range — it's physiology.

### Syncopation and Dopamine

When rhythm is **perfectly predictable**, it's boring — your brain gets no reward for predicting correctly because the prediction is trivial. When it's **perfectly random**, your brain can't lock on — it gives up.

The sweet spot is **syncopation**: mostly predictable with just enough surprise to trigger a dopamine reward.

```
Boring (no surprise):        💥 💥 💥 💥 💥 💥 💥 💥
Random (no pattern):         💥 ── 💥 💥 ── ── 💥 ──
                             Can't predict = frustrating

⭐ Syncopated (just right):  💥 ── 💥 ── 💥 ── ── 💥
                             Expected a hit on beat 8...
                             Got it on the "&" of 7!
                             Brain: 🎯 dopamine!
```

This is why the best drum patterns aren't just "beats on the grid" — they play with expectation. And this is exactly what Euclidean rhythms, ghost notes, and swing do.

```js
setcpm(128/4)

// Boring — perfectly on the grid
$: s("bd bd bd bd").bank("RolandTR909")

// ⭐ Syncopated — same number of kicks, more groove
$: s("bd ~ [~ bd] ~, ~ cp ~ cp, hh*8").bank("RolandTR909")
```

### Micro-Timing — The Source of "Feel"

Two patterns with the same notes can feel completely different depending on micro-timing. When events are placed slightly before or after the grid, it creates "feel":

- **Ahead of the beat** (rushing) → tense, driving, urgent
- **On the beat** (quantized) → mechanical, robotic, precise
- **Behind the beat** (dragging) → laid-back, groovy, relaxed

In Strudel, `.swingBy()` shifts every other note to simulate this:

```js
setcpm(128/4)

// Straight — mechanical, robotic
$: s("hh*16").bank("RolandTR909").gain(".5 .3 .7 .3")

// Swung — groovy, human
$: s("hh*16").bank("RolandTR909").gain(".5 .3 .7 .3").swingBy(1/6, 4)
```

---

## N.2 The Grid — Time Signatures and Subdivisions

All electronic drum programming happens on a **grid**. Understanding the grid is understanding the skeleton of every beat you'll ever build.

### The 4/4 Grid

Nearly all electronic dance music uses **4/4 time** — four beats per bar. Each beat can be subdivided:

```
ONE bar of 4/4 at 128 BPM:

Beat:       1       2       3       4
Quarter:    |       |       |       |
            ↓       ↓       ↓       ↓
8th notes:  1   &   2   &   3   &   4   &
            ↓   ↓   ↓   ↓   ↓   ↓   ↓   ↓
16th notes: 1 e & a 2 e & a 3 e & a 4 e & a
            ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

In Strudel terms:
- 4 quarter notes = s("bd*4")    — kick on every beat
- 8 eighth notes  = s("hh*8")    — hat on every 8th
- 16 sixteenths   = s("hh*16")   — hat on every 16th
```

### How Strudel Maps to the Grid

Strudel's cycle system maps elegantly to musical bars when you use `setcpm()`:

```
setcpm(128/4)  →  128 BPM, 1 cycle = 1 bar of 4/4

Events in a cycle:
┌────────────────────────────────────────┐
│ s("bd sd bd sd")                       │  4 events = quarter notes
│ s("hh*8")                              │  8 events = eighth notes
│ s("hh*16")                             │  16 events = sixteenth notes
│ s("bd")                                │  1 event  = whole note
└────────────────────────────────────────┘
```

### The Phrase Hierarchy

Beats group into bars, bars group into phrases. Most dance music thinks in powers of 2:

```
1 beat     = quarter note         (0.47s at 128 BPM)
4 beats    = 1 bar                (1.88s)
4 bars     = 1 phrase             (7.5s)
8 bars     = 1 section            (15s)
16 bars    = 1 major section      (30s)
32 bars    = 1 movement/build     (60s)
```

> **Why does this matter?** Most drum fills happen at **phrase boundaries** (every 4 or 8 bars). Most structural changes happen every 16 or 32 bars. When you're coding a drum pattern, use `.lastOf(4, ...)` or `.firstOf(8, ...)` to add fills at musically correct moments.

```js
setcpm(128/4)
// Snare fill every 4th cycle (= every 4 bars = phrase boundary)
$: s("~ cp ~ cp").bank("RolandTR909")
  .lastOf(4, x => x.ply(4))
  .room(0.1).gain(0.65)
```

### Beyond 4/4

While 4/4 dominates, other time signatures create unique grooves:

| Time Signature | Feel | Example Genre |
|---------------|------|---------------|
| **4/4** | Straight, danceable | Techno, house, trance |
| **3/4** | Waltz, rolling | Ambient, experimental |
| **6/8** | Compound, swinging | Tribal, Afro-house |
| **5/4** | Odd, lopsided | Experimental techno |
| **7/8** | Asymmetric, driving | Math-rock influenced electronic |

In Strudel, odd meters are easy — just use a different number of events:

```js
setcpm(128/4)
// 7/8 feel — 7 events per cycle
s("bd ~ sd ~ bd sd ~").bank("RolandTR909")
```

---

## N.3 The Drum Kit — Anatomy and Frequency Map

Every element in the electronic drum kit occupies a specific frequency range and serves a specific musical function. Understanding this lets you mix drums that don't fight each other.

### The Complete Drum Kit Map

```
Frequency Map of a Drum Kit:

Hz:  30    60    100   200   500   1k    2k    4k    8k   16k
     │     │     │     │     │     │     │     │     │     │
     ┌─────────────────┐
     │   KICK (bd)     │ 30–200 Hz — the foundation, felt in chest
     └─────────────────┘
                 ┌──────────────┐
                 │  TOMS (lt/mt/ht) │ 80–500 Hz — fills, transitions
                 └──────────────┘
                       ┌─────────────────────┐
                       │  SNARE/CLAP (sd/cp) │ 150–5000 Hz — backbeat
                       └─────────────────────┘
                             ┌──────────────────┐
                             │  RIMSHOT (rim)   │ 300–8000 Hz — accent
                             └──────────────────┘
                                         ┌──────────────────┐
                                         │  HI-HAT (hh/oh) │ 3k–16k Hz — groove
                                         └──────────────────┘
                                              ┌───────────────┐
                                              │  CRASH (cr)   │ 4k–16k Hz — accent
                                              └───────────────┘
                                              ┌───────────────┐
                                              │  RIDE (rd)    │ 4k–14k Hz — shimmer
                                              └───────────────┘
```

### Element-by-Element Reference

| Element | Strudel Name | Frequency Range | Musical Role | Typical Pattern | Filter Tip |
|---------|-------------|-----------------|--------------|-----------------|------------|
| **Kick** | `bd` | 30–200 Hz | Foundation, pulse | Every beat (`bd*4`) | No HPF! Leave sub intact |
| **Snare** | `sd` | 150–5000 Hz | Backbeat, crack | Beats 2 & 4 (`~ sd ~ sd`) | Slight HPF to reduce kick bleed |
| **Clap** | `cp` | 200–8000 Hz | Backbeat (thinner) | Beats 2 & 4 (`~ cp ~ cp`) | `.room(0.1)` for width |
| **Closed Hat** | `hh` | 3000–16000 Hz | Groove, motion | 8ths or 16ths (`hh*8`) | `.hpf(6000)` to clean |
| **Open Hat** | `oh` | 2000–16000 Hz | Accent, air | Off-beats, every 4th 16th | `.cut(1)` with `hh` |
| **Low Tom** | `lt` | 80–300 Hz | Fills, tribal | Fills, tribal patterns | Watch for kick clash |
| **Mid Tom** | `mt` | 100–400 Hz | Fills | Fills, rolls | — |
| **High Tom** | `ht` | 150–500 Hz | Fills | Fills, rolls | — |
| **Rimshot** | `rim` | 300–8000 Hz | Accent, click | Euclidean patterns | `.gain(0.3)` — sits behind |
| **Cowbell** | `cb` | 400–4000 Hz | Accent, Latin | Off-beats, sparse | `.gain(0.25)` |
| **Crash** | `cr` | 4000–16000 Hz | Climax, downbeat | Beat 1 of new sections | Sparse! |
| **Ride** | `rd` | 4000–14000 Hz | Sustained shimmer | Alternative to hats | — |

### Hearing Each Element

```js
setcpm(60/4)
// Cycle through every drum element
s("bd sd cp hh oh lt mt ht, ~ ~ ~ ~ ~ rim cb cr")
  .bank("RolandTR909")
```

### The Frequency Separation Rule

> **Every drum element should occupy its own frequency space.** When two elements share the same range, they fight for attention and the mix sounds muddy.

```js
setcpm(128/4)
// ✅ Good separation — each element in its own range
$: s("bd*4").bank("RolandTR909")                          // 30–200 Hz
$: s("~ cp ~ cp").bank("RolandTR909").hpf(300)            // 300+ Hz
$: s("hh*16").bank("RolandTR909").hpf(6000).gain(".5 .3 .7 .3")  // 6k+ Hz
$: s("rim(5,16)").bank("RolandTR909").gain(0.3).hpf(1000) // 1k+ Hz
```

---

## N.4 Rudiments for the Electronic Musician

Drum rudiments are foundational sticking patterns from acoustic drumming. In electronic music, we don't have sticks — but we can translate these concepts into programmed rhythm with **velocity**, **timing**, and **layering**.

### The Single Stroke Roll

The simplest rudiment: alternating hits. In electronic music, this becomes alternating hi-hat **velocities**:

```
Acoustic:   R L R L R L R L    (Right-Left alternation)
Electronic: ▓ ░ ▓ ░ ▓ ░ ▓ ░    (Loud-Soft alternation)
```

```js
setcpm(128/4)
// Single stroke roll — alternating accents on hats
$: s("hh*16").bank("RolandTR909")
  .gain(".8 .3 .8 .3 .8 .3 .8 .3 .8 .3 .8 .3 .8 .3 .8 .3")
```

### The Paradiddle

**RLRR LRLL** — the fundamental hand independence pattern. In electronic music, map R and L to different sounds:

```
Paradiddle:     R  L  R  R  L  R  L  L
Hi-hat:         hh ── hh hh ── hh ── ──
Snare ghost:    ── sd ── ── sd ── sd sd
```

```js
setcpm(128/4)
// Paradiddle mapped to hi-hat (R) and ghost snare (L)
$: s("hh ~ hh hh ~ hh ~ ~").bank("RolandTR909")
  .gain(".8 0 .5 .5 0 .5 0 0")
$: s("~ sd ~ ~ sd ~ sd sd").bank("RolandTR909")
  .gain("0 .15 0 0 .15 0 .15 .15")
```

### The Flam

A **grace note** just before the main hit — thickens the sound and adds push:

```
Normal hit:    ──── 💥
Flam:          ─ 💥💥    (soft grace note just before loud main hit)
```

In Strudel, create a flam by layering two hits with slight timing offset:

```js
setcpm(128/4)
// Flam snare — grace note before main hit
$: s("~ [~ cp] ~ [~ cp]").bank("RolandTR909").gain(0.65)
$: s("~ cp ~ cp").bank("RolandTR909").gain(0.2)
  .nudge(0.02)  // slightly early — the "grace note"
```

### The Drag

Two soft grace notes before the main hit — creates a "dragging" feel:

```
Normal hit:    ──── 💥
Drag:          💥💥💥    (two soft taps + one loud hit)
```

```js
setcpm(128/4)
// Drag on snare — two ghost notes leading into the hit
$: s("~ [sd sd cp] ~ [sd sd cp]").bank("RolandTR909")
  .gain("0 [.12 .12 .7] 0 [.12 .12 .7]")
```

### Rudiments Cheat Sheet for Electronic Music

| Rudiment | Acoustic | Electronic Translation | Strudel Technique |
|----------|----------|----------------------|-------------------|
| **Single stroke** | R L R L | Loud-quiet alternation | `.gain("1 .3")` pattern |
| **Double stroke** | RR LL RR LL | Two same-sound hits | `s("[hh hh] [sd sd]")` |
| **Paradiddle** | RLRR LRLL | Hat-snare interleaving | Separate `$:` patterns |
| **Flam** | grace + main | Layered hits, tight timing | `.nudge()` or `[~ x]` grouping |
| **Drag** | grace grace main | Triple with velocity ramp | `.gain("[.1 .1 .7]")` |
| **Buzz roll** | zzzzzz | Rapid 32nds | `s("sd*32").gain(...)` ramp |

---

## N.5 The Four-on-the-Floor and Beyond

The **four-on-the-floor** kick pattern — a kick on every beat — is the most fundamental pattern in dance music. But it's just the starting point.

### Pattern 1: Classic Four-on-the-Floor

```
Beat:    1   2   3   4
Kick:    x   x   x   x
Snare:   ─   x   ─   x
Hat:     x x x x x x x x
```

```js
setcpm(128/4)
s("bd*4, ~ cp ~ cp, hh*8").bank("RolandTR909")
```

### Pattern 2: Syncopated Kick

Move the third kick off the beat to create tension:

```
16ths:   1 e & a 2 e & a 3 e & a 4 e & a
Kick:    x ─ ─ ─ ─ ─ ─ ─ ─ ─ x ─ ─ ─ ─ ─
Snare:   ─ ─ ─ ─ x ─ ─ ─ ─ ─ ─ ─ x ─ ─ ─
```

```js
setcpm(128/4)
s("bd ~ [~ bd] ~, ~ cp ~ cp, hh*16").bank("RolandTR909")
  .gain("1 1 1 1, .65 .65 .65 .65, .5 .3 .7 .3")
```

### Pattern 3: Off-Beat Kick (House Variation)

```
16ths:   1 e & a 2 e & a 3 e & a 4 e & a
Kick:    x ─ ─ x ─ ─ x ─ x ─ ─ ─ x ─ ─ ─
```

```js
setcpm(124/4)
s("bd ~ ~ bd ~ ~ bd ~ bd, ~ cp ~ cp, hh*8")
  .bank("RolandTR808")
```

### Pattern 4: Half-Time

The snare hits only on beat 3 instead of 2 and 4 — creates a slower, heavier feel at the same BPM:

```
Beat:    1       2       3       4
Kick:    x       ─       ─       ─
Snare:   ─       ─       x       ─
```

```js
setcpm(140/4)
// Half-time — feels like 70 BPM despite running at 140
s("bd ~ ~ ~ ~ ~ sd ~, hh*8").bank("RolandTR909")
  .gain("1 0 0 0 0 0 .7 0, .5 .3 .7 .3")
```

### Pattern 5: Breakbeat

No four-on-the-floor — the kick and snare interleave in syncopated patterns:

```
16ths:   1 e & a 2 e & a 3 e & a 4 e & a
Kick:    x ─ ─ ─ ─ ─ ─ ─ ─ ─ x ─ ─ ─ ─ ─
Snare:   ─ ─ ─ ─ x ─ ─ ─ ─ ─ ─ ─ x ─ ─ x
```

```js
setcpm(130/4)
s("bd ~ ~ ~ sd ~ ~ ~ ~ ~ bd ~ sd ~ ~ sd, hh*16")
  .bank("RolandTR909")
  .gain("1 0 0 0 .7 0 0 0 0 0 .8 0 .7 0 0 .5, .4 .2 .5 .2")
```

---

## N.6 Euclidean Rhythms — Mathematics Meets Music

One of the most powerful tools in Strudel: **Euclidean rhythms** — an algorithm that distributes N beats as evenly as possible across M steps. Discovered by Godfried Toussaint in 2004, this simple algorithm generates rhythms found in musical traditions across the entire world.

### The Algorithm

Given `k` beats and `n` steps, the Euclidean algorithm distributes the beats as evenly as possible:

```
E(3,8) — 3 beats in 8 steps:

Step:    1  2  3  4  5  6  7  8
         x  .  .  x  .  .  x  .

This is the Cuban Tresillo — one of the most common
rhythmic patterns on Earth!
```

### Why Does It Sound Good?

Euclidean rhythms maximize the **distance** between beats. This creates a pattern that is:
1. **Regular enough** for the brain to entrain (predictable)
2. **Irregular enough** to create interest (syncopated)
3. **Mathematically balanced** — the closest thing to "perfectly spaced" beats in an uneven division

### Strudel Syntax

```js
// Three ways to write Euclidean rhythms in Strudel:

// 1. Inline mini-notation
s("bd(3,8)")              // 3 beats in 8 steps

// 2. With rotation (offset)
s("bd(3,8,1)")            // same pattern, rotated 1 step

// 3. .euclid() function
s("bd").euclid(3,8)       // equivalent to bd(3,8)
s("bd").euclid(3,8,1)     // with rotation
```

### The Essential Euclidean Reference Table

| Pattern | Beats | Steps | Musical Name | Origin | Sound |
|---------|-------|-------|-------------|--------|-------|
| **E(2,3)** | 2 | 3 | — | — | `x . x` |
| **E(2,5)** | 2 | 5 | Khafif-e-ramal | Persian | `x . x . .` |
| **E(3,4)** | 3 | 4 | Cumbia | Colombian | `x . x x` |
| **E(3,7)** | 3 | 7 | — | Turkish | `x . x . x . .` |
| **E(3,8)** | 3 | 8 | **Tresillo** | Cuban/West African | `x . . x . . x .` |
| **E(4,7)** | 4 | 7 | — | — | `x . x . x . x` |
| **E(4,9)** | 4 | 9 | Aksak | Turkish | `x . x . x . x . .` |
| **E(4,11)** | 4 | 11 | — | — | `x . . x . . x . . x .` |
| **E(4,12)** | 4 | 12 | — | — | `x . . x . . x . . x . .` |
| **E(5,6)** | 5 | 6 | Aak | Korean | `x . x x x x` |
| **E(5,8)** | 5 | 8 | **Cinquillo** | Cuban | `x . x x . x x .` |
| **E(5,9)** | 5 | 9 | — | Turkish | `x . x . x . x . x` |
| **E(5,11)** | 5 | 11 | — | — | `x . x . x . x . x . .` |
| **E(5,12)** | 5 | 12 | Venda clapping | South African | `x . . x . x . . x . x .` |
| **E(5,16)** | 5 | 16 | **Bossa Nova** | Brazilian | `x . . x . . x . . . x . . x . .` |
| **E(7,8)** | 7 | 8 | — | — | `x . x x x x x x` |
| **E(7,12)** | 7 | 12 | — | West African | `x . x x . x . x x . x .` |
| **E(7,16)** | 7 | 16 | — | — | Widely used in techno |
| **E(9,16)** | 9 | 16 | — | — | Dense, driving |
| **E(11,16)** | 11 | 16 | — | — | Very dense |
| **E(13,16)** | 13 | 16 | — | — | Almost continuous |

### Hear the Most Important Patterns

```js
setcpm(128/4)
// The world's rhythms in Strudel

// Tresillo (Cuban) — E(3,8)
$: s("rim(3,8)").bank("RolandTR909").gain(0.4)

// Cinquillo (Cuban) — E(5,8)
$: s("cb(5,8)").bank("RolandTR909").gain(0.3)

// Bossa Nova — E(5,16)
$: s("cp(5,16)").bank("RolandTR909").gain(0.5)

// Four-on-the-floor (for reference)
$: s("bd*4").bank("RolandTR909")
```

### Building a Full Beat with Euclidean Patterns

The magic happens when you **layer** different Euclidean patterns:

```js
setcpm(128/4)
// Polyrhythmic Euclidean techno beat
$: s("bd(4,16)").bank("RolandTR909").gain(0.85)
$: s("cp(2,5)").bank("RolandTR909").gain(0.55).room(0.1)
$: s("hh(7,16)").bank("RolandTR909").gain(0.4).hpf(6000)
$: s("rim(5,12)").bank("RolandTR909").gain(0.3).pan(rand)
$: s("oh(3,8)").bank("RolandTR909").gain(0.25).cut(1)
```

### Rotation — Shifting the Downbeat

The third parameter rotates the pattern, creating variations from the same Euclidean structure:

```js
setcpm(128/4)
// Same E(3,8) pattern, different rotations — different grooves
$: s("bd(3,8,<0 1 2>)").bank("RolandTR909")
$: s("hh*8").bank("RolandTR909").gain(".5 .3")
```

> **The deepest insight:** Almost every great "world music" rhythm is Euclidean. The tresillo (Cuban), the clave (Afro-Cuban), the Bossa Nova, West African bell patterns, Turkish aksak — they're all maximally-even distributions of beats. Mathematics and musical tradition converge.

---

## N.7 Ghost Notes, Accents, and Velocity Shaping

The difference between a drum machine and a drummer is **dynamics**. A machine plays every hit at the same volume. A drummer naturally plays some hits loud (accents) and some soft (ghost notes). Programming dynamics transforms a robotic loop into a living groove.

### What Are Ghost Notes?

Ghost notes are **very quiet** drum hits — typically snare taps at ~10–20% volume — that fill the spaces between main beats. They're almost subliminal, but remove them and the groove collapses.

```
Without ghost notes:          With ghost notes:
16ths: 1 e & a 2 e & a       16ths: 1 e & a 2 e & a
Snare: ─ ─ ─ ─ X ─ ─ ─       Snare: ─ ○ ─ ○ X ─ ○ ─
                               (○ = ghost note, X = accent)
```

```js
setcpm(128/4)
// Without ghost notes — flat, mechanical
$: s("bd*4, ~ sd ~ sd, hh*16").bank("RolandTR909")

// With ghost notes — groovy, alive
$: s("bd*4").bank("RolandTR909")
$: s("sd*8").bank("RolandTR909")
  .gain("0 .12 0 .12 .7 0 .12 0")
$: s("hh*16").bank("RolandTR909").gain(".5 .3 .7 .3").hpf(6000)
```

### Accent Patterns

Accents are the **loudest** hits — they define the rhythmic skeleton that your brain locks onto. The pattern of accents is what distinguishes one groove from another.

**Common accent patterns for 16th-note hats:**

```
Standard techno:    ▓ ░ ▓ ░ ▓ ░ ▓ ░ ▓ ░ ▓ ░ ▓ ░ ▓ ░
                    (accents on every downbeat)

Upbeat push:        ░ ▓ ░ ▓ ░ ▓ ░ ▓ ░ ▓ ░ ▓ ░ ▓ ░ ▓
                    (accents on every upbeat — pushier)

American house:     ▓ ░ ░ ░ ▓ ░ ░ ░ ▓ ░ ░ ░ ▓ ░ ░ ░
                    (accents on quarter notes only)

▓ = loud (.7–1.0), ░ = soft (.2–.4)
```

```js
setcpm(128/4)
// Standard techno hat accent pattern
$: s("hh*16").bank("RolandTR909")
  .gain(".8 .3 .5 .3 .8 .3 .5 .3 .8 .3 .5 .3 .8 .3 .5 .3")
  .hpf(6000)
```

### Velocity Shapes

Instead of repeating a fixed pattern, you can use **continuous signals** to shape velocity over time:

```js
setcpm(128/4)
// Sine-wave gain — creates a "breathing" hat pattern
$: s("hh*16").bank("RolandTR909")
  .gain(sine.range(0.2, 0.8).fast(2))
  .hpf(6000)

// Random gain — humanized, organic feel
$: s("hh*16").bank("RolandTR909")
  .gain(rand.range(0.2, 0.7))
  .hpf(6000)
```

### Swing — The Secret Sauce

**Swing** displaces every other note (typically the "e" and "a" of each beat) slightly later, creating a triplet-like feel:

| Swing Amount | Feel |
|-------------|------|
| 0% | Perfectly straight — robotic |
| ~15% (1/6) | Subtle groove — **most techno** |
| ~25% (1/4) | Heavy shuffle — house, garage |
| 50% | Full triplet — jazz swing |
| >50% | Extreme — experimental only |

```js
setcpm(128/4)
// Subtle swing — the techno sweet spot
$: s("hh*16").bank("RolandTR909")
  .gain(".5 .3 .7 .3").hpf(6000)
  .swingBy(1/6, 4)

// Heavy shuffle — deep house feel
$: s("hh*16").bank("RolandTR808")
  .gain(".6 .25 .8 .25").hpf(5000)
  .swingBy(1/4, 4)
```

---

## N.8 Polyrhythms and Polymeters

**Polyrhythm**: two different rhythmic groupings played simultaneously against the same pulse. **Polymeter**: two patterns with different cycle lengths, creating phase relationships.

### 3 Against 4 — The Classic Polyrhythm

The simplest and most powerful polyrhythm: one layer plays 3 beats per bar while another plays 4:

```
Beat:    1 . . . 2 . . . 3 . . . 4 . . .
4-beat:  x . . . x . . . x . . . x . . .
3-beat:  x . . . . x . . . . x . . . . .

Combined: They align every 12 beats (LCM of 3 and 4)
```

```js
setcpm(128/4)
// 3 against 4
$: s("bd*4").bank("RolandTR909")                    // 4 per cycle
$: s("rim*3").bank("RolandTR909").gain(0.4)         // 3 per cycle
$: s("hh*8").bank("RolandTR909").gain(".5 .3").hpf(6000)
```

### 5 Against 4 — The Complex Polyrhythm

```js
setcpm(128/4)
// 5 against 4 — more disorienting
$: s("bd*4").bank("RolandTR909")
$: s("cb*5").bank("RolandTR909").gain(0.3)
$: s("hh*16").bank("RolandTR909").gain(".4 .2 .5 .2").hpf(6000)
```

### Euclidean Polyrhythms

Combine different Euclidean patterns for organic polyrhythmic textures:

```js
setcpm(128/4)
// Each layer has a different Euclidean density
$: s("bd(4,16)").bank("RolandTR909").gain(0.85)     // steady
$: s("cp(3,8)").bank("RolandTR909").gain(0.55)      // tresillo
$: s("rim(5,12)").bank("RolandTR909").gain(0.3)     // cross-rhythm
$: s("hh(7,16)").bank("RolandTR909").gain(0.35).hpf(6000)
$: s("oh(2,5)").bank("RolandTR909").gain(0.2).cut(1) // sparse accent
```

### Polymeters with Comma Separation

In Strudel's mini-notation, comma-separated groups create **true polymeters** — each group has its own length:

```js
setcpm(128/4)
// Polymeter: 4-beat kick, 5-beat rimshot, 3-beat cowbell
s("bd*4, rim*5, cb*3").bank("RolandTR909")
  .gain("0.85, 0.3, 0.25")
```

### When to Use Polyrhythms in Electronic Music

| Technique | When to Use | Effect |
|-----------|-------------|--------|
| **3:4 polyrhythm** | Breakdowns, builds | Creates tension and complexity |
| **5:4 polyrhythm** | Experimental sections | Disorienting, hypnotic |
| **Euclidean layers** | Main groove | Organic, evolving rhythm |
| **Polymeter** | Percussion layers | Patterns that shift against each other |

> **The golden rule:** Keep the **kick and clap on simple patterns** (they're the anchor). Put polyrhythmic complexity on **secondary elements** (rimshot, cowbell, shaker, toms). This maintains danceability while adding interest.

---

## N.9 Hi-Hat Artistry

Hi-hats are the **most expressive element** in electronic drum programming. They define the groove, energy level, and genre more than any other element.

### Open vs. Closed — The Cut Group

In real life, pressing the hi-hat pedal mutes the ringing open hat. Strudel replicates this with `.cut()`:

```js
setcpm(128/4)
// Without cut — open hat rings over closed hat (unrealistic)
$: s("[hh hh hh oh]*4").bank("RolandTR909")

// With cut — closed hat mutes the open hat (realistic)
$: s("[hh hh hh oh]*4").bank("RolandTR909").cut(1)
```

### Essential Hi-Hat Patterns

**Straight 8ths** — clean, simple:
```js
setcpm(128/4)
$: s("hh*8").bank("RolandTR909").gain(".7 .3").hpf(6000)
```

**16ths with open hat accent** — standard techno:
```js
setcpm(128/4)
$: s("[hh hh hh <hh oh>]*4").bank("RolandTR909")
  .gain(".6 .3 .5 .3 .6 .3 .5 .8")
  .hpf(6000).cut(1)
```

**Sparse & delayed** — minimal techno:
```js
setcpm(128/4)
$: s("hh ~ hh ~ ~ hh ~ hh").bank("RolandTR909")
  .gain(".5 0 .3 0 0 .7 0 .3")
  .delay(0.3).delaytime(0.1875).delayfeedback(0.4)
  .hpf(6000)
```

**Ride instead of hat** — dub techno:
```js
setcpm(124/4)
$: s("rd*8").bank("RolandTR909")
  .gain(".5 .3 .6 .3")
  .room(0.5).roomsize(0.7)
  .hpf(4000)
```

### Genre-Specific Hi-Hat Recipes

| Genre | Pattern | Signature |
|-------|---------|-----------|
| **Minimal Techno** | `hh*16` quiet, `oh` every 4th bar | Subtle, background |
| **Deep House** | `hh*8` swung, occasional `oh` | Loose, groovy |
| **Acid House** | `hh*16` straight, heavy accent on "&" | Driving, relentless |
| **Drum & Bass** | Rapid `hh` rolls, `oh` for accents | Frantic, complex |
| **Trance** | `hh*8` or `hh*16` straight, offbeat `oh` | Steady, building |
| **Industrial** | Distorted `hh*16`, no `oh` | Harsh, mechanical |

---

## N.10 Genre Drum Cookbook

Complete, ready-to-paste drum patterns for each genre. Every recipe includes a grid diagram and working Strudel code.

---

### ⚫ Minimal Techno (126–132 BPM)

**Philosophy:** Less is more. The kick is king. Hats are subtle. Percussion is sparse.

```
16ths:  1 e & a 2 e & a 3 e & a 4 e & a
Kick:   x . . . x . . . x . . . x . . .
Clap:   . . . . x . . . . . . . x . . .
Hat:    x . x . x . x . x . x . x . x .
Rim:    . . x . . . . . x . . . . . x .
```

```js
setcpm(128/4)
$: s("bd*4").bank("RolandTR909").gain(0.85)
$: s("~ cp ~ cp").bank("RolandTR909").gain(0.6).room(0.12)
$: s("hh*16").bank("RolandTR909")
  .gain(".5 .25 .4 .25 .5 .25 .4 .25")
  .hpf(6000).swingBy(1/6, 4)
$: s("rim(5,16)").bank("RolandTR909")
  .gain(0.28).pan(rand).delay(0.15).delaytime(0.1875)
```

---

### 🏠 Deep House (118–122 BPM)

**Philosophy:** Groovy, swung. TR-808 kick. Open hats on upbeats. Optional shaker/tambourine layer.

```
16ths:  1 e & a 2 e & a 3 e & a 4 e & a
Kick:   x . . . x . . . x . . . x . . .
Clap:   . . . . x . . . . . . . x . . .
Hat:    x . x . x . x . x . x . x . x .
O-hat:  . . . . . . . x . . . . . . . x
```

```js
setcpm(120/4)
$: s("bd*4").bank("RolandTR808").gain(0.8)
$: s("~ cp ~ cp").bank("RolandTR808").gain(0.55).room(0.15)
$: s("[hh hh hh <hh oh>]*2").bank("RolandTR808")
  .gain(".6 .3 .5 .7").hpf(5000).cut(1)
  .swingBy(1/4, 4)
```

---

### 🌊 Dub Techno (120–130 BPM)

**Philosophy:** Stripped to essentials. Heavy reverb on everything. Ride cymbal instead of hat. Groove emerges from effects, not complexity.

```js
setcpm(124/4)
$: s("bd*4").bank("RolandTR909").gain(0.8)
$: s("~ sd ~ sd").bank("RolandTR909")
  .gain(0.35).room(0.6).roomsize(0.8)
$: s("rd*8").bank("RolandTR909")
  .gain(".4 .2 .35 .2").hpf(4000)
  .room(0.5).delay(0.3).delaytime(0.375).delayfeedback(0.6)
$: s("rim(3,8)").bank("RolandTR909")
  .gain(0.2).room(0.4)
```

---

### 🧪 Acid House (120–130 BPM)

**Philosophy:** Driving 16th-note hats. Strong clap on 2 & 4. 808 kick. Cowbell on upbeats.

```js
setcpm(126/4)
$: s("bd*4").bank("RolandTR808").gain(0.85)
$: s("~ cp ~ cp").bank("RolandTR808").gain(0.65)
$: s("hh*16").bank("RolandTR808")
  .gain(".6 .3 .7 .3").hpf(5000)
$: s("~ cb ~ cb ~ cb ~ cb").bank("RolandTR808").gain(0.2)
$: s("oh(3,8)").bank("RolandTR808").gain(0.25).cut(1)
```

---

### 🥁 Drum & Bass (170–180 BPM)

**Philosophy:** Breakbeat-derived. Kick on 1 and the "and" of 2. Snare on 2 and 4 (half-time feel). Complex hi-hat rolls. Ghost snares essential.

```
16ths:  1 e & a 2 e & a 3 e & a 4 e & a
Kick:   x . . . . . x . . . . . . . . .
Snare:  . . . . x . . . . . . . x . . .
Ghost:  . . ○ . . . . . ○ . ○ . . . ○ .
Hat:    x x . x x x . x x x . x x x . x
```

```js
setcpm(174/4)
$: s("bd ~ ~ ~ ~ ~ bd ~, ~ ~ ~ ~ ~ ~ ~ ~")
  .bank("RolandTR909").gain(0.85)
$: s("~ sd ~ ~ ~ ~ ~ ~").bank("RolandTR909").gain(0.7)
$: s("sd*8").bank("RolandTR909")
  .gain("0 0 .12 0 0 0 0 0")
$: s("[hh hh ~ hh hh hh ~ hh]*2").bank("RolandTR909")
  .gain(".5 .3 0 .4 .6 .3 0 .5")
  .hpf(6000)
```

---

### 🔩 Industrial Techno (130–150 BPM)

**Philosophy:** Heavy, distorted, aggressive. Everything pushed hard. Distorted kick. Noisy hats. Relentless.

```js
setcpm(140/4)
$: s("bd*4").bank("RolandTR909").distort(1.5).gain(0.85)
$: s("~ [cp cp] ~ cp").bank("RolandTR909")
  .gain(0.7).distort(0.5)
$: s("hh*16").bank("RolandTR909")
  .gain(".7 .4 .6 .4").hpf(4000)
  .distort(0.3).crush(8)
$: s("mt(5,16)").bank("RolandTR909")
  .gain(0.35).distort(0.8)
```

---

### 🔮 Trance (136–150 BPM)

**Philosophy:** Steady, building. Four-on-the-floor is non-negotiable. Crisp clap. Open hat on upbeats. Ride for energy.

```js
setcpm(140/4)
$: s("bd*4").bank("RolandTR909").gain(0.85)
$: s("~ cp ~ cp").bank("RolandTR909").gain(0.6).room(0.1)
$: s("[hh hh hh oh]*4").bank("RolandTR909")
  .gain(".6 .3 .5 .8").hpf(6000).cut(1)
$: s("rd*8").bank("RolandTR909").gain(0.2).hpf(5000)
```

---

### 🌙 Lo-Fi Hip Hop (80–95 BPM)

**Philosophy:** Swung, lazy, imperfect. Vinyl crackle texture. Light touch.

```js
setcpm(85/4)
$: s("bd ~ ~ ~ ~ ~ bd ~").bank("RolandTR808").gain(0.75)
$: s("~ ~ ~ ~ sd ~ ~ ~").bank("RolandTR808")
  .gain(0.6).room(0.2)
$: s("hh*8").bank("RolandTR808")
  .gain(".4 .2 .5 .2").hpf(6000)
  .swingBy(1/4, 4)
$: s("crackle*2").density(0.05).gain(0.08)
```

---

### 💥 Breakbeat (130–140 BPM)

**Philosophy:** No four-on-the-floor. Syncopated kick-snare interplay. Sampled breaks are traditional, but we can build our own.

```js
setcpm(135/4)
$: s("bd ~ ~ ~ sd ~ ~ bd ~ ~ bd ~ sd ~ ~ ~")
  .bank("RolandTR909").gain(0.8)
$: s("hh*16").bank("RolandTR909")
  .gain(".5 .3 .6 .3 .5 .3 .7 .3").hpf(6000)
$: s("oh(3,16)").bank("RolandTR909")
  .gain(0.25).cut(1)
$: s("rim(5,12)").bank("RolandTR909").gain(0.25).pan(rand)
```

---

## N.11 Building Fills and Transitions

Drum fills mark **boundaries** between phrases, sections, and structural changes. They tell the listener "something is about to change."

### The Snare Roll

The most common fill — increasing density on the snare leading into a new section:

```js
setcpm(128/4)
// Normal beat for 3 cycles, snare roll on the 4th
$: s("~ cp ~ cp").bank("RolandTR909").gain(0.65)
  .lastOf(4, x => x.ply(4))
```

### The Accelerating Roll

Start slow, end fast — creates a build-up effect:

```js
setcpm(128/4)
// Accelerating snare roll — 4th notes → 8th → 16th → 32nd
$: s("~ cp ~ cp").bank("RolandTR909").gain(0.65)
  .lastOf(8, x => x.fast("<1 2 4 8>"))
```

### Tom Fill

Classic descending tom fill — high to low:

```js
setcpm(128/4)
// Tom fill every 4th cycle
$: s("~ cp ~ cp").bank("RolandTR909").gain(0.6)
  .lastOf(4, _ => s("ht ht mt mt lt lt lt lt").bank("RolandTR909").gain(0.5))
```

### Hi-Hat Build

Increase hi-hat density approaching a drop:

```js
setcpm(128/4)
// Hats get denser each cycle over 4 cycles, then reset
$: s("hh*<8 12 16 32>").bank("RolandTR909")
  .gain(sine.range(0.2, 0.6).fast(2))
  .hpf(6000)
```

### The Percussion Swell

Layer multiple percussion elements with increasing density:

```js
setcpm(128/4)
// Build percussion density every 8 cycles
$: s("bd*4").bank("RolandTR909").gain(0.85)
$: s("~ cp ~ cp").bank("RolandTR909").gain(0.6)
$: s("hh*16").bank("RolandTR909").gain(".5 .3").hpf(6000)
$: s("rim(5,16)").bank("RolandTR909").gain(0.3)
  .lastOf(8, x => x.ply(2).gain(0.4))
```

### Fill Timing Guideline

| Fill Length | When to Use | Strudel |
|------------|-------------|---------|
| 1 beat (4 16ths) | Subtle, frequent | `.lastOf(2, ...)` |
| 2 beats (half bar) | Phrase boundary | `.lastOf(4, ...)` |
| 1 bar | Section change | `.lastOf(8, ...)` |
| 2 bars | Major transition | `.lastOf(16, ...)` |
| 4+ bars | Build/drop | `.lastOf(32, ...)` |

---

## N.12 Practice Challenges

### Challenge 1: Build a Basic Beat ⭐

Create a 128 BPM minimal techno beat with: kick on every beat, clap on 2 and 4, 8th-note hats. Use TR-909.

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
$: s("bd*4").bank("RolandTR909").gain(0.85)
$: s("~ cp ~ cp").bank("RolandTR909").gain(0.6)
$: s("hh*8").bank("RolandTR909").gain(".5 .3").hpf(6000)
```
</details>

### Challenge 2: Euclidean Exploration ⭐⭐

Create a beat using **only** Euclidean rhythms — no `*` multiplication. Use at least 4 different E(k,n) patterns with different drum sounds.

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
$: s("bd(4,16)").bank("RolandTR909").gain(0.85)
$: s("cp(2,5)").bank("RolandTR909").gain(0.55).room(0.1)
$: s("hh(9,16)").bank("RolandTR909").gain(0.35).hpf(6000)
$: s("rim(5,12)").bank("RolandTR909").gain(0.3).pan(rand)
$: s("oh(3,8)").bank("RolandTR909").gain(0.2).cut(1)
```
</details>

### Challenge 3: Ghost Note Groove ⭐⭐

Program a beat with **ghost snares** — soft snare hits between the main claps. The claps should be at ~0.65 gain and the ghosts at ~0.12.

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
$: s("bd*4").bank("RolandTR909").gain(0.85)
$: s("sd*16").bank("RolandTR909")
  .gain("0 .1 0 .12 .65 0 .1 0 0 .12 0 .1 .65 0 .12 0")
$: s("hh*16").bank("RolandTR909")
  .gain(".5 .25 .4 .25").hpf(6000).swingBy(1/6, 4)
```
</details>

### Challenge 4: Genre Transformation ⭐⭐⭐

Take the minimal techno beat from Challenge 1 and transform it into a **Drum & Bass** pattern at 174 BPM with a breakbeat feel — syncopated kick, snare on 2 & 4 (half-time), ghost notes.

<details>
<summary>Solution</summary>

```js
setcpm(174/4)
$: s("bd ~ ~ ~ ~ ~ bd ~ ~ ~ ~ ~ ~ ~ ~ ~")
  .bank("RolandTR909").gain(0.85)
$: s("~ ~ ~ ~ sd ~ ~ ~ ~ ~ ~ ~ sd ~ ~ ~")
  .bank("RolandTR909").gain(0.7)
$: s("sd*16").bank("RolandTR909")
  .gain("0 0 .12 0 0 0 0 .12 0 .12 0 0 0 0 .12 0")
$: s("[hh hh ~ hh hh hh ~ hh]*2").bank("RolandTR909")
  .gain(".5 .3 0 .4 .6 .3 0 .5").hpf(6000)
```
</details>

### Challenge 5: Polyrhythmic Build ⭐⭐⭐⭐

Create a beat that combines:
- Four-on-the-floor kick
- A 3-against-4 rimshot polyrhythm
- An E(5,16) cowbell pattern
- 16th-note hats with swing and velocity shaping
- A snare roll fill every 8th cycle

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
// Kick — the anchor
$: s("bd*4").bank("RolandTR909").gain(0.85)

// Clap — backbeat
$: s("~ cp ~ cp").bank("RolandTR909").gain(0.6).room(0.12)

// 3-against-4 rimshot polyrhythm
$: s("rim*3").bank("RolandTR909").gain(0.3).pan(rand)
  .delay(0.15).delaytime(0.1875).delayfeedback(0.3)

// Euclidean cowbell
$: s("cb(5,16)").bank("RolandTR909").gain(0.2)

// Swung hats with velocity shaping
$: s("[hh hh hh <hh oh>]*4").bank("RolandTR909")
  .gain(".6 .3 .5 .3 .6 .3 .5 .8")
  .hpf(6000).cut(1).swingBy(1/6, 4)

// Snare roll fill every 8th cycle
$: s("~ cp ~ cp").bank("RolandTR909").gain(0.6)
  .lastOf(8, x => x.ply(4).gain(0.5))
```
</details>

---

## N.13 Key Takeaways

1. **Rhythm works because of entrainment** — your brain locks to predictable beats and rewards syncopated surprises with dopamine
2. **The 16th-note grid is your canvas** — every electronic drum pattern can be understood as events placed on a 16-step grid per bar
3. **Each drum element has a frequency range** — respect the separation (kick = sub, hats = treble, snare = mid) to avoid muddiness
4. **Rudiments translate to electronic music** — paradiddles, flams, and drags become velocity patterns, layered hits, and `.nudge()` timing
5. **Four-on-the-floor is just the beginning** — syncopated kicks, half-time, and breakbeats create entirely different feels at the same tempo
6. **Euclidean rhythms are universal** — the algorithm generates patterns found in Cuban, African, Turkish, Brazilian, and Korean traditions
7. **Ghost notes create groove** — soft hits at 10–15% volume between accents are what separate a machine from a musician
8. **Polyrhythms create complexity** — layer different rhythmic densities on secondary elements while keeping the kick simple
9. **Hi-hats define the genre** — the hat pattern is often the single biggest differentiator between techno, house, DnB, and trance
10. **Fills mark boundaries** — use `.lastOf()` with `.ply()` or `.fast()` to place snare rolls and tom fills at musically correct phrase boundaries

---

**← Back to [Module 5 — Samples & Drum Machines](../modules/Module_05_Samples_DrumMachines.md)**
**← Back to [README — Course Overview](../README.md)**
