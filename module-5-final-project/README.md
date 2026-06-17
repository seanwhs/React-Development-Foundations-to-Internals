# Capstone Project — Synthesis Application

> Module 5 — React Development: Foundations to Internals

---

# Overview

This is the **capstone project** of the curriculum.

You are expected to design and build a **production-grade React application** that synthesizes everything learned across all modules:

* JSX & Components (Module 1)
* State & Hooks (Module 2)
* Fiber & Reconciliation mental model (Module 3)
* Performance & Concurrent React thinking (Module 4)

This is not a tutorial project.

It is an **engineering assessment of React mastery**.

---

# Objective

Build a **fully functional React application** that demonstrates:

* Strong component architecture
* Correct state modeling
* Clean data flow
* Realistic side-effect handling
* Performance-aware design
* Scalable folder and module structure

The domain is flexible, but the architecture must be rigorous.

---

# Suggested Project Types

Choose ONE:

* Task Management System (Notion / Trello style)
* Analytics Dashboard (data-heavy UI)
* E-commerce Admin Panel
* Real-time Chat Application
* Personal Finance Tracker
* Learning Management System (LMS)

You may extend or customize the domain, but complexity must be equivalent.

---

# Core Requirements

## 1. Component Architecture

Your application must be decomposed into clear layers:

```text id="c1a8p0"
App
├── Layout
├── Pages
│   ├── Dashboard
│   ├── Detail View
│   ├── Settings
│   └── Profile
├── Features
└── Shared UI Components
```

Each component should have a clear responsibility and boundary.

---

## 2. State Design

You must explicitly define and justify:

* Local state
* Shared state
* Derived state
* Server state (if applicable)

State placement must be intentional, not accidental.

---

## 3. Data Flow

All data must follow a predictable flow:

```text id="d2k9q1"
API → State → Components → UI
```

Requirements:

* Unidirectional data flow
* Minimal prop drilling (use composition or context where appropriate)
* Clear separation between data and presentation

---

## 4. Hooks Usage

You must correctly use:

* `useState`
* `useEffect`
* `useRef`
* `useMemo`
* `useCallback`
* Custom hooks

Rules:

* No unnecessary memoization
* Effects must have explicit dependency logic
* Side effects must be isolated and predictable

---

## 5. Side Effects Management

Your system must correctly handle:

* API requests
* Subscriptions / event listeners
* Cleanup logic
* Async workflows

No side effects should leak into rendering logic.

---

## 6. Performance Considerations

You are expected to reason about:

* Component re-renders
* Prop stability
* List rendering efficiency
* Expensive computations
* Rendering boundaries

You are NOT required to over-optimize, but you MUST demonstrate awareness.

---

## 7. File Structure

Recommended structure:

```text id="s9v3k2"
src/

├── components/        # Reusable UI primitives
├── features/          # Feature-based modules
├── pages/             # Route-level components
├── hooks/             # Custom hooks
├── services/          # API layer
├── state/             # Global/shared state (if needed)
├── utils/             # Pure utilities
├── styles/            # Styling system
└── App.jsx
```

---

# Engineering Constraints

## Required Principles

Your implementation must follow:

* Composition over inheritance
* Separation of concerns
* Predictable state updates
* Locality of reasoning
* Minimal coupling between modules

---

## Forbidden Patterns

Avoid:

* Overuse of global state without justification
* Mixing UI logic and side effects
* Large monolithic components
* Uncontrolled prop drilling
* Unnecessary memoization “by default”

---

# React Architecture Alignment

Your design should implicitly align with React’s internal model:

```text id="r8v2m1"
State Update
    ↓
Scheduler
    ↓
Fiber Render Phase
    ↓
Reconciliation
    ↓
Commit Phase
    ↓
UI Update
```

You are not implementing Fiber — but your architecture should respect its behavior.

---

# Evaluation Criteria

Your project will be evaluated on:

## 1. Architecture Quality

* Clear separation of concerns
* Scalable structure
* Logical module boundaries

## 2. State Design

* Correct placement
* Minimal redundancy
* Predictable updates

## 3. Code Clarity

* Readability
* Maintainability
* Component design

## 4. React Correctness

* Proper Hook usage
* Correct effect management
* No anti-patterns

## 5. Performance Awareness

* Reasonable rendering behavior
* Awareness of re-renders
* Efficient UI updates

---

# Deliverables

You must submit:

* Full source code
* README explaining architecture decisions
* State model explanation
* Component breakdown diagram (optional but recommended)

---

# Final Note

This project is not about demonstrating React knowledge.

It is about demonstrating **engineering judgment using React**.

You are expected to make trade-offs, justify decisions, and structure a system that could scale beyond a toy application.

---

# Outcome

By completing this capstone, you should be able to:

* Design React systems from scratch
* Reason about rendering and performance
* Structure scalable frontend architectures
* Apply React internals knowledge indirectly in real code

---

# End of Capstone Specification
