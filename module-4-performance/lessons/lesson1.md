# Lesson 1 — React Performance: Thinking Like the React Scheduler

> Module 4 — Performance

---

# Learning Objectives

By the end of this lesson, you should be able to:

* Understand what "performance" means in React.
* Explain why React re-renders components.
* Distinguish between rendering and DOM updates.
* Identify unnecessary renders.
* Explain how React's scheduler prioritizes work.
* Understand why React applications remain responsive despite frequent renders.
* Build a mental model for performance optimization before learning memoization.

---

# Where We Are in the Curriculum

Throughout this curriculum we have progressively uncovered React's architecture.

In **Module 1**, we learned:

```text
UI = f(State)
```

React computes the user interface from application state.

---

In **Module 2**, we learned:

* Hooks
* State updates
* Lifecycle
* Effects

Calling `setState()` schedules a render.

---

In **Module 3**, we discovered what actually performs that work:

```text
State Update

↓

Fiber Scheduler

↓

Render Phase

↓

Commit Phase
```

We now understand that React is an execution engine powered by Fiber.

The next question naturally follows:

> **How does React stay fast as applications grow?**

---

# Performance Begins with Measurement

A common mistake is attempting to optimize before identifying a bottleneck.

Performance work should follow a simple process:

```text
Measure

↓

Identify

↓

Optimize

↓

Measure Again
```

Without measurement, optimization often increases complexity without improving the user experience.

---

# What Does "Slow" Mean?

Performance problems can arise from different sources.

* Slow JavaScript execution
* Expensive rendering
* Excessive DOM updates
* Large component trees
* Network latency
* Large bundle sizes
* Blocking synchronous work

React primarily addresses rendering performance, but understanding the broader picture helps you choose the right optimization strategy.

---

# Rendering Is Not the Same as Updating the DOM

One of the most important concepts in React performance is distinguishing **rendering** from **committing**.

Rendering means React executes component functions and builds a new React element tree.

```jsx
function Dashboard() {
    console.log("Rendering...");
    return <Chart />;
}
```

This does **not** update the browser.

Only after reconciliation determines what changed does React enter the Commit Phase.

```text
State Update

↓

Render

↓

Compare Trees

↓

Commit

↓

DOM Updates
```

Rendering can happen many times while DOM updates remain minimal.

---

# Why Does React Re-render So Much?

Consider this component.

```jsx
function Counter() {

    const [count, setCount] = useState(0);

    return (
        <>
            <Display count={count} />
            <Button onClick={() => setCount(count + 1)} />
        </>
    );
}
```

Every call to `setCount()` executes `Counter` again.

This also executes its child components unless React determines they can be skipped.

React intentionally favors predictable rendering over manual optimization.

---

# Re-rendering Is Usually Cheap

Many developers assume every render is expensive.

In reality, most component renders involve:

* Executing JavaScript functions
* Creating lightweight React Elements
* Comparing trees in memory

None of these operations modify the browser.

The expensive step is usually:

```text
DOM Updates

Layout

Painting
```

React minimizes these operations through reconciliation.

---

# The Cost of Unnecessary Work

Imagine a dashboard containing hundreds of components.

```text
Dashboard

├── Sidebar

├── Notifications

├── Charts

├── Analytics

├── Tables

├── Filters

└── User Profile
```

If updating a notification causes every component to re-render, the application performs more work than necessary.

React's performance tools help reduce this unnecessary computation.

---

# React's Performance Philosophy

React does not try to prevent rendering.

Instead, it focuses on making rendering:

* Predictable
* Interruptible
* Prioritized
* Efficient

Rather than asking:

> "Can React avoid rendering?"

React asks:

> "Can React perform rendering efficiently and only commit meaningful changes?"

---

# Scheduling Work

Fiber introduced a scheduler that assigns priority to updates.

Conceptually:

```text
User Types

↓

High Priority

↓

Render Immediately
```

Background updates receive lower priority.

```text
Analytics Update

↓

Low Priority

↓

Render Later
```

This prioritization keeps interfaces responsive during heavy workloads.

---

# Update Priorities

React groups updates into different priority levels.

Conceptually:

| Priority  | Examples             |
| --------- | -------------------- |
| Immediate | User input, typing   |
| High      | Button clicks        |
| Normal    | State updates        |
| Low       | Background rendering |
| Idle      | Non-urgent work      |

The scheduler always attempts to complete the most important work first.

---

# Interruptible Rendering

Before Fiber:

```text
Render Everything

↓

Finish

↓

Browser Responds
```

With Fiber:

```text
Render

↓

Pause

↓

Handle User Input

↓

Resume Rendering
```

Interruptible rendering improves responsiveness without sacrificing correctness.

---

# Concurrent Rendering

Concurrent Rendering allows React to prepare a new UI without blocking the current one.

```text
Current UI

↓

Background Render

↓

User Continues Working

↓

Render Complete

↓

Commit
```

The interface remains interactive while React prepares the next screen.

Concurrent rendering does **not** mean multiple threads; it means React can pause, resume, and prioritize rendering on a single JavaScript thread.

---

# React Profiler

Performance optimization starts with observation.

The React Profiler helps answer questions such as:

* Which components rendered?
* Why did they render?
* How long did rendering take?
* Which updates were expensive?

Typical workflow:

```text
Profile

↓

Identify Slow Components

↓

Optimize

↓

Profile Again
```

---

# Avoid Premature Optimization

Optimization introduces complexity.

For example, memoization can reduce rendering work, but it also adds comparison overhead.

Before optimizing, ask:

* Is the component actually slow?
* Does it render frequently?
* Is the optimization measurable?
* Does the optimization improve user experience?

A simple component often performs better without additional optimization.

---

# A Performance Mental Model

Think of React performance as a pipeline.

```text
State Update

↓

Scheduler

↓

Fiber

↓

Render Phase

↓

Reconciliation

↓

Commit Phase

↓

Browser Paint
```

Performance improvements target different stages of this pipeline.

For example:

* Memoization reduces unnecessary rendering.
* Concurrent Rendering improves scheduling.
* React Server Components reduce client-side work.
* Code splitting reduces download size.
* Virtualization reduces rendered elements.

---

# Looking Ahead

This module explores several advanced performance techniques.

We'll cover:

* `React.memo`
* `useMemo`
* `useCallback`
* React Profiler
* Concurrent Rendering
* `startTransition`
* `useTransition`
* `useDeferredValue`
* React Server Components (RSC)
* Code Splitting
* Lazy Loading
* Suspense
* Rendering large lists efficiently

Each topic builds upon the Fiber architecture introduced in Module 3.

---

# Key Takeaways

* Performance begins with measurement, not optimization.
* Rendering is not the same as updating the DOM.
* React intentionally re-renders components to maintain correctness.
* Most renders are inexpensive because they occur entirely in memory.
* Fiber enables scheduling, prioritization, and interruptible rendering.
* Concurrent Rendering improves responsiveness by allowing React to prepare updates without blocking the UI.
* Optimization techniques should be applied only when they solve measurable problems.

---

# Summary

Modern React performance is the result of architectural design rather than isolated optimizations. Fiber separates rendering from committing, allowing React to schedule, interrupt, and prioritize work based on user interactions.

Instead of preventing renders, React focuses on minimizing expensive operations and keeping interfaces responsive. Understanding this philosophy provides the foundation for using memoization, concurrent rendering, and server components effectively rather than treating them as isolated APIs.

---

# Looking Ahead

In **Lesson 2**, we explore **Memoization in React**, including:

* Why components re-render
* `React.memo`
* `useMemo`
* `useCallback`
* Referential equality
* Stable props and callbacks
* When memoization improves performance
* When memoization makes performance worse
* How memoization interacts with the Fiber reconciler

By the end of the next lesson, you'll be able to reason about React performance using the same mental models employed by the React team.
