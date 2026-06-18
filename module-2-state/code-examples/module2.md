# 📁 `module-2-state/lesson1.md`

# Lesson 1 — Understanding State and Hooks

> Module 2 — State & Hooks

---

# Learning Objectives

By the end of this lesson, you should be able to:

- Understand why React needs state
- Distinguish variables vs state
- Use `useState` correctly
- Explain re-rendering behavior
- Understand how Hooks connect to rendering

---

# Why State Exists

React applications are dynamic:

- counters
- forms
- filters
- UI toggles

Static variables cannot represent changing UI.

---

# Variables Are Not Reactive

```jsx
function Counter() {
    let count = 0;

    return (
        <button onClick={() => count++}>
            {count}
        </button>
    );
}
````

Problem:

> UI does not update when `count` changes.

---

# Introducing useState

```jsx id="q3x9kl"
import { useState } from "react";

function Counter() {
    const [count, setCount] = useState(0);

    return (
        <button onClick={() => setCount(count + 1)}>
            {count}
        </button>
    );
}
```

---

# What useState Actually Does

* stores value inside React (Fiber)
* triggers re-render when updated
* preserves value across renders

---

# Re-render Cycle

```text
setState()
→ schedule update
→ re-run component
→ generate new UI
→ commit changes
```

---

# Key Insight

State is not a variable.

State is a **managed memory inside React’s rendering system**.

---

# 📁 `module-2-state/lesson2.md`

# Lesson 2 — Component Lifecycle

---

# Learning Objectives

- Understand mount, update, unmount
- Understand render cycle
- Understand how state triggers lifecycle

---

# Component Lifecycle

React components go through phases:

```text
Mount → Render → Update → Unmount
````

---

# Mount Phase

Component appears for first time:

* state initialized
* UI created
* DOM inserted

---

# Update Phase

Triggered by:

* state change
* props change

Component re-runs.

---

# Unmount Phase

Component removed:

* cleanup runs
* event listeners removed
* memory released

---

# Render vs Commit

Render:

* calculate UI

Commit:

* update DOM

---

# Key Insight

Rendering is not UI update.

It is computation before UI update.

---

# 📁 `module-2-state/lesson3.md`

# Lesson 3 — useEffect and Side Effects

---

# Learning Objectives

- Understand side effects
- Learn useEffect
- Understand dependency arrays
- Learn cleanup behavior

---

# What is a Side Effect?

Anything outside rendering:

- API calls
- timers
- subscriptions
- logging

---

# useEffect

```jsx id="k8m2pn"
useEffect(() => {
    console.log("Mounted");

    return () => {
        console.log("Cleanup");
    };
}, []);
````

---

# Dependency Array

| Dependency | Meaning                |
| ---------- | ---------------------- |
| []         | run once               |
| [value]    | run when value changes |
| none       | run every render       |

---

# Cleanup Function

Runs when:

* component unmounts
* dependencies change

---

# Key Insight

useEffect connects React to external systems.

---

# 📁 `module-2-state/lesson4.md`

# Lesson 4 — Hooks and Fiber Model

---

# Learning Objectives

- Understand Hooks storage
- Understand Hook order rules
- Connect Hooks to Fiber

---

# Where Hooks Live

Hooks are NOT inside components.

They are stored in Fiber nodes.

---

# Hook Order Matters

```jsx
useState()
useEffect()
useMemo()
````

Order must never change.

---

# Why Order Matters

React stores Hooks in a linked list:

```text
Fiber → Hook1 → Hook2 → Hook3
```

Changing order breaks mapping.

---

# Rules of Hooks

* only call at top level
* only call in React functions
* never inside loops or conditions

---

# Key Insight

Hooks are tied to execution order, not variable names.

---

# 📁 `module-2-state/exercise.md`

# Exercise — Build Lifecycle-Aware Component

---

# Task

Build a User Profile system:

## Requirements

- Fetch user data (simulate with timeout)
- Display loading state
- Display user info
- Cleanup logging on unmount

---

# Requirements

- useState
- useEffect
- loading state
- cleanup function

---

# Bonus

Add a refresh button that re-fetches data.

---

# 📁 `module-2-state/solution.md`

# Solution — Module 2

---

# UserProfile Component

```jsx id="p9q2mv"
import { useEffect, useState } from "react";

function UserProfile() {
    const [user, setUser] = useState(null);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
        let active = true;

        setLoading(true);

        setTimeout(() => {
            if (active) {
                setUser({ name: "Sean", role: "Engineer" });
                setLoading(false);
            }
        }, 1000);

        return () => {
            active = false;
            console.log("Cleanup on unmount");
        };
    }, []);

    if (loading) return <p>Loading...</p>;

    return (
        <div>
            <h2>{user.name}</h2>
            <p>{user.role}</p>
        </div>
    );
}

export default UserProfile;
````

---

# Key Takeaway

Hooks connect component execution to React’s lifecycle system.
