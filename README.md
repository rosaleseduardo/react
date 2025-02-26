<div align = "center">
  <img
    src = "./assets/logo-react.jpg"
    width = "70%"
    alt = "reactjs-logo" />
</div>

<hr>

## Table of Contents

- [A (Mostly) Complete Guide to React Rendering Behavior](#a-mostly-complete-guide-to-react-rendering-behavior)
- [Why using React?](#why-using-react)
- [Routing](#routing)
- [State Management](#state-management)
  - [Context API](#context-api)
  - [Redux](#redux)
    - [What is it?](#what-is-it)
    - [Purpose and Use Cases](#purpose-and-use-cases)
    - [Tradeoffs](#tradeoffs)
    - [When Should I Consider Using This?](#when-should-i-consider-using-this)
    - [Redux is too much boilerplate, how all of this came about?](#redux-is-too-much-boilerplate-how-all-of-this-came-about)
- [Difference Between ReactNode and JSXElement](#difference-between-reactnode-and-jsxelement)
- [Queueing a Series of State Updates](#queueing-a-series-of-state-updates)
- [Suspense and Lazy Loading - PENDING](#suspense-and-lazy-loading)
- [What is VDOM And How Does it work? - PENDING](#what-is-vdom-and-how-does-it-work)
- [How does strict mode impacts the react app - PENDING](#how-does-strict-mode-impacts-the-react-app)
- [Development Patterns](#development-patterns)
  - [Presentational and Container Components](#presentational-and-container-components)
  - [Higher Order Components](#higher-order-components)
  - [Render Props](#render-props)
  - [Controlled vs. Uncontrolled Components](#controlled-vs-uncontrolled-components)
  - [Custom Hooks](#custom-hooks)
  - [Compound Components](#compound-components)
  - [Portals](#portals)
  - [Controlled vs. Uncontrolled Input Components](#controlled-vs-uncontrolled-input-components)
  - [Error Boundaries](#error-boundaries)
  - [Functional Components & Hooks](#functional-components--hooks)

# A (Mostly) Complete Guide to React Rendering Behavior

- [A (Mostly) Complete Guide to React Rendering Behavior](https://blog.isquaredsoftware.com/2020/05/blogged-answers-a-mostly-complete-guide-to-react-rendering-behavior/)

# Why using React?

React is a popular JavaScript library for building user interfaces, primarily
for web applications. Here are some key reasons why developers use React:

1. **Component-Based Architecture**

    React allows developers to build UIs using reusable, self-contained
    components. Each component can manage its own state, making it easier to
    develop, maintain, and scale applications. This modular approach promotes
    code reusability and makes it easier to manage large projects.

2. **Declarative Programming**

    React uses a declarative style, meaning you describe what the UI should look
    like, and React handles the rendering. This approach simplifies the
    development process, especially when dealing with dynamic content and
    complex UIs. By declaring the UI's state and how it should behave, React
    updates the DOM automatically when data changes.

3. **Efficient Rendering with Virtual DOM**

    React uses a virtual DOM to minimize direct manipulation of the actual DOM.
    Changes are first applied to the virtual DOM, and React then determines the
    most efficient way to update the real DOM. This results in faster rendering
    and better performance, particularly for applications with a lot of
    interactive elements.

4. **Strong Ecosystem and Community**

    React has a robust ecosystem of tools, libraries, and extensions, including
    state management solutions like Redux, React Query, and routing libraries
    like React Router. There’s a large community around React, which means
    plenty of tutorials, examples, and support if you run into issues.

5. **React Hooks**

    Introduced in React 16.8, hooks allow developers to use state and other React
    features without writing class components. This makes code simpler, cleaner,
    and easier to understand. Hooks like useState, useEffect, and useContext
    bring powerful functionalities that were previously only available in class
    components.

6. **Cross-Platform Development with React Native**

    React isn’t just for web development. With React Native, you can use the same
    codebase to build native mobile applications for iOS and Android, reducing
    development time and effort. This cross-platform approach helps companies
    save resources while maintaining consistent user experiences across devices.

7. **SEO-Friendly (with Server-Side Rendering)**

    React can be optimized for SEO through server-side rendering (SSR) or static
    site generation (SSG). Tools like Next.js make it easy to set up SSR, which
    improves page load times and helps search engines index content better. SEO
    is critical for many businesses, and React’s flexibility allows for
    solutions that meet these needs.

8. **Strong Support for TypeScript**

    React works well with TypeScript, providing static type checking and helping
    catch errors during development. This ensures better code quality and reduces
    the chances of runtime errors. TypeScript’s integration with React makes the
    development process smoother, especially for larger projects with complex
    data flows.

These benefits make React a versatile and efficient tool for building modern web
applications, from small personal projects to large enterprise-grade systems.

# Routing

Read [this documentation](https://reactrouter.com/en/main/start/overview).

# State Management

- [A better way of solving prop drilling in React apps](https://blog.logrocket.com/solving-prop-drilling-react-apps/)
- [What is Context API and useContext? Everything You Need to Know](https://www.alphabold.com/what-is-context-api-and-usecontext/)

## Context API

Read [this documentation](https://react.dev/reference/react/createContext).

## Redux

### What is it?

Redux is a pattern and library for managing and updating application state, using
events called "actions". It serves as a centralized store for state that needs to
be used across your entire application, with rules ensuring that the state can
only be updated in a predictable fashion.

Redux focuses on making state updates predictable and traceable, with inspiration
from the [Flux Architecture](https://facebook.github.io/flux/docs/in-depth-overview/)
pattern and Functional Programming principles. It relies on separating descriptions
of "what happened", called "actions", from the logic that decides how state should
be updated, called "reducer functions". Redux centralizes global application state
into a single "store", and provides browser DevTools to view the history of state
changes over time.

### Purpose and Use Cases

**The primary purpose of Redux is to help you manage "global" state** — state
that is needed across many parts of your application.

**The patterns and tools provided by Redux make it easier to understand when,where, why, and how the state in your application is being updated, and how your application logic will behave when
those changes occur**. Redux guides you towards writing code that is predictable
and testable, which helps give you confidence that your application will work as
expected.

Because any component can access data that is being kept in the Redux store, it
can be used to avoid "prop-drilling". However, this is an added benefit, and not
specifically the main reason to use Redux.

### Tradeoffs

Redux asks you to write your code following specific patterns:

- Application state should be described using plain JS objects and arrays
- There must be a single Redux store that holds the application state
- The store can only be updated by describing events that happen as plain "action"
objects, and "dispatching" them to the store for processing
- The logic for updating the store state must be written as "reducer" functions,
which look at the current state and the action object to decide what the resulting
new state should be. Reducer functions must be "pure", with no side effects, and
must calculate the new state using immutable updates.

This adds a level of indirection to the process of managing state. Because of
this, Redux is not intended to be the shortest or fastest way to update state.

However, the indirection of separating descriptions of "what happened" from the
logic that determines "how the state should be updated" allows Redux to track how
the state changed every time an action is dispatched. The Redux DevTools can then
display the history of dispatched actions, the diff between states, and the complete
state tree after each dispatched action. This makes it easier to understand how
the application behaves as things change. This also enables Redux "middleware",
which can inspect dispatched actions and run additional logic in response.

Because Redux is a single store, all components that have subscribed to reading
state updates have to re-run comparison checks after every dispatched action, to
see if the data needed by that specific component has changed. This does require
that users carefully define how each component reads data from the Redux store,
including use of "memoized" selector functions and semi-manual optimizations. It
also means that it's generally not a good idea to put most UI state values like
"is this dropdown open?" or form state into the Redux store.

Redux is UI-agnostic and can be used with any UI layer, including React, Angular,
Vue, and vanilla JS.

### When Should I Consider Using This?

Redux helps you deal with shared state management, but there are more concepts
to learn, and more code to write. It also adds some indirection to your code,
and asks you to follow certain restrictions. It's a trade-off between short term
and long term productivity.

**Redux is more useful when:**

- You have large amounts of application state that are needed in many places in
the app
- The app state is updated semi-frequently
- The logic to update that state may be complex
- The app has a medium or large-sized codebase, and might be worked on by many people
- You want to see a history of changes to your application state over time
- You want to write as much of your logic as possible separate from the UI layer

**You should probably avoid using Redux if:**

- You prefer an Object-Oriented approach instead of a Functional Programming
approach
- Your app mostly depends on fetching and caching data from the server and you
are using another tool to manage that cached server state
- You want UI updates and data dependencies to be as automatic as possible, with
minimal manual optimizations
- You prefer writing logic that directly applies state updates with minimal
indirection
- Most of the data in your application is only needed by specific subsections of
the component tree, with little "global" state
- You want to stick with built-in React APIs and avoid additional third-party
dependencies

### Redux is too much boilerplate, how all of this came about?

There are very good reasons for it. Even from the beginning like, some of the
very early documentation it was said _"Redux is not intended to be the shortest
way to write code, it's meant to make your code predictable" ([see reference - Discussions (2dn Section)](https://redux.js.org/faq/general))_

The maintainer has described that that fact could be better described in two
things:

1. Inherit complexity (Stuff that is built into the thing that cannot be avoided,
that's how it works)
2. Incidental complexity (Stuff that kind of comes along with it but isn't neccesary)

So, some of this is stuff that is more work because that's the way redux was
designed. There's something called _indirection_ which is the process of having
to define action objects, and dispatch them, and write your code separately.
For that matter, Redux expects that you update your state **_immutably_**, which
means to have to make copies of all the different objects and arrays and that's
not how JavaScript works ny default so, it takes more effort to make a copy of a
thing.

On top of that, there were a bunch of different reasons people adopted Redux in
2015-2016:

1. It was the best of the Flux Library that came out at that time.
2. React, didn't have a really good solution for avoiding the _prop drilling_
problem, avoiding passing data through many layers of the component tree.

Since then, a lot of things have changed:

1. React developed its own native solution to solve the _prop drilling_ problem.
2. There are other many good library and ecosystems that solve some of the same
kinds of problems.

# Difference Between ReactNode and JSXElement

 - **JSX.Element**

    Represents a single React element created using JSX. It's a more specific
    type that corresponds to what React.createElement returns. Typically used
    when you're expecting a single element to be rendered.

    ```TypeScript
      const element: JSX.Element = <h1>Hello, world!</h1>;
    ```

 - **React.Node**

  A broader type that can represent anything that React can render.

  It includes:

  - JSX.Element
  - Strings (e.g., "text")
  - Numbers (e.g., 123)
  - Booleans (true or false are ignored by React)
  - null or undefined (representing no rendering)
  - Arrays of other ReactNode elements
  - React.Fragment

  ```TypeScript
    const node: React.ReactNode = (
      <>
        <h1>Hello, world!</h1>
        <p>This is a paragraph.</p>
        {"Some text"}
      </>
    );
  ```

  **Key Differences:**

  1. JSX.Element is used when you want to be strict about the type and ensure
  only a single React element is accepted.
  2. ReactNode is more flexible and can represent any renderable output,
  including multiple elements, text, arrays, and even null.

# Queueing a Series of State Updates

Read [this documentation](https://react.dev/learn/queueing-a-series-of-state-updates).

# Development Patterns

### Presentational and Container Components

  - _Presentational Components_: These are primarily concerned with how things look.
  They receive data and callbacks exclusively via props and don’t have their own
  state (except for UI state).

  - _Container Components_: These are concerned with how things work. They handle
  state, business logic, and data fetching, passing data down to presentational
  components as props.

  Example:

    - Presentational: Button, Card
    - Container: UserListContainer (fetches users, manages state)

### Higher Order Components

A function that takes a component and returns a new component with added props or behavior. This pattern helps in sharing common functionality between components.

Use Cases: Authentication checks, logging, data fetching, etc.

### Render Props

A pattern where a component accepts a function as a prop. This function returns React elements, allowing you to dynamically decide what to render.

Example: Sharing state between components, custom input handlers, etc.

Code Example:

```JavaScript
const MouseTracker = ({ render }) => {
  const [position, setPosition] = useState({ x: 0, y: 0 });

  const handleMouseMove = (event) => {
    setPosition({ x: event.clientX, y: event.clientY });
  };

  return (
    <div onMouseMove={handleMouseMove}>
      {render(position)}
    </div>
  );
};

<MouseTracker render={({ x, y }) => (
  <p>The mouse position is ({x}, {y})</p>
)} />
```

### Controlled vs. Uncontrolled Components

  - Controlled Components: The React component controls the form input values via state.
  - Uncontrolled Components: Form inputs maintain their own state. You access values via refs.

Use Case: For simple forms, uncontrolled components can be used. For more complex forms, controlled components offer better control and validation.

### Custom Hooks

With React Hooks, custom hooks allow you to extract reusable logic into separate functions. This pattern has become more common than HOCs or Render Props in modern React.

Use Case: Handling forms, data fetching, authentication, subscriptions, etc.

### Compound Components

Components that are designed to work together, giving flexibility to the user of the component. It allows developers to create components that manage their internal state while still being configurable by the user.

Use Case: Custom dropdowns, tabs, modals, etc.

Example:

```JavaScript
const Tabs = ({ children }) => {
  const [activeIndex, setActiveIndex] = useState(0);

  return (
    <div>
      {React.Children.map(children, (child, index) =>
        React.cloneElement(child, { activeIndex, setActiveIndex, index })
      )}
    </div>
  );
};
```

### Portals

Read [this documentation](https://react.dev/reference/react-dom/createPortal).

Also, take a look at this [video tutorial](https://www.youtube.com/watch?v=lDAmVwY-oPU).

### Controlled vs. Uncontrolled Input Components

  - Controlled: Component state drives the input value.

  - Uncontrolled: Native HTML form elements control their state via refs.

Example: Forms can be handled using either approach, but controlled components are usually preferred for complex forms.

### Error Boundaries

A way to catch JavaScript errors anywhere in the component tree and provide a fallback UI. Error boundaries are created by implementing the componentDidCatch and getDerivedStateFromError lifecycle methods.

Use Case: Wrapping components that may fail, such as third-party widgets or components with unpredictable external dependencies.

### Functional Components & Hooks

In modern React, functional components and hooks have replaced many class-based patterns. Hooks allow for logic reuse, state management, and side-effects management without classes.

- Key Hooks: useState, useEffect, useContext, useReducer, useMemo, useCallback, and useRef.