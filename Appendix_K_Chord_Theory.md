# Appendix K — The Theory of Perfect Chords

> **A deep dive into the music theory behind building chords that work — from the physics of sound to genre-specific recipes you can paste into Strudel.**

This appendix goes beyond Module 8's practical introduction to chords. Here, we explore *why* certain combinations of notes sound good together, how to build any chord from first principles, and how different electronic music genres use harmony to create their signature moods.

> [!NOTE]
> **This appendix is reference material, not a prerequisite.** You can build great tracks with just Module 8's knowledge. Come here when you want to understand the *why* behind the *what*, or when you need chord ideas for a specific genre.

---

## K.1 Why Chords Work — The Physics of Harmony

Before we talk about chord *types*, let's understand why certain notes sound good together at all. The answer isn't cultural preference — it's physics.

### The Overtone Series

When you play a note — say, A2 at 110 Hz — you don't hear just one frequency. You hear a **fundamental** (110 Hz) plus a series of quieter **overtones** at exact integer multiples:

```
Overtone Series for A2 (110 Hz):
─────────────────────────────────────────────────────────
Harmonic   Frequency    Note       Interval from Root
─────────────────────────────────────────────────────────
1st        110 Hz       A2         Fundamental (root)
2nd        220 Hz       A3         Octave
3rd        330 Hz       E4         Perfect 5th + octave
4th        440 Hz       A4         2 octaves
5th        550 Hz       C#5        Major 3rd + 2 octaves
6th        660 Hz       E5         Perfect 5th + 2 octaves
7th        770 Hz       ~G5        Minor 7th + 2 octaves (slightly flat)
8th        880 Hz       A5         3 octaves
─────────────────────────────────────────────────────────
```

Notice something? The first intervals that appear naturally in the overtone series are the **octave** (2:1), the **perfect fifth** (3:2), the **perfect fourth** (4:3), and the **major third** (5:4). These are the most consonant intervals in Western music — *because they already exist inside every single note you play*.

### Frequency Ratios and Consonance

The simpler the frequency ratio between two notes, the more **consonant** (pleasant, stable) they sound together:

| Ratio | Interval | Consonance |
|-------|----------|------------|
| 2:1 | Octave | Maximum — sounds like "the same note" |
| 3:2 | Perfect 5th | Very consonant — open, powerful |
| 4:3 | Perfect 4th | Consonant — stable, open |
| 5:4 | Major 3rd | Consonant — bright, warm |
| 6:5 | Minor 3rd | Consonant — dark, emotional |
| 45:32 | Tritone | Maximum dissonance — tense, unstable |

**Why?** When two frequencies have a simple ratio, their overtones *align*. When overtones align, your ear hears a smooth, fused sound. When they don't, you hear **beating** — a rapid wobbling effect caused by two close-but-not-matching frequencies interfering with each other.

### Beating Frequencies — The Source of Dissonance

When two notes with close frequencies are played together, you hear a periodic pulsing called **beats**. The beat frequency equals the difference between the two frequencies:

```
Beat Frequency = |f₁ - f₂|

Example:
  A4 = 440 Hz
  Bb4 = 466 Hz
  Beat frequency = 26 Hz → fast, rough "buzzing" = dissonance

  A4 = 440 Hz
  E5 = 660 Hz
  Beat frequency = 220 Hz → too fast to hear as beating → smooth = consonance
```

- **< 20 Hz difference** → slow beats, heard as a "wah-wah" pulsing
- **20–50 Hz difference** → fast beating, perceived as **roughness** (dissonance)
- **> 50 Hz difference** → too fast to perceive as beats → smooth (or heard as two separate pitches)

This is why chords sound muddy in low registers — bass notes are so close in frequency that even "consonant" intervals produce audible beating. **Rule of thumb: keep chords above C3. Below that, use only octaves and fifths.**

```js
// ❌ Muddy — minor 3rd in bass register
note("[c2,eb2,g2]").s("sawtooth").lpf(1000)

// ✅ Clear — same chord, higher register
note("[c3,eb3,g3]").s("sawtooth").lpf(2000)

// ✅ Also clear — bass uses only the root
$: note("c2").s("sine").lpf(200)
$: note("[c3,eb3,g3]").s("sawtooth").lpf(2000)
```

### What This Means for Chord Building

1. **Octaves and 5ths are always safe** — they have the simplest ratios
2. **3rds give chords their character** — major 3rd = bright, minor 3rd = dark
3. **7ths and extensions add "color"** — more complex ratios = more tension
4. **Low-register chords need wide spacing** — to avoid beating/muddiness
5. **Dissonance isn't bad** — it creates tension that makes resolution feel good

---

## K.2 The Complete Interval Map

Every chord is built from **intervals** — the distance in semitones between two notes. Here is the complete chromatic interval map:

| Semitones | Name | Quality | Character | Strudel Example |
|-----------|------|---------|-----------|----------------|
| 0 | Unison | Perfect | Identical | `note("[c3,c3]")` |
| 1 | Minor 2nd | Dissonant | Tension, clash, "horror movie" | `note("[c3,db3]")` |
| 2 | Major 2nd | Mild dissonance | Bright tension, passing | `note("[c3,d3]")` |
| 3 | **Minor 3rd** | Consonant | **Dark, sad, emotional** | `note("[c3,eb3]")` |
| 4 | **Major 3rd** | Consonant | **Bright, happy, warm** | `note("[c3,e3]")` |
| 5 | Perfect 4th | Consonant | Open, strong, suspended | `note("[c3,f3]")` |
| 6 | Tritone | Dissonant | Maximum tension, "the devil's interval" | `note("[c3,fs3]")` |
| 7 | **Perfect 5th** | Consonant | **Powerful, stable, open** | `note("[c3,g3]")` |
| 8 | Minor 6th | Consonant | Bittersweet | `note("[c3,ab3]")` |
| 9 | Major 6th | Consonant | Warm, gentle | `note("[c3,a3]")` |
| 10 | Minor 7th | Mild dissonance | Jazzy, bluesy, wants to resolve | `note("[c3,bb3]")` |
| 11 | Major 7th | Dissonance | Dreamy, sophisticated, tense | `note("[c3,b3]")` |
| 12 | Octave | Perfect | Same note, different register | `note("[c3,c4]")` |

> 💡 **Exercise:** Play each interval above in Strudel. Listen to how the feeling changes as you move from 0 to 12 semitones. Notice the "tension curve" — low at 0, 5, 7, and 12 (consonant); high at 1, 6, and 11 (dissonant).

### The Three Categories

**Consonant** (stable, restful): Unison, minor 3rd, major 3rd, perfect 4th, perfect 5th, minor 6th, major 6th, octave

**Mildly dissonant** (tension, but usable): Major 2nd, minor 7th

**Dissonant** (clash, needs resolution): Minor 2nd, tritone, major 7th

In Minimal Techno, you'll mostly use the **consonant** intervals for pads and the **mildly dissonant** ones for adding depth to 7th chords. The **dissonant** intervals are used sparingly — for tension, transitions, or intentional unease.

---

## K.3 Triads — The Foundation of All Chords

A **triad** is three notes stacked in thirds. There are four types, each built from a different combination of major and minor thirds:

### The Four Triad Types

```
Major Triad: Root + Major 3rd (4) + Perfect 5th (7)
             Intervals: [0, 4, 7]
             Sound: Bright, happy, resolved
             Example: C Major = C, E, G

Minor Triad: Root + Minor 3rd (3) + Perfect 5th (7)
             Intervals: [0, 3, 7]
             Sound: Dark, sad, emotional
             Example: C Minor = C, Eb, G

Diminished:  Root + Minor 3rd (3) + Diminished 5th (6)
             Intervals: [0, 3, 6]
             Sound: Tense, unstable, "dark and scary"
             Example: C Dim = C, Eb, Gb

Augmented:   Root + Major 3rd (4) + Augmented 5th (8)
             Intervals: [0, 4, 8]
             Sound: Mysterious, dreamy, unresolved
             Example: C Aug = C, E, G#
```

### Hear Them All

```js
setcpm(128/4)

// Major — bright
$: note("<[c3,e3,g3] ~ ~ ~>")
  .s("sawtooth").lpf(2000).attack(0.2).release(0.8)

// Minor — dark (⭐ most common in techno)
$: note("<~ [c3,eb3,g3] ~ ~>")
  .s("sawtooth").lpf(2000).attack(0.2).release(0.8)

// Diminished — tense
$: note("<~ ~ [c3,eb3,gb3] ~>")
  .s("sawtooth").lpf(2000).attack(0.2).release(0.8)

// Augmented — mysterious
$: note("<~ ~ ~ [c3,e3,gs3]>")
  .s("sawtooth").lpf(2000).attack(0.2).release(0.8)
```

### Which Triads for Which Genre?

| Triad | Techno | Deep House | Ambient | Trance |
|-------|--------|-----------|---------|--------|
| **Minor** | ⭐⭐⭐ Primary | ⭐⭐ Common | ⭐⭐ Common | ⭐⭐ Common |
| **Major** | ⭐ Rare | ⭐⭐ Common | ⭐⭐ Common | ⭐⭐⭐ Primary |
| **Diminished** | ⭐ Tension | ⭐ Rare | ⭐ Rare | ⭐ Transition |
| **Augmented** | ⭐ FX | ⭐ Rare | ⭐⭐ Texture | ⭐ FX |

> **Bottom line:** If you're making techno, start with **minor triads**. They're your bread and butter. Everything else builds on top of them.

---

## K.4 Seventh Chords — Adding Tension and Color

Seventh chords add a **fourth note** — the 7th scale degree — to a triad. This extra note adds harmonic tension, richness, and "sophistication." In electronic music, 7th chords are used for pads, stabs, and adding emotional depth.

### The Five Seventh Chord Types

| Type | Formula (semitones) | Symbol | Sound | Example (C root) |
|------|-------------------|--------|-------|-------------------|
| **Major 7th** | [0, 4, 7, 11] | Cmaj7 | Dreamy, lush, sophisticated | C, E, G, B |
| **Minor 7th** | [0, 3, 7, 10] | Cm7 | Dark, smooth, jazzy | C, Eb, G, Bb |
| **Dominant 7th** | [0, 4, 7, 10] | C7 | Bluesy, wants to resolve | C, E, G, Bb |
| **Half-Diminished** | [0, 3, 6, 10] | Cø7 | Dark, tense, unresolved | C, Eb, Gb, Bb |
| **Fully Diminished** | [0, 3, 6, 9] | C°7 | Maximum tension, symmetrical | C, Eb, Gb, A |

### Hear the Difference

```js
setcpm(60/4)

// Major 7th — dreamy, "coffee shop" vibe
note("[c3,e3,g3,b3]").s("triangle")
  .lpf(3000).attack(0.3).release(1.5).room(0.4)
```

```js
// Minor 7th — dark and smooth (⭐ best for techno pads)
note("[c3,eb3,g3,bb3]").s("sawtooth")
  .lpf(2000).attack(0.3).release(1.2).room(0.4)
```

```js
// Dominant 7th — bluesy tension
note("[c3,e3,g3,bb3]").s("sawtooth")
  .lpf(2500).attack(0.2).release(1.0)
```

### Best Seventh Chords for Electronic Music

| Genre | Best 7th Chords | Why |
|-------|----------------|-----|
| **Minimal Techno** | Cm7, Fm7 | Dark + smooth, not too complex |
| **Deep House** | Cm7, Dm7, Fmaj7 | Smooth, jazzy warmth |
| **Melodic Techno** | Cm7, Abmaj7, Bbmaj7 | Emotional + sophisticated |
| **Ambient** | Cmaj7, Fmaj7, Am7 | Dreamy, floating quality |
| **Acid House** | C7, F7 | Bluesy, raw |

### In Strudel

```js
setcpm(128/4)
// Dark minimal techno pad using minor 7th chords
$: chord("<Cm7 Fm7 Abmaj7 Bb>").voicing()
  .s("sawtooth")
  .lpf(sine.range(800, 2200).slow(16)).lpq(2)
  .attack(0.5).sustain(0.5).release(1.5)
  .room(0.5).roomsize(0.7)
  .gain(0.22)
```

---

## K.5 Extended Chords — Color and Sophistication

Beyond 7ths, you can keep stacking thirds to create **extended chords** — 9ths, 11ths, and 13ths. These chords add "color" and complexity without fundamentally changing the harmonic function.

### Extended Chord Types

| Chord | Formula (semitones) | Notes (C root) | Character |
|-------|-------------------|-----------------|-----------|
| **9th** (minor) | [0, 3, 7, 10, 14] | C, Eb, G, Bb, D | Rich, soulful, deep |
| **9th** (major) | [0, 4, 7, 11, 14] | C, E, G, B, D | Lush, dreamy, jazz |
| **11th** | [0, 3, 7, 10, 14, 17] | C, Eb, G, Bb, D, F | Dense, orchestral |
| **add9** | [0, 3, 7, 14] | C, Eb, G, D | Spacious, modern |
| **sus2** | [0, 2, 7] | C, D, G | Open, ambiguous |
| **sus4** | [0, 5, 7] | C, F, G | Suspended, unresolved |

### The Practical Reality

In electronic music, you **rarely** use full extended chords. They're too dense for a mix that already has kick, bass, hats, and percussion. Instead, you:

1. **Use the extension as a "color note"** — add just the 9th to a triad, omitting the 7th
2. **Spread notes across octaves** — use voicings that put the extension high and the root low
3. **Use sus chords** — sus2 and sus4 are incredible for techno because they avoid the 3rd, making the chord neither major nor minor — just *open*

### Building Extended Chords in Strudel

```js
setcpm(128/4)

// Minor 9th — rich, soulful pad
$: note("[c3,eb3,g3,bb3,d4]").s("sawtooth")
  .lpf(1800).lpq(2)
  .attack(0.4).sustain(0.5).release(1.5)
  .room(0.5).gain(0.2)
```

```js
// add9 — spacious, modern (omit the 7th for clarity)
$: note("[c3,eb3,g3,d4]").s("triangle")
  .lpf(3000).attack(0.3).release(1.2)
  .room(0.4).gain(0.22)
```

```js
// sus2 — open, ambiguous (⭐ great for techno transitions)
$: note("<[c3,d3,g3] [f3,g3,c4] [ab3,bb3,eb4] [bb3,c4,f4]>")
  .s("sawtooth").lpf(2000)
  .attack(0.4).sustain(0.5).release(1.2)
  .room(0.4).gain(0.2)
```

```js
// sus4 — suspended, unresolved → resolving to minor
$: note("<[c3,f3,g3] [c3,eb3,g3]>").s("sawtooth")
  .lpf(2000).attack(0.3).release(0.8)
```

### Using `addVoicings()` for Extended Chords

Strudel's built-in `chord()` doesn't know every extension. Use `addVoicings()` to define custom chords:

```js
setcpm(128/4)

addVoicings('extended', {
  'm9':   ['3 7 10 14', '10 14 15 19'],     // minor 9th
  'maj9': ['4 7 11 14', '11 14 16 19'],      // major 9th
  'add9': ['3 7 14', '7 14 15'],             // minor add9
  'sus2': ['2 7', '7 14'],                    // suspended 2nd
  'sus4': ['5 7', '7 12 17'],                 // suspended 4th
})

chord("<Cm9 Fm Abmaj9 Bb>")
  .dict('extended')
  .anchor(60)
  .voicing()
  .s("sawtooth")
  .lpf(sine.range(1000, 2500).slow(8))
  .attack(0.4).release(1.2)
  .room(0.5).gain(0.2)
```

---

## K.6 Chord Inversions — Smooth Movement

An **inversion** rearranges which note is on the bottom of a chord. Same notes, different bass note — completely different feel.

### The Three Inversions of a Triad

```
C Minor Triad:
─────────────────────────────────────────
Root Position:    C  - Eb - G      (root on bottom)
1st Inversion:    Eb - G  - C      (3rd on bottom)
2nd Inversion:    G  - C  - Eb     (5th on bottom)
─────────────────────────────────────────
```

### Why Inversions Matter

1. **Voice leading** — inversions allow chords to move with small, smooth note movements instead of large jumps
2. **Bass control** — you can change which note is "felt" as the foundation
3. **Variety** — the same chord sounds different in each inversion, giving you more sonic options from the same harmonic material

### Hearing the Difference

```js
setcpm(60/4)

// Root position — standard, grounded
note("[c3,eb3,g3]").s("sawtooth").lpf(2000).attack(0.2).release(1)
```

```js
// 1st inversion — lighter, more open
note("[eb3,g3,c4]").s("sawtooth").lpf(2000).attack(0.2).release(1)
```

```js
// 2nd inversion — floating, less stable
note("[g3,c4,eb4]").s("sawtooth").lpf(2000).attack(0.2).release(1)
```

### Inversions for Smooth Progressions

Without inversions, moving from Cm to Fm requires large jumps:

```
Cm (root):  C3  Eb3  G3
Fm (root):  F3  Ab3  C4    ← every note jumps 4-5 semitones
```

With inversions, you can minimize movement:

```
Cm (root):   C3  Eb3  G3
Fm (2nd inv): C3  F3   Ab3  ← C stays, others move 1-2 semitones
```

```js
setcpm(128/4)

// Without inversions — jumpy
$: note("<[c3,eb3,g3] [f3,ab3,c4] [ab3,c4,eb4] [bb3,d4,f4]>")
  .s("sawtooth").lpf(2000).attack(0.3).release(0.8).gain(0.2)

// With inversions — smooth voice leading
$: note("<[c3,eb3,g3] [c3,f3,ab3] [c3,eb3,ab3] [bb2,d3,f3]>")
  .s("sawtooth").lpf(2000).attack(0.3).release(0.8).gain(0.2)
```

> 💡 **Tip:** Strudel's `.voicing()` handles inversions automatically via voice leading. But understanding inversions lets you build custom voicings when you want precise control.

---

## K.7 Voicing Techniques — How You Spread the Notes

**Voicing** is the art of arranging chord notes across the frequency spectrum. The same chord can sound completely different depending on how you voice it.

### Close vs. Open Voicing

**Close voicing** — all notes within one octave:
```
Cm close: C3, Eb3, G3        (span: 7 semitones)
```

**Open voicing** — notes spread across multiple octaves:
```
Cm open:  C3, G3, Eb4        (span: 15 semitones)
```

```js
setcpm(128/4)

// Close voicing — dense, focused, good for stabs
$: note("[c3,eb3,g3]").s("sawtooth")
  .lpf(3000).decay(0.15).sustain(0).gain(0.3)

// Open voicing — spacious, atmospheric, good for pads
$: note("[c3,g3,eb4]").s("sawtooth")
  .lpf(2000).attack(0.4).release(1.2).room(0.5).gain(0.2)
```

### The "Low Note Spacing" Rule

A critical rule for good-sounding chords:

> **The lower the register, the wider the intervals should be.**

This is because of beating frequencies (Section K.1). In low registers, even consonant intervals create muddiness.

```
✅ Good spacing:                    ❌ Bad spacing:
─────────────────────               ─────────────────
C2 ─────── (root, bass)             C2 ─── (root)
           (wide gap)               Eb2 ── (minor 3rd — too close!)
G3 ─────── (5th, mid)               G2 ─── (5th — still muddy)
Eb4 ────── (3rd, treble)            Bb2 ── (7th — total mud)
```

### Drop-2 Voicing — The Secret Weapon

**Drop-2** takes the second note from the top of a close voicing and drops it down an octave. This creates a balanced, professional-sounding chord used extensively in jazz and electronic music:

```
Close:   C3, Eb3, G3, Bb3
Drop-2:  G2, C3, Eb3, Bb3     (G dropped one octave)
```

```js
setcpm(128/4)

// Close voicing Cm7 — dense, can sound cluttered
note("[c3,eb3,g3,bb3]").s("sawtooth")
  .lpf(2000).attack(0.3).release(1).gain(0.2)
```

```js
// Drop-2 voicing Cm7 — ⭐ balanced, professional
note("[g2,c3,eb3,bb3]").s("sawtooth")
  .lpf(2000).attack(0.3).release(1).gain(0.2)
```

### Voicing Cheatsheet

| Voicing | When to Use | Sound |
|---------|-------------|-------|
| **Close** | Stabs, rhythmic chords, higher register | Dense, focused, punchy |
| **Open** | Pads, atmospheric, breakdowns | Spacious, wide, ambient |
| **Drop-2** | Pads, leads, any register | Balanced, professional |
| **Root + 5th only** | Bass, power chords | Massive, open, genre-neutral |
| **Root + 10th** | Arpeggios, minimal voicings | Clarity, space |

> **For Strudel:** Use `.voicing()` for automatic smart voicings. Use `.anchor("c4")` + `.mode("below")` to control where notes land. For full control, spell out notes manually.

---

## K.8 Voice Leading — The Art of Smooth Transitions

**Voice leading** is how individual notes move from one chord to the next. Good voice leading makes progressions sound smooth and musical. Bad voice leading makes them sound jarring and amateur.

### The Three Rules

1. **Move each note as little as possible** — prefer stepwise motion (1–2 semitones) over jumps
2. **Keep common tones** — if two chords share a note, don't move it
3. **Move voices in contrary motion** — when one note goes up, another goes down

### Example: Cm → Fm

**Bad voice leading** (parallel motion, big jumps):
```
Cm:  C3 → Eb3 → G3
Fm:  F3 → Ab3 → C4      ← every note jumps up 5 semitones
```

**Good voice leading** (smooth, minimal movement):
```
Cm:  C3 → Eb3 → G3
Fm:  C3 → F3  → Ab3     ← C stays, Eb→F (+2), G→Ab (+1)
```

### Why `.voicing()` Matters

Strudel's `.voicing()` function **automatically applies voice leading**. It tracks which notes were played in the previous chord and finds the closest arrangement for the next chord. This is why:

```js
// This sounds smooth — .voicing() handles transitions
chord("<Cm Fm Ab Bb>").voicing()

// This sounds jumpy — raw chord names without voice leading
note("<[c3,eb3,g3] [f3,ab3,c4] [ab3,c4,eb4] [bb3,d4,f4]>")
```

Always use `.voicing()` when writing chord progressions in Strudel unless you have a specific reason to voice chords manually.

---

## K.9 The Chords of Minimal Techno

Now that we have the theory, let's apply it to the genre this course focuses on. Minimal Techno has a very specific relationship with harmony.

### The Philosophy: Less is More

Minimal Techno uses chords differently from pop, jazz, or even other electronic genres:

- **Fewer chords** — many tracks use only 1 or 2 chords for the entire duration
- **Slower harmonic rhythm** — chords change every 4, 8, or even 16 bars
- **Filtered chords** — the filter IS the harmonic movement (opening/closing the LPF changes which overtones are heard, effectively "changing" the chord's character without changing notes)
- **Mood over movement** — the goal is hypnosis, not surprise

### Why C Minor?

C minor is the most common key in Minimal Techno for practical reasons:

1. **C is the easiest root** — no sharps or flats in the root
2. **Minor = dark** — sets the right mood instantly
3. **Good bass range** — C2 at 65 Hz sits perfectly in the sub-bass sweet spot
4. **History** — Robert Hood, Richie Hawtin, and many foundational minimal artists used C minor extensively

### The Essential Minimal Techno Progressions

**The One-Chord Track** — Yes, this works:
```js
setcpm(128/4)
// A single minor chord, filtered slowly
$: chord("Cm").voicing()
  .s("sawtooth")
  .lpf(sine.range(400, 3000).slow(32)).lpq(3)
  .attack(0.3).sustain(0.5).release(1.2)
  .room(0.4).roomsize(0.6)
  .gain(0.22)
```

**The Two-Chord Hypnotic Loop** — i → iv:
```js
setcpm(128/4)
// ⭐ The most classic minimal techno progression
$: chord("<Cm!4 Fm!4>").voicing()
  .s("sawtooth")
  .lpf(sine.range(800, 2500).slow(16)).lpq(2)
  .attack(0.5).sustain(0.5).release(1.5)
  .room(0.45).roomsize(0.65)
  .gain(0.22)
```

**The Dark Minor Descent** — i → VII → VI → V:
```js
setcpm(128/4)
$: chord("<Cm Bb Ab G>").voicing()
  .s("sawtooth")
  .lpf(sine.range(1000, 2800).slow(16)).lpq(2)
  .attack(0.5).sustain(0.5).release(1.5)
  .room(0.45).roomsize(0.65)
  .gain(0.22)
```

**The Dramatic Sweep** — i → VI → III → VII:
```js
setcpm(128/4)
$: chord("<Cm Ab Eb Bb>").voicing()
  .s("sawtooth")
  .lpf(2000)
  .attack(0.4).decay(1.0).sustain(0.4).release(1.5)
  .room(0.5).roomsize(0.7)
  .gain(0.2)
```

### Pad vs. Stab Voicings

The **same chord progression** sounds completely different depending on whether you use it as a **pad** (long, sustained) or a **stab** (short, rhythmic):

```js
setcpm(128/4)

// As a pad — atmospheric, background
$: chord("<Cm Fm>").voicing()
  .s("sawtooth")
  .lpf(1500).attack(0.5).sustain(0.6).release(1.5)
  .room(0.5).gain(0.2)

// As a stab — rhythmic, driving
$: chord("<Cm Fm>*4").voicing()
  .s("sawtooth")
  .lpf(3000).decay(0.1).sustain(0)
  .gain(0.35)
```

### The Filter-as-Progression Technique

One of the most powerful techniques in Minimal Techno: use a **single chord** and let the **filter** create the harmonic movement. As an LPF opens, you progressively reveal more overtones — effectively "brightening" the chord over time. As it closes, the chord becomes darker and more subdued.

```js
setcpm(128/4)

// Single chord, filter creates all the movement
$: chord("Cm7").voicing()
  .s("sawtooth")
  .lpf(sine.range(200, 4000).slow(64)).lpq(4)
  .attack(0.2).sustain(0.8).release(1.0)
  .room(0.3)
  .gain(0.25)
```

This is why many minimal techno tracks can sustain a single chord for 5+ minutes without sounding boring — the filter automates the evolution.

---

## K.10 Genre Chord Cookbook

Here are complete, ready-to-paste chord recipes for different electronic music genres. Each includes the typical key, progression, voicing style, sound design, and a working Strudel example.

---

### 🏠 Deep House (118–122 BPM)

**Character:** Warm, soulful, jazzy. Uses 7th chords extensively. Major keys are more common than in techno.

**Typical keys:** A minor, D minor, F major, G minor

**Signature chords:** Minor 7ths, Major 7ths, Dominant 9ths

**Classic progressions:**
- ii → V → I (jazz-influenced)
- i → iv → VII → III
- vi → IV → I → V

```js
setcpm(120/4)
// Deep house pad — warm, jazzy
$: chord("<Dm7 G7 Cmaj7 Fmaj7>").voicing()
  .s("triangle")
  .lpf(2500).lpq(1)
  .attack(0.3).sustain(0.6).release(1.2)
  .room(0.35).roomsize(0.5)
  .gain(0.25)
```

---

### 🌊 Dub Techno (120–130 BPM)

**Character:** Hypnotic, submerged, washed-out. Heavy reverb and delay on everything. Often a single chord for the entire track.

**Typical keys:** C minor, A minor, D minor

**Signature chords:** Simple minor triads or single notes with layers of effects creating "harmonic" texture

**Classic approach:** One chord → automate filter → massive reverb + delay chain

```js
setcpm(124/4)
// Dub techno chord — submerged in reverb and delay
$: chord("Am").voicing()
  .s("sawtooth")
  .lpf(sine.range(400, 1800).slow(32)).lpq(3)
  .attack(0.4).sustain(0.6).release(2.0)
  .room(0.8).roomsize(0.95).roomlp(2000)
  .delay(0.6).delaytime(0.375).delayfeedback(0.7)
  .gain(0.18)
```

---

### 🌿 Ambient (No Fixed Tempo)

**Character:** Floating, textural, timeless. Chords evolve through sound design rather than harmonic motion. Extended chords and modal harmony.

**Typical keys:** Any. Often modal (Lydian, Mixolydian) rather than major/minor.

**Signature chords:** Maj7, add9, sus2, clusters

**Classic approach:** Layered drones with slow modulation, stacked 5ths, avoid strong resolution

```js
setcps(0.08)
// Ambient pad — floating, evolving
$: note("[c3,g3,d4,a4]").s("triangle")
  .lpf(sine.range(800, 3000).slow(64))
  .attack(2.0).sustain(0.8).release(4.0)
  .room(0.9).roomsize(0.98).roomlp(3000)
  .gain(0.15)

$: note("[f3,c4,g4,e5]").s("sine")
  .lpf(perlin.range(600, 2500).slow(32))
  .attack(3.0).sustain(0.9).release(5.0)
  .room(0.9).roomsize(0.98)
  .gain(0.1)
```

---

### 🎹 Melodic Techno (124–130 BPM)

**Character:** Emotional, cinematic, driving. More complex chords than minimal techno. Uses arpeggiated progressions and layered pads.

**Typical keys:** A minor, C minor, D minor, F minor

**Signature chords:** Minor 7ths, suspensions resolving to minor, the "Anjuna" sound

**Classic progressions:**
- i → VI → III → VII (the "Melodic Techno" progression)
- i → iv → v → VI
- i → III → VII → iv

```js
setcpm(126/4)
// Melodic techno — emotional, layered
$: chord("<Am Fmaj7 C Em>").voicing()
  .s("sawtooth")
  .lpf(sine.range(1200, 3500).slow(16)).lpq(2)
  .attack(0.4).sustain(0.5).release(1.5)
  .room(0.5).roomsize(0.7).roomlp(3000)
  .gain(0.2)

// Arpeggio layer over the same progression
$: n("<[0 2 4 7] [0 2 4 5] [0 2 4 7] [0 2 4 6]>*2")
  .scale("A3:minor")
  .s("triangle")
  .lpf(4000).decay(0.12).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .gain(0.18)
```

---

### 🔮 Trance (136–150 BPM)

**Character:** Euphoric, powerful, anthemic. Uses strong chord progressions with clear resolution. Major keys for uplift, minor keys for "dark trance."

**Typical keys:** A minor, D minor, C major, G major

**Signature chords:** Power chords (root + 5th), supersaw pads with major progressions

**Classic progressions:**
- I → V → vi → IV (the "anthem" progression)
- vi → IV → I → V
- i → III → VII → VI (dark trance)

```js
setcpm(140/4)
// Trance pad — euphoric, layered supersaws
$: chord("<Am F C G>").voicing()
  .s("sawtooth")
  .lpf(sine.range(2000, 6000).slow(8)).lpq(1)
  .attack(0.2).sustain(0.7).release(1.0)
  .room(0.4).roomsize(0.6)
  .gain(0.25)
```

---

### 🥁 Drum & Bass (170–180 BPM)

**Character:** Fast, energetic. Liquid DnB uses lush chords; neurofunk uses minimal/dark chords. Extended chords are common in liquid.

**Typical keys:** A minor, D minor, G minor

**Signature chords:** Am9, Cmaj7, Em7 (liquid); simple minor triads (dark/neuro)

**Classic progressions:**
- vi → IV → V → I (liquid)
- vi → VII → i (dark)
- Am9 → Cmaj7 → Em7 → Fmaj7 (liquid anthem)

```js
setcpm(174/4)
// Liquid DnB pad — lush, emotional
$: chord("<Am Cmaj7 Em7 Fmaj7>").voicing()
  .s("sawtooth")
  .lpf(3000).lpq(1)
  .attack(0.2).sustain(0.5).release(1.0)
  .room(0.4).roomsize(0.5)
  .gain(0.22)
```

---

### 🧪 Acid House (120–130 BPM)

**Character:** Squelchy, raw, minimal harmony. The TB-303's resonant filter is the main harmonic element, not traditional chords.

**Typical keys:** C minor, A minor

**Signature sound:** Single notes through a resonant LPF with high Q, creating "harmonic" texture from the filter resonance itself

```js
setcpm(126/4)
// Acid bass — the 303 IS the chord
$: note("<c2 c2 eb2 c2 f2 f2 c2 c2>")
  .s("sawtooth")
  .lpf(sine.range(200, 4000).fast(2)).lpq(18)
  .ftype("ladder")
  .decay(0.18).sustain(0)
  .distort(0.3)
  .gain(0.45)
```

---

### 🌙 Lo-Fi / Downtempo (80–100 BPM)

**Character:** Warm, nostalgic, slightly detuned. Jazz-influenced 7th and 9th chords. Intentional "imperfection."

**Typical keys:** Eb major, Bb major, C minor, F minor

**Signature chords:** Maj7, m9, dim7, add9

```js
setcpm(85/4)
// Lo-fi pad — warm, detuned, jazzy
$: chord("<Ebmaj7 Cm7 Fm7 Bb7>").voicing()
  .s("sawtooth")
  .lpf(1800).lpq(1)
  .attack(0.3).sustain(0.5).release(1.5)
  .room(0.3)
  .gain(0.22)
```

---

## K.11 Building Your Own Voicing Dictionary

Strudel's `addVoicings()` function lets you define exactly how chords should be arranged. This is the most powerful tool for creating genre-specific harmonic textures.

### How It Works

```js
addVoicings('dictionaryName', {
  'chordQuality': ['voicing1', 'voicing2', ...],
  // ...
})
```

Each voicing is a string of **semitone intervals from the root**, separated by spaces:

| Interval | Semitones | Name |
|----------|-----------|------|
| Minor 3rd | 3 | Dark character note |
| Major 3rd | 4 | Bright character note |
| Perfect 5th | 7 | Stability |
| Minor 7th | 10 | Jazzy tension |
| Major 7th | 11 | Dreamy tension |
| Octave | 12 | Reinforcement |
| Minor 9th | 13 | Dark color |
| Major 9th | 14 | Bright color |
| Minor 10th | 15 | = Minor 3rd, octave up |
| Major 10th | 16 | = Major 3rd, octave up |
| Perfect 11th | 17 | Suspension |
| Perfect 12th | 19 | = 5th, octave up |

### Genre-Specific Dictionaries

**Minimal Techno Dictionary** — dark, sparse, open voicings:
```js
addVoicings('minimalTechno', {
  'm':    ['3 7 12', '7 12 15'],           // minor: open, dark
  'm7':   ['3 7 10', '10 15 19'],          // minor 7: jazzy distance
  'maj':  ['4 7 12', '7 12 16'],           // major: bright, rare
  '':     ['7 12', '12 19'],               // power chord: root+5th
})

chord("<Cm!4 Fm!4>")
  .dict('minimalTechno')
  .anchor(60).voicing()
  .s("sawtooth")
  .lpf(sine.range(800, 2500).slow(16))
  .attack(0.5).release(1.5)
  .room(0.5).gain(0.22)
```

**Deep House Dictionary** — warm, close, jazzy:
```js
addVoicings('deepHouse', {
  'm7':   ['3 7 10 14', '7 10 15 19'],     // minor 9 (close + spread)
  'maj7': ['4 7 11 14', '7 11 16 19'],     // major 9
  '7':    ['4 7 10 14', '7 10 16 19'],     // dominant 9
  'm':    ['3 7 10', '7 10 15'],            // minor 7 (always add 7th)
})
```

**Ambient Dictionary** — wide, ethereal, stacked 5ths:
```js
addVoicings('ambient', {
  'm':    ['7 14 19', '7 12 19 24'],       // stacked 5ths
  'maj':  ['7 14 19', '7 11 19 24'],       // 5ths + maj7
  'sus2': ['2 7 14', '7 14 19'],           // very open sus2
  'sus4': ['5 7 12 17', '7 12 17 19'],     // wide sus4
})
```

---

## K.12 Chord Design Techniques

The notes are only half the story. How you **design** the chord sound is equally important.

### Layering for Richness

Layer multiple oscillator types for a thicker, more complex chord sound:

```js
setcpm(128/4)
// Layer 1: saw for brightness
$: chord("<Cm Fm>").voicing()
  .s("sawtooth").lpf(2000)
  .attack(0.4).release(1.2)
  .gain(0.15).orbit(2)

// Layer 2: triangle for body
$: chord("<Cm Fm>").voicing()
  .s("triangle").lpf(3000)
  .attack(0.5).release(1.5)
  .gain(0.12).orbit(2)

// Layer 3: sine sub for warmth
$: note("<c2 f2>").s("sine")
  .lpf(200).gain(0.2)
```

### Detuning for "Fatness"

Slight detuning between layers creates a chorus-like "supersaw" effect:

```js
setcpm(128/4)
// Detuned saw pad — fat, "supersaw" effect
$: chord("<Cm Fm>").voicing()
  .s("sawtooth").lpf(2000)
  .attack(0.4).release(1.2)
  .gain(0.12)

$: chord("<Cm Fm>").voicing()
  .s("sawtooth").lpf(2000)
  .attack(0.4).release(1.2)
  .gain(0.12)
  .transpose(0.08)   // slightly sharp

$: chord("<Cm Fm>").voicing()
  .s("sawtooth").lpf(2000)
  .attack(0.4).release(1.2)
  .gain(0.12)
  .transpose(-0.08)  // slightly flat
```

### Reverb and Delay as Harmonic Extensions

Effects can make simple chords sound more complex. A short stab with heavy reverb creates a "pad-like" tail:

```js
setcpm(128/4)
// Stab → reverb transforms a short chord into an atmospheric wash
$: chord("<Cm Fm Ab Bb>*2").voicing()
  .s("sawtooth")
  .lpf(3500).decay(0.08).sustain(0)
  .room(0.7).roomsize(0.8).roomlp(2500)
  .delay(0.3).delaytime(0.1875).delayfeedback(0.5)
  .gain(0.3)
```

### Sidechain Pumping on Chords

The signature "breathing" effect of dance music — make chords duck on every kick:

```js
setcpm(128/4)

// Kick (sidechain source)
$: s("bd*4").bank("RolandTR909").gain(0.85)

// Chord pad (sidechain target)
$: chord("<Cm7 Fm7>").voicing()
  .s("sawtooth")
  .lpf(2000).attack(0.3).release(1.5)
  .gain(0.25).orbit(2)

// Sidechain ducker
$: s("bd*4").bank("RolandTR909")
  .duckorbit(2).duckattack(0.2).duckdepth(1).gain(0)
```

---

## K.13 Practice Challenges

### Challenge 1: Build a Chord from Intervals ⭐

Using the interval table in K.2, manually build a **minor 7th chord** on F using `note()`. Don't use `chord()`.

<details>
<summary>Solution</summary>

```js
// F minor 7th = F + Ab (3) + C (7) + Eb (10)
note("[f3,ab3,c4,eb4]").s("sawtooth")
  .lpf(2000).attack(0.3).release(1)
```
</details>

### Challenge 2: Voice a Progression Smoothly ⭐⭐

Write a Cm → Fm → Ab → Bb progression using `note()` (not `chord()`) with smooth voice leading — each note should move no more than 2 semitones between chords.

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
note("<[c3,eb3,g3] [c3,f3,ab3] [c3,eb3,ab3] [bb2,d3,f3]>")
  .s("sawtooth")
  .lpf(2000).attack(0.3).release(1)
  .room(0.4).gain(0.22)
```
</details>

### Challenge 3: Create a Custom Voicing Dictionary ⭐⭐⭐

Create a voicing dictionary called `'myChords'` that defines minor, major, and minor 7th chords using open voicings (spread across > 1 octave). Use it with a 4-chord progression.

<details>
<summary>Solution</summary>

```js
setcpm(128/4)

addVoicings('myChords', {
  'm':   ['7 12 15', '3 12 19'],     // root-5th-oct-min3
  'maj': ['7 12 16', '4 12 19'],     // root-5th-oct-maj3
  'm7':  ['7 10 15', '3 10 19'],     // root-5th-7th-min3
})

chord("<Cm Fm Ab Bb>")
  .dict('myChords')
  .anchor(60)
  .voicing()
  .s("sawtooth")
  .lpf(sine.range(1000, 2800).slow(16)).lpq(2)
  .attack(0.4).sustain(0.5).release(1.5)
  .room(0.5).roomsize(0.7)
  .gain(0.22)
```
</details>

### Challenge 4: Genre Adaptation ⭐⭐⭐

Take the Final Project's chord line (`chord("<Cm!2 Fm!2 Ab!2 Bb!2>")`) and rewrite it as a **Dub Techno** version — single chord, heavy reverb, long delay, slow filter modulation.

<details>
<summary>Solution</summary>

```js
setcpm(124/4)

// Dub techno — one chord, effects do the work
$: chord("Cm").voicing()
  .s("sawtooth")
  .lpf(sine.range(300, 2000).slow(64)).lpq(4)
  .attack(0.5).sustain(0.7).release(2.0)
  .room(0.85).roomsize(0.95).roomlp(1800)
  .delay(0.65).delaytime(0.375).delayfeedback(0.75)
  .gain(0.18)
```
</details>

### Challenge 5: The Perfect Pad ⭐⭐⭐⭐

Design a pad sound using **two layers** (saw + triangle), **open voicings**, **sidechain ducking**, and a **slow filter sweep**. Use a minor 7th chord progression of your choice.

<details>
<summary>Solution</summary>

```js
setcpm(128/4)

// Kick (sidechain source)
$: s("bd*4").bank("RolandTR909").gain(0.85)

// Layer 1: Saw — brightness
$: chord("<Cm7 Fm7 Abmaj7 G7>").voicing()
  .s("sawtooth")
  .lpf(sine.range(800, 2500).slow(16)).lpq(2)
  .attack(0.5).sustain(0.5).release(1.5)
  .room(0.5).roomsize(0.7)
  .gain(0.15).orbit(2)

// Layer 2: Triangle — body
$: chord("<Cm7 Fm7 Abmaj7 G7>").voicing()
  .s("triangle")
  .lpf(3000)
  .attack(0.6).sustain(0.6).release(2.0)
  .room(0.5).roomsize(0.7)
  .gain(0.1).orbit(2)

// Sidechain ducker
$: s("bd*4").bank("RolandTR909")
  .duckorbit(2).duckattack(0.15).duckdepth(0.8).gain(0)
```
</details>

---

## K.14 Key Takeaways

1. **Chords work because of physics** — simpler frequency ratios = more consonance; the overtone series explains why 5ths and 3rds are the building blocks
2. **Intervals are the alphabet** — learn the 12 intervals and you can build any chord
3. **Triads are the foundation** — minor triads are the currency of techno
4. **7th chords add depth** — minor 7ths are the go-to for sophisticated pads
5. **Extended chords are color** — use add9, sus2, sus4 for variety without complexity
6. **Inversions enable smooth voice leading** — or just use `.voicing()` and let Strudel handle it
7. **Voicing matters as much as chord choice** — open = spacious, close = dense, drop-2 = balanced
8. **Minimal Techno needs minimal chords** — 1–2 chords + filter modulation is often enough
9. **The filter is a harmonic tool** — LPF sweeps change the perceived chord character without changing notes
10. **Every genre has a chord palette** — match your chords to your genre's conventions, then break the rules intentionally

---

**← Back to [Module 8 — Writing Chords & Melodies](./Module_08_Chords_Melodies.md)**
**← Back to [README — Course Overview](./README.md)**
