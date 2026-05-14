# DSA Interview Cheatsheet — Design Spec

**Date:** 2026-05-14
**Deliverable:** Single HTML file, no dependencies, open in any browser

---

## Goal

A night-before interview study reference covering 16 core algorithms at easy/medium LeetCode difficulty. Targeted at a mid-size startup interview. User is comfortable with arrays/strings; needs stronger coverage on trees, graphs, DP, and backtracking.

---

## Output

- **File:** `dsa-cheatsheet.html` in project root
- **Format:** Single self-contained HTML file (inline CSS + JS + Mermaid loaded via CDN script tag for diagram rendering)
- **Language for code samples:** TypeScript

---

## Layout

Tabbed single-page UI:
- 16 tabs in a scrollable tab bar across the top
- One tab active at a time; content renders below
- No frameworks — pure HTML/CSS/JS

---

## Tabs (17)

1. Two Pointers
2. Sliding Window
3. Prefix Sums
4. Hash Maps & Sets
5. Fast & Slow Pointers
6. Linked List Reversal
7. Tree DFS
8. Tree BFS
9. BST
10. Graph BFS
11. Graph DFS
12. Binary Search
13. Sorting
14. Monotonic Stack
15. 1D DP
16. 2D DP
17. Backtracking

---

## Per-Tab Structure

Each tab renders these sections in order:

### 1. Header
- Algorithm name (large)
- One-sentence description
- "When to reach for this" — 3–4 bullet recognition cues (e.g. *"Sorted array + find pair that sums to target"*)

### 2. Diagram
- Mermaid diagrams for: recursion trees, graph/tree traversals, DP state tables, decision flows, pointer-step sequences
- Colored nodes/edges to distinguish: current node, visited, active path, left/right subtrees, base cases vs computed cells
- ASCII array annotations for simpler linear structures (Two Pointers, Sliding Window, Prefix Sums) — cleaner than a diagram for 1D array manipulation
- Diagrams are most detailed for: Backtracking, 1D DP, 2D DP, Tree DFS, Graph BFS/DFS

### 3. Step-by-step walkthrough
- Plain English explanation tied directly to the diagram
- Numbered steps matching diagram state

### 4. Code template (TypeScript)
- Reusable pattern skeleton, not a fully solved problem
- Inline comments only where non-obvious
- Syntax-highlighted code block

### 5. Practice problems
- 2–3 named LeetCode problems
- Each has: problem name + the key insight that maps it to this algorithm
- Difficulty label (Easy / Medium)

---

## Diagram Strategy by Topic

| Topic | Diagram Type | Color Use |
|---|---|---|
| Two Pointers | ASCII array with pointer arrows | left=blue, right=red |
| Sliding Window | ASCII array with window bracket | window=green, current=yellow |
| Prefix Sums | ASCII array with cumulative sum row | sum cells=purple |
| Hash Maps & Sets | ASCII key→value table | hit=green, miss=red |
| Fast & Slow Pointers | Mermaid flowchart (linked list nodes) | slow=blue, fast=orange, cycle=red |
| Linked List Reversal | Mermaid sequence (pointer steps) | prev=blue, curr=green, next=orange |
| Tree DFS | Mermaid graph (tree + traversal order labels) | visited=teal, current=yellow |
| Tree BFS | Mermaid graph (tree + level color bands) | level 0=blue, 1=green, 2=orange |
| BST | Mermaid graph (tree + comparison path) | search path=yellow, target=red |
| Graph BFS | Mermaid graph (nodes + BFS frontier) | visited=gray, frontier=blue, start=green |
| Graph DFS | Mermaid graph (nodes + DFS stack path) | visited=gray, current path=orange |
| Binary Search | ASCII array with lo/mid/hi annotations | lo=blue, hi=red, mid=green |
| Monotonic Stack | ASCII stack + array walk | stack contents=purple, popped=red |
| 1D DP | Mermaid graph (decision tree + memo table) | base case=green, computed=blue, target=red |
| 2D DP | ASCII/HTML grid table with cell coloring | base row/col=green, computed=blue, answer=red |
| Backtracking | Mermaid graph (full decision/recursion tree) | chosen path=green, pruned=red, leaf=yellow |

---

## Content Priorities

- **Highest diagram effort:** Backtracking, 1D DP, 2D DP, Graph BFS/DFS, Tree DFS
- **Sorting:** Tab 13, kept brief — merge sort concept (divide & conquer), quicksort idea, and TypeScript built-in `.sort()` with custom comparator. Lower priority than the rest.
- Arrays/strings covered thoroughly for completeness even though user is comfortable

---

## Non-Goals

- No dark/light mode toggle
- No print stylesheet
- No external fonts or network requests beyond one CDN script tag for Mermaid
- No backend
