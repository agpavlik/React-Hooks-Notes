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

`useContext` allows to access and consume context data within a React component. To fully understand useContext, it's essential to grasp the concept of context in React and how it can be used to efficiently pass data down the component tree. `Context` in React is a mechanism that provides a way to share data, like themes, authentication, or other application-wide settings, between components without explicitly passing the data through every level of the component tree. It's particularly useful for managing state and making data accessible to multiple components that are not directly connected in the component hierarchy.

#### Anatomy of useContext:

useContext is a hook that's used to access the context within a functional component. It takes a context object as an argument and returns the current context value for that context. Here's how it works:

- `Creating a Context`: To use useContext, you first need to create a context using the createContext function. This function takes an initial value as an argument and returns a context object.

```javascript
const MyContext = React.createContext(initialValue);
```

- `Providing Context`: The context data is typically provided at a higher level in the component tree using the Context.Provider component. It sets the value that can be accessed by any child component that subscribes to this context.

```javascript
<MyContext.Provider value={contextValue}>
  {/_ Your component hierarchy _/}
</MyContext.Provider>
```

- `Consuming Context`: Now, you can use the useContext hook in any functional component that is a descendant of the Context.Provider. It allows you to access the context data.

```javascript
const contextData = useContext(MyContext);
```

#### Key Concepts:

- `Provider-Consumer Relationship`: useContext relies on the provider-consumer relationship. The provider sets the context value, and the consumer (the component that uses useContext) reads that value.
- `Avoid Prop Drilling`: useContext is helpful in eliminating prop drilling, which is the process of passing data down the component tree via props. It simplifies the code and makes it more maintainable.
- `Default Value`: When a component accesses a context with useContext, it will receive the value from the nearest Context.Provider ancestor in the component tree. If there's no provider, it will use the default value provided when the context was created.

#### Use Cases:

- `Theme Switching`: You can use context to manage the theme of your application. Components that need access to the current theme can consume the theme context to style themselves accordingly.

```javascript
// First, create a theme context and a provider:
// ThemeContext.js
import React, { createContext, useState } from "react";

export const ThemeContext = createContext();

export const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState("light");

  const toggleTheme = () => {
    setTheme(theme === "light" ? "dark" : "light");
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

---
// Now, you can use the useContext hook to access the theme in any component:
// App.js
import React, { useContext } from 'react';
import { ThemeContext, ThemeProvider } from './ThemeContext';

const App = () => {
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <div className={theme}>
      <button onClick={toggleTheme}>Toggle Theme</button>
    </div>
  );
};

export default App;

```

- `User Authentication`: Storing user authentication status and user information in context makes it accessible to various parts of the application, like headers, sidebars, and protected routes.

```javascript

```

- `Localization`: You can use context to provide language and translation data to components that need to display content in different languages.

```javascript

```

#### Common Pitfalls:

- Overusing Context: It's important to use context judiciously. Don't store data that is not truly global in context, as it can lead to unnecessary re-renders and complexity.
- Performance Concerns: Using context for deeply nested components can lead to performance issues, as changes in context can trigger re-renders for many components. Consider optimizing by using memoization or breaking down the context into smaller, more specialized contexts.
- Testing Challenges: Testing components that rely heavily on context can be challenging. You may need to provide mock contexts for your tests to isolate and control the context data.

---

## 🔥 useDebugValue <a name="3"></a>

---

## 🔥 useDeferredValue <a name="4"></a>

---

## 🔥 useEffect <a name="5"></a>

`useEffect` is a hook provided by React that allows you to perform side effects in functional components. Side effects are operations that happen outside of the usual React component lifecycle, like fetching data from an API, modifying the DOM, setting up subscriptions, or scheduling timers. These operations are crucial for building dynamic and interactive applications.

#### Anatomy of useEffect:

It is a function that takes two arguments:

1. `Effect function`: This is the first argument, and it's a function that contains the code for your side effect. It's executed after the component has rendered. This function can return a cleanup function, which will be called when the component unmounts or when dependencies change.

2. `Dependency array`: The second argument is an array that contains variables or values that the effect depends on. When any of these dependencies change, the effect function will be re-executed. If you pass an empty array ([]), the effect runs once when the component mounts and doesn't depend on any specific variable.

#### Use Cases:

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

#### Common Pitfalls:

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

`useReducer` is a powerful and versatile hook in React that provides a more controlled way to manage state in your components compared to useState. It's especially useful when dealing with complex state management or when you need to perform state updates based on the previous state.

#### Anatomy of useReducer:

At its core, useReducer is a function that helps you manage state in your React components. It takes two arguments: a `reducer function` and an `initial state`. The reducer function is responsible for specifying how the state should change in response to various actions, and the initial state sets the starting point for your state management.

- `Reducer Function`: The reducer function is the heart of useReducer. It takes two arguments: the current state and an action. This function's purpose is to determine the new state based on the current state and the action. It returns the updated state. The action is typically an object that describes what kind of state change you want to make.

  ```javascript
  const reducer = (state, action) => {
    switch (action.type) {
      case "INCREMENT":
        return { count: state.count + 1 };
      case "DECREMENT":
        return { count: state.count - 1 };
      default:
        return state;
    }
  };
  ```

- `Initial State`: You provide an initial state when you call useReducer. This initial state sets the starting value for your state. It can be a simple value, an object, or any data structure that suits your needs.
  ```javascript
  const initialState = { count: 0 };
  ```
- `Dispatch`: useReducer returns an array with two elements: the current state and a dispatch function. The dispatch function is used to send actions to the reducer, triggering state updates. It works like a messenger that communicates your intentions to the reducer.
  ```javascript
  const [state, dispatch] = useReducer(reducer, initialState);
  ```
- `Action`: Actions are plain JavaScript objects that have a type property (a string) and any additional data needed for the state update. When you call dispatch with an action, the reducer function is invoked with the current state and the action. It's up to the reducer to decide how to update the state based on the action type.
  ```javascript
  // Dispatch an action to increment the count
  dispatch({ type: "INCREMENT" });
  // Dispatch an action to decrement the count
  dispatch({ type: "DECREMENT" });
  ```
- `Updated State`: After dispatching an action, the reducer processes it, computes the new state, and returns it. React then updates the component with this new state. You can access the current state using the state variable.

  ```javascript
  console.log(state.count); // Access the current count
  ```

#### Benefits of useReducer:

- Predictable State Updates: With useReducer, state updates are predictable and follow a clear pattern. The reducer function specifies how the state should change in response to actions.
- Complex State Management: It's excellent for managing complex state logic, such as forms, lists, or any state that depends on the previous state.
- Testing: Reducer functions are pure functions, making them easy to test, which is crucial for writing robust and maintainable code.
- Readability: Separating the logic for state updates into a single reducer function can improve the readability of your components, especially as they grow in complexity.

#### Use cases:

- `Counter Example`: In this simple example, we'll create a counter using useReducer. We have two actions, "INCREMENT" and "DECREMENT," to update the count.

```javascript
import React, { useReducer } from "react";

const initialState = { count: 0 };

const reducer = (state, action) => {
  switch (action.type) {
    case "INCREMENT":
      return { count: state.count + 1 };
    case "DECREMENT":
      return { count: state.count - 1 };
    default:
      return state;
  }
};
function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: "INCREMENT" })}>Increment</button>
      <button onClick={() => dispatch({ type: "DECREMENT" })}>Decrement</button>
    </div>
  );
}

export default Counter;
```

- `Todo List Example`: In this example, we'll implement a simple todo list using useReducer. We have actions for adding and removing tasks.

```javascript
import React, { useReducer, useState } from "react";

const initialState = { todos: [] };

const reducer = (state, action) => {
  switch (action.type) {
    case "ADD_TODO":
      return { todos: [...state.todos, action.payload] };
    case "REMOVE_TODO":
      return { todos: state.todos.filter((todo) => todo !== action.payload) };
    default:
      return state;
  }
};
function TodoList() {
  const [task, setTask] = useState("");
  const [state, dispatch] = useReducer(reducer, initialState);

  const addTodo = () => {
    dispatch({ type: "ADD_TODO", payload: task });
    setTask("");
  };

  return (
    <div>
      <input
        type="text"
        value={task}
        onChange={(e) => setTask(e.target.value)}
      />
      <button onClick={addTodo}>Add Task</button>
      <ul>
        {state.todos.map((todo, index) => (
          <li key={index}>
            {todo}{" "}
            <button
              onClick={() => dispatch({ type: "REMOVE_TODO", payload: todo })}
            >
              Remove
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default TodoList;
```

- `Theme Toggle Example`: In this example, we'll create a theme toggle using useReducer to switch between light and dark themes.

```javascript
import React, { useReducer } from "react";

const themes = {
  LIGHT: "light",
  DARK: "dark",
};

const initialState = { theme: themes.LIGHT };

const reducer = (state, action) => {
  switch (action.type) {
    case "TOGGLE_THEME":
      return {
        theme: state.theme === themes.LIGHT ? themes.DARK : themes.LIGHT,
      };
    default:
      return state;
  }
};

function ThemeToggle() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div className={`App ${state.theme}`}>
      <button onClick={() => dispatch({ type: "TOGGLE_THEME" })}>
        Toggle Theme
      </button>
    </div>
  );
}

export default ThemeToggle;
```

- `Shopping Cart Example`: In this example, we'll create a shopping cart using useReducer. We'll have actions for adding and removing items from the cart and calculating the total price.

```javascript
import React, { useReducer } from "react";

const initialState = {
  cart: [],
  totalPrice: 0,
};

const reducer = (state, action) => {
  switch (action.type) {
    case "ADD_TO_CART":
      return {
        cart: [...state.cart, action.payload],
        totalPrice: state.totalPrice + action.payload.price,
      };
    case "REMOVE_FROM_CART":
      const updatedCart = state.cart.filter(
        (item) => item.id !== action.payload.id
      );
      return {
        cart: updatedCart,
        totalPrice: state.totalPrice - action.payload.price,
      };
    default:
      return state;
  }
};
function ShoppingCart() {
  const [state, dispatch] = useReducer(reducer, initialState);

  const products = [
    { id: 1, name: "Product A", price: 10 },
    { id: 2, name: "Product B", price: 15 },
    { id: 3, name: "Product C", price: 20 },
  ];

  return (
    <div>
      <h1>Shopping Cart</h1>
      <ul>
        {" "}
        {products.map((product) => (
          <li key={product.id}>
            {product.name} - ${product.price}
            <button
              onClick={() =>
                dispatch({ type: "ADD_TO_CART", payload: product })
              }
            >
              Add to Cart
            </button>
          </li>
        ))}
      </ul>
      <h2>Cart</h2>
      <ul>
        {state.cart.map((item) => (
          <li key={item.id}>
            {item.name} - ${item.price}
            <button
              onClick={() =>
                dispatch({ type: "REMOVE_FROM_CART", payload: item })
              }
            >
              Remove from Cart
            </button>
          </li>
        ))}{" "}
      </ul>
      <p>Total Price: ${state.totalPrice}</p>
    </div>
  );
}

export default ShoppingCart;
```

- `User Authentication Example`: In this example, we'll use useReducer to manage user authentication state, including login and logout actions.

```javascript
import React, { useReducer } from "react";

const initialState = {
  isAuthenticated: false,
  user: null,
};

const reducer = (state, action) => {
  switch (action.type) {
    case "LOGIN":
      return {
        isAuthenticated: true,
        user: action.payload,
      };
    case "LOGOUT":
      return initialState;
    default:
      return state;
  }
};

function UserAuthentication() {
  const [state, dispatch] = useReducer(reducer, initialState);

  const login = (user) => {
    dispatch({ type: "LOGIN", payload: user });
  };

  const logout = () => {
    dispatch({ type: "LOGOUT" });
  };
  return (
    <div>
      {state.isAuthenticated ? (
        <div>
          <p>Welcome, {state.user}!</p>
          <button onClick={logout}>Logout</button>
        </div>
      ) : (
        <div>
          <p>Please log in</p>
          <button onClick={() => login("exampleUser")}>Login</button>
        </div>
      )}
    </div>
  );
}

export default UserAuthentication;
```

- `Accordion Component`: In this example, we'll create an accordion component that allows you to expand and collapse sections. useReducer will help us manage the state of each section.

```javascript
import React, { useReducer } from "react";

const initialState = { sections: [] };

const reducer = (state, action) => {
  switch (action.type) {
    case "TOGGLE_SECTION":
      const updatedSections = state.sections.map((section, index) => {
        if (index === action.payload) {
          return { ...section, isOpen: !section.isOpen };
        }
        return section;
      });
      return { sections: updatedSections };
    case "ADD_SECTION":
      return {
        sections: [...state.sections, { title: action.payload, isOpen: false }],
      };
    default:
      return state;
  }
};

function Accordion() {
  const [state, dispatch] = useReducer(reducer, initialState);

  const addSection = (title) => {
    dispatch({ type: "ADD_SECTION", payload: title });
  };
  return (
    <div>
      <button onClick={() => addSection("Section 1")}>Add Section</button>
      {state.sections.map((section, index) => (
        <div key={index}>
          <button
            onClick={() => dispatch({ type: "TOGGLE_SECTION", payload: index })}
          >
            {section.title}
          </button>
          {section.isOpen && <p>Section Content for {section.title}</p>}
        </div>
      ))}
    </div>
  );
}

export default Accordion;
```

- `Chat Application`: In this example, we'll create a simple chat application where you can add messages to different chat rooms using useReducer

```javascript
import React, { useReducer, useState } from "react";

const initialState = { rooms: {} };

const reducer = (state, action) => {
  switch (action.type) {
    case "ADD_MESSAGE":
      const { roomId, message } = action.payload;
      const updatedRooms = { ...state.rooms };
      updatedRooms[roomId] = [...(updatedRooms[roomId] || []), message];
      return { rooms: updatedRooms };
    default:
      return state;
  }
};
function ChatApp() {
  const [state, dispatch] = useReducer(reducer, initialState);
  const [message, setMessage] = useState("");
  const [selectedRoom, setSelectedRoom] = useState("general");

  const addMessage = () => {
    dispatch({
      type: "ADD_MESSAGE",
      payload: { roomId: selectedRoom, message },
    });
    setMessage("");
  };
  return (
    <div>
      <div>
        <select
          onChange={(e) => setSelectedRoom(e.target.value)}
          value={selectedRoom}
        >
          <option value="general">General</option>
          <option value="random">Random</option>
        </select>
        <input
          type="text"
          value={message}
          onChange={(e) => setMessage(e.target.value)}
        />
        <button onClick={addMessage}>Send</button>
      </div>
      <div>
        {state.rooms[selectedRoom] &&
          state.rooms[selectedRoom].map((msg, index) => (
            <p key={index}>{msg}</p>
          ))}
      </div>
    </div>
  );
}

export default ChatApp;
```

## 🔥 useRef <a name="15"></a>

`useRef` is a React hook that provides a simple and effective way to reference and manipulate DOM elements, as well as to store values that persist across renders, reducing the need for additional state management. It is primarily used to access and interact with DOM elements directly, but its value-persisting feature also makes it a powerful tool for a wide range of use cases.

#### Anatomy of useRef:

- Creation: To create a useRef object, you can use the useRef function. It doesn't require an initial value like useState does.

- DOM Interaction: You can attach a ref attribute to a React element in your JSX to gain access to the DOM element it represents. Once created, a ref object can be assigned to the ref attribute, allowing you to manipulate the DOM element directly.

#### Key Concepts:

- Mutable Ref: A useRef object is a mutable object, and its value can be changed without causing the component to re-render. This makes it suitable for storing references to DOM elements, values that don't trigger re-renders, and various other use cases.

- Preservation of Value: The value stored in a useRef persists between renders. When you update a useRef object's current property, it doesn't trigger a re-render, which is in contrast to changing the state using useState, which does cause re-renders.

- .current Property: The useRef object has a current property, which holds the current value of the reference. You can access and modify this property directly, like myRef.current.

- Lifecycle Independence: Unlike state variables, which are tied to a component's lifecycle, useRef doesn't change its value when the component re-renders. It remains consistent and is suitable for persisting values and references across renders.

#### Use Cases:

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
//Here's an example of using useRef to interact with an external library, such as a video player. The ref holds a reference to the video player instance:
import React, { useEffect, useRef } from "react";
import VideoPlayerLibrary from "video-player-library"; // Replace with your library

function VideoPlayer() {
  const videoPlayerRef = useRef(null);

  useEffect(() => {
    const player = new VideoPlayerLibrary(videoPlayerRef.current);
    player.initialize();
    // You can interact with the video player here
  }, []);

  return <div ref={videoPlayerRef}></div>;
}
```

#### Common Pitfalls:

- Directly Mutating the current Property: Be cautious when directly modifying the current property of a useRef. While it's mutable, doing so should be reserved for special cases. Mutating current won't trigger re-renders, which means React won't be aware of changes, potentially leading to unexpected behavior.

- Using useRef for State: useRef should not be used to manage state that needs to trigger re-renders. For state management, use useState or useReducer instead.

- Not Considering Timing: Since useRef values persist between renders, they may not be immediately updated if you set them within an event handler or effect. Make sure to handle timing considerations appropriately when using useRef.

---

## 🔥 useState <a name="16"></a>

`useState` is a React hook that allows functional components to declare and manage state. It enables you to introduce dynamic behavior into your components by defining variables that can hold and update data. These state variables are the building blocks of interactive and responsive interfaces. `useState` is a cornerstone of React development because it empowers functional components to manage dynamic data and reactivity. It simplifies state management and reduces the complexity of your code by handling updates and re-renders automatically. This makes your components more predictable and easier to maintain.

#### Anatomy of useState:

- Creation: To create a state variable, you call the useState function with an initial state value. The function returns an array with two elements: the current state value and a function to update that state.

- State Variable: The first element in the array is the current state value, which you can read and display in your component.

- State Updater: The second element is a function that allows you to modify the state, triggering re-renders of the component with the updated state.

#### Use Cases:

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

- `Countdown Timer`: Create a simple countdown timer that decrements a value over time.

```javascript
import React, { useState, useEffect } from "react";

function CountdownTimer() {
  const [seconds, setSeconds] = useState(60);

  useEffect(() => {
    if (seconds > 0) {
      const timer = setInterval(() => setSeconds(seconds - 1), 1000);
      return () => clearInterval(timer);
    }
  }, [seconds]);

  return (
    <div>
      <p>Time remaining: {seconds} seconds</p>
    </div>
  );
}
```

- `Toggle Password Visibility`: Allow users to toggle password visibility in an input field.

```javascript
import React, { useState } from "react";

function PasswordInput() {
  const [password, setPassword] = useState("");
  const [showPassword, setShowPassword] = useState(false);

  return (
    <div>
      <input
        type={showPassword ? "text" : "password"}
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      />
      <button onClick={() => setShowPassword(!showPassword)}>
        {showPassword ? "Hide" : "Show"} Password
      </button>
    </div>
  );
}
```

- `Simple Form Validation`: Implement a simple form validation using state to show error messages.

```javascript
// In this example, useState is used to manage email input and validate it with a state variable for error handling.
import React, { useState } from "react";

function FormValidation() {
  const [email, setEmail] = useState("");
  const [validEmail, setValidEmail] = useState(true);

  const validateEmail = (input) => {
    const isValid = /\S+@\S+\.\S+/.test(input);
    setValidEmail(isValid);
  };

  return (
    <div>
      <input
        type="text"
        value={email}
        onChange={(e) => {
          setEmail(e.target.value);
          validateEmail(e.target.value);
        }}
      />
      {!validEmail && <p>Please enter a valid email.</p>}
    </div>
  );
}
```

- `Dynamic List Rendering`: Render a dynamic list of items with the ability to add and remove them.

```javascript
import React, { useState } from "react";

function DynamicList() {
  const [items, setItems] = useState([]);
  const [newItem, setNewItem] = useState("");

  const addItem = () => {
    if (newItem) {
      setItems([...items, newItem]);
      setNewItem("");
    }
  };

  const removeItem = (index) => {
    const updatedItems = items.filter((_, i) => i !== index);
    setItems(updatedItems);
  };
  return (
    <div>
      <input
        type="text"
        value={newItem}
        onChange={(e) => setNewItem(e.target.value)}
      />
      <button onClick={addItem}>Add Item</button>
      <ul>
        {items.map((item, index) => (
          <li key={index}>
            {item}
            <button onClick={() => removeItem(index)}>Remove</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

- `Multi-Step Form`: Build a multi-step form with useState to track the current step and gather user input progressively.

```javascript
import React, { useState } from 'react';

function MultiStepForm() {
  const [step, setStep] = useState(1);
  const [formData, setFormData] = useState({ name: '', email: '', password: '' });

  const nextStep = () => {
    if (step < 3) setStep(step + 1);
  }

  const previousStep = () => {
    if (step > 1) setStep(step - 1);
  }

  const updateFormData = (field, value) => {
    setFormData({ ...formData, [field]: value });
  }
  return (
    <div>
      {step === 1 && (
        <div>
          <input
            type="text"
            placeholder="Name"
            value={formData.name}
            onChange={(e) => updateFormData('name', e.target.value)}
          />
          <button onClick={nextStep}>Next</button>
        </div>
      )}
      {step === 2 && (
        <div>
          <input>
          type="text"
            placeholder="Email"
            value={formData.email}
            onChange={(e) => updateFormData('email', e.target.value)}
          />
          <button onClick={previousStep}>Previous</button>
          <button onClick={nextStep}>Next</button>
        </div>
      )}
      {step === 3 && (
        <div>
          <input
            type="password"
            placeholder="Password"
            value={formData.password}
            onChange={(e) => updateFormData('password', e.target.value)}
          />
          <button onClick={previousStep}>Previous</button>
          <button>Submit</button>
        </div>
      )}
    </div>
  );
}
```

- `Display Dynamic Content`: Display content based on user selection using useState.

```javascript
import React, { useState } from "react";

function DynamicContentDisplay() {
  const [selectedOption, setSelectedOption] = useState("option1");

  return (
    <div>
      <select
        value={selectedOption}
        onChange={(e) => setSelectedOption(e.target.value)}
      >
        <option value="option1">Option 1</option>
        <option value="option2">Option 2</option>
      </select>
      {selectedOption === "option1" && <p>Content for Option 1</p>}
      {selectedOption === "option2" && <p>Content for Option 2</p>}
    </div>
  );
}
```

- `Toggle Modal`: Create a modal that can be shown and hidden with useState.

```javascript
import React, { useState } from "react";

function ModalExample() {
  const [showModal, setShowModal] = useState(false);

  return (
    <div>
      <button onClick={() => setShowModal(true)}>Show Modal</button>
      {showModal && (
        <div className="modal">
          <p>Modal Content</p>
          <button onClick={() => setShowModal(false)}>Close</button>
        </div>
      )}
    </div>
  );
}
```

- `Pagination`: Implement a basic pagination system to navigate through a list of items.

```javascript
import React, { useState } from "react";

function PaginationExample() {
  const itemsPerPage = 5;
  const totalItems = 20;
  const [currentPage, setCurrentPage] = useState(1);

  const startIndex = (currentPage - 1) * itemsPerPage;
  const endIndex = startIndex + itemsPerPage;
  const visibleItems = Array.from(
    { length: totalItems },
    (_, index) => index + 1
  ).slice(startIndex, endIndex);

  const goToPage = (page) => setCurrentPage(page);

  return (
    <div>
      <ul>
        {visibleItems.map((item) => (
          <li key={item}>Item {item}</li>
        ))}
      </ul>
      <div className="pagination">
        {Array.from(
          { length: Math.ceil(totalItems / itemsPerPage) },
          (_, index) => index + 1
        ).map((page) => (
          <button
            key={page}
            onClick={() => goToPage(page)}
            className={page === currentPage ? "active" : ""}
          >
            {page}
          </button>
        ))}
      </div>
    </div>
  );
}
```

- `Image Slider`: Create a simple image slider that cycles through a list of images.

```javascript
import React, { useState, useEffect } from "react";

function ImageSlider() {
  const images = ["image1.jpg", "image2.jpg", "image3.jpg"];
  const [currentImage, setCurrentImage] = useState(0);

  useEffect(() => {
    const timer = setInterval(() => {
      setCurrentImage((currentImage + 1) % images.length);
    }, 3000);

    return () => clearInterval(timer);
  }, [currentImage]);
  return (
    <div>
      <img src={images[currentImage]} alt={`Image ${currentImage + 1}`} />
    </div>
  );
}
```

-`Color Picker`: Create a color picker that allows users to select and preview colors.

```javascript
import React, { useState } from "react";

function ColorPicker() {
  const [selectedColor, setSelectedColor] = useState("#FF5733");

  return (
    <div>
      <input
        type="color"
        value={selectedColor}
        onChange={(e) => setSelectedColor(e.target.value)}
      />
      <div
        style={{
          width: "100px",
          height: "100px",
          backgroundColor: selectedColor,
        }}
      ></div>
    </div>
  );
}
```

- `Real-Time Clock`: Create a real-time clock that updates every second.

```javascript
import React, { useState, useEffect } from "react";

function RealTimeClock() {
  const [currentTime, setCurrentTime] = useState(new Date());

  useEffect(() => {
    const intervalId = setInterval(() => setCurrentTime(new Date()), 1000);
    return () => clearInterval(intervalId);
  }, []);

  return (
    <div>
      <p>Current Time: {currentTime.toLocaleTimeString()}</p>
    </div>
  );
}
```

- `Quiz App`: Create a simple quiz application where users can answer questions one by one.

```javascript
import React, { useState } from "react";

function QuizApp() {
  const questions = [
    { question: "What is the capital of France?", answer: "Paris" },
    { question: "Which planet is known as the Red Planet?", answer: "Mars" },
    // Add more questions here
  ];

  const [currentQuestion, setCurrentQuestion] = useState(0);
  const [userAnswer, setUserAnswer] = useState("");
  const [score, setScore] = useState(0);

  const checkAnswer = () => {
    if (
      userAnswer.toLowerCase() ===
      questions[currentQuestion].answer.toLowerCase()
    ) {
      setScore(score + 1);
    }
    // Move to the next question
    setCurrentQuestion(currentQuestion + 1);
    setUserAnswer("");
  };

  return (
    <div>
      {currentQuestion < questions.length ? (
        <div>
          <h3>Question {currentQuestion + 1}:</h3>
          <p>{questions[currentQuestion].question}</p>
          <input
            type="text"
            value={userAnswer}
            onChange={(e) => setUserAnswer(e.target.value)}
          />
          <button onClick={checkAnswer}>Submit Answer</button>
        </div>
      ) : (
        <div>
          <p>
            Quiz completed! Your score: {score}/{questions.length}
          </p>
        </div>
      )}
    </div>
  );
}
```

- `Character Counter`: Create a character counter for a text input field.

```javascript
import React, { useState } from "react";

function CharacterCounter() {
  const [text, setText] = useState("");

  return (
    <div>
      <textarea
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="Enter text..."
      />
      <p>Character Count: {text.length}</p>
    </div>
  );
}
```

- `Toggle Password Strength`: Show the strength of a password as the user types it.

```javascript
import React, { useState } from "react";

function PasswordStrengthMeter() {
  const [password, setPassword] = useState("");

  const calculateStrength = (text) => {
    // Implement password strength calculation logic here
    return text.length >= 8 ? "Strong" : "Weak";
  };

  return (
    <div>
      <input
        type="password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
        placeholder="Enter password..."
      />
      <p>Password Strength: {calculateStrength(password)}</p>
    </div>
  );
}
```

- `Language Selector`: Build a language selector to change the website's language.

```javascript
import React, { useState } from "react";

function LanguageSelector() {
  const languages = ["English", "Spanish", "French", "German"];
  const [selectedLanguage, setSelectedLanguage] = useState("English");

  return (
    <div>
      <h2>{`Welcome! Select Language: ${selectedLanguage}`}</h2>
      <select
        value={selectedLanguage}
        onChange={(e) => setSelectedLanguage(e.target.value)}
      >
        {languages.map((language) => (
          <option key={language} value={language}>
            {language}
          </option>
        ))}
      </select>
      <p>{`You have selected ${selectedLanguage}.`}</p>
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
