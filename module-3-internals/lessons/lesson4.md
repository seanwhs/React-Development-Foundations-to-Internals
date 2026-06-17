# Lesson 4 — The Complete React Execution Model (End-to-End Fiber System)

> Module 3 — Internals

---

# Learning Objectives

By the end of this lesson, you should be able to:

* Trace a full React update from start to finish
* Connect scheduling, rendering, reconciliation, and commit into one system
* Understand how Fiber coordinates all React subsystems
* Explain how React maintains consistency under interruption
* Describe the full lifecycle of a state update in React
* Build a unified mental model of React as a runtime engine
* Understand how Concurrent Rendering fits into the full pipeline

---

# Where We Are Now

Across Module 3, we’ve studied three core systems:

### 1. Fiber (Execution Unit)

> How React represents work

### 2. Reconciliation (Diffing System)

> How React decides what changed

### 3. Scheduling (Control System)

> How React decides when to do work

Now we unify everything:

> **React is a coordinated execution pipeline built on Fiber**

---

# The Big Picture

Every React update follows this full pipeline:

```text id="m1v8qz"
State Update
   ↓
Scheduler (Priority Assignment)
   ↓
Fiber Work Loop (Render Phase)
   ↓
Reconciliation (Tree Comparison)
   ↓
Effect Tagging
   ↓
Commit Phase (DOM Mutations)
   ↓
Effects Execution (useEffect)
   ↓
Browser Paint
```

This is React’s real execution model.

---

# Step 1 — State Update Begins the Cycle

Everything starts with:

```jsx id="q9x2pl"
setCount(count + 1);
```

This does NOT update the UI.

It only:

> creates a work request for React

---

# Step 2 — Scheduler Receives the Update

React immediately:

* Assigns priority
* Places update in a queue
* Decides when to process it

```text id="v7k3mn"
Update → Priority Queue → Scheduler
```

---

# Step 3 — Fiber Work Begins (Render Phase)

React begins executing Fiber nodes:

```text id="c3v9qp"
App Fiber
  → Header Fiber
  → Main Fiber
  → Button Fiber
```

Each Fiber is processed as a unit of work.

---

# Step 4 — Component Execution

React runs component functions:

```jsx id="h2q8xn"
function App() {
    return <Header />;
}
```

Becomes:

```text id="t8v1pl"
App() executes
→ returns React Elements
```

---

# Step 5 — Hooks Are Resolved from Fiber

During execution:

* React reads Hook state from Fiber
* Stores updated Hook results back into Fiber

```text id="k6q9wx"
Fiber
 ├── useState
 ├── useEffect
 ├── useMemo
```

Hooks are not re-created—they are reused.

---

# Step 6 — Reconciliation Happens

React compares:

```text id="n1v7qp"
Previous Fiber Tree
        vs
New Fiber Tree
```

It decides:

* What stays
* What changes
* What is removed
* What is added

---

# Step 7 — Effect Tags Are Assigned

React marks each Fiber:

```text id="x4q8mz"
Placement → New node
Update → Props changed
Deletion → Remove node
```

These flags guide the commit phase.

---

# Step 8 — Work-In-Progress Tree Becomes Ready

React builds a full alternate tree:

```text id="p9v2qw"
Current Tree
     ↓
Work Tree (complete)
```

Only when complete does React proceed.

---

# Step 9 — Commit Phase (Final Application)

Now React applies changes:

```text id="b7x1nm"
DOM Insertions
DOM Updates
DOM Removals
```

This phase is:

> synchronous and non-interruptible

---

# Step 10 — Effects Execute After Commit

After DOM is stable:

```text id="z2v9qx"
useEffect runs
```

This ensures:

* UI is fully updated
* DOM is consistent
* Side effects are safe

---

# The Full React Pipeline (Unified View)

```text id="u8v3qp"
setState()
   ↓
Scheduler (priority + queue)
   ↓
Render Phase (Fiber execution)
   ↓
Component functions run
   ↓
Hooks resolved from Fiber
   ↓
Reconciliation (diffing)
   ↓
Effect flags assigned
   ↓
Commit Phase (DOM updates)
   ↓
useEffect execution
   ↓
Browser paint
```

---

# React as a Pipeline System

React is not one process.

It is a **pipeline of coordinated systems**:

| Stage          | Responsibility             |
| -------------- | -------------------------- |
| Scheduler      | When work runs             |
| Fiber          | How work is structured     |
| Reconciliation | What changed               |
| Commit         | Apply changes              |
| Effects        | Sync with external systems |

---

# Why Fiber Is Central

Fiber connects everything:

* State lives in Fiber
* Hooks live in Fiber
* Work is Fiber units
* Scheduling operates on Fiber
* Reconciliation modifies Fiber

React is essentially:

> A Fiber execution engine with layered control systems

---

# Interruptions in the Pipeline

React can interrupt ONLY during:

* Scheduling
* Render phase
* Reconciliation

React cannot interrupt:

* Commit phase
* DOM mutation phase

---

# Example: Complex Update Flow

User types in input:

```jsx id="q1v8zx"
setText("Hello");
```

React does:

```text id="k9v3qp"
1. Schedule update (high priority)
2. Start rendering Fiber
3. Build new UI tree
4. Compare with previous tree
5. Detect text change
6. Commit minimal DOM update
7. Run effects if needed
```

---

# Concurrent Rendering in Context

Concurrent rendering means:

> React can pause Step 2–4 and resume later

This allows:

* Smooth typing
* Responsive UI
* Prioritized updates

---

# Why React Feels Instant

Even though React:

* Re-runs components
* Rebuilds UI trees
* Compares structures

It feels fast because:

* Work is split into small chunks
* Only differences are committed
* High priority updates interrupt low priority work

---

# Mental Model Shift

At this point you should stop thinking:

❌ React updates UI

Instead think:

✔ React executes a scheduled computation pipeline

---

# Common Misconceptions (Fixed)

❌ React mutates DOM directly
✔ React builds a new UI tree and commits changes

❌ Components update themselves
✔ Components are re-executed

❌ State changes instantly
✔ State changes are scheduled work

❌ Re-render means DOM update
✔ Re-render means function re-execution

---

# Key Takeaways

* React updates follow a strict multi-stage pipeline
* Scheduler determines WHEN work happens
* Fiber structures all rendering work
* Reconciliation determines WHAT changes
* Commit applies changes to the DOM
* Effects run after commit
* Work can be interrupted before commit
* React is a coordinated execution engine, not a renderer

---

# Summary

This lesson unifies React’s internal architecture into a single coherent system. Every state update flows through scheduling, rendering, reconciliation, and committing before producing a final UI update. Fiber acts as the central execution layer, enabling React to pause, resume, prioritize, and coordinate rendering work efficiently.

Understanding this pipeline means you now understand React not as a UI library, but as a structured runtime system designed to execute and manage UI computation over time.

---

# Module 3 Complete

You now understand:

* Fiber execution model
* Reconciliation system
* Scheduling system
* Time slicing and concurrency
* Full React render pipeline

---

# Next Module Preview

> **Module 4 — Performance & Advanced React**

We will now move from internals → optimization and real-world architecture:

* Memoization strategies (`useMemo`, `useCallback`, `React.memo`)
* Render optimization patterns
* Concurrent React in practice
* Suspense and lazy loading
* Server Components
* Performance profiling using React DevTools

This is where understanding internals becomes practical engineering leverage.
