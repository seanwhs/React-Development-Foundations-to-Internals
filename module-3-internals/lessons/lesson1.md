# Lesson 1 — React Fiber: The Execution Engine of React

> Module 3 — Internals

---

# Learning Objectives

By the end of this lesson, you should be able to:

* Explain why React Fiber was introduced.
* Understand the limitations of the original Stack Reconciler.
* Describe what a Fiber is.
* Explain how Fiber represents a component tree.
* Understand how rendering became interruptible.
* Explain the Render and Commit phases.
* Connect Hooks, Components, and State to the Fiber architecture.
* Build the mental model that powers Concurrent React.

---

# Where We Are in the Curriculum

In Module 1, we learned:

```text
UI = f(State)
```

We discovered that components are simply JavaScript functions that return React Elements.

---

In Module 2, we learned:

* State
* Hooks
* Component lifecycle
* Rendering

Every call to `setState()` caused React to render again.

But one important question remained unanswered:

> **What exactly performs the rendering?**

The answer is **React Fiber**.

---

# Before Fiber

Early versions of React used what is now called the **Stack Reconciler**.

The rendering process looked like this.

```text
Component

↓

Children

↓

Grandchildren

↓

Entire Tree

↓

DOM Update
```

React recursively processed every component until the entire tree finished rendering.

---

# The Problem

Imagine an application containing thousands of components.

```text
App

├── Header

├── Sidebar

├── Dashboard

│   ├── Chart

│   ├── Table

│   ├── Statistics

│   ├── Timeline

│   └── Map

└── Footer
```

Rendering the entire tree could take several milliseconds.

During this time:

* Scrolling freezes.
* Animations stop.
* Typing feels delayed.
* User interactions wait.

JavaScript cannot pause halfway through a recursive function.

React needed a different architecture.

---

# Enter Fiber

Fiber completely redesigned React's rendering engine.

Instead of processing the component tree recursively, React converts every component into a small unit of work called a **Fiber**.

```text
Component

↓

Fiber

↓

Fiber

↓

Fiber

↓

Fiber
```

Now React can pause between units of work.

---

# What is a Fiber?

A Fiber is a JavaScript object representing one component instance.

Conceptually:

```javascript
const fiber = {

    type,

    props,

    state,

    child,

    sibling,

    return,

    alternate,

    flags

};
```

Each component in your application has a corresponding Fiber.

```text
<App/>

↓

Fiber(App)

↓

Fiber(Header)

↓

Fiber(Main)

↓

Fiber(Button)

↓

Fiber(Footer)
```

React no longer traverses component functions directly.

It traverses the Fiber tree.

---

# Components Become Fibers

Consider a simple application.

```jsx
function App() {
    return (
        <>
            <Header />
            <Dashboard />
            <Footer />
        </>
    );
}
```

Internally React builds something similar to:

```text
Fiber(App)

├── Fiber(Header)

├── Fiber(Dashboard)

└── Fiber(Footer)
```

Every render creates work on this Fiber tree.

---

# Why Fiber is Faster

Fiber allows React to divide rendering into many small tasks.

Instead of:

```text
Render Entire App

↓

Finish

↓

Browser Responds
```

React performs:

```text
Fiber 1

↓

Fiber 2

↓

Pause

↓

Browser Responds

↓

Fiber 3

↓

Fiber 4

↓

Pause

↓

Continue
```

Rendering becomes cooperative.

---

# Time Slicing

Fiber introduced **time slicing**.

Conceptually:

```text
16 ms Frame

├── React Work

├── React Work

├── Pause

├── Browser Paint

├── Continue React
```

React no longer monopolizes the JavaScript thread.

This makes applications feel responsive.

---

# The Fiber Tree

Unlike React Elements, the Fiber tree is persistent.

```text
Current Fiber Tree

↓

State Update

↓

Work-in-Progress Tree

↓

Commit

↓

Current Tree Replaced
```

React always maintains two versions of the tree.

---

# Double Buffering

Fiber uses **double buffering**.

```text
Current Tree

↓

User clicks button

↓

Work Tree

↓

Finish Rendering

↓

Swap Trees
```

Only after rendering finishes does React replace the current tree.

This guarantees UI consistency.

---

# The Alternate Pointer

Every Fiber has an `alternate`.

```text
Current Fiber

⇄

Work Fiber
```

The alternate connects the current tree and the work-in-progress tree.

React switches between them after each successful render.

---

# Render Phase

Rendering does **not** update the DOM.

Instead React calculates what should change.

```text
State Update

↓

Execute Components

↓

Create React Elements

↓

Build Work Tree

↓

Find Differences
```

The render phase is pure computation.

It can be paused, resumed, or even restarted.

---

# Commit Phase

Once rendering finishes:

```text
Render Complete

↓

Commit

↓

Update DOM

↓

Run Effects

↓

Paint Screen
```

The Commit Phase cannot be interrupted.

It is intentionally kept very short.

---

# Fiber Traversal

React walks the Fiber tree using linked pointers.

```text
Fiber

├── child

├── sibling

└── return
```

This structure avoids deep recursive function calls and allows React to stop and resume traversal efficiently.

---

# Where Hooks Live

Module 2 introduced Hooks.

Hooks are not stored inside component functions.

They live on the Fiber.

```text
Fiber

├── useState

├── useEffect

├── useMemo

├── useReducer

└── useRef
```

When a component executes again, React retrieves Hook data from the Fiber.

---

# State Updates Flow Through Fiber

```text
setState()

↓

Update Queue

↓

Fiber Scheduler

↓

Render Phase

↓

Diff

↓

Commit Phase

↓

Browser Updates
```

Everything eventually becomes work on the Fiber tree.

---

# Fiber Enables Concurrent React

Because rendering is interruptible, React can:

* Pause rendering
* Resume later
* Restart rendering
* Prioritize urgent updates
* Delay background work

Without Fiber, none of these capabilities would be possible.

This architecture powers modern React features such as transitions, Suspense, selective hydration, and concurrent rendering.

---

# Visual Summary

```text
State Changes

↓

Scheduler

↓

Fiber Tree

↓

Render Phase

↓

Work-in-Progress Tree

↓

Reconciliation

↓

Commit Phase

↓

DOM

↓

Browser Paint
```

This is the complete React rendering pipeline.

---

# Key Takeaways

* Fiber is React's execution engine.
* Every component corresponds to a Fiber node.
* Fiber replaces the old Stack Reconciler.
* Rendering is divided into small units of work.
* React maintains both a current tree and a work-in-progress tree.
* The Render Phase calculates changes without touching the DOM.
* The Commit Phase applies changes to the browser.
* Hooks, state, scheduling, and reconciliation all operate through the Fiber tree.
* Fiber is the architectural foundation of Concurrent React.

---

# Summary

React Fiber transformed React from a synchronous rendering library into a cooperative scheduling engine. Instead of traversing component trees recursively, React represents each component as a Fiber node, allowing rendering to be paused, resumed, prioritized, or restarted.

This architecture separates rendering from committing, enabling React to prepare updates in memory before applying them to the DOM. Fiber is the invisible execution engine that connects components, Hooks, state updates, reconciliation, scheduling, and rendering into a unified system.

Understanding Fiber is the key to understanding how modern React actually works under the hood.

---

# Looking Ahead

In Lesson 2 we will study **Reconciliation**, the algorithm that runs on top of the Fiber architecture.

We'll answer questions such as:

* How does React know what changed?
* Why are keys important?
* What is tree diffing?
* Why doesn't React compare every DOM node?
* How does React preserve component state?
* How do keys influence mounting and unmounting?
* How does reconciliation interact with the Fiber tree?

By the end of the next lesson, you'll understand how React efficiently transforms one UI tree into another with minimal DOM updates.
