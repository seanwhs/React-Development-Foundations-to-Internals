# 📄 `resources/04-reconciliation.md`

# React Reconciliation

This document explains how React determines what changed between renders.

---

## 1. What is Reconciliation?

Reconciliation is React’s process of comparing two UI trees:

- Previous render tree
- Next render tree

React computes the minimal set of changes needed to update the UI.

---

## 2. Why Reconciliation Exists

Updating the DOM directly is expensive.

React avoids full DOM replacement by:

- comparing virtual representations
- updating only what changed

---

## 3. Tree Comparison Model

React compares element trees like this:

Previous Tree  
→ App  
→ Header  
→ List  

Next Tree  
→ App  
→ Header  
→ List (updated items)

React determines differences efficiently instead of rebuilding everything.

---

## 4. Keys and Identity

Keys help React identify stable elements across renders.

Example:

```jsx id="k2v9dp"
items.map(item => (
  <li key={item.id}>{item.name}</li>
))
````

Without keys, React cannot reliably track identity.

---

## 5. Reconciliation Rules

React uses heuristics:

* Different component type → replace entire subtree
* Same type → update props only
* Stable key → preserve component instance

---

## 6. Important Insight

Reconciliation is NOT full tree diffing.

It is an optimized heuristic designed for performance.

React trades perfect diffing for speed.
```
