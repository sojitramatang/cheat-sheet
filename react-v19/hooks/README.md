# React Hook

## Table of Contents
- [React Hook](#react-hook)
  - [Table of Contents](#table-of-contents)
  - [What Is React Hook?](#what-is-react-hook)
  - [Why Use React Hook?](#why-use-react-hook)
  - [Types of React Hooks.](#types-of-react-hooks)
    - [State \& Lifecycle](#state--lifecycle)
    - [Context](#context)
    - [Refs](#refs)
    - [Performance Optimization](#performance-optimization)
    - [Debugging](#debugging)
    - [Concurrent Features](#concurrent-features)
    - [External Store Subscription](#external-store-subscription)
  - [useState](#usestate)
  - [useEffect](#useeffect)
  - [`useContext`](#usecontext)
  - [useReducer](#usereducer)
  - [useRef](#useref)
  - [useImperativeHandle](#useimperativehandle)
  - [useLayoutEffect](#uselayouteffect)
  - [useInsertionEffect](#useinsertioneffect)
  - [useId](#useid)
  - [useTransition](#usetransition)
  - [Key Points](#key-points)
  - [useDeferrdValue](#usedeferrdvalue)
  - [useSyncExternalStore](#usesyncexternalstore)
  - [useActionState *`v19`*](#useactionstate-v19)
  - [Explanation](#explanation)
  - [Additional Notes](#additional-notes)
  - [useCallback](#usecallback)
  - [useMemo](#usememo)
  - [useOptimistic](#useoptimistic)
  - [useDebugValue](#usedebugvalue)

---

## What Is React Hook?
React Hooks are functions that let you use state and other React features without writing a class. They were introduced in React 16.8 and allow you to manage state and side effects in functional components, making it easier to share logic between components and reducing the need for class components.
Hooks are a way to use React features in functional components, allowing you to manage state, lifecycle methods, and other features without the need for class components. They provide a more concise and readable way to write React components, making it easier to understand and maintain your code.

---

## Why Use React Hook?
1. **Simplified Code**: Hooks allow you to write cleaner and more concise code by eliminating the need for class components and lifecycle methods.
2. **Reusability**: Hooks enable you to extract and reuse stateful logic across components, making it easier to share functionality without duplicating code.
3. **Improved Readability**: Hooks make it easier to understand the flow of data and state in your components, leading to better readability and maintainability.
4. **No More "this" Keyword**: Hooks eliminate the need to use the "this" keyword, reducing confusion and making it easier to work with state and props.
5. **Easier Testing**: Hooks can be tested in isolation, making it easier to write unit tests for your components.
6. **Performance Optimization**: Hooks allow you to optimize performance by using techniques like memoization and lazy loading, leading to faster rendering and improved user experience.
7. **Access to React Features**: Hooks provide access to React features like context, refs, and memoization, allowing you to build more complex and powerful components.
8.  **Integration with Existing Code**: Hooks can be used alongside class components, allowing you to gradually adopt them in your existing codebase without needing to refactor everything at once.
9.  **Custom Hooks**: You can create your own custom hooks to encapsulate and reuse stateful logic, making it easier to manage complex state and side effects in your components.
10. **Better Handling of Side Effects**: Hooks like `useEffect` provide a more intuitive way to handle side effects in your components, making it easier to manage things like data fetching, subscriptions, and timers.
11. **Easier to Understand Lifecycle Methods**: Hooks provide a more straightforward way to manage component lifecycle methods, making it easier to understand when and how your components will update.
12. **Future-Proofing**: Hooks are the future of React development, and learning them now will help you stay up-to-date with the latest trends and best practices in React development.

---

## Types of React Hooks.
### State & Lifecycle

* `useState`
* `useReducer`
* `useEffect`
* `useLayoutEffect`
* `useInsertionEffect`

### Context

* `useContext`

### Refs

* `useRef`
* `useImperativeHandle`

### Performance Optimization

* `useMemo`
* `useCallback`

### Debugging

* `useDebugValue`

### Concurrent Features

* `useTransition`
* `useDeferredValue`
* `useId`
* `useActionState`
* `useOptimistic`


### External Store Subscription

* `useSyncExternalStore`

---

## useState
The `useState` hook is used to add state to functional components. It returns an array with two elements: the current state value and a function to update that value. You can use the `useState` hook to manage local component state, such as form inputs, toggles, and counters.
```javascript
import { useState } from 'react';
function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

**Explanation:**
1. **useState(0)**: The `useState` hook initializes the state variable `count` to `0`. The `setCount` function is used to update the state.
2. **Updating State**: The `setCount` function is called when the button is clicked, updating the state and causing the component to re-render with the new count value.
3. **Re-rendering**: When the state is updated using `setCount`, React re-renders the component, displaying the updated count value.
4. **Functional Updates**: You can also pass a function to `setCount` that takes the previous state as an argument, allowing you to update the state based on its previous value:
```javascript
setCount(prevCount => prevCount + 1);
```
1. **Lazy Initialization**: You can also pass a function to `useState` for lazy initialization, which will only run on the initial render:
```javascript
const [count, setCount] = useState(() => {
  // Expensive calculation
  return initialValue;
});
```

---

## useEffect
The `useEffect` hook is used to perform side effects in functional components. It runs after the component renders and can be used for tasks like data fetching, subscriptions, and manually changing the DOM. You can also specify dependencies to control when the effect runs.

Lifecycle methods in class components are replaced by the `useEffect` hook in functional components. The `useEffect` hook takes two arguments:
```javascript
useEffect(effect, dependencies);
```

Parameters:
- `effect`: A function that contains the side effect logic.
- `dependencies`: An optional array of dependencies that determine when the effect should run. If any of the dependencies change, the effect will run again.

```javascript
import { useEffect } from 'react';
function Example() {
  useEffect(() => {
    // Your side effect logic here
    console.log('Component mounted or updated');

    // Cleanup function (optional)
    return () => {
      console.log('Cleanup before component unmounts or updates');
    };
  }, [/* dependencies */]);

  return <div>Example Component</div>;
}
```

**Explanation:**
1. **Effect Function**: The function passed to `useEffect` contains the side effect logic. It runs after the component renders.
2. **Cleanup Function**: The optional cleanup function is returned from the effect function. It runs before the component unmounts or before the effect runs again if the dependencies change.
3. **Dependencies Array**: The dependencies array controls when the effect runs. If you pass an empty array (`[]`), the effect runs only once after the initial render. If you omit the array, the effect runs after every render.
4. **Conditional Effects**: You can specify dependencies to control when the effect runs. If any of the dependencies change, the effect will run again.
5. **Data Fetching**: The `useEffect` hook is commonly used for data fetching. You can make API calls inside the effect and update the state with the fetched data.
6. **Event Listeners**: You can also use `useEffect` to add event listeners and clean them up when the component unmounts or updates.
7. **Timers**: You can use `useEffect` to set up timers (e.g., `setTimeout`, `setInterval`) and clear them when the component unmounts or updates.

---

## `useContext`

The `useContext` hook allows you to access context values directly in your functional components, eliminating the need for `Context.Consumer`. It makes context consumption much simpler and cleaner.

```javascript
import { createContext, useContext } from 'react';

// Create a Context object
const MyContext = createContext();

// A Provider component that will supply context values to child components
function MyProvider({ children }) {
  // Define the value you want to provide through context
  const value = 'Hello, World!';
  
  // The Provider component makes the value available to all components below it
  return (
    <MyContext.Provider value={value}>
      {children}
    </MyContext.Provider>
  );
}

// A component that consumes the context value using useContext
function MyComponent() {
  // Use the useContext hook to access the context value
  const contextValue = useContext(MyContext);
  
  // Display the context value in the component
  return <div>{contextValue}</div>;
}

// Example usage of the Provider and Consumer components
function App() {
  return (
    <MyProvider>
      <MyComponent />
    </MyProvider>
  );
}

export default App;
```

**Explanation:**
1. **`createContext()`**: This creates a context object that holds a value and allows sharing it between components without passing props manually.
   
2. **`MyProvider` Component**: This component acts as the provider of context values to its child components. It uses the `MyContext.Provider` to make the value available for consumption.

3. **`useContext(MyContext)`**: Inside the `MyComponent` function, the `useContext` hook allows you to access the context value (`Hello, World!`) provided by `MyProvider`. 

4. **`App` Component**: The `App` component wraps `MyComponent` inside `MyProvider`, ensuring that `MyComponent` can access the context value.

**Key Points:**
- `createContext` creates the context object.
- `MyContext.Provider` is used to pass down the context value.
- `useContext(MyContext)` allows functional components to consume the context value.

---

## useReducer
The `useReducer` hook is used for managing complex state logic in functional components. It is similar to `useState`, but it allows you to manage state transitions using a reducer function, making it more suitable for complex state management scenarios.

```javascript
import { useReducer } from 'react';
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
}
function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </div>
  );
}
```
**Explanation:**
1. **Reducer Function**: The `reducer` function takes the current state and an action as arguments and returns the new state based on the action type.
2. **useReducer Hook**: The `useReducer` hook takes the reducer function and the initial state as arguments and returns the current state and a dispatch function to trigger state updates.
3. **Dispatching Actions**: The `dispatch` function is used to send actions to the reducer, which updates the state accordingly.

---

## useRef
The `useRef` hook is used to create mutable references that persist for the full lifetime of the component. It can be used to access DOM elements or store mutable values without causing re-renders.

```javascript
import { useRef } from 'react';
function TextInput() {
  const inputRef = useRef(null);

  const focusInput = () => {
    // Access the DOM element and focus it
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}
```
**Explanation:**
1. **useRef Hook**: The `useRef` hook creates a mutable reference object that persists for the full lifetime of the component.
2. **Accessing DOM Elements**: The `ref` attribute is used to attach the reference to the input element, allowing you to access it directly using `inputRef.current`.
3. **Focusing the Input**: The `focusInput` function uses the reference to focus the input element when the button is clicked.

---

## useImperativeHandle
The `useImperativeHandle` hook is used to customize the instance value that is exposed when using `ref` in a parent component. It allows you to control what values or methods are accessible through the ref.

```javascript
import { useImperativeHandle, forwardRef, useRef } from 'react';

function ParentComponent() {
  const childRef = useRef();

  const handleClick = () => {
    // Call the method exposed by the child component
    console.log(childRef.current.value());
  };

  return (
    <div>
      <ChildComponent ref={childRef} />
      <button onClick={handleClick}>Get Input Value</button>
    </div>
  );
}

const ChildComponent = forwardRef((props, ref) => {
  const inputRef = useRef();

  useImperativeHandle(ref, () => ({
    value: () => inputRef.current?.value,
  }));

  return <input ref={inputRef} type="text" />;
});

```
**Explanation:**
1. **Purpose:** `useImperativeHandle` allows a child component to expose specific methods or properties to its parent via a ref.\
2. **Usage with forwardRef:** It must be used with `forwardRef` to receive the ref from the parent.
3. **Custom Ref Exposure:** Instead of exposing the entire child DOM or component instance, you control what the parent can access (e.g., a .value() method).
4. **Encapsulation:** Promotes separation of concerns by hiding internal details of the child component while still allowing the parent limited, controlled interaction.
5. **Avoiding Unnecessary Re-renders:** The value exposed via the ref doesn’t trigger React re-renders, so using it avoids unnecessary updates in the parent component.

---

## useLayoutEffect

`useLayoutEffect` is a React Hook that is similar to `useEffect`, but it fires synchronously **after all DOM mutations** and **before the browser paints**. This makes it suitable for reading layout from the DOM and synchronously re-rendering.

Use this hook when you need to **measure DOM elements** or **trigger layout calculations** before the browser updates the screen.

```javascript
import React, { useState, useRef, useLayoutEffect } from 'react';

function LayoutEffectExample() {
  const [width, setWidth] = useState(0);
  const boxRef = useRef(null);

  useLayoutEffect(() => {
    if (boxRef.current) {
      const rect = boxRef.current.getBoundingClientRect();
      setWidth(rect.width);
    }
  }, []);

  return (
    <div>
      <div ref={boxRef} style={{ width: '50%' }}>
        Resize the window to see width changes.
      </div>
      <p>Box width: {width}px</p>
    </div>
  );
}
```

**Explanation:**
1. We use `useRef` to get a reference to the DOM element.
2. `useLayoutEffect` runs after the DOM has been updated, but before the browser has painted the screen, ensuring our measurement of the DOM (width) is accurate and not visually disruptive.
3. If we had used `useEffect` instead, the user might momentarily see incorrect layout data before it updates.

---

## useInsertionEffect

`useInsertionEffect` is a React Hook introduced in React 18 that allows you to inject styles or perform DOM mutations *synchronously* **before** any layout or paint occurs. This is particularly useful for CSS-in-JS libraries that need to inject styles during server-side rendering (SSR) hydration or before layout calculations.

Unlike `useEffect` and `useLayoutEffect`, `useInsertionEffect` is designed for injecting styles **safely and predictably** during render without causing visual inconsistencies.

```javascript
useInsertionEffect(effect: () => void | (() => void), deps?: DependencyList): void;
```
- `effect:` A function that performs the insertion (e.g., appending a style tag to the DOM).
- `deps` (optional): An array of dependencies that determine when the effect runs. It behaves similarly to the dependencies array in `useEffect` or `useLayoutEffect`.

```javascript
import React, { useInsertionEffect } from 'react';

const DynamicStyledComponent = () => {
  useInsertionEffect(() => {
    const styleTag = document.createElement('style');
    styleTag.textContent = `
      .dynamic-style {
        color: red;
        font-weight: bold;
      }
    `;
    document.head.appendChild(styleTag);

    return () => {
      document.head.removeChild(styleTag);
    };
  }, []);

  return <div className="dynamic-style">Styled with useInsertionEffect</div>;
};
```

1. The hook runs **synchronously before mutations**, which makes it ideal for injecting styles that need to be applied *before* the DOM is painted.
2. This avoids flashes of unstyled content (FOUC) and ensures the layout is computed with the correct styles in place.
3. **Not for general side effects**: Unlike `useEffect` or `useLayoutEffect`, this hook should only be used for **inserting styles or DOM mutations that must occur early**. Using it for data fetching or subscriptions is an anti-pattern.
4. Runs only in **client-side rendering** (not during server rendering).

> ⚠️ Warning: Use this hook sparingly and only for critical style injection logic, as it may affect performance if overused.

---

## useId

`useId` is a built-in React hook introduced in React 18. It generates a stable, unique ID that is consistent between the server and client during hydration. This is especially useful for accessibility attributes such as `aria-*` or form field `id` attributes, where the ID must remain consistent across renders.


```jsx
const id = useId();
```
- This hook must be called inside a React component or another custom hook.
- The generated ID will be unique per component instance.
- `useId` does not take any arguments.
- A string representing a unique ID.

```jsx
import React, { useId } from 'react';

function FormField() {
  const id = useId();

  return (
    <div>
      <label htmlFor={id}>Username:</label>
      <input id={id} type="text" name="username" />
    </div>
  );
}
```

**Explanation:**

1. `useId` is called to generate a unique and stable ID.
2. That ID is assigned to both the `label`'s `htmlFor` and the `input`'s `id`.
3. This ensures proper association between the label and the input field for accessibility tools.
4. Because `useId` generates the same ID on the server and client, it prevents mismatches during server-side rendering (SSR) and hydration.


    > ⚠️ Important Notes
    >
    >Do **not** use `useId` for keys in lists; use stable values like database IDs instead.
    >
    >It’s ideal for IDs that need to be deterministic and consistent across renders.

**When to Use**

* Associating form inputs with labels.
* Generating unique `aria-describedby` or `aria-labelledby` attributes.
* Any case where a unique and stable ID is needed in the DOM.

---

## useTransition

The `useTransition` hook is part of React's Concurrent Mode, designed to manage state transitions with lower priority. This hook allows you to mark updates that can be deferred, making UI updates more responsive by keeping the user interface interactive while a larger, potentially slower update occurs in the background.

This hook provides a way to manage time-consuming operations (like data fetching, animations, or complex computations) while keeping the application responsive.

```ts
const [isPending, startTransition] = useTransition()
````

- **isPending**: A boolean value indicating if the transition is ongoing. It helps to keep track of whether the component is waiting for the state change to complete.
- **startTransition**: A function to wrap around the state updates you want to mark as low priority.
- `useTransition` returns an array with two values:
  - `isPending`: A boolean indicating whether the transition is still ongoing.
  - `startTransition`: A function to wrap the updates you want to defer.

```jsx
import { useState, useTransition } from 'react';

const ListComponent = () => {
  const [items, setItems] = useState([]);
  const [isPending, startTransition] = useTransition();

  const fetchItems = async () => {
    const newItems = await fetch('/api/items')
      .then(res => res.json());
    startTransition(() => {
      setItems(newItems);
    });
  };

  return (
    <div>
      <button onClick={fetchItems}>Load Items</button>
      {isPending && <p>Loading...</p>}
      <ul>
        {items.map((item, index) => (
          <li key={index}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
};

export default ListComponent;
```

**Explanation:**
1. **useTransition** helps to manage transitions efficiently. In the above example, when the `Load Items` button is clicked, it triggers an asynchronous fetch operation for new items. The `startTransition` function is used to wrap the `setItems` update, indicating that this is a low-priority update.
2. **isPending**: While the items are being fetched and rendered, the `isPending` state will be `true`, allowing us to show a loading indicator (`<p>Loading...</p>`). This ensures that the UI remains responsive, even if the fetching process takes time.

**Why Use `useTransition`?**
- Without `useTransition`, every state update would be treated with equal priority, causing the UI to potentially freeze or become unresponsive if the state change involves expensive operations like network requests or complex computations. By wrapping the update with `startTransition`, React can defer the state update and let the UI remain interactive.

## Key Points

> `useTransition` is ideal for scenarios where some UI updates can be deferred to keep the interface responsive.
>
> It works best in combination with other concurrent features of React, like `Suspense`.
>
> It doesn’t make the operation faster, but it helps prioritize user interaction and UI updates.

---

## useDeferrdValue
The `useDeferredValue` hook is a React hook that allows you to defer updating a state or value that is computationally expensive. This helps to improve the user experience by keeping the interface responsive while waiting for a more expensive computation to complete.

`useDeferredValue` is used to defer a value's update until React has completed rendering the high-priority updates. This is especially useful in situations where you are rendering large lists or performing heavy computations and don't want to block the UI. 

In simple terms, it helps to prioritize important updates (like user input) over less important ones (like expensive calculations or rendering large components).

`useDeferredValue` takes a single argument:
- **value**: The value to be deferred (e.g., a state value, or computed value).
- The hook returns the deferred value that can be used in the render cycle.

```jsx
import React, { useState, useDeferredValue } from 'react';

function SearchComponent() {
  const [searchQuery, setSearchQuery] = useState('');
  const deferredSearchQuery = useDeferredValue(searchQuery);

  return (
    <div>
      <input
        type="text"
        value={searchQuery}
        onChange={(e) => setSearchQuery(e.target.value)}
        placeholder="Search..."
      />
      <ListComponent query={deferredSearchQuery} />
    </div>
  );
}

function ListComponent({ query }) {
  // Simulate a heavy rendering operation for the list
  console.log("Rendering List with query:", query);
  return <div>{/* List items based on query */}</div>;
}

export default SearchComponent;
```

**Explanation:**
1. **Initial Setup**: We have an input field where users can type their search query. This search query is stored in the `searchQuery` state.
2. **Deferred Value**: The `useDeferredValue` hook is called with the `searchQuery` value. It returns a deferred version of the query called `deferredSearchQuery`. This deferred value will only update when the important updates (like UI interactions) are processed, not during heavy computations or rendering.
3. **Rendering**: The `ListComponent` receives the deferred value `deferredSearchQuery` as its prop. This ensures that even though the search input might change rapidly, the list rendering (which could be computationally expensive) doesn't trigger unnecessarily on each input change.
4. By using `useDeferredValue`, React can prioritize the input handling over rendering the large list, improving the perceived performance and responsiveness of the application.

**Why Use `useDeferredValue`?**
- **Performance Optimization**: For scenarios where the user interface is slow to update because of computationally expensive operations (e.g., filtering, sorting, or rendering large lists).
- **Improved User Experience**: Ensures smooth user interactions (like typing in a search box) without interruptions from heavy rendering tasks.
- **Non-blocking UI**: Defers low-priority state updates to avoid blocking high-priority tasks like user input handling.

**When Not to Use It**
- **Low-cost computations**: If the operation is not computationally expensive, deferring the update might not provide any benefit.
- **Frequent value updates**: If the value updates too often or if it’s too time-sensitive, deferring it might delay updates and hurt user experience.

---

## useSyncExternalStore

`useSyncExternalStore` is a hook introduced in React 18, designed for subscribing to external stores (e.g., state management libraries, browser APIs, etc.). It allows React to read from an external data source and ensures that components using it stay in sync with any updates to that external store.

It provides a way to interact with non-React data sources while making sure the component is always rendered with the most up-to-date state.

```javascript
useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot?)
```

**Parameters:**
1. **`subscribe`** (Function):
   * A function that subscribes the component to an external store. It will be called when the external store changes, and it should trigger a re-render by calling the provided callback.
2. **`getSnapshot`** (Function):
   * A function that retrieves the current value (or snapshot) from the external store. It should return the current value of the store, which is used for rendering.
3. **`getServerSnapshot`** (Optional function):
   * A function used when rendering on the server side (SSR). It returns the initial snapshot for the server-side render. This argument is optional.

**Why use `useSyncExternalStore`?**

Before React 18, it was often necessary to manually manage subscriptions to external data sources or use custom hooks to synchronize external state with React components. React 18 standardizes this process and ensures updates are handled in a more efficient and predictable manner.

Let's imagine we have an external store, like a simple counter that can be shared across components. Here's how you might use `useSyncExternalStore` to subscribe to it:

**Store Setup (External Store):**

```javascript
let count = 0;
const listeners = new Set();

function subscribe(callback) {
  listeners.add(callback);
  return () => {
    listeners.delete(callback);
  };
}

function getSnapshot() {
  return count;
}

function increment() {
  count += 1;
  listeners.forEach(callback => callback());
}
```

This simple store has:

* A `subscribe` function to listen for changes.
* A `getSnapshot` function to retrieve the current state.

**Component Using `useSyncExternalStore`:**

```javascript
import React from 'react';
import { useSyncExternalStore } from 'react';

function Counter() {
  // Subscribe to the external store
  const count = useSyncExternalStore(subscribe, getSnapshot);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}
```
**How it Works:**

* The component subscribes to the `subscribe` function of the store using `useSyncExternalStore`.
* It uses `getSnapshot` to get the current value of `count`.
* When the `increment` function is called, it updates the store's state and calls all subscribed listeners, triggering a re-render of the component with the updated state.

**Explanation:**

1. **`subscribe`**: This function ensures the component is notified of any changes in the external store. In the example, we use a `Set` to hold all listeners, and when the state changes (e.g., `increment()`), we call the callback functions to update all subscribed components.
2. **`getSnapshot`**: This function retrieves the current value of the external store (in this case, the current `count`). It is called whenever React needs to render the component.
3. **Reactivity**: When the external store's state changes (as with `increment()`), the `subscribe` function notifies the React component to re-render. This ensures the component always has the most up-to-date state.
4. **Server-side rendering (SSR)**: React 18 also lets you pass an optional third argument, `getServerSnapshot`, to handle server-side rendering, which allows the snapshot to be precomputed on the server.

**Key Benefits:**

1. **Consistency**: By using `useSyncExternalStore`, React guarantees that the component is always synchronized with the external store, even across re-renders.
2. **Optimized Re-renders**: React will re-render only when the external store’s state has actually changed, minimizing unnecessary updates.
3. **External Store Integration**: It provides a standard way of integrating external stores with React's rendering process.

---

## useActionState *`v19`*

`useActionState` is a React hook that helps manage state updates from server actions, especially in forms.
It returns the current state, a dispatch function to trigger the action, and a form action handler.


```javascript
function ChangeName({ name, setName }) {
  const [error, submitAction, isPending] = useActionState(
    async (previousState, formData) => {
      const error = await updateName(formData.get("name"));
      if (error) {
        return error;
      }
      redirect("/path");
      return null;
    },
    null,
  );

  return (
    <form action={submitAction}>
      <input type="text" name="name" />
      <button type="submit" disabled={isPending}>Update</button>
      {error && <p>{error}</p>}
    </form>
  );
}
```

## Explanation
- **useActionState**: This hook is used to manage the state of an action, including error handling and loading states. It takes a function that performs the action and returns an array containing the error, a function to submit the action, and a boolean indicating if the action is pending.
- **submitAction**: This function is used to trigger the action. It takes the form data as an argument and returns a promise that resolves when the action is complete.
- **isPending**: This boolean indicates whether the action is still in progress. You can use it to disable buttons or show loading indicators.
- **Error Handling**: The error handling logic is included in the action function. If an error occurs, it is returned and can be displayed in the UI.
- **Redirect**: The redirect function is called after the action is complete, allowing you to navigate to a different route.
## Additional Notes
- React.useActionState was previously called ReactDOM.useFormState in the Canary releases, but we’ve renamed it and deprecated useFormState.

---

## useCallback

1. **Purpose**: `useCallback` is a React Hook used to memoize a callback function, preventing it from being re-created on every render unless its dependencies change.
2. **Optimization**: It's mainly used to optimize performance, especially when passing functions to child components that rely on referential equality (e.g., `React.memo`).

```jsx
import React, { useState, useCallback } from 'react';

const CounterButton = React.memo(({ onClick }) => {
  console.log("Button rendered");
  return <button onClick={onClick}>Increment</button>;
});

function Counter() {
  const [count, setCount] = useState(0);
  const [other, setOther] = useState(false);

  const increment = useCallback(() => {
    setCount(prev => prev + 1);
  }, []);

  return (
    <div>
      <p>Count: {count}</p>
      <CounterButton onClick={increment} />
      <button onClick={() => setOther(!other)}>Toggle Other State</button>
    </div>
  );
}
```

**Explanation:**

- Without `useCallback`, the `increment` function would be re-created every time the parent component (`Counter`) re-renders, causing the `CounterButton` (even wrapped in `React.memo`) to re-render unnecessarily.
- With `useCallback`, `increment` is only re-created if its dependencies change — here, it has no dependencies (`[]`), so it stays the same across renders.
- This avoids unnecessary renders and boosts performance in complex apps.

---

## useMemo

1. `useMemo` is a React Hook that **memoizes the result of a computation** — it caches the result and only recalculates it when its dependencies change.
2. It helps to **optimize performance** by preventing expensive function executions on every render.

```jsx
import React, { useState, useMemo } from 'react';

function ExpensiveComponent({ number }) {
  const [count, setCount] = useState(0);

  const squaredNumber = useMemo(() => {
    console.log('Calculating square...');
    return number * number;
  }, [number]);

  return (
    <div>
      <p>Squared Number: {squaredNumber}</p>
      <button onClick={() => setCount(count + 1)}>Re-render ({count})</button>
    </div>
  );
}
```

**Explanation:**
* When the component re-renders (like clicking the button), `useMemo` checks if `number` has changed.
* If `number` hasn’t changed, it **reuses the cached `squaredNumber`**, skipping the calculation and `console.log`.
* This is useful when the calculation inside `useMemo` is **computationally heavy**, preventing unnecessary work on each render.

---

## useOptimistic

1. `useOptimistic` is a **React 18+ hook** used to manage **temporary optimistic UI updates** before an async operation finishes.
2. It lets you **simulate a successful state change** immediately (e.g. adding a comment) while the real state is still being updated in the background.

```jsx
import React, { useState, useOptimistic } from 'react';

function TodoList() {
  const [todos, setTodos] = useState([]);
  
  const [optimisticTodos, addOptimisticTodo] = useOptimistic(
    todos,
    (currentTodos, newTodo) => [...currentTodos, newTodo]
  );

  const handleAddTodo = async () => {
    const newTodo = { id: Date.now(), text: 'New Task' };

    // Show it in the UI immediately
    addOptimisticTodo(newTodo);

    // Simulate async action (e.g. POST to server)
    await new Promise((res) => setTimeout(res, 1000));

    // Update real state
    setTodos((prev) => [...prev, newTodo]);
  };

  return (
    <div>
      <button onClick={handleAddTodo}>Add Todo</button>
      <ul>
        {optimisticTodos.map((todo) => (
          <li key={todo.id}>{todo.text}</li>
        ))}
      </ul>
    </div>
  );
}

export default TodoList;
```

**Explanation:**
* `useOptimistic(state, updateFn)` returns a **temporary view of state** (`optimisticTodos`) and a **function to apply optimistic updates**.
* When `addOptimisticTodo` is called, the UI updates *immediately* — even though the real state (`todos`) hasn't changed yet.
* After the async task (like an API call) completes, you update the actual state (`setTodos`) to sync everything.

> This gives users **faster, more responsive feedback**, especially useful in slow network conditions.

---

## useDebugValue

1. **`useDebugValue`** is a React hook that helps you **display custom debug information** for custom hooks in React DevTools.
2. It’s primarily used in **development mode** to aid in debugging and improving the development experience.
3. It does **not affect the app’s behavior** or performance, but it provides insights into the state or logic of custom hooks when inspecting React components.

```jsx
import React, { useState, useEffect } from 'react';

function useCounter() {
  const [count, setCount] = useState(0);

  // Display the current count value in React DevTools
  useDebugValue(count > 5 ? 'High' : 'Low');

  return [count, setCount];
}

function Counter() {
  const [count, setCount] = useCounter();

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default Counter;
```

**Explanation**
1. **`useDebugValue`** takes one or two arguments:

   - The **first argument** is the value you want to display in the React DevTools.
   - The **optional second argument** can be a **function** to transform or format the debug value.

2. In this example, the `useCounter` custom hook:

   - Displays a **custom label** (`High` or `Low`) in React DevTools based on whether `count` is greater than 5.
   - This allows you to easily see the value of `count` and whether it’s in a "high" or "low" state when inspecting `useCounter` in DevTools.

**When to Use `useDebugValue`?**
- **Custom Hooks:** If you create complex hooks and want to make it easier to debug them using React DevTools.
- **Development Only:** Since `useDebugValue` doesn’t affect production builds, it's best for enhancing the development and debugging experience without any runtime performance impact.
---
