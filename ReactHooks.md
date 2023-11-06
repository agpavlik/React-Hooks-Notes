# React Hooks Notes

This guide contains explanations and examples of 18 React Hooks.

- [useCallback](#1)
- [useContext](#2)
- [useDebugValue](#3)
- [useDeferredValue](#4)
- [useEffect](#5) ❗
- [useFetcher](#6)
- [useId](#7)
- [useImperativeHandle](#8)
- [useImperativeMethods](#9)
- [useInsertionEffect](#10)
- [useLayoutEffect](#11)
- [useMemo](#12)
- [useMutationEffect](#13)
- [useReducer](#14) ❗
- [useRef](#15) ❗
- [useState](#16) ❗
- [useSyncExternalStore](#17)
- [useTransition](#18)

---

## 🔥 useCallback

---

## 🔥 useContext

---

## 🔥 useDebugValue

---

## 🔥 useDeferredValue

---

## 🔥 useEffect

`useEffect` is a hook provided by React that allows you to perform side effects in functional components. Side effects are operations that happen outside of the usual React component lifecycle, like fetching data from an API, modifying the DOM, setting up subscriptions, or scheduling timers. These operations are crucial for building dynamic and interactive applications.

useEffect is a function that takes two arguments:

1. `Effect function`: This is the first argument, and it's a function that contains the code for your side effect. It's executed after the component has rendered. This function can return a cleanup function, which will be called when the component unmounts or when dependencies change.

2. `Dependency array`: The second argument is an array that contains variables or values that the effect depends on. When any of these dependencies change, the effect function will be re-executed. If you pass an empty array ([]), the effect runs once when the component mounts and doesn't depend on any specific variable.

Key Concepts and Use Cases:

- Fetching Data: You can use useEffect to fetch data from an API when the component mounts or when certain dependencies change.

```javascript

```

- Updating the DOM: You can use it to modify the DOM, like adding or removing classes, handling scroll events, or focusing on input fields.
- Subscriptions: If your component needs to listen to external events or manage subscriptions, useEffect is the right place to set up and clean up these connections.
- Timers and Intervals: You can use useEffect to set up timers or intervals for tasks like periodic data updates or animations, and clear them when the component unmounts.

Common Pitfalls:

- Not including dependencies when necessary, leading to unintended side effects.
- Forgetting to return a cleanup function, causing memory leaks or unintended behavior.
- Overusing useEffect, leading to performance issues.

## 🔥 useFetcher

---

## 🔥 useId

---

## 🔥 useImperativeHandle

---

## 🔥 useImperativeMethods

---

## 🔥 useInsertionEffect

---

## 🔥 useLayoutEffect

---

## 🔥 useMemo

---

## 🔥 useMutationEffect

---

## 🔥 useReducer

---

## 🔥 useRef

---

## 🔥 useState

---

## 🔥 useSyncExternalStore

---

## 🔥 useTransition

---
