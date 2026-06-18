# 📁 `module-4-performance/lesson1.md`

# Lesson 1 — React Performance Mental Model

> Module 4 — Performance

---

# Learning Objectives

By the end of this lesson, you should be able to:

- Understand what “performance” means in React
- Distinguish render vs commit costs
- Understand why re-renders happen
- Identify unnecessary work
- Build a mental model for optimization

---

# What “Performance” Actually Means

React performance is NOT:

- “avoiding re-renders”

React performance IS:

- minimizing unnecessary work
- reducing expensive DOM operations
- keeping UI responsive

---

# Render vs Commit

## Render Phase

- function execution
- JSX creation
- reconciliation planning

## Commit Phase

- DOM updates
- layout recalculation
- paint

---

# Key Insight

Rendering is cheap.

DOM updates are expensive.

---

# Why React Re-renders

React re-renders when:

- state changes
- props change
- parent re-renders

This is expected behavior.

---

# Performance Mental Model

```text id="m1p9k2"
State Change
   ↓
Render Phase
   ↓
Reconciliation
   ↓
Commit Phase
   ↓
DOM Update
````

---

# Key Insight

Performance is about controlling work, not eliminating renders.

---

# 📁 `module-4-performance/lesson2.md`

# Lesson 2 — Memoization in React

---

# Learning Objectives

- Understand React.memo
- Understand useMemo
- Understand useCallback
- Understand referential equality
- Know when NOT to use memoization

---

# Why Memoization Exists

React re-renders components frequently.

Memoization helps avoid unnecessary work.

---

# React.memo

Prevents re-render if props are unchanged.

```jsx id="r2k9mv"
const Child = React.memo(function Child({ value }) {
    console.log("Render Child");
    return <div>{value}</div>;
});
````

---

# useMemo

Caches computed values.

```jsx id="m9k2vx"
const result = useMemo(() => {
    return expensiveCalculation(data);
}, [data]);
```

---

# useCallback

Caches function references.

```jsx id="c7v2mp"
const handleClick = useCallback(() => {
    setCount(c => c + 1);
}, []);
```

---

# Referential Equality

```js id="e8k2mv"
{} !== {}
[] !== []
() => {} !== () => {}
```

React compares references, not deep values.

---

# When NOT to Use Memoization

* small components
* cheap computations
* rarely re-rendering components

---

# Key Insight

Memoization is a tool for reducing work, not a default practice.

---

# 📁 `module-4-performance/lesson3.md`

# Lesson 3 — Concurrent React

---

# Learning Objectives

- Understand concurrency
- Understand transitions
- Understand scheduling
- Understand non-blocking rendering

---

# What is Concurrent React?

Concurrent React allows rendering to be:

- interruptible
- prioritized
- deferred

---

# Transitions

Mark updates as non-urgent:

```jsx id="t2k9mv"
startTransition(() => {
    setState(value);
});
````

---

# useTransition

```jsx id="u9k2mv"
const [isPending, startTransition] = useTransition();
```

---

# useDeferredValue

```jsx id="d2k9mv"
const deferred = useDeferredValue(value);
```

Delays expensive updates.

---

# Key Insight

React prioritizes user input over background work.

---

# 📁 `module-4-performance/lesson4.md`

# Lesson 4 — React Server Components (RSC)

---

# Learning Objectives

- Understand Server Components
- Distinguish server vs client rendering
- Reduce bundle size conceptually

---

# What are Server Components?

Server Components:

- run on server only
- never shipped to browser
- reduce JS bundle size

---

# Client Components

Client Components:

- have state
- handle events
- run in browser

---

# Separation Model

```text id="s2k9mv"
Server → fetch data
Client → render UI
````

---

# Why RSC Matters

* less JavaScript
* faster initial load
* better performance

---

# Key Insight

React is shifting from client-centric UI to hybrid server-client rendering.

---

# 📁 `module-4-performance/exercise.md`

# Exercise — Optimize a Slow UI

---

# Task

You are given a dashboard with:

- large list rendering
- expensive computations
- frequent re-renders

---

# Requirements

- identify unnecessary renders
- apply memoization where appropriate
- avoid over-optimization

---

# Goals

- improve rendering efficiency
- maintain readability
- avoid premature optimization

---

# Bonus

Introduce `useTransition` for filtering large lists.

---

# 📁 `module-4-performance/solution.md`

# Solution — Module 4 Performance

---

# Example Optimization

```jsx id="o2k9mv"
const Item = React.memo(function Item({ value }) {
    return <div>{value}</div>;
});
````

---

# Memoized Filtering

```jsx id="f9k2mv"
const filtered = useMemo(() => {
    return items.filter(i => i.active);
}, [items]);
```

---

# Transition Example

```jsx id="t9k2mv"
const handleChange = (value) => {
    startTransition(() => {
        setFilter(value);
    });
};
```

---

# Key Takeaway

Optimize only after identifying real performance bottlenecks.

```
