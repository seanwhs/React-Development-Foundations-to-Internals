# 📁 `module-3-internals/lesson1.md`

# Lesson 1 — React Fiber: The Execution Model

> Module 3 — Internals (Fiber & Reconciliation)

---

# Learning Objectives

By the end of this lesson, you should be able to:

- Explain why React Fiber exists
- Understand limitations of the Stack Reconciler
- Describe what a Fiber node is
- Understand render vs commit phases
- Explain interruptible rendering conceptually

---

# The Problem Before Fiber

React used to render like this:

```text
Render Entire Tree → Commit → Done
````

This caused:

* UI blocking
* input lag
* poor responsiveness

---

# The Core Issue

JavaScript execution is synchronous.

So React could not:

* pause rendering
* prioritize updates
* handle user input mid-render

---

# Enter Fiber

Fiber breaks rendering into small units of work.

Each unit = a Fiber node.

---

# What is a Fiber?

A Fiber is a JavaScript object representing a component instance.

```js id="x9f2lm"
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

---

# Fiber Tree Structure

```text
App
├── Header
├── Main
│   ├── List
│   └── Item
└── Footer
```

Becomes:

```text
Fiber(App)
 ├── Fiber(Header)
 ├── Fiber(Main)
 │    ├── Fiber(List)
 │    └── Fiber(Item)
 └── Fiber(Footer)
```

---

# Render vs Commit

## Render Phase (interruptible)

* execute components
* build Fiber tree
* compute changes

## Commit Phase (not interruptible)

* update DOM
* run effects

---

# Key Insight

Fiber enables React to pause and resume rendering work safely.

---

# 📁 `module-3-internals/lesson2.md`

# Lesson 2 — Reconciliation Algorithm

---

# Learning Objectives

- Understand reconciliation process
- Understand tree diffing strategy
- Learn how React reuses components
- Understand role of keys

---

# What is Reconciliation?

Reconciliation is React’s diffing process:

> Compare previous UI tree vs next UI tree

---

# High-Level Process

```text id="r9v2k1"
Old Tree
   ↓
New Tree
   ↓
Diffing Algorithm
   ↓
Minimal Updates
   ↓
Commit
````

---

# Key Rules

## 1. Different Type → Replace

```jsx
<div />
<span />
```

React destroys and recreates.

---

## 2. Same Type → Update

```jsx
<div className="a" />
<div className="b" />
```

Only props update.

---

## 3. Keys Preserve Identity

```jsx id="k2p9qz"
items.map(item => (
    <li key={item.id}>{item.name}</li>
))
```

Keys allow stable tracking.

---

# Why Keys Matter

Without keys:

* React reuses wrong nodes
* state is lost
* UI becomes unstable

---

# Key Insight

Reconciliation is heuristic, not full tree comparison.

---

# 📁 `module-3-internals/lesson3.md`

# Lesson 3 — Fiber Work Loop & Scheduling

---

# Learning Objectives

- Understand React scheduler
- Understand work loop concept
- Understand prioritization
- Learn interruptible rendering

---

# React Work Model

React breaks work into units:

```text id="w1k8mz"
Work Unit → Fiber Node
````

---

# Work Loop

```text
Start
  ↓
Process Fiber
  ↓
Check time
  ↓
Pause if needed
  ↓
Resume later
```

---

# Why This Matters

React avoids blocking the main thread.

---

# Prioritization

React assigns priority:

| Priority | Example            |
| -------- | ------------------ |
| High     | typing             |
| Medium   | clicks             |
| Low      | background updates |

---

# Key Insight

React schedules rendering like an operating system scheduler.

---

# 📁 `module-3-internals/lesson4.md`

# Lesson 4 — Hooks Inside Fiber

---

# Learning Objectives

- Understand where Hooks live
- Understand Hook order dependency
- Understand Fiber-linked Hook list

---

# Hooks Are Not in Components

Hooks are stored in Fiber:

```text
Fiber
 ├── Hook1
 ├── Hook2
 └── Hook3
````

---

# Hook Order Matters

```jsx
useState()
useEffect()
useMemo()
```

Order must NEVER change.

---

# Why Order Matters

React uses a linked list:

```text id="h3k9mv"
Fiber → Hook1 → Hook2 → Hook3
```

---

# Rules of Hooks

* only at top level
* never inside loops
* never inside conditions

---

# Key Insight

Hooks are positional, not named.

---

# 📁 `module-3-internals/exercise.md`

# Exercise — Build a Mini Fiber Simulator

---

# Task

Simulate rendering behavior:

## Requirements

- Create a tree of nodes
- Simulate traversal
- Pause execution mid-tree
- Resume execution

---

# Goals

- understand interruptible rendering
- understand work loop
- simulate Fiber traversal

---

# Bonus

Add priority levels (high vs low work)

---

# 📁 `module-3-internals/solution.md`

# Solution — Module 3 Internals

---

# Fiber Traversal Simulator

```js id="f2k9mv"
const tree = {
    name: "App",
    children: [
        { name: "Header", children: [] },
        { name: "Main", children: [
            { name: "List", children: [] },
            { name: "Item", children: [] }
        ]},
        { name: "Footer", children: [] }
    ]
};

function work(node) {
    console.log("Processing:", node.name);
}

function traverse(node) {
    const stack = [node];

    while (stack.length) {
        const current = stack.pop();

        work(current);

        if (current.children) {
            for (let i = current.children.length - 1; i >= 0; i--) {
                stack.push(current.children[i]);
            }
        }
    }
}

traverse(tree);
````

---

# Key Takeaway

Fiber is just controlled traversal of a tree with scheduling capability.
Just say 👍
```
