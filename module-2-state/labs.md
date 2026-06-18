# 📁 `module-2-state/labs.md`

# Labs — Module 2: State & Hooks (Hooks & Lifecycle)

This module shifts your mental model from:

> “UI as static output”

to:

> “UI as a function of changing state over time”

You are now entering React’s runtime system.

---

# Lab 1 — Why State Exists

## Objective

Understand why normal variables do not trigger UI updates.

---

## Task

```jsx id="lab1a1"
function Counter() {
  let count = 0;

  function increment() {
    count++;
    console.log(count);
  }

  return (
    <>
      <h1>{count}</h1>
      <button onClick={increment}>Increment</button>
    </>
  );
}
````

---

## Experiment

Click the button multiple times.

Observe:

* console changes
* UI does NOT change

---

## Questions

* Why is UI not updating?
* What is React not tracking?
* What is missing from this model?

---

## Expected Insight

> React does not observe variables. It only reacts to state changes.

---

# Lab 2 — Introducing useState

## Objective

Understand reactive state binding.

---

## Task

```jsx id="lab2a1"
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <>
      <h1>{count}</h1>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </>
  );
}
```

---

## Experiment

* Click button
* Observe UI updates

---

## Questions

* Why does UI update now?
* What does setState trigger internally?

---

## Expected Insight

> State updates trigger a re-render cycle.

---

# Lab 3 — Re-render Understanding

## Objective

Visualize render execution.

---

## Task

Add logging:

```jsx id="lab3a1"
function Counter() {
  console.log("Render Counter");

  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      {count}
    </button>
  );
}
```

---

## Experiment

Click multiple times.

---

## Observation

* Component function runs again each time

---

## Questions

* Why does the function re-run?
* Is React “updating” or “re-executing”?

---

## Expected Insight

> A render is a re-execution of the component function.

---

# Lab 4 — State is NOT a Variable

## Objective

Separate memory model from variable model.

---

## Task

```jsx id="lab4a1"
function Counter() {
  let count = 0;
  const [state, setState] = useState(0);

  function increment() {
    count++;
    setState(state + 1);
  }

  return (
    <>
      <p>Variable: {count}</p>
      <p>State: {state}</p>
      <button onClick={increment}>Increment</button>
    </>
  );
}
```

---

## Experiment

Click multiple times.

---

## Observation

* `count` resets on render
* `state` persists

---

## Questions

* Why does `count` reset?
* Where is state stored?

---

## Expected Insight

> State lives outside function execution — in Fiber.

---

# Lab 5 — Hook Order Rules

## Objective

Understand deterministic Hook execution.

---

## Task (Broken Example)

```jsx id="lab5a1"
function Example({ condition }) {
  if (condition) {
    const [a] = useState(1);
  }

  const [b] = useState(2);

  return <div />;
}
```

---

## Experiment

Run and observe error.

---

## Questions

* Why does React throw an error?
* Why does order matter?

---

## Expected Insight

> Hooks depend on execution order, not variable names.

---

# Lab 6 — Lifecycle Simulation

## Objective

Understand mount → update → unmount cycle.

---

## Task

```jsx id="lab6a1"
import { useEffect, useState } from "react";

function Demo() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log("Mounted or Updated");

    return () => {
      console.log("Cleanup");
    };
  });

  return (
    <button onClick={() => setCount(count + 1)}>
      {count}
    </button>
  );
}
```

---

## Experiment

Click multiple times.

---

## Observation

* effect runs after commit
* cleanup runs before next effect

---

## Questions

* When does useEffect run?
* What triggers cleanup?

---

## Expected Insight

> Effects run after commit phase, not during render.

---

# Lab 7 — State Update Flow

## Objective

Map state updates to React pipeline.

---

## Task

Mentally trace:

```jsx id="lab7a1"
setCount(count + 1);
```

---

## Write Flow

```
State Update
→ Scheduler
→ Render Phase
→ Reconciliation
→ Commit Phase
→ UI Update
```

---

## Expected Insight

> State updates are asynchronous scheduled work.

---

# Lab Summary

After this module, you should understand:

* state triggers rendering
* components re-execute on updates
* hooks are stored outside functions (Fiber)
* lifecycle = mount → update → unmount
* effects run after commit
* hook order is structural constraint

---

# Final Principle

> State is not data storage.

> State is a trigger into React’s execution engine.

---

# End of Labs
