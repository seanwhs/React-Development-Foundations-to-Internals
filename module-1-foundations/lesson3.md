# Lesson 3 — React Elements, Reconciliation, and the Diffing Model

> Module 1 — Foundations

---

# Learning Objectives

By the end of this lesson, you should be able to:

* Explain what reconciliation is in React
* Understand why React does not re-render the entire DOM
* Distinguish between element identity and component identity
* Explain how React compares two UI trees
* Understand why keys are required in lists
* Describe how React minimizes DOM operations
* Connect reconciliation to the rendering pipeline

---

# Where We Are in the Mental Model

So far we have built a layered understanding:

### 1. UI as a function of state

```text
UI = f(State)
```

### 2. JSX becomes React Elements

```text
JSX → React.createElement → React Elements
```

### 3. React Elements form a tree

```text
React Element Tree → UI Description
```

Now we answer a critical question:

> When state changes, how does React update the UI efficiently?

The answer is **Reconciliation**.

---

# What is Reconciliation?

Reconciliation is React’s process of:

> Comparing the previous UI description with the new one and determining the minimal set of changes needed.

In simple terms:

```text
Old Tree + New Tree → Difference → DOM Updates
```

---

# Why Not Rebuild Everything?

A naive approach would be:

```text
State Change → Re-render entire DOM → Replace everything
```

This would be extremely slow because:

* DOM operations are expensive
* Layout recalculations are costly
* Repainting affects performance
* Large trees would freeze UI

Instead, React avoids full reconstruction.

---

# React’s Strategy

React uses a smarter approach:

```text
Step 1: Build new React Element Tree
Step 2: Compare with previous tree
Step 3: Apply only what changed
```

This is called **diffing**.

---

# Element Identity Matters

Consider:

```jsx
<h1>Hello</h1>
```

If React sees the same type and structure across renders, it assumes:

> This node can be reused.

But if something changes:

```jsx
<h1>Hello</h1>  →  <h1>Hi</h1>
```

React updates only the text node.

---

# Tree Comparison (High-Level)

Let’s compare two UI trees:

### Old Tree

```text
div
 ├── h1 "Hello"
 └── button "Click"
```

### New Tree

```text
div
 ├── h1 "Hello"
 └── button "Click me"
```

React sees:

* `div` is the same → reuse
* `h1` unchanged → reuse
* `button` changed → update text only

Result:

```text
Only button text updates in DOM
```

---

# What React DOES NOT Do

React does NOT:

* Compare pixel-by-pixel UI
* Compare full DOM trees
* Rebuild everything on every render
* Track visual differences

It only compares **React Elements**, not DOM nodes.

---

# Component Identity vs Element Identity

This is crucial.

## Element Identity

```jsx
<h1>Hello</h1>
```

This is a description of UI.

---

## Component Identity

```jsx
function Title() {
    return <h1>Hello</h1>;
}
```

This is a reusable function that produces elements.

---

React compares:

* Element type (`h1`, `button`, etc.)
* Component type (`Title`, `App`, etc.)

NOT function internals.

---

# When React Replaces a Node

React replaces a node when:

### 1. Type changes

```jsx
<h1>Hello</h1>
```

becomes

```jsx
<p>Hello</p>
```

➡️ React destroys old node and creates new one

---

### 2. Key changes

We will cover this next.

---

# Lists: The Real Complexity

Consider this list:

```jsx
<ul>
  <li>Apple</li>
  <li>Banana</li>
  <li>Mango</li>
</ul>
```

Now update it:

```jsx
<ul>
  <li>Apple</li>
  <li>Mango</li>
</ul>
```

React must determine:

* What was removed?
* What stayed?
* What changed?

---

# Why Keys Exist

Without keys:

React uses **index-based comparison**:

```text
0 → Apple
1 → Banana
2 → Mango
```

After update:

```text
0 → Apple
1 → Mango
```

React incorrectly assumes:

* Banana → Mango (wrong mapping)

---

# Correct Approach: Keys

```jsx
<ul>
  <li key="a">Apple</li>
  <li key="b">Banana</li>
  <li key="c">Mango</li>
</ul>
```

Now React tracks identity:

```text
a → Apple
b → Banana
c → Mango
```

After update:

```text
a → Apple
c → Mango
```

React correctly removes only `b`.

---

# Key Insight

Keys are NOT for performance.

Keys are for **identity tracking**.

They allow React to understand:

> "Which item is which across renders?"

---

# What Happens During Reconciliation

Reconciliation runs in three conceptual steps:

---

## 1. Render Phase

React creates a new element tree:

```text
State → Component execution → New React Elements
```

---

## 2. Diffing Phase

React compares:

```text
Old Tree ↔ New Tree
```

It identifies:

* What changed
* What stayed
* What should be removed

---

## 3. Commit Phase

React applies changes to the DOM:

```text
Insert / Update / Delete DOM nodes
```

---

# Important Rule

Reconciliation NEVER touches the DOM.

Only the Commit Phase does.

---

# Example: State Update Flow

```jsx
setCount(2);
```

Becomes:

```text
1. Schedule update
2. Re-render component
3. Create new element tree
4. Reconcile with previous tree
5. Commit DOM changes
```

---

# Why React is Fast

React is fast because:

* It works in memory first
* It minimizes DOM operations
* It reuses unchanged subtrees
* It batches updates
* It uses identity (keys) efficiently

---

# Common Misconception

❌ React re-renders the entire UI

✔ React re-executes components
✔ Then selectively updates the DOM

---

# Another Important Insight

React does NOT detect “visual changes”.

It detects **structural differences in the element tree**.

---

# Think of It Like This

```text
Old Blueprint
        ↓
New Blueprint
        ↓
Compare Instructions
        ↓
Only Rebuild Changed Parts
```

React is not editing the house.

It is comparing blueprints.

---

# Connection to Fiber (Preview)

In Module 3 we will see that:

* Each element becomes a Fiber node
* Reconciliation happens on the Fiber tree
* The work is split into units
* Updates can be paused and resumed

For now, remember:

```text
Reconciliation = comparing UI descriptions
Fiber = how React executes reconciliation
```

---

# Key Takeaways

* Reconciliation is React’s diffing process
* React compares React Elements, not DOM nodes
* Only changed parts of the UI are updated
* Keys provide identity for list items
* Component type and element type determine reuse
* Reconciliation happens before DOM updates
* React is optimized around structural comparison, not visual comparison

---

# Summary

Reconciliation is the mechanism that makes React efficient. Instead of rebuilding the entire UI, React compares previous and new element trees, determines what changed, and updates only those parts in the DOM.

This process is the foundation of React’s performance model and directly connects to the Fiber architecture, which we will explore in the next module.

---

# Looking Ahead

In the next module, we transition from *what React does* to *how React executes it*:

> **Module 2 — State & Hooks Deep Dive**

You will learn:

* Why state updates are asynchronous
* How Hooks are stored internally
* Why hook order matters
* Component lifecycle in detail
* How React schedules updates
* How Fiber executes reconciliation work

This is where React stops being a model and becomes a runtime system.
