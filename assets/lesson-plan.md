# 📄 `assets/lesson-plan.md`

# Lesson Plan — React Development: Foundations to Internals

This document defines the **instructional flow** of the entire curriculum.

It is designed to ensure progressive mental model construction across all modules.

---

# 1. Curriculum Structure Overview

```text id="cp1k9q"
Module 1 → Foundations
Module 2 → State & Hooks
Module 3 → Internals (Fiber & Reconciliation)
Module 4 → Performance & Concurrency
Module 5 → Capstone Synthesis Project
````

Each module builds on the previous one.

No module is independent.

---

# 2. Teaching Progression Model

Each lesson follows a strict cognitive progression:

```text id="tp1k9q"
Problem → Limitation → React Solution → Internal Model → Mental Model → Exercise
```

This ensures students understand:

* WHY React exists
* HOW React works
* WHAT React is doing internally

---

# 3. Module Breakdown

---

## Module 1 — Foundations

### Goal:

Understand React as a declarative UI system.

### Topics:

* JSX
* Components
* Props
* State basics
* UI = f(State)

### Outcome:

Students can build simple component trees.

---

## Module 2 — State & Hooks

### Goal:

Understand React’s runtime behavior.

### Topics:

* useState
* useEffect
* lifecycle
* re-rendering
* Hooks rules

### Outcome:

Students understand how state triggers rendering.

---

## Module 3 — Internals

### Goal:

Understand React’s execution engine.

### Topics:

* Fiber architecture
* reconciliation
* render vs commit
* work scheduling
* interruptible rendering

### Outcome:

Students can mentally simulate React execution.

---

## Module 4 — Performance & Concurrency

### Goal:

Understand React optimization strategy.

### Topics:

* memoization
* React.memo / useMemo / useCallback
* concurrent rendering
* transitions
* server components

### Outcome:

Students can reason about performance trade-offs.

---

## Module 5 — Capstone

### Goal:

Apply full-stack React reasoning.

### Topics:

* architecture design
* state modeling
* performance-aware structure
* scalable component systems

### Outcome:

Students build production-grade React systems.

---

# 4. Standard Lesson Structure

Each lesson must follow this format:

---

## 1. Concept Introduction

Introduce problem or limitation.

---

## 2. React Solution

Show how React solves it.

---

## 3. Code Example

Minimal, focused implementation.

---

## 4. Internal Model

Explain what React is doing internally:

* render phase
* fiber work
* reconciliation
* commit phase

---

## 5. Mental Model Summary

Compress concept into:

> one principle or equation

---

## 6. Exercise

Hands-on reinforcement.

---

# 5. Instructor Timing Guidelines

Recommended pacing:

| Section              | Time      |
| -------------------- | --------- |
| Concept Introduction | 10–15 min |
| Deep Explanation     | 15–25 min |
| Code Walkthrough     | 10–20 min |
| Mental Model Review  | 5–10 min  |
| Exercise             | 20–40 min |

---

# 6. Student Cognitive Goals

At each stage students should be able to:

## Foundation Level

* describe what React is
* build simple components

## Intermediate Level

* understand state and lifecycle
* predict re-renders

## Advanced Level

* simulate Fiber execution
* reason about reconciliation
* evaluate performance trade-offs

---

# 7. Key Teaching Constraint

Do NOT introduce:

* advanced APIs before mental model readiness
* optimization techniques before measurement understanding
* Fiber internals before state/lifecycle clarity

---

# 8. Success Criteria

A successful student can:

* explain React without mentioning APIs
* reason about rendering steps
* debug UI behavior logically
* design scalable component structures

---

# 9. Final Principle

> The goal is not React usage.

The goal is React reasoning.

---

# End of Lesson Plan

```
```
