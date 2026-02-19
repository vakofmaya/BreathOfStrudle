# Appendix M — The Craft of Melody

> **How to write melodies that work — from the theory of melodic construction to practical techniques for every electronic genre, including generative and algorithmic approaches in Strudel.**

This appendix goes far beyond Module 8's practical introduction to melody. Here, we explore the principles that make melodies memorable, the techniques composers use to develop musical ideas, and the specific approaches that define melody in electronic music — including generative methods that let Strudel compose for you.

> [!NOTE]
> **Module 8 teaches you how to play melodies in Strudel.** This appendix teaches you how to *write* them — how to create melodic ideas that are compelling, how to develop them, and how to match your melodic approach to your genre.

---

## M.1 What Makes a Melody?

A melody is a sequence of pitches and rhythms that forms a musical *thought* — something your brain perceives as a coherent, meaningful unit. But what separates a random sequence of notes from a melody you can't get out of your head?

### The Five Elements of a Strong Melody

1. **Contour** — the shape of the melody (rises, falls, arcs)
2. **Rhythm** — when notes happen (not just which notes)
3. **Repetition** — patterns your brain can latch onto
4. **Variation** — enough change to stay interesting
5. **Resolution** — a sense of going somewhere and arriving

Every great melody, from Beethoven to Burial, uses these five elements. The difference between genres is how much of each element is emphasized.

### Melody in Electronic Music: A Different Beast

Traditional melody writing assumes a singer or lead instrument playing a continuous line. Electronic music — especially minimal techno — uses melody differently:

- **Melodies are often implied** — a few notes + delay creates the illusion of a full melody
- **Repetition is the goal** — hypnotic loops, not singable tunes
- **Sound design IS melody** — a filter sweep on a single note can be more "melodic" than 16 different pitches
- **Generative approaches** — algorithms create evolving melodies that no human would write

> **The mantra of electronic melody:** *It's not what you play — it's what you leave out.*

---

## M.2 Melodic Contour — The Shape of a Melody

**Contour** is the overall shape of a melody — the pattern of ups and downs. Even without knowing the exact notes, you can recognize a melody by its contour alone.

### The Five Basic Contours

```
1. ASCENDING        ↗          Rising energy, building tension
   n("0 1 2 3 4 5 6 7")

2. DESCENDING       ↘          Releasing tension, resolving, calming
   n("7 6 5 4 3 2 1 0")

3. ARCH             ∧          Builds up, then resolves — the most natural shape
   n("0 2 4 6 7 5 3 1")

4. INVERTED ARCH    ∨          Dips down, then recovers — creates longing
   n("7 5 3 1 0 2 4 6")

5. STATIC           —          Flat, repetitive, hypnotic — common in techno
   n("0 0 0 0 0 0 0 0")
```

### Hearing Each Contour

```js
setcpm(128/4)

// Ascending — energy builds
$: n("0 1 2 3 4 5 6 7").scale("C3:minor")
  .s("triangle").lpf(3000).decay(0.15).sustain(0)
```

```js
// Arch — natural rise and fall (⭐ the "default good melody shape")
$: n("0 2 4 6 7 5 3 1").scale("C3:minor")
  .s("triangle").lpf(3000).decay(0.15).sustain(0)
```

```js
// Static with variation — techno approach (same note, rhythm changes)
$: n("0 0 ~ 0 ~ 0 0 ~").scale("C3:minor")
  .s("triangle").lpf(3000).decay(0.15).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
```

### Contour Rules for Electronic Music

| Genre | Preferred Contour | Why |
|-------|------------------|-----|
| **Minimal Techno** | Static, small arch | Hypnotic repetition over melodic shape |
| **Melodic Techno** | Arch, descending | Emotional arcs, resolution |
| **Trance** | Large ascending, arch | Building energy toward drops |
| **Ambient** | Inverted arch, wavy | Creating a sense of floating, no destination |
| **DnB (Liquid)** | Arch, ascending | Uplifting, emotional |

---

## M.3 Motifs — The Building Blocks

A **motif** is a short musical idea — typically 2 to 4 notes — that acts as the DNA of your melody. Every great melody can be reduced to a motif that gets repeated, varied, and developed.

### What Makes a Good Motif

- **Short** — 2–4 notes maximum. If you can't hum it back after hearing it once, it's too complex.
- **Rhythmically distinctive** — the *rhythm* of a motif is often more recognizable than the pitches
- **Contains one "hook" interval** — a leap or an unexpected note that makes it memorable

### Building a Motif

```js
setcpm(128/4)

// A simple 3-note motif
$: n("<0 3 5 ~>").scale("C4:minor:pentatonic")
  .s("triangle").decay(0.12).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .gain(0.25)
```

That's it: root, skip to the 3rd, step to the 5th, rest. Three notes and a space. Delay fills the gap. This is a complete techno melody.

### Developing a Motif

Once you have a motif, you develop it through **transformation techniques**:

**1. Repetition** — play it again, exactly:
```js
n("<[0 3 5 ~] [0 3 5 ~]>")
```

**2. Transposition** — same shape, different starting note:
```js
n("<[0 3 5 ~] [2 5 7 ~]>")
```

**3. Inversion** — flip the direction (goes up → goes down):
```js
// Original: up-up-rest → Inverted: down-down-rest
n("<[0 3 5 ~] [5 3 0 ~]>")
```

**4. Augmentation** — stretch it out (slower rhythm):
```js
n("<0 ~ 3 ~ 5 ~ ~ ~>")
```

**5. Fragmentation** — use only part of the motif:
```js
n("<0 3 ~ ~>")
```

**6. Embellishment** — add passing notes between the motif notes:
```js
n("<0 2 3 4 5 ~ ~ ~>")
```

### Complete Motif Development Example

```js
setcpm(128/4)

// Cycle 1-2: Original motif
// Cycle 3-4: Transposed up
// Cycle 5-6: Inverted
// Cycle 7-8: Fragment
$: n("<[0 3 5 ~]!2 [2 5 7 ~]!2 [5 3 0 ~]!2 [0 3 ~ ~]!2>")
  .scale("C4:minor:pentatonic")
  .s("triangle").lpf(3500).decay(0.12).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .gain(0.25)
```

> **This is how one 3-note idea becomes a 32-cycle melodic arc.** Professional producers do this instinctively — now you can do it deliberately.

---

## M.4 Stepwise Motion vs. Leaps

How your melody moves from one note to the next defines its character.

### Stepwise Motion (Conjunct)

Moving by **scale steps** (1–2 scale degrees):

```
0 → 1 → 2 → 3 → 4    (smooth, flowing)
```

**Character:** Smooth, singable, calm, connected
**Best for:** Pads, background melodies, transitions

```js
setcpm(128/4)
$: n("0 1 2 3 4 3 2 1").scale("C4:minor")
  .s("triangle").lpf(3000).decay(0.15).sustain(0)
```

### Leaps (Disjunct)

Jumping by **3+ scale degrees**:

```
0 → 4 → 2 → 7 → 3    (dramatic, angular)
```

**Character:** Dramatic, attention-grabbing, energetic, surprising
**Best for:** Hooks, lead lines, moments of intensity

```js
setcpm(128/4)
$: n("0 4 2 7 3 6 1 5").scale("C4:minor")
  .s("triangle").lpf(3000).decay(0.15).sustain(0)
```

### The Balance Rule

> **After a leap, move stepwise in the opposite direction.**

This is the oldest rule in melody writing, and it works because it creates a natural sense of tension (the leap) and resolution (the stepwise return).

```js
setcpm(128/4)
// Leap up to 4, then step down: 4→3→2
$: n("0 ~ 4 3 2 ~ 5 4").scale("C4:minor:pentatonic")
  .s("triangle").lpf(3500).decay(0.12).sustain(0)
  .delay(0.3).delaytime(0.1875).delayfeedback(0.4)
  .gain(0.25)
```

### The "Interval Emotion" Map

| Interval (Scale Steps) | Feeling | Use In |
|------------------------|---------|--------|
| 1 step (2nd) | Smooth, subtle | Background, flow |
| 2 steps (3rd) | Gentle, melodic | Main melodies |
| 3 steps (4th) | Open, strong | Hooks, beginnings |
| 4 steps (5th) | Powerful, heroic | Climactic moments |
| 5+ steps (6th+) | Dramatic, surprising | Rare moments, emphasis |
| Octave leap | Grand, emphatic | Transitions, drops |

### For Minimal Techno

Minimal techno almost exclusively uses **stepwise motion and small leaps (2nds and 3rds)**. Large leaps are rare — they draw too much attention.

```js
setcpm(128/4)
// ⭐ Good minimal melody: mostly stepwise, one small leap
$: n("<0 ~ 2 ~ 3 ~ 2 0>").scale("C4:minor:pentatonic")
  .s("triangle").decay(0.12).sustain(0)
  .delay(0.5).delaytime(0.1875).delayfeedback(0.5)
  .gain(0.22)
```

---

## M.5 Rhythm in Melody — When Notes Happen

**The rhythm of a melody is more important than the pitches.** You can often change every note in a melody and keep the rhythm, and it'll still feel like the same melody. Change the rhythm and keep the notes, and it becomes unrecognizable.

### Rhythmic Techniques for Electronic Melody

**1. Syncopation** — placing notes on unexpected beats:
```js
setcpm(128/4)
// On-beat: predictable
n("0 2 4 5").scale("C4:minor").s("triangle").decay(0.12).sustain(0)

// Syncopated: groove
n("~ 0 2 ~ ~ 4 5 ~").scale("C4:minor").s("triangle").decay(0.12).sustain(0)
```

**2. Rests as melody** — silence is a note:
```js
setcpm(128/4)
// The rests ARE the melody
$: n("<0 ~ ~ ~ 3 ~ ~ ~>").scale("C4:minor:pentatonic")
  .s("triangle").decay(0.15).sustain(0)
  .delay(0.5).delaytime(0.1875).delayfeedback(0.6)
  .gain(0.25)
```

**3. Rhythmic density variation** — change how many notes per phrase:
```js
setcpm(128/4)
// Sparse → dense → sparse (creates arc)
$: n("<[0 ~ ~ ~] [0 2 ~ ~] [0 2 3 5] [0 ~ ~ ~]>")
  .scale("C4:minor:pentatonic")
  .s("triangle").decay(0.12).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .gain(0.22)
```

**4. Swing** — humanize timing:
```js
setcpm(128/4)
$: n("0 2 4 5 7 5 4 2").scale("C4:minor:pentatonic")
  .s("triangle").decay(0.12).sustain(0)
  .swingBy(1/6, 4)  // slight shuffle
```

### The Power of Delay as Rhythm

In electronic music, **delay creates rhythm from sparse input**. Two notes with delay become four. Four become eight. This is the single most important production technique for electronic melody:

```js
setcpm(128/4)

// Without delay: sparse, bare
$: n("0 ~ ~ ~ 3 ~ ~ ~").scale("C4:minor:pentatonic")
  .s("triangle").decay(0.12).sustain(0).gain(0.25)
```

```js
// With delay: full, rhythmic, alive
$: n("0 ~ ~ ~ 3 ~ ~ ~").scale("C4:minor:pentatonic")
  .s("triangle").decay(0.12).sustain(0)
  .delay(0.5).delaytime(0.1875).delayfeedback(0.55)
  .gain(0.25)
```

> **The delay formula for 128 BPM techno:**
> - Dotted 8th: `.delaytime(0.1875)` — the techno standard
> - Quarter note: `.delaytime(0.25)` — wider, more spacious
> - 8th note: `.delaytime(0.125)` — tight, rhythmic

---

## M.6 Tension and Resolution — The Emotional Arc

Every compelling melody creates **tension** (instability, anticipation) and **resolution** (stability, satisfaction). This push-and-pull is what gives music emotional power.

### Creating Tension

| Technique | How to Do It | Strudel |
|-----------|-------------|---------|
| **Move away from root** | Go to higher scale degrees (5, 6, 7) | `n("5 6 7 ~")` |
| **Use dissonant intervals** | 2nds, 7ths, tritone | `n("0 1 ~ ~")` or `n("0 6 ~ ~")` |
| **Leave phrases unresolved** | End on a non-root note | `n("0 2 4 5")` — ends on 5th degree |
| **Increase rhythmic density** | More notes per cycle | `n("0 1 2 3 4 5 6 7")` |
| **Ascend** | Rising contour | `n("0 2 4 6")` |

### Creating Resolution

| Technique | How to Do It | Strudel |
|-----------|-------------|---------|
| **Return to root** | Land on scale degree 0 | `n("5 4 2 0")` |
| **Step down to root** | Approach from above (2→1→0) | `n("7 6 5 4 3 2 1 0")` |
| **Use longer notes** | Sustain the resolution | `n("0 ~ ~ ~")` |
| **Decrease density** | Fewer notes after tension | `n("[0 2 4 6] [0 ~ ~ ~]")` |
| **Descend** | Falling contour | `n("6 4 2 0")` |

### A Complete Tension-Resolution Arc

```js
setcpm(128/4)

// 8 cycles: build tension → resolve
$: n("<
  [0 ~ 2 ~]
  [0 ~ 3 ~]
  [0 2 4 5]
  [2 4 5 7]
  [5 7 5 7]
  [7 6 5 4]
  [3 2 1 0]
  [0 ~ ~ ~]
>").scale("C4:minor:pentatonic")
  .s("triangle").lpf(3500).decay(0.12).sustain(0)
  .delay(0.35).delaytime(0.1875).delayfeedback(0.4)
  .gain(0.25)
```

> **Cycles 1–2:** Sparse, grounded (root). **3–4:** Building (more notes, ascending). **5:** Peak tension (high, repeated). **6–7:** Resolution (descending). **8:** Rest (home).

### Tension in Minimal Techno

In minimal techno, tension and resolution are more subtle:
- **Tension:** Filter opening, added percussion, melody notes moving away from root
- **Resolution:** Filter closing, stripping elements, melody returning to root or silence

```js
setcpm(128/4)
// Filter IS the tension/resolution — melody stays static
$: n("0 ~ 0 ~ 0 ~ 0 ~").scale("C4:minor")
  .s("sawtooth")
  .lpf(sine.range(400, 4000).slow(16)).lpq(4)
  .decay(0.12).sustain(0)
  .gain(0.25)
```

---

## M.7 Melodic Structures and Phrasing

### Call and Response

A **call** (musical question) followed by a **response** (musical answer). One of the most instinctive structures in music — it mimics conversation.

```js
setcpm(128/4)
// Call: ascending. Response: descending with variation
$: n("<[0 2 4 ~] [4 3 2 0] [0 2 4 ~] [5 3 1 0]>")
  .scale("C4:minor:pentatonic")
  .s("triangle").lpf(3500).decay(0.12).sustain(0)
  .delay(0.3).delaytime(0.1875).delayfeedback(0.4)
  .gain(0.22)
```

### AABA Form

The classic songwriting structure: state an idea (A), repeat it (A), contrast it (B), then return (A):

```js
setcpm(128/4)
$: n("<
  [0 ~ 3 ~ 5 ~ 3 ~]
  [0 ~ 3 ~ 5 ~ 3 ~]
  [4 ~ 6 ~ 7 ~ 6 ~]
  [0 ~ 3 ~ 5 ~ 3 ~]
>").scale("C4:minor:pentatonic")
  .s("triangle").lpf(3500).decay(0.12).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .gain(0.22)
```

### The "Question-Answer" Ending Technique

End your **call** (question) on a non-root tone and your **response** (answer) on the root:

```js
setcpm(128/4)
// Question: ends on 4 (unresolved)
// Answer: ends on 0 (resolved)
$: n("<[0 2 3 4] [4 3 2 0]>")
  .scale("C4:minor:pentatonic")
  .s("triangle").decay(0.12).sustain(0)
  .delay(0.3).delaytime(0.1875)
  .gain(0.25)
```

---

## M.8 Generative Melody in Strudel

One of Strudel's superpowers: **algorithmic melody generation**. Use mathematical functions to create melodies that evolve endlessly — perfect for long techno sets and ambient pieces.

### Method 1: Random with Scale Safety

```js
setcpm(128/4)
// Random scale degrees, quantized to 8 notes per cycle
$: n(irand(5).segment(8)).scale("C4:minor:pentatonic")
  .s("triangle").lpf(3000).decay(0.1).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .degradeBy(0.3)   // randomly remove 30% of notes
  .gain(0.25)
```

> Why `irand(5)` with pentatonic? The minor pentatonic has 5 notes (degrees 0–4). `irand(5)` picks from 0–4, so every note is valid. With `.degradeBy(0.3)`, 30% of notes are replaced with silence — creating natural-sounding rests.

### Method 2: Sine-Driven (Smooth, Repeating)

```js
setcpm(128/4)
// Sine wave controls pitch — creates smooth, cycling melody
$: n(sine.segment(16).range(0, 7)).scale("C4:minor")
  .s("triangle").lpf(2500).decay(0.1).sustain(0)
```

The sine wave creates an **arch contour** that repeats every cycle — a smooth, predictable melody that feels organic.

### Method 3: Saw-Driven (Ascending Ramp)

```js
setcpm(128/4)
// Saw wave = ascending ramp → always ascending melody
$: n(saw.segment(8).range(0, 7)).scale("C4:minor")
  .s("triangle").lpf(3000).decay(0.12).sustain(0)
```

### Method 4: Perlin Noise (Organic, Evolving)

```js
setcpm(128/4)
// Perlin noise = smooth randomness — never exactly repeats
$: n(perlin.segment(8).range(0, 12)).scale("C3:minor:pentatonic")
  .s("sawtooth").lpf(2000).decay(0.2).sustain(0)
  .delay(0.3).delaytime(0.1875).delayfeedback(0.4)
  .gain(0.25)
```

> **Perlin noise** is "smooth randomness" — each value is close to the previous one, so the melody moves by steps, not jumps. It never exactly repeats but always sounds coherent.

### Method 5: Combining Signals

```js
setcpm(128/4)
// Sine provides shape, perlin adds variation
$: n(sine.add(perlin.mul(0.3)).segment(12).range(0, 8))
  .scale("C4:minor:pentatonic")
  .s("triangle").lpf(3500).decay(0.1).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .degradeBy(0.2)
  .gain(0.22)
```

### Method 6: Probabilistic (Weighted Random)

Use `.degradeBy()`, `.sometimes()`, and friends for melodies that have a consistent *skeleton* but vary in the details:

```js
setcpm(128/4)
// Fixed skeleton — probabilistic ornamentation
$: n("0 ~ 3 ~ 5 ~ 3 ~")
  .scale("C4:minor:pentatonic")
  .sometimes(x => x.add(note(12)))  // sometimes octave up
  .rarely(x => x.add(note(7)))       // rarely a 5th up
  .s("triangle").decay(0.12).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .gain(0.22)
```

### Method 7: `.iter()` — Rotating Melodies

`.iter(n)` shifts the start position of a pattern every cycle, creating a melody that "rotates" through different starting points:

```js
setcpm(128/4)
$: n("0 2 4 5 7 5 4 2").scale("C4:minor:pentatonic")
  .s("triangle").decay(0.12).sustain(0)
  .iter(4)  // shifts start by 2 positions each cycle
  .delay(0.3).delaytime(0.1875).delayfeedback(0.4)
  .gain(0.22)
```

### Method 8: `.superimpose()` — Self-Harmonizing Melody

```js
setcpm(128/4)
// Melody harmonizes with itself, transposed by a 5th
$: n("0 ~ 2 ~ 4 ~ 5 ~").scale("C4:minor:pentatonic")
  .s("triangle").decay(0.12).sustain(0)
  .superimpose(x => x.scaleTranspose(4).gain(0.15))
  .delay(0.3).delaytime(0.1875).delayfeedback(0.4)
  .gain(0.22)
```

### Generative Method Comparison

| Method | Character | Best For |
|--------|-----------|----------|
| `irand` | Random, unpredictable | Experimental, ambient |
| `sine.segment` | Smooth, cycling, predictable | Repeating patterns, pads |
| `saw.segment` | Always ascending | Builds, risers |
| `perlin.segment` | Organic, evolving | Long sets, ambient, generative |
| `irand + degradeBy` | Random with gaps | Minimal techno, sparse |
| `.iter()` | Rotating, phasing | Hypnotic techno |
| `.superimpose()` | Self-harmonizing | Rich melodic layers |

---

## M.9 Genre-Specific Melody Approaches

### ⚫ Minimal Techno

**Philosophy:** The melody shouldn't be the focus. It's texture, not a tune.

**Rules:**
1. Maximum 4 notes per phrase
2. Rests outnumber notes
3. Delay does 50% of the melodic work
4. Stay on minor pentatonic
5. Return to root frequently

```js
setcpm(128/4)
// ⭐ The quintessential minimal techno melody
$: n("<0 ~ 3 ~ , ~ 2 ~ 5>")
  .scale("C4:minor:pentatonic")
  .s("triangle").decay(0.12).sustain(0)
  .delay(0.5).delaytime(0.1875).delayfeedback(0.55)
  .degradeBy(0.25)
  .pan(sine.range(0.3, 0.7).slow(4))
  .gain(0.22)
```

---

### 🎹 Melodic Techno

**Philosophy:** Emotion through melody, but always serving the groove.

**Rules:**
1. Longer phrases (8–16 notes)
2. Arch contour — build up, come down
3. Use arpeggios as melodic elements
4. Harmonic minor for drama, natural minor for melancholy
5. Layer lead + delay + reverb

```js
setcpm(126/4)
// Melodic techno lead — emotional, arching
$: n("<[0 2 4 6 7 6 4 2] [0 2 4 5 7 5 2 0]>")
  .scale("A3:harmonicMinor")
  .s("sawtooth").lpf(4000).decay(0.12).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .room(0.3).gain(0.22)
```

---

### 🌿 Ambient

**Philosophy:** Melody as landscape. Not a journey — a place.

**Rules:**
1. Slow, long notes
2. No rhythmic urgency
3. Lydian or whole tone for floating quality
4. Layer multiple slow-moving lines
5. Perlin noise for organic evolution

```js
setcps(0.08)
// Ambient melody — slow, floating, organic
$: n(perlin.segment(4).range(0, 7)).scale("C4:lydian")
  .s("triangle")
  .lpf(perlin.range(800, 3000).slow(32))
  .attack(1.0).sustain(0.7).release(3.0)
  .room(0.85).roomsize(0.95).roomlp(3000)
  .delay(0.5).delaytime(0.5).delayfeedback(0.65)
  .gain(0.12)
```

---

### 🔮 Trance

**Philosophy:** The melody IS the track. Euphoric, singable, unforgettable.

**Rules:**
1. Start simple, build complexity
2. Use the "anthem" contour — arch shape over 8+ bars
3. Major or harmonic minor for emotional impact
4. Layer with arpeggios
5. Supersaw sound + delay + reverb

```js
setcpm(140/4)
// Trance lead — anthemic, building
$: n("<[0 2 4 5 7 5 4 2]*2 [0 2 4 7 9 7 4 2]*2>")
  .scale("A3:minor")
  .s("sawtooth").lpf(5000).decay(0.1).sustain(0)
  .delay(0.3).delaytime(0.1875).delayfeedback(0.4)
  .gain(0.25)
```

---

### 🥁 Drum & Bass (Liquid)

**Philosophy:** Uplifting melody over high-speed breaks. The melody is the emotional anchor.

**Rules:**
1. Wide register — use the full pentatonic range
2. Longer note values (against fast drums)
3. extended chords underneath for richness
4. Reverb-heavy

```js
setcpm(174/4)
// Liquid DnB melody — smooth, emotional
$: n("<0 ~ 2 4 ~ 7 4 2>").scale("A3:minor:pentatonic")
  .s("triangle").lpf(4000)
  .attack(0.05).decay(0.2).sustain(0.3).release(0.5)
  .delay(0.4).delaytime(0.109).delayfeedback(0.5)
  .room(0.3).gain(0.22)
```

---

### 🧪 Acid

**Philosophy:** The melody is the bass. The filter is the expression.

**Rules:**
1. Monophonic (one note at a time)
2. Rapid, syncopated patterns
3. Accent + slide = the character
4. Resonant filter does the melodic work
5. Stay in minor, use blues notes

```js
setcpm(126/4)
// Acid line — the 303 melody approach
$: note("<c2 c2 eb2 c2 ~ f2 eb2 c2>")
  .s("sawtooth")
  .lpf(sine.range(300, 5000).fast(2)).lpq(18)
  .ftype("ladder")
  .decay(0.15).sustain(0)
  .distort(0.3)
  .gain(0.45)
```

---

## M.10 Melody + Effects = Complete Sound

### The Delay-as-Melody Technique

Two notes + delay = a complete melodic pattern:

```js
setcpm(128/4)
// Just TWO notes — delay creates the rest
$: n("0 ~ ~ ~ 3 ~ ~ ~").scale("C4:minor:pentatonic")
  .s("triangle").decay(0.12).sustain(0)
  .delay(0.6).delaytime(0.1875).delayfeedback(0.6)
  .gain(0.25)
```

### The Reverb-as-Sustain Technique

Short notes + reverb = pad-like sustain:

```js
setcpm(128/4)
$: n("0 ~ 3 ~ 5 ~ 3 ~").scale("C4:minor:pentatonic")
  .s("triangle").decay(0.05).sustain(0)
  .room(0.7).roomsize(0.85).roomlp(3000)
  .gain(0.22)
```

### The Pan-as-Movement Technique

Panning adds spatial interest without adding notes:

```js
setcpm(128/4)
$: n("0 ~ 3 ~ 5 ~ 3 ~").scale("C4:minor:pentatonic")
  .s("triangle").decay(0.12).sustain(0)
  .pan(sine.range(0.2, 0.8).slow(4))
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .gain(0.22)
```

### The Complete "Minimal Lead" Recipe

```js
setcpm(128/4)
$: n("<0 ~ 3 ~ , ~ 2 ~ 5>")
  .scale("C4:minor:pentatonic")
  .s("triangle")
  .lpf(3500)
  .decay(0.12).sustain(0).release(0.08)
  .delay(0.45).delaytime(0.1875).delayfeedback(0.55)
  .pan(sine.range(0.3, 0.7).slow(4))
  .degradeBy(0.25)
  .gain(0.25)
```

This is the lead sound from the course's "Pulse" track. Every parameter is intentional:
- **Triangle wave** — soft, not aggressive
- **LPF 3500** — warm, not piercing
- **Short decay, no sustain** — plucky, percussive
- **Dotted-8th delay** — creates rhythmic echoes
- **Sine panning** — moves gently left-right
- **degradeBy 0.25** — randomly drops 25% of notes for human feel

---

## M.11 Melody and Harmony — Making Them Work Together

### Chord Tones vs. Non-Chord Tones

When a melody note is part of the underlying chord, it **sounds stable**. When it's not, it **creates tension**.

| Chord | Chord Tones (stable) | Non-Chord Tones (tension) |
|-------|---------------------|--------------------------|
| Cm (C, Eb, G) | Scale degrees 0, 2, 4 | 1, 3, 5, 6 |
| Fm (F, Ab, C) | Scale degrees 3, 5, 0 | 1, 2, 4, 6 |

**Rule:** Land on **chord tones** on strong beats. Use **non-chord tones** as passing notes on weak beats.

```js
setcpm(128/4)
// Chord tones on beats 1 and 3, passing tones between
$: chord("<Cm Fm>").voicing()
  .s("sawtooth").lpf(2000)
  .attack(0.5).release(1.5).room(0.5)
  .gain(0.2)

$: n("<[0 1 2 ~ 4 3 2 ~] [3 4 5 ~ 0 6 5 ~]>")
  .scale("C4:minor")
  .s("triangle").decay(0.12).sustain(0)
  .delay(0.3).delaytime(0.1875)
  .gain(0.22)
```

### Following the Bass

In techno, the melody should **orbit around the bass note**. If the bass plays C, the melody should reference C (degree 0). If the bass moves to F, the melody should reference F (degree 3).

```js
setcpm(128/4)
// Bass follows chord roots
$: note("<c2!4 f2!4>").s("sine")
  .lpf(200).decay(0.3).sustain(0.5).gain(0.5)

// Melody shifts with the bass
$: n("<[0 ~ 2 ~ 4 ~ 2 ~] [3 ~ 5 ~ 0 ~ 5 ~]>")
  .scale("C4:minor:pentatonic")
  .s("triangle").decay(0.12).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .gain(0.22)
```

---

## M.12 Practice Challenges

### Challenge 1: Build a Motif ⭐

Write a 3-note motif using minor pentatonic. Add delay. Listen to whether it "sticks" in your head.

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
$: n("<0 4 2 ~>").scale("C4:minor:pentatonic")
  .s("triangle").decay(0.12).sustain(0)
  .delay(0.5).delaytime(0.1875).delayfeedback(0.5)
  .gain(0.25)
```
</details>

### Challenge 2: Develop It ⭐⭐

Take your motif from Challenge 1 and create a 4-cycle development: original → transposed → inverted → fragment.

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
$: n("<[0 4 2 ~] [2 6 4 ~] [4 0 2 ~] [0 4 ~ ~]>")
  .scale("C4:minor:pentatonic")
  .s("triangle").decay(0.12).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .gain(0.25)
```
</details>

### Challenge 3: Call and Response ⭐⭐

Write a 2-cycle melody where the first cycle is a "question" (doesn't end on root) and the second is an "answer" (ends on root).

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
$: n("<[0 2 4 ~ 5 ~ ~ ~] [5 4 2 ~ 0 ~ ~ ~]>")
  .scale("C4:minor:pentatonic")
  .s("triangle").lpf(3500).decay(0.12).sustain(0)
  .delay(0.35).delaytime(0.1875).delayfeedback(0.4)
  .gain(0.25)
```
</details>

### Challenge 4: Generative Melody ⭐⭐⭐

Create a melody using Perlin noise that slowly evolves over time. Add delay, panning, and probabilistic note removal.

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
$: n(perlin.segment(8).range(0, 7))
  .scale("C4:minor:pentatonic")
  .s("triangle").lpf(3500).decay(0.1).sustain(0)
  .delay(0.45).delaytime(0.1875).delayfeedback(0.55)
  .pan(sine.range(0.2, 0.8).slow(4))
  .degradeBy(0.3)
  .gain(0.22)
```
</details>

### Challenge 5: Melody over Chords ⭐⭐⭐

Write a pad + bass + lead stack where the melody uses chord tones on strong beats. Use a 4-chord progression.

<details>
<summary>Solution</summary>

```js
setcpm(128/4)

// Bass
$: note("<c2 f2 ab2 bb2>").s("sine")
  .lpf(200).decay(0.3).sustain(0.5).gain(0.5)

// Pad
$: chord("<Cm Fm Ab Bb>").voicing()
  .s("sawtooth").lpf(sine.range(800, 2200).slow(16))
  .attack(0.5).sustain(0.5).release(1.5)
  .room(0.5).gain(0.2).orbit(2)

// Lead — chord tones on beats, passing tones between
$: n("<[0 1 2 ~ 4 ~ 2 ~] [3 4 5 ~ 0 ~ 5 ~] [5 4 2 ~ 0 ~ 2 ~] [6 5 3 ~ 1 ~ 3 ~]>")
  .scale("C4:minor")
  .s("triangle").decay(0.12).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .gain(0.22)
```
</details>

---

## M.13 Key Takeaways

1. **A melody is contour + rhythm + repetition + variation + resolution** — not just notes
2. **Contour** (the shape) is more important than the exact pitches — ascend for energy, descend for resolution, arch for a complete thought
3. **Motifs** (2–4 note ideas) are the DNA — develop them through transposition, inversion, fragmentation, and embellishment
4. **Stepwise motion** = smooth and calm; **leaps** = dramatic and attention-grabbing; balance them
5. **Rhythm matters more than pitch** — a melody with great rhythm and boring notes beats boring rhythm and great notes
6. **Rests ARE notes** — in electronic music, what you don't play defines the melody as much as what you do
7. **Delay is the producer's melody tool** — two sparse notes + delay = a full, rhythmic melodic line
8. **Generative approaches** (`irand`, `perlin`, `sine.segment`, `.iter()`) let Strudel compose evolving melodies for you
9. **In minimal techno:** 3 notes, pentatonic, delay, rests — that's a complete melody
10. **Melody and harmony are partners** — land on chord tones on strong beats, use non-chord tones as passing notes

---

**← Back to [Module 8 — Writing Chords & Melodies](./Module_08_Chords_Melodies.md)**
**← Back to [Appendix K — The Theory of Perfect Chords](./Appendix_K_Chord_Theory.md)**
**← Back to [Appendix L — The Art of Scales](./Appendix_L_Scale_Theory.md)**
**← Back to [README — Course Overview](./README.md)**
