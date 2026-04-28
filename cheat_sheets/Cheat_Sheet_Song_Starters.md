# 🌬️ Breath of Strudel — Song Starters Cheat Sheet

> **25 ready-to-paste openers for when the audience walks in.**
> Each starter is a self-contained mini-groove — paste into [strudel.cc](https://strudel.cc/), hit `Ctrl+Enter`, and let the room fill up around the sound.

---

## 🧬 The Song Starter Formula

What separates a good opener from a random loop? Research into the DJ sets of **Robert Hood**, **Richie Hawtin**, **Ricardo Villalobos**, **Basic Channel**, and **Stephan Bodzin** reveals a consistent formula:

| Principle | Why It Matters |
|-----------|---------------|
| **Low-to-medium energy** | People are arriving, talking, settling — don't assault them |
| **Minimum 3 elements** | A kick alone is boring; 3+ layers create a "world" to enter |
| **Hypnotic repetition with micro-variation** | The brain locks on to the loop, then notices the subtle changes |
| **Space between the sounds** | Silence is an instrument — let each element breathe |
| **Gradual timbral evolution** | Slow filter sweeps, drifting delays — the sound *lives* |
| **120–130 BPM sweet spot** | Fast enough to groove, slow enough to be background |
| **Dark, warm, or dubby — never harsh** | The opener sets the emotional temperature of the room |

### The 3-Layer Minimum

Every starter below has at least three independent `$:` patterns. The layers follow a hierarchy:

```
┌─────────────────┐
│  FOUNDATION     │  Kick / sub-bass — the pulse you feel
├─────────────────┤
│  TEXTURE        │  Hi-hats / percussion / noise — the air around the pulse
├─────────────────┤
│  COLOUR         │  Pad / chord / melody / delay tail — the emotional signature
└─────────────────┘
```

### How to Use These

1. **Pick one** that matches the mood you want
2. **Paste** into [strudel.cc](https://strudel.cc/) → `Ctrl+Enter`
3. **Let it play** for 2–4 minutes as-is (audience entering)
4. **Evolve** — uncomment optional lines, tweak filter values, add elements from other starters
5. **Transition** into your main set by layering in heavier elements

---

## 🌫️ Category I — Fog & Atmosphere

*Ambient-leaning openers. Pads drift, textures hiss, kicks are barely there. Perfect for the first 10 minutes when the room is still half-empty.*

---

### 1. 「 Fog Signal 」

> *A lighthouse horn through dense marine fog. Sub-bass drones, distant reverbed percussion, and a pad that barely moves.*

**Key:** C minor · **BPM:** 120 · **Energy:** ●○○○○

```js
setcpm(120/4)

// Sub drone — felt, not heard
$: note("c1").s("sine")
  .attack(2).decay(1).sustain(0.8).release(3)
  .gain(0.4)

// Spectral percussion — distant rim through fog
$: s("rim(3,8)").bank("RolandTR909")
  .gain(0.15).pan(rand)
  .room(0.7).roomsize(0.85).roomlp(1500)
  .delay(0.5).delaytime(0.375).delayfeedback(0.65)

// Pad — glacial chord movement
$: note("<[c4,eb4,g4] [bb3,d4,f4] [ab3,c4,eb4] [g3,bb3,d4]>")
  .s("sawtooth")
  .lpf(perlin.range(800, 2000).slow(32)).lpq(1)
  .attack(1.5).decay(2).sustain(0.6).release(2)
  .room(0.8).roomsize(0.9).roomlp(2000)
  .gain(0.12)

// Texture — vinyl warmth
$: s("crackle*2").density(0.04).gain(0.06)
```

---

### 2. 「 First Light 」

> *Pre-dawn. The room is pitch black, and somewhere a synth tone is rising through the haze like sunlight under a door.*

**Key:** D minor · **BPM:** 122 · **Energy:** ●○○○○

```js
setcpm(122/4)

// Kick — barely present, muffled
$: note("d1!4").s("sine")
  .penv(12).pdecay(.1).decay(.5).sustain(0)
  .lpf(100).gain(0.45)

// Shimmering hats — sparse, high, reverbed
$: s("hh*8").bank("rolandcompurhythm78")
  .gain(".2 .1 .15 .1").hpf(8000)
  .room(0.5).roomsize(0.7)
  .degradeBy(0.3)

// Rising pad — slow filter sweep upward
$: note("[d4,f4,a4]").s("sawtooth")
  .lpf(saw.range(400, 2500).slow(64)).lpq(2)
  .attack(2).sustain(0.5).release(3)
  .room(0.6).roomsize(0.8)
  .gain(0.15)
```

---

### 3. 「 Dead Channel 」

> *Television static at 3am. White noise textures, a distant detuned pad, and a kick that sounds like it's behind a wall.*

**Key:** E minor · **BPM:** 118 · **Energy:** ●○○○○

```js
setcpm(118/4)

// Muted kick — behind the wall
$: note("e1!4").s("sine")
  .penv(10).pdecay(.12).decay(.6).sustain(0)
  .lpf(80).gain(0.35)

// Static texture — filtered white noise
$: s("white*4").begin(rand).end(rand.add(0.01))
  .bpf(sine.range(2000, 6000).slow(16)).bpq(4)
  .decay(0.05).sustain(0)
  .gain(0.08)

// Detuned pad — eerie, slightly off
$: note("<[e4,g4,b4] [d4,f#4,a4]>")
  .s("sawtooth")
  .superimpose(x => x.add(note(0.15)))
  .lpf(1200).lpq(1)
  .attack(1).sustain(0.4).release(2)
  .room(0.85).roomsize(0.92)
  .gain(0.1)

// Crackle bed
$: s("crackle*2").density(0.05).gain(0.07)
```

---

### 4. 「 Interstellar Drift 」

> *Floating through empty space. No gravity, no rush. FM bells ping off distant reverb walls while a sub hums below.*

**Key:** A minor · **BPM:** 120 · **Energy:** ●●○○○

```js
setcpm(120/4)

// Sub hum
$: note("a1").s("sine")
  .attack(1).decay(2).sustain(0.7).release(3)
  .gain(0.35)

// FM bell pings — sparse, random
$: note("<a4 ~ c5 ~ e5 ~ ~ ~>")
  .s("sine").fm(5).fmh(3.5)
  .fmdecay(0.15).fmsustain(0)
  .decay(0.8).sustain(0)
  .room(0.7).roomsize(0.85)
  .delay(0.5).delaytime(0.375).delayfeedback(0.6)
  .degradeBy(0.3).gain(0.15)

// Texture — brown noise breath
$: s("brown").begin(perlin).end(perlin.add(0.008))
  .hpf(2000).lpf(5000)
  .room(0.6).roomsize(0.8)
  .gain(0.04)

// Ghost hats
$: s("hh(3,8)").bank("RolandTR909")
  .gain(0.12).hpf(9000)
  .room(0.4).roomsize(0.6)
  .pan(rand)
```

---

### 5. 「 Concrete Garden 」

> *Organic sounds growing through industrial cracks. A pad like moss on steel, with dripping echoed percussion.*

**Key:** F minor · **BPM:** 122 · **Energy:** ●●○○○

```js
setcpm(122/4)

// Grounding pulse — soft kick
$: note("f1!4").s("sine")
  .penv(14).pdecay(.09).decay(.45).sustain(0)
  .gain(0.5)

// Dripping percussion — rimshot through long delay
$: s("rim(5,16)").bank("RolandTR909")
  .gain(0.2).pan(sine.range(0.2, 0.8).slow(4))
  .delay(0.6).delaytime(0.234).delayfeedback(0.7)
  .room(0.3)

// Moss pad — warm, green, humid
$: chord("<Fm Ab>").voicing()
  .s("triangle")
  .lpf(perlin.range(600, 1800).slow(24)).lpq(2)
  .attack(1).sustain(0.6).release(2)
  .room(0.5).roomsize(0.7)
  .gain(0.18)
```

---

## 🫀 Category II — Pulse & Heartbeat

*The kick is present and steady. Hypnotic, metronomic, meditative. The room starts to feel a pulse.*

---

### 6. 「 Basement Pulse 」

> *Four-on-the-floor in a concrete basement. Dry kick, crisp hats, and a sub-bass that vibrates through the floor.*

**Key:** C minor · **BPM:** 126 · **Energy:** ●●●○○

```js
setcpm(126/4)

// Kick — punchy, no frills
$: note("c1!4").s("sine")
  .penv(24).pdecay(.05).decay(.3).sustain(0)
  .distort("1:.6").gain(0.82)

// Hats — 16ths, accent pattern, dry
$: s("hh*16").bank("RolandTR909")
  .gain(".4 .2 .6 .2").hpf(7000)

// Sub-bass — pure, minimal motion
$: note("<c2!8 f2!8>").s("sine")
  .lpf(150).decay(0.3).sustain(0)
  .gain(0.5)
```

---

### 7. 「 Hypnotic Descent 」

> *A slow sinking feeling. The filter crawls downward over 32 bars while the kick stays perfectly steady.*

**Key:** D minor · **BPM:** 124 · **Energy:** ●●●○○

```js
setcpm(124/4)

// Kick — steady anchor
$: note("d1!4").s("sine")
  .penv(20).pdecay(.06).decay(.35).sustain(0)
  .gain(0.8)

// Hats — minimal, swung
$: s("hh*8").bank("RolandTR909")
  .gain(".3 .15 .25 .15").hpf(6500)
  .swingBy(1/6, 4)

// Clap — on the 2 and 4
$: s("~ cp ~ cp").bank("RolandTR909")
  .room(0.12).gain(0.55)

// Descending filtered bass — the slow sink
$: note("[d2 d2 <f2 a2> d2]*2").s("sawtooth")
  .lpf(saw.range(1200, 200).slow(32)).lpq(10)
  .ftype("ladder")
  .decay(0.18).sustain(0).release(0.03)
  .gain(0.45)
```

---

### 8. 「 Heart Monitor 」

> *Clinical. Precise. A single sine kick like a heartbeat, mechanical hats, and a chord that flatlines.*

**Key:** G minor · **BPM:** 128 · **Energy:** ●●○○○

```js
setcpm(128/4)

// The heartbeat — kick
$: note("g1!4").s("sine")
  .penv(18).pdecay(.07).decay(.4).sustain(0)
  .gain(0.75)

// Mechanical hats — perfectly even
$: s("hh*8").bank("RolandTR909")
  .gain(0.25).hpf(8000)

// Flatline chord — one sustained note cluster
$: note("[g3,bb3,d4]").s("sawtooth")
  .lpf(1000).lpq(1)
  .attack(0.8).sustain(0.6).release(2)
  .room(0.4).roomsize(0.6)
  .gain(0.15)

// Blip — medical device ping
$: note("g5").s("sine")
  .struct("~ ~ ~ x")
  .decay(0.08).sustain(0)
  .delay(0.3).delaytime(0.234).delayfeedback(0.4)
  .degradeBy(0.5).gain(0.12)
```

---

### 9. 「 Pendulum 」

> *Back and forth, back and forth. A swinging groove that leans slightly left, then right, then left again.*

**Key:** A minor · **BPM:** 126 · **Energy:** ●●●○○

```js
setcpm(126/4)

// Kick — steady
$: note("a1!4").s("sine")
  .penv(22).pdecay(.05).decay(.3).sustain(0)
  .gain(0.8)

// Swinging hats — heavy swing
$: s("hh*16").bank("RolandTR909")
  .gain(".5 .2 .4 .2").hpf(6500)
  .swingBy(1/4, 4)

// Panning bass — swings L to R
$: note("<a2!4 e2!4>").s("sawtooth")
  .lpf(500).lpq(8).ftype("ladder")
  .decay(0.2).sustain(0)
  .pan(sine.range(0.2, 0.8).slow(2))
  .gain(0.45)

// Clap — off-centre
$: s("~ cp ~ ~").bank("RolandTR909")
  .room(0.15).gain(0.5)
```

---

### 10. 「 Metronome Noir 」

> *A black-and-white metronome in a smoke-filled room. Precise, dark, unwavering.*

**Key:** C minor · **BPM:** 130 · **Energy:** ●●●○○

```js
setcpm(130/4)

// Kick — tight, sharp
$: note("c1!4").s("sine")
  .penv(30).pdecay(.04).decay(.25).sustain(0)
  .distort("1.2:.5").gain(0.85)

// Tick-tock rimshot — alternating pan
$: s("rim*4").bank("RolandTR909")
  .gain(0.22).pan("<0.3 0.7>")

// Dark chord — static, ominous
$: note("[c3,eb3,g3]").s("sawtooth")
  .lpf(800).lpq(3)
  .attack(0.3).sustain(0.5).release(1)
  .gain(0.15)
```

---

## 🌊 Category III — Deep Currents

*Sub-bass heavy, dubby, submerged. Sounds arrive through water. The groove is slow and inevitable.*

---

### 11. 「 Mariana 」

> *Named after the deepest point on Earth. You feel this one in your sternum. Sub-bass pressure, delay-drenched textures, and darkness.*

**Key:** C minor · **BPM:** 118 · **Energy:** ●●○○○

```js
setcpm(118/4)

// Deep kick — softened, round
$: note("c1!4").s("sine")
  .penv(14).pdecay(.09).decay(.5).sustain(0)
  .gain(0.7)

// Sub pressure — sine drone with slow beating
$: note("c1").s("sine")
  .superimpose(x => x.add(note(0.08)))
  .attack(2).sustain(0.8).release(3)
  .gain(0.2)

// Deep hat — filtered, reverbed
$: s("hh*8").bank("RolandTR909")
  .gain(".3 .15 .2 .15").hpf(7000)
  .room(0.4).roomsize(0.6)

// Submerged chord stab
$: note("<[c3,eb3,g3] [ab2,c3,eb3]>")
  .s("sawtooth")
  .lpf(600).lpq(3)
  .decay(0.1).sustain(0).release(0.05)
  .room(0.8).roomsize(0.9).roomlp(1500)
  .delay(0.5).delaytime(0.375).delayfeedback(0.6)
  .gain(0.25)
```

---

### 12. 「 Black Current 」

> *An underwater current pulling you sideways. Rolling bass, dubby percussion, and an undertow you can't escape.*

**Key:** F minor · **BPM:** 120 · **Energy:** ●●●○○

```js
setcpm(120/4)

// Kick — deep, round
$: note("f1!4").s("sine")
  .penv(16).pdecay(.08).decay(.45).sustain(0)
  .gain(0.75)

// Rolling bass — dubby
$: note("[f2 f2 <ab2 c3> f2]*2").s("sawtooth")
  .lpf(350).lpq(12).ftype("ladder")
  .lpenv(3).lpd(0.12).lps(0)
  .decay(0.2).sustain(0).release(0.03)
  .gain(0.5)

// Dubby rimshot — drenched
$: s("rim(3,8)").bank("RolandTR909")
  .gain(0.2).pan(rand)
  .delay(0.6).delaytime(0.1875).delayfeedback(0.7)
  .room(0.35).roomsize(0.5)

// Sparse hats
$: s("hh*8").bank("RolandTR909")
  .gain(".25 .12 .2 .12").hpf(7500)
```

---

### 13. 「 Abyss Stare 」

> *"When you gaze long into the abyss…" Deep sub, sparse metallic FM pings, and a reverb tail that never ends.*

**Key:** E minor · **BPM:** 120 · **Energy:** ●●○○○

```js
setcpm(120/4)

// Soft kick
$: note("e1!4").s("sine")
  .penv(12).pdecay(.1).decay(.5).sustain(0)
  .gain(0.55)

// Sub-bass drone
$: note("e1").s("sine")
  .attack(1.5).sustain(0.7).release(2)
  .gain(0.3)

// Metallic FM pings — sparse, random
$: note("<e5 ~ b4 ~ g5 ~ ~ ~>")
  .s("sine").fm(7).fmh(4.2)
  .fmdecay(0.1).fmsustain(0)
  .decay(1).sustain(0)
  .room(0.85).roomsize(0.95)
  .delay(0.4).delaytime(0.234).delayfeedback(0.65)
  .degradeBy(0.4).gain(0.1)

// Texture
$: s("crackle*2").density(0.03).gain(0.05)
```

---

### 14. 「 Tidal Lock 」

> *Locked in orbit. A bass pattern that repeats with gravitational inevitability, chords pulled along by its mass.*

**Key:** G minor · **BPM:** 122 · **Energy:** ●●●○○

```js
setcpm(122/4)

// Kick — steady orbit
$: note("g1!4").s("sine")
  .penv(20).pdecay(.06).decay(.35).sustain(0)
  .gain(0.78)

// Locked bass — repeating, heavy
$: note("[g2 g2 bb2 g2]*2").s("sawtooth")
  .lpf(400).lpq(10).ftype("ladder")
  .decay(0.18).sustain(0).release(0.03)
  .gain(0.5)

// Pulled chords — slow voice-led motion
$: chord("<Gm Cm Eb F>").voicing()
  .s("sawtooth")
  .lpf(sine.range(800, 2200).slow(16)).lpq(2)
  .attack(0.5).sustain(0.4).release(1.5)
  .room(0.5).roomsize(0.7)
  .gain(0.18).orbit(2)

// Hats — light texture
$: s("hh*8").bank("RolandTR909")
  .gain(".25 .12 .2 .12").hpf(7000)
```

---

### 15. 「 Benthic Zone 」

> *The ocean floor. No light reaches here. Only pressure, warmth from hydrothermal vents, and the deep pulse of the Earth.*

**Key:** D minor · **BPM:** 118 · **Energy:** ●○○○○

```js
setcpm(118/4)

// Pressure pulse — barely a kick
$: note("d1!4").s("sine")
  .penv(8).pdecay(.15).decay(.6).sustain(0)
  .lpf(60).gain(0.4)

// Hydrothermal texture — filtered noise, slow modulation
$: s("brown*2").begin(perlin).end(perlin.add(0.01))
  .bpf(sine.range(1000, 4000).slow(24)).bpq(3)
  .room(0.6).roomsize(0.8)
  .gain(0.06)

// Deep drone chord — almost infrasonic
$: note("[d2,f2,a2]").s("sine")
  .attack(3).sustain(0.8).release(4)
  .room(0.7).roomsize(0.85)
  .gain(0.2)

// Sparse ping — bioluminescence
$: note("<d5 ~ a4 ~ f5 ~ ~ ~>")
  .s("triangle")
  .decay(0.3).sustain(0)
  .delay(0.5).delaytime(0.375).delayfeedback(0.6)
  .room(0.5).roomsize(0.7)
  .degradeBy(0.5).gain(0.08)
```

---

## ⚡ Category IV — Tension & Edge

*Darker, moodier, more dissonant. These starters set a tone of unease, anticipation, or controlled menace. Use them when the room needs intensity without volume.*

---

### 16. 「 Surveillance 」

> *You're being watched. Cold percussion, a phrygian bass line that never resolves, and a chord that hovers like a drone overhead.*

**Key:** C phrygian · **BPM:** 126 · **Energy:** ●●●○○

```js
setcpm(126/4)

// Kick — clinical
$: note("c1!4").s("sine")
  .penv(28).pdecay(.04).decay(.28).sustain(0)
  .gain(0.82)

// Sharp hats — surveillance clicks
$: s("hh*16").bank("RolandTR909")
  .gain(".4 .15 .5 .15").hpf(8000)

// Phrygian bass — tense, unresolved
$: note("[c2 db2 <eb2 c2> db2]*2").s("sawtooth")
  .lpf(450).lpq(14).ftype("ladder")
  .lpenv(4).lpd(0.1).lps(0)
  .decay(0.15).sustain(0)
  .gain(0.5)

// Drone overhead — hovering chord
$: note("[c4,db4,g4]").s("sawtooth")
  .lpf(sine.range(600, 1400).slow(16)).lpq(3)
  .attack(0.5).sustain(0.5).release(1.5)
  .room(0.4).roomsize(0.5)
  .gain(0.13)
```

---

### 17. 「 Abandoned Factory 」

> *Empty concrete halls. Your footsteps echo. Machines hum in the distance. Something drips.*

**Key:** E minor · **BPM:** 124 · **Energy:** ●●○○○

```js
setcpm(124/4)

// Footstep kick — muted, echoing
$: note("e1!4").s("sine")
  .penv(16).pdecay(.08).decay(.4).sustain(0)
  .room(0.25).roomsize(0.4)
  .gain(0.6)

// Machine hum — distant, cyclical
$: note("e2").s("sawtooth")
  .lpf(sine.range(150, 400).slow(16)).lpq(6)
  .ftype("ladder")
  .attack(0.5).sustain(0.5).release(1)
  .gain(0.2)

// Dripping — delayed rimshot, sparse
$: s("~ ~ rim ~").bank("rolandcompurhythm78")
  .gain(0.2)
  .delay(0.6).delaytime(0.1875).delayfeedback(0.65)
  .room(0.5).roomsize(0.7)

// Dust texture
$: s("crackle*2").density(0.04).gain(0.06)
```

---

### 18. 「 Voltage 」

> *Live wires sparking in the dark. A buzzing bass, crackling hats, and a lead that arcs like electricity.*

**Key:** A minor · **BPM:** 128 · **Energy:** ●●●●○

```js
setcpm(128/4)

// Kick — hard, distorted edge
$: note("a1!4").s("sine")
  .penv(26).pdecay(.04).decay(.28).sustain(0)
  .distort("1.5:.5").gain(0.85)

// Crackling hats — bit-crushed
$: s("hh*16").bank("RolandTR909")
  .gain(".5 .2 .6 .2").hpf(6000)
  .crush(8)

// Buzzing bass — aggressive filter
$: note("[a2 a2 <c3 e3> a2]*2").s("sawtooth")
  .lpf(sine.range(250, 1200).slow(8)).lpq(15)
  .ftype("ladder")
  .decay(0.12).sustain(0)
  .distort("1:.5")
  .gain(0.5)

// Electric arc — sparse lead
$: n("<0 ~ 4 ~ 7 ~ ~ ~>")
  .scale("A3:minor:pentatonic").s("square")
  .lpf(3000).decay(0.06).sustain(0)
  .delay(0.5).delaytime(0.1875).delayfeedback(0.55)
  .gain(0.15)
```

---

### 19. 「 Phantom Limb 」

> *Something's missing and you can feel it. A groove with deliberate holes — the spaces where the beat should be are louder than the beat itself.*

**Key:** F minor · **BPM:** 126 · **Energy:** ●●●○○

```js
setcpm(126/4)

// Kick — with gaps (the "missing" beats)
$: note("<f1!4 [f1 ~ f1 f1] f1!4 [f1 f1 ~ f1]>").s("sine")
  .penv(20).pdecay(.06).decay(.35).sustain(0)
  .gain(0.78)

// Hats — filling the gaps
$: s("hh*16").bank("RolandTR909")
  .gain(".3 .15 .4 .15").hpf(7000)
  .degradeBy(0.2)

// Ghost notes — clap fragments
$: s("~ cp ~ ~, ~ ~ ~ cp").bank("RolandTR909")
  .gain(0.3).room(0.2).roomsize(0.35)
  .degradeBy(0.4)

// Phantom pad — appears and disappears
$: chord("<Fm ~ Ab ~>").voicing()
  .s("sawtooth")
  .lpf(1200).lpq(2)
  .attack(0.4).sustain(0.3).release(1)
  .room(0.5).roomsize(0.65)
  .gain(0.15)
```

---

### 20. 「 Night Architecture 」

> *The geometry of a city at night. Angular, precise, cold. Every element placed with mathematical intention.*

**Key:** D dorian · **BPM:** 128 · **Energy:** ●●●○○

```js
setcpm(128/4)

// Kick — precise
$: note("d1!4").s("sine")
  .penv(24).pdecay(.05).decay(.3).sustain(0)
  .gain(0.82)

// Angular percussion — Euclidean, panned
$: s("rim(7,16)").bank("RolandTR909")
  .gain(0.22).pan(rand)
  .iter(4)

// Dorian bass — the bright minor
$: note("[d2 d2 <e2 a2> d2]*2").s("sawtooth")
  .lpf(500).lpq(10).ftype("ladder")
  .decay(0.15).sustain(0)
  .gain(0.48)

// Cold chord — dorian voicing
$: note("<[d4,f4,a4] [c4,e4,g4]>")
  .s("triangle")
  .lpf(sine.range(1000, 2500).slow(16)).lpq(2)
  .attack(0.4).sustain(0.4).release(1.5)
  .room(0.3).roomsize(0.5)
  .gain(0.18)
```

---

## 🌀 Category V — Groove & Motion

*The dancefloor is starting to fill. These starters have more rhythmic complexity — polyrhythms, Euclidean patterns, swing, and enough bass to move bodies.*

---

### 21. 「 Clockwork 」

> *Interlocking gears. Every element is a cog — remove one and the whole machine stops. Euclidean percussion meshes with a rolling acid bass.*

**Key:** C minor · **BPM:** 128 · **Energy:** ●●●●○

```js
setcpm(128/4)

// Kick — the main gear
$: note("c1!4").s("sine")
  .penv(24).pdecay(.05).decay(.32).sustain(0)
  .distort("1.2:.6").gain(0.85)

// Hat gear — 16ths with accents
$: s("hh*16").bank("RolandTR909")
  .gain(".5 .2 .7 .2").hpf(6500)
  .swingBy(1/6, 4)

// Rim gear — Euclidean interlock
$: s("rim(5,16)").bank("RolandTR909")
  .gain(0.25).pan(rand)
  .delay(0.15).delaytime(0.1875).delayfeedback(0.3)

// Acid bass gear — rolling
$: note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
  .lpf(sine.range(200, 800).slow(8)).lpq(12)
  .ftype("ladder")
  .decay(0.15).sustain(0)
  .gain(0.5)

// Clap — on the 2 and 4
$: s("~ cp ~ cp").bank("RolandTR909")
  .room(0.12).gain(0.6)
```

---

### 22. 「 Tresillo 」

> *Named after the Cuban rhythm pattern. A 3-over-8 groove that makes straight techno feel like it's breathing.*

**Key:** G minor · **BPM:** 126 · **Energy:** ●●●●○

```js
setcpm(126/4)

// Kick — tresillo pattern (3,8)
$: note("g1(3,8)").s("sine")
  .penv(22).pdecay(.05).decay(.3).sustain(0)
  .gain(0.8)

// Straight hats — contrast with the kick
$: s("hh*16").bank("RolandTR909")
  .gain(".4 .2 .5 .2").hpf(7000)

// Clap — standard backbeat anchor
$: s("~ cp ~ cp").bank("RolandTR909")
  .room(0.1).gain(0.55)

// Syncopated bass — follows the tresillo
$: note("g2(3,8)").s("sawtooth")
  .lpf(500).lpq(8).ftype("ladder")
  .decay(0.2).sustain(0)
  .gain(0.48)

// Pad — slow harmonic bed
$: chord("<Gm Cm>").voicing()
  .s("sawtooth")
  .lpf(1800).lpq(1)
  .attack(0.5).sustain(0.5).release(1.5)
  .room(0.4).roomsize(0.6)
  .gain(0.15).orbit(2)
```

---

### 23. 「 Polyrhythm City 」

> *Multiple time signatures collide and somehow form a groove. The 5-against-16 rimshot vs the 3-against-8 clap creates a conversation.*

**Key:** A minor · **BPM:** 124 · **Energy:** ●●●●○

```js
setcpm(124/4)

// Kick — four on the floor (the anchor)
$: note("a1!4").s("sine")
  .penv(22).pdecay(.05).decay(.32).sustain(0)
  .gain(0.82)

// Hats — 16ths
$: s("hh*16").bank("RolandTR909")
  .gain(".4 .2 .5 .2").hpf(7000)

// Rim — 5 against 16 (polyrhythm)
$: s("rim(5,16)").bank("RolandTR909")
  .gain(0.25).pan(rand)
  .delay(0.12).delaytime(0.1875).delayfeedback(0.3)
  .iter(4)

// Clap — 3 against 8 (polyrhythm)
$: s("cp(3,8)").bank("RolandTR909")
  .room(0.15).gain(0.4)

// Bass — minimal, supportive
$: note("<a2 a2 e2 a2>").s("sine")
  .lpf(200).decay(0.3).sustain(0)
  .gain(0.45)
```

---

### 24. 「 Mercury 」

> *Liquid metal. Smooth, shapeshifting, impossibly fluid. A groovy bass that changes shape through filter modulation.*

**Key:** D minor · **BPM:** 126 · **Energy:** ●●●●○

```js
setcpm(126/4)

// Kick — smooth, rounded
$: note("d1!4").s("sine")
  .penv(18).pdecay(.07).decay(.38).sustain(0)
  .gain(0.8)

// Hats — flowing, swung
$: s("[hh hh hh hh hh hh hh <hh oh>]*2")
  .bank("RolandTR909")
  .gain(".4 .2 .6 .2 .4 .2 .8 .2")
  .hpf(6500).swingBy(1/6, 4).cut(1)

// Clap
$: s("~ <cp cp cp [cp cp]> ~ cp")
  .bank("RolandTR909").room(0.12).gain(0.55)

// Shapeshifting bass — heavy filter modulation
$: note("[d2 d2 <f2 a2> d2]*2").s("sawtooth")
  .lpf(sine.range(200, 1500).slow(4)).lpq(14)
  .ftype("ladder")
  .lpenv(5).lpa(0.005).lpd(0.12).lps(0)
  .decay(0.18).sustain(0).release(0.03)
  .gain(0.5)

// Sparse lead — delay-filled
$: n("<0 ~ 3 ~ 5 ~ ~ ~>")
  .scale("D4:minor:pentatonic").s("triangle")
  .decay(0.1).sustain(0)
  .delay(0.45).delaytime(0.1875).delayfeedback(0.55)
  .degradeBy(0.3).gain(0.15)
```

---

### 25. 「 Event Horizon 」

> *The point of no return. Everything is being pulled toward the drop — but we're not there yet. This is the last moment of calm before the set truly begins.*

**Key:** C minor · **BPM:** 128 · **Energy:** ●●●●○

```js
setcpm(128/4)

// Kick — full power
$: note("c1!4").s("sine")
  .penv(24).pdecay(.05).decay(.35).sustain(0)
  .distort("1.2:.6").gain(0.85)

// Hats — full 16ths with swing
$: s("[hh hh hh hh hh hh hh <hh oh>]*2")
  .bank("RolandTR909")
  .gain(".5 .3 .7 .3 .5 .3 1 .3")
  .hpf(6000).swingBy(1/6, 4).cut(1)

// Clap — backbeat
$: s("~ cp ~ cp").bank("RolandTR909")
  .room(0.12).roomsize(0.25).gain(0.6)

// Acid bass — the gravitational pull
$: note("<c2!8 f2!8>").s("sawtooth")
  .lpf(350).lpq(14)
  .lpenv(4).lpa(0.005).lpd(0.10).lps(0)
  .ftype("ladder")
  .decay(0.18).sustain(0).release(0.03)
  .gain(0.55)

// Pad — dark chords, sidechained
$: chord("<Cm Fm Ab Bb>").voicing()
  .s("sawtooth")
  .lpf(sine.range(1000, 2800).slow(16)).lpq(2)
  .attack(0.5).decay(1).sustain(0.5).release(1.5)
  .room(0.45).roomsize(0.65).roomlp(3000)
  .gain(0.2).orbit(2)

// Sidechain pump
$: s("bd*4").bank("RolandTR909")
  .duckorbit(2).duckattack(0.2).duckdepth(1).gain(0)
```

---

## 🎯 Set Flow — Playing Them in Order

If you want a complete warm-up set from these starters, here's a suggested flow that builds energy naturally over 30–45 minutes:

```
PHASE 1 — The Room Fills (0–10 min)
  → #1  Fog Signal          ●○○○○  (ambient)
  → #3  Dead Channel         ●○○○○  (textured)
  → #15 Benthic Zone         ●○○○○  (deep)

PHASE 2 — Bodies Start Moving (10–20 min)
  → #5  Concrete Garden      ●●○○○  (organic pulse emerges)
  → #11 Mariana              ●●○○○  (sub-bass arrives)
  → #17 Abandoned Factory    ●●○○○  (mood darkens)

PHASE 3 — The Groove Takes Hold (20–30 min)
  → #7  Hypnotic Descent     ●●●○○  (filter movement)
  → #12 Black Current        ●●●○○  (dubby groove)
  → #16 Surveillance         ●●●○○  (tension builds)

PHASE 4 — Ready for the Drop (30–45 min)
  → #22 Tresillo             ●●●●○  (rhythmic complexity)
  → #24 Mercury              ●●●●○  (full groove)
  → #25 Event Horizon        ●●●●○  (the handoff to your main set)
```

> **Transition tip:** When moving between starters, don't stop and paste the next one. Instead, start **commenting out** elements of the current starter (`_$:`) while **uncommenting** or adding elements from the next one. Morph, don't cut.

---

## 📊 Quick Reference — All 25 Starters

| # | Name | Key | BPM | Energy | Category | Elements |
|---|------|-----|-----|--------|----------|----------|
| 1 | Fog Signal | Cm | 120 | ●○○○○ | Fog | Sub + rim + pad + crackle |
| 2 | First Light | Dm | 122 | ●○○○○ | Fog | Kick + hats + rising pad |
| 3 | Dead Channel | Em | 118 | ●○○○○ | Fog | Kick + noise + pad + crackle |
| 4 | Interstellar Drift | Am | 120 | ●●○○○ | Fog | Sub + FM bell + noise + hats |
| 5 | Concrete Garden | Fm | 122 | ●●○○○ | Fog | Kick + rim + pad |
| 6 | Basement Pulse | Cm | 126 | ●●●○○ | Pulse | Kick + hats + sub |
| 7 | Hypnotic Descent | Dm | 124 | ●●●○○ | Pulse | Kick + hats + clap + bass |
| 8 | Heart Monitor | Gm | 128 | ●●○○○ | Pulse | Kick + hats + chord + blip |
| 9 | Pendulum | Am | 126 | ●●●○○ | Pulse | Kick + hats + bass + clap |
| 10 | Metronome Noir | Cm | 130 | ●●●○○ | Pulse | Kick + rim + chord |
| 11 | Mariana | Cm | 118 | ●●○○○ | Deep | Kick + sub + hats + stab |
| 12 | Black Current | Fm | 120 | ●●●○○ | Deep | Kick + bass + rim + hats |
| 13 | Abyss Stare | Em | 120 | ●●○○○ | Deep | Kick + sub + FM pings + crackle |
| 14 | Tidal Lock | Gm | 122 | ●●●○○ | Deep | Kick + bass + chords + hats |
| 15 | Benthic Zone | Dm | 118 | ●○○○○ | Deep | Kick + noise + drone + ping |
| 16 | Surveillance | C phryg | 126 | ●●●○○ | Tension | Kick + hats + bass + drone |
| 17 | Abandoned Factory | Em | 124 | ●●○○○ | Tension | Kick + hum + rim + crackle |
| 18 | Voltage | Am | 128 | ●●●●○ | Tension | Kick + hats + bass + lead |
| 19 | Phantom Limb | Fm | 126 | ●●●○○ | Tension | Kick + hats + clap + pad |
| 20 | Night Architecture | D dor | 128 | ●●●○○ | Tension | Kick + rim + bass + chord |
| 21 | Clockwork | Cm | 128 | ●●●●○ | Groove | Kick + hats + rim + bass + clap |
| 22 | Tresillo | Gm | 126 | ●●●●○ | Groove | Kick + hats + clap + bass + pad |
| 23 | Polyrhythm City | Am | 124 | ●●●●○ | Groove | Kick + hats + rim + clap + bass |
| 24 | Mercury | Dm | 126 | ●●●●○ | Groove | Kick + hats + clap + bass + lead |
| 25 | Event Horizon | Cm | 128 | ●●●●○ | Groove | Kick + hats + clap + bass + pad + duck |

---

*Every set starts with a single sound. These are yours.* 🌬️
