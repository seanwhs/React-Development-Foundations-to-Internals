# Slides Outline — Styling React Components

---

# Slide 1 — Title

# Styling React Components

## From Inline Styles to CSS Modules

**Building Maintainable User Interfaces**

Presenter: **[Your Name]**

---

# Slide 2 — The Core Question

## How Do We Style React Components?

React itself **does not provide CSS**.

Its responsibility is to describe **what the UI should look like**, not how styles are written.

Instead, React integrates naturally with CSS.

### React Philosophy

* UI is described using components.
* CSS controls presentation.
* Choose a styling approach that improves maintainability.
* Prevent style leakage between components.

### Three Common Approaches

* Inline Styles
* External CSS
* CSS Modules

---

# Slide 3 — Our Case Study

## Three Components

All three produce exactly the same visual output.

```
<App>

 ├── GoogleComponent
 ├── AppleComponent
 └── AmazonComponent

```

Each component renders a company card.

| Component       | Styling Method |
| --------------- | -------------- |
| GoogleComponent | Inline Styles  |
| AppleComponent  | External CSS   |
| AmazonComponent | CSS Modules    |

### App.jsx

```jsx
import GoogleComponent from "./components/GoogleComponent";
import AppleComponent from "./components/AppleComponent";
import AmazonComponent from "./components/AmazonComponent";

export default function App() {
  return (
    <div>
      <GoogleComponent />
      <AppleComponent />
      <AmazonComponent />
    </div>
  );
}
```

---

# Slide 4 — React Styling Fundamentals

## JSX Is JavaScript

React doesn't use HTML directly.

Instead, JSX compiles into JavaScript.

### HTML

```html
<div class="card" style="color:red">
```

### JSX

```jsx
<div
  className="card"
  style={{ color: "red" }}
>
```

Notice:

* class → className
* style becomes a JavaScript object
* CSS properties use camelCase

```
font-size
↓

fontSize
```

---

# Slide 5 — React Rendering Pipeline

```
React Component

       ↓

      JSX

       ↓

React.createElement()

       ↓

Virtual DOM

       ↓

Reconciliation

       ↓

Browser DOM

       ↓

CSS Rendering
```

Regardless of styling technique...

CSS is still responsible for painting pixels on the screen.

---

# Slide 6 — Method 1: Inline Styles

## JavaScript Objects

```jsx
export default function GoogleComponent() {

  const styles = {
    border: "2px solid #4285F4",
    padding: "20px",
    borderRadius: "8px",
    backgroundColor: "#f9f9f9",
    fontFamily: "Arial"
  };

  return (
    <div style={styles}>
      <h2>Google</h2>
      <p>Search Engine</p>
    </div>
  );
}
```

### Rules

* JavaScript object
* camelCase properties
* values are strings or numbers

---

# Slide 7 — Inline Styles

## Advantages

✔ Easy to generate dynamically

```jsx
const buttonStyle = {
  backgroundColor:
    isDark ? "black" : "white"
};
```

✔ Uses props easily

```jsx
<div style={{
  color: props.color
}}>
```

✔ Perfect for runtime styling

---

## Limitations

❌ No hover

```css
:hover
```

❌ No media queries

```css
@media(...)
```

❌ Can clutter component logic

---

# Slide 8 — Method 2: External CSS

## Traditional CSS

### AppleComponent.jsx

```jsx
import "./AppleComponent.css";

export default function AppleComponent() {
  return (
    <div className="card">
      <h2>Apple</h2>
      <p>Technology Company</p>
    </div>
  );
}
```

### AppleComponent.css

```css
.card {
    border: 2px solid black;
    padding: 20px;
    border-radius: 8px;
    background: white;
}
```

---

# Slide 9 — External CSS

## Advantages

✔ Familiar syntax

✔ Works with every CSS feature

```css
.card:hover{
    background:#eee;
}
```

```css
@media(max-width:768px){
    .card{
        width:100%;
    }
}
```

---

## Disadvantage

Global namespace.

```
.card
```

may exist in

```
AppleComponent.css
```

and

```
GoogleComponent.css
```

Result:

Unexpected style overrides.

---

# Slide 10 — The Global CSS Problem

Suppose two developers write

```css
.card{
    background:red;
}
```

and

```css
.card{
    background:blue;
}
```

Which wins?

The answer depends on

* import order
* CSS specificity
* cascade rules

Large applications become difficult to maintain.

---

# Slide 11 — Method 3: CSS Modules

## Component-Level CSS

### AmazonComponent.jsx

```jsx
import styles from "./AmazonComponent.module.css";

export default function AmazonComponent() {
  return (
    <div className={styles.card}>
      <h2>Amazon</h2>
      <p>E-Commerce Platform</p>
    </div>
  );
}
```

---

### AmazonComponent.module.css

```css
.card{
    border:2px solid orange;
    padding:20px;
    border-radius:8px;
}
```

---

# Slide 12 — What Happens During Build?

You write

```css
.card
```

Webpack/Vite transforms it into

```css
.card__3xa91
```

or

```css
.card_fY8mP
```

Your JSX

```jsx
styles.card
```

becomes

```html
<div class="card__3xa91">
```

Every component receives its own unique class names.

No collisions.

---

# Slide 13 — CSS Modules Benefits

## Advantages

✔ Component encapsulation

✔ No naming conflicts

✔ Supports

* hover
* animations
* media queries
* keyframes

Example

```css
.card:hover{
    transform:scale(1.05);
}
```

Works exactly like normal CSS.

---

# Slide 14 — Comparing All Three

| Feature        | Inline   | CSS  | CSS Modules |
| -------------- | -------- | ---- | ----------- |
| Dynamic values | ✅        | ❌    | ❌           |
| Hover          | ❌        | ✅    | ✅           |
| Media Queries  | ❌        | ✅    | ✅           |
| Global Scope   | No       | Yes  | No          |
| Encapsulation  | Yes      | No   | Yes         |
| Easy Theming   | Moderate | Good | Excellent   |

---

# Slide 15 — Choosing the Right Tool

## Recommended Strategy

```
Application

├── Global CSS
│      Reset
│      Variables
│      Typography
│
├── CSS Modules
│      Components
│      Layout
│      Cards
│      Buttons
│
└── Inline Styles
       Dynamic Colors
       Runtime Sizes
       Calculated Values
```

Each technique solves a different problem.

---

# Slide 16 — Real World Example

Imagine a dashboard.

Global CSS

```css
body{
    font-family:Inter;
}
```

CSS Modules

```css
DashboardCard.module.css
```

Inline Styles

```jsx
<div
  style={{
    width: progress + "%"
  }}
>
```

The three approaches work together.

---

# Slide 17 — Best Practices

✔ Prefer CSS Modules for reusable components.

✔ Keep global CSS minimal.

✔ Use inline styles only when values are computed.

Avoid

```jsx
style={{
    padding:"20px",
    margin:"20px",
    border:"1px solid black",
    ...
}}
```

Large style objects reduce readability.

---

# Slide 18 — Summary

| Technique     | Scope     | Best For       |
| ------------- | --------- | -------------- |
| Inline Styles | Local     | Dynamic values |
| External CSS  | Global    | Themes, resets |
| CSS Modules   | Component | Reusable UI    |

### Rule of Thumb

* Dynamic → Inline
* Global → CSS
* Components → CSS Modules

---

# Slide 19 — Key Takeaways

React does **not replace CSS**.

Instead, React offers multiple ways to connect components with styling.

The best React applications combine:

* Global CSS
* CSS Modules
* Inline styles

Each where it makes architectural sense.

---

# Slide 20 — Questions?

# Thank You

Questions?

Discussion

**Styling React Components**
