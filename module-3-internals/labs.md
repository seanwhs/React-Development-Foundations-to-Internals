# 📁 `module-3-internals/labs.md`

# Labs — Module 3: Internals (Fiber & Reconciliation)

This module moves you from:

> “React as a component system”

to:

> “React as an execution engine”

You will now simulate how React actually runs your application.

---

# Lab 1 — Why Fiber Exists

## Objective

Understand the limitation of recursive rendering.

---

## Thought Experiment (No Code First)

Imagine a UI with 10,000 components.

Questions:

- What happens if React renders everything synchronously?
- What happens to input responsiveness?
- What happens to animations?

---

## Expected Insight

> Without interruption, rendering blocks the main thread.

---

# Lab 2 — Simulating the Stack Reconciler Problem

## Objective

Understand pre-Fiber rendering model.

---

## Conceptual Model

```text id="lab2m1"
App
→ Header
→ Sidebar
→ Content
→ Footer
````

Now imagine recursion:

```text id="lab2m2"
render(App)
  render(Header)
  render(Sidebar)
  render(Content)
  render(Footer)
```

---

## Problem

* cannot pause
* cannot prioritize
* blocks UI thread

---

## Insight

> React needed a way to pause rendering work.

---

# Lab 3 — Introducing Fiber Units

## Objective

Understand Fiber as unit of work.

---

## Concept

Each component becomes a Fiber node:

```text id="lab3m1"
Component → Fiber Node → Work Unit
```

---

## Structure

```js id="lab3m2"
Fiber = {
  type,
  props,
  child,
  sibling,
  return,
  alternate
}
```

---

## Questions

* Why is tree linked, not recursive?
* Why do we need sibling pointers?

---

## Insight

> Fiber converts recursion into iteration-friendly linked structure.

---

# Lab 4 — Fiber Tree Traversal Simulation

## Objective

Manually simulate traversal.

---

## Given Tree

```text id="lab4m1"
App
├── A
├── B
│   ├── B1
│   └── B2
└── C
```

---

## Traversal Rule

React visits:

1. child
2. sibling
3. return

---

## Exercise

Write traversal order:

---

## Expected Answer

```text id="lab4m2"
App
→ A
→ B
→ B1
→ B2
→ C
```

---

## Insight

> Fiber traversal is controlled, not recursive.

---

# Lab 5 — Render vs Commit Separation

## Objective

Understand React’s two-phase system.

---

## Task

Simulate mentally:

```text id="lab5m1"
State Change → Render Phase → Commit Phase
```

---

## Rules

### Render Phase

* pure computation
* no DOM changes
* can be paused

### Commit Phase

* DOM updates
* cannot be interrupted

---

## Questions

* Why split into two phases?
* What breaks if commit is interruptible?

---

## Insight

> React separates “calculation” from “mutation”.

---

# Lab 6 — Double Buffering Model

## Objective

Understand work-in-progress trees.

---

## Concept

```text id="lab6m1"
Current Tree
      ↕
Work-In-Progress Tree
```

---

## Simulation

1. React renders new tree
2. keeps old tree intact
3. swaps after commit

---

## Questions

* Why not mutate current tree directly?
* What happens if render is interrupted?

---

## Insight

> Double buffering ensures consistent UI state.

---

# Lab 7 — Reconciliation Simulation

## Objective

Understand diffing behavior.

---

## Task

Compare:

### Tree A

```text id="lab7m1"
App
├── A
├── B
└── C
```

### Tree B

```text id="lab7m2"
App
├── A
├── B
└── D
```

---

## Question

What changes?

---

## Expected Answer

* C removed
* D added

---

## Insight

> React compares structure, not full DOM.

---

# Lab 8 — Keys and Identity

## Objective

Understand identity tracking in lists.

---

## Example

```jsx id="lab8m1"
items.map(item => <Row key={item.id} />)
```

---

## Broken Version

```jsx id="lab8m2"
items.map((item, index) => <Row key={index} />)
```

---

## Questions

* Why is index key dangerous?
* What breaks during reorder?

---

## Insight

> Keys define identity across renders.

---

# Lab 9 — Hook Storage in Fiber

## Objective

Understand where state lives.

---

## Concept

```text id="lab9m1"
Fiber Node
  ├── Hooks
  ├── State Queue
  ├── Effects
```

---

## Questions

* Why don’t hooks live inside functions?
* What happens on re-render?

---

## Insight

> Hooks are stored in Fiber, not in component scope.

---

# Lab 10 — Full React Execution Simulation

## Objective

Simulate complete lifecycle.

---

## Flow

```text id="lab10m1"
setState
  ↓
Schedule Update
  ↓
Render Phase (Fiber Work Loop)
  ↓
Reconciliation
  ↓
Commit Phase
  ↓
DOM Update
  ↓
Paint
```

---

## Task

Explain each step in your own words.

---

## Insight

> React is a pipeline, not a library of functions.

---

# Lab Summary

After this module, you should understand:

* why Fiber exists
* how rendering is split into work units
* how traversal works
* why commit is separate
* how reconciliation minimizes work
* how keys preserve identity
* how hooks are stored internally

---

# Final Principle

> React is not executed as components.

> React is executed as a controlled traversal of a Fiber tree.

---

# End of Labs

```
```
