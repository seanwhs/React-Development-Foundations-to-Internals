# 📄 `resources/02-jsx-and-elements.md`

# JSX and React Elements

This document explains what JSX actually is and how React represents UI internally.

---

## 1. What JSX Really Is

JSX is not HTML.

It is syntactic sugar for:

React.createElement()

Example:

```jsx
const el = <h1>Hello</h1>;
````

This becomes:

```js
React.createElement("h1", null, "Hello");
```

---

## 2. React Elements

A React Element is a plain JavaScript object describing UI.

Example:

```js
{
  type: "h1",
  props: {
    children: "Hello"
  }
}
```

Key point:

> This is NOT a DOM node.

---

## 3. JSX is NOT HTML

JSX differs from HTML:

* Uses `className` instead of `class`
* Uses camelCase for attributes (`onClick`)
* Supports JavaScript expressions inside `{}`

Example:

```jsx
const name = "Sean";

return <h1>Hello {name}</h1>;
```

---

## 4. Element Tree

React builds a tree of elements:

App
→ div
→ h1
→ span

This tree is a lightweight JavaScript structure.

---

## 5. Key Insight

React Elements are immutable descriptions of UI.

They are not updated — they are recreated on every render.

```
