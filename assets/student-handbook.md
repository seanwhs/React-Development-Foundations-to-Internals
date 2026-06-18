# 📄 `assets/student-handbook.md`

# Student Handbook — React Development: Foundations to Internals

This handbook defines how you should approach this curriculum as a learner, builder, and engineer.

It is not optional reading.

It is part of the system.

If you skip this, everything else becomes “API memorization” instead of “system understanding”.

---

# 0. How to Read This Curriculum (Critical)

Most React learners fail for one reason:

> They learn React as a set of APIs instead of a runtime system.

This curriculum is designed to reverse that failure mode.

You are expected to:

- think in execution steps
- visualize trees
- simulate rendering mentally
- reason about scheduler behavior

If you cannot simulate React in your head, you do not understand React yet.

---

# 1. Philosophy of This Curriculum

This course is NOT:

- a tutorial collection
- a syntax reference
- a “follow-along React guide”
- a checklist of Hooks to memorize

This course IS:

> A mental model training system for React as a deterministic execution engine.

You are learning:

- how React executes your code
- how React schedules rendering work
- how React transforms state into UI
- how React minimizes DOM operations
- how React prioritizes user interactions

---

## 1.1 React is a System, Not a Library

A library gives you functions.

React gives you:

- a rendering model
- a scheduling model
- a reconciliation model
- a lifecycle model
- a component execution model

If you treat React like jQuery with JSX, you will hit a ceiling quickly.

---

## 1.2 React is Deterministic

Given the same:

- state
- props
- component structure

React will always produce the same output.

Any “magic” feeling comes from:

- hidden scheduling complexity
- batching updates
- reconciliation heuristics

Not randomness.

---

# 2. What You Are Expected to Learn

By the end of this curriculum, you should be able to:

## 2.1 Execution Prediction

- Predict when a component will render
- Predict when children will re-render
- Predict when DOM updates occur

## 2.2 Internal Reasoning

- Explain Fiber traversal
- Explain render vs commit phases
- Explain reconciliation decisions
- Explain hook storage behavior

## 2.3 Architecture Design

- Design scalable component trees
- Decide state placement intentionally
- Avoid unnecessary global state
- Structure feature-based modules

## 2.4 Performance Reasoning

- Identify unnecessary renders
- Understand why they happen
- Fix them without guessing
- Avoid over-optimization traps

---

# 3. Learning Model (Core Loop)

You will follow this loop:

```text id="lm2k9q"
Concept → Mental Model → Simulation → Code → Observation → Correction
````

Each stage matters.

---

## 3.1 Concept

Understand the idea at a high level.

Example:

> “State triggers re-render”

---

## 3.2 Mental Model

Translate into execution flow:

```text id="mm2k9q"
setState()
 → schedule update
 → render phase
 → reconciliation
 → commit
```

---

## 3.3 Simulation

Before coding, predict:

* what will render
* what will re-run
* what will not change

---

## 3.4 Code

Write minimal code to test hypothesis.

---

## 3.5 Observation

Observe actual React behavior.

---

## 3.6 Correction

Fix mental model if mismatch occurs.

This is where real learning happens.

---

# 4. Non-Negotiable Principles

---

## 4.1 Never Skip Mental Models

If you cannot explain:

* render vs commit
* state update flow
* reconciliation process
* Fiber execution model

You are operating at API level, not system level.

---

## 4.2 Code Is Not Truth

Code is:

> a manifestation of React’s execution model

Not the definition of it.

React internals define what code *means*.

---

## 4.3 Avoid Premature Optimization

Do NOT default to:

* `useMemo`
* `useCallback`
* `React.memo`

These are:

> performance tools, not structural tools

Use them only when:

* you have measured a problem
* you understand the render cause
* you can justify the trade-off

---

## 4.4 Think in Trees Always

React is fundamentally tree transformation.

You must visualize:

* Component Tree (your code)
* React Element Tree (JSX output)
* Fiber Tree (execution units)
* Work-In-Progress Tree (current render cycle)

If you cannot visualize these, you will struggle in Modules 3–4.

---

## 4.5 State is Not Storage — It is a Trigger

State is not:

* a database
* a cache
* a variable container

State is:

> a signal that triggers React’s execution pipeline

---

# 5. Mental Models You Must Internalize

---

## 5.1 UI Equation

```text id="ui2k9q"
UI = f(State)
```

Everything in React reduces to this.

---

## 5.2 Rendering Pipeline

```text id="rp2k9q"
State Change
   ↓
Render Phase (compute UI)
   ↓
Reconciliation (diff)
   ↓
Commit Phase (DOM update)
   ↓
Paint
```

---

## 5.3 Fiber Execution Model

```text id="fb2k9q"
Component → Fiber Node → Work Unit → Scheduler → Commit
```

---

## 5.4 Update Lifecycle

```text id="ul2k9q"
setState → Queue Update → Schedule Work → Render → Diff → Commit
```

---

# 6. Common Failure Modes

---

## 6.1 “React is Magic”

Wrong model:

* React “decides what to do”

Correct model:

* React executes deterministic steps on a tree

---

## 6.2 Confusing Render and Commit

| Phase  | Meaning      |
| ------ | ------------ |
| Render | computation  |
| Commit | DOM mutation |

Most bugs come from confusing these.

---

## 6.3 Treating Hooks as Features

Hooks are NOT:

* utilities
* helpers
* magic functions

Hooks are:

> controlled access points into React’s Fiber system

---

## 6.4 Overusing Global State

Global state is not default architecture.

It is:

* a scaling decision
* a coupling trade-off
* a performance consideration

---

## 6.5 Premature Memoization

Memoization does NOT mean:

* faster UI automatically

It means:

* less recomputation at the cost of comparison overhead

---

# 7. How to Study This Curriculum

---

## Step 1 — Read Concept

Do not rush.

Understand intent first.

---

## Step 2 — Draw the System

Always draw:

* component tree
* render flow
* state transitions

---

## Step 3 — Predict Behavior

Before running code:

> What will React do?

---

## Step 4 — Implement Minimal Code

Avoid complexity.

One idea per file.

---

## Step 5 — Break Things Intentionally

Create:

* infinite renders
* stale closures
* missing dependencies
* unnecessary memoization

---

## Step 6 — Explain Out Loud

If you cannot explain it simply:

* your mental model is incomplete

---

# 8. What Mastery Looks Like

You are proficient when you can:

## 8.1 Prediction

* predict renders without DevTools
* explain update chains

## 8.2 Internal Reasoning

* describe Fiber traversal
* explain reconciliation decisions
* explain hook storage behavior

## 8.3 Architecture Design

* design scalable React systems
* place state intentionally
* avoid over-globalization

## 8.4 Debugging Ability

* fix performance issues without guessing
* identify root cause of re-renders

---

# 9. Advanced Insight (Important)

React is not optimized for:

* minimal renders
* minimal execution
* minimal function calls

React is optimized for:

> correct UI output under a controllable execution model

Performance comes from:

* scheduling
* batching
* reconciliation heuristics

not from avoiding renders entirely.

---

# 10. Final Principle

> React is not something you use.

> React is something you reason about.

If you treat it as a system, you gain control.

If you treat it as a library, it controls you.

---

# End of Handbook

```
```
