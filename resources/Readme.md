# 📄 `resources/README.md`

# Resources — Curated React Knowledge Base

This folder contains curated reference material that supports the main curriculum:

> React Development: Foundations to Internals

These documents are not lessons.

They are **compressed knowledge references** designed to reinforce mental models across all modules.

---

# Purpose

The resources serve three goals:

## 1. Reinforcement

They reinforce core ideas from the curriculum:

- React rendering model
- Fiber architecture
- reconciliation behavior
- performance model
- concurrency and server rendering

---

## 2. Mental Model Compression

Each file distills complex React concepts into:

- conceptual primitives
- execution flow diagrams
- simplified mental models

No implementation details are required to understand them.

---

## 3. Revision & Interview Preparation

These documents can be used for:

- quick revision
- interview preparation
- system design discussions
- debugging mental models

---

# Structure

```text
resources/
├── 01-react-fundamentals.md
├── 02-jsx-and-elements.md
├── 03-fiber-architecture.md
├── 04-reconciliation.md
├── 05-performance-model.md
└── 06-concurrency-and-rsc.md
````

---

# How to Use These Resources

## Recommended Workflow

### 1. After each module

Re-read the corresponding resource file:

| Module   | Resource               |
| -------- | ---------------------- |
| Module 1 | 01-react-fundamentals  |
| Module 2 | 02-jsx-and-elements    |
| Module 3 | 03-fiber-architecture  |
| Module 3 | 04-reconciliation      |
| Module 4 | 05-performance-model   |
| Module 4 | 06-concurrency-and-rsc |

---

### 2. Before building projects

Scan relevant files to recall:

* rendering pipeline
* reconciliation rules
* performance principles

---

### 3. Before interviews

Focus on:

* Fiber model
* reconciliation heuristics
* concurrency model
* render vs commit distinction

---

# Design Philosophy

These notes follow a strict principle:

> Reduce React to its execution model, not its API surface.

We avoid:

* framework-specific APIs
* library-specific patterns
* implementation details that change over time

We focus on:

* stable architecture concepts
* execution behavior
* mental models that persist across React versions

---

# Key Insight

If you understand these six documents deeply, you understand:

* how React renders
* how React schedules work
* how React updates the UI
* how React optimizes performance
* how React scales to large applications

---

# End of Resources

```
```
