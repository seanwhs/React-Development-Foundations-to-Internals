# Lesson 2 — JSX, React Elements, and the Virtual DOM

> Module 1 — Foundations

---

# Learning Objectives

By the end of this lesson, you should be able to:

* Explain what JSX actually is (and what it is not)
* Understand how JSX is compiled by Babel
* Describe `React.createElement()` and its role
* Understand what a React Element is
* Distinguish between React Elements and DOM Elements
* Explain the concept of the Virtual DOM (as a model, not a feature)
* Connect JSX → React Elements → Fiber → DOM pipeline

---

# What is JSX?

JSX is one of the most misunderstood parts of React.

At first glance, it looks like HTML inside JavaScript:

```jsx
const title = <h1>Hello React</h1>;
```

But JSX is NOT HTML.

It is NOT a template language.

It is NOT interpreted by the browser.

JSX is **syntactic sugar for JavaScript function calls**.

---

# What JSX Compiles To

Every JSX expression is transformed into `React.createElement()` calls.

### Example

```jsx
function Hello() {
    return <h1>Hello!</h1>;
}
```

Becomes:

```javascript
function Hello() {
    return React.createElement("h1", null, "Hello!");
}
```

---

# JSX with Props

```jsx
function Button() {
    return <button disabled={true}>Save</button>;
}
```

Becomes:

```javascript
function Button() {
    return React.createElement(
        "button",
        { disabled: true },
        "Save"
    );
}
```

---

# JSX with Nested Elements

```jsx
function App() {
    return (
        <div>
            <h1>Hello</h1>
            <p>Welcome</p>
        </div>
    );
}
```

Becomes:

```javascript
function App() {
    return React.createElement(
        "div",
        null,
        React.createElement("h1", null, "Hello"),
        React.createElement("p", null, "Welcome")
    );
}
```

---

# What is React.createElement?

`React.createElement()` is a function that describes **what the UI should look like**.

It does NOT create DOM elements.

Instead, it returns a plain JavaScript object.

---

# React Element (Important Concept)

A React Element is:

> A lightweight JavaScript object describing a UI node.

Example:

```javascript
{
  type: "h1",
  props: {
    children: "Hello!"
  }
}
```

This is NOT:

* A DOM node
* A real HTML element
* Something visible in the browser

It is just a **description of UI structure**.

---

# React Elements vs DOM Elements

## React Element (Virtual representation)

```javascript
{
  type: "button",
  props: {
    disabled: true,
    children: "Save"
  }
}
```

---

## DOM Element (Browser representation)

```html
<button disabled>
    Save
</button>
```

---

# Key Insight

React never directly "edits HTML".

Instead:

```
React Elements → React DOM Renderer → Real DOM
```

---

# The Virtual DOM (Correct Mental Model)

The term "Virtual DOM" is often misunderstood.

It is NOT:

* A separate DOM in memory
* A browser feature
* A real tree structure inside the browser

It IS:

> A JavaScript representation of UI created using React Elements.

---

# Why React Uses This Model

React uses this abstraction to:

* Compare UI states efficiently
* Avoid direct DOM manipulation
* Batch updates
* Recompute UI declaratively

---

# Full JSX Pipeline

Every JSX expression follows this pipeline:

```
JSX

↓

Babel Transform

↓

React.createElement()

↓

React Element Tree

↓

Fiber Tree Construction

↓

Reconciliation

↓

DOM Updates
```

This connects directly to Module 1’s rendering pipeline.

---

# JSX is Just Syntax

This is valid React:

```javascript
const element = React.createElement(
    "h1",
    null,
    "Hello"
);
```

JSX is just a cleaner way to write:

```jsx
const element = <h1>Hello</h1>;
```

They produce the exact same result.

---

# Component JSX vs HTML

## JSX (React world)

```jsx
function Title() {
    return <h1 className="title">Hello</h1>;
}
```

## HTML (Browser world)

```html
<h1 class="title">Hello</h1>
```

Important differences:

* `className` instead of `class`
* `htmlFor` instead of `for`
* CamelCase event handlers (`onClick`)
* JavaScript expressions inside `{}`

---

# JSX is Not a Template

Unlike templating systems:

* JSX is not string-based
* JSX is not parsed at runtime
* JSX is compiled ahead of time

It becomes function calls before execution.

---

# How React Builds the UI

When a component runs:

```jsx
function App() {
    return <h1>Hello</h1>;
}
```

React does:

```
1. Execute App()
2. Get JSX result
3. Convert to React Element
4. Store in memory
5. Compare with previous tree
6. Apply changes to DOM
```

---

# JSX Expressions

You can embed JavaScript inside JSX:

```jsx
const name = "Sean";

return <h1>Hello {name}</h1>;
```

This becomes:

```javascript
React.createElement("h1", null, "Hello ", name);
```

---

# Conditional Rendering in JSX

```jsx
function App({ isLoggedIn }) {
    return (
        <div>
            {isLoggedIn ? <h1>Welcome</h1> : <h1>Please Login</h1>}
        </div>
    );
}
```

JSX simply embeds JavaScript expressions.

---

# JSX Children

Children are passed as arguments to `createElement`.

```jsx
<div>
    <h1>Hello</h1>
</div>
```

Becomes:

```javascript
React.createElement(
    "div",
    null,
    React.createElement("h1", null, "Hello")
);
```

---

# Key Takeaways

* JSX is syntactic sugar for `React.createElement()`
* React Elements are plain JavaScript objects
* React Elements are NOT DOM nodes
* The Virtual DOM is a conceptual model, not a real browser feature
* React builds a UI description tree before touching the DOM
* JSX is compiled before runtime (via Babel)
* React separates UI description from UI rendering

---

# Summary

JSX is not magic — it is just JavaScript.

React uses JSX to create a tree of React Elements, which describe the UI in a lightweight, immutable format. This abstraction allows React to compute differences efficiently and update the DOM only where necessary.

Understanding this pipeline is essential because everything in React — components, hooks, reconciliation, Fiber, and concurrent rendering — depends on this representation layer.

---

# Looking Ahead

In Lesson 3, we move one level deeper into React’s core model:

> **What is reconciliation, and how does React decide what changed?**

We will explore:

* Diffing algorithms (high-level)
* Why React does not compare every node
* Key-based identity in lists
* Component identity vs element identity
* Why order matters in reconciliation
* How state is preserved across renders

This is where React transitions from a UI description system → to a change detection system.
