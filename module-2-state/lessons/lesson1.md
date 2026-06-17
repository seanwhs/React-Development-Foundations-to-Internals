# Lesson 1 — State, Hooks, and the React Lifecycle

> Module 2 — State Management

---

# Learning Objectives

By the end of this lesson, you should be able to:

* Explain why React needs state.
* Distinguish between local variables and state.
* Understand why changing a variable does not update the UI.
* Explain how Hooks integrate with the React rendering process.
* Understand the React component lifecycle.
* Describe the relationship between state updates and rendering.
* Visualize how Hooks are stored within the Fiber tree.

---

# Why State Exists

In Module 1 we learned the most important equation in React:

```text
UI = f(State)
```

If state never changes, the UI never changes.

Real applications are dynamic:

* Counters
* Forms
* Shopping carts
* Dashboards
* Chat applications
* Games

All require data that changes over time.

React introduces **state** to model changing application data.

---

# Local Variables vs React State

Consider a normal JavaScript variable.

```jsx
function Counter() {
    let count = 0;

    function increment() {
        count++;
        console.log(count);
    }

    return (
        <>
            <h1>{count}</h1>
            <button onClick={increment}>
                Increment
            </button>
        </>
    );
}
```

Clicking the button prints:

```text
1
2
3
4
```

But the UI remains:

```text
0
```

Why?

Because React only renders when **state changes**.

Changing a normal variable does not notify React.

---

# Introducing useState

React provides the `useState` Hook.

```jsx
import { useState } from "react";

function Counter() {

    const [count, setCount] = useState(0);

    return (
        <>
            <h1>{count}</h1>

            <button
                onClick={() => setCount(count + 1)}
            >
                Increment
            </button>
        </>
    );
}
```

Now every click updates the interface.

---

# What Does useState Return?

```jsx
const [count, setCount] = useState(0);
```

Equivalent conceptual model:

```javascript
const state = useState(0);

const count = state[0];

const setCount = state[1];
```

The Hook returns two things:

* Current value
* State updater function

---

# What Happens When setState Runs?

Calling

```jsx
setCount(count + 1);
```

does **not** immediately change the UI.

Instead React schedules work.

```text
Button Click

↓

setCount()

↓

Update Queue

↓

Scheduler

↓

Render Phase

↓

Reconciliation

↓

Commit Phase

↓

Browser Updates
```

Notice how this connects directly to Module 1's rendering pipeline.

---

# React Re-renders Components

Every state update causes React to execute the component again.

```jsx
function Counter() {

    console.log("Rendering...");

    const [count, setCount] = useState(0);

    ...
}
```

Every click prints

```text
Rendering...
```

The component function runs again.

This surprises many beginners.

---

# Components Are Re-executed

React does **not** modify the previous function.

Instead it executes it again.

Think of every render as producing a completely new description of the UI.

```text
Render #1

↓

Component executes

↓

React Elements

↓

Render #2

↓

Component executes again

↓

New React Elements

↓

Compare Trees
```

---

# Hooks

Hooks are functions that allow components to interact with React's internal systems.

Common Hooks include:

| Hook          | Purpose                       |
| ------------- | ----------------------------- |
| `useState`    | Local state                   |
| `useEffect`   | Side effects                  |
| `useMemo`     | Memoized values               |
| `useCallback` | Memoized functions            |
| `useRef`      | Persistent mutable references |
| `useContext`  | Shared application state      |

Every Hook connects the component to the Fiber architecture.

---

# Rules of Hooks

Hooks must always be called:

✅ At the top level

✅ Inside React components

Never:

```jsx
if (loggedIn) {
    useState();
}
```

Never:

```jsx
for (...) {
    useState();
}
```

Never:

```jsx
function helper() {
    useState();
}
```

Why?

Because React identifies Hooks **by call order**.

---

# The React Lifecycle

Every component moves through several stages.

```text
Mount

↓

Render

↓

Commit

↓

Effects

↓

Updates

↓

Unmount
```

Understanding this lifecycle is essential for writing correct Hooks.

---

# Mount Phase

When a component first appears:

```jsx
<App />

↓

Counter

↓

Button
```

React:

* Creates Fiber nodes
* Initializes Hooks
* Produces React Elements
* Builds the DOM

---

# Update Phase

Whenever props or state change:

```text
State Update

↓

Render Again

↓

Compare Trees

↓

Update Browser
```

This cycle repeats throughout the component's lifetime.

---

# Unmount Phase

Eventually the component disappears.

```text
Dashboard

↓

User navigates away

↓

Dashboard removed

↓

Cleanup
```

React removes:

* Effects
* Event listeners
* References
* DOM nodes

---

# Where Hooks Live

A common misconception is that Hooks exist inside components.

They do not.

Hooks are stored inside the component's **Fiber node**.

Conceptually:

```text
Fiber

├── useState

├── useEffect

├── useMemo

├── useRef

└── useCallback
```

When React renders again, it retrieves Hook data from the Fiber.

This explains why Hook order matters.

---

# State Updates Are Asynchronous

Multiple updates can be batched.

```jsx
setCount(count + 1);

setCount(count + 1);

setCount(count + 1);
```

This does **not** necessarily increment by three.

Instead use functional updates.

```jsx
setCount(c => c + 1);

setCount(c => c + 1);

setCount(c => c + 1);
```

React processes each update sequentially.

---

# Connecting to Fiber

Every state update eventually becomes work for the Fiber reconciler.

```text
setState()

↓

Update Queue

↓

Fiber Scheduler

↓

Render Phase

↓

Diff Algorithm

↓

Commit Phase
```

Module 3 will explore this pipeline in depth.

---

# Key Takeaways

* State is React's mechanism for storing changing data.
* Updating state schedules a re-render.
* Components are executed again on every render.
* Hooks provide access to React's internal capabilities.
* Hook order is fixed because Hooks are stored on the Fiber node.
* Every state update flows through the React rendering pipeline.
* The lifecycle explains when rendering, effects, updates, and cleanup occur.

---

# Summary

State transforms React from a static UI library into a dynamic application framework. Rather than manually modifying the DOM, developers update state, and React determines how to efficiently re-render the interface.

Hooks provide the API for interacting with this rendering engine, while the component lifecycle explains when rendering occurs and how components evolve over time. Understanding these concepts is the foundation for mastering effects, context, memoization, and the Fiber architecture.

---

# Looking Ahead

In Lesson 2 we explore **`useEffect`**, one of the most important—and most misunderstood—Hooks in React.

We'll learn:

* Why side effects exist
* Render phase vs Commit phase
* Dependency arrays
* Cleanup functions
* Common infinite loop mistakes
* Synchronizing React with external systems
* How Effects are scheduled and executed by Fiber
