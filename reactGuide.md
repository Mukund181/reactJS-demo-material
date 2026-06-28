# ⚛️ The Ultimate React Mastery Guide: From Zero to Expert

This guide is structured to teach you React step-by-step, assuming you know modern JavaScript (ES6+). 

---

## Table of Contents

1. [Introduction to React & Core Philosophy](#1-introduction-to-react--core-philosophy)
2. [Underlying Architecture (Virtual DOM & Fiber)](#2-underlying-architecture-virtual-dom--fiber)
3. [JSX & The Rendering Pipeline](#3-jsx--the-rendering-pipeline)
4. [Components: Structure, Types & Composition](#4-components-structure-types--composition)
5. [State & Props: The Data Flow](#5-state--props-the-data-flow)
6. [Core Hooks: useState & useEffect](#6-core-hooks-usestate--useeffect)
7. [DOM Access & Persistent Values: useRef](#7-dom-access--persistent-values-useref)
8. [Performance Optimization & Memoization (useMemo, useCallback, memo)](#8-performance-optimization--memoization-usememo-usecallback-memo)
9. [Sharing State: Context API & prop-drilling](#9-sharing-state-context-api--prop-drilling)
10. [Complex State Management: useReducer](#10-complex-state-management-usereducer)
11. [Concurrent Features: useTransition & useDeferredValue](#11-concurrent-features-usetransition--usedeferredvalue)
12. [React 19 Hooks & Core Actions (use, useActionState, useFormStatus, useOptimistic)](#12-react-19-hooks--core-actions-use-useactionstate-useformstatus-useoptimistic)
13. [Modern State Management: Redux Toolkit (RTK) & RTK Query](#13-modern-state-management-redux-toolkit-rtk--rtk-query)
14. [Routing: Building Single-Page Applications](#14-routing-building-single-page-applications)
15. [Advanced Patterns (HOCs, Render Props, Compound Components)](#15-advanced-patterns-hocs-render-props-compound-components)
16. [Performance & Production Optimization](#16-performance--production-optimization)
17. [Testing React Applications](#17-testing-react-applications)
18. [Machine Coding & Custom Hook Implementations](#18-machine-coding--custom-hook-implementations)

---

---

## 1. Introduction to React & Core Philosophy

To understand React, you must understand the problem it solves. Before React, developers manipulated the DOM directly using Imperative JavaScript.

### 1.1 Declarative vs. Imperative Programming

- **Imperative (How to do it):** You write step-by-step instructions telling the browser how to change the DOM.
- **Declarative (What to do):** You describe the desired UI state, and React handles updating the DOM to match that state.

#### Code Comparison:
```js
// ❌ Imperative (Vanilla JS)
const container = document.getElementById('container');
const btn = document.createElement('button');
let count = 0;
btn.textContent = `Clicks: ${count}`;
btn.addEventListener('click', () => {
  count++;
  btn.textContent = `Clicks: ${count}`; // Manually updating DOM
});
container.appendChild(btn);

// ✅ Declarative (React)
function ClickCounter() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => setCount(count + 1)}>
      Clicks: {count}
    </button>
  ); // We describe the UI; React manages the DOM synchronization
}
```

### 1.2 Library vs. Framework

| Feature | Library (e.g., React) | Framework (e.g., Angular, Nest.js) |
| :--- | :--- | :--- |
| **Control** | You are in control. You choose your routing, styling, and state management. | The framework is in control (Inversion of Control). You must follow its folder structure and conventions. |
| **Scope** | Focuses strictly on the View layer of MVC. | Complete out-of-the-box solution (routing, HTTP clients, form builders). |

---

## 2. Underlying Architecture (Virtual DOM & Fiber)

This is one of the most critical conceptual sections for senior-level React knowledge.

### 2.1 The Real DOM Bottleneck
The real browser DOM is slow. Not because JavaScript is slow, but because modifying DOM nodes triggers browser **reflows** (recalculating layouts) and **repaints** (drawing pixels to screen).

### 2.2 The Virtual DOM (VDOM)
The Virtual DOM is a lightweight, in-memory representation of the real DOM. It is a tree structure made of plain JavaScript objects.

```js
// A conceptual Virtual DOM node representation
const vnode = {
  type: 'button',
  props: {
    className: 'btn-primary',
    children: 'Click me'
  }
};
```

### 2.3 Reconciliation and the Diffing Algorithm
When a component's state changes:
1. React generates a **new Virtual DOM tree**.
2. React compares (diffs) the new VDOM tree with the previous VDOM tree.
3. React calculates the minimum list of operations needed to update the real DOM.
4. **The Diffing Algorithm rules:**
   - **Different element types:** If the element type changes (e.g., changing from `<div>` to `<span>`), React tears down the entire old tree and builds a new one from scratch.
   - **Same element types:** If they are the same type, React keeps the DOM node and updates only the changed attributes.
   - **Keys on Lists:** Keys tell React which elements changed, were added, or were removed. Without keys, React will re-render every item in a list when any item changes.

### 2.4 React Fiber
Introduced in React 16, **Fiber** is the core reconciliation engine rewrite. 
- **The old reconciler (Stack Reconciler):** Rendered components synchronously. If the component tree was deep, rendering could block the main browser thread, causing lag and unresponsive input fields.
- **The Fiber Reconciler:** Breaks rendering work into small units (Fibers) and executes them incrementally. It can pause, resume, prioritize, or discard rendering work.

#### Render Phase vs. Commit Phase
```
                       ┌─────────────────────────┐
                       │      Trigger Event      │
                       └────────────┬────────────┘
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│ 1. RENDER PHASE (Asynchronous & Interruptible)                         │
│ - React calculates changes (reconciliation).                           │
│ - Runs component rendering and diffing.                                │
│ - No real DOM changes are made here.                                   │
└───────────────────────────────────┬────────────────────────────────────┘
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│ 2. COMMIT PHASE (Synchronous & Non-interruptible)                      │
│ - React applies the calculated updates to the real DOM.                │
│ - Runs lifecycle methods/effects (componentDidMount, useEffect layout).│
└────────────────────────────────────────────────────────────────────────┘
```

---

## 3. JSX & The Rendering Pipeline

### 3.1 What is JSX?
JSX stands for **JavaScript XML**. It allows you to write HTML-like structures inside your JavaScript files.

It is not valid browser JavaScript. It must be compiled to standard JavaScript objects.

### 3.2 JSX Compilation (Babel / Vite)
Under the hood, a compiler converts JSX elements to React API calls.

```jsx
// 🟢 JSX Code:
const element = <h1 className="title">Hello World</h1>;

// 🔵 Compiled code (React 17+ Automatic Runtime):
import { jsx as _jsx } from "react/jsx-runtime";
const element = _jsx("h1", { className: "title", children: "Hello World" });
```

### 3.3 Core Rules of JSX
1. **Return a single root element:** All components must return one root. Use a Fragment (`<>...</>`) to group elements without adding extra nodes to the DOM.
2. **Close all tags:** Self-closing tags like `<img>` must end with a slash: `<img />`.
3. **Use camelCase for properties:** Since JSX compiles to JavaScript, standard JS naming rules apply. Use `className` instead of `class`, `htmlFor` instead of `for`, and `tabIndex` instead of `tabindex`.

---

## 4. Components: Structure, Types & Composition

React components are the building blocks of user interfaces.

### 4.1 Functional Components vs. Class Components

```javascript
// ❌ Legacy Class Component
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}

// ✅ Modern Functional Component
function Welcome({ name }) {
  return <h1>Hello, {name}</h1>;
}
```

Modern React utilizes Functional Components almost exclusively. Class components are legacy code; however, you still need to know them for understanding legacy codebases and custom Error Boundaries (which cannot yet be written as functional components).

### 4.2 Fragment
Fragments group children without wrapping them in an extra DOM element:

```jsx
// Without extra markup
return (
  <>
    <h1>Title</h1>
    <p>Paragraph</p>
  </>
);
```

### 4.3 Component Composition (Children Prop)
Composition is the practice of combining smaller, independent components to build complex interfaces. Instead of configuring child components via props, you can pass them as children.

```jsx
function Card({ children }) {
  return <div className="card-border">{children}</div>;
}

// Usage
function App() {
  return (
    <Card>
      <h2>Card Title</h2>
      <p>Card Content passed as children</p>
    </Card>
  );
}
```

---

## 5. State & Props: The Data Flow

Data flows in React recursively down the tree (unidirectional data flow).

### 5.1 Props (Properties)
Props are read-only configuration parameters passed to components by their parent. **A component must never modify its own props.**

```jsx
function Greeting({ name, age = 18 }) { // default props via ES6 destructuring
  return <p>Hello {name}, you are {age} years old.</p>;
}
```

### 5.2 State (Local Variable Storage)
State is a mutable data store managed inside a component. Changing state triggers a component re-render.

```jsx
const [count, setCount] = useState(0);
```

### 5.3 Batching State Updates
React groups multiple state updates inside the same event handler into a single re-render to improve performance.

```jsx
function handleClick() {
  setCount(c => c + 1);
  setCount(c => c + 1);
  setCount(c => c + 1);
  // React updates count to 3 and renders ONCE.
}
```

#### The Functional Updater Pattern
Always use a functional updater if your new state depends on the previous state:

```js
// ❌ Bad: state overrides can collide
setCount(count + 1);

// ✅ Good: functional update ensures current state is accessed
setCount(prev => prev + 1);
```

### 5.4 Lifting State Up
When multiple components need access to the same state, lift that state up to their closest common ancestor.

```jsx
// Parent component stores the state
function Parent() {
  const [value, setValue] = useState("");
  return (
    <>
      <Input value={value} onChange={setValue} />
      <Display value={value} />
    </>
  );
}
```

---

## 6. Core Hooks: useState & useEffect

Hooks are built-in functions introduced in React 16.8 that allow functional components to use state and lifecycle methods.

### 6.1 The Rules of Hooks
1. **Only Call Hooks at the Top Level:** Do not call Hooks inside loops, conditions, or nested functions.
2. **Only Call Hooks from React Functions:** Call them from functional components or custom hooks, not plain JavaScript helper functions.

### 6.2 useEffect: Managing Side Effects
`useEffect` lets you run side-effect operations (fetching data, subscribing to external sockets, manual DOM manipulations) after a component renders.

#### Lifecycle Mapping with useEffect:
```javascript
useEffect(() => {
  console.log("Runs on every single render cycle");
});

useEffect(() => {
  console.log("Equivalent to componentDidMount — runs ONCE on load");
}, []);

useEffect(() => {
  console.log("Runs on mount, and whenever dependencyChanges changes");
}, [dependencyChanges]);
```

#### Cleanup Functions (Avoiding Memory Leaks)
If your effect sets up a subscription, interval, or event listener, return a cleanup function to remove it when the component unmounts or before the effect runs again.

```javascript
useEffect(() => {
  const handleResize = () => console.log(window.innerWidth);
  window.addEventListener("resize", handleResize);
  
  // Cleanup function
  return () => {
    window.removeEventListener("resize", handleResize);
  };
}, []);
```

---

## 7. DOM Access & Persistent Values: useRef

### 7.1 Accessing DOM Elements Directly
When you need to manipulate a DOM node directly (e.g., focus an input, scroll a window), use `useRef` to store a reference to the element.

```jsx
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  
  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.focus();
  };
  
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus Input</button>
    </>
  );
}
```

### 7.2 Storing Mutable Values (Without Re-rendering)
Unlike `useState`, changing a ref's `.current` value **does not trigger a re-render**.

```javascript
function RenderCounter() {
  const renderCount = useRef(0);
  
  useEffect(() => {
    renderCount.current = renderCount.current + 1;
  }); // runs on every render
  
  return <p>This component has rendered {renderCount.current} times.</p>;
}
```

### 7.3 React 19 Ref simplification
In React 19, `forwardRef` is deprecated. You can now pass `ref` as a normal prop down to children!

```jsx
// React 19 Custom Input Component
function CustomInput({ ref, label }) {
  return (
    <label>
      {label}
      <input ref={ref} />
    </label>
  );
}
```

---

## 8. Performance Optimization & Memoization (useMemo, useCallback, memo)

By default, when a parent component's state changes, **all of its children re-render recursively**, even if their props haven't changed. React provides tools to optimize this behavior.

### 8.1 React.memo
`React.memo` is a higher-order component. It tells React to skip re-rendering a component if its props haven't changed.

```jsx
const ChildComponent = React.memo(({ name }) => {
  console.log("Child render");
  return <p>Welcome, {name}</p>;
});
```

### 8.2 The Referential Equality Problem
If props are non-primitive types (objects, arrays, functions), they are re-created on every parent render. This breaks `React.memo` because `{}` does not equal `{}` in JavaScript reference checks.

To fix this, use `useMemo` and `useCallback`.

### 8.3 useMemo: Cache Calculated Values
`useMemo` returns a memoized value. It recalculates the value only when one of its dependencies changes.

```javascript
const expensiveCalculation = useMemo(() => {
  return performHeavyMath(data);
}, [data]); // only runs performance-intensive code if 'data' changes
```

### 8.4 useCallback: Cache Function Definitions
`useCallback` returns a memoized version of a callback function, preventing it from being re-created on every render.

```javascript
const handleAction = useCallback(() => {
  doSomething(id);
}, [id]); // returns the same function reference unless 'id' changes
```

### 8.5 The React Compiler (React Forget)
> [!NOTE]
> React 19 introduces the **React Compiler**. If your build system integrates the compiler, it auto-memoizes component dependencies, rendering `useMemo` and `useCallback` largely obsolete in new codebases. However, you must still understand them for existing codebases.

---

## 9. Sharing State: Context API & prop-drilling

### 9.1 Prop Drilling
Prop drilling is the process of passing props down multiple nested component layers just to reach a deeply nested child component that needs them.

```
App (has state 'theme')
 └── Parent
      └── Child
           └── ThemeButton (needs 'theme')
```

### 9.2 Context API
Context provides a way to pass data through the component tree without manually passing props down through every level.

```jsx
// 1. Create context
const ThemeContext = createContext("light");

// 2. Wrap parent in Provider
function App() {
  const [theme, setTheme] = useState("dark");
  return (
    <ThemeContext.Provider value={theme}>
      <Main />
    </ThemeContext.Provider>
  );
}

// 3. Consume in deeply nested children
function ThemeButton() {
  const theme = useContext(ThemeContext);
  return <button className={theme}>Styled Button</button>;
}
```

---

## 10. Complex State Management: useReducer

`useReducer` is an alternative to `useState` for managing complex state objects or state transitions that depend on previous values. It uses a pattern similar to Redux.

```javascript
// 1. Define Initial State
const initialState = { count: 0 };

// 2. Define Reducer Function
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

// 3. Use in Component
function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </>
  );
}
```

---

## 11. Concurrent Features: useTransition & useDeferredValue

Introduced in React 18 to handle intensive updates without blocking the user interface.

### 11.1 useTransition
`useTransition` lets you mark state transitions as non-urgent. This allows user interactions (typing, clicking) to interrupt long-running background tasks.

```javascript
function SearchApp() {
  const [isPending, startTransition] = useTransition();
  const [query, setQuery] = useState("");
  const [results, setResults] = useState([]);
  
  const handleChange = (e) => {
    setQuery(e.target.value); // Urgent: Update search input immediately
    
    startTransition(() => {
      // Non-urgent: Filter list in background
      setResults(filterLargeArray(e.target.value));
    });
  };
  
  return (
    <div>
      <input type="text" onChange={handleChange} value={query} />
      {isPending && <p>Loading results...</p>}
      <ResultsList data={results} />
    </div>
  );
}
```

### 11.2 useDeferredValue
`useDeferredValue` accepts a value and returns a deferred version of it that lags behind the actual value during heavy updates.

```javascript
const deferredList = useDeferredValue(hugeList);
// Renders immediately with old list, then updates in background with new list
```

---

## 12. React 19 Hooks & Core Actions (use, useActionState, useFormStatus, useOptimistic)

React 19 modernizes codebases by adding first-class APIs for asynchronous actions, forms, and resource consumption.

### 12.1 The `use` API
The new `use()` function allows reading context or promises directly inside rendering logic, even conditionally.

```jsx
import { use } from 'react';

function UserProfile({ userPromise }) {
  // Reads a promise directly during rendering!
  const user = use(userPromise);
  return <h1>{user.name}</h1>;
}
```

### 12.2 useActionState (Formerly useFormState)
Manages the response state of form actions automatically.

```jsx
import { useActionState } from 'react';

async function updateName(prevState, formData) {
  const name = formData.get("name");
  try {
    await saveNameApi(name);
    return { success: true, message: "Name saved!" };
  } catch (err) {
    return { success: false, message: err.message };
  }
}

function NameForm() {
  // state holds returned value from action; formAction handles submit
  const [state, formAction, isPending] = useActionState(updateName, null);

  return (
    <form action={formAction}>
      <input name="name" type="text" />
      <button disabled={isPending}>Save</button>
      {state && <p>{state.message}</p>}
    </form>
  );
}
```

### 12.3 useFormStatus
Accesses form status information (like pending status) from child components without manually passing down props.

```jsx
import { useFormStatus } from 'react-dom';

function SubmitButton() {
  // Reads loading state from ancestor form context
  const { pending } = useFormStatus();
  return <button disabled={pending}>{pending ? "Submitting..." : "Submit"}</button>;
}
```

### 12.4 useOptimistic
Allows showing temporary feedback to the user while an asynchronous operation is in progress.

```javascript
import { useOptimistic } from 'react';

function Messages({ initialMessages }) {
  // returns optimistic state and function to update it immediately
  const [optimisticMessages, addOptimisticMessage] = useOptimistic(
    initialMessages,
    (state, newMessage) => [...state, { text: newMessage, sending: true }]
  );

  const sendMessageAction = async (formData) => {
    const text = formData.get("message");
    addOptimisticMessage(text); // UI updates instantly!
    await apiSend(text);       // Background request
  };
  
  // render optimisticMessages...
}
```

---

## 13. Modern State Management: Redux Toolkit (RTK) & RTK Query

For large applications, local state or Context API can become unmanageable. Redux Toolkit provides an architecture for scalable global state management.

```
┌────────────────────────────────────────────────────────┐
│                        VIEW (UI)                       │
│                   - Dispatches Action                  │
└───────────────────────────┬────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│                        ACTION                          │
│                   - Describes "What to do"             │
└───────────────────────────┬────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│                        REDUCER                         │
│                   - Updates Store state immutably      │
└───────────────────────────┬────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│                      STORE (STATE)                     │
│                   - Single source of truth             │
└────────────────────────────────────────────────────────┘
```

### 13.1 Setting Up a Slice in RTK
A slice contains the state and the reducer logic for a single feature module.

```javascript
// store/counterSlice.js
import { createSlice } from '@reduxjs/redux-toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      // Immer library under the hood allows write-like mutations safely
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    }
  }
});

export const { increment, decrement, incrementByAmount } = counterSlice.actions;
export default counterSlice.reducer;
```

### 13.2 Configuring the Store

```javascript
// store/store.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './counterSlice';

export const store = configureStore({
  reducer: {
    counter: counterReducer
  }
});
```

### 13.3 Consuming Store State and Dispatching Actions in Components

```jsx
import { useSelector, useDispatch } from 'react-redux';
import { increment } from './store/counterSlice';

export function Counter() {
  const count = useSelector((state) => state.counter.value);
  const dispatch = useDispatch();

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => dispatch(increment())}>Increment</button>
    </div>
  );
}
```

### 13.4 Advanced Data Fetching: RTK Query
RTK Query wraps async data fetching into clean hooks, automating loading states, caching, and cache invalidation.

```javascript
// store/apiSlice.js
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';

export const usersApi = createApi({
  reducerPath: 'usersApi',
  baseQuery: fetchBaseQuery({ baseUrl: 'https://api.example.com/' }),
  endpoints: (builder) => ({
    getUsers: builder.query({
      query: () => 'users',
    }),
  }),
});

// Auto-generated hook based on query name
export const { useGetUsersQuery } = usersApi;
```

---

## 14. Routing: Building Single-Page Applications

To create multiple "pages" without reloading the browser, use client-side routing.

### 14.1 Setting up React Router

```jsx
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link> | <Link to="/about">About</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/user/:userId" element={<UserProfile />} />
      </Routes>
    </BrowserRouter>
  );
}
```

### 14.2 Extracting Route Params
Use the `useParams` hook to capture variable segments in URLs:

```jsx
import { useParams } from 'react-router-dom';

function UserProfile() {
  const { userId } = useParams();
  return <h1>Viewing profile for User ID: {userId}</h1>;
}
```

### 14.3 Protected Routes
Route guards check conditional logic (like authentication) before rendering nested layouts.

```jsx
import { Navigate, Outlet } from 'react-router-dom';

function ProtectedRoute({ isAuthenticated }) {
  return isAuthenticated ? <Outlet /> : <Navigate to="/login" replace />;
}

// Router configuration:
// <Route element={<ProtectedRoute isAuthenticated={user} />}>
//   <Route path="/dashboard" element={<Dashboard />} />
// </Route>
```

---

## 15. Advanced Patterns (HOCs, Render Props, Compound Components)

Reusable design patterns help you write cleaner, DRY (Don't Repeat Yourself) components.

### 15.1 Higher-Order Components (HOC)
A function that accepts a component as an argument and returns a enhanced version of that component.

```jsx
function withLogging(WrappedComponent) {
  return function EnhancedComponent(props) {
    useEffect(() => {
      console.log(`Component loaded with props:`, props);
    }, [props]);
    return <WrappedComponent {...props} />;
  };
}
```

### 15.2 Render Props
Passing a function as a prop to delegate rendering logic from a child to a parent.

```jsx
function MouseTracker({ render }) {
  const [pos, setPos] = useState({ x: 0, y: 0 });
  const handleMove = (e) => setPos({ x: e.clientX, y: e.clientY });
  
  return <div style={{ height: '100vh' }} onMouseMove={handleMove}>{render(pos)}</div>;
}

// Usage
// <MouseTracker render={({ x, y }) => <h1>Pointer: {x}, {y}</h1>} />
```

### 15.3 Compound Components
A pattern where components work together to share state implicitly through Context, creating a clean API.

```jsx
const TabsContext = createContext();

function Tabs({ children, defaultValue }) {
  const [activeTab, setActiveTab] = useState(defaultValue);
  return (
    <TabsContext.Provider value={{ activeTab, setActiveTab }}>
      <div className="tabs-container">{children}</div>
    </TabsContext.Provider>
  );
}

function TabList({ children }) {
  return <div className="tabs-bar">{children}</div>;
}

function Tab({ value, label }) {
  const { activeTab, setActiveTab } = useContext(TabsContext);
  return (
    <button 
      onClick={() => setActiveTab(value)}
      className={activeTab === value ? 'active' : ''}
    >
      {label}
    </button>
  );
}

function TabPanel({ value, children }) {
  const { activeTab } = useContext(TabsContext);
  return activeTab === value ? <div>{children}</div> : null;
}

// Usage API
// <Tabs defaultValue="home">
//   <TabList>
//     <Tab value="home" label="Home" />
//     <Tab value="profile" label="Profile" />
//   </TabList>
//   <TabPanel value="home">Welcome home!</TabPanel>
//   <TabPanel value="profile">User details...</TabPanel>
// </Tabs>
```

---

## 16. Performance & Production Optimization

### 16.1 Code Splitting & Lazy Loading
Split your production JS bundle into smaller chunks that load only when a user navigates to a new page.

```jsx
import { lazy, Suspense } from 'react';

// Lazily load components
const LazyAnalytics = lazy(() => import('./pages/Analytics'));

function App() {
  return (
    <Suspense fallback={<div>Loading Page...</div>}>
      <LazyAnalytics />
    </Suspense>
  );
}
```

### 16.2 Virtualized Lists
When rendering thousands of items, render only the elements currently visible inside the viewport to save memory and avoid DOM thrashing.

Use libraries like `react-window` or `react-virtualized`.

---

## 17. Testing React Applications

### 17.1 Testing Core Components (React Testing Library)

```javascript
import { render, screen, fireEvent } from '@testing-library/react';
import Counter from './Counter';

test('increments counter on button click', () => {
  // Render component to simulated browser environment
  render(<Counter />);
  
  const button = screen.getByRole('button', { name: '+' });
  const text = screen.getByText(/Count: 0/i);
  
  expect(text).toBeInTheDocument();
  
  // Simulate user interactions
  fireEvent.click(button);
  
  expect(screen.getByText(/Count: 1/i)).toBeInTheDocument();
});
```

---

## 18. Machine Coding & Custom Hook Implementations

These custom hook implementations are standard requirements in technical interviews.

### 18.1 `useDebounce`

```javascript
import { useState, useEffect } from 'react';

export function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    // Cleanup: resets timer if values change before delay ends
    return () => {
      clearTimeout(handler);
    };
  }, [value, delay]);

  return debouncedValue;
}
```

### 18.2 `useThrottle`

```javascript
import { useState, useEffect, useRef } from 'react';

export function useThrottle(value, limit) {
  const [throttledValue, setThrottledValue] = useState(value);
  const lastRan = useRef(Date.now());

  useEffect(() => {
    const handler = setTimeout(() => {
      if (Date.now() - lastRan.current >= limit) {
        setThrottledValue(value);
        lastRan.current = Date.now();
      }
    }, limit - (Date.now() - lastRan.current));

    return () => clearTimeout(handler);
  }, [value, limit]);

  return throttledValue;
}
```

### 18.3 `useLocalStorage`

```javascript
import { useState, useEffect } from 'react';

export function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    try {
      const saved = localStorage.getItem(key);
      return saved ? JSON.parse(saved) : initialValue;
    } catch {
      return initialValue;
    }
  });

  useEffect(() => {
    try {
      localStorage.setItem(key, JSON.stringify(value));
    } catch (err) {
      console.error("Storage error:", err);
    }
  }, [key, value]);

  return [value, setValue];
}
```

### 18.4 `useWindowSize`

```javascript
import { useState, useEffect } from 'react';

export function useWindowSize() {
  const [windowSize, setWindowSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight,
  });

  useEffect(() => {
    function handleResize() {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight,
      });
    }

    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return windowSize;
}
```

### 18.5 `usePrevious`

```javascript
import { useRef, useEffect } from 'react';

export function usePrevious(value) {
  const ref = useRef(null);

  // Runs AFTER render, storing the value for the NEXT render cycle
  useEffect(() => {
    ref.current = value;
  }, [value]);

  return ref.current;
}
```

### 18.6 `useOnClickOutside`

```javascript
import { useEffect } from 'react';

export function useOnClickOutside(ref, handler) {
  useEffect(() => {
    const listener = (event) => {
      // Do nothing if clicking ref's element or descendent elements
      if (!ref.current || ref.current.contains(event.target)) {
        return;
      }
      handler(event);
    };

    document.addEventListener('mousedown', listener);
    document.addEventListener('touchstart', listener);

    return () => {
      document.removeEventListener('mousedown', listener);
      document.removeEventListener('touchstart', listener);
    };
  }, [ref, handler]);
}
```

---

> **Last updated**: June 2026
>
> **Pro-Tip for Mastery**: Copy the custom hooks from Section 18 into a local Sandbox/IDE and use them to construct simple React applications. Practice tracing the exact render loop steps as state propagates from parents to custom children.
