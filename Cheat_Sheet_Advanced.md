# 🌬️ Breath of Strudel — Advanced Cheat Sheet

> **Tough-to-remember techniques that separate a good set from a great one.**
> You already know the basics. This is the stuff you'll keep forgetting until you don't.

---

## 🔧 Filter Envelopes — The Secret Sauce

### Low-Pass Filter Envelope

| Parameter | Alias | What It Controls | Typical |
|-----------|-------|-----------------|---------|
| `.lpf(300)` | `.cutoff()` | Base cutoff frequency | 200–600 for bass |
| `.lpenv(4)` | `.lpe()` | Sweep depth (semitones above base) | 2–8 |
| `.lpa(0.005)` | `.lpattack()` | Time to reach peak cutoff | 0.001–0.01 |
| `.lpd(0.12)` | `.lpdecay()` | Fall time from peak → sustain | 0.05–0.5 |
| `.lps(0)` | `.lpsustain()` | Held cutoff level | 0–0.5 |
| `.lpr(0.1)` | `.lprelease()` | Cutoff fade after note off | 0–0.5 |

**The acid bass formula:**
```js
.lpf(400).lpq(12).lpenv(4).lpa(.005).lpd(.12).lps(0).ftype("ladder")
```

### HPF & BPF Envelopes — Same System

| Filter | Prefix | Example |
|--------|--------|---------|
| High-pass | `hp` | `.hpf(100).hpenv(4).hpa(.01).hpd(.3).hps(0)` |
| Band-pass | `bp` | `.bpf(1000).bpenv(4).bpa(.01).bpd(.5).bps(.3)` |

---

## 🎛️ FM Synthesis Parameters

| Parameter | What It Does | Sweet Spots |
|-----------|-------------|-------------|
| `.fm(N)` | Modulation index (harmonics) | 1–3 warm, 6–8 metallic, 12+ extreme |
| `.fmh(N)` | Harmonicity ratio | Integer = harmonic, non-integer = bell/inharmonic |
| `.fmdecay(t)` | FM envelope decay | 0.05–0.3 |
| `.fmsustain(N)` | FM sustain level | 0 = percussive, 0.5 = sustained |
| `.fmattack(t)` | FM attack time | 0 = instant |
| `.fmenv(N)` | FM envelope depth | Higher = more dramatic |

**FM bell percussion:**
```js
note("c5").fm(6).fmh(3.5).fmdecay(.2).fmsustain(0).decay(.5).sustain(0)
```

**Inharmonic / bell:** use non-integer `.fmh()` values: `1.41`, `2.76`, `3.14`

---

## ⚡ Sidechain Ducking — Full Setup

```js
// 1. PAD on orbit 2
$: note("<[c4,eb4,g4] [bb3,d4,f4]>").s("sawtooth")
  .lpf(2000).attack(.3).release(1).room(.4).gain(0.25).orbit(2)

// 2. AUDIBLE KICK (orbit 0, default)
$: s("bd*4").bank("RolandTR909").gain(0.85)

// 3. SILENT DUCK TRIGGER
$: s("bd*4").bank("RolandTR909")
  .duckorbit(2).duckattack(0.2).duckdepth(1).gain(0)
```

| Parameter | What It Does | Range |
|-----------|-------------|-------|
| `.duckorbit(N)` | Which orbit to duck | Must match pad's `.orbit(N)` |
| `.duckattack(t)` | Recovery time (seconds) | 0.1 = snappy, **0.2 = standard**, 0.35 = deep |
| `.duckdepth(d)` | Duck amount | 0.6 = subtle, **1 = full** |

### Common Sidechain Mistakes

| Mistake | Symptom | Fix |
|---------|---------|-----|
| Missing `.orbit(2)` on pad | No pump | Add `.orbit(2)` |
| No `.gain(0)` on trigger | Extra kick sound | Trigger must be silent |
| Only trigger, no real kick | No audible kick | You need TWO kick patterns |
| Pad & kick on orbit 0 | Kick gets ducked too | Put pad on orbit 2+ |

---

## 🔄 Pattern Manipulation — Time Modifiers

| Function | What It Does | Use For |
|----------|-------------|---------|
| `.slow(N)` | Stretch over N cycles | Long progressions, macro changes |
| `.fast(N)` | Speed up by factor N | Fills, buildups |
| `.rev()` | Reverse order | Fills, variation |
| `.palindrome()` | Forward then backward | Subtle loop variation |
| `.iter(N)` | Rotate start by 1 each cycle | **Minimal techno secret weapon** |
| `.ply(N)` | Repeat each event N times | 32nd-note fills |
| `.early(t)` / `.late(t)` | Nudge timing by fraction | Humanize, groove |
| `.segment(N)` | Quantize signal to N steps | Convert continuous → discrete |

---

## 🎲 Random & Conditional Modifiers

### Probability Functions (per event)

| Function | Chance | Example |
|----------|--------|---------|
| `.almostAlways(fn)` | ~90% | `.almostAlways(x => x.speed(1.5))` |
| `.often(fn)` | ~75% | `.often(x => x.crush(4))` |
| `.sometimes(fn)` | ~50% | `.sometimes(x => x.add(note(12)))` |
| `.rarely(fn)` | ~25% | `.rarely(x => x.rev())` |
| `.almostNever(fn)` | ~10% | `.almostNever(x => x.distort(3))` |

### Conditional (per cycle)

| Function | What It Does | Example |
|----------|-------------|---------|
| `.lastOf(N, fn)` | Apply on last cycle of N | `.lastOf(4, x => x.ply(2))` |
| `.firstOf(N, fn)` | Apply on first cycle of N | `.firstOf(3, x => x.fast(2))` |
| `.every(N, fn)` | Alias for `lastOf` | `.every(4, x => x.rev())` |
| `.when(pat, fn)` | Apply when pattern = 1 | `.when("<1 0 0 1>", x => x.speed(2))` |
| `.chunk(N, fn)` | Rotate fn through N chunks | `.chunk(4, x => x.add(7))` |

### Degradation

| Code | What It Does |
|------|-------------|
| `.degrade()` | Remove ~50% of events randomly |
| `.degradeBy(0.3)` | Remove 30% of events |
| `chooseCycles(a, b, c)` | Pick one pattern per cycle randomly |

---

## 📡 Continuous Signals for Modulation

| Signal | Shape | Character |
|--------|-------|-----------|
| `sine` | Smooth oscillation | Gentle, predictable |
| `cosine` | Offset sine | Phase-shifted |
| `saw` | Ramp up, snap down | Buildups |
| `tri` | Linear up-down | Triangle LFO |
| `square` | Binary switching | Gate effect |
| `rand` | New random per event | Per-event chaos |
| `irand(N)` | Random integer 0 to N–1 | Scale degrees |
| `perlin` | Smooth organic random | Natural movement |

### How to Use

```js
signal.range(min, max).slow(N)
```

### Common Modulation Targets (Copy & Tweak)

| Target | Code |
|--------|------|
| Filter sweep | `.lpf(sine.range(200, 4000).slow(8))` |
| Resonance swell | `.lpq(sine.range(0, 15).slow(16))` |
| Volume ramp | `.gain(saw.range(0.2, 1).slow(4))` |
| Auto-pan | `.pan(sine.range(0.3, 0.7).slow(4))` |
| Delay build | `.delay(saw.range(0, 0.6).slow(8))` |
| Reverb grow | `.room(sine.range(0, 0.8).slow(16))` |
| FM modulation | `.fm(sine.range(0, 8).slow(8))` |
| Pattern thinning | `.degradeBy(saw.range(0, 0.9).slow(16))` |
| Random filter per hit | `.lpf(rand.range(2000, 10000))` |
| Generative melody | `n(irand(8).segment(8)).scale("C4:minor:pentatonic")` |
| Perlin melody | `n(perlin.segment(8).range(0, 12)).scale("C3:minor:pentatonic")` |
| Sine melody | `n(sine.segment(16).range(0, 7)).scale("C4:minor")` |

---

## 🎵 Chord Voicing System

### Using `.voicing()`

```js
chord("<Cm7 Fm Abmaj7 G7>").voicing().s("sawtooth").lpf(2000)
```

| Modifier | What It Does |
|----------|-------------|
| `.voicing()` | Auto-spread + voice leading |
| `.anchor("c4")` | Center voicings around C4 |
| `.mode("below")` | All notes below anchor |
| `.dict("dictName")` | Use custom voicing dictionary |

### Custom Voicing Dictionaries

```js
addVoicings('techno', {
  'm':    ['3 7 12', '7 12 15'],
  'maj':  ['4 7 12', '7 12 16'],
  '7':    ['4 7 10', '7 10 16'],
  'm7':   ['3 7 10', '7 10 15'],
})
chord("<Cm7 Fm>").dict('techno').anchor(60).voicing()
```

Numbers = **semitone intervals**: 3=min 3rd, 4=maj 3rd, 7=5th, 10=min 7th, 12=octave

### Transposition

| Function | What It Does | Example |
|----------|-------------|---------|
| `.transpose(N)` | Shift N semitones | `.transpose(2)` = up whole step |
| `.scaleTranspose(N)` | Shift N scale degrees | `.scaleTranspose(1)` = up one degree |
| `.add(note(12))` | Add octave | Used with `.sometimes()` |

---

## 🔊 Complete Effects Reference

| Effect | Primary | Parameters | Sweet Spot |
|--------|---------|-----------|------------|
| **Delay** | `.delay(0.5)` | `.delaytime(0.1875)` `.delayfeedback(0.5)` | `"0.5:0.1875:0.5"` |
| **Reverb** | `.room(0.4)` | `.roomsize(0.7)` `.roomlp(3000)` `.roomdim(4000)` | Tight: `.room(.15).roomsize(.3)` |
| **Distortion** | `.distort("1.5:.6")` | `amount:asymmetry` | Kick: `"1.5:.6"`, Gritty: `"3:.5"` |
| **Bit Crush** | `.crush(8)` | 16=clean, 1=destroyed | 8 = noticeable crunch |
| **Coarse** | `.coarse(8)` | 1=clean, higher=more | Digital aliasing |
| **Phaser** | `.phaser(0.5)` | `.phaserdepth(.8)` `.phasercenter(1000)` | Pads: `.phaser(.3).phaserdepth(.6)` |
| **Tremolo** | `.tremolo("4:0.5")` | `speed:depth` | Choppy: `"8:1"` |
| **Compressor** | `.compressor("-20:20:10:.002:.02")` | `threshold:ratio:knee:attack:release` | — |
| **Postgain** | `.postgain(1.5)` | After all effects | Boost after compression |

---

## 🌐 Orbits — Independent Effect Buses

| Orbit | Typical Use | Settings |
|-------|-------------|----------|
| 0 (default) | Kick, hats, bass | Dry or minimal fx |
| 1 | Clap / percussion | Short reverb |
| 2 | Pad / melody (sidechain target) | Big reverb + delay |

> **Rule:** Low-end in center (orbit 0, no pan). Pads & melodies on orbit 2 for sidechain pump.

---

## 📊 Stereo & Panning

| Technique | Code | Use For |
|-----------|------|---------|
| Static pan | `.pan(0.3)` | Place element L/R |
| Random pan | `.pan(rand)` | Percussion texture |
| Auto-pan (sine) | `.pan(sine.range(.3,.7).slow(4))` | Moving elements |
| Stereo width | `.jux(rev)` | Right channel reversed |
| Partial width | `.juxBy(0.3, rev)` | Subtle stereo |

> **Keep centered:** Kick, bass, clap. **Pan freely:** Rimshots, percs, hats, leads.

---

## 🧬 Layering Constructs

| Function | What It Does | Use |
|----------|-------------|-----|
| `stack(a, b, c)` | All play at once | Combining elements |
| `cat(a, b, c)` | One per cycle, rotate | Sequential build |
| `seq(a, b)` | Same as `cat` | Alias |
| `arrange([N, pat], ...)` | N cycles of each pattern | Scripted arrangements |
| `.superimpose(fn)` | Original + modified copy | Instant thickness |
| `.off(t, fn)` | Time-shifted modified copy | Canon, echo, doubling |

### `superimpose` & `off` — Power Moves

```js
// Octave-doubled melody
note("c3 eb3 g3").superimpose(x => x.add(note(12)))

// Canon effect — copy plays 1/8 later, octave up
note("c3 eb3 g3").off(1/8, x => x.add(note(12)).gain(0.5))

// Stacked canons
note("0 2 4 6").scale("C3:minor").s("triangle")
  .off(1/4, x => x.add(note(7)))
  .off(1/8, x => x.add(note(12)).gain(0.3))
```

---

## 🏗️ Arrangement Structure — Scripted Track

### Standard Minimal Techno Form

| Section | Bars | Energy | Technique |
|---------|------|--------|-----------|
| **Intro** | 16–32 | Low | Kick only → add hats |
| **Build** | 8–16 | Rising | Layer elements, filter sweeps |
| **Main** | 32–64 | Full | Full groove |
| **Breakdown** | 8–16 | Drops | Strip elements, reverb wash |
| **Drop** | 32–64 | Peak | Full energy return |
| **Outro** | 16–32 | Falling | Fade to kick only |

**At 128 BPM: 1 cycle = 1 bar = 4 beats. 128 bars ≈ 4 minutes.**

### Transition Techniques (ready to paste)

| Technique | Code |
|-----------|------|
| HPF buildup | `.hpf(saw.range(50, 5000).slow(8))` |
| LPF darken | `.lpf(saw.range(8000, 200).slow(8))` |
| LPF brighten | `.lpf(saw.range(200, 8000).slow(8))` |
| Reverb wash | `.room(saw.range(0, .9).slow(16))` |
| Pattern thin | `.degradeBy(saw.range(0, .8).slow(8))` |
| Fade out | `.gain(saw.range(.8, 0).slow(16))` |
| Fade in | `.gain(saw.range(0, .8).slow(16))` |
| Snare roll | `s("sd").fast(saw.range(1,16).slow(4)).gain(saw.range(0,.5).slow(4))` |
| Pitch riser | `note(saw.range(40,72).segment(32).slow(8)).s("sine").gain(0.15)` |

---

## 🔌 MIDI & I/O

| Function | What It Does |
|----------|-------------|
| `.midi()` | Send to default MIDI device |
| `.midi("Device Name")` | Send to specific device |
| `.midichan(2)` | MIDI channel |
| `ccn(74).ccv(sine.segment(16).range(0,127)).midi()` | MIDI CC |
| `midi_clock().midi()` | Sync clock to hardware |
| `.osc()` | Send OSC messages |
| `audioin()` | Process live mic/line input |

---

## 🎯 Sample Manipulation

| Function | What It Does |
|----------|-------------|
| `.begin(0.25)` | Start from 25% through sample |
| `.end(0.5)` | End at 50% of sample |
| `.speed(2)` | Double speed / octave up |
| `.speed(-1)` | Play reversed |
| `.clip(1)` | Fit to time slot |
| `.loopAt(2)` | Fit long sample into 2 cycles |
| `.fit()` | Auto-fit to event duration |
| `.chop(N)` | Cut into N slices, play in order |
| `.slice(N, "pat")` | Cut into N slices, resequence |
| `.splice(N, "pat")` | Slice + auto-stretch |
| `.striate(N)` | Progressive granular slicing |

```js
// Breakbeat slice and resequence
samples('github:tidalcycles/dirt-samples')
s("breaks165").slice(8, "0 1 <2 2*2> 3 [4 0] 5 6 7")
```

---

## 🎨 Advanced Scales Reference

| Scale | Code | Mood |
|-------|------|------|
| Natural Minor | `"C:minor"` | Dark, standard |
| Minor Pentatonic | `"C:minor:pentatonic"` | Safe, melodic |
| Dorian | `"C:dorian"` | Slightly brighter |
| Phrygian | `"C:phrygian"` | Eastern, dark |
| Harmonic Minor | `"C:harmonic:minor"` | Dramatic, cinematic |
| Whole Tone | `"C:whole:tone"` | Dreamy, floating |
| Lydian | `"C:lydian"` | Bright, ethereal |
| Chromatic | `"C:chromatic"` | All 12 notes |

---

## 🧩 Kick Drum Variations Reference

| Style | `penv` | `pdecay` | `decay` | Extra |
|-------|--------|----------|---------|-------|
| Tight, punchy | 36 | .04 | .25 | — |
| Deep, round | 18 | .08 | .5 | — |
| Hard, distorted | 36 | .05 | .35 | `.distort("3:.5")` |
| Sub-heavy | 12 | .10 | .6 | — |
| Clicky | 48 | .03 | .2 | `.distort("1:.5")` |

---

## 📋 Reverb Recipes

| Preset | Code |
|--------|------|
| Tight (claps) | `.room(.15).roomsize(.3)` |
| Medium hall (pads) | `.room(.4).roomsize(.6).roomlp(4000)` |
| Cathedral (breakdown) | `.room(.8).roomsize(.95).roomlp(2000).roomdim(3000)` |
| Dark atmospheric | `.room(.6).roomsize(.85).roomlp(1500).roomdim(2000)` |

---

## ⚙️ Swing

```js
.swingBy(amount, subdivision)
```

| Amount | Feel |
|--------|------|
| `1/8` | Very subtle |
| **`1/6`** | **Standard techno groove** |
| `1/4` | Noticeable shuffle |
| `1/3` | Heavy swing (jazzy) |

Subdivision: typically `4` (for 16th-note grid)

---

## 🔥 Performance Power Moves — Quick Copy

### Evolving Hi-Hat (layer all these on)
```js
$: s("hh*16").bank("RolandTR909").gain(".5 .3 .8 .3")
  .swingBy(1/6, 4)
  .sometimes(x => x.speed(1.5))
  .lastOf(4, x => x.ply(2))
  .degradeBy(perlin.range(0, 0.3).slow(8))
  .hpf(sine.range(4000, 10000).slow(16))
```

### Breathing Bass
```js
$: note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
  .lpf(sine.range(300, 1500).slow(8))
  .lpq(sine.range(5, 15).slow(12))
  .ftype("ladder").decay(.2).sustain(0)
  .lastOf(4, x => x.rev())
  .rarely(x => x.add(note(12)))
```

### 16-Cycle Tension Builder
```js
$: note("c2!4").s("sawtooth")
  .lpf(saw.range(100, 6000).slow(16))
  .lpq(saw.range(0, 15).slow(16))
  .distort(saw.range(0, 4).slow(16))
  .gain(saw.range(0.3, 0.8).slow(16))
  .decay(.2).sustain(0)
```

### Generative Percussion
```js
$: s("rim(5,16)").bank("RolandTR909")
  .iter(4).gain(rand.range(0.2, 0.5)).pan(rand)
  .delay(.2).delaytime(.1875)
  .sometimes(x => x.speed(1.5))
  .lastOf(8, x => x.fast(2))
```

---

## 🧠 Wavetables

```js
note("c3").s("wt_flute")     // Use wt_ prefix
note("c3").s("wt_piano")
note("c3").s("wt_violin")
note("c3").s("wt_saw")
.n("<1 2 3 4 5>"             // Scan through variations
```

> 1000+ wavetables from AKWF library. Good for pads and evolving textures.

---

## 🧪 Misc Hard-to-Remember

| Function | What It Does |
|----------|-------------|
| `.euclid(3, 8)` | Euclidean rhythm as modifier |
| `.struct("x ~ x ~ x ~ ~ x")` | Binary rhythm mask |
| `.noise(0.15)` | Blend noise into oscillator |
| `.density(0.03)` | Control crackle density |
| `soundAlias('RolandTR808_bd', 'kick')` | Create sample alias |
| `._scope()` | Show oscilloscope |
| `.pianoroll()` | Show pianoroll viz |
| `._pianoroll()` | Inline pianoroll |
| `.compressor("-20:20:10:.002:.02")` | `threshold:ratio:knee:attack:release` |
| `.distort("amount:asymmetry")` | e.g. `"1.5:.6"` |
| `.delay("level:time:feedback")` | Inline combo |
| `.adsr("a:d:s:r")` | Inline envelope |

---

## ⚡ Error Recovery (Live)

| Problem | Fix |
|---------|-----|
| Syntax error | Fix typo → re-evaluate. Old pattern keeps playing. |
| Everything wrong | `Ctrl+.` → pause → fix → re-evaluate known-good template |
| One bad element | Replace with `$: s("~")` → fix → swap back |
| Lost track | Comment out all, `Ctrl+Enter`, rebuild from kick |
| CPU overload | Fewer `*16`, less reverb, fewer orbits |

---

*Print this. Tape it next to your screen. You'll need it mid-set.* 🔥
