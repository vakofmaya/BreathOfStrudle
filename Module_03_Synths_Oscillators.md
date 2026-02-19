# Module 3 — Synths & Oscillators

> **Goal:** Understand how Strudel generates sound from scratch using oscillators. Learn the four basic waveforms, noise types, FM synthesis, and wavetable synthesis — then use them to design sounds for Minimal Techno.

---

## 3.1 How Sound Synthesis Works

Instead of playing back a **recorded sample** (like a WAV file of a drum hit), a **synthesizer** generates sound mathematically by producing a repeating waveform — an **oscillator**.

In Strudel, you select a synth waveform with `sound()` (or its shorthand `s()`) or by just setting `note()` (which defaults to a triangle wave).

```js
// Play a note using a synth
note("c3").sound("sine")

// Shorthand
note("c3").s("sine")

// Just note() without sound() defaults to "triangle"
note("c3")
```

---

## 3.2 The Four Basic Waveforms

Every synthesizer starts with one or more of these fundamental shapes. Each has a different **harmonic content** (the overtones above the base frequency), which is what gives it its **timbre** (tonal character).

### Sine Wave

```js
note("c3").s("sine")._scope()
```

- **Shape:** Smooth, rounded
- **Harmonics:** None — only the fundamental frequency
- **Sound:** Pure, clean, "electronic"
- **Use in Techno:** Sub-bass, kick drum (with pitch envelope), pure test tones
- The `._scope()` at the end shows a real-time oscilloscope — use it to see the waveform!

**Listen and compare octaves:**
```js
note("c1 c2 c3 c4").s("sine")
```

### Sawtooth Wave

```js
note("c3").s("sawtooth")._scope()
```

- **Shape:** Ramp up, sharp drop
- **Harmonics:** All harmonics (1st, 2nd, 3rd, 4th...) — very rich
- **Sound:** Buzzy, bright, "classic synth"
- **Use in Techno:** Bass lines, pads, leads — the workhorse of subtractive synthesis

**The sawtooth is the most harmonically rich basic waveform.** This matters because filtering (Module 4) works by removing harmonics — and you need harmonics to remove.

```js
note("c2 c2 c2 c2").s("sawtooth")
```

### Square Wave

```js
note("c3").s("square")._scope()
```

- **Shape:** Flat top, flat bottom, sharp jumps
- **Harmonics:** Only odd harmonics (1st, 3rd, 5th, 7th...)
- **Sound:** Hollow, nasal, "chiptune" or "Game Boy"
- **Use in Techno:** Stabs, hollow bass, retro leads

```js
note("c2 c2 c2 c2").s("square")
```

### Triangle Wave

```js
note("c3").s("triangle")._scope()
```

- **Shape:** Smooth zig-zag
- **Harmonics:** Only odd harmonics, but quieter than square (they fall off much faster)
- **Sound:** Soft, mellow, between sine and square
- **Use in Techno:** Soft leads, gentle melodies, pads
- **This is the default** when you write just `note()` without specifying a sound

```js
note("c3 e3 g3 c4")   // defaults to triangle
```

### Side-by-Side Comparison

```js
// Hear all four waveforms cycle through
note("c2 <eb2 <g2 g1>>".fast(2))
  .sound("<sawtooth square triangle sine>")
  ._scope()
```

> 💡 **Exercise:** Play each waveform on the same note (`c2`) and listen carefully. Which one sounds brightest? Which sounds purest? Which sounds most "buzzy"?

---

## 3.3 Harmonic Content — Why It Matters

Here's a visual way to think about it:

| Waveform | Harmonics Present | Brightness |
|----------|-------------------|-----------|
| **Sine** | Fundamental only | 🔇 Dullest |
| **Triangle** | Odd harmonics (quiet) | 🔈 Soft |
| **Square** | Odd harmonics (loud) | 🔉 Hollow but present |
| **Sawtooth** | All harmonics | 🔊 Brightest |

This directly affects what happens when you apply **filters** (Module 4):
- A **sine wave** through a low-pass filter barely changes — there are no harmonics to remove
- A **sawtooth** through a low-pass filter changes dramatically — you're sculpting all those harmonics

> **Rule of thumb:** Start with `sawtooth` when you want to shape a sound with filters. Start with `sine` when you want something pure.

---

## 3.4 Playing Notes

### Note Names

Strudel uses standard note naming: `c d e f g a b`, with sharps (`#`) and flats (`b`):

```js
// C minor scale
note("c3 d3 eb3 f3 g3 ab3 bb3 c4")
```

```js
// Note name + octave number (middle C = c4, sub bass = c1-c2)
note("c1")    // very low
note("c2")    // low (typical bass range)
note("c3")    // mid
note("c4")    // middle C
note("c5")    // high
```

### Using `n()` with `.scale()`

Instead of note names, you can use **numbers** (scale degrees) and let `.scale()` convert them:

```js
// 0 = root, 1 = 2nd, 2 = 3rd, etc.
n("0 1 2 3 4 5 6 7").scale("C:minor")
```

This is extremely powerful for staying in key:

```js
// Random notes, always in C minor pentatonic
n(irand(8).segment(8)).scale("C:minor:pentatonic").s("triangle")
```

We'll dive deeper into scales in Module 8, but it's useful to know now.

> 💡 **Exercise:** Play `note("c2 eb2 f2 g2")` with each of the four waveforms. Notice how the same melody has a completely different character depending on the oscillator.

---

## 3.5 Noise Sources

Noise is **random sound** — it contains all frequencies without a definite pitch. It's essential for percussion, ambience, and special effects.

### White Noise
All frequencies at equal volume. Harsh, bright, "TV static":

```js
s("white")._scope()
```

### Pink Noise
Lower frequencies louder, higher frequencies quieter. Warmer, more natural:

```js
s("pink")._scope()
```

### Brown Noise
Even more low-frequency emphasis. Deep, rumbly, like wind:

```js
s("brown")._scope()
```

### Comparison

```js
// Hear them alternate
s("<white pink brown>")._scope()
```

### Noise as Percussion

Noise with a very short envelope makes great hi-hats and percussion:

```js
// Noise hi-hats
s("bd*2, <white pink brown>*8")
  .decay(.04).sustain(0)._scope()
```

> We haven't covered `.decay()` and `.sustain()` in detail yet (that's Module 6), but the idea is simple: `.decay(.04)` makes the sound fade out in 0.04 seconds, and `.sustain(0)` means it doesn't hold — so you get a very short click/tick.

### Adding Noise to an Oscillator

Blend noise into any oscillator with the `.noise()` parameter:

```js
// Add increasing amounts of noise to a sine
note("c3").noise("<0.1 0.25 0.5>")._scope()
```

### Crackle — Vinyl Texture

The `crackle` type produces subtle random pops and clicks, great for atmosphere:

```js
s("crackle*4").density("<0.01 0.04 0.2 0.5>".slow(2))._scope()
```

> 💡 **Exercise:** Create a hi-hat pattern using only noise. Try: `s("white*16").decay(.03).sustain(0)`. Experiment with different decay values.

---

## 3.6 FM Synthesis

FM (Frequency Modulation) synthesis was popularized by the Yamaha DX7 in the 1980s. It creates complex timbres by using one oscillator (the **modulator**) to rapidly change the frequency of another (the **carrier**).

In Strudel, you control FM with two main parameters:

| Parameter | What it does | Range |
|-----------|-------------|-------|
| `fm` | **Modulation index** — how much the frequency wobbles. Higher = more harmonics = more "metallic" | 0 → ~20+ |
| `fmh` | **Harmonicity ratio** — the frequency ratio between modulator and carrier. Integer ratios sound harmonic, non-integer ratios sound inharmonic/bell-like | 0.5 → ~10+ |

### Basic FM

```js
// Low FM = subtle warmth
note("c3").fm(1)._scope()

// Medium FM = organ-like harmonics
note("c3").fm(3)._scope()

// High FM = metallic, bell-like
note("c3").fm(8)._scope()
```

### Harmonicity Ratio

```js
// Integer ratios = musical overtones
note("c3").fm(4).fmh(1)._scope()    // fundamental
note("c3").fm(4).fmh(2)._scope()    // octave above
note("c3").fm(4).fmh(3)._scope()    // octave + fifth

// Non-integer = inharmonic (bell, metallic)
note("c3").fm(4).fmh(1.41)._scope()
note("c3").fm(4).fmh(3.14)._scope()
```

### FM Envelope — Decaying Modulation

In real-world sounds, harmonics usually die out faster than the fundamental. FM envelope simulates this:

```js
// fmattack: how long FM takes to reach peak
// fmdecay: how long FM takes to die away
// fmsustain: FM level after decay
// fmenv: overall FM envelope depth

note("c3").fm(6).fmh(2)
  .fmattack(0).fmdecay(0.3).fmsustain(0)
  ._scope()
```

The modulation starts strong and fades, producing a bright attack that mellows out — like a plucked or struck sound.

### FM for Techno

**Metallic percussion:**
```js
// Bell / ride cymbal
note("c5 c5 c5 c5").fm(8).fmh(3.5)
  .decay(0.3).sustain(0)
```

**Tonal kick enhancement:**
```js
// FM adds bite to the kick attack
note("c1!4").s("sine")
  .fm(2).fmh(1).fmdecay(0.05).fmsustain(0)
  .decay(0.4).sustain(0)
```

**Evolving texture:**
```js
// FM index changes over time
note("c3 eb3 g3 bb3")
  .fm("<1 2 4 8>")
  .fmh("<1 2 3>")
  ._scope()
```

> 💡 **Exercise:**
> 1. Start with `note("c3").fm(1)._scope()`
> 2. Gradually increase `fm` to 2, 4, 8, 16 — listen to how harmonics build up
> 3. Try `note("c3").fm(8).fmh(1.5)._scope()` for a bell sound
> 4. Add `.fmdecay(0.2).fmsustain(0)` to make it percussive

---

## 3.7 Wavetable Synthesis

Wavetable synthesis uses a **single cycle waveform** (a tiny recorded shape) and loops it to produce a tone. Strudel ships with **over 1000 wavetables** from the AKWF (Adventure Kid Waveforms) set.

### How to Use Wavetables

Any sample name starting with `wt_` is treated as a wavetable (auto-looped):

```js
// Flute wavetable
note("c3 e3 g3 c4").s("wt_flute")._scope()
```

### Exploring Wavetables

The AKWF set includes many categories. Some you can try:

```js
note("c3").s("wt_flute")._scope()
note("c3").s("wt_piano")._scope()
note("c3").s("wt_violin")._scope()
note("c3").s("wt_saw")._scope()
note("c3").s("wt_square")._scope()
```

### Scanning Through Wavetables

Use `.n()` to scroll through different wavetable variations within a set:

```js
note("<[g3,b3,e4]!2 [a3,c3,e4] [b3,d3,f#4]>")
  .n("<1 2 3 4 5 6 7 8 9 10>/2")
  .s("wt_flute")
  .room(0.5).size(0.9)
  .release(0.125)
  .decay("<0.1 0.25 0.3 0.4>").sustain(0)
  .cutoff("<1000 2000 4000>")
  .fast(4)
  ._scope()
```

### When to Use Wavetables

- When basic waveforms (sine/saw/square/tri) don't give you the timbre you want
- For **organic, evolving** pad sounds
- For emulating acoustic instruments (loosely)
- For unique textures not possible with standard oscillators

> 💡 **Exercise:** Try playing the same melody with different wavetables. Start with `note("c3 eb3 g3 c4").s("wt_flute")`, then change `wt_flute` to `wt_piano`, `wt_violin`, `wt_saw`, etc.

---

## 3.8 Practical Recipes for Techno

Now let's apply what we've learned to build some basic techno sounds using only oscillators (no samples yet):

### Recipe: Sub Bass (Sine)

```js
setcpm(128/4)
note("c1 c1 <eb1 f1> c1").s("sine")
```

### Recipe: Buzzy Bass (Sawtooth)

```js
setcpm(128/4)
note("c2 c2 <eb2 f2> c2").s("sawtooth")
```

### Recipe: Noise Hi-Hats

```js
setcpm(128/4)
s("white*16").decay(.03).sustain(0)
```

### Recipe: FM Bell Hit

```js
setcpm(128/4)
note("~ c5 ~ <c5 g4>").s("sine")
  .fm(6).fmh(3.5)
  .fmdecay(0.2).fmsustain(0)
  .decay(0.5).sustain(0)
  .room(0.3)
```

### Recipe: Basic Kick (Sine + Pitch Envelope)

```js
setcpm(128/4)
note("c1!4").s("sine")
  .penv(24).pdecay(.06)
  .decay(.4).sustain(0)
```

> We'll cover `penv` (pitch envelope) fully in Module 6, but even now you can use this recipe. The idea: the pitch starts high and drops quickly to the fundamental, creating the "punch" of a kick.

### Recipe: Combining Everything

```js
setcpm(128/4)

// Kick
$: note("c1!4").s("sine")
  .penv(24).pdecay(.06)
  .decay(.4).sustain(0)

// Noise hats
$: s("white*16").decay(.03).sustain(0).gain(0.5)

// Buzzy bass
$: note("c2 ~ <eb2 f2> ~").s("sawtooth")

// FM bell
$: note("~ c5 ~ <c5 g4>").s("sine")
  .fm(6).fmh(3.5).fmdecay(0.15).fmsustain(0)
  .decay(0.4).sustain(0).gain(0.3)
  .room(0.3)
```

> 💡 **Exercise:** Paste this into Strudel and modify it:
> 1. Change the bass notes
> 2. Change the FM bell's `fmh` value
> 3. Add `._scope()` to any line to see the waveform
> 4. Try swapping `"sawtooth"` for `"square"` on the bass

---

## 3.9 Practice Challenges

### Challenge 1: Waveform ID ⭐
Play each waveform one at a time (sine, sawtooth, square, triangle) on the same note. Close your eyes between changes and try to identify which is which by ear alone.

### Challenge 2: Noise Drum Kit ⭐⭐
Build a complete drum loop using ONLY noise (no samples). You'll need:
- Kick: use `note("c1").s("sine")` with short decay (ok, one oscillator)
- Hi-hat: `white` noise with very short decay
- Snare: `pink` noise with medium decay

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
$: note("c1!4").s("sine").penv(24).pdecay(.06).decay(.4).sustain(0)
$: s("white*16").decay(.03).sustain(0).gain(.4)
$: s("~ pink ~ pink").decay(.1).sustain(0).gain(.5)
```
</details>

### Challenge 3: FM Exploration
Create a pattern that sweeps through FM modulation amounts over 4 cycles:

<details>
<summary>Solution</summary>

```js
note("c3 eb3 g3 bb3")
  .fm("<1 3 6 12>")
  .fmh(2)
  .fmdecay(0.3).fmsustain(0)
  ._scope()
```
</details>

### Challenge 4: Multi-Oscillator Stack
Layer three oscillators playing the same bass note but with different waveforms for a thick sound:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
stack(
  note("c2").s("sawtooth").gain(0.4),
  note("c2").s("square").gain(0.3),
  note("c1").s("sine").gain(0.5)
)
```
</details>

---

## 3.10 Key Takeaways

1. **Sine** = pure, no harmonics → sub-bass, kick fundamentals
2. **Sawtooth** = all harmonics → bass, pads, leads (filter it in Module 4!)
3. **Square** = odd harmonics, hollow → stabs, retro sounds
4. **Triangle** = soft odd harmonics → gentle leads, default sound
5. **Noise** (white/pink/brown) = percussion, texture, atmosphere
6. **FM Synthesis** = metallic, bell-like, percussive timbres
7. **Wavetables** = 1000+ unique waveform shapes for organic textures
8. Always use `._scope()` to **see** what you hear

---

**Previous:** [← Module 2 — Cycles, Tempo & Mini-Notation](./Module_02_Cycles_Tempo_MiniNotation.md)
**Next:** [Module 4 — Subtractive Synthesis →](./Module_04_Subtractive_Synthesis.md)
