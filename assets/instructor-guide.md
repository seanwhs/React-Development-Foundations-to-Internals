# 📄 `assets/instructor-guide.md`

# Instructor Guide — React Development: Foundations to Internals

This guide is intended for instructors, mentors, and reviewers facilitating this curriculum.

It defines **how to teach**, not what React is.

---

# 1. Instructor Philosophy

Your role is not to explain React.

Your role is to:

- shape mental models
- correct misconceptions
- enforce reasoning discipline
- prevent premature abstraction
- guide system-level thinking

---

# 2. Teaching Strategy

This curriculum is built on a **progressive mental model stack**:

```text id="tg1k9q"
Foundations → State → Internals → Performance → Synthesis
````

Each module depends on the previous one.

Do NOT skip layers.

---

# 3. Core Teaching Principle

> Students do not fail because they lack knowledge.
> They fail because their mental model is inconsistent.

Your job is to detect and correct:

* incorrect mental models
* shallow memorization
* API-first thinking
* confusion between render and commit
* misunderstanding of Fiber

---

# 4. What to Emphasize

## 4.1 React is a System, Not a Library

Repeat this until it becomes intuitive:

* React is a rendering engine
* React is not a UI toolkit
* React is not DOM manipulation

---

## 4.2 Everything is a Tree Transformation

Students must internalize:

* Component Tree
* React Element Tree
* Fiber Tree
* Work-in-Progress Tree

If they cannot visualize trees, they do not understand React.

---

## 4.3 Render ≠ Commit

This is the most important distinction:

| Phase  | Meaning      |
| ------ | ------------ |
| Render | computation  |
| Commit | DOM mutation |

Students often confuse these.

---

## 4.4 Hooks Are Not Features

Hooks are:

* execution hooks into Fiber
* state bindings to render cycles
* not independent utilities

---

# 5. Common Student Misconceptions

## Misconception 1: “React updates the DOM on state change”

Correction:

React schedules a render → computes differences → commits updates.

---

## Misconception 2: “Re-renders are bad”

Correction:

Re-renders are normal and expected.

Unnecessary DOM commits are the real issue.

---

## Misconception 3: “useMemo improves performance by default”

Correction:

Memoization adds cost unless justified.

---

# 6. How to Run a Lesson

Each lesson should follow this structure:

## Step 1 — Problem Framing

Start with a limitation in vanilla JS.

---

## Step 2 — Introduce React Solution

Show how React abstracts the problem.

---

## Step 3 — Reveal Internal Model

Expose:

* render phase
* fiber behavior
* reconciliation

---

## Step 4 — Mental Model Check

Ask:

* What happens on state change?
* What runs again?
* What is reused?

---

## Step 5 — Code Exercise

Keep code minimal.

Focus on reasoning, not syntax.

---

# 7. Debugging Sessions (Critical)

Introduce intentional bugs:

* infinite re-render loops
* stale closures
* missing dependency arrays
* unnecessary memoization

Ask students to:

* predict behavior
* observe behavior
* explain mismatch

---

# 8. Evaluation Criteria

Students should be evaluated on:

## 8.1 Mental Model Accuracy

Can they explain:

* render vs commit
* Fiber execution
* reconciliation rules

---

## 8.2 Predictive Ability

Can they predict:

* when components re-render
* what changes in DOM
* what stays stable

---

## 8.3 System Thinking

Can they reason about:

* component architecture
* state placement
* performance boundaries

---

# 9. Red Flags

Watch for students who:

* memorize Hooks without understanding lifecycle
* treat React as DOM abstraction only
* overuse memoization
* cannot explain rendering pipeline

---

# 10. Teaching Golden Rule

> If a student can explain React in terms of functions, trees, and state transitions — they understand it.

If they cannot, they are still in API memorization mode.

---

# 11. Final Note

This curriculum is designed to produce engineers who can reason about React internals, not just use React.

Do not rush progression.

Understanding compounds.

Skipping does not.
