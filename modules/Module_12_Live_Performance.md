# Module 12 — Live Performance Techniques

> **Goal:** Put everything together and prepare for live coding performance. Learn performance workflows, real-time modification techniques, MIDI/OSC integration, visual feedback, and build your own performance template. This module turns you from a producer into a performer.

---

## 12.1 What is Live Coding?

Live coding means **writing and modifying code in real time** while the audience listens. The music emerges from an ongoing conversation between you and the machine. Every change you make is heard immediately.

In Strudel:
- You write patterns in the REPL
- Press **Ctrl+Enter** to evaluate → changes take effect on the next cycle boundary
- The audience hears the transition seamlessly
- You can modify parameters, add elements, remove elements, swap sounds — all live

### Why Live Code Techno?

- **Full transparency** — the audience sees exactly what creates the sound
- **Improvisation** — no two performances are the same
- **Constraint-driven creativity** — you create within the moment's constraints
- **Minimal Techno's repetitive nature** is perfect for gradual live modification

---

## 12.2 Essential Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| **Ctrl+Enter** | Evaluate all code |
| **Ctrl+.** | **Hush** — stop all sound instantly |

The two most important:
1. **Ctrl+Enter** — your "play/update" button. Every change you make takes effect when you press this.
2. **Ctrl+.** — your emergency stop. Silences everything immediately.

> **Performance tip:** Practice hitting Ctrl+. instantly. If something goes wrong during a performance, this is your safety button.

---

## 12.3 The `$:` Pattern System

The `$:` syntax is the backbone of live performance. Each `$:` pattern runs independently:

```js
setcpm(128/4)

$: s("bd*4").bank("RolandTR909")
$: s("hh*16").bank("RolandTR909").gain(".5 .3 .8 .3")
$: s("~ cp ~ cp").bank("RolandTR909").room(0.15)
```

### Key Properties

1. **Independent evaluation** — change one line and re-evaluate; others continue unchanged
2. **Pattern identity** — each `$:` label remembers its pattern; re-evaluating replaces it smoothly
3. **Seamless transition** — the new pattern starts on the next cycle boundary, no clicks or gaps

### Named Patterns with `$name:`

For more control, name your patterns:

```js
$kick: s("bd*4").bank("RolandTR909")
$hats: s("hh*16").bank("RolandTR909").gain(0.4)
$clap: s("~ cp ~ cp").bank("RolandTR909")
$bass: note("c2 ~ <eb2 f2> ~").s("sine").lpf(200)
```

---

## 12.4 Performance Workflow

### Phase 1: Preparation

Before you start performing, prepare your code — but don't evaluate it all at once.

```js
setcpm(128/4)

// ============ DRUMS ============
$: s("bd*4").bank("RolandTR909")
// $: s("hh*16").bank("RolandTR909").gain(".5 .3 .8 .3").hpf(6000)
// $: s("~ cp ~ cp").bank("RolandTR909").room(0.15)
// $: s("rim(5,16)").bank("RolandTR909").gain(.3).pan(rand)

// ============ BASS ============
// $: note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
//   .lpf(400).lpq(12).lpenv(4).lpd(.12).lps(0)
//   .ftype("ladder").decay(.2).sustain(0).gain(.55)

// ============ HARMONY ============
// $: note("<[c4,eb4,g4] [bb3,d4,f4]>").s("sawtooth")
//   .lpf(2000).attack(.3).release(1).room(.4).gain(.25).orbit(2)

// ============ SIDECHAIN ============
// $: s("bd*4").bank("RolandTR909")
//   .duckorbit(2).duckattack(.2).duckdepth(1).gain(0)

// ============ MELODY ============
// $: n("<0 ~ 3 ~ 5 ~ 7 ~>*2").scale("C4:minor:pentatonic")
//   .s("triangle").decay(.15).sustain(0)
//   .delay(.4).delaytime(.1875).delayfeedback(.5).gain(.2)

// ============ TEXTURE ============
// $: s("crackle*2").density(.03).gain(.1)
```

### Phase 2: Building Up

Evaluate the kick first (Ctrl+Enter with cursor on that line):

```js
$: s("bd*4").bank("RolandTR909")
```

Wait 4–8 bars, then uncomment and evaluate the hats:

```js
$: s("hh*16").bank("RolandTR909").gain(".5 .3 .8 .3").hpf(6000)
```

Continue adding elements one by one, leaving several bars between additions.

### Phase 3: Full Groove

When all elements are playing, start **modifying** them in real time:

```js
// Change the bass notes
$: note("[c2 c2 <f2 g2> c2]*2").s("sawtooth")  // ← changed eb to f and g

// Change the hi-hat pattern
$: s("hh*16").bank("RolandTR909").gain(".5 .3 .8 .3 .5 .3 1 .3")  // ← new accent

// Add filter sweep to the pad
$: note("<[c4,eb4,g4] [bb3,d4,f4]>").s("sawtooth")
  .lpf(sine.range(800, 3000).slow(8))  // ← ADDED MOVEMENT
  .attack(.3).release(1).room(.4).gain(.25).orbit(2)
```

### Phase 4: Breakdown

Comment out or silence elements for a breakdown:

```js
// Silence the kick (replace with rest)
$: s("~")   // was: s("bd*4")...

// Or comment out and re-evaluate
// $: s("bd*4").bank("RolandTR909")
```

### Phase 5: Drop

Bring elements back with extra energy:

```js
// Kick returns
$: s("bd*4").bank("RolandTR909")

// Hats with more variations
$: s("hh*16").bank("RolandTR909")
  .gain(".5 .3 .8 .3")
  .sometimes(x => x.speed(1.5))  // ← NEW: pitch variation
  .lastOf(4, x => x.ply(2))     // ← NEW: fills every 4th bar
```

### Phase 6: Ending

Strip back and fade:

```js
// Remove everything except kick
// (comment out other $: lines)

// Fade the kick
$: s("bd*4").bank("RolandTR909")
  .gain(saw.range(0.8, 0).slow(16))

// Then silence
// hush
```

---

## 12.5 Real-Time Modification Techniques

These are your performance "moves" — things you can change mid-set:

### Hot-Swap Sounds

```js
// Change drum machine mid-performance
$: s("bd sd, hh*8").bank("RolandTR909")
// →
$: s("bd sd, hh*8").bank("RolandTR808")
```

### Live Filter Sweeps

```js
// Static cutoff
$: note("c2!4").s("sawtooth").lpf(400)

// → Change to swept cutoff
$: note("c2!4").s("sawtooth").lpf(sine.range(200, 4000).slow(8))
```

### Pattern Transformation

```js
// Straight beat
$: s("bd sd [~ bd] sd")

// → Add iter for rotation
$: s("bd sd [~ bd] sd").iter(4)

// → Add reverse every 4th cycle
$: s("bd sd [~ bd] sd").iter(4).lastOf(4, rev)
```

### Adding Fills

```js
// Normal hats
$: s("hh*8").gain(".5 .3 .8 .3")

// → With fill every 4th bar
$: s("hh*8").gain(".5 .3 .8 .3").lastOf(4, x => x.ply(2))

// → With random speed variations
$: s("hh*8").gain(".5 .3 .8 .3").sometimes(x => x.speed(1.5))
```

### Changing Key / Progression

```js
// Original
$: chord("<Cm Fm>").voicing()

// → Change progression
$: chord("<Cm Bbm Ab G>").voicing()

// → Transpose everything up
$: chord("<Cm Bbm Ab G>").voicing().transpose(2)
```

### Adding/Removing Sidechain

```js
// Add pump
$: s("bd*4").duckorbit(2).duckattack(.2).duckdepth(1).gain(0)

// Remove pump (just delete the line or silence it)
// $: s("bd*4").duckorbit(2)...
```

### Crash Cymbal Accents

Drop a one-shot crash for transitions:

```js
// Play once then remove
$: s("cr").bank("RolandTR909")
// (after it plays, comment it out)
```

---

## 12.6 Muting & Hush Techniques

### Hush — Stop Everything

```js
hush
```

Or press **Ctrl+.** (the fastest way).

### Mute Individual Patterns

```js
// Method 1: Replace sound with silence
$: s("~")  // was: s("bd*4")

// Method 2: Comment out and re-evaluate
// $: s("bd*4").bank("RolandTR909")

// Method 3: Set gain to 0
$: s("bd*4").bank("RolandTR909").gain(0)
```

### Mute with a Fade

```js
// Fade out over 8 cycles
$: s("hh*16").bank("RolandTR909")
  .gain(saw.range(0.5, 0).slow(8))
```

---

## 12.7 MIDI Output

Send your patterns to external hardware synths or DAWs:

### Basic MIDI Output

```js
// Send notes to first available MIDI device
note("c3 eb3 g3 bb3").midi()
```

### Specific MIDI Device

```js
// Send to a named MIDI output
note("c3 eb3 g3 bb3").midi("IAC Driver")
```

### MIDI Channel

```js
// Send on MIDI channel 2
note("c3 eb3 g3 bb3").midi().midichan(2)
```

### MIDI CC (Control Change)

Automate hardware parameters remotely:

```js
// Send CC #74 (cutoff on most synths) with a sine LFO
ccn(74).ccv(sine.segment(16).range(0, 127)).midi()
```

### MIDI Clock

```js
// Send MIDI clock so hardware syncs to Strudel's tempo
midi_clock().midi()
```

### Example: MIDI + Internal Sounds

```js
setcpm(128/4)

// Internal drums
$: s("bd*4, hh*8, ~ cp ~ cp").bank("RolandTR909")

// External hardware bass via MIDI
$: note("[c2 c2 <eb2 f2> c2]*2").midi("USB MIDI Device")

// External hardware pad via MIDI on channel 2
$: chord("<Cm Fm>").voicing().midi("USB MIDI Device").midichan(2)
```

---

## 12.8 OSC Output

Send patterns via OSC (Open Sound Control) to SuperCollider, Max/MSP, or other software:

```js
// Send OSC messages
note("c3 eb3 g3 bb3").osc()
```

---

## 12.9 Live Audio Input — `audioin()`

Strudel can process **live audio** from your microphone or audio interface, enabling hybrid performances where you run real instruments through Strudel's pattern engine:

```js
// Process live input through Strudel's effects
audioin().lpf(sine.range(200, 4000).slow(8)).room(0.3)

// Rhythmic gating of live input
audioin().struct("x ~ x ~ x ~ [x x] ~").gain(0.8)
```

> [!NOTE]
> Your browser will ask for microphone permission. This works best with an external audio interface. For live performance, connect a synth, guitar, or microphone and process it through Strudel's filters, effects, and pattern engine in real time.

---

## 12.10 Visual Feedback

### Oscilloscope — `._scope()`

Shows real-time waveform visualization:

```js
note("c2").s("sawtooth").lpf(1000)._scope()
```

### Pianoroll Visualization

Strudel's REPL shows a pianoroll-style visualization by default, showing event placement and pitch.

### Performance Visibility Tips

1. **Use `._scope()`** on your main bass or pad to give visual feedback
2. **Font size** — increase editor font size so the audience can read your code
3. **Clean code** — format your patterns neatly, one per line
4. **Comments** — add section labels so you (and the audience) know what's what

---

## 12.11 Advanced Performance Concepts

### Pre-Mapped Variations

Prepare multiple variations of a pattern and swap between them:

```js
// Version A (straight)
$: s("bd*4").bank("RolandTR909")

// Version B (syncopated) — swap in when ready
$: s("bd ~ [~ bd] ~").bank("RolandTR909")

// Version C (half-time) — swap in for breakdown
$: s("bd ~ ~ ~").bank("RolandTR909")
```

### Gradual Complexity

Start simple and add transformation layers:

```js
// Level 1: raw
$: s("hh*8")

// Level 2: accented
$: s("hh*8").gain(".5 .3 .8 .3")

// Level 3: + swing
$: s("hh*8").gain(".5 .3 .8 .3").swingBy(1/6, 4)

// Level 4: + random variation
$: s("hh*8").gain(".5 .3 .8 .3").swingBy(1/6, 4).sometimes(x => x.speed(1.5))

// Level 5: + conditional fill
$: s("hh*8").gain(".5 .3 .8 .3").swingBy(1/6, 4)
  .sometimes(x => x.speed(1.5)).lastOf(4, x => x.ply(2))
```

### Error Recovery

Things will go wrong during a performance. Here's how to handle it:

| Problem | Solution |
|---------|----------|
| Code error (syntax) | Fix the typo, re-evaluate. The previous pattern keeps playing until you evaluate successfully. |
| Everything sounds wrong | Press **Ctrl+.** (hush). Pause. Fix. Re-evaluate from your known-good template. |
| One element sounds bad | Replace that `$:` line with `$: s("~")` to mute it while you fix. |
| Lost track of what's playing | Comment out everything, `Ctrl+Enter`, start fresh from kick. |
| CPU overload | Reduce pattern complexity: fewer `*16`, less reverb, fewer orbits. |

### Performance Endurance

To sustain a 30–60 minute set:

1. **Have templates ready** — don't start from a blank screen
2. **Know your patterns** — practice transitions offline
3. **Keep a "reset" state** — a simple, known-good configuration you can always return to
4. **Use sections** — plan approximate timing: 10 min intro/build, 30 min main, 10 min breakdown/drop, 10 min outro
5. **Stay calm** — the audience doesn't hear mistakes as clearly as you do

---

## 12.11 Complete Performance Template

Here's a ready-to-use template. Copy this into Strudel before your performance:

```js
// ╔═══════════════════════════════════════════════════╗
// ║       MINIMAL TECHNO — LIVE SET TEMPLATE          ║
// ║       128 BPM  |  Key: C minor                    ║
// ╚═══════════════════════════════════════════════════╝

setcpm(128/4)

// ═══════════════ DRUMS ═══════════════

$: s("bd*4").bank("RolandTR909")

// $: s("hh*16").bank("RolandTR909")
//   .gain(".5 .3 .8 .3")
//   .hpf(6000)
//   .swingBy(1/6, 4)

// $: s("~ cp ~ cp").bank("RolandTR909")
//   .room(0.15)

// $: s("rim(5,16)").bank("RolandTR909")
//   .gain(0.3).pan(rand)
//   .delay(0.2).delaytime(0.1875)

// ═══════════════ BASS ═══════════════

// $: note("[c2 c2 <eb2 f2> c2]*2").s("sawtooth")
//   .lpf(400).lpq(12)
//   .lpenv(4).lpa(.005).lpd(.12).lps(0)
//   .ftype("ladder")
//   .decay(.2).sustain(0).release(.05)
//   .gain(0.55)

// ═══════════════ HARMONY ═══════════════

// $: chord("<Cm7 Fm Abmaj7 G7>").voicing()
//   .s("sawtooth")
//   .lpf(sine.range(1200, 2500).slow(16))
//   .lpq(2)
//   .attack(.4).sustain(.5).release(1)
//   .room(.5).roomsize(.7)
//   .gain(0.25)
//   .orbit(2)

// ═══════════════ SIDECHAIN ═══════════════

// $: s("bd*4").bank("RolandTR909")
//   .duckorbit(2)
//   .duckattack(0.2)
//   .duckdepth(1)
//   .gain(0)

// ═══════════════ MELODY ═══════════════

// $: n("<0 ~ 3 ~ 5 ~ 7 ~>*2")
//   .scale("C4:minor:pentatonic")
//   .s("triangle")
//   .decay(.15).sustain(0)
//   .delay(.4).delaytime(.1875).delayfeedback(.5)
//   .degradeBy(0.3)
//   .gain(0.2)

// ═══════════════ TEXTURE ═══════════════

// $: s("crackle*2").density(.03).gain(.1)
// $: s("brown").begin(rand).end(rand.add(.01))
//   .hpf(2000).gain(.04).room(.8).roomsize(.9)

// ═══════════════ PERFORMANCE TOOLS ═══════════════

// FILL: uncomment, evaluate, then comment back out
// $: s("cr").bank("RolandTR909")

// RISER: uncomment for builds
// $: note(saw.range(40, 72).segment(32).slow(8))
//   .s("sine").gain(0.15)

// SNARE ROLL: uncomment for builds
// $: s("sd").fast(saw.range(1, 16).slow(4))
//   .bank("RolandTR909")
//   .gain(saw.range(0, 0.5).slow(4))
```

### How to Use This Template

1. **Before the set:** Load the template. Evaluate just the kick line and `setcpm`.
2. **Build up:** Uncomment lines one by one from top to bottom. Evaluate after each.
3. **Modify:** While the groove is running, change values — filter cutoffs, note patterns, add `.sometimes()`, etc.
4. **Breakdown:** Comment out kick + bass. Let pad and melody play alone. Uncomment the riser.
5. **Drop:** Bring back kick + bass. Comment out the riser. Add new modifiers for extra energy.
6. **Outro:** Comment out elements from bottom to top. Fade the kick with `.gain(saw.range(0.8, 0).slow(16))`.
7. **End:** `hush` or Ctrl+.

---

## 12.12 Practice Challenges

### Challenge 1: 5-Minute Solo ⭐⭐
Time yourself for 5 minutes. Start with just a kick. Build up to a full groove by evaluating patterns one at a time. Practice creating a breakdown and bringing the energy back.

### Challenge 2: Sound Swap ⭐
While a full groove is playing, swap the drum machine from TR-909 to TR-808 in real time. Then swap back. Do it smoothly with no dead air.

### Challenge 3: Live Filter Performance ⭐⭐
While a bass is playing, manually change the `.lpf()` value and re-evaluate to create a "filter sweep":
- Start at `lpf(200)`
- Change to `lpf(400)`, evaluate
- Change to `lpf(800)`, evaluate
- Change to `lpf(1600)`, evaluate
- Back down: `lpf(800)`, `lpf(400)`, `lpf(200)`

### Challenge 4: Error Recovery ⭐
While performing, intentionally introduce a typo. Practice using Ctrl+. to hush, fixing the error, and smoothly restarting.

### Challenge 5: Record a Set ⭐⭐⭐
Use your browser's audio recording tools (or screen recording) to capture a 10-minute live coded set. Listen back and identify areas for improvement.

---

## 12.13 Performance Checklist

Use this before every performance:

- [ ] **Template loaded** and working
- [ ] **Tempo set** (`setcpm(128/4)`)
- [ ] **Audio output** tested (speakers, headphones, DI box)
- [ ] **Browser tab** on Strudel REPL (nothing else loading)
- [ ] **Font size** large enough to read (and for audience if projected)
- [ ] **Fallback plan** — know how to restart from scratch
- [ ] **MIDI devices** connected and tested (if using hardware)
- [ ] **Volume levels** right (kick at ~0.8, nothing clipping)
- [ ] **Practice transitions** — build up, breakdown, drop at least once
- [ ] **Emergency hush** — Ctrl+. working? Tested!

---

## 12.14 Key Takeaways

1. **`$:` patterns** are your instrument — each line is independently modifiable
2. **Ctrl+Enter** to evaluate, **Ctrl+.** to hush — practice these reflexes
3. Build up **gradually** — one element every 4–8 bars
4. Performance moves: **hot-swap sounds**, **add/remove modifiers**, **filter sweeps**, **pattern transforms**
5. Break down by **commenting out** elements → bring back for the drop
6. **Prepare templates** — never start from a blank screen in a performance
7. **MIDI output** with `.midi()` for hardware integration
8. Use `._scope()` for **visual feedback**
9. **Error recovery** — the old pattern keeps playing until you successfully evaluate a new one
10. **Practice** — live coding is a skill that gets better with repetition

---

## What's Next?

**You've completed the course!** 🎉

You now have everything you need to:
- Design any sound in Strudel (synths, samples, effects)
- Write chords, melodies, and bass lines
- Build complete track arrangements
- Perform live

Go to the **[Final Project — Building a Complete Minimal Techno Track](./Final_Project_Complete_Track.md)** to put everything from all 12 modules together into a complete track.

Then **make it your own.** Change the notes, swap the sounds, modify the groove — this is YOUR music now.

---

**Previous:** [← Module 11 — Structuring a Full Track](./Module_11_Track_Structure.md)
**Course README:** [← Back to Course Overview](../README.md)
