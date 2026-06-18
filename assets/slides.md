# 📄 `assets/slides.md`

# Slides — React Development: Foundations to Internals

This document provides a **slide-ready structure** for teaching the full curriculum.

Each section is designed to map directly to lecture slides.

---

# 1. Slide Design Principles

Each slide should:

- contain one idea only
- use minimal text
- prioritize diagrams and flows
- reinforce mental models

Avoid:

- long paragraphs
- API dumping
- over-explanation on slides

---

# 2. Global Narrative Flow

```text id="gn1k9q"
Foundations → State → Internals → Performance → Capstone
````

Each module deepens the same mental model:

> UI = f(State)

---

# 3. Module 1 — Foundations Slides

---

## Slide 1 — What is React?

* React is a UI library
* It builds interfaces from state
* It is declarative

---

## Slide 2 — Core Equation

```text id="eq1k9q"
UI = f(State)
```

---

## Slide 3 — Imperative vs Declarative

Imperative:

* “do this, then this”

Declarative:

* “this is what UI should look like”

---

## Slide 4 — Components

* Functions that return UI
* Reusable building blocks

---

## Slide 5 — Component Tree

```text id="ct1k9q"
App
├── Header
├── Main
└── Footer
```

---

# 4. Module 2 — State & Hooks Slides

---

## Slide 1 — Why State Exists

* UI must change over time
* Variables are not reactive

---

## Slide 2 — useState

```jsx id="us1k9q"
const [state, setState] = useState()
```

---

## Slide 3 — Re-render Cycle

```text id="rr1k9q"
setState → Render → Commit → UI Update
```

---

## Slide 4 — Lifecycle

* Mount
* Update
* Unmount

---

## Slide 5 — Hooks Rule

* Must be called in order
* Must be top-level

---

# 5. Module 3 — Internals Slides

---

## Slide 1 — What is Fiber?

* Execution engine of React
* Breaks rendering into units

---

## Slide 2 — Fiber Node

```js id="fn1k9q"
Fiber = {
  type,
  props,
  child,
  sibling
}
```

---

## Slide 3 — Render vs Commit

Render:

* compute UI

Commit:

* update DOM

---

## Slide 4 — Reconciliation

* compare trees
* find minimal updates

---

## Slide 5 — Work Loop

```text id="wl1k9q"
Work → Pause → Resume → Commit
```

---

# 6. Module 4 — Performance Slides

---

## Slide 1 — What is Performance?

* not avoiding renders
* reducing unnecessary work

---

## Slide 2 — Memoization Tools

* React.memo
* useMemo
* useCallback

---

## Slide 3 — Concurrent React

* interrupt rendering
* prioritize updates

---

## Slide 4 — Server Components

* run on server
* reduce client JS

---

## Slide 5 — Key Insight

Performance = control over work, not elimination of renders

---

# 7. Capstone Slides

---

## Slide 1 — Goal

Build a production-grade React system

---

## Slide 2 — Requirements

* architecture
* state design
* performance awareness

---

## Slide 3 — Evaluation

* correctness
* scalability
* clarity
* reasoning ability

---

# 8. Final Teaching Slide

> React is not a library of features.

> It is a system of computation, scheduling, and rendering.

---

# End of Slides

```
```
