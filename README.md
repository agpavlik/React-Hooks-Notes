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

`useRef` is a React hook that provides a simple and effective way to reference and manipulate DOM elements, as well as to store values that persist across renders, reducing the need for additional state management.. It is primarily used to access and interact with DOM elements directly, but its value-persisting feature also makes it a powerful tool for a wide range of use cases.

- Creation: To create a useRef object, you can use the useRef function. It doesn't require an initial value like useState does.

- DOM Interaction: You can attach a ref attribute to a React element in your JSX to gain access to the DOM element it represents. Once created, a ref object can be assigned to the ref attribute, allowing you to manipulate the DOM element directly.

- Value Persistence: useRef can be used to persist values across renders. Changes to the current property of a useRef object do not trigger re-renders, making it suitable for storing mutable data that doesn't impact the rendering of the component.

Key Concepts and Use Cases:

- `DOM Manipulation`: useRef is frequently used to reference and manipulate DOM elements, such as focusing an input field, measuring elements, or triggering animations.

```javascript
//In this example, we use useRef to create a reference to the input element and focus on it when the component mounts.
import React, { useRef, useEffect } from "react";

function DOMManipulationExample() {
  const inputRef = useRef(null);

  useEffect(() => {
    inputRef.current.focus(); // Focus the input element on component mount
  }, []);

  return <input ref={inputRef} type="text" />;
}
```

- `Preserving Values`: You can use useRef to store values that should persist across renders, such as previous state or props, and to keep track of values that shouldn't trigger re-renders.

```javascript
//In this example, renderCount is a useRef object used to store the render count without causing re-renders, even when the state changes.

import React, { useRef, useState } from "react";

function ValuePersistenceExample() {
  const renderCount = useRef(0);
  const [state, setState] = useState(0);

  renderCount.current++; // Increment the render count without causing re-render

  return (
    <div>
      <p>Render count: {renderCount.current}</p>
      <button onClick={() => setState(state + 1)}>Increment State</button>
    </div>
  );
}
```

- `Accessing Child Components`: When you need to access child components' methods or properties, useRef can be used to hold references to those components.

```javascript
//In this example, we use useRef to create a reference to a child component and call its doSomething method from the parent component.
import React, { useRef } from "react";

function ChildComponent() {
  const childRef = useRef();

  const handleClick = () => {
    childRef.current.doSomething();
  };

  return (
    <div>
      <button onClick={handleClick}>Call Child Method</button>
    </div>
  );
}

function ParentComponent() {
  return (
    <div>
      <ChildComponent ref={childRef} />
    </div>
  );
}
```

- `Synchronizing with External Libraries`: useRef is ideal for interfacing with third-party libraries that require direct access to DOM elements.

```javascript
//In this example, we use useRef to create a reference to a container element and initialize an external library when the component mounts.
import React, { useRef, useEffect } from "react";

function ExternalLibraryIntegrationExample() {
  const containerRef = useRef();

  useEffect(() => {
    // Initialize an external library that requires a DOM element
    externalLibrary.init(containerRef.current);
  }, []);

  return <div ref={containerRef}>Container for External Library</div>;
}
```

Common Pitfalls:

- Attempting to mutate the current property of a useRef object directly in a render function, which can lead to unexpected behavior.

- Misusing useRef for managing all state when useState is more appropriate for value changes that trigger re-renders.

---

## 🔥 useState <a name="16"></a>

`useState` is a React hook that allows functional components to declare and manage state. It enables you to introduce dynamic behavior into your components by defining variables that can hold and update data. These state variables are the building blocks of interactive and responsive interfaces. `useState` is a cornerstone of React development because it empowers functional components to manage dynamic data and reactivity. It simplifies state management and reduces the complexity of your code by handling updates and re-renders automatically. This makes your components more predictable and easier to maintain.

- Creation: To create a state variable, you call the useState function with an initial state value. The function returns an array with two elements: the current state value and a function to update that state.

- State Variable: The first element in the array is the current state value, which you can read and display in your component.

- State Updater: The second element is a function that allows you to modify the state, triggering re-renders of the component with the updated state.

Key Concepts and Use Cases:

- `Managing Component State`: Use useState to declare and manage component-specific state, such as form input values, visibility toggles, or any data that should change over time.

```javascript
// In this example, useState is used to create and manage the count state variable, which is updated when the "Increment" button is clicked.
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  const increment = () => setCount(count + 1);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}
```

- `User Input Handling`: Capture and respond to user interactions like clicks, keyboard input, and form submissions by updating the state accordingly.

```javascript
// In this example, useState is used to create and manage the text state variable, which updates as the user types in the input field.
import React, { useState } from "react";

function TextInput() {
  const [text, setText] = useState("");

  const handleChange = (e) => setText(e.target.value);

  return (
    <div>
      <input type="text" value={text} onChange={handleChange} />
      <p>You typed: {text}</p>
    </div>
  );
}
```

- `Dynamic Rendering`: Conditionally render different parts of your component based on the state, creating interactive and responsive user interfaces.

```javascript
//In this example, useState is used to control the visibility of content based on the showContent state variable.
import React, { useState } from "react";In this example, the useState hook initializes the state with the provided initial value, and the "Update State" button allows you to change the state dynamically.

function ToggleContent() {
  const [showContent, setShowContent] = useState(false);

  const toggle = () => setShowContent(!showContent);

  return (
    <div>
      <button onClick={toggle}>Toggle Content</button>
      {showContent && <p>This content is dynamically displayed.</p>}
    </div>
  );
}
```

- `State Initialization`: Set the initial state with useState by passing in the desired initial value. This value is used when the component first mounts.

```javascript
//In this example, the useState hook initializes the state with the provided initial value, and the "Update State" button allows you to change the state dynamically.
import React, { useState } from "react";

function InitialStateExample() {
  const [initialState, setInitialState] = useState(
    "This is the initial state."
  );

  return (
    <div>
      <p>Current State: {initialState}</p>
      <button onClick={() => setInitialState("Updated state")}>
        Update State
      </button>
    </div>
  );
}
```

- `Todo List`: Create a simple todo list where you can add and remove items.

```javascript
//In this example, useState is used to manage the todo list and the input field's value.
import React, { useState } from "react";

function TodoList() {
  const [todos, setTodos] = useState([]);
  const [todoInput, setTodoInput] = useState("");

  const addTodo = () => {
    if (todoInput) {
      setTodos([...todos, todoInput]);
      setTodoInput("");
    }
  };
  const removeTodo = (index) => {
    const newTodos = todos.filter((_, i) => i !== index);
    setTodos(newTodos);
  };

  return (
    <div>
      <input
        type="text"
        value={todoInput}
        onChange={(e) => setTodoInput(e.target.value)}
      />
      <button onClick={addTodo}>Add Todo</button>
      <ul>
        {todos.map((todo, index) => (
          <li key={index}>
            {todo}
            <button onClick={() => removeTodo(index)}>Remove</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

- `Toggle Theme`: Allow users to toggle between light and dark themes.

```javascript
//
import React, { useState } from "react";

function ThemeToggle() {
  const [isDarkTheme, setIsDarkTheme] = useState(false);

  const toggleTheme = () => {
    setIsDarkTheme(!isDarkTheme);
  };

  const themeClass = isDarkTheme ? "dark" : "light";

  return (
    <div className={themeClass}>
      <button onClick={toggleTheme}>Toggle Theme</button>
      <p>Current Theme: {isDarkTheme ? "Dark" : "Light"}</p>
    </div>
  );
}
```

- `Shopping Cart`: Create a simple shopping cart that tracks items and their quantities.

```javascript
import React, { useState } from 'react';

function ShoppingCart() {
  const [cart, setCart] = useState([]);
  const [item, setItem] = useState('');
  const [quantity, setQuantity] = useState(1);

  const addItemToCart = () => {
    if (item && quantity > 0) {
      const newItem = { item, quantity };
      setCart([...cart, newItem]);
      setItem('');
      setQuantity(1);
    }
  }return (
    <div>
      <input
        type="text"
        placeholder="Item"
        value={item}
        onChange={(e) => setItem(e.target.value)}
      />
      <input
        type="number"
        placeholder="Quantity"
        value={quantity}
        onChange={(e) => setQuantity(Number(e.target.value))}
      />
      <button onClick={addItemToCart}>Add to Cart</button>
      <ul>
      {cart.map((item, index) => (
          <li key={index}>
            {item.quantity} x {item.item}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

- `Counter with Minimum and Maximum Values`: Create a counter that increments and decrements within a specified range.

```javascript
//In this example, useState is used to create a counter that can increment and decrement within the range of 0 to 10.
import React, { useState } from "react";

function CounterWithRange() {
  const [count, setCount] = useState(0);

  const increment = () => {
    if (count < 10) {
      setCount(count + 1);
    }
  };

  const decrement = () => {
    if (count > 0) {
      setCount(count - 1);
    }
  };
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={decrement}>Decrement</button>
      <button onClick={increment}>Increment</button>
    </div>
  );
}
```

- `User Authentication`: Manage user authentication state, such as tracking whether a user is logged in or not.

```javascript
import React, { useState } from "react";

function AuthExample() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  const login = () => {
    setIsLoggedIn(true);
  };

  const logout = () => {
    setIsLoggedIn(false);
  };
  return (
    <div>
      {isLoggedIn ? (
        <div>
          <p>Welcome, User!</p>
          <button onClick={logout}>Logout</button>
        </div>
      ) : (
        <div>
          <p>Please log in to access the content.</p>
          <button onClick={login}>Login</button>
        </div>
      )}
    </div>
  );
}
```

- `Conditional Rendering`: Toggle the display of different components based on user interaction.

```javascript
//In this example, useState is used to toggle the display of a message based on user interaction.
import React, { useState } from "react";

function ConditionalRenderExample() {
  const [showMessage, setShowMessage] = useState(false);

  const toggleMessage = () => {
    setShowMessage(!showMessage);
  };

  return (
    <div>
      <button onClick={toggleMessage}>Toggle Message</button>
      {showMessage && <p>This message is conditionally displayed.</p>}
    </div>
  );
}
```

Common Pitfalls:

- Not providing a proper initial state value, which can lead to unexpected behavior.

- Mutating state directly, as state updates should be done through the state updater function provided by useState.

- Overusing state, leading to complex and difficult-to-maintain components.

---

## 🔥 useSyncExternalStore <a name="17"></a>

---

## 🔥 useTransition <a name="18"></a>

---
