# 📄 `resources/03-fiber-architecture.md`

# React Fiber Architecture

This document explains React Fiber, the internal execution engine of React.

---

## 1. What is Fiber?

Fiber is React’s internal unit of work.

Every component in your app corresponds to a Fiber node.

---

## 2. Why Fiber Exists

Before Fiber, React rendering was:

- synchronous
- recursive
- blocking

This caused UI freezes on large trees.

Fiber fixes this by making rendering:

- interruptible
- schedulable
- prioritizable

---

## 3. Fiber Node Structure (Conceptual)

Each Fiber node represents a component instance:

```js id="xq9v2k"
const fiber = {
    type,        // component type
    props,       // input data
    state,       // local state
    child,       // first child fiber
    sibling,     // next sibling fiber
    return,      // parent fiber
    alternate,   // previous fiber (work-in-progress)
    flags        // side effects to apply
};
````

---

## 4. Fiber Tree

React transforms the component tree into a Fiber tree:

App
→ Header
→ Main
→ Footer

Each becomes a Fiber node linked via:

* child
* sibling
* return

---

## 5. Work-in-Progress Model

React maintains two trees:

* Current Fiber tree (what is on screen)
* Work-in-progress Fiber tree (being built)

Only when complete does React swap them.

---

## 6. Render vs Commit

### Render Phase (can be paused)

* Execute components
* Build Fiber tree
* Calculate changes

### Commit Phase (cannot be paused)

* Apply DOM updates
* Run effects

---

## 7. Key Insight

Fiber is React’s execution engine.

It allows React to break rendering into small units of work instead of blocking the browser.

```
