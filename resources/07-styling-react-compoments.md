# Styling React Components: From Inline Styles to CSS Modules

## Introduction

One of the first questions developers ask after learning how to create React components is:

> **"How do I style them?"**

Unlike HTML, where styling usually means linking a CSS file and assigning CSS classes, React provides several different ways to style components. At first, this flexibility can seem confusing. Why are there multiple approaches? Why can some components use JavaScript objects while others use traditional CSS files? What are CSS Modules, and why do modern React projects prefer them?

The answer lies in React's design philosophy.

React is **not a CSS framework**.

Instead, React is a JavaScript library whose primary responsibility is **describing and rendering user interfaces**. It does not attempt to replace CSS or invent a completely new styling language. Rather, it embraces existing web standards while allowing styling to integrate naturally with JavaScript and component-based development.

As React applications grow from a handful of components to hundreds or even thousands, styling becomes more than simply choosing colors and fonts. It becomes an architectural concern. Good styling practices improve maintainability, encourage component reuse, and reduce unintended interactions between unrelated parts of the application.

In this article, we'll explore the three most common styling approaches built into modern React development:

* **Inline Styles**
* **External CSS Stylesheets**
* **CSS Modules**

Each technique offers different trade-offs. Understanding when and why to use each one will help you write cleaner, more maintainable React applications.

---

# Our Example Application

Throughout this article, we'll work with a simple application that renders three components. Although the application is intentionally small, each component demonstrates a different styling technique.

```jsx
// App.jsx

import GoogleComponent from "./components/GoogleComponent";
import AppleComponent from "./components/AppleComponent";
import AmazonComponent from "./components/AmazonComponent";

const App = () => {
  return (
    <div>
      <GoogleComponent />
      <AppleComponent />
      <AmazonComponent />
    </div>
  );
};

export default App;
```

When rendered, the application displays three company cards:

```
Google
Organizing the world's information.

Apple
Think Different.

Amazon
Work Hard ◆ Have Fun ◆ Make History
```

Although the visual result appears similar, each component is styled using an entirely different technique.

---

# Styling in React Is Different

Before diving into each approach, it is worth understanding **why styling looks different in React.**

Traditional web development separates responsibilities into three technologies:

* HTML describes the structure.
* CSS controls presentation.
* JavaScript provides behavior.

A typical HTML page looks like this:

```html
<h1 class="title">Hello World</h1>
```

```css
.title {
    color: blue;
    font-size: 2rem;
}
```

The browser loads the HTML, downloads the stylesheet, and matches CSS selectors to HTML elements.

React still follows this model—but with one significant difference.

Instead of writing HTML directly, we write **JSX**, which is ultimately JavaScript.

```jsx
<h1>Hello World</h1>
```

looks like HTML, but it is actually JavaScript syntax that Babel compiles into calls to `React.createElement()`.

This means every attribute inside JSX must also obey JavaScript rules.

That design decision explains several differences you encounter immediately:

```jsx
className
```

instead of

```html
class
```

and

```jsx
style={{ color: "red" }}
```

instead of

```html
style="color:red"
```

Understanding this distinction makes React's styling APIs much easier to understand.

---

# React Doesn't Replace CSS

A common misconception among beginners is that React introduces a new styling language.

It does not.

The browser is still responsible for rendering HTML and applying CSS.

React simply generates the HTML.

The rendering pipeline looks something like this:

```
React Component
        │
        ▼
      JSX
        │
        ▼
React.createElement()
        │
        ▼
 React Elements
        │
        ▼
 Virtual DOM
        │
        ▼
    Browser DOM
        │
        ▼
 CSS Rendering Engine
```

Notice something important.

React stops once it updates the DOM.

From that point onward, the browser's rendering engine handles layout, typography, spacing, colors, animations, and every other visual aspect of the page.

This separation of responsibilities is one of React's greatest strengths. React manages **what should appear**, while CSS determines **how it should appear**.

---

# Three Ways to Style Components

Our sample project demonstrates three increasingly scalable styling approaches.

| Component | Styling Technique |
| --------- | ----------------- |
| Google    | Inline Styles     |
| Apple     | External CSS      |
| Amazon    | CSS Modules       |

Each solves a different problem.

We'll begin with the simplest approach.

---

# Method 1 — Inline Styles

The Google component uses inline styles.

```jsx
const GoogleComponent = () => {

    const containerStyle = {
        color: '#4285F4',
        fontFamily: 'Arial, sans-serif',
        textAlign: 'center',
        margin: '20px',
    };

    return (
        <div style={containerStyle}>

            <h1>Google</h1>

            <p
                style={{
                    color: '#34A853',
                    fontSize: '1.2rem',
                }}
            >
                Organizing the world's information.
            </p>

        </div>
    );
};

export default GoogleComponent;
```

At first glance, this might look unusual.

Instead of writing CSS inside a stylesheet, we define a JavaScript object named `containerStyle`.

```jsx
const containerStyle = {
    color: '#4285F4',
    fontFamily: 'Arial, sans-serif',
    textAlign: 'center',
    margin: '20px',
};
```

The object is then passed directly to the `style` prop.

```jsx
<div style={containerStyle}>
```

Unlike HTML, where the `style` attribute accepts a string, React expects a JavaScript object.

For example, in HTML you might write:

```html
<div style="color: blue; margin: 20px;">
```

In React, the equivalent code becomes:

```jsx
<div
    style={{
        color: "blue",
        margin: "20px",
    }}
>
```

Because the value is a JavaScript object, each CSS property becomes an object property.

---

# Why Does React Use camelCase?

One of the first differences developers notice is that CSS property names change.

Traditional CSS uses kebab-case:

```css
font-size
background-color
margin-top
border-radius
```

JavaScript object properties cannot contain hyphens unless they are quoted, so React adopts camelCase instead.

| CSS Property     | React Property  |
| ---------------- | --------------- |
| font-size        | fontSize        |
| background-color | backgroundColor |
| text-align       | textAlign       |
| border-radius    | borderRadius    |
| margin-top       | marginTop       |
| font-weight      | fontWeight      |

Once you recognize that the `style` prop is simply a JavaScript object, this naming convention becomes completely natural.

---

# How React Applies Inline Styles

When React renders the component, it converts the JavaScript object into the equivalent DOM style properties.

Conceptually, React performs something similar to:

```javascript
element.style.color = "#4285F4";
element.style.fontFamily = "Arial, sans-serif";
element.style.margin = "20px";
```

This happens automatically during rendering.

The browser ultimately sees ordinary inline styles attached to the generated HTML element.

In other words, React is not inventing a new styling system—it is simply providing a JavaScript-friendly way of describing existing CSS.

---

# Dynamic Styling with Inline Styles

One of the greatest advantages of inline styles is that they are **ordinary JavaScript objects**.

Because they are JavaScript, they can be computed dynamically.

Instead of hard-coding colors or spacing, we can derive them from:

* Component state
* Component props
* User preferences
* API responses
* Screen size
* Theme settings

This is where inline styles become much more powerful than plain HTML.

Consider a simple example.

```jsx
const Button = ({ primary }) => {

    return (

        <button
            style={{
                backgroundColor: primary ? "#1976d2" : "#cccccc",
                color: "white",
                padding: "12px 20px",
                border: "none",
                borderRadius: "8px"
            }}
        >
            Click Me
        </button>

    );

};
```

Notice that the button color depends entirely on the value of `primary`.

If

```jsx
<Button primary={true} />
```

the button appears blue.

If

```jsx
<Button primary={false} />
```

it appears gray.

No CSS classes need to be switched manually.

---

# Styling Based on State

React applications are driven by **state**.

Since JSX is re-evaluated every time state changes, inline styles automatically update as well.

Consider a simple example.

```jsx
import { useState } from "react";

const LightSwitch = () => {

    const [isOn, setIsOn] = useState(false);

    return (

        <div>

            <button
                onClick={() => setIsOn(!isOn)}
            >
                Toggle
            </button>

            <h2
                style={{
                    color: isOn ? "green" : "red"
                }}
            >
                {isOn ? "ON" : "OFF"}
            </h2>

        </div>

    );

};

export default LightSwitch;
```

Every time the button is clicked,

React

↓

re-renders the component

↓

creates new React Elements

↓

updates the DOM

↓

updates the inline styles.

Notice that we never manually manipulate the DOM.

We never write

```javascript
element.style.color = "green";
```

React computes the correct style from the current state.

This is another example of React's declarative philosophy.

Instead of telling React **how** to update the page, we simply describe **what** the page should look like for a given state.

---

# Building Style Objects

Because styles are JavaScript objects, we can compose them like any other object.

```jsx
const baseStyle = {

    padding: "20px",

    borderRadius: "10px",

};

const warningStyle = {

    backgroundColor: "orange",

    color: "white",

};

const errorStyle = {

    backgroundColor: "red",

    color: "white",

};
```

We can combine them using the spread operator.

```jsx
<div
    style={{
        ...baseStyle,
        ...warningStyle,
    }}
>
```

React treats the final object exactly the same as if it had been written by hand.

This makes it possible to build reusable styling objects while keeping components concise.

---

# Using Variables Inside Styles

Since styles are JavaScript, variables work naturally.

```jsx
const primaryColor = "#4285F4";

const cardWidth = 400;

return (

    <div

        style={{

            color: primaryColor,

            width: cardWidth,

        }}

    >

        Hello

    </div>

);
```

Notice that numeric values such as

```jsx
width: 400
```

are automatically interpreted as pixels.

React renders

```css
width: 400px;
```

Some properties, however, still require explicit units.

```jsx
fontSize: "2rem"

margin: "20px"

padding: "1em"
```

Understanding which CSS properties accept unitless values is important when working extensively with inline styles.

---

# Common Beginner Mistakes

Because JSX resembles HTML, beginners often write HTML-style syntax.

For example,

```jsx
<div style="color:red">
```

This is incorrect.

React expects a JavaScript object.

The correct version is

```jsx
<div

    style={{

        color: "red"

    }}

>
```

Notice the double braces.

```jsx
style={{

}}
```

Many beginners ask why there are **two pairs of braces**.

The answer is simple.

The outer braces

```jsx
{}
```

tell JSX:

> "The following value is JavaScript."

The inner braces

```jsx
{}
```

create the JavaScript object itself.

Therefore

```jsx
style={{ color: "red" }}
```

means

> Evaluate this JavaScript object and assign it to the `style` property.

---

Another common mistake is using CSS syntax.

Incorrect

```jsx
style={{

    font-size: "20px"

}}
```

Correct

```jsx
style={{

    fontSize: "20px"

}}
```

Remember:

React uses JavaScript property names—not CSS property names.

---

# Advantages of Inline Styles

Inline styles have several strengths.

## 1. Everything Lives Inside the Component

Component logic and styling remain together.

There is no need to switch between multiple files.

Small components become highly self-contained.

---

## 2. Dynamic Values Are Simple

Conditional rendering becomes trivial.

```jsx
style={{

    opacity: loading ? 0.5 : 1

}}
```

No CSS classes.

No DOM manipulation.

No additional libraries.

---

## 3. No Class Name Collisions

Since there are no CSS classes,

there can be no duplicate class names.

Every component owns its styling object.

---

## 4. JavaScript Features Are Available

Because styles are objects,

we can use

* variables
* functions
* loops
* conditions
* spread operators
* object composition

This level of flexibility is impossible with plain CSS alone.

---

# Limitations of Inline Styles

Despite their convenience, inline styles have significant drawbacks.

These limitations become more apparent as applications grow.

---

## No Pseudo Classes

Traditional CSS allows selectors such as

```css
button:hover

button:focus

button:active
```

Inline styles cannot express these selectors.

There is no equivalent syntax for

```css
:hover
```

inside a JavaScript object.

---

## No Media Queries

Responsive design relies heavily on media queries.

```css
@media (max-width: 768px) {

    .container {

        width: 100%;

    }

}
```

Inline styles cannot define media queries.

Developers must compute responsive values manually using JavaScript or use another styling solution.

---

## No Animations

CSS supports powerful animations.

```css
transition

animation

@keyframes
```

Inline styles do not provide a natural way to define keyframes.

Animations become significantly harder to manage.

---

## Repetition

Imagine fifty buttons.

Each one repeats

```jsx
style={{

    padding: ...

    borderRadius: ...

    backgroundColor: ...

}}
```

The duplication quickly becomes difficult to maintain.

CSS classes solve this much more elegantly.

---

## Readability

Large inline style objects reduce readability.

Consider the following.

```jsx
return (

<div

style={{

...

twenty properties

...

}}

>

...
```

Component logic becomes buried inside styling definitions.

Developers often spend more time scrolling than understanding the component itself.

---

# When Should You Use Inline Styles?

Inline styles are best suited for situations where styling depends directly on component logic.

Examples include:

* Dynamic colors
* Dynamic widths
* Calculated positions
* Theme switching
* Temporary prototypes
* Rapid experimentation

For example,

```jsx
<div

style={{

    width: `${progress}%`

}}

>
```

Here, the width comes directly from application state.

Writing a CSS class for every possible percentage would be impractical.

Inline styles are the correct solution.

---

# When Should You Avoid Inline Styles?

Inline styles become less suitable when you need:

* hover effects
* responsive layouts
* animations
* reusable designs
* consistent typography
* shared component themes

These are precisely the problems CSS was designed to solve.

As projects become larger, developers naturally migrate toward external stylesheets or CSS Modules.

---

# Transitioning to External CSS

Our Google component demonstrated that React can style components entirely through JavaScript.

While this approach is elegant for small, dynamic pieces of UI, most real-world applications still rely heavily on CSS.

The browser's styling engine is extremely powerful, offering capabilities that inline styles cannot easily replicate. Features such as responsive layouts, pseudo-classes, transitions, animations, and complex selectors are all native strengths of CSS.

This leads us to the second styling technique in React: **External CSS Stylesheets**.

In the next section, we'll examine the **Apple component**, which uses a traditional CSS file. We'll learn how React integrates with standard CSS, why JSX uses `className` instead of `class`, how styles are loaded into the application, and why global styles can become problematic as projects grow.

---

# External CSS in React (The Apple Component)

While inline styles keep everything inside JavaScript, real-world applications quickly need something more scalable and expressive.

This is where **external CSS stylesheets** come in.

Our Apple component demonstrates the most traditional approach to styling in React: separating structure and presentation.

```jsx id="apple1"
// components/AppleComponent.jsx

import './AppleComponent.css'; // Import external stylesheet

const AppleComponent = () => {

  return (
    <div className="apple-container">
      <h1>Apple</h1>
      <p className="apple-paragraph">
        Think Different.
      </p>
    </div>
  );
};

export default AppleComponent;
```

And the corresponding CSS file:

```css id="applecss1"
/* components/AppleComponent.css */

.apple-container {
    color: #A2AAAD; /* Apple Grey */
    font-family: 'San Francisco', Arial, sans-serif;
    text-align: center;
    margin: 20px;
}

.apple-paragraph {
    color: #333;
    font-size: 1.2rem;
}
```

At first glance, this feels like “normal web development”—and that is exactly the point.

React does not remove CSS.

It embraces it.

---

# How External CSS Works in React

When you import a CSS file in a React component:

```jsx id="importcss"
import './AppleComponent.css';
```

you are not importing styles as JavaScript objects.

Instead, you are instructing the build tool (Vite, Webpack, etc.) to:

1. Load the CSS file
2. Inject it into the document `<head>`
3. Make its rules available globally

Conceptually, the browser ends up with:

```html id="styleinject"
<style>
.apple-container { ... }
.apple-paragraph { ... }
</style>
```

This means CSS behaves exactly as it does in traditional websites.

There is no transformation of class names, no scoping, and no encapsulation.

---

# Why React Uses className Instead of class

In HTML:

```html id="htmlclass"
<div class="container"></div>
```

In React:

```jsx id="jsxclass"
<div className="container"></div>
```

This is because **class is a reserved keyword in JavaScript**.

React uses `className` as a safe JavaScript property name, which ultimately maps to the DOM's `class` attribute.

So this:

```jsx id="map1"
<div className="apple-container">
```

becomes this in the browser:

```html id="map2"
<div class="apple-container">
```

Nothing special happens at runtime—React simply translates JSX into DOM elements.

---

# The Mental Model: CSS is Still Global

The most important concept to understand is this:

> External CSS in React is still global CSS.

This means any stylesheet can affect any element in the application if class names match.

For example:

```css id="global1"
.container {
    background: red;
}
```

If another component also uses:

```jsx id="globaljsx"
<div className="container">
```

then both components are affected.

React does not prevent this.

The browser does not prevent this.

This is why large applications eventually face **CSS collision problems**.

---

# The CSS Cascade Still Applies

Because React uses real CSS, all standard CSS rules apply:

### 1. Specificity

```css id="spec1"
div {
    color: blue;
}

.apple-container {
    color: red;
}
```

The more specific selector wins.

---

### 2. Order of Imports

If two rules have equal specificity, the later one wins.

```css id="order1"
/* loaded first */
.container {
    color: blue;
}
```

```css id="order2"
/* loaded second */
.container {
    color: red;
}
```

Final result: red.

---

### 3. Inheritance

Some properties like `color` and `font-family` are inherited automatically.

```css id="inherit1"
.apple-container {
    color: #A2AAAD;
}
```

All child elements inherit that color unless overridden.

---

# Advantages of External CSS

Despite its simplicity, external CSS remains extremely powerful.

---

## 1. Full CSS Feature Set

Unlike inline styles, external CSS supports everything:

* pseudo-classes (`:hover`, `:focus`)
* pseudo-elements (`::before`, `::after`)
* media queries
* keyframe animations
* complex selectors

Example:

```css id="hover1"
.apple-container:hover {
    transform: scale(1.02);
    transition: all 0.3s ease;
}
```

This is impossible with inline styles alone.

---

## 2. Separation of Concerns

External CSS restores a clean separation:

| Layer      | Responsibility |
| ---------- | -------------- |
| JSX        | Structure      |
| CSS        | Styling        |
| JavaScript | Logic          |

This makes code easier to reason about for teams coming from traditional web development.

---

## 3. Easy to Share and Reuse

A CSS file can be reused across multiple components.

```css id="reuse1"
.button {
    padding: 10px 20px;
    border-radius: 8px;
}
```

Any component can adopt this style simply by using:

```jsx id="reusejsx"
className="button"
```

---

## 4. Familiar to All Developers

External CSS requires no learning curve.

Every web developer already understands:

* selectors
* cascade
* specificity
* inheritance

This makes onboarding easier for teams transitioning to React.

---

# Disadvantages of External CSS

However, this simplicity comes at a cost.

---

## 1. Global Scope Problems

The biggest issue is **style collisions**.

In large applications:

* class names repeat
* styles override each other
* debugging becomes difficult

Example:

```css id="collision1"
.card {
    padding: 20px;
}
```

Used in multiple unrelated components, it becomes unpredictable.

---

## 2. Hard to Trace Dependencies

If a component looks like this:

```jsx id="trace1"
<div className="header">
```

You must search across the entire project to find:

* where `.header` is defined
* whether it is overridden
* whether it is reused elsewhere

This slows down debugging.

---

## 3. No Component Encapsulation

React encourages component-based architecture.

But global CSS breaks encapsulation.

A component should ideally be:

> self-contained, predictable, and reusable

External CSS does not guarantee this.

---

## 4. Scaling Issues in Large Apps

As applications grow:

* CSS files increase
* naming conventions become strict (BEM, etc.)
* maintainability decreases

This is why modern React applications often move toward CSS Modules or CSS-in-JS solutions.

---

# When External CSS Is a Good Choice

External CSS is still widely used when:

* building small to medium applications
* working with design systems
* integrating with existing CSS codebases
* using global themes or resets
* collaborating with designers who write CSS directly

It is also ideal when your project benefits from a **shared global design language**.

---

# Transition to CSS Modules

The Apple component shows both the strength and weakness of external CSS.

It is simple, powerful, and familiar—but it lacks isolation.

This leads to a key question in modern React development:

> How do we keep the simplicity of CSS while avoiding global conflicts?

The answer is **CSS Modules**.

CSS Modules introduce local scoping, allowing styles to behave like JavaScript modules—encapsulated, reusable, and collision-free.

In the next section, we will explore the Amazon component and understand how CSS Modules solve the scaling problems of traditional CSS while preserving its full expressive power.

---

# CSS Modules in React (The Amazon Component)

As React applications grow, one problem becomes increasingly difficult to ignore:

> **Global CSS is unpredictable at scale.**

External stylesheets are simple, but they introduce a hidden risk—any class name can affect any component.

CSS Modules solve this problem by introducing **local scope for CSS**.

Our Amazon component demonstrates this approach.

```jsx id="amazon1"
// components/AmazonComponent.jsx

import styles from './AmazonComponent.module.css';

const AmazonComponent = () => {

  return (
    <div className={styles.container}>

      <h1 className={styles.header}>
        Amazon
      </h1>

      <p className={styles.paragraph}>
        Work Hard 💠 Have Fun 💠 Make History
      </p>

    </div>
  );
};

export default AmazonComponent;
```

And the CSS file:

```css id="amazoncss1"
/* components/AmazonComponent.module.css */

.container {
    color: #FF9900; /* Amazon Orange */
    font-family: Verdana, sans-serif;
    text-align: center;
    margin: 20px;
}

.header {
    font-size: 3rem;
    font-weight: bold;
}

.paragraph {
    color: #146EB4; /* Amazon Blue */
    font-size: 1.2rem;
}
```

At first glance, this looks identical to external CSS.

But the import tells a different story:

```jsx id="importmodule"
import styles from './AmazonComponent.module.css';
```

This single `.module.css` suffix changes everything.

---

# What Makes CSS Modules Different?

When you use CSS Modules, your CSS is no longer global.

Instead, it becomes **locally scoped to the component**.

React (via the build tool) transforms your CSS classes into unique identifiers.

For example:

```css id="beforetransform"
.container {
    color: orange;
}
```

might become:

```css id="aftertransform"
.container__3x9k1 {
    color: orange;
}
```

And your JavaScript import becomes:

```javascript id="jsmapping"
styles.container // => "container__3x9k1"
```

So this:

```jsx id="jsxmodule"
<div className={styles.container}>
```

becomes:

```html id="finalhtml"
<div class="container__3x9k1">
```

This ensures that every class name is unique across the entire application.

---

# Why CSS Modules Exist

CSS Modules were created to solve a very specific problem:

> **How do we keep CSS simple while preventing global collisions?**

React applications can easily grow into:

* hundreds of components
* thousands of class names
* multiple teams working in parallel

In such environments, global CSS becomes dangerous.

For example:

```css id="globalproblem"
.button {
    background: red;
}
```

If another developer writes:

```css id="globalproblem2"
.button {
    background: blue;
}
```

the final result depends on:

* import order
* build order
* browser cascade rules

This unpredictability is exactly what CSS Modules eliminate.

---

# Mental Model: CSS Modules as JavaScript Objects

CSS Modules behave like JavaScript objects.

Think of this CSS:

```css id="modulecss"
.container {
    color: #FF9900;
}

.header {
    font-size: 3rem;
}
```

as being transformed into:

```javascript id="modulejs"
{
  container: "container__3x9k1",
  header: "header__8jd92"
}
```

So when you write:

```jsx id="accessstyle"
styles.container
```

you are effectively accessing a **mapped class name**.

This is why CSS Modules feel natural in React—they align perfectly with JavaScript's module system.

---

# Advantages of CSS Modules

CSS Modules combine the best of both worlds:

* full CSS feature set
* no global scope pollution
* predictable styling behavior

---

## 1. True Style Encapsulation

Each component owns its styles.

```jsx id="encap1"
<div className={styles.container}>
```

No other component can accidentally override it.

This makes components:

* reusable
* portable
* predictable

---

## 2. No Naming Conflicts

You can safely reuse class names everywhere:

```css id="safe1"
/* AmazonComponent.module.css */
.container { }
```

```css id="safe2"
/* AppleComponent.module.css */
.container { }
```

Both are completely independent.

No collisions. No overrides. No surprises.

---

## 3. Scales Naturally in Large Applications

CSS Modules work well in:

* enterprise apps
* multi-team codebases
* design systems
* component libraries

Because styles are tied to components, not global files.

---

## 4. Works with Existing CSS Knowledge

You still write:

* normal CSS
* selectors
* media queries
* animations

Nothing new to learn in CSS syntax.

Only the import mechanism changes.

---

## 5. Encourages Component Thinking

CSS Modules reinforce React’s mental model:

> A component should be self-contained.

Each component owns:

* structure (JSX)
* behavior (JavaScript)
* styling (CSS Module)

This improves modularity and maintainability.

---

# Limitations of CSS Modules

Despite their advantages, CSS Modules are not perfect.

---

## 1. Still Requires CSS Files

You still maintain separate `.css` files:

```
AmazonComponent.jsx
AmazonComponent.module.css
```

This can feel verbose compared to inline styles or CSS-in-JS.

---

## 2. No Runtime Styling Logic

Unlike inline styles, CSS Modules do not easily support:

```jsx id="dynamic1"
style={{ width: progress + "%" }}
```

Dynamic values require either:

* inline styles
* CSS variables
* helper libraries

---

## 3. Slight Learning Curve

Developers must understand:

* module imports
* object mapping
* build-time transformation

While simple, it is still an abstraction on top of CSS.

---

# Combining CSS Modules with Dynamic Styles

CSS Modules do not replace inline styles—they complement them.

A common pattern is hybrid styling:

```jsx id="hybrid1"
import styles from './Button.module.css';

const Button = ({ active }) => {

  return (
    <button
      className={styles.button}
      style={{
        opacity: active ? 1 : 0.5
      }}
    >
      Click Me
    </button>
  );
};
```

Here:

* CSS Modules handle structure and layout
* Inline styles handle dynamic values

This is a very common real-world pattern.

---

# When to Use CSS Modules

CSS Modules are ideal when:

* building medium to large React applications
* working in teams
* requiring style isolation
* creating reusable components
* avoiding global CSS conflicts

They are especially useful for:

* design systems
* component libraries
* SaaS dashboards
* enterprise applications

---

# Comparing All Three Approaches

Now that we have explored all three techniques, we can compare them more clearly.

| Feature          | Inline Styles | External CSS   | CSS Modules |
| ---------------- | ------------- | -------------- | ----------- |
| Scope            | Local         | Global         | Local       |
| Dynamic values   | Excellent     | Poor           | Medium      |
| Pseudo selectors | No            | Yes            | Yes         |
| Media queries    | No            | Yes            | Yes         |
| Reusability      | Low           | Medium         | High        |
| Collision safety | Yes           | No             | Yes         |
| Maintainability  | Medium        | Low (at scale) | High        |
| Learning curve   | Low           | Low            | Medium      |

---

# Final Mental Model

A modern React application typically uses all three approaches together:

* **Inline styles** → dynamic, state-driven styling
* **External CSS** → global themes, resets, legacy support
* **CSS Modules** → component-level styling (most common in production)

The key insight is not choosing one approach exclusively.

Instead, you choose based on **responsibility boundaries**.

---

# Key Takeaway

Styling in React is not about selecting a single method.

It is about understanding **where styling belongs in your architecture**.

* If styling depends on **state**, use inline styles.
* If styling is **global or shared**, use external CSS.
* If styling belongs to a **component**, use CSS Modules.

React gives you flexibility, but also responsibility.

Good React developers don’t just write components—they design styling systems.

---

# Next Step

Now that we understand how React handles styling at the component level, the next question becomes more interesting:

> How do large-scale React applications manage consistent design across hundreds of components?

This leads us into advanced styling systems like:

* Tailwind CSS
* Styled Components
* Emotion
* Design systems and tokens
* Theming architecture in React

That is where styling evolves from "CSS management" into **UI engineering**.

