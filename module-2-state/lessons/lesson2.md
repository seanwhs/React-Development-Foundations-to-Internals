# Lesson 2 — `useEffect` and the React Side Effect System

> Module 2 — State Management

---

# Learning Objectives

By the end of this lesson, you should be able to:

* Explain what a side effect is in React
* Distinguish between render logic and effect logic
* Understand when `useEffect` runs in the lifecycle
* Describe dependency arrays and their behavior
* Explain cleanup functions and why they exist
* Avoid common `useEffect` pitfalls (loops, stale state)
* Understand how effects relate to the commit phase
* Connect `useEffect` to Fiber execution behavior

---

# What is a Side Effect?

A **side effect** is anything that affects the outside world.

In React terms:

> A side effect is any operation that happens outside of rendering UI.

Examples:

* Fetching data from an API
* Subscribing to events
* Logging
* Updating the DOM manually
* Setting timers (`setTimeout`, `setInterval`)
* Working with localStorage

---

# Render vs Side Effect

React components must remain **pure during render**.

### Pure render means:

```text
Same input → Same output
No external interaction
```

---

## ❌ Wrong: Side effect in render

```jsx
function App() {
    console.log("Fetching data...");
    fetch("/api/data");

    return <h1>Hello</h1>;
}
```

This is wrong because render should NOT perform external operations.

---

## ✅ Correct: Use Effect

```jsx
import { useEffect } from "react";

function App() {

    useEffect(() => {
        console.log("Fetching data...");
        fetch("/api/data");
    }, []);

    return <h1>Hello</h1>;
}
```

Now the side effect is separated from rendering.

---

# When Does `useEffect` Run?

`useEffect` runs **after the commit phase**.

```text
Render Phase
   ↓
Reconciliation
   ↓
Commit Phase (DOM updated)
   ↓
useEffect runs
```

This is extremely important:

> Effects NEVER run during rendering.

---

# Why This Separation Exists

React splits work into two worlds:

## 1. Render Phase (Pure)

* Calculate UI
* No side effects
* Can be paused or restarted (Fiber)

---

## 2. Commit Phase (Real World)

* DOM updates
* Layout changes
* Effects execution

---

# Basic useEffect

```jsx
useEffect(() => {
    console.log("Component mounted");
});
```

This runs after every render.

---

# Dependency Array

The dependency array controls WHEN the effect runs.

---

## 1. No dependency array

```jsx
useEffect(() => {
    console.log("Runs every render");
});
```

Runs after every render.

---

## 2. Empty dependency array

```jsx
useEffect(() => {
    console.log("Runs once on mount");
}, []);
```

Runs only once when component mounts.

---

## 3. With dependencies

```jsx
useEffect(() => {
    console.log("Count changed:", count);
}, [count]);
```

Runs only when `count` changes.

---

# Mental Model for Dependencies

Think of dependencies as:

> A list of values React watches for changes.

If ANY value changes → effect runs again.

---

# Cleanup Function

Effects can return a cleanup function.

```jsx
useEffect(() => {
    const id = setInterval(() => {
        console.log("Tick");
    }, 1000);

    return () => {
        clearInterval(id);
    };
}, []);
```

---

# When Does Cleanup Run?

Cleanup runs:

* Before effect re-runs
* When component unmounts

```text
Effect runs
↓
Component updates
↓
Cleanup runs
↓
Effect runs again
```

---

# Why Cleanup Exists

Without cleanup:

* Memory leaks
* Duplicate subscriptions
* Multiple timers
* Stale listeners

React ensures predictable teardown.

---

# Common useEffect Mistake: Infinite Loop

```jsx
useEffect(() => {
    setCount(count + 1);
}, [count]);
```

What happens:

```text
count changes
↓
effect runs
↓
setCount updates state
↓
component re-renders
↓
count changes again
↓
loop forever
```

---

# Fixing Infinite Loops

Use conditions or restructure logic:

```jsx
useEffect(() => {
    if (count < 5) {
        setCount(count + 1);
    }
}, [count]);
```

Or move logic outside effect when possible.

---

# Stale Closures (Important Concept)

```jsx
useEffect(() => {
    const id = setInterval(() => {
        console.log(count);
    }, 1000);

    return () => clearInterval(id);
}, []);
```

This logs **stale value of count**.

Why?

Because effect captured the initial render value.

---

# Fixing Stale Values

Option 1: Add dependency

```jsx
useEffect(() => {
    const id = setInterval(() => {
        console.log(count);
    }, 1000);

    return () => clearInterval(id);
}, [count]);
```

---

Option 2: useRef (advanced preview)

```jsx
const countRef = useRef(count);
```

---

# useEffect Lifecycle in React Terms

```text
Render Phase
   ↓
Commit DOM updates
   ↓
Effect scheduled
   ↓
Effect executes
```

This ensures UI is always consistent before side effects run.

---

# Multiple Effects

A component can have multiple effects:

```jsx
useEffect(() => {
    console.log("Fetch data");
}, []);

useEffect(() => {
    console.log("Subscribe to events");
}, []);
```

React runs them independently.

---

# Effect Ordering

Effects run in the order they are declared:

```text
Top → Bottom execution after commit
```

---

# What useEffect is NOT

❌ Not for deriving state
❌ Not for rendering logic
❌ Not for calculations
❌ Not for synchronous UI updates

---

# What useEffect IS

✔ Synchronizing with external systems
✔ Managing side effects
✔ Handling lifecycle behavior
✔ Bridging React with the outside world

---

# Connecting to Fiber

From Module 3 perspective:

```text
State Update
   ↓
Render Phase (Fiber work)
   ↓
Commit Phase
   ↓
DOM updated
   ↓
Effects executed
```

Important insight:

> Effects are scheduled AFTER Fiber commits the UI.

---

# Why React Delays Effects

React ensures:

* UI is consistent
* DOM is stable
* No partial rendering state leaks

Then effects run safely.

---

# Real-World Mental Model

Think of React like this:

### Render Phase

> "What should the UI look like?"

### Commit Phase

> "Apply it to the browser"

### Effects

> "Now sync with the outside world"

---

# Key Takeaways

* Side effects must not run during render
* `useEffect` runs after commit phase
* Dependency arrays control execution timing
* Cleanup prevents memory leaks and duplication
* Infinite loops occur when state updates trigger effects incorrectly
* Effects connect React to external systems
* Hooks interact with Fiber scheduling indirectly
* Render and effect phases are strictly separated

---

# Summary

`useEffect` is React’s mechanism for handling anything outside pure rendering. It ensures that side effects run only after the UI is safely committed to the DOM. By controlling execution timing through dependencies and cleanup functions, React maintains predictable behavior even in complex applications.

Understanding `useEffect` is essential because it defines how React interacts with the external world while preserving the purity of its rendering model.

---

# Looking Ahead

In Lesson 3, we go deeper into state mechanics:

> **How React batches updates, schedules renders, and manages state consistency**

We will explore:

* State batching behavior
* Functional updates in depth
* Concurrent updates
* Update queues in Fiber
* Priority-based rendering
* How multiple state updates are processed internally

This is where React state becomes a scheduling system, not just a variable container.
