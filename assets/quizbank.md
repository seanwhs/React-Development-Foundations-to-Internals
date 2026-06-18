# 📄 `assets/quizbank.md`

# Quiz Bank — React Development: Foundations to Internals

This quiz bank is designed to test **mental models**, not memorization.

Each question evaluates whether the learner understands React as a system.

---

# Module 1 — Foundations

---

## Q1. What is React’s core principle?

A. DOM manipulation library  
B. UI = f(State)  
C. CSS-in-JS system  
D. Event handling framework  

**Answer:** B

---

## Q2. What is a React component?

A. A class only  
B. A DOM node  
C. A function that returns UI  
D. A state container  

**Answer:** C

---

## Q3. What triggers a re-render?

A. Variable mutation  
B. State update  
C. CSS change  
D. DOM event only  

**Answer:** B

---

## Q4. React updates UI by:

A. Direct DOM mutation  
B. Recomputing UI description  
C. Replacing entire page  
D. Using jQuery internally  

**Answer:** B

---

# Module 2 — State & Hooks

---

## Q5. What does useState do?

A. Mutates variables  
B. Stores persistent state and triggers re-render  
C. Updates DOM  
D. Caches API responses  

**Answer:** B

---

## Q6. Why do components re-run on state change?

A. React reloads the page  
B. Component function is re-executed  
C. Browser refreshes automatically  
D. Hooks invalidate DOM  

**Answer:** B

---

## Q7. What is a side effect?

A. Pure function output  
B. Anything outside rendering  
C. JSX transformation  
D. Props update  

**Answer:** B

---

## Q8. useEffect runs:

A. During render phase  
B. After commit phase  
C. Before component definition  
D. Only once in app lifetime  

**Answer:** B

---

# Module 3 — Internals

---

## Q9. What is Fiber?

A. CSS engine  
B. React’s execution unit  
C. DOM node  
D. State manager  

**Answer:** B

---

## Q10. Render phase is:

A. DOM mutation phase  
B. Pure computation phase  
C. Network request phase  
D. Event handling phase  

**Answer:** B

---

## Q11. Commit phase is:

A. Interruptible  
B. Pure computation  
C. DOM update phase  
D. State initialization  

**Answer:** C

---

## Q12. Reconciliation is:

A. Full tree diff  
B. Heuristic comparison of UI trees  
C. CSS calculation  
D. DOM repaint system  

**Answer:** B

---

## Q13. Why are keys important?

A. Styling  
B. Performance only  
C. Identity tracking in lists  
D. DOM selection  

**Answer:** C

---

# Module 4 — Performance

---

## Q14. React performance is primarily about:

A. Avoiding renders  
B. Reducing unnecessary work  
C. Removing JSX  
D. Using fewer components  

**Answer:** B

---

## Q15. React.memo does what?

A. Prevents DOM updates  
B. Skips re-render if props are equal  
C. Freezes state  
D. Caches API calls  

**Answer:** B

---

## Q16. useMemo is used for:

A. Storing DOM nodes  
B. Caching computed values  
C. Creating components  
D. Managing lifecycle  

**Answer:** B

---

## Q17. Concurrent rendering allows:

A. Multi-threaded UI rendering  
B. Interruptible rendering work  
C. Direct DOM manipulation  
D. CSS optimization  

**Answer:** B

---

## Q18. Server Components:

A. Run in browser  
B. Run only on server  
C. Replace React entirely  
D. Manage state only  

**Answer:** B

---

# Advanced Concept Questions

---

## Q19. What is the relationship between Fiber and Hooks?

A. Unrelated systems  
B. Hooks are stored in Fiber nodes  
C. Hooks replace Fiber  
D. Fiber runs inside Hooks  

**Answer:** B

---

## Q20. Why is React rendering interruptible?

A. Browser limitation  
B. Fiber scheduler design  
C. JavaScript is async  
D. JSX requirement  

**Answer:** B

---

# Scenario Questions

---

## Q21. A component re-renders unexpectedly. Most likely reason?

A. CSS update  
B. State or props change  
C. Browser bug  
D. JSX syntax  

**Answer:** B

---

## Q22. A heavy list causes lag. Best first step?

A. Add React.memo everywhere  
B. Measure performance  
C. Rewrite React  
D. Disable state  

**Answer:** B

---

## Q23. useEffect runs infinitely. Likely cause?

A. Missing dependency array logic  
B. Fiber bug  
C. JSX error  
D. CSS loop  

**Answer:** A

---

# Final Principle

> The goal is not to memorize React APIs.

> The goal is to understand React’s execution model.

--

# End of Quiz Bank
```
