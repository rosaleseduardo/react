<div align = "center">
  <img
    src = "./assets/logo-react.jpg"
    width = "70%"
    alt = "reactjs-logo" />
</div>

<hr>

## Table of Contents

- [A (Mostly) Complete Guide to React Rendering Behavior](#a-mostly-complete-guide-to-react-rendering-behavior)
- [Design Patterns](#design-patterns)
  - [Context API](#context-api)
  - [Redux](#redux)
    - [What is it?](#what-is-it)
    - [Purpose and Use Cases](#purpose-and-use-cases)
    - [Tradeoffs](#tradeoffs)
    - [When Should I Consider Using This?](#when-should-i-consider-using-this)
    - [Redux is too much boilerplate, how all of this came about?](#redux-is-too-much-boilerplate-how-all-of-this-came-about)

# A (Mostly) Complete Guide to React Rendering Behavior

- [A (Mostly) Complete Guide to React Rendering Behavior](https://blog.isquaredsoftware.com/2020/05/blogged-answers-a-mostly-complete-guide-to-react-rendering-behavior/)

# Design Patterns

- [A better way of solving prop drilling in React apps](https://blog.logrocket.com/solving-prop-drilling-react-apps/)
- [What is Context API and useContext? Everything You Need to Know](https://www.alphabold.com/what-is-context-api-and-usecontext/)

## Context API

## Redux

### What is it?

Redux is a pattern and library for managing and updating application state, using events called "actions". It serves as a centralized store for state that needs to be used across your entire application, with rules ensuring that the state can only be updated in a predictable fashion.

Redux focuses on making state updates predictable and traceable, with inspiration from the [Flux Architecture](https://facebook.github.io/flux/docs/in-depth-overview/) pattern and Functional Programming principles. It relies on separating descriptions of "what happened", called "actions", from the logic that decides how state should be updated, called "reducer functions". Redux centralizes global application state into a single "store", and provides browser DevTools to view the history of state changes over time.

### Purpose and Use Cases

**The primary purpose of Redux is to help you manage "global" state** — state that is needed across many parts of your application.

**The patterns and tools provided by Redux make it easier to understand when, where, why, and how the state in your application is being updated, and how your application logic will behave when those changes occur**. Redux guides you towards writing code that is predictable and testable, which helps give you confidence that your application will work as expected.

Because any component can access data that is being kept in the Redux store, it can be used to avoid "prop-drilling". However, this is an added benefit, and not specifically the main reason to use Redux.

### Tradeoffs

Redux asks you to write your code following specific patterns:

- Application state should be described using plain JS objects and arrays
- There must be a single Redux store that holds the application state
- The store can only be updated by describing events that happen as plain "action" objects, and "dispatching" them to the store for processing
- The logic for updating the store state must be written as "reducer" functions, which look at the current state and the action object to decide what the resulting new state should be. Reducer functions must be "pure", with no side effects, and must calculate the new state using immutable updates.

This adds a level of indirection to the process of managing state. Because of this, Redux is not intended to be the shortest or fastest way to update state.

However, the indirection of separating descriptions of "what happened" from the logic that determines "how the state should be updated" allows Redux to track how the state changed every time an action is dispatched. The Redux DevTools can then display the history of dispatched actions, the diff between states, and the complete state tree after each dispatched action. This makes it easier to understand how the application behaves as things change. This also enables Redux "middleware", which can inspect dispatched actions and run additional logic in response.

Because Redux is a single store, all components that have subscribed to reading state updates have to re-run comparison checks after every dispatched action, to see if the data needed by that specific component has changed. This does require that users carefully define how each component reads data from the Redux store, including use of "memoized" selector functions and semi-manual optimizations. It also means that it's generally not a good idea to put most UI state values like "is this dropdown open?" or form state into the Redux store.

Redux is UI-agnostic and can be used with any UI layer, including React, Angular, Vue, and vanilla JS.

### When Should I Consider Using This?

Redux helps you deal with shared state management, but there are more concepts to learn, and more code to write. It also adds some indirection to your code, and asks you to follow certain restrictions. It's a trade-off between short term and long term productivity.

**Redux is more useful when:**

- You have large amounts of application state that are needed in many places in the app
- The app state is updated semi-frequently
- The logic to update that state may be complex
- The app has a medium or large-sized codebase, and might be worked on by many people
- You want to see a history of changes to your application state over time
- You want to write as much of your logic as possible separate from the UI layer

**You should probably avoid using Redux if:**

- You prefer an Object-Oriented approach instead of a Functional Programming approach
- Your app mostly depends on fetching and caching data from the server and you are using another tool to manage that cached server state
- You want UI updates and data dependencies to be as automatic as possible, with minimal manual optimizations
- You prefer writing logic that directly applies state updates with minimal indirection
- Most of the data in your application is only needed by specific subsections of the component tree, with little "global" state
- You want to stick with built-in React APIs and avoid additional third-party dependencies

### Redux is too much boilerplate, how all of this came about?

There are very good reasons for it. Even from the beginning like, some of the very early documentation it was said _"Redux is not intended to be the shortest way to write code, it's meant to make your code predictable" ([see reference - Discussions (2dn Section)](https://redux.js.org/faq/general))_

The maintainer has described that that fact could be better described in two things:

1. Inherit complexity (Stuff that is built into the thing that cannot be avoided, that's how it works)
2. Incidental complexity (Stuff that kind of comes along with it but isn't neccesary)

So, some of this is stuff that is more work because that's the way redux was designed. There's something called _indirection_ which is the process of having to define action objects, and dispatch them, and write your code separately. For that matter, Redux expects that you update your state **_immutably_**, which means to have to make copies of all the different objects and arrays and that's not how JavaScript works ny default so, it takes more effort to make a copy of a thing.

On top of that, there were a bunch of different reasons people adopted Redux in 2015-2016:

1. It was the best of the Flux Library that came out at that time.
2. React, didn't have a really good solution for avoiding the _prop drilling_ problem, avoiding passing data through many layers of the component tree.

Since then, a lot of things have changed:

1. React developed its own native solution to solve the _prop drilling_ problem.
2. There are other many good library and ecosystems that solve some of the same kinds of problems.
