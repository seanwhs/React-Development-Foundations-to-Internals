# Lesson 4 — React Server Components, Suspense, and Rendering Boundaries

> Module 4 — Performance

---

# Learning Objectives

By the end of this lesson, you should be able to:

* Explain what React Server Components (RSC) are
* Distinguish between server rendering and client rendering
* Understand why React is shifting work away from the browser
* Explain Suspense as a rendering boundary mechanism
* Understand how code splitting reduces bundle cost
* Describe streaming UI at a high level
* Understand how rendering boundaries affect performance
* Build a mental model of “where work happens” in modern React

---

# Where We Are in the Performance Model

So far:

### Lesson 1

* React performance = scheduling + rendering efficiency

### Lesson 2

* Memoization reduces unnecessary renders

### Lesson 3

* Concurrency improves scheduling and responsiveness

Now we move beyond the browser:

> **Can we avoid doing work in the browser at all?**

---

# The New Performance Problem

Even with concurrency:

* JavaScript still runs in the browser
* Large bundles still download
* Heavy components still execute client-side
* Initial load still blocks interaction

So the next optimization is radical:

> Move work out of the browser entirely.

---

# The Core Idea: Split the Rendering World

Modern React divides rendering into two environments:

```text id="a1v8qp"
Server
Client (Browser)
```

Each has different responsibilities.

---

# Why This Matters

The browser is expensive:

* Limited CPU
* Limited memory
* Competes with UI thread
* Must handle input + rendering + scripts

The server is not:

* More CPU available
* No UI blocking
* Can precompute results
* Can stream responses

---

# React Server Components (RSC)

Server Components allow:

> Components that execute only on the server

Example:

```jsx id="r9x2mn"
async function ProductList() {
    const products = await fetchProducts();

    return (
        <ul>
            {products.map(p => (
                <li key={p.id}>{p.name}</li>
            ))}
        </ul>
    );
}
```

---

# Key Property

Server Components:

* Do NOT run in the browser
* Do NOT increase bundle size
* Do NOT ship JavaScript to client
* Execute only on server

---

# Client Components

Client Components:

* Run in browser
* Handle interactivity
* Use hooks like `useState`, `useEffect`

Example:

```jsx id="c3v8qp"
"use client";

function Counter() {
    const [count, setCount] = useState(0);

    return (
        <button onClick={() => setCount(c => c + 1)}>
            {count}
        </button>
    );
}
```

---

# The Key Split

```text id="k8v2mn"
Server Components → data + rendering
Client Components → interactivity
```

---

# Why This Is a Performance Revolution

Previously:

* Everything shipped to browser
* Everything executed in client JS

Now:

* Heavy work stays on server
* Only interactive parts reach browser

---

# Bundle Size Reduction

Without RSC:

```text id="b2q9wx"
All components → JavaScript bundle
```

With RSC:

```text id="x7v3qp"
Server components → NOT shipped
Client components → only interactivity shipped
```

Result:

> Smaller bundles, faster load times

---

# Suspense: Rendering Boundaries

Suspense allows React to:

> Pause rendering until something is ready

Example:

```jsx id="s8v2qz"
<Suspense fallback={<Loading />}>
    <ProductList />
</Suspense>
```

---

# What Suspense Does

Instead of blocking the UI:

```text id="t2q9wx"
Wait for data → block render → show UI
```

Suspense:

```text id="m7v3qp"
Render fallback immediately
→ stream real UI when ready
```

---

# Streaming UI Concept

React can send UI in parts:

```text id="c9v1mn"
Shell loads
   ↓
Header renders
   ↓
Main content streams in
   ↓
Heavy components load later
```

---

# Why Streaming Matters

It improves:

* Time to First Paint
* Perceived performance
* User experience
* Interaction readiness

---

# Suspense + Server Components Together

They combine into:

```text id="p3v8qp"
Server renders component
   ↓
Streams result
   ↓
Client hydrates interactive parts
```

---

# Code Splitting

Without code splitting:

```text id="k4v9mn"
Entire app downloaded at once
```

With code splitting:

```text id="x2v7qp"
Only needed modules loaded initially
Others loaded on demand
```

---

# React.lazy

Example:

```jsx id="l8v2mn"
const Dashboard = React.lazy(() => import("./Dashboard"));

function App() {
    return (
        <Suspense fallback="Loading...">
            <Dashboard />
        </Suspense>
    );
}
```

---

# Why This Improves Performance

* Smaller initial bundle
* Faster startup
* Reduced parsing time
* Better caching

---

# Rendering Boundaries

React now defines boundaries for work:

| Boundary Type       | Purpose                |
| ------------------- | ---------------------- |
| Server boundary     | Avoid client JS        |
| Suspense boundary   | Delay rendering safely |
| Code split boundary | Load lazily            |
| Transition boundary | Prioritize updates     |

---

# Mental Model Shift

Before:

❌ Everything is rendered in one place

Now:

✔ Rendering is distributed across boundaries

---

# Where Work Happens Now

```text id="z8v3qp"
Server:
  - Data fetching
  - Heavy computation
  - Static rendering

Client:
  - Interactivity
  - Event handling
  - UI updates

Suspense:
  - Coordination layer
```

---

# Why React Architecture Keeps Expanding

React is evolving from:

> UI library

to:

> distributed rendering system

---

# Performance Strategy Hierarchy

React performance is now layered:

### 1. Reduce client work (RSC)

### 2. Reduce rendering work (memoization)

### 3. Schedule work efficiently (concurrency)

### 4. Execute work efficiently (Fiber)

---

# Real-World Example: E-commerce Page

Without modern React:

* Load full JS bundle
* Render all components
* Fetch data in browser
* Block UI until ready

---

With RSC + Suspense:

```text id="e3v8qp"
Server renders product list
   ↓
Streams HTML
   ↓
Client hydrates only buttons/cart
   ↓
Page becomes interactive early
```

---

# Key Insight

> Performance is no longer just runtime optimization
> It is architectural distribution of computation

---

# Common Misunderstandings

❌ Server Components are APIs
✔ They are execution environments

❌ Suspense is loading UI
✔ Suspense is a rendering coordination system

❌ React runs everything in browser
✔ React splits execution between server and client

---

# Key Takeaways

* React Server Components move rendering to the server
* Client Components handle interactivity
* Suspense defines rendering boundaries
* Code splitting reduces initial JavaScript load
* Streaming improves perceived performance
* React now distributes rendering across environments
* Performance is architectural, not just runtime-based
* Boundaries define how and where work is executed

---

# Summary

Modern React performance extends beyond the browser. By splitting execution between server and client, React reduces bundle size, improves load times, and shifts heavy computation away from the UI thread. Suspense and streaming enable gradual rendering, while code splitting ensures only necessary code is loaded.

Together, these mechanisms transform React into a distributed rendering system where performance is determined by architecture rather than just optimization techniques.

---

# Module 4 Complete

You now understand:

* Memoization strategies
* Concurrency and scheduling
* React Server Components
* Suspense and streaming
* Code splitting
* Rendering boundaries

---

# Next Module Preview

> **Module 5 — Capstone: Production React Architecture**

We will now synthesize everything:

* Fiber architecture
* Reconciliation system
* Scheduling model
* Performance strategies
* Server/client boundaries

Into a single real-world system design:

> A production-grade React application architecture

This is where React stops being concepts—and becomes engineering design.
