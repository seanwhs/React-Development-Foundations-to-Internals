# Lesson 3 — Scheduling, Time Slicing, and Concurrent Rendering

> Module 3 — Internals

---

# Learning Objectives

By the end of this lesson, you should be able to:

* Explain what React scheduling is
* Understand why rendering must be interruptible
* Describe time slicing in Fiber
* Understand how React prioritizes updates
* Explain what “concurrent rendering” actually means
* Distinguish between urgent and non-urgent updates
* Understand how rendering can be paused and resumed
* Connect scheduling to the Fiber work loop

---

# Where We Are in the System

From previous lessons:

```text id="k1x9qv"
State Update
   ↓
Fiber Render Phase
   ↓
Reconciliation
   ↓
Commit Phase
```

We already know:

* Fiber performs rendering work
* Reconciliation decides changes
* Commit applies DOM updates

Now we focus on a deeper question:

> **How does React decide WHEN to perform rendering work?**

This is the job of the **Scheduler**.

---

# Why Scheduling Exists

In early React:

```text id="p3q8wd"
Render happens immediately and fully
```

Problem:

* Long renders block the main thread
* UI freezes during heavy updates
* Input lag becomes noticeable
* Animations stutter

---

# The Core Problem

JavaScript is single-threaded:

```text id="8xqv2l"
Only one thing runs at a time
```

So if React is rendering:

* User input is blocked
* Browser painting is delayed
* Scroll becomes janky

---

# React’s Solution: Interruptible Rendering

React Fiber introduced:

> The ability to pause rendering work mid-way

This enables:

* Smooth UI updates
* Responsive input handling
* Background rendering
* Prioritized updates

---

# The Scheduler

The scheduler decides:

> Which work should run first, and how long it should run

Conceptually:

```text id="m8w2pq"
Update → Scheduler → Assign Priority → Execute Work
```

---

# Time Slicing

React breaks rendering into small chunks.

Instead of:

```text id="q7v1lm"
Render entire tree → block thread → commit
```

React does:

```text id="t4k9xn"
Work chunk → pause → yield to browser → continue
```

---

# What is a Time Slice?

A time slice is a small unit of execution time.

Typically:

```text id="h6z1pq"
~5–10 milliseconds of work
```

After that:

* React yields control back to browser
* Browser can paint or handle input

---

# The Work Loop

React Fiber uses a loop:

```text id="v9m2xq"
while (workRemaining) {
    performUnitOfWork();
    if (shouldYield()) {
        break;
    }
}
```

This is the heart of scheduling.

---

# What is a Unit of Work?

A unit of work = one Fiber node.

Example:

```text id="c2p9zw"
<App />
  → <Header />
  → <Sidebar />
  → <Main />
```

Each component = one Fiber unit.

---

# Rendering Becomes Cooperative

Instead of React owning the thread:

```text id="r8q2vn"
React works → Browser works → React continues
```

This is called:

> Cooperative multitasking

---

# Priority Levels

Not all updates are equal.

React assigns priority:

## High Priority

* Typing
* Clicking
* Dragging
* Animations

## Low Priority

* Data fetching
* Rendering large lists
* Background updates

---

# Example: Input vs Data Render

```jsx id="x9k2pl"
setInput("hello");
setLargeList(data);
```

React prioritizes:

1. Input update first
2. List rendering later

---

# Scheduling Model

Conceptually:

```text id="n4v7qz"
Update arrives
    ↓
Assign priority
    ↓
Place in queue
    ↓
Scheduler selects work
    ↓
Fiber executes
```

---

# Interruptible Rendering

React can stop rendering at any time:

```text id="j3q8wx"
Render A
Render B
Render C
PAUSE ← browser needs to paint
Resume Render D
```

This prevents UI freezing.

---

# Why This is Powerful

Without interruption:

* Large renders block UI
* No responsiveness during updates

With interruption:

* UI stays responsive
* Input is smooth
* Animations continue

---

# Concurrent Rendering (Concept)

Concurrent rendering means:

> React can prepare multiple versions of the UI without committing immediately

---

# Key Idea

React can:

* Start rendering
* Pause rendering
* Restart rendering
* Discard unfinished work

---

# Example Scenario

User types quickly:

```text id="b5v1mn"
a → ab → abc → abcd
```

React may:

* Start rendering "a"
* Abort it
* Start rendering "ab"
* Abort it
* Render only "abcd"

---

# Why React Aborts Work

Because newer updates are more important.

This ensures:

> UI always reflects latest state

---

# Fiber Enables Resumable Work

Fiber nodes store:

* Progress
* State
* Work pointers

So React can resume where it left off.

---

# Work-In-Progress Tree

React builds a second tree:

```text id="u7x9qp"
Current Tree
     ↓
Work-In-Progress Tree
     ↓
Swap when complete
```

This is safe rendering isolation.

---

# Double Buffering (Review)

React always maintains:

* Current UI
* Work-in-progress UI

Only after completion does React swap them.

---

# Scheduling + Reconciliation Together

```text id="k8q1zw"
Scheduler
   ↓
Select Fiber Work
   ↓
Render Phase
   ↓
Reconciliation
   ↓
Commit
```

Scheduling decides WHEN.

Reconciliation decides WHAT changes.

---

# Commit Phase is NOT Interruptible

Important distinction:

| Phase          | Interruptible |
| -------------- | ------------- |
| Render         | Yes           |
| Reconciliation | Yes           |
| Commit         | No            |

Commit must be fast and atomic.

---

# Why Commit Cannot Be Interrupted

Because DOM must stay consistent:

* No partial updates
* No flickering UI
* No inconsistent layout

---

# Real Mental Model

Think of React as:

> A traffic controller for UI updates

It decides:

* What runs first
* What can wait
* What can be paused
* What must finish immediately

---

# Common Misunderstanding

❌ React re-renders everything immediately
✔ React schedules rendering work

❌ Rendering blocks the browser
✔ Rendering is split into chunks

❌ Updates happen instantly
✔ Updates are prioritized tasks

---

# Key Takeaways

* React uses a scheduler to manage rendering work
* Rendering is broken into small units (Fibers)
* Time slicing allows interruption of work
* React prioritizes updates (urgent vs non-urgent)
* Concurrent rendering enables pausing and resuming work
* Work-in-progress trees isolate rendering state
* Commit phase is always synchronous and final
* Scheduling ensures responsiveness under load

---

# Summary

Scheduling transforms React from a simple UI renderer into a cooperative execution engine. Instead of blocking the browser, React divides rendering into small units of work that can be paused, resumed, or discarded. This allows React to prioritize user interactions and maintain responsiveness even during heavy rendering tasks.

Together with Fiber and reconciliation, scheduling forms the third core pillar of React’s internal architecture: execution control.

---

# Looking Ahead

In Lesson 4, we connect everything:

> **How Fiber, reconciliation, and scheduling work together as a unified execution system**

We will explore:

* Complete render lifecycle tracing
* How updates flow from state → scheduler → fiber → commit
* How React ensures consistency across interruptions
* How concurrent rendering affects UI stability
* End-to-end mental model of React internals

This is where React becomes a fully integrated runtime system rather than separate mechanisms.
