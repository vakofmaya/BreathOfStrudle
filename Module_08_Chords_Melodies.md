# Module 8 — Writing Chords & Melodies

> **Goal:** Learn how to write pitched music in Strudel — from note names and MIDI numbers to scales, chord progressions, voicings, arpeggios, and melodic lines. Build the harmonic and melodic elements for your Minimal Techno track.

---

## 8.1 Notes in Strudel

There are three ways to specify pitch:

### Note Names — `note()`

Standard letter names with optional sharp `s` / flat `b` and octave number:

```js
// C minor scale ascending
note("c3 d3 eb3 f3 g3 ab3 bb3 c4")

// Middle C = c4, sub bass = c1-c2, high = c5+
note("c1").s("sine")   // very low
note("c4").s("sine")   // middle
note("c6").s("sine")   // high
```

**Note naming:**

| Name | Meaning |
|------|---------|
| `c` | C natural |
| `cs` or `c#` | C sharp |
| `db` | D flat |
| `eb` | E flat |
| `3` (suffix) | Octave 3 |

```js
// Sharps and flats
note("c3 cs3 d3 eb3 e3 f3 fs3 g3 ab3 a3 bb3 b3 c4")
```

### MIDI Numbers — `note()` with numbers

```js
// MIDI 60 = middle C (c4)
note("48 51 55 58")    // = c3 eb3 g3 bb3

// Useful for calculated patterns
note("48 60 72 84")    // C across 4 octaves
```

### Scale Degrees — `n()` with `.scale()`

Instead of specifying exact notes, use numbers as **degrees of a scale**:

```js
// 0 = root, 1 = 2nd, 2 = 3rd, 3 = 4th, etc.
n("0 1 2 3 4 5 6 7").scale("C3:minor")
```

This is the most powerful approach — change the scale and all notes follow:

```js
// Same pattern, different scale = completely different mood
n("0 2 4 6 4 2 0 ~").scale("C3:minor")       // dark
n("0 2 4 6 4 2 0 ~").scale("C3:major")       // bright
n("0 2 4 6 4 2 0 ~").scale("C3:dorian")      // jazzy
n("0 2 4 6 4 2 0 ~").scale("C3:phrygian")    // Eastern
```

> 💡 **Exercise:** Play `n("0 1 2 3 4 5 6 7").scale("C3:minor")`. Then change `minor` to `major`, `dorian`, `phrygian`, `minor:pentatonic`. Notice how the same pattern changes mood.

---

## 8.2 Scales — Your Musical Safety Net

A **scale** is a set of notes that "go together." When you use `.scale()`, every number you write snaps to a note in that scale — you can't play a wrong note.

### How `.scale()` Works

```js
// C minor scale: C D Eb F G Ab Bb
// Degrees:       0 1  2 3 4  5  6

n("0").scale("C3:minor")    // → C3
n("1").scale("C3:minor")    // → D3
n("2").scale("C3:minor")    // → Eb3
n("3").scale("C3:minor")    // → F3
n("7").scale("C3:minor")    // → C4 (wraps to next octave)
```

Negative numbers go down:

```js
n("-1").scale("C3:minor")   // → Bb2 (one scale step below root)
n("-2").scale("C3:minor")   // → Ab2
```

### Scales for Minimal Techno

| Scale | Strudel Name | Character | Best For |
|-------|-------------|-----------|----------|
| Natural Minor | `C:minor` | Dark, classic | Standard dark techno |
| Minor Pentatonic | `C:minor:pentatonic` | Safe, melodic | Melodies (can't go wrong) |
| Dorian | `C:dorian` | Slightly brighter minor | Groovier techno |
| Phrygian | `C:phrygian` | Dark, Middle Eastern | Exotic, tension |
| Harmonic Minor | `C:harmonic:minor` | Dramatic, cinematic | Builds, drops |
| Whole Tone | `C:whole:tone` | Dreamy, floating | Breakdowns, transitions |
| Chromatic | `C:chromatic` | All notes, no safety net | Experimental |
| Lydian | `C:lydian` | Bright, ethereal | Melodic techno |

### Specifying Root and Octave

```js
n("0 2 4 6").scale("C3:minor")     // root = C, octave 3
n("0 2 4 6").scale("A2:minor")     // root = A, octave 2
n("0 2 4 6").scale("F#3:dorian")   // root = F#, octave 3
```

> 💡 **Exercise:** Write `n("0 2 4 5 7 9 11 12")` and try it with these scales:
> - `.scale("C3:minor")`
> - `.scale("C3:minor:pentatonic")`
> - `.scale("C3:phrygian")`
>
> Which one sounds most "techno" to you?

---

## 8.3 Chords — Stacking Notes

### Manual Chords (Polyphony)

Stack notes using commas within brackets:

```js
// C minor triad
note("[c3,eb3,g3]")

// Four-chord progression — one chord per cycle
note("<[c3,eb3,g3] [bb2,d3,f3] [ab2,c3,eb3] [g2,bb2,d3]>")
  .s("sawtooth").lpf(2000)
```

### Using `chord()`

Strudel understands chord names directly:

```js
// Chord name format: Root + Quality
chord("Cm").voicing()       // C minor
chord("Cmaj").voicing()     // C major
chord("C7").voicing()       // C dominant 7th
chord("Cm7").voicing()      // C minor 7th
chord("Cdim").voicing()     // C diminished
chord("Caug").voicing()     // C augmented
```

### Chord Progressions

```js
// Classic dark techno progression
chord("<Cm Bbm Ab G>").voicing()
  .s("sawtooth").lpf(2000)
  .room(0.3)
```

```js
// More chords per cycle
chord("<Cm Fm Bbm Eb>*2").voicing()
  .s("triangle")
```

### Common Chord Progressions for Minimal Techno

All in C minor — the most common key for dark techno:

```js
// i - VII - VI - V (classic dark minor)
chord("<Cm Bb Ab G>").voicing()

// i - iv - VII - III (deep house/techno)
chord("<Cm Fm Bb Eb>").voicing()

// i - iv (minimal two-chord loop)
chord("<Cm Fm>").voicing()

// i - VI - III - VII (dramatic)
chord("<Cm Ab Eb Bb>").voicing()

// i - v - iv - VII (moody)
chord("<Cm Gm Fm Bb>").voicing()
```

> 💡 **Exercise:** Try each progression above with `.s("sawtooth").lpf(1500).attack(0.3).release(0.8).room(0.4)`. Which one do you like best for your track?

---

## 8.4 The Voicing System

When you call `.voicing()`, Strudel automatically:
1. **Spreads the chord notes** across a comfortable range
2. **Applies voice leading** — minimizes jumps between consecutive chords
3. Uses a **voicing dictionary** to determine note arrangements

### Why Voice Leading Matters

Without voice leading, chords jump around:
```
Cm  → [C3, Eb3, G3]
Bbm → [Bb2, Db3, F3]   ← big jump down
Ab  → [Ab2, C3, Eb3]   ← another jump
```

With voice leading, notes move smoothly:
```
Cm  → [C4, Eb4, G4]
Bbm → [Bb3, Db4, F4]   ← each note moves by 1-2 steps
Ab  → [Ab3, C4, Eb4]   ← smooth motion
```

This sounds much more musical and professional.

### Voicing Parameters

```js
chord("<Cm Bbm Ab G>").voicing()
  .anchor("c4")          // center voicings around C4
  .mode("below")         // place notes below anchor
```

**Anchor modes:**
- `"below"` — all notes below the anchor
- `"above"` — all notes above the anchor

### Custom Voicing Dictionaries

You can define exactly how chords should be voiced:

```js
addVoicings('techno', {
  'm':    ['3 7 12', '7 12 15'],      // minor: 2 possible voicings
  'maj':  ['4 7 12', '7 12 16'],      // major
  '7':    ['4 7 10', '7 10 16'],      // dominant 7
  'm7':   ['3 7 10', '7 10 15'],      // minor 7
})

chord("<Cm7 Fm Bbm Ab>*2")
  .dict('techno')
  .anchor(60)             // MIDI note 60 = C4
  .voicing()
  .s("triangle")
  .attack(0.2).release(0.8)
  .room(0.5)
```

The numbers in the array are **intervals in semitones** from the root:
- `3` = minor third
- `4` = major third
- `7` = perfect fifth
- `10` = minor seventh
- `12` = octave

> 💡 **Exercise:** Create a simple two-chord loop with `chord("<Cm Fm>").voicing()` and apply different synth sounds. Try `"sawtooth"`, `"triangle"`, `"square"`, and `"wt_flute"`.

---

## 8.5 Arpeggios — Chords as Melodies

An **arpeggio** breaks a chord into individual notes played one after another. This is a staple of electronic music.

### Manual Arpeggio

```js
setcpm(128/4)
// Play chord tones one by one
note("<[c3 eb3 g3 c4] [bb2 d3 f3 bb3] [ab2 c3 eb3 ab3] [g2 bb2 d3 g3]>")
  .s("triangle")
  .lpf(3000)
  .decay(0.2).sustain(0)
  .delay(0.3).delaytime(0.1875)
```

### Using `.arp()` or `.struct()` with Chords

```js
setcpm(128/4)
// Chord with rhythmic pattern
chord("<Cm Fm Bbm Ab>")
  .voicing()
  .struct("[x x x x]*2")         // play 8 evenly spaced notes
  .s("triangle")
  .lpf(3000)
  .decay(0.15).sustain(0)
  .delay(0.3)
```

### Scale-Based Arpeggios

```js
setcpm(128/4)
// Arpeggio using scale degrees (0, 2, 4 = root, 3rd, 5th)
n("<[0 2 4 7] [0 2 4 6] [-1 1 3 5] [-2 0 2 4]>*2")
  .scale("C3:minor")
  .s("triangle")
  .lpf(4000)
  .decay(0.15).sustain(0)
  .delay(0.3).delaytime(0.1875).delayfeedback(0.4)
```

---

## 8.6 Writing Melodies

### Approach A: Scale Degrees (Recommended)

Using scale degrees is the easiest way to write melodies that stay in key:

```js
setcpm(128/4)
n("0 2 4 5 7 5 4 2").scale("C4:minor:pentatonic")
  .s("triangle")
  .lpf(4000)
  .decay(0.15).sustain(0)
```

### Approach B: Note Names

For more precise control:

```js
setcpm(128/4)
note("c4 eb4 g4 f4 eb4 d4 c4 ~")
  .s("triangle")
  .lpf(4000)
  .decay(0.15).sustain(0)
```

### Approach C: Generative / Random Melodies

Let Strudel compose for you:

```js
setcpm(128/4)
// Random notes in scale, quantized to 8 steps per cycle
n(irand(8).segment(8))
  .scale("C4:minor:pentatonic")
  .s("triangle")
  .lpf(3000)
  .decay(0.1).sustain(0)
```

```js
// Sine-driven melody (smooth, repeating)
n(sine.segment(16).range(0, 7))
  .scale("C4:minor")
  .s("triangle")
  .lpf(2500)
  .decay(0.1).sustain(0)
```

```js
// Perlin noise melody (organic, evolving)
n(perlin.segment(8).range(0, 12))
  .scale("C3:minor:pentatonic")
  .s("sawtooth")
  .lpf(2000)
  .decay(0.2).sustain(0)
```

### Melodic Tips for Minimal Techno

1. **Less is more** — use rests (`~`) generously
2. **Repetition** — repeat phrases with small variations
3. **Small intervals** — stepwise motion (1-2 scale steps) sounds smoother
4. **Anchor notes** — return to the root (0) frequently
5. **Delay is your friend** — it fills space without adding notes

```js
// Good minimal melody: sparse, repetitive, delayed
setcpm(128/4)
n("<0 ~ 3 ~ 5 ~ 7 ~>")
  .scale("C4:minor:pentatonic")
  .s("triangle")
  .decay(0.15).sustain(0)
  .delay(0.5).delaytime(0.1875).delayfeedback(0.5)
```

```js
// Bad for minimal: too many notes, no space
n("0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15")
  .scale("C4:minor")
  .s("triangle")
```

> 💡 **Exercise:** Write a 4-note melody using `n()` and `.scale("C4:minor:pentatonic")`. Add rests with `~`. Add `.delay()` for echo. Keep it minimal!

---

## 8.7 Transposition

### Transpose by Semitones — `.transpose()`

Shift all notes up or down by a fixed number of semitones:

```js
// Original
note("c3 eb3 g3")

// Up 2 semitones (one whole step)
note("c3 eb3 g3").transpose(2)

// Pattern the transposition
note("c3 eb3 g3").transpose("<0 -2 -5 -7>")
```

### Transpose Within Scale — `.scaleTranspose()`

Shift by scale degrees instead of semitones — stays in key:

```js
// Original: 0 2 4 = C Eb G in C minor
n("0 2 4").scale("C3:minor")

// Shift up 1 scale degree
n("0 2 4").scale("C3:minor").scaleTranspose(1)  // → D F Ab

// Pattern it
n("0 2 4").scale("C3:minor")
  .scaleTranspose("<0 1 2 3>")  // shift each cycle
```

### Transpose for Key Changes

```js
setcpm(128/4)
// Same melody, different keys each cycle
note("c3 eb3 g3 f3")
  .transpose("<0 -2 -5 -7>")
  .s("triangle")
  .lpf(3000)
  .decay(0.15).sustain(0)
```

### `.add(note(N))` — Add Intervals

Add a fixed interval to create harmonies:

```js
// Add an octave above sometimes
n("0 2 4 5").scale("C4:minor:pentatonic")
  .s("triangle")
  .sometimes(x => x.add(note(12)))    // octave up 50% of the time
```

```js
// Add a fifth above
n("0 2 4 5").scale("C3:minor")
  .s("sawtooth")
  .add(note("<0 7>"))                  // root or root+fifth alternating
  .lpf(2000)
```

---

## 8.8 Combining Chords and Melodies

In a full track, chords and melodies should complement each other:

```js
setcpm(128/4)

// Pad — slow chords
$: chord("<Cm7 Fm Abmaj7 G7>").voicing()
  .s("sawtooth")
  .lpf(sine.range(1200, 2500).slow(16))
  .attack(0.4).sustain(0.5).release(1)
  .room(0.5).roomsize(0.7)
  .gain(0.2)
  .orbit(2)

// Bass — follows the root of each chord
$: note("<c2 f2 ab2 g2>")
  .s("sine")
  .lpf(200)
  .decay(0.3).sustain(0.5)
  .gain(0.5)

// Melody — pentatonic over the top
$: n("<0 ~ 3 ~ 5 ~ 7 ~>*2")
  .scale("C4:minor:pentatonic")
  .s("triangle")
  .decay(0.15).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .degradeBy(0.3)
  .gain(0.25)

// Arpeggio — broken chord pattern
$: n("<[0 2 4] [0 2 4] [-1 1 3] [-2 0 2]>")
  .scale("C3:minor")
  .s("triangle")
  .lpf(3000)
  .decay(0.1).sustain(0)
  .delay(0.3).delaytime(0.1875)
  .gain(0.15)
```

### Bass Following Chords

The bass should generally play the **root** of the current chord:

| Chord | Bass Note |
|-------|-----------|
| Cm | C |
| Fm | F |
| Ab | Ab |
| Bb | Bb |
| G | G |

```js
// Match bass to chord progression
$: chord("<Cm Fm Ab Bb>").voicing().s("sawtooth").lpf(2000).gain(0.2)
$: note("<c2 f2 ab2 bb2>").s("sine").lpf(200).gain(0.5)
```

---

## 8.9 Musical Intervals — A Quick Reference

Understanding intervals helps you build custom chords and melodies:

| Semitones | Interval | Sound | Example from C |
|-----------|----------|-------|---------------|
| 0 | Unison | Same note | C → C |
| 1 | Minor 2nd | Tense, dissonant | C → Db |
| 2 | Major 2nd | Stepwise | C → D |
| 3 | Minor 3rd | Sad, dark | C → Eb |
| 4 | Major 3rd | Happy, bright | C → E |
| 5 | Perfect 4th | Open, strong | C → F |
| 7 | Perfect 5th | Powerful, stable | C → G |
| 10 | Minor 7th | Jazzy, tension | C → Bb |
| 12 | Octave | Same note, higher | C → C |

---

## 8.10 Practice Challenges

### Challenge 1: Chord Progression
Write a 4-chord progression in C minor using `chord()` and `.voicing()`. Apply a pad-like envelope:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
chord("<Cm Fm Ab Bb>").voicing()
  .s("sawtooth")
  .lpf(2000).lpq(2)
  .attack(0.4).sustain(0.5).release(1)
  .room(0.4)
```
</details>

### Challenge 2: Pentatonic Melody
Write a sparse melody using `n()` and `scale("C4:minor:pentatonic")`. Use no more than 4 notes per cycle, with rests:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
n("<0 ~ 4 ~ , ~ 2 ~ 5>")
  .scale("C4:minor:pentatonic")
  .s("triangle")
  .decay(0.15).sustain(0)
  .delay(0.4).delaytime(0.1875)
  .gain(0.3)
```
</details>

### Challenge 3: Arpeggio
Create an arpeggio that follows a chord progression:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
n("<[0 2 4 7]*2 [0 2 4 6]*2 [-1 1 3 5]*2 [-2 0 2 4]*2>")
  .scale("C3:minor")
  .s("triangle")
  .lpf(3000)
  .decay(0.12).sustain(0)
  .delay(0.3).delaytime(0.1875).delayfeedback(0.4)
```
</details>

### Challenge 4: Generative Melody
Create a melody that uses `irand()` or `sine` to generate notes, quantized to a scale:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
n(irand(8).segment(8))
  .scale("C4:minor:pentatonic")
  .s("triangle")
  .lpf(3000)
  .decay(0.1).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .degradeBy(0.3)
  .gain(0.3)
```
</details>

### Challenge 5: Full Harmonic Stack
Build a complete harmonic layer with bass root + chord pad + melody, all in the same key:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)

// Bass
$: note("<c2 f2 ab2 bb2>").s("sine")
  .lpf(200).decay(0.3).sustain(0.5)
  .gain(0.5)

// Pad
$: chord("<Cm Fm Ab Bb>").voicing()
  .s("sawtooth").lpf(1800).lpq(2)
  .attack(0.4).sustain(0.5).release(1)
  .room(0.4).gain(0.2)

// Melody
$: n("<0 ~ 4 7 ~ 4 2 ~>")
  .scale("C4:minor:pentatonic")
  .s("triangle")
  .decay(0.12).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .gain(0.25)
```
</details>

---

## 8.11 Key Takeaways

1. Three ways to specify pitch: **note names** (`note("c3")`), **MIDI numbers** (`note(48)`), **scale degrees** (`n(0).scale("C3:minor")`)
2. `.scale()` is your safety net — you can't play wrong notes
3. **Minor pentatonic** is the safest scale for techno melodies
4. Use `chord("Cm").voicing()` for automatic, voice-led chord progressions
5. Arpeggios = chords played as melodies = instant musicality
6. `.transpose()` shifts by semitones, `.scaleTranspose()` shifts by scale degrees
7. **Less is more** — Minimal Techno melodies use space and repetition
8. **Delay** is the most important melodic effect — it fills gaps naturally
9. Bass should follow the **root** of the chord progression
10. Generative melodies (`irand`, `sine.segment`) add organic variation

---

**Previous:** [← Module 7 — Building Core Sounds](./Module_07_Building_Core_Sounds.md)
**Next:** [Module 9 — Effects & Spatial Processing →](./Module_09_Effects_Spatial.md)
