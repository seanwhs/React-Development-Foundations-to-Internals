# Lesson 3 — Concurrent Rendering: Transitions, Priorities, and Non-Blocking UI

> Module 4 — Performance

---

# Learning Objectives

By the end of this lesson, you should be able to:

* Explain what Concurrent Rendering means in practical terms
* Understand why React needs transitions
* Distinguish urgent vs non-urgent updates
* Use `startTransition`, `useTransition`, and `useDeferredValue`
* Understand how concurrency interacts with Fiber scheduling
* Prevent UI blocking in real-world scenarios (search, filtering, dashboards)
* Build a mental model of priority-based rendering

---

# Where We Are in the Model

From previous lessons:

```text id="q1v8mn"
State Update
   ↓
Scheduler
   ↓
Fiber Render
   ↓
Reconciliation
   ↓
Commit
```

We improved performance using memoization.

Now we address a deeper problem:

> **What happens when rendering itself is too slow?**

---

# The Real Performance Problem

Even with memoization:

* Large lists still render slowly
* Expensive filters block typing
* Charts freeze UI during updates
* Search inputs lag under load

The issue is not *extra renders*.

The issue is:

> **blocking work during rendering**

---

# The Core Idea of Concurrency

Concurrent Rendering means:

> React can pause rendering work and prioritize more important updates.

Not parallel execution.

Not multiple threads.

Instead:

> Intelligent scheduling on a single thread.

---

# Two Types of Updates

React classifies updates into:

## 1. Urgent Updates

* Typing in input
* Clicking buttons
* Animations
* Drag interactions

---

## 2. Non-Urgent Updates

* Filtering lists
* Rendering charts
* Expensive computations
* Background UI updates

---

# The Problem Without Concurrency

```text id="a9x2pl"
User types "react"

↓

// expensive filtering starts

Render 50,000 items

↓

// input feels frozen
```

The UI becomes unresponsive.

---

# The Solution: Prioritization

React now separates work:

```text id="k7v3qz"
Urgent update → render immediately

Non-urgent update → render later
```

---

# startTransition — Marking Non-Urgent Work

Example:

```jsx id="m8x2pq"
import { startTransition } from "react";

startTransition(() => {
    setFilteredData(result);
});
```

Now React treats this update as:

> low priority

---

# What Actually Happens

```text id="p3v9qn"
User input (high priority)
   ↓
Render input immediately
   ↓
Transition update queued
   ↓
Render later when idle
```

---

# useTransition — Tracking Pending Work

```jsx id="v7q2mn"
const [isPending, startTransition] = useTransition();

function handleChange(e) {
    setInput(e.target.value);

    startTransition(() => {
        setSearchResults(expensiveFilter(e.target.value));
    });
}
```

---

# Why useTransition Matters

It allows UI to:

* Stay responsive
* Show loading states
* Delay heavy computation safely

Example UI behavior:

```text id="t2k9wx"
Typing → instant input update
       → results update slightly delayed
       → spinner shows during transition
```

---

# useDeferredValue — Deferring Expensive Rendering

Instead of delaying state updates:

> defer the derived value

```jsx id="x8q1pl"
const deferredQuery = useDeferredValue(query);
```

Now:

* `query` updates immediately
* `deferredQuery` lags behind

---

# Why This Works

React prioritizes:

```text id="c4v8qz"
User input state (urgent)
   ↓
Derived expensive UI (non-urgent)
```

This avoids blocking typing.

---

# Real Example: Search UI

```jsx id="n3v9qk"
function Search() {
    const [query, setQuery] = useState("");
    const deferredQuery = useDeferredValue(query);

    const results = filter(data, deferredQuery);

    return (
        <>
            <input value={query} onChange={...} />
            <List items={results} />
        </>
    );
}
```

---

# What Happens Internally

```text id="h7x3qp"
User types
   ↓
Input updates immediately
   ↓
Deferred value lags
   ↓
List renders later
   ↓
No UI blocking
```

---

# Concurrency and Fiber Scheduling

Concurrency is not separate from Fiber.

It directly modifies scheduling behavior:

```text id="r8v2mn"
Scheduler assigns priority
   ↓
Fiber work is segmented
   ↓
Low priority work is paused
   ↓
High priority interrupts execution
```

---

# Time Slicing Revisited

From Module 3:

```text id="f2q9wx"
React breaks work into chunks
```

Concurrency extends this:

* Some chunks are paused
* Some are delayed
* Some are discarded

---

# Rendering Can Be Aborted

If a higher priority update arrives:

```text id="z3k8qp"
Render A (low priority)
   ↓
User types (high priority)
   ↓
Abort Render A
   ↓
Render B instead
```

This is a key behavior of Concurrent React.

---

# Why Aborting Work is Safe

Because rendering is:

> pure computation

React can safely discard unfinished work.

---

# Mental Model Shift

Before concurrency:

❌ Rendering is continuous and blocking

After concurrency:

✔ Rendering is interruptible and prioritized

---

# UI Responsiveness Model

React optimizes for:

| Principle               | Meaning                   |
| ----------------------- | ------------------------- |
| Urgency first           | Input should never lag    |
| Defer non-critical work | Heavy UI can wait         |
| Abort outdated renders  | Only latest state matters |

---

# Real-World Pattern: Typeahead Search

Without concurrency:

```text id="b5q9wx"
User types
→ full list filter runs
→ UI freezes
```

With concurrency:

```text id="m7v3qp"
User types
→ input updates immediately
→ filtering delayed
→ results appear smoothly
```

---

# When to Use Concurrency

Use it when:

* Filtering large datasets
* Rendering expensive lists
* Updating dashboards
* Performing heavy UI recalculation

---

# When NOT to Use It

Avoid when:

* Updates are already fast
* No perceptible lag exists
* Simplicity matters more than optimization

---

# Relationship to Memoization

| Concept     | Purpose                             |
| ----------- | ----------------------------------- |
| Memoization | Avoid unnecessary work              |
| Concurrency | Schedule necessary work efficiently |

They solve different problems:

* Memoization reduces work
* Concurrency organizes work

---

# Key Insight

> Memoization is micro-optimization
> Concurrency is macro-optimization

---

# Full Performance Stack

```text id="q9v2mn"
Memoization
   ↓
Avoid unnecessary renders

Concurrency
   ↓
Prioritize necessary renders

Fiber
   ↓
Execute renders efficiently
```

---

# Key Takeaways

* Concurrent rendering allows React to prioritize updates
* Urgent updates are executed immediately
* Non-urgent updates can be delayed or interrupted
* `startTransition` marks low-priority updates
* `useTransition` tracks pending transitions
* `useDeferredValue` delays derived computations
* Rendering is interruptible and abortable
* Fiber scheduler enforces priority-based execution
* UI responsiveness is the main goal of concurrency

---

# Summary

Concurrent rendering transforms React from a simple rendering system into a priority-based scheduling engine. Instead of treating all updates equally, React classifies updates as urgent or non-urgent and schedules them accordingly. This ensures that user interactions remain responsive even when the application performs heavy rendering work.

This system works hand-in-hand with Fiber’s interruptible rendering model, allowing React to pause, resume, or discard work based on priority.

---

# Looking Ahead

In **Lesson 4**, we move into production architecture:

> **React Server Components, Code Splitting, and Rendering Boundaries**

We will explore:

* Server vs Client rendering models
* React Server Components (RSC)
* Streaming UI
* Suspense boundaries
* Code splitting with `React.lazy`
* Reducing client-side JavaScript
* Shifting work from browser → server

This is where performance becomes **system design**, not just UI optimization.
