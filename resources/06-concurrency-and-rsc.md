# 📄 `resources/06-concurrency-and-rsc.md`

# Concurrent React & Server Components

This document explains React concurrency and server-side rendering concepts.

---

## 1. Concurrent Rendering

Concurrent rendering allows React to:

- pause rendering work
- resume rendering later
- prioritize urgent updates
- discard low-priority work if needed

This improves UI responsiveness under load.

---

## 2. Why Concurrency Exists

Without concurrency:

- heavy renders block input
- UI becomes unresponsive
- long tasks freeze the browser

With concurrency:

- user input stays responsive
- rendering is scheduled intelligently

---

## 3. Transitions

Transitions mark updates as non-urgent.

Example:

```js id="q8v2kz"
startTransition(() => {
    setState(value);
});
````

This tells React:

> This update can wait.

---

## 4. useTransition

`useTransition` exposes pending state:

```js id="v2k9mn"
const [isPending, startTransition] = useTransition();
```

You can show loading states during low-priority updates.

---

## 5. useDeferredValue

`useDeferredValue` delays non-urgent rendering work.

```js id="m7v3qp"
const deferredValue = useDeferredValue(value);
```

This keeps urgent UI responsive while deferring expensive updates.

---

## 6. React Server Components (RSC)

Server Components:

* run only on the server
* are never sent to the browser as JavaScript
* reduce client bundle size
* perform heavy work outside the UI thread

---

## 7. Client vs Server Components

### Server Components

* data fetching
* heavy computation
* no interactivity

### Client Components

* state
* events
* interactivity

---

## 8. Suspense

Suspense allows React to pause rendering until dependencies are ready.

```jsx id="s8v2qz"
<Suspense fallback={<Loading />}>
    <Component />
</Suspense>
```

It enables streaming UI updates.

---

## 9. Key Insight

React is no longer just a UI library.

It is a distributed rendering system that spans:

* client
* server
* streaming boundaries
* scheduling layers

```
