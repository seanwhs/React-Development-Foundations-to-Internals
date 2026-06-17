# 📄 `resources/01-react-fundamentals.md`

# React Fundamentals — Mental Model Reference

This document summarizes the core mental model of React as a UI system.

---

## 1. React’s Core Equation

React is built on a single principle:

UI = f(State)

The user interface is a deterministic function of application state.

---

## 2. Declarative vs Imperative UI

### Imperative

You manually update the DOM:

```js
element.innerText = "Hello";
````

### Declarative (React)

You describe what the UI should be:

```jsx
<h1>Hello</h1>
```

React handles DOM updates automatically.

---

## 3. Components

Components are JavaScript functions that return UI descriptions:

```jsx
function App() {
    return <h1>Hello React</h1>;
}
```

---

## 4. Rendering Model

React rendering follows this pipeline:

State Change
→ Component Execution
→ React Elements
→ Reconciliation
→ Commit (DOM Update)

---

## 5. Key Insight

React does not “update the UI”.

It recomputes a UI description and applies only the minimal necessary changes.

```


👉 `resources/02-jsx-and-elements.md`
