# Appendix L — The Art of Scales

> **Everything you need to know about musical scales — what they are, why they work, how to choose the right one for your track, and how to use every scale Strudel offers.**

This appendix goes deeper than Module 8's practical introduction to scales. Here, we explore the theory of how scales are constructed, the emotional character of every mode, exotic scales from around the world, and a complete genre-by-genre guide to choosing the perfect scale.

> [!NOTE]
> **This appendix is reference material.** Module 8 gives you everything you need to start writing melodies and chord progressions. Come here when you want deeper understanding, when you're stuck on a mood, or when you want to explore scales beyond the usual minor and pentatonic.

---

## L.1 What Is a Scale?

A **scale** is an ordered collection of notes that defines the "sound world" of a piece of music. It determines:

- Which notes "belong" — play any note in the scale and it sounds right
- Which notes **don't** belong — these create tension or dissonance
- The **mood** — major scales sound bright; minor scales sound dark
- The **chords** available — chords are built from scale notes

### Scales vs. Keys

A **key** tells you two things: the **root note** and the **scale type**.

```
Key of C minor = Root: C + Scale: Minor
Key of A major = Root: A + Scale: Major
```

In Strudel, you specify both with `.scale()`:

```js
n("0 1 2 3 4 5 6 7").scale("C3:minor")      // Key of C minor
n("0 1 2 3 4 5 6 7").scale("A3:major")       // Key of A major
```

### How Scales Are Built — Whole and Half Steps

Every scale is a pattern of **whole steps** (W = 2 semitones) and **half steps** (H = 1 semitone):

```
Major Scale Pattern:    W  W  H  W  W  W  H
                        C→D→E→F→G→A→B→C

Minor Scale Pattern:    W  H  W  W  H  W  W
                        C→D→Eb→F→G→Ab→Bb→C
```

That's it. Every scale is just a **recipe of intervals**. Change the recipe, change the mood.

---

## L.2 The Seven Modes — One Scale, Seven Moods

The most powerful concept in scale theory: **the seven modes of the major scale**. These are seven different scales that all use the same notes but start on different degrees, giving each a completely different emotional character.

Think of it this way: the notes of C major are C D E F G A B. If you play these notes starting from C, you get the bright, happy major scale. But if you play the same notes starting from A, you get the dark, sad natural minor. Same notes, different mood — because the **pattern of whole and half steps changes** relative to the root.

### The Complete Mode Chart

```
Mode         Start On   Pattern              Character
─────────────────────────────────────────────────────────────────
Ionian       1st        W W H W W W H        Bright, happy, resolved
Dorian       2nd        W H W W W H W        Minor but hopeful, jazzy
Phrygian     3rd        H W W W H W W        Dark, exotic, Spanish
Lydian       4th        W W W H W W H        Dreamy, ethereal, floating
Mixolydian   5th        W W H W W H W        Bluesy, laid-back, rock
Aeolian      6th        W H W W H W W        Sad, dark, melancholic
Locrian      7th        H W W H W W W        Unstable, dissonant, uneasy
─────────────────────────────────────────────────────────────────
```

### Each Mode in Detail

---

#### 🟡 Ionian (Major) — *The Sound of Resolution*

**Formula:** 1 2 3 4 5 6 7
**Strudel name:** `major` or `ionian`
**Mood:** Happy, bright, triumphant, resolved
**Genre use:** Trance (uplifting), progressive house, pop-EDM
**Techno use:** Rare — too "happy" for dark techno. Used in melodic techno for uplifting sections.

```js
setcpm(128/4)
n("0 1 2 3 4 5 6 7").scale("C3:major").s("triangle")
  .lpf(3000).decay(0.15).sustain(0)
```

> **Why it sounds happy:** The major 3rd (4 semitones above root) and major 7th (11 semitones) create a bright, resolved quality. There are no "darkening" intervals.

---

#### 🔵 Dorian — *The Cool Minor*

**Formula:** 1 2 ♭3 4 5 6 ♭7
**Strudel name:** `dorian`
**Mood:** Minor but sophisticated, jazzy, cool, slightly hopeful
**What makes it special:** It's a minor scale with a **raised 6th** — this one note lifts it from "sad" to "cool"
**Genre use:** Deep house, tech house, jazz-influenced techno
**Techno use:** ⭐⭐ Common in groovier techno. The raised 6th adds warmth that prevents the mood from becoming too bleak.

```js
setcpm(128/4)
n("0 1 2 3 4 5 6 7").scale("C3:dorian").s("triangle")
  .lpf(3000).decay(0.15).sustain(0)
```

```js
// Dorian groove — jazzy, grooving techno
setcpm(128/4)
$: n("<0 ~ 3 ~ 5 6 5 3>").scale("C3:dorian")
  .s("sawtooth").lpf(2500).decay(0.12).sustain(0)
  .delay(0.35).delaytime(0.1875).delayfeedback(0.4)
  .gain(0.25)
```

> **Compared to natural minor:** The only difference is one note — the 6th degree. In C minor, Ab. In C Dorian, A♮. That single semitone changes everything.

---

#### 🟣 Phrygian — *The Dark Exotic*

**Formula:** 1 ♭2 ♭3 4 5 ♭6 ♭7
**Strudel name:** `phrygian`
**Mood:** Dark, exotic, Spanish, Middle Eastern, tense, mysterious
**What makes it special:** The **♭2** (one semitone above root) creates an unmistakable "exotic" tension. It's the most characteristic interval of this mode.
**Genre use:** ⭐⭐⭐ Dark techno, dub techno, industrial, psytrance
**Techno use:** Extremely effective for dark, brooding techno. The ♭2 creates a sense of tension without needing complex chord progressions.

```js
setcpm(128/4)
n("0 1 2 3 4 5 6 7").scale("C3:phrygian").s("triangle")
  .lpf(3000).decay(0.15).sustain(0)
```

```js
// Phrygian bass — dark, exotic techno
setcpm(128/4)
$: n("<0 1 0 ~ 3 0 5 0>*2").scale("C2:phrygian")
  .s("sawtooth").lpf(500).lpq(12).ftype("ladder")
  .decay(0.15).sustain(0)
  .gain(0.5)
```

> **The "Phrygian Dominant" variant:** Raise the 3rd of Phrygian (1 ♭2 3 4 5 ♭6 ♭7) and you get the **Phrygian Dominant** scale — the sound of flamenco. In Strudel: `"C:spanish"`.

---

#### 🟢 Lydian — *The Dreamscape*

**Formula:** 1 2 3 #4 5 6 7
**Strudel name:** `lydian`
**Mood:** Ethereal, dreamy, floating, otherworldly, expansive
**What makes it special:** The **#4** (raised 4th, a tritone above root) creates a sense of floating — the scale seems to "hover" rather than resolve.
**Genre use:** ⭐⭐ Melodic techno, ambient, progressive house, film scores
**Techno use:** Perfect for breakdowns, intros, and atmospheric sections. Creates a sense of wonder.

```js
setcpm(128/4)
n("0 1 2 3 4 5 6 7").scale("C3:lydian").s("triangle")
  .lpf(3000).decay(0.15).sustain(0)
```

```js
// Lydian pad — ethereal, floating
setcpm(128/4)
$: n("<0 2 3 4 6 4 2 0>").scale("C4:lydian")
  .s("triangle")
  .lpf(sine.range(1500, 4000).slow(16))
  .attack(0.4).sustain(0.5).release(1.5)
  .room(0.6).roomsize(0.8)
  .gain(0.18)
```

> **Pop culture:** The Lydian mode is why the *Simpsons* theme sounds so quirky and the opening of *E.T.* sounds so magical. That raised 4th is the "wonder" interval.

---

#### 🟠 Mixolydian — *The Bluesy Groove*

**Formula:** 1 2 3 4 5 6 ♭7
**Strudel name:** `mixolydian`
**Mood:** Bluesy, funky, laid-back, groovy, slightly rebellious
**What makes it special:** It's a major scale with a **♭7** — giving it a "dominant" sound that's restless but not sad.
**Genre use:** Funk, disco-house, acid house, breakbeat
**Techno use:** Rare in minimal techno. Used in more funky, groove-oriented electronic music.

```js
setcpm(128/4)
n("0 1 2 3 4 5 6 7").scale("C3:mixolydian").s("triangle")
  .lpf(3000).decay(0.15).sustain(0)
```

```js
// Mixolydian groove — funky, bluesy
setcpm(126/4)
$: n("<0 2 4 6 5 4 2 0>*2").scale("C3:mixolydian")
  .s("sawtooth").lpf(3500).decay(0.1).sustain(0)
  .delay(0.25).delaytime(0.1875)
  .gain(0.25)
```

---

#### 🔴 Aeolian (Natural Minor) — *The Classic Dark*

**Formula:** 1 2 ♭3 4 5 ♭6 ♭7
**Strudel name:** `minor` or `aeolian`
**Mood:** Sad, dark, melancholic, serious, emotional
**Genre use:** ⭐⭐⭐ The default scale for all dark electronic music
**Techno use:** THE standard scale for minimal and dark techno. If in doubt, use minor.

```js
setcpm(128/4)
n("0 1 2 3 4 5 6 7").scale("C3:minor").s("triangle")
  .lpf(3000).decay(0.15).sustain(0)
```

```js
// Minor pad — classic dark techno
setcpm(128/4)
$: chord("<Cm Fm Ab Bb>").voicing()
  .s("sawtooth")
  .lpf(sine.range(800, 2500).slow(16)).lpq(2)
  .attack(0.5).sustain(0.5).release(1.5)
  .room(0.45).gain(0.22)
```

> **Why minor dominates techno:** The ♭3 (minor 3rd) creates a dark, emotional character. The ♭6 and ♭7 reinforce the darkness. Almost every iconic techno track — from Robert Hood to Richie Hawtin to Recondite — uses minor tonality.

---

#### ⚫ Locrian — *The Unstable Edge*

**Formula:** 1 ♭2 ♭3 4 ♭5 ♭6 ♭7
**Strudel name:** `locrian`
**Mood:** Extremely dark, unstable, dissonant, uneasy, nightmarish
**What makes it special:** The **♭5** (diminished 5th) means even the most basic triad built on the root is diminished — inherently unstable.
**Genre use:** Very rare. Experimental, noise, some industrial techno
**Techno use:** Almost never as the primary scale. Use fragments of Locrian for moments of extreme tension.

```js
setcpm(128/4)
n("0 1 2 3 4 5 6 7").scale("C3:locrian").s("triangle")
  .lpf(3000).decay(0.15).sustain(0)
```

> **Use with caution:** Locrian is so unstable that it doesn't really have a "home" chord. The diminished 5th prevents any sense of rest. It's a color, not a foundation.

---

### Mode Comparison in One Listen

Play all seven modes back-to-back to hear the emotional spectrum:

```js
setcpm(60/4)
// Each mode plays for one cycle — same root, same pattern, different mood
n("0 1 2 3 4 5 6 7")
  .scale("<C3:ionian C3:dorian C3:phrygian C3:lydian C3:mixolydian C3:aeolian C3:locrian>")
  .s("triangle")
  .lpf(3000).decay(0.2).sustain(0)
```

---

## L.3 The Minor Scale Family

In electronic music, minor scales dominate. But "minor" isn't just one scale — it's a family of three, each with a different character:

### Natural Minor (Aeolian)

**Formula:** 1 2 ♭3 4 5 ♭6 ♭7
**Strudel:** `minor` or `aeolian`
**Character:** Pure darkness. No "leading tone" — the 7th doesn't pull strongly to the root. This makes it feel open and unresolved, which is perfect for hypnotic loops.

```js
n("0 1 2 3 4 5 6 7").scale("C3:minor")
```

### Harmonic Minor

**Formula:** 1 2 ♭3 4 5 ♭6 **7** (natural 7th!)
**Strudel:** `harmonicMinor`
**Character:** Dramatic, cinematic, slightly "Middle Eastern." The raised 7th creates a **leading tone** that pulls strongly to the root, plus an exotic **augmented 2nd interval** between ♭6 and 7.

```js
n("0 1 2 3 4 5 6 7").scale("C3:harmonicMinor")
```

```js
// Harmonic minor — dramatic, cinematic build
setcpm(128/4)
$: n("<0 2 4 6 7 6 4 2>").scale("C3:harmonicMinor")
  .s("sawtooth").lpf(3000).decay(0.12).sustain(0)
  .delay(0.3).delaytime(0.1875).delayfeedback(0.4)
  .gain(0.25)
```

> **Use in techno:** Harmonic minor is excellent for builds, transitions, and moments where you want to ramp up tension. The raised 7th creates a strong "pull" back to the root — perfect for drops.

### Melodic Minor (Ascending)

**Formula:** 1 2 ♭3 4 5 **6 7** (both raised!)
**Strudel:** `melodicMinor`
**Character:** A minor scale that "wants" to be major. Sophisticated, jazz-influenced, bittersweet. Only the ♭3 keeps it minor.

```js
n("0 1 2 3 4 5 6 7").scale("C3:melodicMinor")
```

> **Use in techno:** Less common than natural minor, but effective for melodic techno pads and leads that need emotional complexity without full major brightness.

### Which Minor Scale When?

| Minor Type | Strudel Name | Best For |
|-----------|-------------|----------|
| **Natural Minor** | `minor` | Default for all dark techno — hypnotic, unresolved |
| **Harmonic Minor** | `harmonicMinor` | Builds, drops, dramatic moments, cinematic techno |
| **Melodic Minor** | `melodicMinor` | Sophisticated pads, jazz-influenced melodic techno |

---

## L.4 Pentatonic Scales — The Safety Net

Pentatonic scales use only **5 notes** instead of 7. By removing the two most "dangerous" notes (the ones that create semitone clashes), pentatonic scales are almost impossible to sound bad with.

### Minor Pentatonic

**Formula:** 1 ♭3 4 5 ♭7
**Strudel:** `minor:pentatonic` or `minPent`
**Character:** Dark, safe, bluesy, universally musical
**Notes removed:** The 2nd and ♭6th — the two notes that create semitone intervals

```js
setcpm(128/4)
n("0 1 2 3 4").scale("C3:minor:pentatonic").s("triangle")
  .lpf(3000).decay(0.15).sustain(0)
```

> **Why it's the best beginner scale:** With only 5 notes and no semitone intervals, every combination sounds musical. Improvise freely with `irand` and it always sounds good.

```js
// Generative melody — always sounds good with minor pentatonic
setcpm(128/4)
$: n(irand(5).segment(8)).scale("C4:minor:pentatonic")
  .s("triangle").lpf(3000).decay(0.1).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .degradeBy(0.3).gain(0.25)
```

### Major Pentatonic

**Formula:** 1 2 3 5 6
**Strudel:** `major:pentatonic` or `majPent`
**Character:** Bright, happy, folk-like, uplifting
**Notes removed:** The 4th and 7th — the notes that create tension in the major scale

```js
n("0 1 2 3 4").scale("C3:major:pentatonic").s("triangle")
  .lpf(3000).decay(0.15).sustain(0)
```

> **Use in electronic music:** Major pentatonic is great for uplifting trance, progressive house, and euphoric breakdowns. It's the sound of "pure positivity."

### Blues Scale

**Formula:** 1 ♭3 4 ♭5 5 ♭7
**Strudel:** (build manually with `note()` or use chromatic + skip patterns)
**Character:** The minor pentatonic with one added "blue note" (♭5) — adds a soulful, gritty tension

```js
// Blues scale — minor pentatonic + the blue note
setcpm(128/4)
$: note("<c3 eb3 f3 gb3 g3 bb3 c4 bb3 g3 gb3 f3 eb3>")
  .s("sawtooth").lpf(2000).decay(0.12).sustain(0)
  .delay(0.3).delaytime(0.1875)
  .gain(0.3)
```

### Pentatonic Comparison

| Scale | Notes (C root) | Character | Best For |
|-------|---------------|-----------|----------|
| Minor Pentatonic | C Eb F G Bb | Dark, safe | ⭐ Default techno melody scale |
| Major Pentatonic | C D E G A | Bright, happy | Uplifting sections, trance |
| Blues | C Eb F Gb G Bb | Gritty, soulful | Acid house, funk techno |

---

## L.5 Exotic and World Scales

Strudel inherits TidalCycles' massive scale library — over 80 scales from traditions around the world. Here are the most useful ones for electronic music:

### Whole Tone

**Formula:** 1 2 3 #4 #5 #6 (all whole steps)
**Strudel:** `whole`
**Character:** Dreamlike, underwater, floating, no sense of "home"
**Why it's special:** With only whole steps, every note is equidistant — there's no leading tone, no pull, no resolution. Pure ambiguity.

```js
setcpm(128/4)
$: n("0 1 2 3 4 5").scale("C3:whole")
  .s("triangle")
  .lpf(sine.range(1000, 4000).slow(8))
  .attack(0.3).release(1.0)
  .room(0.6).roomsize(0.8)
  .gain(0.18)
```

> **Use:** Breakdowns, transitions, moments of disorientation. Not for sustained loops — the lack of resolution gets tiring.

### Hungarian Minor

**Formula:** 1 2 ♭3 #4 5 ♭6 7
**Strudel:** `hungarianMinor`
**Character:** Dramatic, dark, "gypsy," cinematic, two augmented seconds create unique tension
**Why it's special:** It combines the darkness of harmonic minor with the floating quality of the #4 (Lydian influence).

```js
setcpm(128/4)
$: n("0 1 2 3 4 5 6 7").scale("C3:hungarianMinor")
  .s("sawtooth").lpf(2500).decay(0.12).sustain(0)
  .delay(0.3).delaytime(0.1875).delayfeedback(0.4)
  .gain(0.25)
```

> **Use:** Dark techno, cinematic builds, industrial. The augmented seconds (♭3→#4 and ♭6→7) give it a distinctly "non-Western" character.

### Hirajoshi (Japanese)

**Formula:** 1 2 ♭3 5 ♭6 (5 notes)
**Strudel:** `hirajoshi`
**Character:** Melancholic, Japanese, sparse, contemplative
**Why it's special:** A pentatonic scale with two semitone intervals — more tension than the standard pentatonic, but still simple.

```js
setcpm(128/4)
$: n("<0 1 2 3 4 ~ 4 3>").scale("C3:hirajoshi")
  .s("triangle")
  .lpf(3000).decay(0.15).sustain(0)
  .delay(0.5).delaytime(0.1875).delayfeedback(0.6)
  .room(0.4).gain(0.2)
```

> **Use:** Ambient techno, lo-fi, downtempo, any track that needs an "Eastern" atmosphere without being a pastiche.

### Phrygian Dominant (Spanish)

**Formula:** 1 ♭2 3 4 5 ♭6 ♭7
**Strudel:** `spanish`
**Character:** Flamenco, Middle Eastern, dramatic, passionate
**Why it's special:** Phrygian with a major 3rd — the clash between ♭2 and 3 creates maximum exotic tension.

```js
setcpm(128/4)
$: n("<0 1 2 3 4 5 6 0>").scale("C3:spanish")
  .s("sawtooth").lpf(3000).decay(0.12).sustain(0)
  .delay(0.3).delaytime(0.1875)
  .gain(0.3)
```

### Diminished (Octatonic)

**Formula:** 1 2 ♭3 4 ♭5 ♭6 6 7 (8 notes — alternating W/H)
**Strudel:** `diminished`
**Character:** Tense, symmetrical, jazz, "spiraling," ominous
**Why it's special:** 8 notes instead of 7, built by alternating whole and half steps. Symmetrical — can be transposed by minor 3rds without changing the pitch set.

```js
setcpm(128/4)
$: n("0 1 2 3 4 5 6 7").scale("C3:diminished")
  .s("sawtooth").lpf(2000).decay(0.1).sustain(0)
  .gain(0.25)
```

> **Use:** Tension passages, horror-style builds, experimental techno. Very dissonant — use in short bursts.

### Prometheus

**Formula:** 1 2 3 #4 6 ♭7
**Strudel:** `prometheus`
**Character:** Mystical, otherworldly, Scriabin-esque
**Why it's special:** Derived from Scriabin's "mystic chord" — a whole-tone-adjacent scale with unique color.

```js
$: n("0 1 2 3 4 5").scale("C3:prometheus")
  .s("triangle").lpf(3000)
  .attack(0.3).release(1.2).room(0.5).gain(0.18)
```

### Complete Exotic Scale Reference

| Scale | Strudel Name | Notes | Character | Best Genre |
|-------|-------------|-------|-----------|------------|
| Whole Tone | `whole` | 6 | Dreamy, floating | Ambient, transitions |
| Hungarian Minor | `hungarianMinor` | 7 | Dark, dramatic, gypsy | Dark techno, cinematic |
| Hirajoshi | `hirajoshi` | 5 | Japanese, contemplative | Ambient, lo-fi |
| Spanish/Phryg. Dom. | `spanish` | 7 | Flamenco, exotic | Dark techno, psytrance |
| Diminished | `diminished` | 8 | Tense, spiraling | Tension builds |
| Prometheus | `prometheus` | 6 | Mystical, ethereal | Ambient, experimental |
| Romanian Minor | `romanianMinor` | 7 | Dark, folk | Eastern European techno |
| Enigmatic | `enigmatic` | 7 | Mysterious, unusual | Experimental |
| Bartók | `bartok` | 7 | Lydian + minor 7th | Melodic techno |
| Iwato | `iwato` | 5 | Japanese, darker | Dark ambient |
| Ritusen | `ritusen` | 5 | Celtic, folk | Melodic, crossover |
| Bhairav | `bhairav` | 7 | Indian classical, morning | World fusion |
| Todi | `todi` | 7 | Indian, deep, introspective | Ambient, meditation |
| Purvi | `purvi` | 7 | Indian, evening, dramatic | World fusion |

---

## L.6 Choosing the Right Scale — The Decision Framework

When you're starting a track, use this framework to find your scale:

### Step 1: Decide the Mood

| I want the track to feel... | Start with |
|-----------------------------|------------|
| Dark, serious, hypnotic | **C minor** (natural minor) |
| Dark but exotic / Eastern | **C Phrygian** |
| Dark and sophisticated / jazzy | **C Dorian** |
| Dark and dramatic / cinematic | **C harmonic minor** |
| Bright, uplifting, euphoric | **C major** or **C Lydian** |
| Dreamy, floating, ambiguous | **C Lydian** or **C whole** |
| Funky, bluesy, groovy | **C Mixolydian** or **C blues** |
| Minimal, safe, can't-go-wrong | **C minor pentatonic** |
| Exotic, world-music influenced | **C spanish**, **C hirajoshi**, **C bhairav** |

### Step 2: Choose the Root Note

The root note determines the bass frequency, which matters for how the track sits in a mix.

| Root | Sub-bass Frequency (Octave 1) | Character |
|------|-------------------------------|-----------|
| **C** | 32.7 Hz (C1), 65.4 Hz (C2) | ⭐ Most common for techno — sits well in most systems |
| **D** | 36.7 Hz (D1), 73.4 Hz (D2) | Slightly higher, still deep |
| **E** | 41.2 Hz (E1), 82.4 Hz (E2) | Clear bass range |
| **F** | 43.7 Hz (F1), 87.3 Hz (F2) | Warm, round bass |
| **G** | 49.0 Hz (G1), 98.0 Hz (G2) | Higher bass, more punch |
| **A** | 55.0 Hz (A1), 110 Hz (A2) | Standard tuning reference, bright |

> **For techno:** C, D, and F are the most popular root notes because their fundamental frequencies in octave 1–2 sit perfectly in the sub-bass sweet spot (30–80 Hz).

### Step 3: Verify by Playing

Paste this into Strudel, change the scale, and listen. Does it match your vision?

```js
setcpm(128/4)

// Test any scale — change the scale name to compare
$: n("0 2 4 6 4 2 0 ~").scale("C3:minor")
  .s("triangle").lpf(3000).decay(0.15).sustain(0)
  .delay(0.3).delaytime(0.1875)

// Bass root
$: note("c2").s("sine").lpf(200).decay(0.3).sustain(0.5)
```

### The Scale Selection Flowchart

```
                    What mood?
                       │
           ┌───────────┼───────────┐
           ▼           ▼           ▼
         DARK       BRIGHT    AMBIGUOUS
           │           │           │
     ┌─────┼─────┐     │     ┌────┼────┐
     ▼     ▼     ▼     ▼     ▼         ▼
  Simple Exotic Drama  Happy  Dream   Float
     │     │     │     │     │         │
   minor phrygian harm. major lydian  whole
     │     minor        │
     │                  │
   Need safety?     Need funk?
     │                  │
  pentatonic       mixolydian
```

---

## L.7 Genre-Specific Scale Recipes

### ⚫ Minimal Techno (126–132 BPM)

**Primary scales:** Natural minor, minor pentatonic
**Secondary:** Dorian, Phrygian
**Typical keys:** C minor, A minor, D minor

```js
setcpm(128/4)
// The classic minimal techno formula
$: n("<0 ~ 3 ~ 5 ~ 7 ~>").scale("C4:minor:pentatonic")
  .s("triangle").decay(0.15).sustain(0)
  .delay(0.5).delaytime(0.1875).delayfeedback(0.5)
  .degradeBy(0.25).gain(0.25)
```

**Philosophy:** Use fewer notes. Let repetition and delay create the melody. The scale is a safety net, not a playground — stay on 2–4 notes.

---

### 🏠 Deep House (118–122 BPM)

**Primary scales:** Dorian, natural minor
**Secondary:** Minor pentatonic, Mixolydian
**Typical keys:** D minor, A minor, G minor

```js
setcpm(120/4)
// Dorian deep house groove
$: n("<0 2 4 5 ~ 5 4 2>").scale("D3:dorian")
  .s("triangle").lpf(3000).decay(0.12).sustain(0)
  .delay(0.3).delaytime(0.25).delayfeedback(0.4)
  .gain(0.22)
```

**Philosophy:** Dorian's raised 6th gives deep house its signature warmth. Use 7th chords (Dm7, Gm7) to leverage the jazzy quality.

---

### 🌊 Dub Techno (120–130 BPM)

**Primary scales:** Natural minor, Phrygian
**Secondary:** Minor pentatonic
**Typical keys:** C minor, A minor, D minor

```js
setcpm(124/4)
// Phrygian dub techno — dark, submerged
$: n("<0 1 ~ 4 ~ 5 0 ~>").scale("C3:phrygian")
  .s("sawtooth")
  .lpf(sine.range(400, 1800).slow(32)).lpq(3)
  .attack(0.3).sustain(0.5).release(2.0)
  .room(0.8).roomsize(0.95).roomlp(2000)
  .delay(0.6).delaytime(0.375).delayfeedback(0.7)
  .gain(0.15)
```

**Philosophy:** Phrygian's ♭2 adds the perfect "submerged darkness." Let reverb and delay do the melodic work.

---

### 🌿 Ambient (No Fixed Tempo)

**Primary scales:** Lydian, whole tone, hirajoshi
**Secondary:** Minor, Dorian, Mixolydian
**Typical keys:** Any (root doesn't matter as much — it's about texture)

```js
setcps(0.08)
// Lydian ambient — floating, ethereal
$: n("<0 2 3 6 ~ 4 2 0>").scale("C4:lydian")
  .s("triangle")
  .lpf(perlin.range(800, 3000).slow(32))
  .attack(2.0).sustain(0.8).release(4.0)
  .room(0.9).roomsize(0.98).roomlp(3000)
  .gain(0.12)
```

**Philosophy:** Lydian's raised 4th creates the "floating" quality that is ambient's signature. Avoid strong resolution — stay modal.

---

### 🔮 Melodic Techno (124–130 BPM)

**Primary scales:** Natural minor, harmonic minor
**Secondary:** Dorian, Lydian (for breakdowns)
**Typical keys:** A minor, C minor, D minor

```js
setcpm(126/4)
// Harmonic minor lead — dramatic, emotional
$: n("<0 2 4 6 7 ~ 6 4>").scale("A3:harmonicMinor")
  .s("sawtooth").lpf(4000).decay(0.12).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .gain(0.22)
```

**Philosophy:** Use natural minor for the main body, switch to harmonic minor for builds (the raised 7th creates tension), and Lydian for breakdowns (ethereal release).

---

### 🔮 Trance (136–150 BPM)

**Primary scales:** Minor (for dark trance), Major (for uplifting)
**Secondary:** Harmonic minor, Lydian
**Typical keys:** A minor, D minor, C major

```js
setcpm(140/4)
// Uplifting trance lead — major scale, euphoric
$: n("<0 2 4 5 7 5 4 2>*2").scale("C4:major")
  .s("sawtooth").lpf(5000).decay(0.1).sustain(0)
  .delay(0.3).delaytime(0.1875).delayfeedback(0.4)
  .gain(0.25)
```

---

### 🥁 Drum & Bass (170–180 BPM)

**Primary scales:** Minor, minor pentatonic (liquid); Phrygian (dark/neuro)
**Secondary:** Dorian, harmonic minor
**Typical keys:** A minor, D minor, G minor

```js
setcpm(174/4)
// Liquid DnB melody — minor pentatonic, smooth
$: n("<0 ~ 2 ~ 4 ~ 3 1>").scale("A3:minor:pentatonic")
  .s("triangle").lpf(4000).decay(0.1).sustain(0)
  .delay(0.4).delaytime(0.109).delayfeedback(0.5)
  .gain(0.22)
```

---

### 🧪 Acid House (120–130 BPM)

**Primary scales:** Blues, minor pentatonic, Phrygian
**Secondary:** Minor, Mixolydian
**Typical keys:** C minor, A minor

```js
setcpm(126/4)
// Acid bass — blues scale through a 303
$: note("<c2 eb2 f2 gb2 g2 eb2 c2 bb1>")
  .s("sawtooth")
  .lpf(sine.range(200, 4000).fast(2)).lpq(18)
  .ftype("ladder")
  .decay(0.18).sustain(0)
  .distort(0.3).gain(0.45)
```

---

## L.8 The Complete Strudel Scale Reference

Strudel inherits all scale names from TidalCycles. Here is the complete list, grouped by category:

### Western Modes

| Strudel Name | Common Name | Notes |
|-------------|-------------|-------|
| `major` / `ionian` | Major / Ionian | 7 |
| `dorian` | Dorian | 7 |
| `phrygian` | Phrygian | 7 |
| `lydian` | Lydian | 7 |
| `mixolydian` | Mixolydian | 7 |
| `minor` / `aeolian` | Natural Minor / Aeolian | 7 |
| `locrian` | Locrian | 7 |

### Minor Variants

| Strudel Name | Common Name | Notes |
|-------------|-------------|-------|
| `harmonicMinor` | Harmonic Minor | 7 |
| `harmonicMajor` | Harmonic Major | 7 |
| `melodicMinor` | Melodic Minor (ascending) | 7 |
| `melodicMinorDesc` | Melodic Minor (descending) | 7 |
| `melodicMajor` | Melodic Major | 7 |

### Pentatonic Scales

| Strudel Name | Common Name | Notes |
|-------------|-------------|-------|
| `minor:pentatonic` / `minPent` | Minor Pentatonic | 5 |
| `major:pentatonic` / `majPent` | Major Pentatonic | 5 |
| `ritusen` | Ritusen (Celtic) | 5 |
| `egyptian` | Egyptian | 5 |
| `kumai` | Kumai (Japanese) | 5 |
| `hirajoshi` | Hirajoshi (Japanese) | 5 |
| `iwato` | Iwato (Japanese) | 5 |
| `chinese` | Chinese | 5 |

### Exotic / World Scales

| Strudel Name | Common Name | Notes |
|-------------|-------------|-------|
| `spanish` | Phrygian Dominant | 7 |
| `hungarianMinor` | Hungarian / Gypsy Minor | 7 |
| `romanianMinor` | Romanian Minor | 7 |
| `neapolitanMinor` | Neapolitan Minor | 7 |
| `neapolitanMajor` | Neapolitan Major | 7 |
| `enigmatic` | Enigmatic | 7 |
| `bartok` | Bartók (Lydian Dominant) | 7 |
| `superLocrian` | Super Locrian / Altered | 7 |
| `lydianMinor` | Lydian Minor | 7 |
| `locrianMajor` | Locrian Major | 7 |
| `leadingWhole` | Leading Whole Tone | 7 |

### Symmetrical Scales

| Strudel Name | Common Name | Notes |
|-------------|-------------|-------|
| `whole` | Whole Tone | 6 |
| `augmented` | Augmented | 6 |
| `augmented2` | Augmented 2 | 6 |
| `diminished` | Diminished (W-H) | 8 |
| `diminished2` | Diminished 2 (H-W) | 8 |
| `chromatic` | Chromatic | 12 |

### Hexatonic Scales

| Strudel Name | Common Name | Notes |
|-------------|-------------|-------|
| `hexMajor7` | Hexatonic Major 7 | 6 |
| `hexDorian` | Hexatonic Dorian | 6 |
| `hexPhrygian` | Hexatonic Phrygian | 6 |
| `hexSus` | Hexatonic Suspended | 6 |
| `hexMajor6` | Hexatonic Major 6 | 6 |
| `hexAeolian` | Hexatonic Aeolian | 6 |

### Indian Scales

| Strudel Name | Common Name | Notes |
|-------------|-------------|-------|
| `indian` | Indian | 7 |
| `bhairav` | Bhairav (morning raga) | 7 |
| `ahirbhairav` | Ahir Bhairav | 7 |
| `hindu` | Hindu | 7 |
| `todi` | Todi | 7 |
| `purvi` | Purvi | 7 |
| `marva` | Marva | 7 |

### Other

| Strudel Name | Common Name | Notes |
|-------------|-------------|-------|
| `pelog` | Pelog (Indonesian) | 5 |
| `prometheus` | Prometheus (Scriabin) | 6 |
| `scriabin` | Scriabin | 6 |
| `gong` | Gong (Chinese) | 5 |
| `shang` | Shang (Chinese) | 5 |
| `jiao` | Jiao (Chinese) | 5 |
| `zhi` | Zhi (Chinese) | 5 |
| `yu` | Yu (Chinese) | 5 |

> 💡 **Exploration exercise:** Pick any scale from this list and plug it into this template — then listen:
> ```js
> n("0 1 2 3 4 5 6 7").scale("C3:SCALENAME").s("triangle").lpf(3000).decay(0.15).sustain(0)
> ```

---

## L.9 Advanced Scale Techniques

### Switching Scales Mid-Track

Use the angle bracket pattern `<>` to change scales on different cycles:

```js
setcpm(128/4)
// Switch between minor and phrygian every 4 cycles
$: n("0 2 4 5 7 5 4 2")
  .scale("<C3:minor!4 C3:phrygian!4>")
  .s("triangle").lpf(3000).decay(0.15).sustain(0)
  .delay(0.3).delaytime(0.1875)
  .gain(0.25)
```

### Combining Scale Degrees with Note Names

You can use `n()` with `.scale()` for melodies while using `note()` for bass — mixing approaches:

```js
setcpm(128/4)
// Scale-based melody
$: n("<0 ~ 3 ~ 5 ~ 7 ~>").scale("C4:minor:pentatonic")
  .s("triangle").decay(0.12).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .gain(0.25)

// Fixed-note bass (root of the key)
$: note("c2").s("sine").lpf(200).decay(0.3).sustain(0.5).gain(0.5)
```

### Scale Transposition with `.scaleTranspose()`

Transpose by **scale degrees** instead of semitones — every shifted note stays in key:

```js
setcpm(128/4)
// Same melody, transposed up by 2 scale degrees each cycle
$: n("0 2 4 5")
  .scale("C3:minor")
  .scaleTranspose("<0 2 4 6>")
  .s("triangle").decay(0.12).sustain(0)
  .delay(0.3).delaytime(0.1875)
  .gain(0.25)
```

### Negative Scale Degrees

Go below the root using negative numbers — the scale wraps downward:

```js
setcpm(128/4)
$: n("<0 -1 -2 -3 0 2 4 2>").scale("C3:minor")
  .s("triangle").lpf(3000).decay(0.15).sustain(0)
```

### High Scale Degrees (Octave Wrapping)

Numbers above the scale length wrap into the next octave:

```js
// In minor (7 notes): degree 7 = root one octave up
// degree 8 = 2nd one octave up, etc.
n("0 2 4 7 9 11 14 12").scale("C3:minor")
  .s("triangle").lpf(3000).decay(0.15).sustain(0)
```

### Layering Different Scales

Create harmonic tension by using different (but compatible) scales in different layers:

```js
setcpm(128/4)

// Bass — natural minor
$: n("<0 0 3 0 5 0 3 0>").scale("C2:minor")
  .s("sawtooth").lpf(400).lpq(10).ftype("ladder")
  .decay(0.15).sustain(0).gain(0.5)

// Pad — Dorian (same minor feel, different 6th degree)
$: chord("<Cm Fm>").voicing()
  .s("sawtooth").lpf(2000)
  .attack(0.5).release(1.5).room(0.5)
  .gain(0.2)

// Lead — minor pentatonic (subset of both, always safe)
$: n("<0 ~ 2 ~ 4 ~ 3 ~>").scale("C4:minor:pentatonic")
  .s("triangle").decay(0.12).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .gain(0.22)
```

---

## L.10 Practice Challenges

### Challenge 1: Mode Ear Training ⭐

Play through all 7 modes on C3 using the comparison template from L.2. Write down which mode sounds "darkest" and which sounds "brightest" to your ear.

<details>
<summary>Solution</summary>

```js
setcpm(60/4)
n("0 1 2 3 4 5 6 7")
  .scale("<C3:ionian C3:dorian C3:phrygian C3:lydian C3:mixolydian C3:aeolian C3:locrian>")
  .s("triangle").lpf(3000).decay(0.2).sustain(0)
```

Most people rank (brightest → darkest): **Lydian > Ionian > Mixolydian > Dorian > Aeolian > Phrygian > Locrian**.
</details>

### Challenge 2: Scale-Based Melody ⭐⭐

Write a sparse melody (no more than 4 notes per cycle) using `n()` and `.scale("C4:dorian")`. Add delay. Keep it groovy.

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
$: n("<0 ~ 4 ~ 5 ~ 3 ~>").scale("C4:dorian")
  .s("triangle").lpf(3500).decay(0.12).sustain(0)
  .delay(0.45).delaytime(0.1875).delayfeedback(0.5)
  .degradeBy(0.2).gain(0.25)
```
</details>

### Challenge 3: Exotic Scale Exploration ⭐⭐

Take the Hirajoshi scale and create a slow, atmospheric line with long reverb. Make it sound "Eastern."

<details>
<summary>Solution</summary>

```js
setcpm(100/4)
$: n("<0 ~ 1 ~ 2 ~ 4 3>").scale("D3:hirajoshi")
  .s("triangle")
  .lpf(sine.range(1000, 3500).slow(16))
  .attack(0.2).sustain(0.3).release(1.5)
  .room(0.7).roomsize(0.85).roomlp(2500)
  .delay(0.5).delaytime(0.25).delayfeedback(0.6)
  .gain(0.18)
```
</details>

### Challenge 4: Scale Switching ⭐⭐⭐

Write a pattern where the scale switches between natural minor and harmonic minor every 4 cycles. The harmonic minor sections should feel more "dramatic."

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
$: n("0 2 4 6 7 6 4 2")
  .scale("<C3:minor!4 C3:harmonicMinor!4>")
  .s("sawtooth").lpf(3000).decay(0.12).sustain(0)
  .delay(0.3).delaytime(0.1875).delayfeedback(0.4)
  .gain(0.25)
```

Notice how the raised 7th in harmonic minor creates a stronger "pull" back to the root — more dramatic, more cinematic.
</details>

### Challenge 5: Full Scale Stack ⭐⭐⭐

Build a complete track snippet using 3 layers, each using a different scale approach: bass (fixed notes), pad (chords in minor), melody (generative with minor pentatonic).

<details>
<summary>Solution</summary>

```js
setcpm(128/4)

// Bass — fixed note (root of key)
$: note("<c2!4 f2!4>").s("sine")
  .lpf(200).decay(0.3).sustain(0.5).gain(0.5)

// Pad — chords in C minor
$: chord("<Cm!4 Fm!4>").voicing()
  .s("sawtooth")
  .lpf(sine.range(800, 2500).slow(16)).lpq(2)
  .attack(0.5).sustain(0.5).release(1.5)
  .room(0.5).roomsize(0.7)
  .gain(0.2).orbit(2)

// Melody — generative minor pentatonic
$: n(irand(5).segment(8)).scale("C4:minor:pentatonic")
  .s("triangle").lpf(3500).decay(0.1).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .degradeBy(0.3).gain(0.22)
```
</details>

---

## L.11 Key Takeaways

1. **A scale = a recipe of intervals** — change the recipe, change the mood
2. **Seven modes, one parent scale** — Ionian through Locrian are the same notes starting on different degrees
3. **Minor is king in techno** — natural minor (Aeolian) is the default; Dorian and Phrygian for variety
4. **Pentatonic = safety net** — only 5 notes, no semitone clashes, always sounds good
5. **Harmonic minor = drama** — use it for builds and drops, the raised 7th creates tension
6. **Lydian = dreams** — the raised 4th floats above reality; perfect for breakdowns
7. **Phrygian = exotic darkness** — the ♭2 is the most distinctive interval in electronic music
8. **80+ scales in Strudel** — from Bhairav to Bartók, the entire world is available
9. **The root note matters** — C and D give the best sub-bass for techno
10. **Experiment with `.scale()`** — change one word and the entire mood shifts; that's the power of scales

---

**← Back to [Module 8 — Writing Chords & Melodies](./Module_08_Chords_Melodies.md)**
**← Back to [Appendix K — The Theory of Perfect Chords](./Appendix_K_Chord_Theory.md)**
**← Back to [README — Course Overview](./README.md)**
