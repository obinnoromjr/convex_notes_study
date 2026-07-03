# Interactive Study-Sheet Generator — Project Instructions

Paste these into the project's custom instructions (Settings → project), or keep this file
and say "follow STUDY_SHEET_INSTRUCTIONS.md". Then just drop in a section of source
material and say **"make a sheet."**

---

## 1. What to do when I paste material

When I paste the text of a Boyd & Vandenberghe section (or any math-heavy lesson) and ask
for a "sheet", "study sheet", "make the HTML", or "follow the same template":

1. Build **one self-contained interactive HTML study page** in the house style defined below.
2. Save it to the project folder with a descriptive `snake_case` filename (e.g.
   `dual_cones.html`).
3. **Add a link to `index.html`** under the correct chapter group (see §8).
4. Verify it (see §7), then present it with `present_files`.

Do **not** ask clarifying questions unless the scope is genuinely ambiguous — the format is
already settled. Skip the frontend-design skill; the design system is fixed here.

Use a task list (build → verify → index → present) for tracking.

---

## 2. THE ONE RULE THAT MATTERS MOST — fidelity

**Preserve every substantive point in the source. Do not compress the lesson.**

Substantive = must appear on the page:
- every definition and theorem (as a `.bv` blockquote, with the `B&V §x · Lxxxx` source ref),
- every worked example and its *specific* details (matrices, maps, numbers, witnesses),
- every proof (as a `.proof` step block),
- every remark, edge case, "only if" nuance, counterexample, and forward/backward pointer,
- the "where each hypothesis is used" style asides.

Rules:
- **Mirror the source's own section structure and numbering.** If the source has sections
  0–7, the page has matching numbered sections (`§0 … §7`). Don't merge two source sections
  into one card unless they're trivially short.
- Examples get their own section or a clearly labelled `.excard`/`.note`, **never** a single
  buried clause.
- When unsure whether something is "substantive" or "phrasing" — **include it.**
- **After building, re-read the source against the page and restore anything dropped.** This
  is a required step, not optional. (Past sheets lost Example 2.19, PSD witnesses, the
  perspective inverse-image statement, etc. — don't repeat that.)

The interactive labs are *additive*. They never replace prose. The prose must stand alone as
a faithful set of notes.

---

## 3. Page skeleton (in order)

```
<head> … fonts + KaTeX + <style> (§4) …</head>
<body>
  <header class="hero"> kicker · h1 (with <em> accent) · sub · meta chips · hero SVG </header>
  <main>
    §0  "where we are"            — 1 short orienting paragraph
    §1…§N  one section per source section, in order:
           - .bv blockquote for each definition/theorem (+ .src line ref)
           - prose paragraphs / .reads lists for the explanation
           - .proof block for each proof
           - .note callouts for remarks / warnings / build-moves
           - a .lab (interactive) where a visual genuinely helps
    §N+1  "expert lens" — the source's mastery/expert bullets as .move cards
    summary — dark .summary block with .scard cards (the source's "quick review")
  </main>
  <footer> B&V §x (p. …) · interactive study sheet · drag any handle </footer>
  <script> KaTeX auto-render + interactive labs </script>
</body>
```

Target **2–3 interactive labs** per sheet — each teaching a *distinct* idea. Good archetypes:
a **membership / verify** lab (drag a point, test a condition), a **proof-made-visual** lab
(watch the inequality hold, or a counterexample escape), and a **named-example** lab.

---

## 4. Design system — paste this `<style>` verbatim

Fonts (in `<head>`):
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,400;9..144,500;9..144,600;9..144,700&family=Newsreader:opsz,wght@6..72,400;6..72,500;6..72,600&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.16.9/katex.min.css">
<script defer src="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.16.9/katex.min.js"></script>
<script defer src="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.16.9/contrib/auto-render.min.js"></script>
```

Design tokens (the palette is fixed — do not swap it per sheet):
```css
:root{
  --paper:#FBFAF7; --paper-2:#F4F1EA; --grid:#E7E1D4;
  --ink:#17223B; --ink-soft:#46506A; --ink-faint:#8A91A3; --rule:#D9D3C6;
  --normal:#D7402B; --normal-soft:#F6DAD3;          /* the through-line accent */
  --halfspace:#2B6CB0; --halfspace-fill:rgba(43,108,176,0.13);
  --inside:#2F855A; --inside-fill:rgba(47,133,90,0.12); --outside:#C0392B;
  --maxw:740px;
}
```

Typography roles:
- **Fraunces** — display (h1/h2/h3, lab titles, card headings).
- **Newsreader** — body prose (19px, line-height 1.62), the "textbook" voice.
- **JetBrains Mono** — labels, eyebrows, `.mono`, readouts, math coordinates.
- **KaTeX** — all real math (`\(…\)` inline, `\[…\]` or `$$…$$` display).

The full class list (hero, section/secnum/eyebrow, bv, reads, lab/canvas/panel, ctrl/range,
chips, readout, verdict[.in/.out/.mid], legend, proof/step[.crux]/why, note[.key/.warn/.build],
moves/move, summary/scard, footer) plus responsive `@media (max-width:820px)` collapse of
`.lab-body` to one column — **copy the `<style>` block from any recent sheet**
(`separating_hyperplane.html` or `supporting_hyperplanes.html` are good, complete references).
Don't hand-rewrite the CSS; clone it so every sheet stays pixel-consistent.

### Accent conventions (semantic colour use)
- **vermilion `--normal`** = the *one object the section is about* (the through-line: the
  normal `a`, the radius/semi-axis, the cone, the map…). Reserve red for it.
- **blue `--halfspace`** = a set / region / halfspace.
- **green `--inside`** = inside / feasible / "condition holds".
- **red `--outside`** = outside / violated.
- **ink navy** = text and neutral geometry.

---

## 5. Component snippets

**Definition/theorem blockquote** (one per definition/theorem — keep the source line ref):
```html
<div class="bv">…statement, math in \(…\)…<span class="src">B&amp;V §2.5.1 · L2382</span></div>
```

**Proof block** (mark the load-bearing step `.crux`):
```html
<div class="proof">
  <div class="proof-head"><span>Claim …</span><span class="target">the design move</span></div>
  <div class="step crux"><div class="math">\[ … \]</div><div class="why"><b>why</b> …</div></div>
  <div class="step"><div class="math">\[ … \]</div><div class="why">…</div></div>
</div>
```

**Callouts:** `.note.key` (red, the key idea), `.note.warn` (amber, a caution / symbol
overload), `.note.build` (ink, a construction/design move). Each: `<div class="ic">!</div>`
+ `<p>…</p>`.

**Tagged bullets** for "three reads / cases / examples":
```html
<ul class="reads"><li><span class="tag">algebra</span><div>…</div></li>…</ul>
```

**Expert-lens cards** and **summary cards**: `.moves > .move` (mh + mnum) and
`.summary > .scards > .scard` (h6 + p). Put the source's expert/quick-review bullets here —
these are a *summary*, not a substitute for the body sections.

---

## 6. Interactive labs

Shell:
```html
<div class="lab">
  <div class="lab-head"><span class="lh-num">LAB 1</span><h4>title</h4></div>
  <div class="lab-body">
    <div class="canvas-col"><canvas id="cv1"></canvas><div class="legend">…</div></div>
    <div class="panel-col"><h5>controls</h5><p class="hint">…</p>
       …sliders / chips…
       <div class="readout" id="ro1"></div><div class="verdict" id="vd1"></div></div>
  </div>
</div>
```

Standard JS (clone the helper block from a recent sheet — keep names identical):
`fitCanvas`, `makeView(canvas,range)` (`.toS/.toW/.s`), `drawGrid`, `dot`, `label`, `arrow`,
`clipHalf` (half-plane clipping), `genericDrag(canvas, pickables, redraw)`, and per-lab
pointer handlers using `setPointerCapture`. Redraw on `ResizeObserver`. Devicepixelratio via
`fitCanvas`. Render math with:
```js
renderMathInElement(document.body,{delimiters:[
  {left:"$$",right:"$$",display:true},{left:"\\[",right:"\\]",display:true},
  {left:"\\(",right:"\\)",display:false}], throwOnError:false});
```

Lab guidelines:
- Make handles **draggable** and controls **live** (sliders/chips). Always show a `.readout`
  (numbers) and a `.verdict` (in/out/mid).
- Pick a coordinate `range` (usually 5) and keep world↔screen mapping via `makeView`.
- For 3-D (cones), use a simple oblique projector with an azimuth slider (see
  `psd_cone.html` / `norm_balls_and_cones.html`).
- **SVG rule:** CSS `var(--…)` does **not** resolve inside SVG presentation attributes.
  In the hero SVG use **hard-coded hex** (`fill="#D7402B"`), never `fill="var(--normal)"`.

---

## 7. Verification (required before presenting)

Run in the workspace shell:
1. Extract the `<script>` and `node --check` it (syntax).
2. Confirm **no `var(--` inside any `<svg>`**.
3. **Node-sample the core math** to prove the lab is correct — e.g. verify an identity over
   10–20k random inputs, or that a constructed object satisfies its defining inequality.
   (Examples used before: PSD three-condition ⇔ eigenvalue test; perspective
   `P(θx+(1−θ)y)=μP(x)+(1−μ)P(y)`; separator `f≤0` on C and `f≥0` on D; ℓ_p boundary norm = r.)
4. Then re-read the source against the page for the fidelity check (§2).

---

## 8. Update index.html (do this every time)

Open `index.html` and add one list item under the right chapter group (create a new
`<h2>` group + `<ul>` if the chapter doesn't exist yet), keeping numeric order:
```html
<li><a class="note" href="FILENAME.html"><span class="sec">2.x.y</span><span class="title">Title</span></a></li>
```
`index.html` uses the same design tokens as the sheets (see §4); match its existing card
markup exactly.

---

## 9. Tone

Prose is warm, precise, textbook-grade — Newsreader body, minimal bolding, no filler. Copy on
controls is plain and imperative ("Drag x₀", "Slide θ"). The single boldest element per sheet
is the vermilion through-line; keep everything else quiet.
```
