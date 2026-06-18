# 📄 `assets/slides.md`

# Slides — React Development: Foundations to Internals

This document provides a **slide-ready structure** for teaching the full curriculum.

It is intentionally designed as a **presentation system for mental model transfer**, not API instruction.

Each slide should compress one concept into one cognitive unit.

---

# 1. Slide Design Principles (Critical)

Each slide must satisfy:

## 1.1 One Idea Per Slide

If a slide contains multiple ideas:

- students lose causal structure
- mental models collapse
- React becomes “API soup”

---

## 1.2 Prefer Structure Over Explanation

Use:

- trees
- arrows
- flows
- comparisons

Avoid:

- paragraphs
- API definitions
- deep explanations

---

## 1.3 Slides Teach “Motion”, Not “Information”

React is dynamic.

Your slides must show:

- transitions
- execution flow
- cause → effect

---

## 1.4 Always Anchor to a Model

Every slide must reinforce at least one:

- UI = f(State)
- Render vs Commit
- Fiber execution model
- Scheduling model

---

## 1.5 Visual Hierarchy Rule

Priority order:

1. diagrams
2. arrows / flows
3. short bullet points
4. code (minimal)

---

# 2. Global Narrative Flow

The curriculum is one continuous system:

```text id="gn2k9q"
Foundations → State → Internals → Performance → Capstone
````

Each module refines the same mental model:

> UI = f(State)

But each layer adds a new dimension:

| Module      | Adds                      |
| ----------- | ------------------------- |
| Foundations | Declarative UI            |
| State       | Reactivity                |
| Internals   | Execution engine (Fiber)  |
| Performance | Scheduling & optimization |
| Capstone    | System design             |

---

# 3. Meta Slide — The React Model

This slide should appear early and repeatedly.

```text id="ms2k9q"
State
  ↓
Component Execution
  ↓
React Element Tree
  ↓
Fiber Tree
  ↓
Reconciliation
  ↓
Commit
  ↓
DOM Update
```

---

# 4. Module 1 — Foundations Slides (Expanded)

---

## Slide 1 — What is React?

React is:

* a UI library
* a declarative system
* a function-based renderer

NOT:

* DOM manipulation tool
* templating engine

---

## Slide 2 — The Core Equation

```text id="eq2k9q"
UI = f(State)
```

Interpretation:

> UI is always a derived output, never a source of truth.

---

## Slide 3 — Imperative vs Declarative

### Imperative Model

```text id="im2k9q"
DOM → manually updated step-by-step
```

* direct instructions
* fragile at scale

---

### Declarative Model

```text id="de2k9q"
State → UI description → React handles DOM
```

* describe outcome
* not steps

---

## Slide 4 — Components

A component is:

```text id="cp2k9q"
Function(state, props) → UI
```

Key idea:

> Components are pure descriptions of UI.

---

## Slide 5 — Component Tree

```text id="ct2k9q"
App
├── Header
│   ├── Logo
│   └── Nav
├── Main
│   ├── Sidebar
│   └── Content
└── Footer
```

Key idea:

> React is always a tree system.

---

## Slide 6 — JSX is Not HTML

```text id="jx2k9q"
JSX → React Elements → Fiber → DOM
```

JSX is:

* syntax
* not UI
* not DOM

---

# 5. Module 2 — State & Hooks Slides (Expanded)

---

## Slide 1 — Why State Exists

Problem:

* UI must change over time
* JS variables are not reactive

Solution:

> State is React’s reactivity system

---

## Slide 2 — useState

```jsx id="us2k9q"
const [state, setState] = useState(initialValue)
```

Key idea:

* state persists across renders
* updates trigger execution cycle

---

## Slide 3 — Re-render Cycle

```text id="rr2k9q"
setState
  ↓
Schedule Update
  ↓
Render Component Again
  ↓
Reconcile
  ↓
Commit
```

---

## Slide 4 — Component Lifecycle

```text id="lc2k9q"
Mount → Update → Unmount
```

Each phase is tied to Fiber execution.

---

## Slide 5 — Hooks Rule

Hooks depend on:

> call order, not variable name

```text id="hr2k9q"
Hook slot #1 → useState
Hook slot #2 → useEffect
```

---

## Slide 6 — Why Hooks Must Be Ordered

React tracks:

* index position in Fiber hook list

NOT:

* variable names

---

# 6. Module 3 — Internals Slides (Expanded)

---

## Slide 1 — What is Fiber?

Fiber is:

> React’s execution unit system

It replaces recursive rendering.

---

## Slide 2 — Fiber Node Structure

```js id="fn2k9q"
Fiber = {
  type,
  props,
  state,
  child,
  sibling,
  return,
  alternate
}
```

---

## Slide 3 — Render vs Commit

### Render Phase

```text id="rp2k9q"
Compute UI
No DOM changes
Interruptible
```

---

### Commit Phase

```text id="cp2k9q"
Apply DOM changes
Run effects
Non-interruptible
```

---

## Slide 4 — Reconciliation

Reconciliation answers:

> “What changed between renders?”

It compares:

* previous Fiber tree
* new Fiber tree

---

## Slide 5 — Work Loop

```text id="wl2k9q"
Fiber Work → Pause → Resume → Continue → Commit
```

---

## Slide 6 — Double Buffering

```text id="db2k9q"
Current Tree
   ↕
Work-In-Progress Tree
```

Ensures:

* consistent UI
* no partial renders

---

## Slide 7 — Why Fiber Exists

Without Fiber:

* rendering blocks UI
* no prioritization
* no interruption

With Fiber:

* scheduling becomes possible
* UI stays responsive

---

# 7. Module 4 — Performance Slides (Expanded)

---

## Slide 1 — What is Performance?

Performance is NOT:

* avoiding renders

Performance IS:

* reducing wasted work
* prioritizing important updates

---

## Slide 2 — Render Cost Breakdown

```text id="pc2k9q"
JS Execution → Cheap
Reconciliation → Medium
DOM Update → Expensive
Layout/Paint → Very Expensive
```

---

## Slide 3 — Memoization Tools

* React.memo → component level
* useMemo → value level
* useCallback → function identity

---

## Slide 4 — Referential Equality

```text id="re2k9q"
{} !== {}
[] !== []
fn !== fn
```

React compares references, not structure.

---

## Slide 5 — Concurrent React

React can:

* pause rendering
* resume later
* prioritize input

```text id="cr2k9q"
User Input → High Priority Render
Background → Low Priority Render
```

---

## Slide 6 — Transitions

```jsx id="tr2k9q"
startTransition(() => {
  setState(...)
})
```

Marks updates as non-urgent.

---

## Slide 7 — Server Components

```text id="sc2k9q"
Server → Render UI
Client → Interactivity
```

Reduces bundle size.

---

## Slide 8 — Key Insight

> Performance is scheduling, not avoidance.

---

# 8. Capstone Slides (Expanded)

---

## Slide 1 — Capstone Goal

Build a system that demonstrates:

* architecture thinking
* state modeling
* performance awareness

---

## Slide 2 — System Requirements

Must include:

* component hierarchy
* state boundaries
* side-effect management
* scalable structure

---

## Slide 3 — Engineering Evaluation

Measured by:

* clarity of design
* correctness of state flow
* performance reasoning
* maintainability

---

## Slide 4 — React Mental Model Check

You must be able to explain:

* render flow
* Fiber execution
* reconciliation
* scheduling

---

# 9. Final Slide (Repeated Across Modules)

```text id="fs2k9q"
React is not a UI library.

React is a deterministic execution engine
that turns state into a UI through scheduling and reconciliation.
```

---

# End of Slides

```
```
