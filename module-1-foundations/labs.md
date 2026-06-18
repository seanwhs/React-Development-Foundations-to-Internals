# 📁 `module-1-foundations/labs.md`

# Labs — Module 1: Foundations (JSX, Components, State, Props)

This module contains hands-on labs designed to build your **core React mental model**:

> UI = f(State)

Each lab is intentionally minimal, but conceptually deep.

You are not building features.

You are building understanding.

---

# Lab 1 — Imperative vs Declarative UI

## Objective

Understand why React exists by experiencing DOM manipulation pain directly.

---

## Task A — Imperative Counter

Build a counter using only DOM APIs:

```html id="lab1a1"
<h1 id="count">0</h1>
<button id="btn">Increment</button>
````

```js id="lab1a2"
let count = 0;

document.getElementById("btn").addEventListener("click", () => {
  count++;
  document.getElementById("count").innerText = count;
});
```

---

## Reflection Questions

* What must you manually update?
* What happens if UI grows to 20 elements?
* Where does complexity come from?

---

## Expected Insight

> UI and state must be manually synchronized in imperative systems.

---

# Lab 2 — First React Component

## Objective

Understand components as functions.

---

## Task

Create a simple component:

```jsx id="lab2a1"
function Hello() {
  return <h1>Hello React</h1>;
}

export default Hello;
```

---

## Extension

Add dynamic props:

```jsx id="lab2a2"
function Hello({ name }) {
  return <h1>Hello {name}</h1>;
}
```

---

## Reflection

* What is a component?
* What does it return?
* Is it a function or UI?

---

## Expected Insight

> A component is a function that returns a UI description.

---

# Lab 3 — Props as Data Flow

## Objective

Understand one-way data flow.

---

## Task

Build:

```jsx id="lab3a1"
function App() {
  return <User name="Sean" age={25} />;
}

function User(props) {
  return (
    <>
      <h1>{props.name}</h1>
      <p>{props.age}</p>
    </>
  );
}
```

---

## Challenge

Add 3 users and render multiple components.

---

## Reflection

* Where does data originate?
* Can child modify props?

---

## Expected Insight

> Props are immutable inputs flowing downward.

---

# Lab 4 — JSX is Not HTML

## Objective

Understand JSX compilation mentally.

---

## Task

Rewrite mentally:

```jsx id="lab4a1"
const element = <h1>Hello</h1>;
```

Into conceptual form:

```js id="lab4a2"
React.createElement("h1", null, "Hello");
```

---

## Experiment

Try nesting:

```jsx id="lab4a3"
<div>
  <h1>Hello</h1>
</div>
```

---

## Reflection

* What does JSX compile into?
* Is JSX executed by browser?

---

## Expected Insight

> JSX is syntactic sugar for React element creation.

---

# Lab 5 — Component Tree Construction

## Objective

Understand UI as a tree.

---

## Task

Build:

```jsx id="lab5a1"
function App() {
  return (
    <>
      <Header />
      <Main />
      <Footer />
    </>
  );
}
```

---

## Subcomponents

```jsx id="lab5a2"
function Header() {
  return <h1>Header</h1>;
}

function Main() {
  return <p>Main content</p>;
}

function Footer() {
  return <small>Footer</small>;
}
```

---

## Reflection

* What is the shape of your app?
* How does React interpret structure?

---

## Expected Insight

> React applications are trees of components.

---

# Lab 6 — State Introduction (Preview)

## Objective

Observe non-reactive variables.

---

## Task

```jsx id="lab6a1"
function Counter() {
  let count = 0;

  function increment() {
    count++;
    console.log(count);
  }

  return (
    <>
      <h1>{count}</h1>
      <button onClick={increment}>Increment</button>
    </>
  );
}
```

---

## Observation

UI does NOT update.

---

## Reflection

* Why doesn’t UI change?
* What is missing?

---

## Expected Insight

> React does not track normal variables.

---

# Lab 7 — Thinking in State (Preview of Next Module)

## Objective

Introduce mental model shift.

---

## Task

Rewrite conceptually:

* UI should reflect state
* not variables

---

## Reflection

Complete sentence:

> “The UI is always a function of ______”

---

## Expected Answer

> state

---

# Lab Summary

After completing this module, you should understand:

* components are functions
* JSX is not HTML
* props are immutable inputs
* UI is derived, not manually updated
* React builds tree structures
* state is required for reactivity (next module)

---

# Final Principle

> If you are still thinking in DOM manipulation, you are not thinking in React.

---

# End of Labs
