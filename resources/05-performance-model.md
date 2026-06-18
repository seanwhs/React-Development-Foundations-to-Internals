# 📄 `resources/05-performance-model.md`

# React Performance Model

This document explains how React performance works at a conceptual level.

---

## 1. What Performance Means in React

Performance in React is not about avoiding renders.

It is about:

- minimizing unnecessary work
- reducing expensive DOM operations
- keeping the UI responsive

---

## 2. Render vs Commit

React has two distinct phases:

### Render Phase (Pure Computation)

- Executes component functions
- Creates React Elements
- Builds the next Fiber tree
- Can be paused or restarted

### Commit Phase (Side Effects)

- Applies DOM updates
- Runs layout effects
- Updates the browser

---

## 3. Why Re-renders Are Normal

React re-renders components frequently because:

- state changes
- props change
- parent components update

This is expected behavior, not a performance bug.

---

## 4. Cost of Rendering

Rendering itself is cheap because:

- it happens in memory
- it does not touch the DOM
- it is just function execution

The real cost is:

- DOM updates
- layout recalculation
- painting

---

## 5. Memoization Tools

React provides tools to reduce unnecessary work:

### React.memo
Prevents re-render if props are unchanged.

### useMemo
Caches expensive computed values.

### useCallback
Caches function references.

---

## 6. When to Use Memoization

Use memoization when:

- renders are frequent
- computation is expensive
- props are stable across renders

Do NOT use it by default.

---

## 7. Key Insight

Performance is not about stopping renders.

It is about controlling and reducing unnecessary work across the render pipeline.
