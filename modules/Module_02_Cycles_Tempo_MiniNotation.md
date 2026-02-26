# Module 2 — Cycles, Tempo & Mini-Notation

> **Goal:** Understand how time works in Strudel, set your tempo for techno, and master the Mini-Notation language that powers every pattern you'll ever write.

---

## 2.1 The Cycle — Strudel's Heartbeat

In traditional DAWs you think in **bars** and **beats**. In Strudel you think in **cycles**.

A cycle is one complete pass through your pattern. By default, one cycle lasts **2 seconds** (0.5 cycles per second).

```js
// One kick per cycle — fires once every 2 seconds
s("bd")
```

### The Key Insight

> Adding more events to a pattern **does not make it longer**.
> It **subdivides** the same cycle into smaller slices.

```js
// Still one cycle, but now 4 kicks share the time
s("bd bd bd bd")
```

Each kick is now ½ second long. The cycle stayed the same length — the events got shorter.

Try adding and removing events:

```js
s("bd bd")           // 2 events — each 1 second
s("bd bd bd")        // 3 events — each ~0.66 seconds
s("bd bd bd bd bd")  // 5 events — each 0.4 seconds
```

> 💡 **Exercise:** Type `s("bd bd bd bd bd bd bd bd")` and listen. Now delete events one by one. Notice how the remaining events get longer.

---

## 2.2 Setting Tempo

### CPS — Cycles Per Second

The raw tempo unit. Default is `0.5` (one cycle every 2 seconds):

```js
// Explicitly set CPS
setcps(0.5)
s("bd bd bd bd")
```

### CPM — Cycles Per Minute

More intuitive if you think in BPM (beats per minute). The formula is:

```
setcpm(BPM / BeatsPerCycle)
```

For **Minimal Techno at 128 BPM** with a standard 4-on-the-floor kick (4 beats per cycle):

```js
setcpm(128 / 4)
s("bd bd bd bd")
```

Now each `bd` is one beat, and 4 beats = 1 cycle = one bar at 128 BPM.

### Per-Pattern Tempo

You can also set tempo on individual patterns:

```js
s("bd bd bd bd").cpm(128 / 4)
```

### Quick Reference

| BPM | Beats/Cycle | `setcpm()` |
|-----|-------------|------------|
| 120 | 4 | `setcpm(120/4)` → `setcpm(30)` |
| 128 | 4 | `setcpm(128/4)` → `setcpm(32)` |
| 130 | 4 | `setcpm(130/4)` → `setcpm(32.5)` |
| 140 | 4 | `setcpm(140/4)` → `setcpm(35)` |

> 💡 **Exercise:** Set the tempo to 128 BPM and type `s("bd bd bd bd")`. Then try `s("bd bd bd bd, hh hh hh hh hh hh hh hh")`. Count along — does it feel like 128?

---

## 2.3 Mini-Notation — The Pattern Language

Mini-notation is the string-based language inside the quotes when you write something like `s("bd sd hh")`. It's how you describe **rhythm** and **structure** in Strudel.

Everything below happens inside double-quotes `"..."` or backticks `` `...` ``.

---

### 2.3.1 Sequences — Spaces Between Events

Events separated by spaces divide the cycle equally:

```js
// 4 events, each ¼ of the cycle
s("bd sd hh cp")
```

```js
// 3 events, each ⅓ of the cycle
s("bd sd hh")
```

```js
// 2 events, each ½ of the cycle
s("bd sd")
```

> 💡 **Exercise:** Try `note("c d e f g a b")`. All 7 notes cram into one cycle. Now try `note("c d e")` — only 3 notes, but they're longer.

---

### 2.3.2 Rests — The `~` Symbol

Silence is just as important as sound. The tilde `~` creates a rest:

```js
// Classic four-on-the-floor with snare on 2 and 4
s("bd ~ sd ~")
```

```js
// Hihat with a gap
s("hh hh ~ hh")
```

```js
// Syncopated kick
s("bd ~ ~ bd ~ ~ bd ~")
```

> 💡 **Exercise:** Start with `s("bd bd bd bd")` and replace some kicks with `~` to create a rhythm you like.

---

### 2.3.3 Sub-Groups — Square Brackets `[ ]`

Brackets let you **subdivide** a single time slot into multiple events:

```js
// Without brackets: 4 events, each ¼ of the cycle
s("bd sd hh cp")

// With brackets: the [hh hh] pair occupies ONE quarter
s("bd sd [hh hh] cp")
```

What happened? The bracket group `[hh hh]` takes the time of **one event** in the outer sequence, and the two hi-hats inside share that time equally. So you get:

- `bd` → ¼ cycle
- `sd` → ¼ cycle
- `hh` → ⅛ cycle
- `hh` → ⅛ cycle
- `cp` → ¼ cycle

You can nest brackets as deep as you want:

```js
// Deep nesting
s("bd [sd [hh hh]] [~ cp] [hh [oh hh]]")
```

> 💡 **Exercise:** Start with `s("bd sd hh cp")` and gradually add brackets:
> 1. `s("bd [sd sd] hh cp")`
> 2. `s("bd [sd sd] [hh hh] cp")`
> 3. `s("bd [sd [sd sd]] [hh hh] [cp ~]")`
>
> Listen to how the rhythm changes!

---

### 2.3.4 Multiplication — The `*` Operator

Repeat an event (or group) faster:

```js
// hh plays 8 times in one cycle
s("hh*8")

// Only the hh is multiplied, bd plays once
s("bd hh*4")

// Multiply a group
s("[bd sd]*2")
```

This is extremely useful for hi-hats and percussion:

```js
// Full drum pattern using *
s("bd sd [~ bd] sd, hh*8")
```

> 💡 **Exercise:** Try `s("hh*1")`, then `s("hh*2")`, `s("hh*4")`, `s("hh*8")`, `s("hh*16")`. Notice how multiplication speeds things up without changing the cycle.

---

### 2.3.5 Division — The `/` Operator

Slow a pattern down so it plays over multiple cycles:

```js
// This 4-note pattern plays over 2 cycles (half speed)
note("[c d e f]/2")
```

```js
// Plays over 4 cycles
note("[c d e f]/4")
```

> 💡 **Exercise:** Try `note("[c d e f g a b c5]/8")` — one note per cycle!

---

### 2.3.6 Slow Sequence — Angle Brackets `< >`

Angle brackets play **one element per cycle**, cycling through them:

```js
// Cycle 1: c, Cycle 2: d, Cycle 3: e, Cycle 4: f, then repeat
note("<c d e f>")
```

This is equivalent to dividing by the number of elements:

```js
// These are the same:
note("<c d e f>")
note("[c d e f]/4")
```

But angle brackets are easier — you can add elements without changing a number:

```js
// Alternate the snare sound every cycle
s("bd <sd cp> bd <sd rim>")
```

Angle brackets are **essential** for creating variation over time:

```js
// Bass note changes every cycle
note("<c2 c2 eb2 f2>").s("sine")

// Drum machine alternates every cycle
s("bd sd, hh*8").bank("<RolandTR808 RolandTR909>")
```

> 💡 **Exercise:** Try `note("<c e g c5>")` and listen for 4 cycles. Then try nesting: `note("<c <e g>>")` — what happens?

---

### 2.3.7 Parallel / Polyphony — The `,` Operator

Play multiple things **at the same time**:

```js
// Kick and hihat simultaneously
s("bd sd bd sd, hh*8")
```

```js
// Three-note chord
note("[c3,e3,g3]")
```

```js
// Full drum kit: kick pattern + hihats + clap
s("bd [~ bd] bd [~ bd], hh*8, ~ cp ~ cp")
```

Each comma-separated pattern runs in parallel, independently sharing the cycle.

> 💡 **Exercise:** Build a drum beat by stacking layers:
> 1. Start: `s("bd ~ bd ~")`
> 2. Add hats: `s("bd ~ bd ~, hh*8")`
> 3. Add snare: `s("bd ~ bd ~, hh*8, ~ sd ~ sd")`
> 4. Add percussion: `s("bd ~ bd ~, hh*8, ~ sd ~ sd, rim*3")`

---

### 2.3.8 Elongation — The `@` Operator

Give an event more **weight** (time) relative to others:

```js
// bd gets 2 units, sd gets 1 unit = bd is twice as long
s("bd@2 sd")
```

```js
// First chord held longer
note("<[c3,e3,g3]@3 [a2,c3,e3]>")
```

---

### 2.3.9 Replication — The `!` Operator

Repeat an event **without speeding up** (unlike `*`):

```js
// bd plays twice, then sd once — 3 events total
s("bd!2 sd")

// Compare with *:
// s("bd*2 sd") — bd plays twice in the TIME of one slot
```

Difference at a glance:

| Pattern | Result |
|---------|--------|
| `s("bd sd")` | 2 events: `bd` `sd` |
| `s("bd!2 sd")` | 3 events: `bd` `bd` `sd` |
| `s("bd*2 sd")` | `bd` `bd` crammed into slot 1, then `sd` in slot 2 |

> 💡 **Exercise:** Compare `s("hh!4")` vs `s("hh*4")` — they sound the same here because there's nothing else. Now compare `s("bd hh!3")` vs `s("bd hh*3")` — hear the difference?

---

### 2.3.10 Sample Selection — The `:` Separator

Many sample banks contain multiple variations. Select them with `:n`:

```js
// Default (first sample)
s("hh")

// Second and third variations
s("hh:1 hh:2")

// Cycle through all variations
s("hh:0 hh:1 hh:2 hh:3")
```

---

### 2.3.11 Randomness — `?` and `|`

**Random removal** with `?`:

```js
// Each hh has 50% chance of being silent
s("hh*8?")

// 20% chance of removal
s("hh*8?0.2")

// 80% chance of removal (very sparse)
s("hh*8?0.8")
```

**Random choice** with `|`:

```js
// Each cycle randomly picks bd OR sd
s("bd|sd")

// Random chord choice
note("[c3,e3,g3] | [a2,c3,e3] | [d3,f3,a3]")
```

> 💡 **Exercise:** Try `s("[bd|sd|cp|hh]*8")` — a random beat every time!

---

### 2.3.12 Euclidean Rhythms — `(beats, steps, offset)`

This is one of Strudel's most powerful features. It distributes N beats across M steps as evenly as possible, producing rhythms found across world music.

```js
// 3 beats over 8 steps = the "tresillo" (x . . x . . x .)
s("bd(3,8)")

// 5 beats over 8 steps
s("hh(5,8)")

// With offset (rotation) as third parameter
s("bd(3,8,1)")
```

**Common Euclidean patterns:**

| Pattern | Name / Feel |
|---------|------------|
| `(1,4)` | 4/4 pulse |
| `(2,3)` | Shuffle |
| `(2,5)` | Bossa nova feel |
| `(3,7)` | West African bell |
| `(3,8)` | Cuban tresillo / Pop clave |
| `(4,7)` | Turkish aksak |
| `(5,8)` | Cinquillo |
| `(5,9)` | Arab rhythm |
| `(7,12)` | West African bell |
| `(7,16)` | Complex shuffle |

**Layered Euclidean rhythms create instant polyrhythms:**

```js
setcpm(128/4)
s("bd(3,8), hh(5,8), cp(2,5), rim(7,16)")
```

> 💡 **Exercise:** Start with just `s("bd(3,8)")`. Then layer: `s("bd(3,8), cp(2,5)")`. Then add: `s("bd(3,8), cp(2,5), hh(5,8)")`. Experiment with different numbers!

---

## 2.4 Combining Everything — Building Rhythms

Now let's combine all the mini-notation concepts to build real patterns:

### Example 1: Basic Four-on-the-Floor

```js
setcpm(128/4)
s("bd bd bd bd, hh*8, ~ sd ~ sd")
```

### Example 2: Syncopated Techno

```js
setcpm(128/4)
s("bd ~ [~ bd] ~, hh*16, ~ cp ~ cp, rim(3,8)")
```

### Example 3: Using Angle Brackets for Variation

```js
setcpm(128/4)
s("bd <~ [~ bd]> bd <~ [bd ~]>, hh*8, ~ <cp sd> ~ <cp rim>")
```

### Example 4: Melodic Pattern with Rests and Brackets

```js
setcpm(128/4)
note("<[c3 ~ eb3 ~] [c3 c3 ~ f3] [c3 ~ g3 ~] [c3 f3 ~ eb3]>")
  .s("sine")
```

### Example 5: Full Beat with Euclidean Rhythms

```js
setcpm(128/4)
s(`
  bd(3,8),
  hh(5,8),
  ~ [cp ~] ~ cp,
  rim(7,16)
`).bank("RolandTR909")
```

> 💡 **Exercise:** Write your own 4-bar drum loop using:
> - At least one `*` multiplication
> - At least one `< >` angle bracket for variation
> - At least one `[ ]` sub-group
> - At least one rest `~`

---

## 2.5 Mini-Notation Cheat Sheet

| Syntax | Name | What it does |
|--------|------|-------------|
| `a b c` | Sequence | Divide cycle equally between events |
| `~` | Rest | Silence |
| `[a b]` | Sub-group | Subdivide a slot into sub-events |
| `a*N` | Multiply | Repeat N times (faster) |
| `[a b]/N` | Divide | Play over N cycles (slower) |
| `<a b c>` | Slow seq | One event per cycle, rotate |
| `a,b` | Parallel | Play simultaneously |
| `a@N` | Elongate | Give N units of time weight |
| `a!N` | Replicate | Repeat N times (same speed) |
| `a:N` | Select | Pick Nth sample variation |
| `a?` | Degrade | 50% chance of removal |
| `a?0.3` | Degrade by | 30% chance of removal |
| `a\|b` | Random pick | Choose one randomly each cycle |
| `a(N,M)` | Euclidean | N beats over M steps |
| `a(N,M,O)` | Euclidean+offset | With rotation offset |

---

## 2.6 Practice Challenges

### Challenge 1: Recreate This Rhythm ⭐⭐
Listen to a typical minimal techno beat and try to recreate it:
- Kick on every beat (1, 2, 3, 4)
- Open hi-hat on the "&" of beat 2
- Closed hi-hat on every 8th note

```js
setcpm(128/4)
// Your answer here — hint: use [hh oh] in the right place
```

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
s("bd*4, [hh hh [hh oh] hh]*2, ~ cp ~ cp")
  .bank("RolandTR909")
```
</details>

### Challenge 2: Euclidean Polyrhythm ⭐⭐⭐
Create a polyrhythmic pattern with:
- Kick: 3 beats over 8 steps
- Snare: 2 beats over 5 steps
- Hi-hat: 7 beats over 12 steps

```js
setcpm(128/4)
// Your answer here
```

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
s("bd(3,8), sd(2,5), hh(7,12)")
  .bank("RolandTR909")
```
</details>

### Challenge 3: Evolving Pattern ⭐⭐⭐
Write a 4-cycle pattern where the kick pattern changes each cycle using angle brackets:

```js
setcpm(128/4)
// Your answer here
```

<details>
<summary>Solution</summary>

```js
setcpm(128/4)
s("<[bd ~ ~ ~] [bd ~ bd ~] [bd ~ [~ bd] ~] [bd [~ bd] bd ~]>, hh*8")
  .bank("RolandTR909")
```
</details>

---

**Previous:** [← Module 1 — The Strudel Environment](./Module_01_Strudel_Environment.md)
**Next:** [Module 3 — Synths & Oscillators →](./Module_03_Synths_Oscillators.md)
