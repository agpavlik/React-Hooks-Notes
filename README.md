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

## 🔥 useCallback <a name="1"></a>

---

## 🔥 useContext <a name="2"></a>

---

## 🔥 useDebugValue <a name="3"></a>

---

## 🔥 useDeferredValue <a name="4"></a>

---

## 🔥 useEffect <a name="5"></a>

`useEffect` is a hook provided by React that allows you to perform side effects in functional components. Side effects are operations that happen outside of the usual React component lifecycle, like fetching data from an API, modifying the DOM, setting up subscriptions, or scheduling timers. These operations are crucial for building dynamic and interactive applications.

useEffect is a function that takes two arguments:

1. `Effect function`: This is the first argument, and it's a function that contains the code for your side effect. It's executed after the component has rendered. This function can return a cleanup function, which will be called when the component unmounts or when dependencies change.

2. `Dependency array`: The second argument is an array that contains variables or values that the effect depends on. When any of these dependencies change, the effect function will be re-executed. If you pass an empty array ([]), the effect runs once when the component mounts and doesn't depend on any specific variable.

Key Concepts and Use Cases:

- `Fetching Data`: You can use useEffect to fetch data from an API when the component mounts or when certain dependencies change.

```javascript
// This example uses the fetch function to make an API request:
import React, { useState, useEffect } from "react";

function DataFetchingExample() {
  const [data, setData] = useState([]);

  useEffect(() => {
    async function fetchData() {
      try {
        const response = await fetch("https://api.example.com/data");
        const result = await response.json();
        setData(result);
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    }
    fetchData();
  }, []); // Empty dependency array ensures it runs once on mount

  return (
    <div>
      <ul>
        {data.map((item) => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

- `Updating the DOM`: You can use it to modify the DOM, like adding or removing classes, handling scroll events, or focusing on input fields.

```javascript
//In this example, we add a CSS class when a button is clicked:
import React, { useState, useEffect } from "react";

function DOMUpdateExample() {
  const [isActive, setIsActive] = useState(false);

  useEffect(() => {
    if (isActive) {
      document.getElementById("myElement").classList.add("active");
    } else {
      document.getElementById("myElement").classList.remove("active");
    }
  }, [isActive]);

  return (
    <div>
      <div id="myElement">This element can be styled</div>
      <button onClick={() => setIsActive(!isActive)}>Toggle Style</button>
    </div>
  );
}
```

- `Subscriptions`: If your component needs to listen to external events or manage subscriptions, useEffect is the right place to set up and clean up these connections.

```javascript
//In this example, we set up a subscription to a WebSocket:
import React, { useEffect } from "react";

function WebSocketExample() {
  useEffect(() => {
    const socket = new WebSocket("wss://example.com/socket");

    socket.onmessage = (event) => {
      console.log("Received message:", event.data);
      // Handle the incoming data
    };

    return () => {
      // Cleanup: Close the WebSocket when the component unmounts
      socket.close();
    };
  }, []);

  return <div>WebSocket subscription</div>;
}
```

- `Timers and Intervals`: You can use useEffect to set up timers or intervals for tasks like periodic data updates or animations, and clear them when the component unmounts.

```javascript
//In this example, we create a timer to update a time display every second:
import React, { useState, useEffect } from "react";

function TimerExample() {
  const [time, setTime] = useState(new Date());

  useEffect(() => {
    const timerID = setInterval(() => {
      setTime(new Date());
    }, 1000);

    return () => {
      // Cleanup: Clear the interval when the component unmounts
      clearInterval(timerID);
    };
  }, []);

  return <div>Current time: {time.toLocaleTimeString()}</div>;
}
```

Common Pitfalls:

- Not including dependencies when necessary, leading to unintended side effects.
- Forgetting to return a cleanup function, causing memory leaks or unintended behavior.
- Overusing useEffect, leading to performance issues.

## 🔥 useFetcher <a name="6"></a>

---

## 🔥 useId <a name="7"></a>

---

## 🔥 useImperativeHandle <a name="8"></a>

---

## 🔥 useImperativeMethods <a name="9"></a>

---

## 🔥 useInsertionEffect <a name="10"></a>

---

## 🔥 useLayoutEffect <a name="11"></a>

---

## 🔥 useMemo <a name="12"></a>

---

## 🔥 useMutationEffect <a name="13"></a>

---

## 🔥 useReducer <a name="14"></a>

---

## 🔥 useRef <a name="15"></a>

---

## 🔥 useState <a name="16"></a>

---

## 🔥 useSyncExternalStore <a name="17"></a>

---

## 🔥 useTransition <a name="18"></a>

---
