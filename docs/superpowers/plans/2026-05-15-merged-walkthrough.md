# Merged Walkthrough + Enriched When-to-use Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Merge each tab's separate `Diagram` + `How it works` sections into one block-annotated section, and enrich each `When to use` entry with a description, signal, and example.

**Architecture:** Single-file static HTML (`dsa-cheatsheet.html`). No build, no test framework — verification is structural `grep` checks plus manual browser review. Phase 1 rebuilds the Two Pointers tab as a locked-format pilot; Phase 2 rolls the locked format to the remaining ~24 tabs and removes dead CSS.

**Tech Stack:** Hand-authored HTML + embedded CSS. No JS/library changes.

**Spec:** `docs/superpowers/specs/2026-05-15-merged-walkthrough-design.md`

---

## File Structure

- Modify: `dsa-cheatsheet.html` only.
  - CSS block (`<style>`, around lines 120–152): add new rules.
  - Per-tab `<div class="tab-content">` sections: replace `When to use`
    list + `Diagram` heading/box + `How it works` heading/box.
- No new files. Old `.diagram-box` / `.walkthrough` CSS removed in the final
  Phase 2 task once unused.

---

## Phase 1 — Pilot (Two Pointers tab)

### Task 1: Add new CSS rules

**Files:**
- Modify: `dsa-cheatsheet.html` (insert after line 151, the
  `.walkthrough li strong` rule, before the `/* Code blocks */` comment)

- [ ] **Step 1: Verify the insertion point**

Run: `grep -n ".walkthrough li strong" dsa-cheatsheet.html`
Expected: one line, `151:  .walkthrough li strong { color: var(--yellow); }`

- [ ] **Step 2: Insert the new CSS**

Use Edit. `old_string` is:

```
  .walkthrough li strong { color: var(--yellow); }

  /* Code blocks */
```

`new_string` is:

```
  .walkthrough li strong { color: var(--yellow); }

  /* Merged walkthrough (diagram block + its explanation) */
  .walkthrough-merged {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 18px 20px;
    overflow-x: auto;
  }
  .wm-setup {
    font-family: 'Fira Code', 'Cascadia Code', monospace;
    font-size: 13px;
    line-height: 1.7;
    white-space: pre;
    color: var(--text-muted);
    border-bottom: 1px solid var(--border);
    padding-bottom: 12px;
    margin-bottom: 14px;
  }
  .wm-block { margin-top: 14px; }
  .wm-block:first-of-type { margin-top: 0; }
  .wm-block .ascii-diagram { margin: 0; }
  .wm-note {
    margin: 8px 0 4px 14px;
    color: var(--text);
    line-height: 1.6;
  }
  .wm-num { color: var(--yellow); font-weight: 700; margin-right: 4px; }

  /* Enriched "when to use" entries */
  .when-box li { margin: 12px 0; }
  .when-box li strong { color: var(--yellow); }
  .w-desc { color: var(--text); margin: 2px 0; }
  .w-sig { color: var(--text-muted); margin: 2px 0; font-size: 13px; }
  .w-sig-label { color: var(--accent); font-weight: 600; }
  .w-ex { color: var(--text-muted); font-style: italic; margin: 2px 0; font-size: 13px; }

  /* Code blocks */
```

- [ ] **Step 3: Verify the CSS landed**

Run: `grep -n "walkthrough-merged\|w-sig-label" dsa-cheatsheet.html`
Expected: at least the rule definitions appear (`.walkthrough-merged {`,
`.w-sig-label {`). No `class=` usages yet.

- [ ] **Step 4: Commit**

```bash
git add dsa-cheatsheet.html
git commit -m "style: add merged-walkthrough and enriched when-to-use CSS"
```

---

### Task 2: Enrich Two Pointers "When to use"

**Files:**
- Modify: `dsa-cheatsheet.html` (the `when-box` inside
  `<div class="tab-content active" id="two-pointers">`, currently
  lines 297–303)

- [ ] **Step 1: Confirm the current block**

Run: `grep -n "Find container with most water" dsa-cheatsheet.html`
Expected: one line inside the two-pointers tab (~line 302).

- [ ] **Step 2: Replace the when-box**

Use Edit. `old_string`:

```
  <h3>When to use</h3>
  <div class="when-box"><ul>
    <li>Sorted array — find a pair that sums to a target</li>
    <li>Check if a string is a palindrome</li>
    <li>Merge two sorted arrays in-place</li>
    <li>Remove duplicates / move zeroes in-place</li>
    <li>Find container with most water</li>
  </ul></div>
```

`new_string`:

```
  <h3>When to use</h3>
  <div class="when-box"><ul>
    <li><strong>Pair summing to a target (sorted)</strong>
      <div class="w-desc">Given a sorted array, find two elements that add up to a target value.</div>
      <div class="w-sig"><span class="w-sig-label">Signal:</span> "sorted array" + "two numbers / a pair that sum to / add up to"</div>
      <div class="w-ex">e.g. Two Sum II — Input Array Is Sorted</div>
    </li>
    <li><strong>Palindrome check</strong>
      <div class="w-desc">Verify a string or array reads the same forward and backward.</div>
      <div class="w-sig"><span class="w-sig-label">Signal:</span> "is a palindrome", "reads the same backward", "ignoring non-alphanumeric"</div>
      <div class="w-ex">e.g. Valid Palindrome</div>
    </li>
    <li><strong>Merge / compare two sorted sequences in place</strong>
      <div class="w-desc">Walk two sorted arrays with one pointer each to merge or compare them without extra space.</div>
      <div class="w-sig"><span class="w-sig-label">Signal:</span> "two sorted arrays", "merge in-place", "without using extra space"</div>
      <div class="w-ex">e.g. Merge Sorted Array</div>
    </li>
    <li><strong>In-place array rewrite (fast / slow pointer)</strong>
      <div class="w-desc">Overwrite an array in one pass so kept elements move to the front; a slow pointer marks the write boundary while a fast pointer scans.</div>
      <div class="w-sig"><span class="w-sig-label">Signal:</span> "in-place", "remove duplicates", "move zeroes", "return the new length"</div>
      <div class="w-ex">e.g. Remove Duplicates from Sorted Array</div>
    </li>
    <li><strong>Maximize over a shrinking range</strong>
      <div class="w-desc">Pointers start at both ends; move the limiting one inward to optimize an area or quantity.</div>
      <div class="w-sig"><span class="w-sig-label">Signal:</span> "container with most water", "maximize area", "move the shorter side"</div>
      <div class="w-ex">e.g. Container With Most Water</div>
    </li>
  </ul></div>
```

- [ ] **Step 3: Verify**

Run: `grep -c "w-sig-label" dsa-cheatsheet.html`
Expected: `5` (five enriched entries; no other tab uses it yet).

- [ ] **Step 4: Commit**

```bash
git add dsa-cheatsheet.html
git commit -m "feat: enrich Two Pointers when-to-use with desc/signal/example"
```

---

### Task 3: Merge Two Pointers Diagram + How it works

**Files:**
- Modify: `dsa-cheatsheet.html` (the `Diagram — Two Sum II` heading/box and
  the `How it works` heading/box inside the two-pointers tab, currently
  lines 305–324)

- [ ] **Step 1: Confirm the current blocks**

Run: `grep -n "Diagram — Two Sum II\|Repeat until L" dsa-cheatsheet.html`
Expected: two lines, both inside the two-pointers tab.

- [ ] **Step 2: Replace both sections with one merged section**

Use Edit. `old_string` is the full current Diagram + How it works
(lines 305–324):

```
  <h3>Diagram — Two Sum II</h3>
  <div class="diagram-box"><div class="ascii-diagram">arr    =  [ 1,  3,  5,  7,  9, 11 ]   target = 10
indices=    0   1   2   3   4   5

Step 1:   [L=0]               [R=5]   sum = 1+11 = 12  > target → R--
Step 2:   [L=0]           [R=4]       sum = 1+9  = 10  == target ✓ → return [0, 4]

Rule: sum &lt; target → L++  (need a bigger number on the left)
      sum &gt; target → R--  (need a smaller number on the right)
      sum == target → found!</div></div>

  <h3>How it works</h3>
  <div class="walkthrough"><ol>
    <li><strong>Place L at index 0, R at last index.</strong></li>
    <li><strong>Compute sum</strong> = arr[L] + arr[R].</li>
    <li>If sum === target → return [L, R].</li>
    <li>If sum &lt; target → L++ (need larger value).</li>
    <li>If sum &gt; target → R-- (need smaller value).</li>
    <li>Repeat until L &ge; R.</li>
  </ol></div>
```

`new_string`:

```
  <h3>How it works — Two Sum II</h3>
  <div class="walkthrough-merged">
  <div class="wm-setup">arr     = [ 1,  3,  5,  7,  9, 11 ]   target = 10
indices =   0   1   2   3   4   5

Rule:  sum &lt; target  → L++   (need a bigger value on the left)
       sum &gt; target  → R--   (need a smaller value on the right)
       sum == target → found</div>

  <div class="wm-block"><div class="ascii-diagram">┌─ ① ──────────────────────────────┐
  [L=0]                        [R=5]
  sum = 1 + 11 = 12   &gt; 10  → R--
└────────────────────────────────────┘</div></div>
  <div class="wm-note"><span class="wm-num">①</span> Start wide: L at index 0,
    R at the last index. Compute sum = arr[L] + arr[R] = 12. 12 is greater than
    the target, so the right value is too big — move R left by one. The loop
    keeps going while L &lt; R.</div>

  <div class="wm-block"><div class="ascii-diagram">┌─ ② ──────────────────────────────┐
  [L=0]                    [R=4]
  sum = 1 + 9 = 10    == 10  ✓
└────────────────────────────────────┘</div></div>
  <div class="wm-note"><span class="wm-num">②</span> Recompute with the new R:
    sum = 1 + 9 = 10, which equals the target → return [L, R] = [0, 4]. If no
    pair ever matches, the loop ends once L &ge; R and the function returns [].</div>
  </div>
```

- [ ] **Step 3: Verify the old headings are gone and the merged one exists**

Run: `grep -n "Diagram — Two Sum II\|How it works — Two Sum II\|walkthrough-merged\"" dsa-cheatsheet.html`
Expected: NO `Diagram — Two Sum II` line; one `How it works — Two Sum II`
heading; one `class="walkthrough-merged"` usage.

- [ ] **Step 4: Verify no stray unescaped angle brackets in the new block**

Run: `grep -n "wm-block" dsa-cheatsheet.html`
Expected: two `wm-block` divs. Manually confirm the lines between them use
`&lt;` / `&gt;` / `&ge;`, never bare `<` or `>` inside the diagram text.

- [ ] **Step 5: Commit**

```bash
git add dsa-cheatsheet.html
git commit -m "feat: merge Two Pointers Diagram + How it works into one section"
```

---

### Task 4: Phase 1 review gate (STOP)

**Files:** none (review only)

- [ ] **Step 1: Open the file in a browser**

Run: `open dsa-cheatsheet.html`
The Two Pointers tab is the default active tab.

- [ ] **Step 2: Visually verify**

Confirm all of:
- "When to use" shows 5 entries, each with a bold title, description,
  blue `Signal:` label, and italic `e.g.` example.
- One "How it works — Two Sum II" section: setup/legend at top, then
  block ① immediately followed by note ①, then block ② followed by note ②.
- The ① / ② glyph in each ASCII box matches the ① / ② badge on its note.
- No leftover separate "Diagram" or numbered "How it works" list.
- No raw `<`, `>`, `&lt;` text visibly rendered wrong.

- [ ] **Step 3: Hand off to the user**

Tell the user: "Phase 1 pilot is live on the Two Pointers tab. Please review
it in the browser. Once you approve, the locked format is exactly what's in
this tab and I'll roll it to the remaining ~24 tabs (Phase 2)."

**Do not start Phase 2 until the user approves the pilot.** If the user
requests format changes, apply them to the Two Pointers tab, re-commit, and
repeat this review gate before Phase 2.

---

## Phase 2 — Rollout to remaining tabs

> Phase 2 begins only after the Task 4 gate is approved. The Two Pointers tab
> as committed is the **locked reference**: copy its exact markup shape and
> CSS classes. Do not introduce new classes or restructure.
>
> **Before authoring any tab, read "Locked format standards" in spec §1.**
> Non-negotiables: open top-rule delimiter (circled-num + space + **36 ×
> U+2500 ─**, no closed box); `<div class="ascii-diagram">` (not `<pre>`);
> examples must exercise every algorithmic branch with the terminal case as
> prose in the final note; pointer spacing is schematic (do not hand-tune);
> all `<`/`>`/`>=` escaped as `&lt;`/`&gt;`/`&ge;`.

Tabs to migrate (everything except `two-pointers`), grouped by archetype
(see spec §2). Process **one tab per task**, in this order. For each tab the
work is identical in shape:

1. Enrich its `When to use`: convert each `<li>` to the
   `<strong>` + `.w-desc` + `.w-sig`/`.w-sig-label` + `.w-ex` structure.
   One example problem per entry — reuse a problem already named in that
   tab's practice list or code template.
2. Merge its `Diagram — <name>` + `How it works` into one
   `<h3>How it works — <name></h3>` + `<div class="walkthrough-merged">`,
   using the archetype mapping below.
3. Verify with grep + browser. Commit per tab.

**Archetype mapping:**

- **Trace** (Sliding Window, Prefix Sums, Hash Maps, Fast & Slow Pointers,
  Linked List Reversal, Binary Search, Sorting, Monotonic Stack, Graph BFS,
  Graph DFS, Tree BFS, Tree DFS, BST): same as the pilot — each diagram step
  becomes a `wm-block` + `wm-note`; shared array/legend goes in `wm-setup`.
- **Table** (1D DP, 2D DP): put the subproblem definition + recurrence +
  base cases in `wm-setup`; make **each `dp` table row** a `wm-block` whose
  `wm-note` explains why that cell took its value. Keep any Mermaid graph as
  a single `wm-block` whose note states what the recurrence means. The
  `<table class="dp-table">` stays as-is inside its block; only the
  surrounding headings/structure change.
- **Structural** (Backtracking, and the Mermaid trees in 1D/2D DP): each
  tree node/branch is a `wm-block` + `wm-note` describing what that node
  represents (not a time step). Backtracking's choose/recurse/unchoose/prune
  steps map onto the decision-tree branches.

### Task 5: Migrate trace-archetype tabs (one commit per tab)

**Files:**
- Modify: `dsa-cheatsheet.html` (each listed tab's `tab-content` div)

- [ ] **Step 1: List the target tabs and their diagram headings**

Run: `grep -n "<h3>Diagram\|tab-content\" id=" dsa-cheatsheet.html`
Expected: pair up each trace tab id with its `Diagram — <name>` heading.

- [ ] **Step 2..N: For each trace tab, in this order — sliding-window,
  prefix-sums, hash-maps, fast-slow, linked-list-reversal, binary-search,
  sorting, monotonic-stack, graph-bfs, graph-dfs, tree-bfs, tree-dfs, bst**

For the tab:
- Replace its `when-box` `<li>`s with the locked enriched structure (mirror
  Task 2 exactly: `<strong>` title, `.w-desc`, `.w-sig` with
  `.w-sig-label`, `.w-ex`). Derive the description from the existing terse
  bullet; derive the signal from problem-statement wording; pick the example
  from that tab's practice/code list.
- Replace its `Diagram — <name>` + `How it works` pair with
  `<h3>How it works — <name></h3>` + `<div class="walkthrough-merged">`
  containing one `wm-setup` (shared header/legend), then alternating
  `wm-block` (`<div class="ascii-diagram">…</div>`) and `wm-note`
  (`<span class="wm-num">①</span> …`) per diagram step. Keep the existing
  step text from the old `How it works` `<ol>` as the basis for each note,
  attached to the matching block. Preserve all `&lt;`/`&gt;`/`&ge;` escaping.
- Verify: `grep -n "Diagram — <name>" dsa-cheatsheet.html` returns nothing;
  `grep -c "walkthrough-merged\"" dsa-cheatsheet.html` increased by 1.
- Commit: `git add dsa-cheatsheet.html && git commit -m "feat: merge + enrich <tab> tab"`

- [ ] **Step final: Browser spot-check**

Run: `open dsa-cheatsheet.html`
Click through each migrated trace tab; confirm block↔note labels line up
and `When to use` renders the 4-part entries.

### Task 6: Migrate table-archetype tabs (1D DP, 2D DP)

**Files:**
- Modify: `dsa-cheatsheet.html` (`dp-1d` and `dp-2d` tab-content divs)

- [ ] **Step 1: Inspect both tabs**

Run: `grep -n "tab-content\" id=\"dp-1d\"\|tab-content\" id=\"dp-2d\"\|dp-table\|<h3>" dsa-cheatsheet.html`
Expected: locate the `When to use`, Mermaid graph, `dp-table`, and
`How it works` parts of each.

- [ ] **Step 2: Migrate dp-1d**

- Enrich `When to use` with the locked 4-part structure (the existing
  entries are already signal-like, e.g. `"How many ways to reach X?"` —
  keep that as the `<strong>` title, add a `.w-desc` explaining it, a
  `.w-sig` with statement wording, and a `.w-ex` example such as
  "House Robber" / "Climbing Stairs").
- Replace `Diagram — House Robber recurrence + dp table` +
  `How it works` with `<h3>How it works — House Robber</h3>` +
  `<div class="walkthrough-merged">`:
  - `wm-setup`: "Subproblem: dp[i] = max money robbing houses 0..i.
    Recurrence: dp[i] = max(dp[i-1], dp[i-2] + nums[i]). Base: dp[0],
    dp[1]." (plain text, escape `<`/`>` if any).
  - One `wm-block` wrapping the existing `<div class="mermaid">…</div>`
    unchanged; its `wm-note` explains the two branches (skip vs rob).
  - One `wm-block` wrapping the existing `<table class="dp-table">…</table>`
    unchanged; its `wm-note` walks the rows ("row i=2: max(7, 2+9)=11 …").
    (For DP, per-row blocks are optional; a single table block with a note
    that narrates the rows is acceptable and matches the locked spirit.)
- Verify: no `Diagram — House Robber` heading remains; `dp-table` and
  `mermaid` markup unchanged (`grep -c "dp-table" dsa-cheatsheet.html`
  unchanged from before this step).
- Commit: `git commit -am "feat: merge + enrich 1D DP tab"`

- [ ] **Step 3: Migrate dp-2d**

Same procedure as Step 2 applied to the `dp-2d` tab: enrich `When to use`;
fold `Diagram — <name>` + `How it works` into
`<h3>How it works — <name></h3>` + `walkthrough-merged`, keeping the 2D
`dp-table` / Mermaid markup intact inside `wm-block`s with explanatory
`wm-note`s. Verify the old `Diagram — <name>` heading is gone and the table
markup is unchanged. Commit: `git commit -am "feat: merge + enrich 2D DP tab"`

### Task 7: Migrate structural-archetype tab (Backtracking)

**Files:**
- Modify: `dsa-cheatsheet.html` (`backtracking` tab-content div)

- [ ] **Step 1: Inspect**

Run: `grep -n "tab-content\" id=\"backtracking\"\|<h3>\|universal template" dsa-cheatsheet.html`

- [ ] **Step 2: Migrate**

- Enrich `When to use` with the locked 4-part structure.
- Replace the `Diagram` + `How it works — The universal template` pair with
  `<h3>How it works — Subsets (universal template)</h3>` +
  `walkthrough-merged`:
  - `wm-setup`: keep the existing subsets-list / pruning text.
  - One `wm-block` wrapping the existing decision-tree ASCII unchanged; its
    `wm-note` maps the template's choose / recurse / unchoose / prune steps
    onto the tree branches (reuse the existing `<ol>` step wording).
- Verify the old `Diagram` heading is gone and one `walkthrough-merged`
  was added.
- Commit: `git commit -am "feat: merge + enrich Backtracking tab"`

### Task 8: Remove dead CSS

**Files:**
- Modify: `dsa-cheatsheet.html` (CSS rules `.diagram-box`, `.walkthrough ol`,
  `.walkthrough li`, `.walkthrough li strong`, lines ~132–151; keep
  `.ascii-diagram` and `.mermaid` — still used inside `wm-block`)

- [ ] **Step 1: Confirm nothing references the old classes**

Run: `grep -n "class=\"diagram-box\"\|class=\"walkthrough\"" dsa-cheatsheet.html`
Expected: NO matches (all tabs migrated).

- [ ] **Step 2: Remove the now-unused rules**

Use Edit to delete the `.diagram-box { … }` rule and the three
`.walkthrough …` rules. Keep `.ascii-diagram` and `.mermaid` (referenced by
`wm-block`). Keep the `/* Diagrams */` comment only if `.ascii-diagram`
stays under it.

- [ ] **Step 3: Verify**

Run: `grep -n "diagram-box\|\.walkthrough " dsa-cheatsheet.html`
Expected: NO matches. Run `grep -c "ascii-diagram" dsa-cheatsheet.html`:
expected still present (used by `wm-block`s).

- [ ] **Step 4: Final browser check + commit**

Run: `open dsa-cheatsheet.html`
Click every tab; confirm no broken layout, every tab has one merged
"How it works — X" and an enriched "When to use".

```bash
git add dsa-cheatsheet.html
git commit -m "refactor: remove dead diagram-box/walkthrough CSS"
```

---

## Self-Review

**Spec coverage:**
- Spec §1 merged section → Tasks 3, 5, 6, 7 (markup shape locked in Task 3).
- Spec §2 three archetypes → Task 5 (trace), Task 6 (table), Task 7
  (structural).
- Spec §3 enriched when-to-use → Task 2 (pilot) + per-tab in Tasks 5–7.
- Spec §4 CSS additions → Task 1.
- Spec §5 rollout / phased gates → Phase 1 (Tasks 1–4 with Task 4 gate) +
  Phase 2 (Tasks 5–8), dead-CSS removal → Task 8.
- Spec success criteria (adjacent block+note, 4-part entries, no content
  change, no dead CSS) → verification steps in Tasks 3, 4, 8.

**Placeholder scan:** Phase 1 contains full exact markup. Phase 2 tasks are
per-tab repetitions of the Task 2/Task 3 locked pattern with the source
content already in the file — concrete procedure + verify + commit, no
"TBD"/"handle edge cases". Acceptable: enumerating exact final markup for 24
tabs would duplicate content the executor reads directly from the file; the
locked pilot is the concrete template.

**Class consistency:** `walkthrough-merged`, `wm-setup`, `wm-block`,
`wm-note`, `wm-num`, `w-desc`, `w-sig`, `w-sig-label`, `w-ex` — defined in
Task 1, used identically in Tasks 2,3,5–7, removed-around in Task 8 (which
keeps `ascii-diagram`/`mermaid`). Consistent throughout.
