# Module 7 — Building Core Sounds

> **Goal:** Apply everything from Modules 3–6 to design the essential sound elements of a Minimal Techno track from scratch: **kick**, **hi-hat**, **clap/snare**, **bass**, **pad**, and **lead/stab**. Each sound is built step-by-step so you understand every decision.

---

## 7.1 The Minimal Techno Sound Palette

A typical Minimal Techno track uses a small, carefully designed set of sounds:

| Element | Role | Character |
|---------|------|-----------|
| **Kick** | Foundation, pulse | Deep, punchy, defined |
| **Hi-Hat** | Groove, movement | Crisp, 16ths, accented |
| **Clap / Snare** | Backbeat | Snappy, with space |
| **Bass** | Harmonic foundation | Dark, filtered, rhythmic |
| **Pad** | Atmosphere, harmony | Warm, wide, evolving |
| **Lead / Stab** | Melodic hook | Sparse, delayed, filtered |
| **Percussion** | Texture, polyrhythm | Subtle, panned, varied |

Let's build each one.

---

## 7.2 Kick Drum Design

The kick is the most important sound in techno. You need to hear it clearly on every system — headphones, PA, laptop speakers.

### Approach A: Sample-Based (Quick)

```js
setcpm(128/4)
s("bd*4").bank("RolandTR909")
```

Pros: Instant, proven, battle-tested.
Cons: Less control, everyone uses it.

### Approach B: Synthesized (Full Control)

**Step 1 — Start with a sine wave at a low frequency:**
```js
note("c1").s("sine")._scope()
```

This is the "body" of the kick — a low, pure tone.

**Step 2 — Add a pitch envelope for the "click":**
```js
note("c1").s("sine")
  .penv(24).pdecay(.06)
  ._scope()
```

The pitch starts 24 semitones above C1, drops to C1 in 60ms. This creates the transient "click" that gives the kick definition.

**Step 3 — Shape the amplitude:**
```js
note("c1").s("sine")
  .penv(24).pdecay(.06)
  .decay(.4).sustain(0)
  ._scope()
```

The body sustains for 400ms then fades.

**Step 4 — Add saturation for warmth:**
```js
note("c1!4").s("sine")
  .penv(24).pdecay(.06)
  .decay(.4).sustain(0)
  .distort("1.5:.6")
```

Mild distortion adds harmonics and makes the kick audible on smaller speakers.

### Approach C: Layered (Best of Both)

Combine a synthesized sub body with a sample transient:

```js
setcpm(128/4)
stack(
  // Layer 1: Sine sub body
  note("c1!4").s("sine")
    .penv(18).pdecay(.08)
    .decay(.5).sustain(0)
    .gain(0.7),
  // Layer 2: Sample click/transient
  s("bd:4*4").bank("RolandTR909")
    .hpf(200)                    // remove sample's low end
    .decay(.05).sustain(0)       // keep only the click
    .gain(0.4)
)
```

### Kick Variations Quick Reference

| Style | penv | pdecay | decay | Extra |
|-------|------|--------|-------|-------|
| Tight, punchy | 36 | .04 | .25 | — |
| Deep, round | 18 | .08 | .5 | — |
| Hard, distorted | 36 | .05 | .35 | `.distort("3:.5")` |
| Sub-heavy | 12 | .1 | .6 | — |
| Clicky | 48 | .03 | .2 | `.distort("1:.5")` |

> 💡 **Exercise:** Build three kick variations and A/B them. Decide which one you prefer for your Minimal Techno track.

---

## 7.3 Hi-Hat Design

Hi-hats provide the groove and movement. In Minimal Techno, 16th-note hi-hats with accent patterns are standard.

### Approach A: Sample-Based

```js
setcpm(128/4)
// 16th notes with accent pattern
s("hh*16").bank("RolandTR909")
  .gain(".6 .3 .8 .3 .6 .3 1 .3 .6 .3 .8 .3 .6 .3 1 .3")
```

### Approach B: Synthesized (Noise)

```js
setcpm(128/4)
s("white*16")
  .hpf(8000)                    // only high frequencies
  .decay(.03).sustain(0)         // very short = closed hat
  .gain(".6 .3 .8 .3")
```

### Adding Swing

Swing shifts every other 16th note slightly late, creating a looser groove:

```js
setcpm(128/4)
s("hh*16").bank("RolandTR909")
  .gain(".6 .3 .8 .3")
  .swingBy(1/6, 4)              // subtle shuffle
```

### Open Hi-Hat + Cut Group

```js
setcpm(128/4)
s("[hh hh hh <hh oh>]*4").bank("RolandTR909")
  .cut(1)                        // open hat gets cut by closed hat
  .gain(".6 .3 .8 .5")
```

### Adding Character

```js
setcpm(128/4)
s("hh*16").bank("RolandTR909")
  .gain(".6 .3 .8 .3")
  .hpf(6000)                    // remove low rumble
  .sometimes(x => x.speed(1.5)) // occasional pitch variation
  .delay(0.05).delaytime(0.03)  // tiny stereo spread
```

> 💡 **Exercise:** Create a hi-hat pattern that:
> 1. Has 16th notes
> 2. Has an accent pattern (louder on certain beats)
> 3. Includes an open hi-hat every 4th beat with cut group
> 4. Uses `.swingBy()` for groove

---

## 7.4 Clap / Snare Design

The clap or snare sits on beats 2 and 4 (the "backbeat") in most techno.

### Sample-Based Clap

```js
setcpm(128/4)
s("~ cp ~ cp").bank("RolandTR909")
  .room(0.15).roomsize(0.3)     // short reverb = space
  .gain(0.7)
```

### Synthesized Snare (Noise + Tone)

```js
setcpm(128/4)
stack(
  // Noise body
  s("~ pink ~ pink")
    .bpf(2000).bpq(2)
    .decay(.12).sustain(0)
    .gain(0.5),
  // Tonal ping
  note("~ e3 ~ e3").s("triangle")
    .decay(0.05).sustain(0)
    .gain(0.2)
).room(0.1)
```

### Adding Variation

```js
setcpm(128/4)
s("~ <cp sd> ~ <cp [cp cp]>").bank("RolandTR909")
  .room(0.1)
  .n("<0 1 2>")                 // rotate through variations
```

---

## 7.5 Bass Sound Design

The bass carries the harmonic foundation. In Minimal Techno it's typically dark, filtered, and rhythmic.

### Style A: Sub Bass (Sine)

The simplest bass — a pure sine wave. Felt more than heard:

```js
setcpm(128/4)
note("<c1 c1 eb1 f1>").s("sine")
  .decay(0.3).sustain(0.6).release(0.1)
  .lpf(200)
```

### Style B: Warm Bass (Triangle + Gentle Filter)

```js
setcpm(128/4)
note("c2 ~ <eb2 f2> ~").s("triangle")
  .lpf(600).lpq(3)
  .decay(0.25).sustain(0.3).release(0.1)
```

### Style C: Acid Bass (Saw + Ladder Filter + Envelope)

The classic "acid" sound — a sawtooth through a resonant, enveloped low-pass filter:

```js
setcpm(128/4)
note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
  .lpf(400).lpq(15)
  .lpenv(5).lpa(0.005).lpd(0.12).lps(0)
  .ftype("ladder")
  .decay(0.2).sustain(0).release(0.05)
```

**Why it works:**
- `sawtooth` = harmonically rich raw material
- `lpf(400)` = dark base
- `lpq(15)` = resonant "squelch"
- `lpenv(5)` = filter opens on each note
- `ftype("ladder")` = aggressive analog character

### Style D: FM Bass

```js
setcpm(128/4)
note("c2 ~ <eb2 f2> ~").s("sine")
  .fm(2).fmh(1)
  .fmdecay(0.2).fmsustain(0)
  .decay(0.3).sustain(0)
  .lpf(1000)
```

### Style E: Reese Bass (Detuned Saws)

Two slightly detuned sawtooths create a thick, moving bass:

```js
setcpm(128/4)
note("c2 ~ <eb2 f2> ~").s("sawtooth")
  .lpf(800).lpq(3)
  .decay(0.3).sustain(0.4)
  // Thicken with delay-based chorus
  .delay(0.1).delaytime(0.003).delayfeedback(0)
```

### Bass Pattern Ideas for Minimal Techno

```js
// Sparse — one note per bar
note("<c2 ~ ~ ~>")

// Syncopated
note("c2 ~ [~ c2] ~")

// Rolling 8th
note("[c2 c2 <eb2 f2> c2]*2")

// Euclidean
note("c2").euclid(3,8)

// Pulsing (same note, rhythmic filter)
note("c2!8").s("sawtooth")
  .lpf("<200 600 400 800 200 600 400 1200>")
  .lpq(8).ftype("ladder")
  .decay(0.15).sustain(0)
```

> 💡 **Exercise:** Build two different bass sounds — one sub (sine) and one acid (saw + filter envelope). Play the same note pattern with each and compare.

---

## 7.6 Pad Sound Design

Pads provide atmosphere and harmonic context. In Minimal Techno, they're subtle, filtered, and often sidechained.

### Style A: Dark Saw Pad

```js
setcpm(128/4)
note("<[c4,eb4,g4] [bb3,d4,f4] [ab3,c4,eb4] [g3,bb3,d4]>")
  .s("sawtooth")
  .lpf(1500).lpq(2)
  .attack(0.5).decay(0.8).sustain(0.6).release(1.5)
  .room(0.5).roomsize(0.7)
  .gain(0.25)
```

### Style B: Evolving Filtered Pad

```js
setcpm(128/4)
note("<[c4,eb4,g4] [bb3,d4,f4]>")
  .s("sawtooth")
  .lpf(sine.range(800, 3000).slow(16))  // filter breathes
  .lpq(3)
  .attack(0.4).release(1)
  .room(0.6).roomsize(0.8)
  .gain(0.2)
```

### Style C: Wavetable Pad

```js
setcpm(128/4)
note("<[c4,eb4,g4]!2 [bb3,d4,f4] [ab3,c4,eb4]>")
  .s("wt_flute")
  .attack(0.3).release(0.5)
  .room(0.5).roomsize(0.9)
  .lpf(3000)
  .gain(0.2)
```

### Style D: Noise-Textured Pad

```js
setcpm(128/4)
note("<[c4,eb4,g4] [bb3,d4,f4]>")
  .s("sawtooth")
  .noise(0.15)                           // add subtle noise
  .lpf(2000).lpq(1)
  .attack(0.5).release(1.5)
  .room(0.7).roomsize(0.85)
  .gain(0.2)
```

### Style E: Chord Stab (Short Pad)

```js
setcpm(128/4)
note("<[c3,eb3,g3] [bb2,d3,f3]>")
  .s("sawtooth")
  .struct("[~ x]*2")                     // rhythmic hits
  .lpf(2000).lpq(5)
  .decay(0.2).sustain(0).release(0.1)
  .room(0.2)
  .gain(0.3)
```

> 💡 **Exercise:** Take Style A and modify it:
> 1. Add filter movement with `.lpf(sine.range(500,3000).slow(8))`
> 2. Change `sawtooth` to `square` — hear the difference
> 3. Add `.delay(0.3).delaytime(0.375)` for rhythmic echo

---

## 7.7 Lead / Stab Sound Design

Leads and stabs provide the melodic hook. In Minimal Techno, they're typically sparse — a few notes with lots of space, often processed with delay.

### Style A: Triangle Lead

```js
setcpm(128/4)
note("<c4 ~ eb4 ~ g4 ~ c5 ~>")
  .s("triangle")
  .lpf(4000)
  .decay(0.15).sustain(0).release(0.1)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .gain(0.3)
```

### Style B: Filtered Square Stab

```js
setcpm(128/4)
note("[c4 eb4 g4 c5]*2")
  .s("square")
  .lpf(3000).lpq(5)
  .lpenv(3).lpd(0.1).lps(0)
  .decay(0.15).sustain(0).release(0.05)
  .delay(0.3).delaytime(0.1875).delayfeedback(0.4)
  .gain(0.3)
```

### Style C: FM Bell

```js
setcpm(128/4)
note("~ c5 ~ <g4 eb5>")
  .s("sine")
  .fm(6).fmh(3.5)
  .fmdecay(0.2).fmsustain(0)
  .decay(0.5).sustain(0)
  .room(0.3)
  .gain(0.25)
```

### Style D: Sparse Random Melody

```js
setcpm(128/4)
n(irand(8).segment(4))
  .scale("C4:minor:pentatonic")
  .s("triangle")
  .lpf(3000)
  .decay(0.1).sustain(0)
  .delay(0.5).delaytime(0.1875).delayfeedback(0.6)
  .degradeBy(0.3)              // randomly drop notes
  .gain(0.25)
```

### Style E: Delayed Pluck with Randomization

```js
setcpm(128/4)
n("<0 3 5 7>*2")
  .scale("C4:minor:pentatonic")
  .s("triangle")
  .decay(0.1).sustain(0)
  .delay(0.5).delaytime(0.1875).delayfeedback(0.5)
  .pan(sine.range(0.3, 0.7).slow(4))
  .sometimes(x => x.add(note(12)))      // octave up sometimes
  .gain(0.25)
```

---

## 7.8 Percussion & Texture

Additional layers that add depth without dominating:

### Rimshot Pattern (Euclidean)

```js
setcpm(128/4)
s("rim(5,16)").bank("RolandTR909")
  .gain(0.3)
  .pan(rand)
  .delay(0.2).delaytime(0.1875)
```

### Vinyl Crackle

```js
setcpm(128/4)
s("crackle*2").density(0.03).gain(0.1)
```

### Ambient Noise Wash

```js
setcpm(128/4)
s("brown").begin(rand).end(rand.add(0.01))
  .hpf(2000).gain(0.04)
  .room(0.8).roomsize(0.9)
```

### Shaker / Maracas

```js
setcpm(128/4)
s("white*8")
  .hpf(10000).bpf(12000).bpq(5)
  .decay(0.02).sustain(0)
  .gain(".2 .1 .15 .1")
```

---

## 7.9 Complete Sound Palette — All Together

Here's every element we built, playing together:

```js
setcpm(128/4)

// KICK — synthesized
$: note("c1!4").s("sine")
  .penv(24).pdecay(.06)
  .decay(.4).sustain(0)
  .distort("1.5:.6")
  .gain(0.8)

// HI-HATS — 909 with accents + swing
$: s("hh*16").bank("RolandTR909")
  .gain(".5 .3 .8 .3 .5 .3 1 .3 .5 .3 .8 .3 .5 .3 1 .3")
  .hpf(6000).swingBy(1/6, 4)

// CLAP — backbeat with reverb
$: s("~ cp ~ cp").bank("RolandTR909")
  .room(0.15).gain(0.6)

// PERCUSSION — Euclidean rim
$: s("rim(5,16)").bank("RolandTR909")
  .gain(0.3).pan(rand)
  .delay(0.2).delaytime(0.1875)

// BASS — acid saw
$: note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
  .lpf(400).lpq(12)
  .lpenv(4).lpa(0.005).lpd(0.12).lps(0)
  .ftype("ladder")
  .decay(0.2).sustain(0).release(0.05)
  .gain(0.55)

// PAD — dark saw with filter movement
$: note("<[c4,eb4,g4] [bb3,d4,f4] [ab3,c4,eb4] [g3,bb3,d4]>")
  .s("sawtooth")
  .lpf(sine.range(1000, 2500).slow(16)).lpq(2)
  .attack(0.4).sustain(0.5).release(1)
  .room(0.5).roomsize(0.7)
  .gain(0.2)
  .orbit(2)

// SIDECHAIN — duck the pad
$: s("bd*4").bank("RolandTR909")
  .duckorbit(2).duckattack(0.2).duckdepth(1)
  .gain(0)

// LEAD — sparse triangle with delay
$: n("<0 ~ 3 ~ 5 ~ 7 ~>*2")
  .scale("C4:minor:pentatonic")
  .s("triangle").lpf(3000)
  .decay(0.15).sustain(0)
  .delay(0.4).delaytime(0.1875).delayfeedback(0.5)
  .degradeBy(0.3).gain(0.25)

// TEXTURE — crackle
$: s("crackle*2").density(0.03).gain(0.1)
```

> 💡 **Exercise:** Paste this entire block into Strudel. Then:
> 1. Comment out everything except the kick — build up from there
> 2. Uncomment hi-hats, then clap, then bass, then pad...
> 3. Modify the bass notes, pad chords, and lead melody
> 4. This is how you'll build a track in Module 11!

---

## 7.10 Practice Challenges

### Challenge 1: Your Own Kick
Design a kick that sounds different from the ones above. Experiment with:
- Different `penv` and `pdecay` values
- Using `triangle` instead of `sine`
- Different amounts or types of distortion

### Challenge 2: Mood Change
Take the complete palette above and change the mood from dark (C minor) to bright by:
- Changing bass notes to major scale tones
- Changing pad chords to major chords
- Changing the lead scale to `"C4:major:pentatonic"`

### Challenge 3: Design a Sound You've Never Heard
Combine techniques in unexpected ways:
- FM synthesis + filter envelope?
- Wavetable + pitch envelope?
- Noise + vowel filter?

---

## 7.11 Key Takeaways

1. **Kick** = `sine` + `penv`/`pdecay` + short `decay` + optional distortion
2. **Hi-hat** = noise or sample + HPF + very short envelope + accent pattern
3. **Clap** = sample or noise + BPF + reverb
4. **Bass** = sawtooth + LPF + filter envelope + ladder filter type
5. **Pad** = sawtooth/wavetable + slow attack + LPF + reverb
6. **Lead** = triangle/square + short decay + delay feedback
7. **Percussion** = Euclidean rhythms + panning + delay
8. **Texture** = crackle, noise wash, filtered noise
9. Layer synthesized + sampled elements for the best results
10. The **sidechain duck** effect (Module 9) makes pads pump with the kick

---

**Previous:** [← Module 6 — Envelopes & Dynamics](./Module_06_Envelopes_Dynamics.md)
**Next:** [Module 8 — Writing Chords & Melodies →](./Module_08_Chords_Melodies.md)
