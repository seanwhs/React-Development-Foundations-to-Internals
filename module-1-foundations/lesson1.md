# Lesson 1 — Thinking in React

> Module 1 — Foundations

---

# Learning Objectives

By the end of this lesson, you should be able to:

* Explain the problem React solves.
* Describe declarative UI development.
* Understand that **UI is a function of state**.
* Explain why React components exist.
* Describe the React rendering pipeline at a high level.
* Build your first React application using Vite.
* Understand how React differs from manipulating the DOM directly.

---

# Why React Exists

Before React, developers typically manipulated the browser's Document Object Model (DOM) manually.

Suppose we have a counter.

```html
<h1 id="count">0</h1>

<button onclick="increment()">
Increment
</button>
```

```javascript
let count = 0;

function increment() {
    count++;

    document.getElementById("count").innerText = count;
}
```

Although this works, larger applications become increasingly difficult to maintain because developers must constantly synchronize application data with the DOM.

As applications grow, hundreds or thousands of DOM updates become difficult to coordinate.

React was created to solve this synchronization problem.

---

# The Core Philosophy

React can be summarized using one equation.

```
UI = f(State)
```

The user interface is simply a function of application state.

Instead of writing instructions like:

> Change this text.

> Remove this element.

> Create another button.

We describe what the UI should look like for a particular state.

React determines how to update the browser.

---

# Imperative vs Declarative Programming

## Imperative

Tell the computer exactly how to perform every operation.

```javascript
button.innerText = "Save";

button.style.background = "green";

button.disabled = false;
```

The programmer is responsible for every DOM mutation.

---

## Declarative

Describe the desired result.

```jsx
<Button
    color="green"
    disabled={false}
>
    Save
</Button>
```

React calculates the DOM updates automatically.

---

# React is NOT the DOM

One of the biggest misconceptions about React is that it directly manipulates the browser.

It does not.

Instead, React builds an internal description of the interface called a **React Element Tree**.

Only after comparing this tree with the previous version does React update the browser.

```
State

↓

React Components

↓

React Elements

↓

Reconciliation

↓

DOM Updates
```

This separation makes React predictable and efficient.

---

# Components

A component is a reusable function that returns part of the user interface.

```jsx
function Welcome() {
    return <h1>Hello React</h1>;
}
```

Components allow applications to be divided into independent pieces.

```
App

├── Header

├── Sidebar

├── Main

│   ├── Dashboard

│   ├── Chart

│   └── Table

└── Footer
```

Large applications become collections of reusable components.

---

# Everything Starts With State

Suppose our application contains:

```javascript
const user = {
    name: "Sean"
};
```

The UI becomes

```jsx
<h1>Hello {user.name}</h1>
```

If state changes

```javascript
user.name = "Alice";
```

the desired UI becomes

```jsx
<h1>Hello Alice</h1>
```

React computes the difference and updates only what changed.

---

# The React Rendering Pipeline

Although we will study this in depth later, every React application follows the same pipeline.

```
State Changes

↓

Component Functions Execute

↓

JSX Creates React Elements

↓

Fiber Builds Work Units

↓

Reconciliation Finds Differences

↓

Commit Phase Updates DOM
```

This pipeline is the central idea of the entire course.

Every advanced topic eventually fits somewhere inside this sequence.

---

# Creating Your First React Project

Install Node.js.

Create a project using Vite.

```bash
npm create vite@latest
```

Choose

```
React

JavaScript
```

Install dependencies.

```bash
npm install
```

Run the development server.

```bash
npm run dev
```

Project structure

```
src/

    main.jsx

    App.jsx

public/

package.json

vite.config.js
```

---

# Your First Component

```jsx
function App() {

    return (
        <h1>
            Hello React
        </h1>
    );

}

export default App;
```

---

# What Actually Happens?

When the browser loads:

```
main.jsx

↓

<App />

↓

App() executes

↓

returns JSX

↓

JSX becomes React Elements

↓

Fiber creates a Fiber Tree

↓

React compares trees

↓

DOM updates
```

Notice that your component is simply a JavaScript function.

---

# Key Takeaways

React is not a templating engine.

React is not the DOM.

React is an execution engine that computes UI from state.

Components are JavaScript functions.

JSX is only syntax.

State drives rendering.

The browser is updated only after React finishes reconciliation.

---

# Summary

This lesson introduced the mental model that powers every React application.

Instead of manually changing the browser, we describe what the interface should look like for a given application state. React executes component functions, builds a tree of React elements, determines what has changed, and updates the browser efficiently.

Understanding this rendering pipeline is far more important than memorizing JSX syntax because every feature in React—hooks, memoization, concurrent rendering, server components, and Fiber—is built upon this foundation.

---

# Looking Ahead

In Lesson 2 we answer a fundamental question:

> **What exactly is JSX?**

We'll learn:

* Why JSX exists
* How Babel compiles JSX
* `React.createElement()`
* React Elements
* Why JSX is not HTML
* The relationship between JSX and the Virtual DOM

This lesson transitions from *thinking in React* to understanding how React represents user interfaces internally.
