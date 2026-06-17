# Capstone Project — Synthesis Application (Production React System Design)

> Module 5 — React Development: Foundations to Internals

---

# Overview

This capstone is a **systems-level React engineering exercise**.

You are not building a “project.”

You are designing a **production-grade frontend system** that demonstrates your ability to:

* model state correctly
* structure scalable UI architecture
* reason about rendering behavior
* manage performance under real-world constraints
* apply React internals (Fiber, scheduling, reconciliation) indirectly through design decisions

This is an **engineering judgment assessment**, not a feature implementation tutorial.

---

# Core Objective

Design and implement a React application that demonstrates:

> A scalable, maintainable, and performance-aware UI system built on sound React architecture principles.

Your system should reflect an understanding that:

```text
UI = f(State)
```

and that every state update flows through:

```text
Scheduler → Fiber Render → Reconciliation → Commit → UI
```

---

# Suggested System Domains

Choose one domain and treat it as a **real product system**, not a demo:

* Task Management Platform (Notion/Trello-scale thinking)
* Analytics / Observability Dashboard (high render complexity)
* E-commerce Admin + Inventory System
* Real-time Collaboration Tool (chat or workspace)
* Personal Finance / Banking Dashboard
* Learning Management System (LMS with content + tracking)

You may extend the domain, but complexity must remain **system-realistic**.

---

# System Design Requirements

## 1. Architecture as a First-Class Concern

You must design your system as a set of **bounded UI domains**:

```text
Application Shell
├── Routing Layer
├── Layout System
├── Domain Modules
│   ├── Feature A
│   ├── Feature B
│   ├── Feature C
├── Shared UI System
└── Infrastructure Layer (API, caching, utilities)
```

### Expectations:

* Each domain should be independently understandable
* Cross-domain dependencies must be explicit and minimal
* Components should reflect **responsibility boundaries**, not file grouping

---

## 2. State Architecture (Critical)

You must explicitly define:

* **Local state** (UI-level state)
* **Derived state** (computed from other state)
* **Shared state** (cross-component)
* **Server state** (external synchronization)

### Required justification:

For every state decision:

> Why does this state live here?

You should be able to defend state placement in terms of:

* render behavior
* update frequency
* dependency coupling
* reconciliation cost

---

## 3. Data Flow Model

Your system must enforce:

```text
Source of Truth → State Layer → Component Tree → UI
```

### Constraints:

* No circular state dependencies
* No hidden mutation flows
* Avoid uncontrolled prop drilling
* Prefer composition or scoped context

You are designing a **predictable render pipeline**, not just UI components.

---

## 4. Hooks & React Execution Discipline

You must demonstrate correct understanding of React execution:

Required Hooks:

* `useState`
* `useEffect`
* `useRef`
* `useMemo`
* `useCallback`
* Custom Hooks

### Required discipline rules:

* Effects must represent **external synchronization only**
* Memoization must be **intentional, not default**
* Derived values should be classified explicitly:

  * compute in render vs memoized vs externalized
* No hidden side effects in render logic

---

## 5. Side Effect Architecture

All side effects must be isolated into controlled boundaries:

* API layer (`services/`)
* custom hooks (`hooks/`)
* effect handlers (`useEffect` usage discipline)

### Required behaviors:

* Proper cleanup (subscriptions, listeners, timers)
* Cancellation handling for async flows
* No uncontrolled side effects in components

You are modeling:

> React render phase must remain pure

---

## 6. Performance Model (React Internals Awareness)

You are not required to micro-optimize.

You ARE required to demonstrate reasoning about:

* render frequency
* reconciliation cost
* subtree invalidation
* prop stability
* list rendering behavior
* unnecessary work triggered by state updates

### Expected mindset:

You should be able to answer:

> “What does this state change trigger in the Fiber tree?”

---

## 7. Rendering Boundaries (Advanced Requirement)

You must intentionally design at least one:

* UI boundary (layout isolation)
* state boundary (context or scoped store)
* rendering boundary (lazy-loaded or deferred UI)
* async boundary (loading/error separation)

This demonstrates understanding of:

```text
Render → Reconcile → Commit → UI
```

and how to control work propagation.

---

## 8. File Structure (Guided, Not Strict)

Your structure should reflect **domain boundaries**, not just file types:

```text
src/

├── app/               # Application bootstrap & routing
├── domains/          # Feature domains (core system units)
├── components/       # Shared UI primitives
├── hooks/            # Custom React hooks
├── services/         # API / external systems
├── state/            # Shared state coordination
├── utils/            # Pure functions
└── styles/
```

### Key Principle:

> Structure should reflect execution boundaries, not organization convenience.

---

# Engineering Constraints

## Required Principles

Your system must demonstrate:

* composition over inheritance
* separation of concerns
* predictable render behavior
* explicit state ownership
* minimal cross-module coupling

---

## Forbidden Patterns

* global state without architectural justification
* mixing rendering logic with side effects
* monolithic components (>200 lines without clear separation)
* uncontrolled prop drilling (without reasoning)
* memoization without measurable or structural reason

---

# React Architecture Alignment

Your system should implicitly respect React’s internal pipeline:

```text
State Update
   ↓
Scheduler (priority assignment)
   ↓
Fiber Render (component execution)
   ↓
Reconciliation (tree diffing)
   ↓
Commit Phase (DOM mutation)
   ↓
UI Update
```

You are not implementing this pipeline — but your architecture should behave *as if it exists underneath every decision*.

---

# Evaluation Criteria

## 1. System Design Quality

* clarity of boundaries
* scalability of architecture
* separation of domains

## 2. State Modeling

* correctness of placement
* minimal redundancy
* predictable updates

## 3. React Correctness

* proper hook usage
* correct effect isolation
* no render-phase side effects

## 4. Performance Awareness

* understanding of render triggers
* avoidance of unnecessary work
* awareness of reconciliation impact

## 5. Engineering Judgment

* ability to justify trade-offs
* awareness of alternatives
* clarity of reasoning in README

---

# Deliverables

You must provide:

* Full source code
* Architecture README (mandatory)
* State model explanation (mandatory)
* Component/domain breakdown
* (Optional) diagram of system flow

---

# Final Note

This capstone is not testing whether you can build a React app.

It is testing whether you can:

> design a system that behaves correctly under React’s execution model

You are being evaluated on:

* structural thinking
* rendering awareness
* system decomposition
* performance reasoning
* architectural judgment

---

# Outcome

By completing this capstone, you should be able to:

* design React systems from scratch
* reason about rendering behavior at scale
* structure production-grade frontend architectures
* make informed performance trade-offs
* think in terms of Fiber execution (implicitly)

---

# End of Capstone Specification

Just tell me.
