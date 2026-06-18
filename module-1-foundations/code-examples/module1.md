# 📁 `module-1-foundations/lesson1.md`

# Lesson 1 — Thinking in React

> Module 1 — Foundations (JSX, Components, State, Props)

---

# Learning Objectives

By the end of this lesson, you should be able to:

- Explain why React exists
- Understand declarative UI
- Distinguish imperative vs declarative programming
- Understand UI as a function of state
- Build your first React component mentally

---

# Why React Exists

Before React, UI was updated manually:

```js
document.getElementById("count").innerText = count;
````

This creates a problem:

> You must constantly synchronize state and UI manually.

As applications grow, this becomes unmanageable.

---

# React’s Core Idea

React simplifies UI development:

```
UI = f(State)
```

Instead of telling the browser HOW to update, we describe WHAT the UI should be.

---

# Imperative vs Declarative

## Imperative

```js
button.innerText = "Save";
button.disabled = false;
```

You control every step.

---

## Declarative

```jsx
<Button disabled={false}>Save</Button>
```

React decides how to update the DOM.

---

# Components

A component is a function that returns UI:

```jsx
function Hello() {
    return <h1>Hello React</h1>;
}
```

Components allow reuse and composition.

---

# Component Tree

Applications are built from components:

```
App
├── Header
├── Main
├── Sidebar
└── Footer
```

---

# Key Insight

React is not a templating system.

It is a system for describing UI as a function of state.


---

# 📁 `module-1-foundations/lesson2.md`

# Lesson 2 — JSX and Props

---

# Learning Objectives

By the end of this lesson, you should be able to:

- Understand JSX syntax
- Understand how JSX becomes JavaScript
- Explain React elements
- Use props for component communication
- Understand one-way data flow

---

# What is JSX?

JSX looks like HTML but is NOT HTML.

```jsx
const element = <h1>Hello</h1>;
````

It compiles to:

```js
React.createElement("h1", null, "Hello");
```

---

# React Elements

A React Element is a plain object:

```js
{
  type: "h1",
  props: {
    children: "Hello"
  }
}
```

---

# Props

Props are inputs to components:

```jsx
function Greeting(props) {
    return <h1>Hello {props.name}</h1>;
}
```

Usage:

```jsx
<Greeting name="Sean" />
```

---

# One-Way Data Flow

Data flows:

```
Parent → Child
```

Never:

```
Child → Parent (directly)
```

---

# Key Insight

Props make components predictable and reusable.


---

# 📁 `module-1-foundations/lesson3.md`

# Lesson 3 — State in React

---

# Learning Objectives

By the end of this lesson, you should be able to:

- Understand what state is
- Use useState correctly
- Understand re-rendering
- Distinguish state vs variables

---

# Why State Exists

State represents data that changes over time:

- counters
- forms
- UI toggles

---

# Variables vs State

```jsx
let count = 0;
````

Changing this does NOT update UI.

---

# useState

```jsx
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

# What Happens on Update

```
setState()
→ re-render component
→ new UI returned
→ React updates DOM
```

---

# Key Insight

State is what triggers rendering in React.

---

# 📁 `module-1-foundations/exercise.md`

# Exercise — Build a Simple UI System

---

# Task

Build a small React app with:

## Requirements

- A `Counter` component
- A `Greeting` component
- A parent `App` component

---

# Counter Requirements

- Display a number
- Button increases count
- Use `useState`

---

# Greeting Requirements

- Accept a `name` prop
- Display: Hello {name}

---

# App Requirements

- Combine both components
- Pass props correctly

---

# Bonus

Add a reset button to Counter.

---

# 📁 `module-1-foundations/solution.md`

# Solution — Module 1 Foundations

---

# Counter Component

```jsx
import { useState } from "react";

function Counter() {
    const [count, setCount] = useState(0);

    return (
        <>
            <h2>{count}</h2>

            <button onClick={() => setCount(count + 1)}>
                Increment
            </button>

            <button onClick={() => setCount(0)}>
                Reset
            </button>
        </>
    );
}

export default Counter;
````

---

# Greeting Component

```jsx
function Greeting({ name }) {
    return <h1>Hello {name}</h1>;
}

export default Greeting;
```

---

# App Component

```jsx
import Counter from "./Counter";
import Greeting from "./Greeting";

function App() {
    return (
        <>
            <Greeting name="Sean" />
            <Counter />
        </>
    );
}

export default App;
```

---

# Key Takeaway

React apps are composed by combining:

* stateful components
* reusable props-based components
* declarative UI functions

