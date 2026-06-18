# 📄 `assets/student-handbook.md`

# Student Handbook — React Development: Foundations to Internals

This handbook defines how you should approach this curriculum as a learner, builder, and engineer.

It is not optional reading.

It is part of the system.

---

# 1. Philosophy of This Curriculum

This course is NOT:

- a tutorial collection
- a syntax reference
- a “follow-along React guide”

This course IS:

> A mental model training system for React as an execution engine.

You are learning:

- how React thinks
- how React schedules work
- how React builds UI
- how React optimizes rendering

---

# 2. What You Are Expected to Learn

By the end of this curriculum, you should be able to:

- Predict when React re-renders components
- Understand Fiber-level execution flow
- Reason about reconciliation outcomes
- Identify unnecessary renders
- Design scalable React architectures
- Debug performance issues without guessing

---

# 3. Learning Model

You will follow this loop:

```text id="lm1k9q"
Concept → Mental Model → Code → Observation → Correction
````

You are NOT learning by memorization.

You are learning by simulation and reasoning.

---

# 4. Non-Negotiable Principles

## 4.1 Never Skip Mental Models

If you cannot explain:

* render vs commit
* state updates
* reconciliation
* Fiber

You are not ready to optimize React.

---

## 4.2 Code Is Evidence, Not Truth

Code does not define React behavior.

React’s execution model defines what code means.

---

## 4.3 Avoid Premature Optimization

Do NOT use:

* `useMemo` everywhere
* `React.memo` everywhere
* `useCallback` everywhere

Optimization is a response to measurement, not habit.

---

## 4.4 Think in Trees

React is fundamentally a tree system:

* Component Tree
* React Element Tree
* Fiber Tree
* Work-in-Progress Tree

Everything is a transformation of trees.

---

# 5. Mental Models You Must Internalize

## 5.1 UI Model

```text id="ui1k9q"
UI = f(State)
```

---

## 5.2 Rendering Model

```text id="rm1k9q"
State Change
→ Render Phase
→ Reconciliation
→ Commit Phase
→ DOM Update
```

---

## 5.3 Fiber Model

```text id="fb1k9q"
Component → Fiber → Work Unit → Scheduler → Commit
```

---

# 6. Common Failure Modes

## 6.1 “React is magic”

React is NOT magic.

It is deterministic execution + scheduling.

---

## 6.2 Confusing Render with DOM Update

Rendering is NOT:

* painting UI
* updating browser

Rendering IS:

* executing functions
* building UI description

---

## 6.3 Overusing Hooks

Hooks are not features.

Hooks are interfaces to React internals.

---

# 7. How to Study This Curriculum

## Step 1 — Read Concept

Understand theory first.

## Step 2 — Draw Mental Model

Always visualize:

* trees
* flows
* execution steps

## Step 3 — Code Implementation

Write minimal examples.

## Step 4 — Break Things

Intentionally create:

* re-render bugs
* stale state bugs
* effect loops

## Step 5 — Explain Out Loud

If you cannot explain it simply, you do not understand it.

---

# 8. What Mastery Looks Like

You have reached proficiency when you can:

* look at a component and predict render behavior
* explain Fiber traversal without code
* debug performance without Profiler first
* design component trees intentionally
* reason about state placement logically

---

# 9. Final Principle

> React is not a UI library you use.

It is a system you reason about.

---

# End of Handbook
