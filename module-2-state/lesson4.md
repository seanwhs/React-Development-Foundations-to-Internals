# Lesson 4 — Connecting State, Hooks, and Lifecycle to Fiber Execution

> Module 2 — State Management

---

# Learning Objectives

By the end of this lesson, you should be able to:

* Connect `useState`, `useEffect`, and rendering to Fiber execution
* Understand how a component becomes a Fiber work unit
* Explain how React executes components during rendering
* Describe how Hooks are resolved in order inside Fiber
* Understand render vs commit phase in a unified model
* Trace a state update from `setState` → UI update
* Build a mental model of React as a runtime system (not a library)

---

# Where We Are in the Curriculum

So far in Module 2:

### Lesson 1

* State triggers renders
* Hooks live in Fiber
* Lifecycle stages exist

### Lesson 2

* Effects run after commit
* Render and side effects are separated

### Lesson 3

* State updates are queued
* React batches updates
* Updates are scheduled, not immediate

Now we unify everything:

> **How does React actually execute all of this internally?**

---

# React Is a Runtime System

At this point, React should no longer be thought of as:

* A UI library
* A templating engine
* A component system

Instead:

> React is a **runtime that executes a tree of work units**

That work unit is the **Fiber node**.

---

# Step 1 — Components Become Fiber Work Units

Every component is converted into a Fiber:

```jsx id="f3x9kd"
function Counter() {
    const [count, setCount] = useState(0);
    return <h1>{count}</h1>;
}
```

Internally:

```text id="k2v9qz"
Counter Component
        ↓
Fiber Node
        ↓
Work Unit
```

---

# Step 2 — Rendering = Executing Components

When React renders:

```text id="r7x1pl"
React calls your function components
```

Example:

```text id="p9v3xq"
Counter() executes
```

React is literally:

> running your functions again and again during render phase

---

# Important Insight

React does NOT “update components”.

It:

> re-executes them to produce new UI descriptions

---

# Step 3 — Hooks Are Resolved by Order

Inside Fiber:

```text id="h8v2md"
Fiber Node
  ├── Hook #1 (useState)
  ├── Hook #2 (useEffect)
  ├── Hook #3 (useMemo)
```

React does NOT store hooks by name.

It stores them by **call order**.

---

# Why Hook Order Matters

Example:

```jsx id="u3q9xk"
useState()
useEffect()
useMemo()
```

React expects:

* First hook → always same
* Second hook → always same
* Third hook → always same

If order changes → state corruption happens.

---

# Step 4 — Render Phase Execution

When state updates:

```text id="x1k9pl"
setState()
```

React enters **Render Phase**.

During render:

```text id="m2v8qz"
1. Execute component function
2. Resolve hooks from Fiber
3. Generate React Elements
4. Build new work-in-progress tree
```

Important:

> No DOM updates happen here

---

# Step 5 — Reconciliation Happens on Fiber Tree

React now compares:

```text id="c7x3wl"
Previous Fiber Tree
        vs
New Fiber Tree
```

It determines:

* Which nodes changed
* Which can be reused
* Which must be removed
* Which must be added

This is reconciliation working at Fiber level.

---

# Step 6 — Commit Phase Updates the DOM

Once reconciliation completes:

```text id="d9v1qs"
React commits changes to the DOM
```

This includes:

* Inserting elements
* Updating attributes
* Removing nodes

---

# Step 7 — Effects Run After Commit

After DOM is updated:

```text id="e5k7mn"
useEffect callbacks run
```

This ensures:

> Effects always see a fully updated UI

---

# Full Execution Pipeline (Unified Model)

Now we combine everything:

```text id="z8q1vp"
setState()
    ↓
Scheduler queues update
    ↓
Fiber Render Phase
    ↓
Component functions execute
    ↓
Hooks resolved in order
    ↓
React Elements created
    ↓
Reconciliation (diff Fiber trees)
    ↓
Commit Phase (DOM updates)
    ↓
useEffect runs
```

This is the complete React runtime loop.

---

# Mental Model: React as a Loop

Think of React as continuously repeating:

```text id="t3v8qk"
1. Receive updates
2. Re-run components
3. Build UI description
4. Compare with previous version
5. Apply minimal DOM changes
6. Run effects
```

---

# What Happens When You Click a Button?

Example:

```jsx id="c8x2pw"
<button onClick={() => setCount(c + 1)}>
    Click
</button>
```

Execution flow:

```text id="b5v9xz"
Click event
    ↓
setState triggered
    ↓
Fiber schedules update
    ↓
Component re-executes
    ↓
New React Elements generated
    ↓
Diffing occurs
    ↓
DOM updates
    ↓
Effects run
```

---

# Key Insight: React Never “Updates a Value”

React does NOT:

```text id="q1v7pm"
change variables in place
```

React DOES:

```text id="h7k3xq"
rebuild UI from scratch and compare results
```

---

# Why This Architecture Works

Because it allows React to:

### 1. Be predictable

Same input → same output

---

### 2. Be interruptible (Fiber)

React can pause rendering work

---

### 3. Be schedulable

Prioritize urgent updates

---

### 4. Be efficient

Only commit differences

---

# Hooks Are Just Fiber State Accessors

Think of Hooks as:

```text id="n4v9qz"
APIs into Fiber storage
```

They:

* read state from Fiber
* write updates into Fiber queue
* trigger scheduling

---

# React Lifecycle Revisited (Now With Fiber)

## Mount

```text id="j8v2mp"
Create Fiber
Run component
Build DOM
Run effects
```

---

## Update

```text id="k3q9wx"
Schedule update
Re-run Fiber
Reconcile
Commit changes
Run effects
```

---

## Unmount

```text id="s7v1cn"
Remove Fiber
Cleanup effects
Remove DOM nodes
```

---

# Why Everything Feels “Reactive”

Because:

> UI is always derived from Fiber execution

Not from manual mutation.

---

# Common Misunderstanding (Now Fixed)

❌ React updates variables
❌ React modifies DOM directly
❌ React tracks individual changes

✔ React re-executes functions
✔ React rebuilds UI trees
✔ React compares results via Fiber

---

# Key Takeaways

* Components are executed as Fiber work units
* Hooks are stored in Fiber and resolved by order
* Rendering is function execution, not mutation
* Reconciliation operates on Fiber trees
* Commit phase is where DOM changes happen
* Effects run after commit, never during render
* React is a runtime loop, not a static renderer
* State updates trigger full re-execution of component logic

---

# Summary

This lesson unifies everything in Module 2 into a single execution model. React is not a collection of features—it is a coordinated runtime system built on Fiber. Components execute as work units, Hooks provide controlled access to internal state, and updates flow through a strict render → reconcile → commit pipeline.

Understanding this model is essential because every advanced React concept—memoization, concurrency, server components, and performance optimization—depends on it.

---

# Module 2 Complete

You now understand:

* State
* Hooks
* Lifecycle
* Effects
* Batching
* Scheduling
* Fiber execution model

---

# Looking Ahead

Next module:

> **Module 3 — Internals: Fiber & Reconciliation Deep Dive**

You will go deeper into:

* Fiber node structure in detail
* Work-in-progress vs current trees
* Time slicing
* Scheduling priorities
* Concurrent rendering behavior
* How React breaks rendering into interruptible units

This is where React becomes a full execution engine architecture.
