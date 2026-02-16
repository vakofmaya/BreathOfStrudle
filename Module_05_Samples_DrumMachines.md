# Module 5 — Samples & Drum Machines

> **Goal:** Learn how to use Strudel's massive library of recorded sounds (samples), navigate the built-in drum machine banks, load your own samples, and manipulate audio with slicing, chopping, and granular techniques.

---

## 5.1 Samples vs Synthesis — When to Use Which

In the previous modules you generated sound from scratch using oscillators. **Samples** are the other half of the equation — they're short recordings of real or pre-made sounds.

| | Synthesis | Samples |
|---|---|---|
| **Source** | Generated mathematically | Pre-recorded audio files |
| **Control** | Total — every parameter is tunable | Limited — you work with what's in the recording |
| **Character** | Clean, predictable | Organic, textured, "real" |
| **Use in Techno** | Bass, pads, leads, synth kicks | Drum hits, vocals, atmospherics, breaks |

In practice, most tracks **mix both**. A synthesized kick layered with a sample gives you control AND texture.

---

## 5.2 Default Samples — The Built-In Library

Strudel ships with hundreds of samples ready to use. The most common:

### Drum Hits

| Name | Sound |
|------|-------|
| `bd` | Bass drum / kick |
| `sd` | Snare drum |
| `hh` | Closed hi-hat |
| `oh` | Open hi-hat |
| `cp` | Clap |
| `mt` | Mid tom |
| `ht` | High tom |
| `lt` | Low tom |
| `rim` | Rimshot |
| `cb` | Cowbell |
| `cr` | Crash cymbal |
| `rd` | Ride cymbal |

### Your First Beat with Samples

```js
setcpm(128/4)
s("bd sd [~ bd] sd, hh*8, ~ cp ~ cp")
```

That's it — `s()` (or `sound()`) triggers samples by name.

### Exploring Available Sounds

Open the **Sounds tab** in the Strudel REPL to browse everything available. You'll see hundreds of entries. Click any entry to preview it.

```js
// Try some of the built-in instrument samples (from VCSL)
s("piano")
s("flute")
s("violin")
```

> 💡 **Exercise:** Open the Sounds tab in Strudel. Search for "bd" and see how many kick drum samples exist. Try `s("bd:0")`, `s("bd:1")`, `s("bd:2")` to hear variations.

---

## 5.3 Sample Variations — The `.n()` and `:` Selector

Most sample banks contain multiple variations. For example, `bd` might have 5 different kick sounds. Select them with `.n()` or the `:` mini-notation shorthand:

### Using `.n()`

```js
// Play the first 4 hi-hat variations
s("hh*4").n("0 1 2 3")
```

```js
// Random variation each hit
s("hh*8").n(irand(4))
```

### Using `:` in Mini-Notation

```js
// Inline selection — same as .n()
s("hh:0 hh:1 hh:2 hh:3")
```

```js
// Mixed in a beat
s("bd:0 sd:2 [~ bd:1] sd:0, hh:0 hh:1 hh:2 hh:3 hh:0 hh:1 hh:2 hh:3")
```

Numbers wrap around: if a bank has 4 samples (0–3), asking for `n(5)` gives you sample 1 (5 mod 4 = 1).

> 💡 **Exercise:** Write a hi-hat pattern with `s("hh*8")` and add `.n("0 1 2 3 0 1 2 3")` to cycle through variations. Then try `.n(irand(4))` for random variation.

---

## 5.4 Drum Machine Banks — `.bank()`

The default sample names (`bd`, `sd`, `hh`, etc.) are generic. Behind the scenes, Strudel has specific samples from classic drum machines. The `.bank()` function lets you pick which machine to use:

```js
// Roland TR-808 — deep, boomy (hip-hop, trap, classic house)
s("bd sd, hh*8").bank("RolandTR808")

// Roland TR-909 — punchy, snappy (THE techno drum machine)
s("bd sd, hh*8").bank("RolandTR909")

// Roland CR-78 — vintage, lo-fi
s("bd sd, hh*8").bank("RolandCR78")

// LinnDrum — 80s classic
s("bd sd, hh*8").bank("LinnDrum")
```

### How `.bank()` Works

Behind the scenes, `bank("RolandTR909")` prepends the machine name to the sample name with an underscore: `bd` becomes `RolandTR909_bd`. This works because all banks follow a standardized naming convention.

### Pattern the Bank

You can even change drum machines over time:

```js
// Alternate between 808 and 909 every cycle
s("bd sd, hh*8").bank("<RolandTR808 RolandTR909>")
```

```js
// Different machine every 4 cycles
s("bd sd [~ bd] sd, hh*8")
  .bank("<RolandTR808 RolandTR909 RolandCR78 LinnDrum>")
```

### Common Banks for Minimal Techno

| Bank | Character | Best For |
|------|-----------|----------|
| `RolandTR909` | Punchy, snappy, bright hats | **Primary techno kit** |
| `RolandTR808` | Deep, boomy, long decay | Sub kicks, claps, cowbell |
| `AkaiLinn` | Tight, compressed | Clean, tight beats |
| `RolandCR78` | Vintage, lo-fi | Retro, stripped-back |

### Sound Aliases

You can create your own shortcut names:

```js
// Create alias: "kick" → "RolandTR808_bd"
soundAlias('RolandTR808_bd', 'kick')
s("kick")
```

> 💡 **Exercise:**
> 1. Play `s("bd sd [~ bd] sd, hh*8, ~ cp ~ cp")` with `.bank("RolandTR909")`
> 2. Change to `.bank("RolandTR808")` — hear the difference
> 3. Try `.bank("<RolandTR909 RolandTR808>")` to alternate every cycle

---

## 5.5 Combining Banks and Variations

```js
setcpm(128/4)

// 909 kick with variation cycling
s("bd*4").bank("RolandTR909").n("<0 1 2 3>")

// 909 hihats alternating open/closed
s("[hh hh [hh oh] hh]*2").bank("RolandTR909").cut(1)
```

The `.cut(1)` above is a **cut group** — any sample in group 1 cuts off any other sample in group 1. This is how real drum machines work: an open hi-hat gets cut when a closed hi-hat plays.

```js
// Without cut — open hihat rings over closed hihat
s("[oh hh]*4").bank("RolandTR909")

// With cut — closed hihat mutes the open hihat (realistic)
s("[oh hh]*4").bank("RolandTR909").cut(1)
```

> 💡 **Exercise:** Build a realistic hi-hat pattern using `oh` and `hh` with `.cut(1)`. Try: `s("[hh hh hh oh hh hh hh oh]*2").bank("RolandTR909").cut(1)`

---

## 5.6 Loading Custom Samples

### From URL

Load any publicly hosted audio file:

```js
samples({ 
  rave: 'rave/AREUREADY.wav' 
}, 'github:tidalcycles/dirt-samples')

s("rave")
```

### From GitHub (Shortcut)

Strudel has a shortcut for GitHub-hosted sample packs:

```js
// Load the entire tidal dirt-samples library
samples('github:tidalcycles/dirt-samples')

// Now use any sample from it
s("breaks165")
```

### From a strudel.json File

If you have a folder of samples with a `strudel.json` mapping file:

```js
samples('https://your-server.com/samples/strudel.json')
```

### From Local Disk

Click **"Import Sounds Folder"** in the Strudel REPL to load samples from your computer directly.

---

## 5.7 Sample Playback Controls

### `begin` — Skip the Start

Trim the beginning of a sample (0 = start, 1 = end):

```js
samples('github:tidalcycles/dirt-samples')
s("rave").begin("<0 .25 .5 .75>").fast(2)
```

### `end` — Cut Off the End

```js
s("bd*2, oh*4").end("<.1 .2 .5 1>")
```

### `speed` — Playback Speed

Change the speed (and therefore pitch) of a sample:

```js
s("bd").speed("<1 1.5 2 0.5>")
// 1 = normal, 2 = double speed/octave up, 0.5 = half speed/octave down
// Negative values play in reverse
s("bd").speed("<1 -1>")
```

### `clip` / `legato` — Duration Control

Make the sample fill exactly its time slot:

```js
note("c a f e").s("piano").clip("<.5 1 2>")
```

### `loop` — Loop Playback

```js
s("casio").loop(1)
```

### `loopAt` — Sync Sample to Cycles

Make a long sample fit perfectly:

```js
samples('github:tidalcycles/dirt-samples')
s("breaks165").loopAt(2)  // fit sample into 2 cycles
```

### `fit` — Auto-fit to Event Duration

```js
samples('github:tidalcycles/dirt-samples')
s("breaks165/2").fit()
```

> 💡 **Exercise:** Load the dirt-samples library and try `s("breaks165").loopAt(1)` vs `.loopAt(2)` vs `.loopAt(4)`. Hear how the breakbeat gets stretched.

---

## 5.8 Slicing, Chopping & Granular Techniques

These are advanced sample manipulation techniques that turn long samples into rhythmic instruments.

### `chop` — Granular Slicing

Cuts a sample into N equal pieces and plays them in sequence:

```js
samples('github:tidalcycles/dirt-samples')

// Chop into 4 pieces
s("breaks165").chop(4).loopAt(2)

// Chop into 16 pieces and reverse
s("breaks165").chop(16).rev().loopAt(2)
```

### `striate` — Progressive Granular

Like `chop`, but each piece is from a progressively later part of the sample:

```js
s("numbers:0 numbers:1 numbers:2").striate(6).slow(3)
```

### `slice` — Chop + Resequence

This is the most powerful technique. Chop a sample into N slices, then play them in any order:

```js
samples('github:tidalcycles/dirt-samples')

// 8 slices, resequenced
s("breaks165")
  .slice(8, "0 1 <2 2*2> 3 [4 0] 5 6 7")
  .slow(0.75)
```

The second argument is a **pattern of slice numbers** — you're resequencing the beat!

```js
// Rearranged beat with every-3rd-cycle variation
s("breaks165")
  .slice(8, "0 1 <2 2*2> 3 [4 0] 5 6 7".every(3, rev))
  .slow(0.75)
```

### `splice` — Slice + Auto-Stretch

Like `slice`, but each slice is time-stretched to fill its pattern slot:

```js
samples('github:tidalcycles/dirt-samples')
s("breaks165")
  .splice(8, "0 1 [2 3 0]@2 3 0@2 7")
```

### Custom Slice Points

Instead of dividing evenly, slice at specific points:

```js
samples('github:tidalcycles/dirt-samples')
s("breaks125").fit()
  .slice([0, .25, .5, .75], "0 1 1 <2 3>")
```

> 💡 **Exercise:**
> 1. Load `samples('github:tidalcycles/dirt-samples')`
> 2. Try `s("breaks165").slice(8, "0 1 2 3 4 5 6 7")`  — normal order
> 3. Now rearrange: `s("breaks165").slice(8, "0 0 3 2 4 4 7 6")` — chopped!
> 4. Add `.every(4, rev)` to reverse every 4th cycle

---

## 5.9 Effects on Samples

Everything from Modules 3 and 4 applies to samples too — filters, envelopes, effects:

```js
setcpm(128/4)

// Filtered 909 drums
s("bd sd [~ bd] sd, hh*8").bank("RolandTR909")
  .lpf(3000)

// Hi-hats with HPF (remove low rumble)
s("hh*16").bank("RolandTR909")
  .hpf(6000)
  .gain(".5 .3 .8 .3")

// Reverb and delay on clap
s("~ cp ~ cp").bank("RolandTR909")
  .room(0.2).roomsize(0.3)
  .delay(0.1).delaytime(0.05)

// Distorted kick
s("bd*4").bank("RolandTR909")
  .distort("2:.5")
```

---

## 5.10 Practical Drum Patterns for Minimal Techno

### Pattern 1: Classic 4/4

```js
setcpm(128/4)
s("bd*4, ~ cp ~ cp, [hh hh hh oh]*2")
  .bank("RolandTR909").cut(1)
```

### Pattern 2: Syncopated

```js
setcpm(128/4)
s("bd ~ [~ bd] ~, ~ cp ~ [cp ~], hh*16")
  .bank("RolandTR909")
  .gain("1 1 1 1, 1 1 1 1, .5 .3 .7 .3")
```

### Pattern 3: Euclidean Polyrhythm

```js
setcpm(128/4)
s("bd(3,8), cp(2,5), hh(7,16), rim(5,12)")
  .bank("RolandTR909")
  .gain("0.8, 0.6, 0.4, 0.3")
```

### Pattern 4: Layered Machines

```js
setcpm(128/4)
$: s("bd*4").bank("RolandTR909")
$: s("~ cp ~ cp").bank("RolandTR808").room(0.1)
$: s("hh*16").bank("RolandTR909").gain(".5 .3 .7 .3").hpf(6000)
$: s("rim(5,16)").bank("RolandCR78").gain(0.3).pan(rand)
```

### Pattern 5: With Texture

```js
setcpm(128/4)
$: s("bd*4").bank("RolandTR909")
$: s("hh*16").bank("RolandTR909").gain(".5 .3 .8 .3").swingBy(1/6, 4)
$: s("~ cp ~ cp").bank("RolandTR909").room(0.15)
$: s("rim(5,16)").bank("RolandTR909").gain(0.3).delay(0.2).delaytime(0.1875)
$: s("crackle*2").density(0.03).gain(0.1)  // vinyl texture
```

---

## 5.11 Practice Challenges

### Challenge 1: Drum Machine Comparison
Write the same beat with three different banks. Listen to each and pick your favorite for a minimal techno context.

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
s("bd sd [~ bd] sd, hh*8, ~ cp ~ cp")
  .bank("<RolandTR909 RolandTR808 RolandCR78>")
```
</details>

### Challenge 2: Hi-Hat Humanization
Create a 16th-note hi-hat pattern with:
- Accent pattern (louder on beats)
- Occasional open hi-hat
- Cut group so open hat gets muted by closed

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
s("[hh hh hh <hh oh>]*4").bank("RolandTR909")
  .gain(".8 .3 .5 .3 .8 .3 .5 .3 .8 .3 .5 .3 .8 .3 .5 .3")
  .cut(1)
```
</details>

### Challenge 3: Breakbeat Remix
Load a breakbeat, slice it into 8 pieces, and create a new pattern from the slices:

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
samples('github:tidalcycles/dirt-samples')
s("breaks165")
  .slice(8, "0 0 [3 2] 1 4 [6 5] 7 <3 0>")
  .loopAt(2)
```
</details>

### Challenge 4: Full Drum Kit
Build a complete drum kit using only 909 samples with at least:
- Kick, snare, closed hat, open hat (with cut group), rimshot

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
$: s("bd*4").bank("RolandTR909")
$: s("~ sd ~ sd").bank("RolandTR909")
$: s("[hh hh hh oh hh hh hh oh]*2").bank("RolandTR909").cut(1).gain(".6 .3 .5 .8")
$: s("rim(5,16)").bank("RolandTR909").gain(0.3)
```
</details>

---

## 5.12 Key Takeaways

1. `s("name")` triggers samples — `bd`, `sd`, `hh`, `oh`, `cp`, `rim`, etc.
2. `.bank("RolandTR909")` selects a drum machine — **909 is the techno standard**
3. `.n()` or `:N` selects sample variations within a bank
4. `.cut(N)` creates mute groups (essential for open/closed hi-hat)
5. `samples()` loads external sample packs from URLs or GitHub
6. `begin`, `end`, `speed` control basic playback
7. `loopAt` and `fit` sync long samples to cycles
8. `chop`, `slice`, `splice` enable granular and resequencing techniques
9. All synth effects (filters, reverb, delay, distortion) work on samples too
10. Mix sample banks freely — use 909 kick + 808 clap + CR78 rim

---

**Previous:** [← Module 4 — Subtractive Synthesis](./Module_04_Subtractive_Synthesis.md)
**Next:** [Module 6 — Envelopes & Dynamics →](./Module_06_Envelopes_Dynamics.md)
