# Design: Merged "How it works" + Enriched "When to use"

**Date:** 2026-05-15
**File affected:** `dsa-cheatsheet.html` (single-file project)

## Problem

Two recurring per-tab sections in the cheatsheet have usability issues:

1. **`Diagram` and `How it works` are separate sections.** The numbered
   `How it works` steps do not map one-to-one with the ASCII diagram, so the
   reader has to mentally cross-reference two blocks that should explain each
   other.
2. **`When to use` is too abstract.** It is a flat list of terse phrases
   (e.g. "Sorted array — find a pair that sums to a target"). A reader who
   does not already know the pattern cannot tell what problem statement it
   refers to.

## Goals

- Merge `Diagram` + `How it works` into a single section where each diagram
  block and its explanation sit adjacent and reference each other by label.
- Enrich each `When to use` entry so the problem type is recognizable from a
  problem statement.

Non-goals: changing code templates, practice problems, tab navigation, or any
algorithm content. No unrelated refactoring.

## 1. Merged section structure

Replace the two headings `Diagram — <X>` and `How it works` with a single
heading: **`How it works — <example name>`** (the example name carried over
from the old Diagram heading).

The diagram is split into labeled blocks. Each block is immediately followed
by the prose note that explains exactly that block. The label glyph (`①`,
`②`, …) appears **both inside the ASCII art and on the prose note**, so the
correspondence is visual and unambiguous.

Markup template:

```html
<h3>How it works — Two Sum II</h3>
<div class="walkthrough-merged">
  <div class="wm-setup">arr = [1,3,5,7,9,11]   target = 10   (shared header / legend / rule)</div>

  <div class="wm-block"><pre class="ascii-diagram">┌─ ① ──────────────────┐
  L=0              R=5
  sum = 1+11 = 12 > 10 → R--
└────────────────────────┘</pre></div>
  <div class="wm-note"><span class="wm-num">①</span> Start wide: L at index 0,
    R at the last index. Sum is too big, so the right value must shrink — move R left.</div>

  <div class="wm-block"><pre class="ascii-diagram">┌─ ② ──────────────────┐
  L=0          R=4
  sum = 1+9 = 10 == 10  ✓
└────────────────────────┘</pre></div>
  <div class="wm-note"><span class="wm-num">②</span> Match found → return [L, R]. Done.</div>
</div>
```

Notes:

- `wm-setup` holds the shared array/target/legend/rule that applied to the
  whole old diagram (not part of any single step).
- Block labels are authored manually inside the ASCII art **and** in the
  `<span class="wm-num">` of the matching note. They must stay in sync; this
  is hand-drawn ASCII art so a CSS counter cannot drive the in-art label.
- `&lt;` / `&gt;` escaping rules already used elsewhere in the file still
  apply inside `<pre>`.

## 2. Diagram archetypes

The block-annotation adapts to three archetypes already present in the file.
Only the first is exercised by the Phase 1 pilot; all three are specified so
Phase 2 does not stall.

- **Trace diagrams** — Two Pointers, Sliding Window, Binary Search, Graph
  BFS/DFS visit order, etc. Blocks = time steps. Each `wm-note` explains what
  changed at that step and why. Perfect 1:1 fit. **Pilot uses this form.**
- **Table diagrams** — 1D DP, 2D DP. Structure: a 1–2 line conceptual
  preamble in `wm-setup` (define the subproblem + recurrence + base cases),
  then **each table row is a labeled block** and its `wm-note` explains why
  `dp[i]` took that value. The Mermaid recurrence graph, where present, stays
  as a single labeled block whose note states what the recurrence means.
- **Structural diagrams** — Mermaid recurrence / decision trees in 1D DP and
  Backtracking. Blocks = tree nodes or branches. Each `wm-note` explains what
  that node/branch represents rather than a time step. The Backtracking
  "universal template" steps map onto the choose/recurse/unchoose branches of
  the decision tree.

## 3. Enriched "When to use"

Each `<li>` in `.when-box` becomes a structured entry: a bold scenario title,
a one-sentence description, a signal line, and one concrete example problem.

```html
<div class="when-box"><ul>
  <li><strong>Pair summing to a target (sorted)</strong>
    <div class="w-desc">Find two elements in a sorted array that add up to a target value.</div>
    <div class="w-sig"><span class="w-sig-label">Signal:</span> "sorted array" + "two numbers that sum to / add up to"</div>
    <div class="w-ex">e.g. Two Sum II — Input Array Is Sorted</div>
  </li>
  ...
</ul></div>
```

- The signal uses a clean styled `Signal:` label (`.w-sig-label`), **not** the
  obscure `⚲` glyph from the brainstorming mockup.
- One example problem per entry; reuse problems already named in that tab's
  practice list or code templates where possible.
- The scenario title is a refined version of the existing terse bullet.

## 4. CSS additions

Add rules (near the existing `.when-box` / `.diagram-box` / `.walkthrough`
block) for:

- `.walkthrough-merged` — container spacing.
- `.wm-setup` — shared header/legend; muted, monospace, small.
- `.wm-block` — wraps the per-step `<pre class="ascii-diagram">`; reuses
  existing `.ascii-diagram` typography.
- `.wm-note` — prose explanation; indented to align under its block.
- `.wm-num` — the label badge on the note (circled-number styling matching
  the in-art label).
- `.w-desc`, `.w-sig`, `.w-sig-label`, `.w-ex` — enriched `when-box` entry
  parts. `.w-ex` muted/italic; `.w-sig-label` emphasized.

The old `.diagram-box` and `.walkthrough` rules remain until Phase 2
completes, then are removed once no tab references them.

## 5. Rollout

- **Phase 1 — pilot.** Rebuild the **Two Pointers** tab only: merged section
  + enriched `When to use` + the new CSS. User reviews the rendered result in
  a browser. Format is locked from the approved pilot.
- **Phase 2 — rollout.** Apply the locked format to the remaining ~24 tabs,
  grouped by archetype (trace → table → structural). After every tab is
  migrated, delete the now-unused `.diagram-box` / `.walkthrough` CSS and
  remove the old `Diagram` / `How it works` heading pairs.

Each phase is a separate review gate; Phase 2 only begins after the pilot is
approved.

## Success criteria

- For any tab, a reader can read one diagram block and its note together
  without scrolling between two sections.
- Each `When to use` entry states what the problem looks like, the signal that
  identifies it, and a concrete example.
- No algorithm content, code template, or practice problem is altered.
- No dead CSS remains after Phase 2.
