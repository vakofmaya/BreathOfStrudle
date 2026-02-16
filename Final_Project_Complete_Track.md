# Final Project — Building a Complete Minimal Techno Track

> **Goal:** Build a professional-quality Minimal Techno track from nothing, one element at a time. Every design decision is explained. When you're done, you'll have a ~5-minute track you can perform live.

---

## Part 1 — What Makes Great Minimal Techno?

Before writing a single line of code, let's study what the masters do.

### Lessons from the Pioneers

**Robert Hood — *Minimal Nation* (1994)**
Hood proved you only need **3–4 components** to make compelling techno. His tracks use a thundering kick, ticking hi-hats, and a single hypnotic synth riff — nothing else. The spaces between the notes are as important as the notes themselves. His basslines are simple one-bar patterns that glide, using portamento to create rhythmic interaction with the kick.

**Richie Hawtin / Plastikman — *Spastik* (1993)**
Built around processed TR-808/909 percussion with swing and rattling snare rolls. Hawtin valued the subtle tempo fluctuations of hardware for a human feel. His approach: use powerful tools to deliver simple elements. Everything is stripped to its essence.

**Ricardo Villalobos — *Easy Lee* (2003)**
Known for complex yet subtle rhythmic layers influenced by Latin American percussion. His tracks are long (10–30 minutes), treating music as a living organism that needs time to unfold. Sound architecture over conventional song structures.

### The 7 Rules of Great Minimal Techno

| # | Rule | What It Means |
|---|------|--------------|
| 1 | **Less is more** | 4–6 elements maximum. Every sound must earn its place. |
| 2 | **Rhythm over melody** | The groove carries the track. Melodic elements are sparse accents. |
| 3 | **Evolve, don't change** | Subtle filter sweeps, not dramatic key changes. The listener shouldn't notice the evolution — they should *feel* it. |
| 4 | **Space is a sound** | Silence, rests, and emptiness are compositional tools. |
| 5 | **The kick is king** | 40–60 Hz, punchy, clean. Tuned to the key. Everything else works around it. |
| 6 | **Groove over grid** | Swing, velocity variation, and micro-timing create a human feel in a machine. |
| 7 | **Sidechain is the glue** | The pumping effect ties everything to the kick, creating a breathing, living mix. |

### Sound Design Principles

- **Intentionality over complexity:** Each sound is polished and purposeful
- **Refinement over flash:** Subtle filter modulation beats dramatic sound design
- **The mid-range is sacred:** Keep it relatively empty — kick/bass live in the lows, hats/textures in the highs, and the mids breathe
- **Saturation adds body:** Subtle distortion makes kicks audible on all systems
- **Reverb with restraint:** Early reflections for "direction," not wash. Reverb creates space, not ambience

---

## Part 2 — Our Track Blueprint

We're building a track called **"Pulse"** — a hypnotic, deep minimal techno track in **C minor** at **128 BPM**.

### Element Plan

We'll use exactly **7 elements** (plus sidechain and texture), following the Robert Hood philosophy of maximum impact from minimum material:

| # | Element | Sound Source | Role | Frequency Range |
|---|---------|-------------|------|----------------|
| 1 | **Kick** | Synthesized sine | Foundation pulse | 40–100 Hz |
| 2 | **Hi-Hats** | TR-909 samples | Groove, movement | 6–16 kHz |
| 3 | **Clap** | TR-909 samples | Backbeat accent | 1–5 kHz |
| 4 | **Rimshot** | TR-909 samples | Polyrhythmic texture | 2–8 kHz |
| 5 | **Bass** | Sawtooth + ladder filter | Harmonic foundation | 60–400 Hz |
| 6 | **Pad** | Sawtooth (voiced chords) | Atmosphere, harmony | 300–3000 Hz |
| 7 | **Lead** | Triangle + delay | Melodic hook | 500–4000 Hz |
| — | **Sidechain** | Silent trigger | Pumping on pad | — |
| — | **Texture** | Crackle + noise | Vinyl warmth | 2–16 kHz |

### Chord Progression

A minimal two-chord loop that shifts every two bars, with occasional variation:

```
| Cm      | Cm      | Fm      | Fm      |
  i           i         iv         iv
```

Extended to 8 bars with more movement:

```
| Cm      | Cm      | Fm      | Fm      | Ab      | Ab      | Bb      | Bb      |
  i           i         iv         iv       VI         VI       VII       VII
```

Why these chords?
- **Cm → Fm**: Root to subdominant — safe, dark, groovy
- **Ab → Bb**: Creates forward motion back toward Cm
- **All diatonic to C natural minor** — no "wrong" notes possible if bass and melody stay in scale

### Arrangement Plan (~128 bars / ~4 minutes)

```
INTRO         16 bars    Kick fades in → add texture
BUILD A        8 bars    + Hi-hats (filtered) → + Clap
MAIN A        32 bars    + Bass + Pad + Sidechain + Rimshot
BREAKDOWN      8 bars    − Kick − Bass, Pad opens up, + Lead melody
BUILD B        8 bars    Kick returns, filter sweep up, + snare roll
MAIN B        32 bars    Full mix: all elements + lead + variation
BREAKDOWN B    8 bars    Atmospheric, stripped
DROP          16 bars    Everything back, peak energy
OUTRO         16 bars    Elements fade out → kick only → silence
─────────────────────────
TOTAL        ~144 bars   ≈ 4:30 at 128 BPM
```

---

## Part 3 — Building Each Sound

Open [strudel.cc](https://strudel.cc/) and let's build each element. **Test each one in isolation before combining.**

### 3.1 — The Kick

The most important sound. We need it to be deep, punchy, and defined.

**Design decisions:**
- **Sine wave** for a clean sub body (40–60 Hz fundamental)
- **Pitch envelope** for the click/transient (starts high, drops fast)
- **Short decay** for a tight tail (doesn't clash with bass)
- **Subtle saturation** for warmth and audibility on small speakers
- **Tuned to C** — matching our key

```js
setcpm(128/4)

// KICK — Deep, punchy, tuned to C
$: note("c1!4").s("sine")
  .penv(24)                    // pitch starts 24 semitones above C1
  .pdecay(.05)                 // drops to C1 in 50ms (click)
  .decay(.35)                  // body sustains ~350ms
  .sustain(0)                  // no sustain — punchy
  .distort("1.2:.6")           // mild saturation for warmth
  .gain(0.85)
```

**Listen and check:**
- ✅ Can you hear the "click" at the start? That's the pitch envelope.
- ✅ Does it feel punchy, not boomy? If boomy, shorten `.decay`.
- ✅ Does it sit low without rumbling? The sine keeps it clean.

**Why these values?**
- `penv(24)` = 2 octaves of pitch sweep — enough click without being too "ticky"
- `pdecay(.05)` = 50ms — fast enough to sound like a transient, not a tom
- `decay(.35)` = 350ms — leaves room for the bass between kicks
- `distort("1.2:.6")` = very mild — adds 2nd/3rd harmonics so the kick is audible on phone speakers

> 🎧 **A/B test:** Remove `.distort()` and listen. Then add it back. The difference is subtle but important on small speakers.

---

### 3.2 — The Hi-Hats

Hi-hats provide groove and energy. We need 16th notes with a strong accent pattern, subtle swing, and character.

**Design decisions:**
- **TR-909 samples** — the industry standard for techno hats
- **16th note grid** for driving energy
- **Accent pattern** that emphasizes beats 1 and 3 with a peak on the "and" of 2 and "and" of 4
- **Subtle swing** for groove (1/6 = gentle shuffle)
- **HPF at 6 kHz** to remove low-frequency bleed and keep the low end clean for kick/bass
- **Open hat** on the last 16th of every other bar for release

```js
setcpm(128/4)

// HI-HATS — 16ths with groove
$: s("[hh hh hh hh hh hh hh <hh oh>]*2")
  .bank("RolandTR909")
  .gain(".5 .3 .7 .3 .5 .3 1 .3")   // accent pattern
  .hpf(6000)                          // clean up low end
  .swingBy(1/6, 4)                    // subtle shuffle groove
  .cut(1)                             // open hat cut by closed
```

**Listen and check:**
- ✅ Can you feel the *groove*? The swing should make it feel looser than a rigid grid.
- ✅ Are the accents audible on the "and" of 2 and 4? That `.5 .3 .7 .3 .5 .3 1 .3` pattern is key.
- ✅ Does the open hat on every other bar add a sense of breath?

**Why these values?**
- `.gain(".5 .3 .7 .3 .5 .3 1 .3")` — creates a push-pull feel. The loudest hit (1) on the "and" of 4 drives momentum into the next bar.
- `.swingBy(1/6, 4)` — shifts every other 16th note slightly late. `1/6` is subtle enough for techno (more would sound jazzy).
- `.cut(1)` — when the open hat plays, the next closed hat immediately cuts its tail, just like a real hi-hat pedal.

---

### 3.3 — The Clap

Sits on beats 2 and 4. Needs to be snappy with just enough reverb for space.

**Design decisions:**
- **TR-909 clap** — iconic techno sound
- **Short reverb** — adds dimension without washing out
- **Occasional variation** — alternating snare/clap for interest

```js
setcpm(128/4)

// CLAP — Backbeat with space
$: s("~ <cp cp cp [cp cp]> ~ <cp [~ sd]>")
  .bank("RolandTR909")
  .room(0.12).roomsize(0.25)      // tight room, not washy
  .gain(0.65)
  .orbit(1)
```

**Why the pattern `"~ <cp cp cp [cp cp]> ~ <cp [~ sd]>"`?**
- Every bar: clap on beats 2 and 4 (standard backbeat)
- Every 4th bar: a quick double-clap on beat 2 → tiny fill
- Every other bar: beat 4 alternates between clap and a late snare hit → variation without disruption
- This creates a 4-bar cycle of micro-variation that keeps the backbeat alive

---

### 3.4 — The Rimshot

Polyrhythmic texture layer. Uses Euclidean distribution for an organic, non-obvious pattern.

**Design decisions:**
- **Euclidean rhythm** — 5 hits across 16 steps creates an interesting pattern that doesn't align with the kick
- **Random panning** — spreads across the stereo field for width
- **Subtle delay** — adds depth without cluttering
- **Pattern rotation** with `iter` — the starting point shifts each bar

```js
setcpm(128/4)

// RIMSHOT — Euclidean polyrhythm
$: s("rim(5,16)").bank("RolandTR909")
  .gain(0.28)
  .pan(rand)                          // stereo spread
  .delay(0.15).delaytime(0.1875)      // subtle echo
  .delayfeedback(0.3)
  .iter(4)                            // rotate start point
```

**Why Euclidean (5,16)?**
The pattern distributes 5 hits across 16 steps: `x . . x . . x . . x . . x . . .`. This creates a tresillo-influenced rhythm that interlocks with the 4/4 kick, generating polyrhythmic tension. The `iter(4)` rotation means each bar starts at a different position, creating 4 bars of variation from a single line of code.

---

### 3.5 — The Bass

The harmonic foundation. Dark, rhythmic, filtered — follows the root of our chord progression.

**Design decisions:**
- **Sawtooth** — harmonically rich, good raw material for filtering
- **Ladder filter** — aggressive, analog-character low-pass
- **Filter envelope** — opens briefly on each note for the "acid" squelch
- **Short decay** — rhythmic, not sustained (avoids clashing with kick tail)
- **Notes follow the chord roots:** C, C, F, F (per our 4-bar chord loop)

```js
setcpm(128/4)

// BASS — Dark acid saw
$: note("<[c2 c2 c2 c2]*2 [c2 c2 c2 c2]*2 [f2 f2 f2 f2]*2 [f2 f2 f2 f2]*2>")
  .s("sawtooth")
  .lpf(350)                           // dark base cutoff
  .lpq(14)                            // high resonance = squelch
  .lpenv(4)                           // filter opens 4 octaves on each note
  .lpa(0.005)                         // instant filter attack
  .lpd(0.10)                          // filter closes in 100ms
  .lps(0)                             // filter fully closes
  .ftype("ladder")                    // aggressive analog character
  .decay(0.18)                        // tight, rhythmic notes
  .sustain(0)
  .release(0.03)
  .gain(0.55)
```

**Listen and check:**
- ✅ Can you hear the "squelch" on each note? That's the filter envelope opening and closing.
- ✅ Does the bass sit *underneath* the kick without fighting it? The short `.decay(.18)` and low `.lpf(350)` keep it out of the kick's space.
- ✅ Do the root notes change on bars 3–4? You should hear the harmonic shift from C to F.

**Simplify the note pattern:**

The bass pattern above is verbose for clarity. Here's the compact version — same musical result:

```js
$: note("<c2!8 f2!8>")
  .s("sawtooth")
  .lpf(350).lpq(14)
  .lpenv(4).lpa(0.005).lpd(0.10).lps(0)
  .ftype("ladder")
  .decay(0.18).sustain(0).release(0.03)
  .gain(0.55)
```

---

### 3.6 — The Pad

Atmospheric chords that sit behind everything. This is where our harmony lives.

**Design decisions:**
- **Sawtooth** for rich harmonics
- **`chord().voicing()`** for automatic voice-led chords
- **Slow filter modulation** via `sine` signal — the pad breathes
- **Long attack/release** — smooth, no sharp edges
- **Reverb** — medium hall for depth
- **Orbit 2** — isolated for sidechain ducking
- **Low gain** — pads should be *felt*, not dominate

```js
setcpm(128/4)

// PAD — Dark chord wash on orbit 2
$: chord("<Cm!2 Fm!2 Ab!2 Bb!2>").voicing()
  .s("sawtooth")
  .lpf(sine.range(1000, 2800).slow(16))   // breathes over 16 bars
  .lpq(2)                                  // gentle resonance
  .attack(0.5)                              // slow fade in
  .decay(1.0)
  .sustain(0.5)
  .release(1.5)                             // slow fade out
  .room(0.45).roomsize(0.65)                // medium hall
  .roomlp(3000)                             // dark reverb tail
  .gain(0.22)                               // subtle — felt, not heard
  .orbit(2)                                 // isolated for sidechain
```

**Why `chord("<Cm!2 Fm!2 Ab!2 Bb!2>")`?**
- Each chord lasts 2 cycles (2 bars) — slow harmonic rhythm matches minimal techno's patient pacing
- `Cm → Fm → Ab → Bb` = i → iv → VI → VII in C minor — classic dark progression with forward motion
- `.voicing()` automatically applies voice leading so chord tones move smoothly (no jarring jumps)

**Why the slow filter movement?**
- `sine.range(1000, 2800).slow(16)` — the filter cutoff sweeps from 1000 Hz to 2800 Hz and back over 16 bars
- This creates imperceptible timbral evolution — the pad gets brighter and darker without the listener consciously noticing
- This is the "evolve, don't change" principle in action

---

### 3.7 — The Sidechain Pump

The pump ties the pad to the kick rhythm, creating the breathing effect that defines minimal techno.

**Design decisions:**
- **Silent trigger** — we don't hear this pattern, it only ducks orbit 2
- **Medium recovery** — 0.2 seconds = noticeable pump without being extreme
- **Full depth** — complete ducking for a dramatic effect

```js
setcpm(128/4)

// SIDECHAIN — Pumps the pad (orbit 2)
$: s("bd*4").bank("RolandTR909")
  .duckorbit(2)                    // targets orbit 2 (pad)
  .duckattack(0.2)                 // recovery time = pump speed
  .duckdepth(1)                    // full duck = dramatic effect
  .gain(0)                         // silent — trigger only
```

> 🎧 **Test this:** Play the pad alone. Then add the sidechain. The pad should "pump" — ducking on every kick and swelling back between hits. This is the sound of every club ever.

---

### 3.8 — The Lead Melody

Sparse, delayed, pentatonic. Fills the space between kicks with minimal material.

**Design decisions:**
- **Triangle wave** — soft, warm, doesn't compete with saw bass/pad
- **Minor pentatonic scale** — impossible to hit a "wrong" note
- **Only 4 active notes per 2 bars** — space is the melody's best friend
- **Dotted-eighth delay** — fills gaps rhythmically, creating a call-and-response effect
- **`degradeBy(0.3)`** — 30% of notes randomly drop out, creating organic variation
- **Auto-pan** — gentle stereo movement for width without panning tricks

```js
setcpm(128/4)

// LEAD — Sparse pentatonic melody with delay
$: n("<0 ~ 4 ~ , ~ 2 ~ 5>")
  .scale("C4:minor:pentatonic")
  .s("triangle")
  .lpf(3500)
  .decay(0.12).sustain(0).release(0.08)
  .delay(0.45)                         // strong delay send
  .delaytime(0.1875)                   // dotted-eighth feel
  .delayfeedback(0.55)                 // several repeats
  .pan(sine.range(0.3, 0.7).slow(4))  // gentle auto-pan
  .degradeBy(0.25)                     // random note drops
  .gain(0.25)
```

**Why this melody pattern?**
- `"<0 ~ 4 ~ , ~ 2 ~ 5>"` — two layers offset by half a cycle
  - Layer 1: scale degree 0 (C) and 4 (G) on beats 1 and 3
  - Layer 2: scale degree 2 (Eb) and 5 (Bb) on beats 2 and 4
- The result is 4 notes per cycle, but offset so there's always a note on each beat
- The delay echoes fill the 16th-note grid between the played notes
- `degradeBy(0.25)` randomly drops ~25% of notes, so no two bars sound identical

**Why C minor pentatonic?**
- The scale contains: C, Eb, F, G, Bb
- Every note is consonant with both Cm and Fm chords
- It's impossible to play a "clash" — any random selection sounds good
- This is why pentatonic scales are the secret weapon of live coding

---

### 3.9 — Texture Layers

Subtle background elements that add depth and warmth.

```js
setcpm(128/4)

// TEXTURE — Vinyl crackle (warmth)
$: s("crackle*2").density(0.03).gain(0.08)

// TEXTURE — High-frequency noise wash (air)
$: s("brown").begin(rand).end(rand.add(0.008))
  .hpf(3000)
  .gain(0.03)
  .room(0.7).roomsize(0.85)
```

These are almost inaudible on their own — they fill the extreme high-frequency space with subtle texture that makes the track feel "complete" on a sound system.

---

## Part 4 — Combining Everything: The Full Loop

Now let's play all elements together. Copy this entire block into Strudel:

```js
// ╔═══════════════════════════════════════════════════╗
// ║             " P U L S E "                         ║
// ║      Minimal Techno — 128 BPM — C minor           ║
// ╚═══════════════════════════════════════════════════╝

setcpm(128/4)

// ── KICK ──
$: note("c1!4").s("sine")
  .penv(24).pdecay(.05)
  .decay(.35).sustain(0)
  .distort("1.2:.6")
  .gain(0.85)

// ── HI-HATS ──
$: s("[hh hh hh hh hh hh hh <hh oh>]*2")
  .bank("RolandTR909")
  .gain(".5 .3 .7 .3 .5 .3 1 .3")
  .hpf(6000)
  .swingBy(1/6, 4)
  .cut(1)

// ── CLAP ──
$: s("~ <cp cp cp [cp cp]> ~ <cp [~ sd]>")
  .bank("RolandTR909")
  .room(0.12).roomsize(0.25)
  .gain(0.65)
  .orbit(1)

// ── RIMSHOT ──
$: s("rim(5,16)").bank("RolandTR909")
  .gain(0.28)
  .pan(rand)
  .delay(0.15).delaytime(0.1875).delayfeedback(0.3)
  .iter(4)

// ── BASS ──
$: note("<c2!8 f2!8>")
  .s("sawtooth")
  .lpf(350).lpq(14)
  .lpenv(4).lpa(0.005).lpd(0.10).lps(0)
  .ftype("ladder")
  .decay(0.18).sustain(0).release(0.03)
  .gain(0.55)

// ── PAD ──
$: chord("<Cm!2 Fm!2 Ab!2 Bb!2>").voicing()
  .s("sawtooth")
  .lpf(sine.range(1000, 2800).slow(16))
  .lpq(2)
  .attack(0.5).decay(1.0).sustain(0.5).release(1.5)
  .room(0.45).roomsize(0.65).roomlp(3000)
  .gain(0.22)
  .orbit(2)

// ── SIDECHAIN ──
$: s("bd*4").bank("RolandTR909")
  .duckorbit(2).duckattack(0.2).duckdepth(1)
  .gain(0)

// ── LEAD ──
$: n("<0 ~ 4 ~ , ~ 2 ~ 5>")
  .scale("C4:minor:pentatonic")
  .s("triangle")
  .lpf(3500)
  .decay(0.12).sustain(0).release(0.08)
  .delay(0.45).delaytime(0.1875).delayfeedback(0.55)
  .pan(sine.range(0.3, 0.7).slow(4))
  .degradeBy(0.25)
  .gain(0.25)

// ── TEXTURE ──
$: s("crackle*2").density(0.03).gain(0.08)
$: s("brown").begin(rand).end(rand.add(0.008))
  .hpf(3000).gain(0.03)
  .room(0.7).roomsize(0.85)
```

### Mix Check

Listen to the full loop and verify:

| Check | What to listen for | Fix if wrong |
|-------|-------------------|--------------|
| **Kick clarity** | Kick is the loudest, punchiest element | Increase kick `.gain()`, decrease others |
| **Low-end balance** | Kick and bass don't fight | Shorten bass `.decay()` or lower bass `.lpf()` |
| **Sidechain pump** | Pad breathes with the kick | Adjust `.duckattack()` (longer = more pump) |
| **Hi-hat groove** | Feels human, not robotic | Adjust `.swingBy()` amount |
| **Stereo width** | Kick/bass center, hats/rim wide | Check `.pan()` values |
| **Reverb clarity** | Space without mud | Reduce `.room()` or lower `.roomsize()` |
| **Lead audibility** | Melody is present but not dominating | Adjust lead `.gain()` |
| **Overall balance** | No single element is too loud | Adjust individual `.gain()` values |

---

## Part 5 — The Full Arrangement

Now let's arrange "Pulse" into a complete track with an intro, builds, breakdowns, and drops.

### Version A: Scripted Arrangement (play-and-forget)

```js
// ╔═══════════════════════════════════════════════════╗
// ║             " P U L S E "                         ║
// ║   Complete Arrangement — ~4:30 at 128 BPM         ║
// ╚═══════════════════════════════════════════════════╝

setcpm(128/4)

// ═══════════ SOUND DEFINITIONS ═══════════

let kick = note("c1!4").s("sine")
  .penv(24).pdecay(.05).decay(.35).sustain(0)
  .distort("1.2:.6").gain(0.85)

let hats = s("[hh hh hh hh hh hh hh <hh oh>]*2")
  .bank("RolandTR909")
  .gain(".5 .3 .7 .3 .5 .3 1 .3")
  .hpf(6000).swingBy(1/6, 4).cut(1)

let clap = s("~ <cp cp cp [cp cp]> ~ <cp [~ sd]>")
  .bank("RolandTR909")
  .room(0.12).roomsize(0.25).gain(0.65).orbit(1)

let rim = s("rim(5,16)").bank("RolandTR909")
  .gain(0.28).pan(rand)
  .delay(0.15).delaytime(0.1875).delayfeedback(0.3)
  .iter(4)

let bass = note("<c2!8 f2!8>")
  .s("sawtooth")
  .lpf(350).lpq(14)
  .lpenv(4).lpa(0.005).lpd(0.10).lps(0)
  .ftype("ladder")
  .decay(0.18).sustain(0).release(0.03).gain(0.55)

let pad = chord("<Cm!2 Fm!2 Ab!2 Bb!2>").voicing()
  .s("sawtooth")
  .lpf(sine.range(1000, 2800).slow(16)).lpq(2)
  .attack(0.5).decay(1.0).sustain(0.5).release(1.5)
  .room(0.45).roomsize(0.65).roomlp(3000).gain(0.22).orbit(2)

let duck = s("bd*4").bank("RolandTR909")
  .duckorbit(2).duckattack(0.2).duckdepth(1).gain(0)

let lead = n("<0 ~ 4 ~ , ~ 2 ~ 5>")
  .scale("C4:minor:pentatonic").s("triangle")
  .lpf(3500).decay(0.12).sustain(0).release(0.08)
  .delay(0.45).delaytime(0.1875).delayfeedback(0.55)
  .pan(sine.range(0.3, 0.7).slow(4))
  .degradeBy(0.25).gain(0.25)

let texture = stack(
  s("crackle*2").density(0.03).gain(0.08),
  s("brown").begin(rand).end(rand.add(0.008))
    .hpf(3000).gain(0.03).room(0.7).roomsize(0.85)
)

let riser = note(saw.range(40, 72).segment(32).slow(8))
  .s("sine").gain(0.12)

let snareRoll = s("sd").fast(saw.range(1, 16).slow(4))
  .bank("RolandTR909").gain(saw.range(0, 0.45).slow(4))

// ═══════════ ARRANGEMENT ═══════════

arrange(
  // ─── INTRO (16 bars) ───
  // Kick emerges from silence. Texture sets the mood.
  [4,  stack(
    kick.gain(saw.range(0, 0.85).slow(4)),     // kick fades in
    texture
  )],
  [12, stack(kick, texture)],                   // steady kick + atmo

  // ─── BUILD A (8 bars) ───
  // Hi-hats enter filtered, then clap joins
  [4,  stack(
    kick,
    hats.hpf(saw.range(12000, 6000).slow(4)),  // hats filter opens
    texture
  )],
  [4,  stack(kick, hats, clap, texture)],

  // ─── MAIN A (32 bars) ───
  // Full groove: bass and pad enter with sidechain
  [32, stack(kick, hats, clap, rim, bass, pad, duck, texture)],

  // ─── BREAKDOWN (8 bars) ───
  // Strip to atmosphere. Pad opens up. Lead melody enters.
  [8,  stack(
    hats.gain(0.2).hpf(sine.range(4000, 10000).slow(8)),
    pad.room(0.75).roomsize(0.9).gain(0.3),
    lead,
    texture
  )],

  // ─── BUILD B (8 bars) ───
  // Kick returns. Filter sweep. Snare roll builds tension.
  [4,  stack(kick, hats, pad, duck, riser, texture)],
  [4,  stack(kick, hats, clap, snareRoll, pad, duck, riser, texture)],

  // ─── MAIN B / DROP (32 bars) ───
  // Everything! Lead melody over the full groove. Peak energy.
  [16, stack(kick, hats, clap, rim, bass, pad, lead, duck, texture)],
  [16, stack(
    kick, hats, clap, rim, bass, pad, duck, texture,
    lead.sometimes(x => x.add(note(12)))       // octave jumps for peak energy
  )],

  // ─── BREAKDOWN B (8 bars) ───
  [8,  stack(
    pad.room(0.8).roomsize(0.95).gain(0.3),
    lead.delay(0.6).delayfeedback(0.7),        // washy delays
    texture
  )],

  // ─── DROP (16 bars) ───
  // Everything returns for the final push
  [16, stack(kick, hats, clap, rim, bass, pad, lead, duck, texture)],

  // ─── OUTRO (16 bars) ───
  // Elements fade one by one
  [4,  stack(
    kick,
    hats.gain(saw.range(0.5, 0).slow(4)),      // hats fade
    bass.gain(saw.range(0.55, 0).slow(4)),     // bass fades
    texture
  )],
  [4,  stack(kick, texture)],                   // just kick + atmo
  [8,  kick.gain(saw.range(0.85, 0).slow(8))]  // kick fades to silence
)
```

### What Each Section Does

| Section | Bars | Energy | What Happens |
|---------|------|--------|-------------|
| **Intro** | 16 | ▁ → ▂ | Kick fades in from silence. Texture sets vinyl-warmth mood. |
| **Build A** | 8 | ▂ → ▄ | Hi-hats enter through a filter (filtered → open). Clap adds backbeat. |
| **Main A** | 32 | ▅ | Full groove: kick + hats + clap + rim + bass + pad + sidechain. Hypnotic loop. |
| **Breakdown** | 8 | ▅ → ▂ | Kick and bass drop out. Pad reverb swells. Lead melody enters for the first time. |
| **Build B** | 8 | ▂ → ▆ | Kick returns. Riser builds tension. Snare roll accelerates. |
| **Main B** | 32 | ▇ | Peak energy. Full groove + lead melody. Second half adds octave jumps. |
| **Breakdown B** | 8 | ▇ → ▂ | Atmospheric. Only pad + lead with washy delays. |
| **Drop** | 16 | ▇ | Everything returns. Final push. |
| **Outro** | 16 | ▇ → ▁ | Elements fade out one by one until silence. |

---

### Version B: Live Performance Arrangement

For live performance, use this template with `$:` patterns. You control when elements enter and exit:

```js
// ╔═══════════════════════════════════════════════════╗
// ║           " P U L S E "  — Live Set               ║
// ║   Uncomment lines and Ctrl+Enter to add elements  ║
// ╚═══════════════════════════════════════════════════╝

setcpm(128/4)

// ════════════ PHASE 1: KICK ════════════
$: note("c1!4").s("sine")
  .penv(24).pdecay(.05).decay(.35).sustain(0)
  .distort("1.2:.6").gain(0.85)

// ════════════ PHASE 2: + HATS ════════════
// $: s("[hh hh hh hh hh hh hh <hh oh>]*2")
//   .bank("RolandTR909")
//   .gain(".5 .3 .7 .3 .5 .3 1 .3")
//   .hpf(6000).swingBy(1/6, 4).cut(1)

// ════════════ PHASE 3: + CLAP ════════════
// $: s("~ <cp cp cp [cp cp]> ~ <cp [~ sd]>")
//   .bank("RolandTR909")
//   .room(0.12).roomsize(0.25).gain(0.65).orbit(1)

// ════════════ PHASE 4: + BASS ════════════
// $: note("<c2!8 f2!8>").s("sawtooth")
//   .lpf(350).lpq(14)
//   .lpenv(4).lpa(0.005).lpd(0.10).lps(0)
//   .ftype("ladder")
//   .decay(0.18).sustain(0).release(0.03).gain(0.55)

// ════════════ PHASE 5: + PAD + PUMP ════════════
// $: chord("<Cm!2 Fm!2 Ab!2 Bb!2>").voicing()
//   .s("sawtooth")
//   .lpf(sine.range(1000, 2800).slow(16)).lpq(2)
//   .attack(0.5).decay(1.0).sustain(0.5).release(1.5)
//   .room(0.45).roomsize(0.65).roomlp(3000)
//   .gain(0.22).orbit(2)
//
// $: s("bd*4").bank("RolandTR909")
//   .duckorbit(2).duckattack(0.2).duckdepth(1).gain(0)

// ════════════ PHASE 6: + RIMSHOT ════════════
// $: s("rim(5,16)").bank("RolandTR909")
//   .gain(0.28).pan(rand)
//   .delay(0.15).delaytime(0.1875).delayfeedback(0.3)
//   .iter(4)

// ════════════ PHASE 7: + LEAD ════════════
// $: n("<0 ~ 4 ~ , ~ 2 ~ 5>")
//   .scale("C4:minor:pentatonic").s("triangle")
//   .lpf(3500).decay(0.12).sustain(0).release(0.08)
//   .delay(0.45).delaytime(0.1875).delayfeedback(0.55)
//   .pan(sine.range(0.3, 0.7).slow(4))
//   .degradeBy(0.25).gain(0.25)

// ════════════ TEXTURE ════════════
// $: s("crackle*2").density(0.03).gain(0.08)

// ════════════ TOOLS ════════════
// RISER (uncomment for builds):
// $: note(saw.range(40,72).segment(32).slow(8)).s("sine").gain(0.12)

// SNARE ROLL (uncomment for builds):
// $: s("sd").fast(saw.range(1,16).slow(4))
//   .bank("RolandTR909").gain(saw.range(0, 0.45).slow(4))

// CRASH (play once for transitions):
// $: s("cr").bank("RolandTR909")
```

**How to perform:**
1. Start with just the kick (Phase 1). Let it run for 8–16 bars.
2. Uncomment Phase 2 (hats), evaluate (Ctrl+Enter). Wait 4–8 bars.
3. Uncomment Phase 3 (clap). Wait.
4. Continue adding phases...
5. For a **breakdown**: comment out kick + bass → evaluate
6. For a **drop**: uncomment kick + bass + riser → evaluate
7. To **modify live**: change filter values, note patterns, add `.sometimes()`, etc.
8. For the **outro**: comment out elements bottom-to-top, add `.gain(saw.range(x, 0).slow(8))` to fade

---

## Part 6 — Making It Your Own

The track above is a starting point. Here's how to make it uniquely yours:

### Change the Key

Replace `c` with any root note. Update bass notes and pad chords accordingly:

```js
// D minor version
$: note("d1!4").s("sine").penv(24)...       // kick tuned to D
$: note("<d2!8 g2!8>")...                    // bass: D, G
$: chord("<Dm!2 Gm!2 Bb!2 C!2>").voicing()  // pad: Dm Gm Bb C
$: n("<0 ~ 4 ~ , ~ 2 ~ 5>")
  .scale("D4:minor:pentatonic")...           // melody in D minor penta
```

### Change the Groove

```js
// Sparser bass (half as many notes)
note("<c2!4 f2!4>")                          // 4 notes per bar instead of 8

// Pulsing bass (filter rhythm instead of note rhythm)  
note("c2!8").s("sawtooth")
  .lpf("<200 600 400 800 200 600 400 1200>")
  .lpq(10).ftype("ladder")
  .decay(0.15).sustain(0)

// Broken beat hats
s("[hh ~ hh hh ~ hh hh ~]*2")

// Triplet feel
s("hh*12").gain(".5 .3 .7")
```

### Change the Mood

```js
// Brighter: use major chords
chord("<C!2 F!2 Am!2 G!2>").voicing()
n("0 2 4 5").scale("C4:major:pentatonic")

// Darker: use phrygian
chord("<Cm!2 Dbmaj!2 Bbm!2 Abmaj!2>").voicing()
n("0 2 4 5").scale("C4:phrygian")

// Dreamy: use lydian
chord("<Cmaj7!2 Dmaj7!2>").voicing()
n("0 2 4 5").scale("C4:lydian")
```

### Add Variation Over Time

```js
// Hi-hat fills every 4th bar
.lastOf(4, x => x.ply(2))

// Bass reverses every 8th bar
.lastOf(8, x => x.rev())

// Lead gets octave jumps sometimes
.sometimes(x => x.add(note(12)))

// Filter builds over 32 bars
.lpf(saw.range(200, 4000).slow(32))
```

### Swap the Drum Machine

```js
// Switch to TR-808 for a deeper, boomier feel
.bank("RolandTR808")

// Switch to CR-78 for vintage character
.bank("RolandCR78")

// Pattern the bank for variety
.bank("<RolandTR909 RolandTR808>")
```

---

## Part 7 — Checklist Before You "Release"

Before you call the track done, verify:

- [ ] **Kick is punchy and clean** — no mud, clear transient
- [ ] **Bass and kick don't clash** — bass decays before the next kick
- [ ] **Sidechain is audible** — pad pumps with the kick
- [ ] **Hi-hats have groove** — swing is subtle but present
- [ ] **Stereo field is right** — kick/bass center, percussion wide
- [ ] **Pad evolves** — filter is moving, not static
- [ ] **Lead is sparse** — delay fills gaps, not too many notes
- [ ] **Arrangement has dynamics** — intro builds, breakdown strips, drop returns
- [ ] **Transitions feel natural** — no sudden jumps, filter sweeps are smooth
- [ ] **Track can loop** — outro leads back into intro for DJ mixing
- [ ] **No element is too loud** — check in headphones AND laptop speakers

---

## What You've Accomplished

You've built a complete Minimal Techno track from scratch using every technique in this course:

| Technique | Where It's Used |
|-----------|----------------|
| **Cycles & Tempo** (Module 2) | `setcpm(128/4)` — the foundation |
| **Synthesis** (Module 3) | Sine kick, sawtooth bass/pad, triangle lead |
| **Subtractive Synthesis** (Module 4) | Ladder filter on bass, LPF on pad |
| **Samples** (Module 5) | TR-909 hats, clap, rimshot |
| **Envelopes** (Module 6) | Pitch envelope on kick, filter envelope on bass, ADSR on pad |
| **Sound Design** (Module 7) | Every element designed with intention |
| **Chords & Melodies** (Module 8) | `chord().voicing()` pad, pentatonic lead, scale degrees |
| **Effects** (Module 9) | Delay, reverb, distortion, sidechain, panning |
| **Pattern Manipulation** (Module 10) | `iter`, `swingBy`, `degradeBy`, `sometimes`, `lastOf`, signals |
| **Track Structure** (Module 11) | `arrange()` with intro/build/main/breakdown/drop/outro |
| **Live Performance** (Module 12) | `$:` template for real-time control |

**This is your music now. Change it. Break it. Make it yours.**

---

**Back to:** [← Course Overview](./README.md)
