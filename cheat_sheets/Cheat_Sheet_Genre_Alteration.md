# 🌬️ Breath of Strudel — Genre Alteration Cheat Sheet

> **One codebase. Many worlds.**
> Every genre below starts from the same Strudel foundation — change the tempo, swap the sounds, shift the scale, adjust the effects — and you're in a completely different genre. Each section includes the facts, the character, and a complete working setup.

---

## 📊 Genre Overview — Quick Comparison

| Genre | BPM | Time Feel | Key/Scale | Kick | Bass | Effects | Sidechain | Pioneer Artists |
|-------|-----|-----------|-----------|------|------|---------|-----------|-----------------|
| **Minimal Techno** | 125–132 | Straight 4/4 | Minor, Dorian | Punchy sine+penv | Saw+ladder LPF | Delay, subtle reverb | Strong pump | Richie Hawtin, Robert Hood, Ricardo Villalobos |
| **Deep House** | 118–124 | Swung 4/4 | Minor, Dorian, Major | Boomy 808 | Warm triangle/sine | Reverb, warm delay | Subtle pump | Larry Heard, Kerri Chandler, Maya Jane Coles |
| **Dub Techno** | 110–125 | Hypnotic 4/4 | Minor | Deep, muted | Sub sine | Heavy reverb + delay | None/subtle | Basic Channel, Deepchord, Monolake |
| **Acid House / Acid Techno** | 125–140 | Driving 4/4 | Minor, Phrygian | 909 punchy | Saw + high-Q ladder | Delay, distortion | Moderate | Phuture, DJ Pierre, Hardfloor |
| **Psytrance** | 138–148 | Driving 4/4 | Phrygian, Harm. Minor | Tight sine+penv | Saw offbeat rolling | Delay, phaser | Tight pump | Infected Mushroom, Astrix, Hallucinogen |
| **Drum & Bass** | 170–178 | Breakbeat | Minor, Minor Pent. | Snappy, tight | Heavy reese saw | Reverb, distortion | Moderate | Goldie, LTJ Bukem, Noisia, Andy C |
| **Lo-Fi Hip Hop** | 70–85 | Swung boom-bap | Maj7, Min7, Jazz | Soft, muted 808 | Warm sine/triangle | Vinyl crackle, crush | None | Nujabes, J Dilla, tomppabeats |
| **Ambient** | 0–60 (or none) | Free / no beat | Lydian, Whole Tone | None | Drone sine/pad | Massive reverb | None | Brian Eno, Stars of the Lid, Aphex Twin (SAW2) |
| **Synthwave / Retrowave** | 90–118 | Straight/swung 4/4 | Minor, Dorian | LinnDrum/808 | Square/saw arp | Gated reverb, chorus | Light pump | Kavinsky, Perturbator, Carpenter Brut |
| **Melodic Techno** | 122–128 | Straight 4/4 | Minor, Harm. Minor, Lydian | Deep, clean | Saw + LPF mod | Long reverb, delay | Moderate | Tale of Us, Stephan Bodzin, Adriatique |
| **Trance** | 128–140 | Driving 4/4 | Minor, Harm. Minor | 909 layered | Saw supersaw | Big reverb, delay, gate | Strong pump | Armin van Buuren, Paul van Dyk, Above & Beyond |
| **Industrial / Hard Techno** | 140–160+ | Pounding 4/4 | Minor, Chromatic | Distorted, clipped | Distorted saw/square | Heavy distort, crush | Aggressive | Surgeon, Paula Temple, Perc |

---

## 🎛️ The Base Template — Minimal Techno (128 BPM)

> This is the course's default. Every genre below shows **what to change** from this baseline.

```js
setcpm(128/4)

$: note("c1!4").s("sine")
  .penv(24).pdecay(.06).decay(.4).sustain(0)
  .distort("1.5:.6").gain(0.85)

$: s("hh*16").bank("RolandTR909")
  .gain(".5 .3 .8 .3").hpf(6000).swingBy(1/6, 4)

$: s("~ cp ~ cp").bank("RolandTR909")
  .room(0.15).gain(0.65)

$: note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
  .lpf(400).lpq(12).lpenv(4).lpd(.12).lps(0)
  .ftype("ladder").decay(.2).sustain(0).gain(0.55)

$: note("<[c4,eb4,g4] [bb3,d4,f4]>").s("sawtooth")
  .lpf(2000).attack(.3).release(1)
  .room(.4).gain(0.25).orbit(2)

$: s("bd*4").bank("RolandTR909")
  .duckorbit(2).duckattack(.2).duckdepth(1).gain(0)
```

---

## 🏠 Deep House — 120 BPM

> **Origin:** Chicago, mid-1980s. Larry Heard ("Can You Feel It") defined the soulful, warm sound.  
> **Character:** Warm, soulful, groovy. Lush chords, deep bass, swung groove. Music for the body AND the soul.

### What Changes from Minimal Techno

| Parameter | Minimal Techno | Deep House |
|-----------|---------------|------------|
| Tempo | 128 BPM | **120 BPM** |
| Kick | 909 / synthesized | **808** (boomy, long decay) |
| Bass | Saw + ladder filter | **Triangle/sine** (warmer) |
| Chords | Minor triads | **Major 7th, Minor 7th** (jazzy) |
| Scale | Minor | **Dorian / Major** (brighter) |
| Swing | Light (1/6) | **Heavier (1/4)** |
| Hi-hats | 16ths, crisp | **8ths or lazy 16ths** |
| Effects | Subtle reverb | **Longer reverb, warm** |

### Complete Deep House Setup

```js
setcpm(120/4)

// Kick — deep 808 boom
$: s("bd*4").bank("RolandTR808").gain(0.85)

// Hi-hats — lazy 8ths with heavy swing
$: s("hh*8").bank("RolandTR808")
  .gain(".6 .3 .5 .3").hpf(5000)
  .swingBy(1/4, 4)

// Clap — warm, roomy
$: s("~ cp ~ cp").bank("RolandTR808")
  .room(0.3).roomsize(0.5).gain(0.55)

// Bass — warm triangle, simple root motion
$: note("<c2 f2 ab2 bb2>").s("triangle")
  .lpf(600).lpq(3)
  .decay(0.35).sustain(0.4).release(0.15)
  .gain(0.5)

// Pad — lush major 7th chords, jazzy voicings
$: chord("<Cmaj7 Fm7 Abmaj7 Bb7>").voicing()
  .s("sawtooth")
  .lpf(sine.range(1200, 3000).slow(16)).lpq(2)
  .attack(0.5).sustain(0.6).release(1.5)
  .room(0.5).roomsize(0.7)
  .gain(0.22).orbit(2)

// Sidechain — gentle pump
$: s("bd*4").bank("RolandTR808")
  .duckorbit(2).duckattack(0.3).duckdepth(0.6).gain(0)
```

---

## 🌫️ Dub Techno — 118 BPM

> **Origin:** Berlin, early 1990s. Basic Channel (Moritz von Oswald & Mark Ernestus) fused Jamaican dub's echo techniques with Detroit techno.  
> **Character:** Hypnotic, submerged. Sounds feel like they're arriving through fog. The delay and reverb *are* the music.

### What Changes from Minimal Techno

| Parameter | Minimal Techno | Dub Techno |
|-----------|---------------|------------|
| Tempo | 128 BPM | **118 BPM** (slower, deeper) |
| Kick | Punchy | **Muted, deep, soft attack** |
| Percussion | Crisp, direct | **Drenched in reverb + delay** |
| Chords | Sustained pad | **Short stabs → long reverb tail** |
| Reverb | Moderate | **Extreme** (roomsize 0.85–0.95) |
| Delay | Light echo | **Heavy, high feedback, filtered** |
| Melody | Sparse, delayed | **Dubby, submerged** |
| Overall | Defined, direct | **Blurred, atmospheric, echo-drenched** |

### Complete Dub Techno Setup

```js
setcpm(118/4)

// Kick — deep, soft, muted
$: note("c1!4").s("sine")
  .penv(16).pdecay(.08).decay(.5).sustain(0)
  .gain(0.75)

// Hi-hats — sparse, reverbed
$: s("hh*8").bank("RolandTR909")
  .gain(".4 .2 .3 .2")
  .hpf(7000).room(0.3).roomsize(0.5)

// Rimshot — euclidean, delay-drenched
$: s("rim(3,8)").bank("RolandTR909")
  .gain(0.3).pan(rand)
  .delay(0.6).delaytime(0.1875).delayfeedback(0.7)
  .room(0.4).roomsize(0.6)

// Chord stab — short hit, MASSIVE reverb tail
$: note("<[c3,eb3,g3] [bb2,d3,f3] [ab2,c3,eb3] [g2,bb2,d3]>")
  .s("sawtooth")
  .lpf(1200).lpq(3)
  .decay(0.08).sustain(0).release(0.02)
  .room(0.85).roomsize(0.95).roomlp(2000).roomdim(3000)
  .delay(0.5).delaytime(0.375).delayfeedback(0.65)
  .gain(0.35).orbit(1)

// Sub bass — deep sine drone
$: note("<c1 c1 ab1 bb1>").s("sine")
  .lpf(150).decay(0.5).sustain(0.5)
  .gain(0.5)

// Texture — vinyl warmth
$: s("crackle*2").density(0.04).gain(0.08)
```

---

## 🧪 Acid House / Acid Techno — 130 BPM

> **Origin:** Chicago, 1987. Phuture's "Acid Tracks" — the Roland TB-303 bass synthesizer's squelchy filter became the defining sound of a movement.  
> **Character:** Hypnotic, squelchy, psychedelic. The resonant filter sweep IS the genre.

### What Changes from Minimal Techno

| Parameter | Minimal Techno | Acid |
|-----------|---------------|------|
| Tempo | 128 BPM | **130–138 BPM** |
| Bass filter | Moderate resonance | **Very high Q (15–25+)** |
| Filter type | Ladder | **Ladder** (essential) |
| Filter envelope | Subtle | **Dramatic, long sweeps** |
| Bass pattern | Sparse | **Rolling 16ths** |
| Distortion | Light | **Medium-heavy** |
| Scale | Minor | **Minor / Phrygian** |

### Complete Acid Setup

```js
setcpm(130/4)

// Kick — 909 punchy
$: s("bd*4").bank("RolandTR909").gain(0.85)

// Hi-hats — driving 16ths
$: s("[hh hh hh <hh oh>]*4").bank("RolandTR909")
  .cut(1).gain(".6 .3 .7 .3").hpf(6000)

// Clap
$: s("~ cp ~ cp").bank("RolandTR909").room(0.1).gain(0.6)

// THE ACID LINE — saw + ladder + high resonance + filter envelope
$: note("[c2 c2 <eb2 f2> c2 <c2 g2> c2 <eb2 d2> c2]*2")
  .s("sawtooth")
  .lpf(sine.range(200, 3500).slow(8))
  .lpq(20)
  .lpenv(6).lpa(.005).lpd(.15).lps(0)
  .ftype("ladder")
  .decay(0.15).sustain(0).release(0.03)
  .distort("2:.5")
  .gain(0.55)

// Percussion — cowbell, classic acid house
$: s("~ ~ cb ~").bank("RolandTR808").gain(0.25)
  .delay(0.2).delaytime(0.125)
```

---

## 🍄 Psytrance — 142 BPM

> **Origin:** Goa, India, late 1980s → Israel/Europe, 1990s. Evolved from Goa trance parties into a global electronic movement.  
> **Character:** Hypnotic, driving, psychedelic. Rolling bass on offbeats, dense layered textures, the Phrygian scale gives it that distinctive "Eastern" edge. Tracks build continuously for 7–10 minutes.

### What Changes from Minimal Techno

| Parameter | Minimal Techno | Psytrance |
|-----------|---------------|-----------|
| Tempo | 128 BPM | **140–148 BPM** |
| Kick | Medium decay | **Shorter, tighter** (leave room for bass) |
| Bass | On-beat, filtered | **Offbeat, rolling, modulated** |
| Scale | Minor | **Phrygian / Harmonic Minor** |
| Build structure | Subtle | **Continuous escalation** |
| Effects | Subtle | **Phaser, heavy delay, filtering** |
| Texture | Minimal | **Dense, layered, alien** |

### Complete Psytrance Setup

```js
setcpm(142/4)

// Kick — tight, punchy, short (leave room for bass)
$: note("c1!4").s("sine")
  .penv(36).pdecay(.04).decay(.2).sustain(0)
  .gain(0.85)

// Offbeat bass — THE psytrance rolling bass
$: note("[~ c2 ~ c2 ~ c2 ~ c2]*2").s("sawtooth")
  .lpf(sine.range(400, 2000).slow(4))
  .lpq(8).ftype("ladder")
  .decay(0.12).sustain(0)
  .distort("1.5:.5")
  .gain(0.55)

// Hi-hats — driving
$: s("hh*16").bank("RolandTR909")
  .gain(".5 .3 .6 .3").hpf(8000)

// Clap — sparse
$: s("~ cp ~ ~").bank("RolandTR909")
  .room(0.1).gain(0.5)

// Phrygian lead — spiraling, psychedelic
$: n("<[0 1 4 3] [0 5 4 1] [0 3 2 5] [0 1 6 3]>")
  .scale("C4:phrygian")
  .s("square")
  .lpf(sine.range(1000, 5000).slow(8)).lpq(5)
  .decay(0.1).sustain(0)
  .delay(0.4).delaytime(0.125).delayfeedback(0.5)
  .phaser(0.5).phaserdepth(0.6)
  .gain(0.2)

// Texture — alien atmosphere
$: note("c5").s("sine")
  .fm(sine.range(2, 12).slow(16))
  .fmh("<1.41 2.76 3.14 1.73>")
  .fmdecay(0.3).fmsustain(0)
  .decay(0.5).sustain(0)
  .room(0.5).roomsize(0.8)
  .delay(0.3).delaytime(0.25).delayfeedback(0.4)
  .degradeBy(0.5).gain(0.12)
```

---

## 🥁 Drum & Bass — 174 BPM

> **Origin:** London, early 1990s. Evolved from breakbeat hardcore and jungle. The "Amen break" is its rhythmic DNA.  
> **Character:** Fast, syncopated breakbeats, heavy sub-bass, intense energy. The drums and bass are equal protagonists.

### What Changes from Minimal Techno

| Parameter | Minimal Techno | Drum & Bass |
|-----------|---------------|-------------|
| Tempo | 128 BPM | **170–178 BPM** |
| Kick pattern | Four-on-the-floor | **Syncopated breakbeat** |
| Hi-hats | 16ths, straight | **Broken, syncopated** |
| Snare | Beats 2 & 4 | **Off-beat, rapid fills** |
| Bass | Saw + filter env | **Heavy, detuned reese** |
| Bass octave | C2 range | **C1–C2 (sub-heavy)** |
| Effects | Subtle | **Distortion, heavy compression** |

### Complete Drum & Bass Setup

```js
setcpm(174/4)

// Drums — syncopated breakbeat pattern
$: s("bd ~ [~ bd] ~, ~ sd ~ [sd ~], [hh hh ~ hh]*2")
  .bank("RolandTR909")
  .gain("0.85, 0.7, .4 .3 .4 .3")
  .compressor("-15:12:10:.002:.02")

// Bonus percussion — ghost snares, rapid fills
$: s("~ ~ ~ sd").bank("RolandTR909")
  .gain(0.25)
  .lastOf(4, x => x.ply(2).fast(2).gain(0.5))

// Bass — heavy reese (detuned saws)
$: note("<c1 c1 <eb1 f1> c1>").s("sawtooth")
  .lpf(sine.range(200, 1200).slow(4))
  .lpq(5).ftype("ladder")
  .decay(0.3).sustain(0.3).release(0.1)
  .distort("2:.5")
  .gain(0.55)

// Sub layer — clean low end underneath
$: note("<c1 c1 <eb1 f1> c1>").s("sine")
  .lpf(100).decay(0.4).sustain(0.4)
  .gain(0.45)

// Pad — atmospheric minor chords
$: chord("<Cm Am7 Fm Gm>").voicing()
  .s("sawtooth")
  .lpf(1500).lpq(2)
  .attack(0.3).sustain(0.5).release(1)
  .room(0.4).roomsize(0.6)
  .gain(0.15).orbit(2)
```

---

## 📻 Lo-Fi Hip Hop — 75 BPM

> **Origin:** Late 2000s internet culture. Draws from J Dilla's off-grid drums, Nujabes' jazz sampling, and the aesthetic of worn cassette tapes.  
> **Character:** Warm, nostalgic, imperfect. Intentionally muted, detuned, and "dusty." The vinyl crackle is not a flaw — it's the point.

### What Changes from Minimal Techno

| Parameter | Minimal Techno | Lo-Fi Hip Hop |
|-----------|---------------|---------------|
| Tempo | 128 BPM | **70–85 BPM** |
| Kick | Punchy, tight | **Soft, muted 808, long** |
| Snare | 909 crisp | **Soft, lo-fi, off-grid** |
| Hi-hats | 16ths, mechanical | **8ths, swung, humanized** |
| Chords | Minor triads | **Major 7th, Minor 9th (jazz)** |
| Scale | Minor | **Dorian / Major 7th voicings** |
| Bass | Filtered saw | **Warm sine or triangle** |
| Effects | Clean reverb | **Vinyl crackle, bit crush, detune** |
| Feel | Quantized, precise | **Loose, "drunken," swung** |

### Complete Lo-Fi Hip Hop Setup

```js
setcpm(75/4)

// Kick — soft, boomy 808
$: s("bd ~ ~ bd ~ ~ bd ~").bank("RolandTR808")
  .gain(0.7).lpf(400)

// Snare — laid back, off-grid
$: s("~ sd ~ ~, ~ ~ ~ sd").bank("RolandTR808")
  .gain(0.5).room(0.15)
  .late(0.02)

// Hi-hats — lazy swing
$: s("hh*8").bank("RolandTR808")
  .gain(".4 .2 .3 .2")
  .swingBy(1/3, 4)
  .hpf(5000)

// Bass — warm, simple
$: note("<c2 a1 f2 g2>").s("triangle")
  .lpf(400).lpq(2)
  .decay(0.4).sustain(0.4)
  .gain(0.45)

// Chords — jazzy Rhodes-like
$: chord("<Cmaj7 Am7 Fmaj7 G7>").voicing()
  .s("triangle")
  .lpf(2000).lpq(1)
  .attack(0.05).decay(0.3).sustain(0.4).release(0.5)
  .room(0.3).roomsize(0.4)
  .gain(0.2)

// Melody — dreamy pentatonic
$: n("<0 ~ 4 ~ 2 ~ 5 ~>")
  .scale("C4:major:pentatonic")
  .s("triangle")
  .lpf(2500)
  .decay(0.2).sustain(0.3).release(0.3)
  .delay(0.3).delaytime(0.2).delayfeedback(0.35)
  .gain(0.18)

// VINYL CRACKLE — the lo-fi soul
$: s("crackle*4").density(0.08).gain(0.12)

// TAPE WOBBLE — subtle pitch instability via noise texture
$: s("brown").begin(rand).end(rand.add(0.005))
  .hpf(3000).lpf(8000).gain(0.03)
```

---

## 🌌 Ambient — No Fixed Tempo

> **Origin:** Brian Eno's *Ambient 1: Music for Airports* (1978) defined the genre — music "as ignorable as it is interesting."  
> **Character:** No beat (or barely there). Texture over rhythm, atmosphere over melody. Long sustained tones, reverb as an instrument.

### What Changes from Minimal Techno

| Parameter | Minimal Techno | Ambient |
|-----------|---------------|---------|
| Tempo | 128 BPM | **Very slow or none** |
| Kick | 4-on-the-floor | **None** |
| Drums | Full kit | **None (or sparse texture)** |
| Bass | Rhythmic saw | **Drone, long sine pad** |
| Chords | Short, filtered | **Massive, sustained, evolving** |
| Scale | Minor | **Lydian, Whole Tone** (floating) |
| Reverb | Moderate | **Enormous** (0.9+ roomsize) |
| Attack | Fast | **Very slow (1–3 seconds)** |
| Modulation | Some | **Everything moves slowly** |

### Complete Ambient Setup

```js
setcps(0.08)  // Very slow — nearly frozen time

// Drone — fundamental tone
$: note("c2").s("sine")
  .attack(3).decay(2).sustain(0.8).release(4)
  .room(0.9).roomsize(0.95).roomlp(1500)
  .gain(0.3)

// Evolving pad — lydian chords, glacial movement
$: note("<[c4,e4,g4,b4] [d4,f#4,a4,c5] [e4,g4,b4,d5]>")
  .s("sawtooth")
  .lpf(perlin.range(600, 2500).slow(32))
  .lpq(1)
  .attack(2).decay(3).sustain(0.7).release(4)
  .room(0.95).roomsize(0.98).roomlp(1200).roomdim(2000)
  .gain(0.15)

// Melody — sparse, generative, floating
$: n(perlin.segment(4).range(0, 8))
  .scale("C4:lydian")
  .s("triangle")
  .attack(0.5).decay(1).sustain(0.3).release(2)
  .delay(0.6).delaytime(0.5).delayfeedback(0.7)
  .room(0.7).roomsize(0.9)
  .degradeBy(0.5)
  .gain(0.1)

// Texture — wind, breath
$: s("brown")
  .begin(perlin).end(perlin.add(0.01))
  .hpf(2000).lpf(6000)
  .room(0.8).roomsize(0.9)
  .gain(0.04)
```

---

## 🌆 Synthwave / Retrowave — 105 BPM

> **Origin:** Late 2000s internet revival of 1980s electronic aesthetics. Inspired by John Carpenter soundtracks, Giorgio Moroder, and Tangerine Dream.  
> **Character:** Nostalgic, neon-lit, cinematic. Big arpeggios, gated snares, pulsing sidechain, the sound of a VHS sunset.

### What Changes from Minimal Techno

| Parameter | Minimal Techno | Synthwave |
|-----------|---------------|-----------|
| Tempo | 128 BPM | **90–118 BPM** |
| Kick | Sine synthesized | **LinnDrum or 808** |
| Snare | 909 clap | **Gated reverb snare** (massive) |
| Bass | Filtered saw | **Pulsing arpeggio or square** |
| Chords | Dark minor | **Epic minor with extensions** |
| Lead | Sparse | **Soaring, vibrato, bright** |
| Effects | Subtle | **Gated reverb, chorus** |

### Complete Synthwave Setup

```js
setcpm(105/4)

// Kick — punchy LinnDrum
$: s("bd*4").bank("LinnDrum").gain(0.8)

// Snare — HUGE gated reverb (the 80s sound)
$: s("~ sd ~ sd").bank("LinnDrum")
  .room(0.7).roomsize(0.6)
  .compressor("-15:20:5:.001:.05")
  .gain(0.75)

// Hi-hats
$: s("hh*8").bank("LinnDrum")
  .gain(".5 .3 .4 .3").hpf(5000)

// Bass — pulsing arpeggio
$: note("[c2 c3 c2 c3]*2").s("square")
  .lpf(800).lpq(5)
  .decay(0.15).sustain(0).release(0.05)
  .gain(0.45)

// Pad — massive, sidechained, cinematic
$: chord("<Cm Abmaj7 Fm Gm>").voicing()
  .s("sawtooth")
  .lpf(sine.range(1500, 3500).slow(16)).lpq(2)
  .attack(0.4).sustain(0.6).release(1.5)
  .room(0.5).roomsize(0.7)
  .gain(0.22).orbit(2)

// Sidechain pump
$: s("bd*4").bank("LinnDrum")
  .duckorbit(2).duckattack(0.25).duckdepth(0.7).gain(0)

// Lead — soaring, bright, delayed
$: n("<0 ~ 4 7 ~ 4 2 ~>*2")
  .scale("C4:minor")
  .s("sawtooth")
  .lpf(4000).lpq(3)
  .attack(0.05).decay(0.3).sustain(0.4).release(0.5)
  .delay(0.4).delaytime(0.234).delayfeedback(0.45)
  .room(0.3)
  .gain(0.22)
```

---

## ✨ Melodic Techno — 124 BPM

> **Origin:** Early 2010s, Berlin/Italy. Afterlife label (Tale of Us) defined the sound — techno with emotion.  
> **Character:** Elegant, emotional, hypnotic. Deep grooves with soaring pads, minor-key melancholy, and progressive builds. The "thinking person's dancefloor."

### Complete Melodic Techno Setup

```js
setcpm(124/4)

// Kick — deep, clean
$: note("c1!4").s("sine")
  .penv(20).pdecay(.07).decay(.45).sustain(0)
  .gain(0.82)

// Hi-hats — subtle, textured
$: s("hh*16").bank("RolandTR909")
  .gain(".4 .2 .5 .2").hpf(7000)

// Clap — spacious
$: s("~ cp ~ cp").bank("RolandTR909")
  .room(0.2).roomsize(0.4).gain(0.55)

// Bass — deep, evolving
$: note("<c2 a1 f2 g2>").s("sawtooth")
  .lpf(sine.range(300, 900).slow(16)).lpq(6)
  .lpenv(3).lpd(0.15).lps(0)
  .ftype("ladder")
  .decay(0.25).sustain(0).release(0.05)
  .gain(0.5)

// Pad — lush, emotional, harmonic minor inflections
$: chord("<Cm Am7 Fm G>").voicing()
  .s("sawtooth")
  .lpf(sine.range(1000, 2800).slow(16)).lpq(2)
  .attack(0.6).decay(1).sustain(0.5).release(2)
  .room(0.6).roomsize(0.8).roomlp(3000)
  .gain(0.2).orbit(2)

// Sidechain — moderate pump
$: s("bd*4").bank("RolandTR909")
  .duckorbit(2).duckattack(0.25).duckdepth(0.8).gain(0)

// Lead — haunting, delayed, minor
$: n("<0 ~ 4 ~ 7 ~ 4 ~>")
  .scale("C4:harmonic:minor")
  .s("triangle")
  .attack(0.1).decay(0.3).sustain(0.2).release(0.5)
  .delay(0.5).delaytime(0.234).delayfeedback(0.55)
  .pan(sine.range(0.3, 0.7).slow(4))
  .degradeBy(0.2).gain(0.18)

// Percussion — textured, panned
$: s("rim(5,16)").bank("RolandTR909")
  .iter(4).gain(rand.range(0.15, 0.35)).pan(rand)
  .delay(0.2).delaytime(0.117)
```

---

## 🔊 Trance — 138 BPM

> **Origin:** Frankfurt/Berlin, early 1990s. Sven Väth, Paul van Dyk, and the Anjunabeats label shaped the sound.  
> **Character:** Euphoric, uplifting, anthemic. Massive builds, soaring leads, supersaw pads, and the iconic "trance gate" effect. Made for hands-in-the-air moments.

### Complete Trance Setup

```js
setcpm(138/4)

// Kick — punchy, layered
$: s("bd*4").bank("RolandTR909").gain(0.85)

// Hi-hats — driving
$: s("[hh hh hh <hh oh>]*4").bank("RolandTR909")
  .cut(1).gain(".6 .3 .5 .4").hpf(6000)

// Clap — big, roomy
$: s("~ cp ~ cp").bank("RolandTR909")
  .room(0.25).roomsize(0.4).gain(0.65)

// Bass — pulsing, sidechained
$: note("c2!8").s("sawtooth")
  .lpf(600).lpq(8)
  .lpenv(3).lpd(0.1).lps(0)
  .ftype("ladder")
  .decay(0.15).sustain(0)
  .gain(0.5)

// Supersaw pad — THE trance pad (wide, huge)
$: chord("<Cm Fm Ab Bb>").voicing()
  .s("sawtooth")
  .superimpose(x => x.add(note(0.12)))
  .lpf(sine.range(1500, 4000).slow(16)).lpq(2)
  .attack(0.3).sustain(0.6).release(1.5)
  .room(0.5).roomsize(0.75)
  .juxBy(0.3, rev)
  .gain(0.2).orbit(2)

// Sidechain — strong pump
$: s("bd*4").bank("RolandTR909")
  .duckorbit(2).duckattack(0.2).duckdepth(1).gain(0)

// Lead — soaring, uplifting
$: n("<0 2 4 7 4 2 0 ~>*2")
  .scale("C4:harmonic:minor")
  .s("sawtooth")
  .lpf(5000).lpq(3)
  .attack(0.02).decay(0.2).sustain(0.3).release(0.3)
  .delay(0.4).delaytime(0.217).delayfeedback(0.5)
  .room(0.3)
  .gain(0.22)

// Trance gate effect on pad layer
$: chord("<Cm Fm Ab Bb>").voicing()
  .s("sawtooth")
  .lpf(3000)
  .struct("[x x ~ x x ~ x ~]*2")
  .decay(0.08).sustain(0).release(0.03)
  .delay(0.3).delaytime(0.1).delayfeedback(0.4)
  .gain(0.15)
```

---

## ⚙️ Industrial / Hard Techno — 145 BPM

> **Origin:** Late 1980s industrial music (Einstürzende Neubauten, Throbbing Gristle) merged with 1990s techno. The Birmingham/Berlin axis (Surgeon, Regis).  
> **Character:** Aggressive, relentless, abrasive. Distorted everything, pounding kicks, noise as melody. Not for the faint of heart.

### Complete Industrial Techno Setup

```js
setcpm(145/4)

// Kick — distorted, punishing
$: note("d1!4").s("sine")
  .penv(36).pdecay(.04).decay(.3).sustain(0)
  .distort("4:.5").gain(0.9)

// Hi-hats — crushed, metallic
$: s("hh*16").bank("RolandTR909")
  .gain(".6 .3 .8 .3")
  .crush(6).hpf(6000)

// Snare — hard, industrial
$: s("~ sd ~ [sd sd]").bank("RolandTR909")
  .distort("2:.5").gain(0.7)

// Bass — distorted, grinding
$: note("[c2!4 <eb2 f2>!2 c2!2]*2").s("sawtooth")
  .lpf(sine.range(200, 1500).fast(2)).lpq(10)
  .ftype("ladder")
  .decay(0.1).sustain(0)
  .distort("3:.5")
  .gain(0.55)

// Noise hit — industrial percussion
$: s("white(3,8)")
  .bpf(sine.range(1000, 5000).slow(4)).bpq(8)
  .decay(0.05).sustain(0)
  .distort("2:.5")
  .gain(0.3)

// Texture — harsh, mechanical
$: s("brown*2").coarse(16).crush(4)
  .gain(0.08)
```

---

## 🔀 Quick Genre Conversion Reference

> Starting from the **base Minimal Techno template** — what to change for each genre:

| What to Change | Deep House | Dub Techno | Acid | Psytrance | DnB | Lo-Fi | Ambient | Synthwave | Trance | Industrial |
|----------------|-----------|------------|------|-----------|-----|-------|---------|-----------|--------|------------|
| **`setcpm()`** | `120/4` | `118/4` | `130/4` | `142/4` | `174/4` | `75/4` | `setcps(0.08)` | `105/4` | `138/4` | `145/4` |
| **Kick bank** | TR808 | — | TR909 | — | TR909 | TR808 | *remove* | LinnDrum | TR909 | — |
| **Bass wave** | triangle | sine | sawtooth | sawtooth | sawtooth | triangle | sine | square | sawtooth | sawtooth |
| **Bass `.lpq()`** | 3 | — | **20** | 8 | 5 | 2 | — | 5 | 8 | 10 |
| **`.ftype()`** | — | — | ladder | ladder | ladder | — | — | — | ladder | ladder |
| **Scale** | dorian | minor | phrygian | phrygian | minor | major | lydian | minor | harm.minor | minor |
| **Chord type** | Maj7 | minor | minor | — | minor | Maj7 | Maj7 | minor+ext | minor | — |
| **Reverb** | medium | **massive** | light | medium | medium | light | **massive** | medium | medium | dry |
| **Distort** | none | none | medium | light | medium | none | none | none | none | **heavy** |
| **Swing** | heavy | none | none | none | — | **heavy** | — | light | none | none |
| **Crackle** | no | yes | no | no | no | **yes** | optional | no | no | no |

---

*Your sounds. Your genre. Your rules.* 🌬️
