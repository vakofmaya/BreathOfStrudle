# Module 1 — The Strudel Environment

> **Goal:** Set up your workspace, understand what Strudel is, navigate the interface, and make your very first sounds. By the end of this module, you'll have a working beat playing in your browser.

---

## 1.1 What is Strudel?

**Strudel** is a free, browser-based live coding environment for making music. You write code, press a key, and hear music — instantly. No downloads, no plugins, no accounts.

It's a JavaScript port of [TidalCycles](https://tidalcycles.org/), a pattern language created by Alex McLean, built on top of the [WebAudio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API). Everything runs in your browser — the sounds are synthesized and played right there.

### Why Strudel?

| Feature | What It Means for You |
|---------|---------------------|
| **Browser-based** | Nothing to install. Open a tab and you're ready. |
| **Free & open source** | No cost, ever. The code is open on [GitHub](https://github.com/tidalcycles/strudel). |
| **Pattern-based** | You describe *what* should happen, not *when*. The system handles the timing. |
| **Live coding** | Changes take effect in real time. Write code → hear music → edit → hear changes. |
| **Built-in sounds** | Hundreds of samples and synthesizers included. |
| **Runs anywhere** | Works on any modern browser — laptop, tablet, even phones (though a keyboard is recommended). |

### How It Relates to Other Tools

```
SuperCollider (audio engine)
    ↓ inspired
TidalCycles (Haskell pattern language)
    ↓ ported to JavaScript
Strudel (browser-based, no install)  ← You are here
```

Strudel shares TidalCycles' pattern language but runs entirely in JavaScript/WebAudio. If you learn Strudel, you'll understand TidalCycles too — and vice versa.

---

## 1.2 Opening Strudel

### Step 1: Open the REPL

Go to **[strudel.cc](https://strudel.cc/)** in your browser.

> [!TIP]
> **Use a Chromium-based browser** (Chrome, Brave, Edge) for best audio performance. Firefox works but may have higher latency.

### Step 2: Allow Audio

When you first load the page, you'll see an editor with some example code. Click **Play** or press **Ctrl+Enter** to start audio. Your browser will ask permission to play sound — allow it.

### Step 3: Hear Your First Sound

The default example might be something like:

```js
s("bd sd")
```

This plays a bass drum (`bd`) followed by a snare (`sd`), repeating forever in a cycle.

> 🎧 **Put on headphones.** You'll hear low frequencies (bass drums, sub-bass) much more clearly than through laptop speakers.

---

## 1.3 The Interface

Strudel's interface is minimal by design. Here's what you see:

### The Editor (center)

This is where you write code. It's a text editor with syntax highlighting. Your patterns go here.

### The Toolbar (top)

| Button/Area | What It Does |
|------------|-------------|
| **Play / Stop** | Start or stop the audio engine |
| **Sounds** | Browse all available sample banks and synth names |
| **Share** | Generate a shareable URL of your current code |
| **Settings** | Adjust latency, volume, and other preferences |

### Visual Feedback (in the editor)

By default, Strudel provides **mini-notation highlighting** — as your pattern plays, the active parts of your mini-notation strings light up in the editor, so you can see which elements are triggering in real time. This is enabled automatically; no extra code needed.

> [!TIP]
> **Want a pianoroll?** It's not on by default, but you can add one with `.pianoroll()` (renders in the page background) or `._pianoroll()` (renders inline below your code). There's also `.punchcard()` / `._punchcard()` which is similar but reflects downstream transformations. See [Visual Feedback docs](https://strudel.cc/learn/visual-feedback/) for all options.
>
> ```js
> s("bd sd hh cp").pianoroll()
> ```

### Settings Worth Knowing

Click **Settings** (gear icon) and check:

- **Latency** — lower = more responsive but may cause glitches. Start with the default.
- **Volume** — the master output level. Start at a comfortable level.

---

## 1.4 Essential Keyboard Shortcuts

These two shortcuts are everything you need to start:

| Shortcut | What It Does | When to Use |
|----------|-------------|-------------|
| **Ctrl+Enter** | **Evaluate** — run all your code | After writing or changing any code |
| **Ctrl+.** | **Hush** — stop all sound immediately | When something sounds wrong, or to stop |

> [!NOTE]
> **No block evaluation.** Unlike TidalCycles, Strudel evaluates *all* code at once — there is no `Ctrl+Shift+Enter` to run a single block. To selectively mute parts of your code, prefix a `$:` pattern with `_` to silence it:
>
> ```js
> $: s("bd*4")           // this plays
> _$: s("hh*8").gain(0.4) // this is muted
> ```

> [!IMPORTANT]
> **Practice `Ctrl+.` (hush) right now.** Make some sound, then press `Ctrl+.` to stop it. In a live performance, this is your emergency brake. Build the muscle memory.

---

## 1.5 Your First Sounds

### A Single Sound

```js
s("bd")
```

This plays a bass drum sample once per **cycle**. A cycle is Strudel's fundamental unit of time (more in Module 2). By default, one cycle = ~2 seconds.

### Multiple Sounds in a Sequence

```js
s("bd sd hh cp")
```

Four sounds in one cycle: bass drum, snare, hi-hat, clap. They divide the cycle equally — each gets 1/4 of the time.

> **Key insight:** Adding more events to a pattern doesn't make it longer — it subdivides the cycle. The cycle length stays the same; the events get faster.

### Layering with Commas

```js
s("bd bd bd bd, hh hh hh hh hh hh hh hh")
```

The comma creates **two layers** playing simultaneously:
- Layer 1: four bass drums per cycle
- Layer 2: eight hi-hats per cycle

This is your first techno beat!

### Using a Specific Drum Machine

```js
s("bd sd, hh*8").bank("RolandTR909")
```

`.bank("RolandTR909")` switches all the samples to the iconic TR-909 drum machine. More on this in Module 5.

### Adding a Note

```js
note("c3 eb3 g3 bb3")
```

This plays four musical notes (C, E-flat, G, B-flat) using the default synth. Welcome to synthesis!

### Combining Drums and Notes

```js
s("bd sd, hh*8").bank("RolandTR909")
note("c2 ~ <eb2 f2> ~").s("sine")
```

> Wait — this might not work as expected. In Strudel, each code block is one pattern. To play multiple independent patterns simultaneously, use the `$:` label syntax:

```js
$: s("bd sd, hh*8").bank("RolandTR909")
$: note("c2 ~ <eb2 f2> ~").s("sine")
```

Each `$:` creates an independent pattern. They all play at the same time. This is the foundation of Strudel's live coding workflow.

---

## 1.6 The `$:` Pattern System

You'll use `$:` throughout this entire course, so let's understand it now:

```js
// Each $: line is an independent, named pattern
$: s("bd*4")           // Pattern 1: kick
$: s("hh*8").gain(0.4) // Pattern 2: hi-hats
$: s("~ cp ~ cp")      // Pattern 3: clap
```

**Key behaviors:**
1. All `$:` patterns play simultaneously
2. You can change one line and re-evaluate — the others keep playing
3. Each `$:` remembers its pattern — re-evaluating the same `$:` replaces it smoothly
4. This is how you build up and break down a track in real time

---

## 1.7 Visual Feedback with `_scope()`

Want to see your sound as a waveform? Add `._scope()`:

```js
note("c2").s("sawtooth")._scope()
```

This shows a real-time oscilloscope below your code. It's useful for:
- Seeing the shape of different waveforms (Module 3)
- Confirming your filters are working (Module 4)
- Visual feedback during performance (Module 12)

---

## 1.8 Common Beginner Mistakes

| Mistake | What Happens | Fix |
|---------|-------------|-----|
| Forgetting to press `Ctrl+Enter` | Nothing changes | Always evaluate after editing |
| Writing `Bd` instead of `bd` | No sound (case-sensitive) | Use lowercase for sample names |
| Missing closing quotes `"` | Error message | Always close your strings |
| Using `=` instead of `:` | Error or wrong behavior | Use `:` to select a sample variant — e.g. `hh:2` plays the 3rd hi-hat variant (0-indexed). See Section 1.10 below. |
| Putting two patterns without `$:` | Only the last one plays | Use `$:` for each independent pattern |
| Audio doesn't start | Browser blocking audio | Click the Play button first, then `Ctrl+Enter` |

---

## 1.9 Stopping and Troubleshooting

### Stop All Sound

```js
hush
```

Or press **Ctrl+.** (faster).

### Something Sounds Wrong?

1. Press `Ctrl+.` to stop everything
2. Check your code for typos
3. Fix the issue
4. Press `Ctrl+Enter` to re-evaluate

> [!NOTE]
> **If you evaluate code with an error, the old pattern keeps playing.** Strudel doesn't stop your music when you make a mistake — it just ignores the bad evaluation. This is a feature, not a bug — especially important during live performance.

### Browser Audio Issues

If audio stops working:
1. Refresh the page
2. Click Play to re-enable the audio context
3. Press `Ctrl+Enter` to re-evaluate your code

---

## 1.10 Exploring the Sounds Browser

Click the **Sounds** tab in the toolbar. You'll see a searchable list of all available samples:

- **Drum hits:** `bd`, `sd`, `hh`, `oh`, `cp`, `mt`, `ht`, `lt`, `rim`, `cb`, `cr`, `rd`
- **Drum machines:** `RolandTR808`, `RolandTR909`, `RolandCR78`, `LinnDrum`, and many more
- **Instruments:** Various acoustic and electronic instrument samples
- **Wavetables:** 1000+ single-cycle waveforms prefixed with `wt_`

Try clicking any sample name to hear it, or type it into your code:

```js
// Explore different samples
s("bd")           // bass drum
s("sd")           // snare
s("hh")           // closed hi-hat
s("oh")           // open hi-hat
s("cp")           // clap
s("rim")          // rimshot
s("cr")           // crash cymbal
s("cb")           // cowbell
```

### Selecting Sample Variants with `:`

Many sample names have **multiple variants** — for example, the Sounds tab might show `RolandTR909_hh(4)`, meaning 4 different hi-hat recordings are available. By default, Strudel plays the first one (index 0).

Use **`:N`** inside mini-notation to select a specific variant (0-indexed):

```js
// Play different hi-hat variants
s("hh:0 hh:1 hh:2 hh:3")

// Mix kick variants over a beat
s("bd:0 bd:1 bd:0 bd:2")
```

You can also use the `.n()` function to achieve the same thing:

```js
// These two are equivalent:
s("hh*8").bank("RolandTR909").n("0 1 2 3")
s("hh:0 hh:1 hh:2 hh:3").bank("RolandTR909")
```

> [!NOTE]
> Numbers that are too high wrap around. If a sample has 4 variants, `hh:5` plays the same as `hh:1`.

---

## 1.11 Practice Challenges

### Challenge 1: Four on the Floor
Write a pattern with a kick drum on every beat (4 per cycle):

<details>
<summary>Solution</summary>

```js
s("bd bd bd bd")
// or equivalently:
s("bd*4")
```
</details>

### Challenge 2: Basic Beat
Add hi-hats (8 per cycle) on top of the four kicks:

<details>
<summary>Solution</summary>

```js
s("bd*4, hh*8")
```
</details>

### Challenge 3: Drum Pattern
Create a pattern with kick, snare on beats 2 and 4, and hi-hats:

<details>
<summary>Solution</summary>

```js
s("bd ~ sd ~, hh*8")
// The ~ is a rest (silence) — you'll learn about this in Module 2
```
</details>

### Challenge 4: Two Independent Patterns
Use `$:` to play a drum beat AND a bass note simultaneously:

<details>
<summary>Solution</summary>

```js
$: s("bd ~ sd ~, hh*8").bank("RolandTR909")
$: note("c2").s("sine")
```
</details>

### Challenge 5: Explore
Try at least 3 different samples from the Sounds browser. Play them in a pattern:

<details>
<summary>Solution (yours will be different!)</summary>

```js
s("bd cp rim cb")
// or try different drum machines:
s("bd sd, hh*8").bank("RolandTR808")
s("bd sd, hh*8").bank("RolandCR78")
```
</details>

---

## 1.12 Key Takeaways

1. **Strudel runs in your browser** at [strudel.cc](https://strudel.cc/) — nothing to install
2. **`Ctrl+Enter`** = evaluate *all* code, **`Ctrl+.`** = hush (stop). There is no block evaluation — use `_$:` to mute individual patterns
3. `s("bd sd hh cp")` plays four sounds in one **cycle** — the fundamental time unit
4. Adding more events **subdivides** the cycle, it doesn't make it longer
5. **Commas** layer patterns: `s("bd*4, hh*8")` plays both simultaneously
6. **`$:`** creates independent patterns — use this for multi-element tracks
7. **Mini-notation highlighting** is the default visual feedback; add `.pianoroll()` or `._scope()` for more
8. Use **`:N`** to select sample variants (`hh:0`, `hh:1`, etc.) or `.n()` for the same effect
9. If something breaks, press `Ctrl+.` — the old pattern keeps playing until you successfully evaluate new code
10. Browse available sounds with the **Sounds** tab

---

**Next:** [Module 2 — Cycles, Tempo & Mini-Notation →](./Module_02_Cycles_Tempo_MiniNotation.md)
