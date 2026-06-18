# 📁 `module-4-performance/labs.md`

# Labs — Module 4: Performance (Memoization, Concurrent React, RSC)

This module shifts your thinking from:

> “Does this work?”

to:

> “How much work is React doing, and is it necessary?”

Performance is not optimization.

Performance is **work control inside React’s execution model**.

---

# Lab 1 — Measuring Before Optimizing

## Objective

Learn that performance starts with observation, not tools.

---

## Task

Create a component:

```jsx id="lab1m1"
function App() {
  console.log("App render");

  return <Child />;
}

function Child() {
  console.log("Child render");
  return <div>Child</div>;
}
````

---

## Experiment

Trigger re-render (add state if needed).

---

## Questions

* Which components re-render?
* Did anything “break”?
* Is this bad?

---

## Expected Insight

> React re-rendering is normal, not inherently a performance issue.

---

# Lab 2 — Understanding Render Cost

## Objective

Distinguish render cost vs DOM cost.

---

## Task

Add heavy computation:

```jsx id="lab2m1"
function Expensive() {
  const start = performance.now();

  while (performance.now() - start < 5) {}

  return <div>Heavy</div>;
}
```

---

## Experiment

Trigger multiple renders.

---

## Questions

* What causes lag?
* Is DOM involved?

---

## Insight

> Render cost = JavaScript execution cost, not DOM updates.

---

# Lab 3 — React.memo Behavior

## Objective

Understand component skipping.

---

## Task

```jsx id="lab3m1"
const Child = React.memo(function Child() {
  console.log("Child render");
  return <div>Child</div>;
});
```

---

## Experiment

Trigger parent re-render.

---

## Questions

* Does Child re-render?
* Why or why not?

---

## Insight

> React.memo prevents re-render if props are unchanged.

---

# Lab 4 — Referential Equality Problem

## Objective

Understand why memoization sometimes fails.

---

## Task

```jsx id="lab4m1"
function Parent() {
  const obj = { value: 1 };

  return <Child data={obj} />;
}
```

---

## Child

```jsx id="lab4m2"
const Child = React.memo(({ data }) => {
  console.log("Child render");
  return <div>{data.value}</div>;
});
```

---

## Experiment

Trigger parent re-render.

---

## Question

Why does Child still re-render?

---

## Insight

> New object reference breaks memoization.

---

# Lab 5 — useMemo for Stabilizing Values

## Objective

Fix referential instability.

---

## Task

```jsx id="lab5m1"
const obj = useMemo(() => {
  return { value: 1 };
}, []);
```

---

## Experiment

Pass to memoized child.

---

## Insight

> useMemo stabilizes references across renders.

---

# Lab 6 — useCallback for Function Identity

## Objective

Understand function reference stability.

---

## Task

```jsx id="lab6m1"
const handleClick = useCallback(() => {
  console.log("clicked");
}, []);
```

---

## Experiment

Pass to memoized child.

---

## Insight

> useCallback stabilizes function identity.

---

# Lab 7 — Over-Memoization Trap

## Objective

Learn when memoization hurts performance.

---

## Task

Memoize everything:

* components
* values
* functions

---

## Experiment

Observe complexity increase.

---

## Questions

* Did performance improve?
* Did code complexity increase?
* Was cost reduction measurable?

---

## Insight

> Memoization has overhead. Not always beneficial.

---

# Lab 8 — Concurrent Rendering Simulation

## Objective

Understand interruptible rendering.

---

## Concept

```text id="lab8m1"
Render A
Pause
User Input
Resume Render
Commit
```

---

## Task

Simulate UI update with heavy render + input.

---

## Questions

* What stays responsive?
* Why does UI not freeze?

---

## Insight

> React prioritizes user input over background rendering.

---

# Lab 9 — useTransition

## Objective

Mark non-urgent updates.

---

## Task

```jsx id="lab9m1"
startTransition(() => {
  setFilter(value);
});
```

---

## Experiment

Filter large dataset.

---

## Questions

* What remains responsive?
* What is deferred?

---

## Insight

> Transitions lower update priority.

---

# Lab 10 — useDeferredValue

## Objective

Delay expensive derived UI updates.

---

## Task

```jsx id="lab10m1"
const deferredValue = useDeferredValue(input);
```

---

## Experiment

Type rapidly into input.

---

## Questions

* Why does UI lag reduce?
* What is being deferred?

---

## Insight

> Deferred values lag behind urgent updates.

---

# Lab 11 — Server Components Mental Model

## Objective

Understand server vs client split.

---

## Task

Conceptual separation:

```text id="lab11m1"
Server:
  fetch data

Client:
  render UI
  handle events
```

---

## Questions

* What runs in browser?
* What is removed from bundle?

---

## Insight

> Server Components reduce client-side JavaScript.

---

# Lab 12 — Full Performance Pipeline

## Objective

Integrate entire mental model.

---

## Flow

```text id="lab12m1"
State Update
  ↓
Render Phase
  ↓
Memoization Checks
  ↓
Reconciliation
  ↓
Scheduling Priority
  ↓
Commit Phase
  ↓
DOM Update
```

---

## Task

Explain where each optimization applies.

---

## Insight

> Performance is distributed across the entire React pipeline.

---

# Lab Summary

After this module, you should understand:

* rendering is not the problem
* unnecessary work is the problem
* memoization controls recomputation
* referential equality is critical
* concurrency improves responsiveness
* server components reduce client load
* optimization is system-level, not local

---

# Final Principle

> React performance is not about doing less rendering.

> It is about doing the right rendering at the right time.

---

# End of Labs
```
