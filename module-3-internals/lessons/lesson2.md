# Lesson 2 — Reconciliation: How React Diffs the Fiber Tree

> Module 3 — Internals

---

# Learning Objectives

By the end of this lesson, you should be able to:

* Explain what reconciliation is in React
* Understand how React compares two Fiber trees
* Describe why React does NOT use a full tree comparison algorithm
* Understand element type vs identity changes
* Explain how keys affect list reconciliation
* Describe how components are mounted, updated, and unmounted
* Understand how state is preserved across renders
* Connect reconciliation to the Render and Commit phases

---

# Where We Are in the Architecture

From Lesson 1:

```text
State Update
   ↓
Fiber Scheduler
   ↓
Render Phase
   ↓
Work-in-Progress Fiber Tree
```

Now we focus on what happens inside that render phase:

> **How does React decide what changed?**

The answer is **Reconciliation**.

---

# What is Reconciliation?

Reconciliation is React’s process of:

> Comparing the previous Fiber tree with the new one to determine the minimal set of changes required.

In simpler terms:

```text
Old UI Tree + New UI Tree → Difference → DOM Updates
```

But importantly:

> React does NOT compare DOM trees.

It compares **Fiber trees**.

---

# Why Reconciliation Exists

Without reconciliation:

```text
State change → Rebuild entire UI → Replace entire DOM
```

This would be extremely inefficient.

Problems:

* Expensive DOM operations
* Layout thrashing
* Poor performance on large trees
* No reuse of existing state

So React optimizes by:

> Reusing as much of the existing Fiber tree as possible.

---

# The Core Idea

React assumes:

> If something looks the same, it probably is the same.

So instead of deep comparison, React uses heuristics.

---

# Step 1 — Same Type = Reuse Fiber

If element type is the same:

```jsx
<div />
<div />
```

React:

* Reuses the existing Fiber node
* Updates props if needed

---

# Step 2 — Different Type = Replace Entire Subtree

```jsx
<div />
<span />
```

React:

* Destroys old Fiber subtree
* Creates a new one

This is important:

> Type change = full remount

---

# Step 3 — Props Comparison

If type is the same:

```jsx
<button disabled={true} />
<button disabled={false} />
```

React:

* Keeps the same Fiber
* Updates only changed props

---

# Key Insight

Reconciliation is NOT deep comparison.

It is:

> A shallow, type-based heuristic algorithm.

---

# Component Reconciliation

Consider:

```jsx
function App() {
    return <Header />;
}
```

Then:

```jsx
function App() {
    return <Sidebar />;
}
```

React:

* Unmounts `Header Fiber`
* Mounts `Sidebar Fiber`

Even though both are components.

---

# Element Identity vs Component Identity

React distinguishes:

## Element Identity

```jsx
<h1 />
```

## Component Identity

```jsx
<Header />
```

Both are treated as **types**, not instances.

If type changes → replace subtree.

---

# Lists Are the Hard Part

Now we reach the most complex case:

```jsx
<ul>
  <li>Apple</li>
  <li>Banana</li>
  <li>Mango</li>
</ul>
```

---

## Case 1 — Without Keys

```jsx
<ul>
  <li>Apple</li>
  <li>Mango</li>
</ul>
```

React uses **index-based matching**:

```text
0 → Apple
1 → Banana → Mango
```

This causes:

* Wrong reuse
* Incorrect updates
* State bugs in child components

---

## Case 2 — With Keys (Correct Model)

```jsx
<ul>
  <li key="a">Apple</li>
  <li key="b">Banana</li>
  <li key="c">Mango</li>
</ul>
```

React now tracks identity:

```text
a → Apple
b → Banana
c → Mango
```

After update:

```jsx
<ul>
  <li key="a">Apple</li>
  <li key="c">Mango</li>
</ul>
```

React:

* Keeps `a`
* Removes `b`
* Keeps `c`

---

# Why Keys Matter

Keys define:

> Stable identity across renders

Not performance.

Not ordering.

But identity tracking.

---

# Reconciliation Strategy (Simplified Model)

React uses a deterministic process:

### Step 1 — Compare type

### Step 2 — Compare key (if list)

### Step 3 — Decide reuse or replace

### Step 4 — Update props

### Step 5 — Mark effects

---

# Fiber-Level Reconciliation

At Fiber level:

```text
Current Fiber Tree
        ↓
New Work-in-Progress Tree
        ↓
Diffing Phase
        ↓
Effect Flags Set
```

Instead of computing DOM differences directly, React marks Fibers with:

* Placement
* Update
* Deletion

---

# Effect Flags (Conceptual Model)

Each Fiber is tagged:

```text
No flag → unchanged
Update → props changed
Placement → new node
Deletion → remove node
```

These flags are used in Commit Phase.

---

# Important Insight

Reconciliation does NOT touch the DOM.

It only prepares instructions.

---

# Mount vs Update

## Mount

```text
No previous Fiber
→ Create new Fiber
→ Build DOM
```

---

## Update

```text
Existing Fiber
→ Compare with new tree
→ Reuse or modify
```

---

# State Preservation Rule

State is preserved when:

> Fiber type AND position remain stable

Example:

```jsx
<Counter />
```

If Counter stays in same place → state preserved.

If moved or replaced → state resets.

---

# Key Insight

State belongs to Fiber, not component function.

---

# Reconciliation is NOT a Diff Algorithm (Strictly)

It is NOT:

* Levenshtein diff
* Tree edit distance
* Deep recursive comparison

It is:

> A heuristic, O(n) traversal with identity checks

---

# Why React Uses Heuristics

Because:

* UI trees can be huge
* Full diffing is expensive
* React must remain fast under user interaction

So React chooses:

> Predictability over perfect diffing

---

# Reconciliation and Performance

React optimizes by:

* Reusing Fibers
* Skipping unchanged branches
* Avoiding deep comparisons
* Using keys for fast identity resolution

---

# Full Reconciliation Pipeline

```text
Render Phase
   ↓
Execute Components
   ↓
Generate React Elements
   ↓
Compare with Previous Fiber Tree
   ↓
Mark Differences (Effects)
   ↓
Commit Phase Applies Changes
```

---

# Real Mental Model

Think of reconciliation as:

> React rewriting the UI blueprint and highlighting changes

Not:

> React editing the DOM directly

---

# Common Misconception

❌ React compares DOM nodes
✔ React compares Fiber nodes

❌ React checks visual differences
✔ React checks structure + identity

❌ React updates everything
✔ React updates only marked Fibers

---

# Why This Matters

Reconciliation explains:

* Why keys are required
* Why state resets sometimes
* Why list bugs happen
* Why components remount unexpectedly
* Why React feels fast even with re-renders

---

# Key Takeaways

* Reconciliation compares previous and new Fiber trees
* React uses heuristics, not deep diffing
* Type changes = full subtree replacement
* Keys provide stable identity in lists
* State is tied to Fiber, not function
* Reconciliation produces “effect flags”
* No DOM work happens during reconciliation
* Commit phase applies reconciliation results

---

# Summary

Reconciliation is the decision-making layer of React’s rendering engine. It determines how the UI changes by comparing Fiber trees and applying structural heuristics. Instead of performing expensive deep comparisons, React relies on type identity and keys to efficiently reuse or replace portions of the tree.

This system allows React to maintain performance while still producing predictable UI updates.

---

# Looking Ahead

In Lesson 3, we move deeper into execution:

> **How React schedules rendering work using Fiber**

We will explore:

* Scheduling priorities
* Time slicing behavior
* Interruptible rendering
* Concurrent rendering model
* How React breaks rendering into units of work
* How updates are prioritized and deferred

This is where React transitions from a diffing system → to a scheduling engine.
