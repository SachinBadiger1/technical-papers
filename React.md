# React Fundamentals: From JSX to Hooks

A short guide to the core ideas of React, from JSX to hooks.

## What is React?

React is a JavaScript library for building user interfaces. It was developed at Facebook, now Meta, and is distributed as open-source software.

The term *library* is significant. React is concerned only with the view layer: given a set of data, it determines what the screen should display. Routing, data fetching, and project structure are left to the developer or to additional tools.

The central idea of React is that a user interface is a tree of components, each of which describes its own portion of the screen. When the data behind a component changes, React determines what has changed and updates only the affected part of the page. The developer specifies the desired result, and React determines the steps required to achieve it.

For this reason, React is widely used to build single page applications (SPAs), in which the page is not fully reloaded as the user navigates between views.

A page composed of components resembles an outline of the screen, as the following example illustrates:

```jsx
function App() {
  return (
    <>
      <Navbar />
      <Sidebar />
      <Dashboard />
      <Footer />
    </>
  );
}
```

The component-based approach offers the following advantages:

- Components can be reused in different parts of an application.
- Small units are easier to read, test, and correct than a single large page.
- Data flows in one direction, from parent to child, which makes the origin of a value easier to trace.
- A large ecosystem of libraries and tools supports the development of React applications.

## JSX

JSX is a syntax extension to JavaScript that resembles HTML. It is not a separate language. It allows the markup of a component and the logic that drives it to be kept together in one place.

```jsx
function Greeting() {
  const name = "Rahul";
  return <h1>Hello, {name}</h1>;
}
```

Curly braces return the code to JavaScript, so that any expression, such as a variable, a calculation, a function call, or a conditional expression, may be placed inside them.

JSX is stricter than HTML. The following rules are the ones most frequently violated by beginners.

**One root element.** A component must return a single parent element. When an additional wrapper `div` in the DOM is undesirable, the children may be enclosed in a fragment, as shown below:

```jsx
return (
  <>
    <h1>Title</h1>
    <p>Some text</p>
  </>
);
```

**`className` instead of `class`.** Because `class` is a reserved word in JavaScript, JSX uses `className` to specify the CSS class of an element.

```jsx
<div className="card">Hello</div>
```

**Every tag must be closed.** HTML permits elements such as `<img>` and `<input>` to remain unclosed. JSX does not, and such elements must be written as self-closing tags:

```jsx
<img src="photo.png" alt="Profile" />
<input type="text" />
```

**Attributes use camelCase.** For example, `onclick` is written as `onClick`, and `tabindex` is written as `tabIndex`.

## Components

A component is a reusable unit of the user interface. In modern React, a component is a function that returns JSX, and its name must begin with a capital letter. The capital letter allows React to distinguish a user-defined component such as `<Header />` from a standard HTML element such as `<header>`.

When an interface is divided in this way, each component has a single responsibility. If one section of the page is displayed incorrectly, the file responsible for it is easy to identify.

## Props

A component that always renders the same output has limited usefulness. Props, short for properties, allow a parent component to pass data to a child component, so that a single component can render different content.

In a typical example, the props are destructured in the function signature, as in `function ProductCard({ title, price })`. An alternative form accepts a single `props` object, as in `function ProductCard(props)`, and reads values such as `props.title` in the function body. The two forms are equivalent.

Two points should be noted:

1. Props are read-only. A component must not modify the props it receives. Data that needs to change belongs in state, which is discussed later in this article.
2. Props may hold any type of value, including strings, numbers, arrays, objects, and functions. The ability to pass functions is important when a child component must communicate with its parent.

String values may be written within quotation marks, whereas all other values must be enclosed in braces. Accordingly, a numeric value is written as `price={1499}` and not as `price="1499"`.

## Babel and its role in React

Browsers cannot interpret JSX. If a browser receives a JavaScript file in which a function contains `<h1>Hello</h1>`, it reports a syntax error. The JSX must therefore be translated into plain JavaScript beforehand, a task that has traditionally been performed by Babel.

Babel is a JavaScript compiler: it reads source code and produces different source code as output. In the context of React, its main function is to convert JSX into function calls, as the following two examples show.

```jsx
// what you write
const element = <h1 className="title">Hello</h1>;
```

```javascript
// roughly what the browser receives
import { jsx as _jsx } from "react/jsx-runtime";

const element = _jsx("h1", { className: "title", children: "Hello" });
```

Babel can also convert newer JavaScript syntax into older syntax, so that the code runs in browsers that do not yet support the newer features.

It is important to note that Babel is not part of React. It is a separate tool on which React projects commonly depend. In a project created with Vite, Babel rarely needs to be configured directly. The React plugin for Vite performs the JSX transformation, and, depending on the plugin chosen, a faster compiler such as SWC may be used instead of Babel. In either case, JSX is accepted as input and plain JavaScript is produced as output.

## Virtual DOM vs Real DOM

The **Real DOM** (Document Object Model) is the tree of objects that the browser constructs from an HTML document. Every element on the page is a node in this tree, and JavaScript can read or modify it.

The **Virtual DOM** is a lightweight description of the user interface that React maintains in the form of plain JavaScript objects. It is never displayed directly on the screen. Its purpose is to allow React to compare two versions of the interface efficiently.

The two structures differ in several respects. The real DOM is constructed and owned by the browser, and modifying it may trigger layout and repaint operations. The virtual DOM consists solely of JavaScript objects that are built and used by React, and no painting is involved. The former represents what is actually displayed; the latter represents what React considers should be displayed.

The process that occurs when state changes can be illustrated with a simple counter, in which a button increments a count stored in state.

When the button is clicked, the following sequence occurs:

1. The state value changes.
2. React calls the component again and gets a new description of the UI.
3. React compares that description with the previous one.
4. It finds the differences (here, only the text inside the `h1`).
5. It applies just those differences to the real DOM.

The comparison step is known as reconciliation. The advantage of the Virtual DOM does not lie in being inherently faster than the real DOM. Rather, it lies in removing the need for the developer to write update code by hand: the developer describes the interface for the current state, and React determines the minimum set of changes to the DOM.

## What is a bundler?

A real React application consists of many files rather than one. It contains numerous JavaScript modules, CSS files, images, fonts, and third-party packages from `node_modules`. A bundler follows the `import` statements, beginning at the entry file, gathers all the required resources, and produces optimized files that the browser can load.

The principal functions of a bundler or build tool are as follows:

- Resolving imports between modules
- Transforming JSX (and TypeScript, if you use it) into plain JavaScript
- Processing CSS and static assets
- Minifying and splitting code for production
- Running a dev server with hot reloading

The tools most commonly encountered include Webpack, Parcel, Rollup, and esbuild. Vite belongs to the same category as a modern build tool, and it is built on top of some of these.

## Why Vite and not Create React App?

Create React App (CRA) was formerly the standard method of starting a React project, and it was based on Webpack. It has since been deprecated by the React team. It no longer receives the updates required by modern React, and the official documentation now recommends frameworks or build tools such as Vite. Consequently, CRA should not be used to start a new project in 2026.

Vite is widely adopted for the following practical reasons:

- **Fast startup.** During development, Vite serves source files as native ES modules and allows the browser to load only what it requires, rather than bundling the entire application before the development server can start.
- **Fast updates.** Hot Module Replacement remains fast as the project grows, because only the modified module is replaced.
- **Minimal configuration.** A new project functions without additional setup, and the file `vite.config.js` is short and readable.
- **Optimized production builds.** The command `npm run build` produces minified and split output.

A new project is created with four commands:

```bash
npm create vite@latest my-react-app -- --template react
cd my-react-app
npm install
npm run dev
```

The terminal then displays a local address, usually `http://localhost:5173`. Opening this address in a browser shows the running React application.

## The folder structure of a Vite React project

A newly created project contains the directories `node_modules`, `public`, and `src`, the last of which holds `assets`, `App.jsx`, `main.jsx`, and the CSS files. At the top level it also contains `index.html`, `package.json`, the lock file, and `vite.config.js`. The exact contents depend on the version of the template.

The purpose of each item is described below.

`node_modules` holds every installed dependency; it should never be edited or committed to version control. `public` contains static files that are served unchanged, such as the favicon. `src` contains the source code of the application, and `assets` within it holds images and other files that are imported into components. `App.jsx` is the root component, and `main.jsx` is the file that starts React. `index.css` contains global styles, and `App.css` contains styles for the `App` component. `package.json` lists the scripts and dependencies, `package-lock.json` records the exact installed versions, and `vite.config.js` holds the Vite configuration, including the React plugin.

Two files merit closer examination, because they show how the parts of the project are connected.

**`index.html`** is almost empty:

```html
<body>
  <div id="root"></div>
  <script type="module" src="/src/main.jsx"></script>
</body>
```

The `div` element with the identifier `root` is the location at which React inserts the application. The `script` tag loads `main.jsx`.

**`main.jsx`** is the file in which React takes control of that element:

```jsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import App from "./App.jsx";
import "./index.css";

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

The code locates the root element, creates a React root on it, and renders `<App />` inside it. `StrictMode` does not produce any visible output. In development, it performs additional checks, such as rendering components twice, in order to help detect errors at an early stage.

The sequence is therefore as follows: the browser loads `index.html`, which loads `main.jsx`, which renders `App`, which in turn renders all other components.

## Class components vs functional components

Earlier React code was written using classes, whereas modern code uses functions.

```jsx
// Functional component
function Welcome({ name }) {
  return <h1>Hello, {name}</h1>;
}
```

```jsx
// Class component
import { Component } from "react";

class Welcome extends Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

The two kinds of component differ in several respects. Functional components are plain functions; they use hooks for state and effects, do not require `this`, generally need less code, and allow logic to be shared through custom hooks. Class components define a `render()` method, store state in `this.state`, use lifecycle methods, depend on `this` throughout (with binding problems for event handlers), require more boilerplate, and relied on patterns such as higher-order components for sharing logic.

Before React 16.8, only class components could hold state, and for this reason classes were widely used. Hooks changed this situation. At present, functional components can do everything that class components can, with the single exception of error boundaries, which still require a class. Class components are still found in older codebases and should be readable to the developer, but new code should be written with functions.

## What are hooks?

Hooks are functions that allow a functional component to use React features such as state and side effects. They were introduced in React 16.8, and their names all begin with `use`.

The hooks encountered first are the following.

`useState` stores a value between renders. `useEffect` runs code that interacts with systems outside React. `useRef` holds a value or a DOM element without causing a re-render. `useContext` reads shared data from a context. `useReducer` manages more complex state. `useMemo` and `useCallback` cache values and functions respectively, for performance.

Two rules govern the use of hooks, and the ESLint plugin for React reports a warning when either is violated:

1. Call hooks only at the top level of a component or custom hook. Not inside loops, conditions, or nested functions.
2. Call hooks only from React components or other hooks, never from regular JavaScript functions.

The reason for the first rule is that React identifies hooks by the order in which they are called. If this order changes between renders, React cannot match each call with its stored value.

### useState

`useState` provides a component with memory. An ordinary variable is reset each time the function runs, whereas state persists between renders, and changing it causes React to render the component again.

```jsx
const [value, setValue] = useState(initialValue);
```

The hook returns the current value and a function that updates it. The count of a like button, for example, would be stored in state in this way.

State must never be modified directly, for example by writing `likes = likes + 1` or `likes++`. React is not notified of such a change, and the interface is therefore not updated. The setter function must always be used.

When the new value depends on the previous value, a function should be passed to the setter:

```jsx
setLikes(prev => prev + 1);
```

This matters when several updates are queued together. Calling `setLikes(likes + 1)` three times in the same handler increases the value only once, because every call reads the same value of `likes`. Calling `setLikes(prev => prev + 1)` three times increases it by three.

State may also hold objects and arrays, but a new object or array must be created rather than modifying the existing one:

```jsx
const [user, setUser] = useState({ name: "Rahul", city: "Pune" });

setUser({ ...user, city: "Bengaluru" });
```

### useEffect

Rendering should only calculate what the screen looks like. Any other operation, such as fetching data, starting a timer, or listening to a browser event, is a side effect, and `useEffect` is the place for such operations. The code inside the effect runs after React has updated the screen.

```jsx
useEffect(() => {
  // effect code

  return () => {
    // optional cleanup
  };
}, [dependencies]);
```

The dependency array determines when the effect runs. If no array is given, the effect runs after every render. If the array is empty, it runs once, after the first render. If the array contains values, the effect runs after the first render and again whenever any of those values changes.

A typical use is loading data when a component first appears, usually by means of a fetch request inside the effect.

The effect callback itself cannot be `async`, so an asynchronous function is defined inside the effect and then called. When the fetched data is rendered as a list, each list item also needs a `key`, which React uses to track which item is which when the list changes.

The function returned from an effect is the cleanup function. React runs it before the effect runs again and when the component is removed. Anything that is started in an effect should be stopped in the cleanup:

```jsx
useEffect(() => {
  const id = setInterval(() => {
    console.log("tick");
  }, 1000);

  return () => clearInterval(id);
}, []);
```

Without the cleanup, the timer would continue to run after the component has been removed.

If an effect appears to run twice in development, the cause is StrictMode, which deliberately mounts, unmounts, and mounts the component again in order to expose missing cleanups. This does not occur in production.

### useRef

`useRef` returns an object with a `.current` property. React keeps the same object for the entire lifetime of the component, and changing `.current` does not trigger a re-render. This makes the hook suitable for two purposes.

The first purpose is to obtain access to a DOM element. When `ref={inputRef}` is placed on an input element, a button handler can call `inputRef.current.focus()` to move the cursor to that input.

After the component mounts, `inputRef.current` refers to the actual `<input>` element, so that browser methods such as `focus()` can be called on it.

The second purpose is to store a value that must be remembered but not displayed, such as a timer identifier:

```jsx
const timerRef = useRef(null);

function start() {
  timerRef.current = setInterval(() => console.log("running"), 1000);
}

function stop() {
  clearInterval(timerRef.current);
}
```

A simple criterion distinguishes the two hooks: if the screen should change when the value changes, `useState` is appropriate; otherwise, `useRef` is the better choice.

## Passing data between components

Data in React flows in one direction: from parent components to child components through props. All other techniques are variations of this principle.

**Parent to child.** Data is passed by means of props:

```jsx
function Child({ message }) {
  return <p>{message}</p>;
}

function Parent() {
  return <Child message="Hello from the parent" />;
}
```

**Child to parent.** A function is passed down as a prop. The parent owns the function, and the child calls it with the data to be sent upward, for example `onSend("Hi parent")` inside a button click handler.

**Between siblings.** There is no direct route between siblings. The shared data is moved up into the parent, which passes it down to both children. This pattern is called lifting state up and is described in the next section.

**Context.** Context is intended for data needed by many components at different depths of the tree, such as the current theme or the logged-in user. Instead of passing props through every intermediate layer, a practice known as prop drilling, the value is provided once near the top of the tree and read with `useContext` wherever it is needed. Context is best used only when prop drilling becomes a real inconvenience; for most small applications, props are sufficient.

## Lifting state up

Two components sometimes need to remain synchronized: one allows the user to change a value, and the other displays it. If each component keeps its own copy of the value in state, the copies will diverge.

The solution is to move the state to the closest common parent and to pass the value and the updater function down. The parent then becomes the single source of truth.

Consider a search box and a results list that share the search text. The parent component holds the query in state and passes it to both.

`SearchBox` does not own the text; it receives the current value and reports changes upward. `Results` does not communicate with `SearchBox` at all; it reads the same `query` from `App`. When the user types in the box, the list is updated because the state they share resides one level above them.

As a general rule, state should initially be kept inside the component that uses it, and moved upward only when a second component needs it.

## Handling events

React handles events through props on JSX elements, whose names are written in camelCase. The value passed must be a function, not a string and not the result of calling a function.

```jsx
function App() {
  function handleClick() {
    alert("Clicked!");
  }

  return <button onClick={handleClick}>Click me</button>;
}
```

An input whose value is tied to state is called a *controlled* input: its value comes from state, and every keystroke updates the state through `onChange`. This keeps React in control of what the field displays.

One case requires a different approach. Events on elements inside the JSX use the props described above, but events on objects outside the JSX, such as `window` or `document`, require a manual listener. The listener should be registered in an effect and removed in the cleanup function, in the same manner as the timer described earlier.

If the cleanup is omitted, an additional listener is added each time the component mounts, and the earlier listeners remain active.
