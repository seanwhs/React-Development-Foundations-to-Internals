# Lesson 3 — State Updates, Batching, and the Update Queue

> Module 2 — State Management

---

# Learning Objectives

By the end of this lesson, you should be able to:

* Explain how React batches state updates
* Understand why state updates are asynchronous
* Distinguish between direct updates and functional updates
* Describe what an update queue is (conceptually)
* Understand how multiple state updates are processed in one render
* Explain stale state problems in state updates
* Connect state updates to Fiber scheduling behavior
* Reason about update priority at a conceptual level

---

# Where We Are in the Mental Model

So far in Module 2:

### Lesson 1

* State triggers re-renders
* Components re-execute
* Hooks live in Fiber

### Lesson 2

* Side effects run after commit
* Effects depend on render completion
* React separates render vs real-world logic

Now we answer a deeper question:

> **What actually happens when you call `setState` multiple times?**

---

# Why State Updates Are Not Immediate

Consider:

```jsx
setCount(count + 1);
console.log(count);
```

Most developers expect:

```text
Updated value immediately
```

But React behaves differently.

Why?

Because React does NOT update state instantly.

It schedules updates.

---

# React Groups State Updates (Batching)

React collects multiple updates and processes them together.

Example:

```jsx
setCount(count + 1);
setCount(count + 1);
setCount(count + 1);
```

You might expect `+3`.

But React batches them into a single render cycle.

---

# What Actually Happens

Conceptually:

```text id="q9x2ld"
Event Handler Starts

↓

setCount(+1)
setCount(+1)
setCount(+1)

↓

React Batches Updates

↓

Single Re-render

↓

Final UI Update
```

---

# Why Batching Exists

Without batching:

* Every update would trigger a render
* UI would become slow
* Work would be duplicated
* Fiber scheduler would be overwhelmed

Batching ensures:

> Multiple state updates → single render

---

# State Updates Are Queued

Internally, React treats updates as a queue.

Conceptually:

```text id="k3v9pm"
Update Queue:

[ +1 ]
[ +1 ]
[ +1 ]
```

Then React processes them together.

---

# Functional Updates Solve a Common Problem

Now consider this:

```jsx
setCount(count + 1);
setCount(count + 1);
setCount(count + 1);
```

All three use the SAME `count` value.

So React sees:

```text
setCount(0 + 1)
setCount(0 + 1)
setCount(0 + 1)
```

Final result:

```text
1
```

---

# Why This Happens

Because `count` is captured from the render.

This is called a **closure snapshot**.

Each update sees the same value.

---

# Correct Approach: Functional Updates

```jsx
setCount(c => c + 1);
setCount(c => c + 1);
setCount(c => c + 1);
```

Now React processes updates sequentially:

```text id="x8m2wp"
c = 0 → 1
c = 1 → 2
c = 2 → 3
```

Final result:

```text
3
```

---

# Key Insight

There are two forms of state updates:

---

## 1. Direct Update

```jsx
setCount(count + 1);
```

Uses snapshot value.

---

## 2. Functional Update

```jsx
setCount(prev => prev + 1);
```

Uses latest queued value.

---

# When React Processes Updates

React does NOT process updates immediately.

Instead:

```text id="u2q9xl"
1. Event occurs
2. Updates are queued
3. Event finishes
4. React processes queue
5. Component re-renders
```

---

# Why React Waits

React delays updates because:

* It can batch multiple updates
* It avoids unnecessary renders
* It keeps UI consistent
* It allows prioritization (Fiber scheduler)

---

# Update Batching Example

```jsx
function handleClick() {
    setCount(c => c + 1);
    setFlag(f => !f);
    setName("React");
}
```

React does NOT re-render three times.

It batches into one render.

---

# React 18 Behavior (Important Concept)

React batches updates:

* Inside event handlers
* Inside effects
* Inside async code (in modern React)

This is part of the Fiber scheduling model.

---

# Update Queue Inside Fiber (Conceptual Model)

Each component Fiber maintains an update queue.

```text id="f9k3wd"
Fiber Node
   ├── state
   ├── hooks
   └── updateQueue
```

Each `setState` pushes into this queue.

---

# Processing the Queue

When React renders:

```text id="r1v7qx"
1. Take previous state
2. Apply all queued updates
3. Produce new state
4. Re-render component
```

---

# State is Not Mutated Directly

Important rule:

❌ You never modify state directly
✔ You always schedule updates

```jsx
count = count + 1; // wrong

setCount(count + 1); // correct
```

---

# Why Direct Mutation Fails

React does NOT track variable changes.

It tracks:

> update events sent through setState

---

# Real Mental Model of setState

Think of `setState` as:

```text id="v3k8qp"
"Hey React, schedule an update for this component"
```

NOT:

```text
"Change this variable now"
```

---

# Multiple State Updates in One Render

Example:

```jsx
setA(1);
setB(2);
setC(3);
```

React:

* queues all updates
* merges them into one render
* produces final UI once

---

# Why This Improves Performance

Without batching:

```text
3 updates → 3 renders → slow UI
```

With batching:

```text
3 updates → 1 render → efficient UI
```

---

# Stale State Problem Revisited

Consider:

```jsx
setCount(count + 1);
setCount(count + 1);
```

Both use same `count` snapshot.

So both compute same result.

---

# Functional Updates Fix Everything

```jsx
setCount(c => c + 1);
```

Now React always uses:

> latest computed state

---

# Connection to Fiber Scheduler

From Module 3 perspective:

```text id="z1q7wv"
setState()
   ↓
Update Queue (Fiber)
   ↓
Scheduler decides timing
   ↓
Render Phase
   ↓
Reconciliation
   ↓
Commit
```

State updates are NOT immediate—they are scheduled work units.

---

# Priority Concept (Preview)

React may treat updates differently:

* User input → high priority
* Background updates → low priority

This allows smooth UI responsiveness.

---

# Key Takeaways

* State updates are asynchronous
* React batches multiple updates into one render
* Updates are stored in a queue inside Fiber
* Direct updates use snapshot values
* Functional updates use latest state safely
* React schedules updates, it does not apply them immediately
* Batching improves performance and consistency
* State updates are processed during render phase, not instantly

---

# Summary

React state updates are not simple variable assignments. They are scheduled updates that React collects, batches, and processes through the Fiber system. This ensures efficient rendering, consistent UI state, and predictable behavior.

Understanding batching and update queues is essential because it explains many confusing behaviors in React—especially why state updates sometimes appear delayed or inconsistent when using direct updates.

---

# Looking Ahead

In Lesson 4, we connect everything together:

> **How React components, state, hooks, and lifecycle behavior map to the Fiber execution model**

We will explore:

* Component rendering as Fiber work units
* Hook execution order in Fiber
* Render vs commit phase integration
* How updates move through the scheduler
* How React reconstructs UI trees efficiently

This is where React stops being a frontend library and becomes a runtime execution system.
