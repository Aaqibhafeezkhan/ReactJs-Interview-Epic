# ⚛️ React.js Interview Epic

> **An exhaustive, continuously updated React interview question bank — from junior fundamentals to staff-level architecture, with verified answers and modern React 19.2 coverage.**

[![React](https://img.shields.io/badge/React-19.2-61DAFB?logo=react&logoColor=white)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-current-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)

## What this repository is

This is intended to become a **living React interview encyclopedia**, not a collection of trivia.

It covers:

- JavaScript and browser fundamentals that React interviews depend on
- React fundamentals and mental models
- JSX, components, props, state and rendering
- Hooks and the Rules of Hooks
- Effects and synchronization
- Context, reducers and state architecture
- Refs and imperative APIs
- Forms and user input
- Suspense, transitions and concurrent rendering concepts
- React 19 and 19.2 APIs
- React Compiler
- React Server Components and Server Functions
- SSR, streaming, hydration and prerendering
- React Router and application routing concepts
- Next.js / framework integration
- TypeScript with React
- API/data fetching and caching
- Performance and profiling
- Accessibility
- Testing
- Security
- Build tooling and deployment
- Architecture and design patterns
- Debugging and production incidents
- Frontend system-design interviews
- Coding exercises and machine-coding prompts
- Senior/staff-level trade-off questions
- Behavioral and project-deep-dive questions

> **Verification policy:** Answers are written from primary documentation and standards wherever possible. React-specific claims should be checked against the current React documentation at [react.dev](https://react.dev/). As of **August 27, 2026**, the React documentation identifies **React 19.2 as the latest version**. Do not treat this README as immutable: React evolves and answers should be reviewed when React releases materially change behavior.

---

## Table of Contents

1. [Current React Baseline](#current-react-baseline)
2. [How to Use This Epic](#how-to-use-this-epic)
3. [JavaScript Prerequisites](#javascript-prerequisites)
4. [Browser & Web Fundamentals](#browser--web-fundamentals)
5. [React Fundamentals](#react-fundamentals)
6. [JSX & Rendering](#jsx--rendering)
7. [Props, State & Data Flow](#props-state--data-flow)
8. [Hooks](#hooks)
9. [Effects](#effects)
10. [Refs & DOM](#refs--dom)
11. [Context & State Architecture](#context--state-architecture)
12. [Forms](#forms)
13. [Suspense, Transitions & Scheduling](#suspense-transitions--scheduling)
14. [React 19+](#react-19)
15. [React Compiler](#react-compiler)
16. [Server Components & Server Functions](#server-components--server-functions)
17. [SSR, Hydration & Streaming](#ssr-hydration--streaming)
18. [Routing](#routing)
19. [TypeScript + React](#typescript--react)
20. [Data Fetching & APIs](#data-fetching--apis)
21. [Performance](#performance)
22. [Accessibility](#accessibility)
23. [Testing](#testing)
24. [Security](#security)
25. [Tooling & Build Systems](#tooling--build-systems)
26. [Architecture & Design Patterns](#architecture--design-patterns)
27. [Debugging & Production](#debugging--production)
28. [Frontend System Design](#frontend-system-design)
29. [Coding / Machine-Coding Questions](#coding--machine-coding-questions)
30. [Senior & Staff Questions](#senior--staff-questions)
31. [Behavioral & Project Questions](#behavioral--project-questions)
32. [Rapid-Fire Questions](#rapid-fire-questions)
33. [Interview Preparation Matrix](#interview-preparation-matrix)
34. [Source & Verification Policy](#source--verification-policy)

---

# Current React Baseline

The baseline for this epic is modern React, not legacy React interview folklore.

- **Latest documented React version:** 19.2.
- React documentation is organized around components, Hooks, APIs, built-in components, React DOM APIs and React Server Components.
- React Compiler is a stable build-time optimization tool and can automatically perform memoization in supported code.
- React Server Components are stable in React 19, while the underlying bundler/framework implementation APIs are explicitly subject to change and should be pinned appropriately.
- React's current documentation distinguishes **Server Functions** from the older informal term **Server Actions**; a Server Function is not necessarily an action prop.

Primary references:

- https://react.dev/learn
- https://react.dev/reference/react
- https://react.dev/versions
- https://react.dev/blog/2025/10/01/react-19-2
- https://react.dev/learn/react-compiler
- https://react.dev/reference/rsc/server-components
- https://react.dev/reference/rsc/server-functions

---

# How to Use This Epic

### Junior

Master sections 3–8, then practice sections 21–23.

### Mid-level

Master the whole React core, TypeScript, data fetching, performance, testing, accessibility and architecture sections.

### Senior

Add SSR/RSC, concurrency, production debugging, security, system design and trade-off questions.

### Staff / Lead

Be able to explain **why**, not merely **how**: architecture boundaries, organizational constraints, migration strategies, observability, performance budgets, failure modes and long-term maintenance.

### The golden interview rule

For almost every React question, answer in this order:

1. **Definition** — what it is.
2. **Mental model** — how it works.
3. **Example** — minimal code or scenario.
4. **Trade-offs** — when it helps and when it hurts.
5. **Modern React caveat** — mention current React behavior where relevant.

---

# JavaScript Prerequisites

React interviews frequently become JavaScript interviews. You should be able to answer these without looking anything up.

### 1. What is lexical scope?

Lexical scope means variable visibility is determined by where code is written, not where a function is called. Closures preserve access to variables from their lexical environment.

### 2. What is a closure?

A closure is a function together with access to variables from its surrounding lexical environment. React relies heavily on closures because components and Hooks are JavaScript functions.

### 3. `var` vs `let` vs `const`?

`var` is function-scoped and has legacy hoisting behavior. `let` and `const` are block-scoped. `const` prevents reassignment of the binding but does not make an object immutable.

### 4. What is the temporal dead zone?

The period between entering a scope and initialization of a `let`, `const`, or `class` binding during which accessing the binding throws a `ReferenceError`.

### 5. What is the event loop?

JavaScript runs synchronous work on a call stack. Asynchronous work is scheduled through host mechanisms and queues; the event loop coordinates when queued callbacks can run after the stack is available.

### 6. Microtask vs task/macrotask?

Promise reactions and `queueMicrotask` use the microtask queue. APIs such as timers generally enqueue tasks. Microtasks are processed before the browser proceeds to the next task/rendering opportunity under the host's event-loop rules.

### 7. What is a Promise?

A Promise represents the eventual completion or failure of an asynchronous operation and has pending, fulfilled and rejected states.

### 8. `async`/`await`?

`async` functions return Promises. `await` pauses execution of that async function until the awaited value settles; it does not block the JavaScript thread.

### 9. `Promise.all` vs `allSettled`?

`Promise.all` fulfills only when all inputs fulfill and rejects when one rejects. `Promise.allSettled` waits for every input and reports each outcome.

### 10. What is referential equality?

Objects, arrays and functions are compared by identity, not structural contents. This matters because React dependency arrays and memoization commonly use `Object.is`-based comparisons.

### 11. Shallow vs deep copy?

A shallow copy copies only the outer structure; nested references remain shared. A deep copy recursively duplicates nested data, subject to the semantics of the chosen cloning method.

### 12. Why is immutability useful in React?

Immutable updates make state transitions easier to reason about and allow identity comparisons to detect changed references. Immutability is a design discipline; JavaScript does not make objects immutable automatically.

### 13. What is debouncing?

Debouncing delays execution until calls stop for a specified period. It is useful for inputs such as search boxes.

### 14. What is throttling?

Throttling limits how frequently work executes during a burst of events, such as scroll or resize events.

### 15. What is prototype inheritance?

JavaScript objects can inherit properties and methods through a prototype chain. `class` syntax is largely syntactic abstraction over JavaScript's prototype-based object model.

### 16. What is `this`?

`this` is determined by how a function is called, except for arrow functions, which capture lexical `this`. React function components do not require the class-component `this` model.

### 17. Why are arrow functions commonly used in React?

They provide concise function syntax and lexical `this`, and are convenient for callbacks and function components. They are not inherently more performant than regular functions.

### 18. What is optional chaining?

`obj?.a?.b` safely stops evaluation when an intermediate value is `null` or `undefined`.

### 19. What is nullish coalescing?

`a ?? b` uses `b` only when `a` is `null` or `undefined`, unlike `a || b`, which treats all falsy values as absent.

### 20. What is a generator?

A generator function can pause and resume execution and produces an iterator. Generators are not a core React mechanism but appear in some state-management ecosystems.

---

# Browser & Web Fundamentals

### 21. What happens after entering a URL?

A browser resolves the hostname, establishes a connection using the appropriate protocol, sends a request, receives resources, parses them, constructs the DOM/CSSOM, performs style/layout/paint work and executes JavaScript as applicable. Modern protocols and caching make the real path more nuanced.

### 22. DOM vs virtual DOM?

The DOM is the browser's actual document object model. React maintains its own representation of the UI and reconciles changes before committing necessary host updates; “virtual DOM” is an informal description, not the entire React architecture.

### 23. What is layout/reflow?

Layout determines geometric relationships and positions of elements. Changes affecting geometry can trigger layout work.

### 24. What is paint?

Painting turns styled layout information into pixels. Compositing may then combine layers for display.

### 25. What is CORS?

Cross-Origin Resource Sharing is a browser security mechanism that lets servers declare which cross-origin requests are permitted.

### 26. Cookies vs localStorage vs sessionStorage?

Cookies can be sent with HTTP requests and have security attributes such as `HttpOnly`, `Secure` and `SameSite`. `localStorage` persists per origin until cleared; `sessionStorage` is scoped to a browsing session/tab context. Web Storage is accessible to JavaScript and therefore should not hold secrets that must be protected from XSS.

### 27. What is HTTP caching?

Browsers and intermediaries can reuse responses according to HTTP caching directives such as `Cache-Control`, validators such as `ETag`, and freshness rules.

### 28. What is an SPA?

A single-page application typically loads an application shell and updates views client-side without full document navigations for every route.

### 29. SPA vs MPA?

An MPA generally navigates between server-delivered documents. An SPA performs more client-side navigation and rendering. Modern frameworks can combine server rendering, streaming and client interactivity, so the distinction is no longer binary.

### 30. What is progressive enhancement?

Start with a functional baseline and enhance it with richer browser capabilities and JavaScript. It improves resilience and accessibility when applied appropriately.

---

# React Fundamentals

### 31. What is React?

React is a JavaScript library for building user interfaces from components. The current React documentation emphasizes components, state, effects, Hooks, APIs and rendering rather than treating React as a complete application framework.

### 32. What is a component?

A component is a reusable unit of UI and logic. Modern React components are commonly JavaScript functions returning React nodes.

### 33. Function component vs class component?

Function components are the modern default and support Hooks. Class components remain supported, but new code generally uses function components.

### 34. What is JSX?

JSX is a syntax extension that lets JavaScript code express UI markup-like structures. It is transformed by tooling into JavaScript expressions used to create React elements.

### 35. Why must component names start with uppercase?

JSX uses capitalization to distinguish user-defined components from intrinsic HTML/SVG elements. `<Button />` refers to a component while `<button>` refers to a host element.

### 36. What is a React element?

A React element is a description of what React should render. It is not the DOM node itself.

### 37. What is reconciliation?

Reconciliation is React's process of determining how a new rendered tree relates to a previous tree and what work is needed before committing host updates.

### 38. What is the render phase?

React calls components and calculates what the UI should look like. Rendering should be free of externally visible side effects because React may render more than once or abandon work.

### 39. What is the commit phase?

React applies the result of rendering to the host environment and runs appropriate commit-related effects. DOM mutations occur during commit.

### 40. What does “render” mean in React?

Rendering means React calls components to determine the next UI description. It does not necessarily mean the browser DOM changed.

### 41. What causes a component to render?

Common causes include initial mounting, state updates, parent renders, context changes and external-store updates. React may also render work speculatively under its scheduling model.

### 42. Does a parent render always mean the DOM changes?

No. A render calculates a new result; reconciliation determines whether host updates are necessary.

### 43. What are keys?

Keys provide stable identity for siblings in a collection. They help React match previous and next children correctly.

### 44. Why is array index a poor key?

When list order or membership changes, an index can identify a different item than before, causing state and DOM identity to move unexpectedly. Stable item IDs are preferable.

### 45. Can keys be globally unique?

No. Keys need to be unique among siblings in the relevant list, not globally across the application.

### 46. What is a Fragment?

A Fragment groups multiple children without adding an extra DOM element. It can be written as `<>...</>` or `<Fragment>...</Fragment>`.

### 47. What is a portal?

A portal renders React children into a different DOM node while keeping them in the same React tree. This is useful for modals, tooltips and overlays.

### 48. What is composition?

Composition builds complex UIs by passing components/elements as children or props rather than relying on inheritance.

### 49. Why is composition preferred over inheritance in React?

React's component model naturally supports composition, which keeps behavior and UI relationships explicit and reusable.

### 50. What does “pure component” mean for function components?

A pure component behaves predictably for the same inputs and avoids side effects during render. React's current documentation strongly emphasizes keeping components and Hooks pure.

---

# JSX & Rendering

### 51. Can JSX return multiple elements?

Yes, if they are grouped by a Fragment or another parent.

### 52. Why does JSX use `className`?

`class` conflicts with the JavaScript language keyword context and React historically uses the DOM property-oriented name `className` for CSS classes in JSX.

### 53. How do you conditionally render?

Use normal JavaScript constructs such as `if`, conditional expressions, logical operators and early returns.

### 54. What is the danger of `condition && <Component />`?

If `condition` can be a value such as `0`, JavaScript can return that value into the rendered output. Explicit boolean checks can be safer when the value domain is not strictly boolean.

### 55. How do you render a list?

Use JavaScript iteration such as `map()` and provide stable keys for each sibling.

### 56. Can a component return `null`?

Yes. It renders nothing in that position.

### 57. What can React render?

React can render elements, strings, numbers, arrays/iterables of nodes, portals and `null`/boolean-like empty nodes, subject to the API and host environment.

### 58. What is hydration?

Hydration attaches React behavior to HTML that was already rendered, typically on the server. The client must reconcile against the server-produced structure.

### 59. What causes hydration mismatch warnings?

Server and client can produce different markup due to non-deterministic rendering, environment-dependent values, time/randomness, invalid nesting, data differences or incompatible component boundaries.

### 60. Should you suppress hydration warnings casually?

No. Suppression is an escape hatch for narrowly understood differences; it should not replace fixing the underlying mismatch.

---

# Props, State & Data Flow

### 61. Props vs state?

Props are inputs supplied by a parent. State is component-owned data that can trigger another render when updated.

### 62. Are props immutable?

A component should treat props as read-only inputs. A child should communicate changes through callbacks or shared state rather than mutating props.

### 63. Why should state updates be immutable?

Immutable updates preserve predictable state transitions and reference identity. Mutating an existing state object can prevent expected updates or create difficult-to-debug shared state.

### 64. What is lifting state up?

Move shared state to the nearest common ancestor that needs to coordinate it, then pass data and callbacks down.

### 65. What is prop drilling?

Passing props through intermediate components that do not otherwise need them. It can be acceptable for small, explicit trees; context or architectural changes may be better when it becomes pervasive.

### 66. When should state live locally?

Keep state as close as practical to the components that own and use it. Do not globalize state merely for convenience.

### 67. When should state be global?

Use shared/global state when many distant parts of the application need coordinated access and the state has a meaningful application-level lifecycle.

### 68. What is derived state?

A value computed from existing props/state. Prefer calculating it during render when possible rather than storing duplicate state.

### 69. Why avoid redundant state?

Duplicated sources of truth can drift out of sync and require synchronization logic.

### 70. How should you update state based on previous state?

Use the functional updater form, e.g. `setCount(c => c + 1)`, when the next value depends on the previous value.

### 71. What is batching?

React can group multiple state updates so they result in fewer renders/commits. Modern React batches updates across more asynchronous contexts than older React versions did.

### 72. Does calling a setter immediately mutate the current state variable?

No. A state setter requests an update; the current render's state value remains the value captured by that render.

### 73. Why can state appear “stale” inside a callback?

Functions close over values from the render in which they were created. Use functional updates, correct dependencies, refs or other appropriate patterns depending on the problem.

### 74. What is controlled state?

State whose value is controlled by a parent or another external owner and passed into a component as a prop.

### 75. What is uncontrolled state?

State maintained internally by a component or DOM element, often accessed through refs when necessary.

---

# Hooks

### 76. What is a Hook?

A Hook is a React API that lets function components use React features such as state, context, refs, effects and transitions.

### 77. What are the Rules of Hooks?

Hooks should be called at the top level of React function components or custom Hooks, not conditionally, inside loops, or from arbitrary nested functions.

### 78. Why can't Hooks be called conditionally?

React relies on consistent Hook call ordering between renders to associate Hook state correctly.

### 79. What is `useState`?

A Hook that adds state to a component and returns the current state plus a setter.

### 80. What is `useReducer`?

A Hook for state transitions represented by a reducer function and dispatched actions. It is useful when state logic is complex or has multiple related transitions.

### 81. `useState` vs `useReducer`?

Use `useState` for straightforward local state. `useReducer` can make complex transition logic explicit and testable. Neither is inherently “more performant.”

### 82. What is `useContext`?

It reads the current value from a matching context provider.

### 83. What is `useRef`?

It returns a mutable ref object whose `.current` value persists across renders without itself causing a re-render when changed.

### 84. What is `useMemo`?

It memoizes a calculated value between renders based on dependencies. It should be used when there is a demonstrated need or when precise memoization is useful—not as a default wrapper around every calculation.

### 85. What is `useCallback`?

It memoizes a function identity based on dependencies. It is useful when stable function identity matters, such as interacting with memoized children or dependencies.

### 86. What is `useEffect`?

`useEffect` synchronizes a component with an external system after rendering. React's documentation specifically cautions against using Effects for ordinary derived data or event-specific logic.

### 87. What is `useLayoutEffect`?

It runs during the commit sequence before the browser paints, making it appropriate for certain layout measurements or synchronous visual adjustments. It can block painting, so it should be used carefully.

### 88. What is `useInsertionEffect`?

A specialized Hook primarily intended for CSS-in-JS library authors to insert styles at the appropriate point. Application code rarely needs it.

### 89. What is `useId`?

It generates stable IDs suitable for associating related elements, particularly accessibility attributes. It is not intended as a list key or general-purpose unique ID generator.

### 90. What is `useImperativeHandle`?

It customizes the value exposed through a ref, allowing a component to expose a narrow imperative API rather than its entire DOM implementation.

### 91. What is `useDebugValue`?

It lets custom Hooks provide developer-facing labels in React DevTools.

### 92. What is `useSyncExternalStore`?

A Hook for subscribing to external stores in a way that integrates correctly with React's rendering model, including concurrent rendering concerns.

### 93. What is `useTransition`?

It lets you mark updates as non-urgent transitions so React can keep urgent interactions responsive while rendering lower-priority work.

### 94. What is `useDeferredValue`?

It lets a component defer updating a non-critical part of the UI while urgent updates proceed.

### 95. What is `use`?

`use` can read a resource such as a Promise or context during rendering in supported React patterns. Unlike ordinary Hooks, it has different call-site rules and can be used conditionally in documented scenarios.

### 96. What is a custom Hook?

A JavaScript function whose name conventionally begins with `use` and that composes React Hooks to share stateful logic between components.

### 97. Does a custom Hook share state automatically?

No. Each invocation gets its own Hook state. A custom Hook shares logic, not a singleton state instance.

---

# Effects

### 98. What should an Effect be used for?

Synchronizing React with something outside React: subscriptions, browser APIs, network connections, imperative widgets and similar external systems.

### 99. When should you NOT use `useEffect`?

Do not use it merely to derive a value from props/state, transform data for rendering, or respond to a user event that can be handled directly by the event handler.

### 100. What is the dependency array?

It declares reactive values that the Effect reads and whose changes should cause the Effect to resynchronize.

### 101. Empty dependency array: what does it mean?

It means the Effect has no declared reactive dependencies. It is not a universal “run once forever” guarantee; development behavior, remounts and component lifecycle still matter.

### 102. What does an Effect cleanup do?

It undoes the synchronization established by the Effect—for example, unsubscribing, disconnecting or aborting an operation.

### 103. Why can an Effect appear to run twice in development?

React Strict Mode may intentionally perform extra development-only setup/cleanup cycles to expose effects that are not resilient to remounting or missing cleanup.

### 104. How do you avoid race conditions in fetching Effects?

Use cancellation such as `AbortController` where supported and ensure stale results cannot overwrite newer state. Prefer a data-fetching framework/library when the application requires caching, deduplication and sophisticated server-state management.

### 105. What is an Effect Event?

React 19.2 introduced `useEffectEvent`, which provides a way to separate non-reactive event-like logic from an Effect while allowing it to read the latest values. Use the documented semantics rather than treating it as a general dependency-array escape hatch.

---

# Refs & DOM

### 106. Ref vs state?

State participates in rendering and updates schedule renders. A ref persists mutable data across renders without triggering a render when changed.

### 107. When should you use a ref?

For imperative DOM access, storing mutable values that do not affect rendering, timers/handles and integration with imperative APIs.

### 108. Should refs replace state?

No. If a value determines what should be rendered, it normally belongs in state or derived data.

### 109. What is `forwardRef`?

Historically, `forwardRef` allowed a function component to receive and forward a ref. React 19 changes the model: `ref` is available as a prop for function components, and the current documentation notes that `forwardRef` is no longer necessary for new React 19 code and is planned for deprecation.

### 110. Callback refs vs object refs?

Object refs are convenient for persistent `.current` values. Callback refs run when React attaches/detaches the ref and can be useful when setup needs to react to node changes.

---

# Context & State Architecture

### 111. What problem does Context solve?

It lets data be consumed by components without manually passing props through every intermediate component.

### 112. Is Context a state-management library?

No. Context provides value propagation. The state itself can live in `useState`, `useReducer`, an external store or another mechanism.

### 113. Does Context cause all components to re-render?

Consumers whose observed context value changes can re-render. Non-consumers are not automatically re-rendered merely because a provider updates, although parent rendering and other dependencies can still matter.

### 114. How do you reduce unnecessary Context renders?

Split contexts by concern, keep provider values stable where appropriate, avoid putting frequently changing unrelated data into one context, and use external-store patterns when subscription granularity requires them.

### 115. Redux vs Context?

Context solves dependency/value propagation. Redux is a state-management architecture with centralized state, actions, reducers, middleware/tooling and subscription semantics. They solve overlapping but different problems.

### 116. What is a reducer?

A pure function that takes current state and an action and returns the next state.

### 117. Why must reducers be pure?

Predictable reducers are easier to test, reason about, replay and debug. Side effects belong outside reducer logic.

### 118. What is normalized state?

Representing entities by stable IDs and relationships rather than deeply duplicating the same object in many locations. It can simplify updates and consistency.

### 119. What is server state vs client state?

Server state is owned by a remote system and requires fetching, caching, synchronization and invalidation. Client state represents UI/application concerns such as open dialogs, selected tabs or local preferences. Treating them identically often creates unnecessary complexity.

---

# Forms

### 120. Controlled vs uncontrolled input?

A controlled input gets its value from React state. An uncontrolled input keeps its current value in the DOM and can be accessed through refs or form APIs.

### 121. When are uncontrolled forms useful?

For simple forms, performance-sensitive large forms, integration with native form behavior, or when a library uses uncontrolled primitives.

### 122. How do you validate a form?

Validate at the appropriate layers: client-side validation for immediate UX, server-side validation for correctness and security. Client validation must never be treated as the security boundary.

### 123. How do you handle file uploads?

Use file inputs/FormData or a suitable upload protocol, validate file type/size/content server-side, and avoid trusting client-provided metadata.

### 124. How do you prevent duplicate form submissions?

Disable or gate the submit action while appropriate, use idempotency keys for server mutations where necessary, and make the UI reflect the request lifecycle.

---

# Suspense, Transitions & Scheduling

### 125. What is Suspense?

Suspense lets React coordinate rendering around components that suspend, showing a fallback while the required content is not ready.

### 126. Is Suspense just a loading spinner component?

No. A `<Suspense>` boundary is a rendering coordination mechanism. The fallback is only one visible consequence.

### 127. What is a Suspense boundary?

A component boundary that determines what UI React can show while child content suspends.

### 128. What is a transition?

A transition marks updates as non-urgent so React can prioritize more urgent interactions.

### 129. Transition vs debounce?

A transition changes React update priority. Debouncing delays work based on time. They solve different problems and can sometimes be combined.

### 130. What is concurrent rendering?

React's scheduler can prepare rendering work in an interruptible way, allowing higher-priority work to take precedence. It does not mean JavaScript rendering happens on multiple threads.

### 131. Can rendering be interrupted?

React's concurrent rendering model allows render work to be interrupted, restarted or abandoned before commit. Therefore render logic must remain pure.

### 132. What is the key mental model for concurrent React?

**Render is a calculation; commit is the externally visible mutation.** Never rely on a render having committed just because a component function ran.

---

# React 19+

### 133. What are important React 19 features?

React 19 introduced major capabilities around Actions/forms, `use`, Server Components/Server Functions integration, ref-as-prop behavior and other improvements. Always distinguish core React APIs from framework-specific features.

### 134. What was added in React 19.1?

React 19.1 included improvements across React and React DOM, including additional APIs and developer experience changes. For interview answers, verify the exact API against the version-specific release notes rather than relying on memory.

### 135. What is React 19.2?

React 19.2 is the current documented latest version in this epic's baseline. It added `<Activity />`, `useEffectEvent`, `cacheSignal`, Performance Tracks and Partial Pre-rendering, among other changes.

### 136. What is `<Activity />`?

`<Activity>` lets applications control and prioritize UI activities with `visible` and `hidden` modes. Hidden activities can preserve UI state while their effects are removed and updates are deprioritized according to React's Activity semantics.

### 137. What is `cacheSignal`?

A React 19.2 API associated with cache lifetimes, allowing cached work to respond to cache invalidation/abortion semantics. Verify exact usage against the current reference before using it in production code.

### 138. What are Performance Tracks?

React 19.2 added React-specific performance information to browser performance tooling, making React scheduling/rendering activity easier to inspect.

### 139. What is Partial Pre-rendering?

React 19.2 added support for partial pre-rendering in React DOM server APIs. Frameworks determine how this capability is integrated into an application's routing and caching architecture.

### 140. Are React 19 Server Components the same as Next.js Server Components?

No. React defines the underlying component model and APIs; frameworks such as Next.js provide the application-level bundling, routing, data and deployment integration.

---

# React Compiler

### 141. What is React Compiler?

React Compiler is a build-time tool that analyzes React code and automatically applies memoization optimizations where appropriate.

### 142. Does React Compiler make `useMemo` obsolete?

Not universally. React's current guidance recommends relying on the compiler for normal memoization in compiled code and using `useMemo`, `useCallback` or `React.memo` when precise manual control is actually needed.

### 143. Should you remove every `useMemo` after enabling Compiler?

No. Existing memoization can affect compiler output and behavior. React recommends testing carefully before removing it.

### 144. Is React Compiler a runtime library?

No. It is primarily a build-time optimization tool.

### 145. What does the Compiler need from your code?

It works best when code follows the Rules of React and maintains purity. Violations can limit optimization or produce compiler diagnostics.

### 146. Which build tools can React Compiler work with?

The current documentation describes support involving Babel, Vite, Metro and Rsbuild, with framework-specific integration also available.

### 147. Does React Compiler optimize every JavaScript function?

No. Compilation follows its supported analysis and configuration rules. Do not assume arbitrary JavaScript is automatically optimized.

---

# Server Components & Server Functions

### 148. What is a React Server Component?

A Server Component renders in a server/build environment separate from the client application and is not sent to the browser as a client component. It can reduce client JavaScript and access server-side resources depending on the framework/environment.

### 149. Can Server Components use `useState`?

No. Server Components do not run as interactive browser components and cannot use client-only interactive Hooks such as `useState`.

### 150. How do Server Components become interactive?

Compose them with Client Components, commonly using the framework-supported client boundary such as `'use client'`.

### 151. Is `'use client'` a React-only directive?

It is part of the React Server Components ecosystem and is interpreted by compatible bundlers/frameworks. It marks a module as a client boundary.

### 152. Is there a `'use server'` directive for Server Components?

No. React's documentation explicitly says there is no directive that marks a component as a Server Component. `'use server'` marks Server Functions.

### 153. What is a Server Function?

An async function executed on the server that can be invoked from client code through a supported React Server Components integration.

### 154. Are Server Functions secure automatically?

No. Treat arguments as untrusted input and authorize every mutation. Server execution is not a substitute for authentication, authorization and input validation.

### 155. What can cross a server/client boundary?

Only values supported by the relevant React/framework serialization protocol. Do not assume arbitrary class instances, functions or database connections can be passed.

### 156. Why use Server Components?

They can keep server-only dependencies and data access out of client bundles, reduce client JavaScript and render data-dependent UI closer to the data source.

### 157. Server Components vs SSR?

SSR is a rendering strategy that produces HTML on the server. Server Components are a component architecture in which some components execute outside the client bundle. They can be used together.

---

# SSR, Hydration & Streaming

### 158. What is SSR?

Server-side rendering generates HTML for a React tree on the server so the browser can receive useful markup before client-side behavior is fully attached.

### 159. What is streaming SSR?

Streaming SSR sends rendered HTML progressively rather than waiting for the complete page, allowing parts of the UI to arrive as they become ready.

### 160. Why does streaming pair well with Suspense?

Suspense boundaries provide meaningful chunks and fallbacks that can be streamed as different parts of the UI become ready.

### 161. What is hydration?

Hydration turns server-rendered HTML into an interactive React application by attaching/reconciling client behavior.

### 162. What is selective hydration?

Modern React can prioritize hydration work around user interaction and scheduling rather than treating the entire page as one indivisible hydration operation.

### 163. `renderToString` vs streaming APIs?

`renderToString` produces a complete HTML string and has limited functionality compared with modern streaming APIs. Current React DOM server documentation provides stream-based APIs for Web Streams and Node.js streams.

### 164. Why is hydration expensive?

The client must load JavaScript, parse/compile it, execute application code and reconcile the server markup. Reducing client JavaScript can therefore improve startup performance.

---

# Routing

### 165. What is client-side routing?

It changes the displayed route without requiring a full document navigation, while keeping URL/history semantics synchronized.

### 166. What is nested routing?

Routes can be organized hierarchically so parent layouts persist while child content changes.

### 167. What is a route loader?

A routing abstraction that retrieves data associated with a route. Exact semantics depend on the routing library/framework.

### 168. How should protected routes work?

Authorization must be enforced by the server. Client-side route guards are useful for UX but are not a security boundary.

### 169. What is code-splitting by route?

Load JavaScript for a route when it is needed rather than shipping the entire application upfront. React.lazy and framework-level route splitting can help.

---

# Phase 5 — Forms, Routing and Resilient UX Deep Dive

This phase focuses on the frontend failure modes that matter in production: user input, navigation, asynchronous requests, partial failure, cancellation, stale data, and accessible recovery paths.

## Forms

### How do you design a production form?

Treat the form as a state machine: initial, editing, submitting, success, and failure states. Define field ownership, validation boundaries, submission semantics, accessibility, and server error mapping before adding UI polish.

### Client validation vs server validation?

Client validation improves feedback and reduces avoidable requests. Server validation is authoritative because clients are untrusted and may be bypassed. A robust form can share validation rules conceptually without assuming client validation is a security boundary.

### How do you handle async validation?

Debounce where appropriate, cancel or ignore stale requests, associate responses with the value/version that initiated them, and avoid allowing an older response to overwrite newer user input.

### How do you prevent duplicate submissions?

Represent submission state explicitly, disable or gate the relevant action when appropriate, and use server-side idempotency keys for mutations where duplicate execution would be harmful. A disabled button alone is not a correctness mechanism.

### How should server errors map to a form?

Keep field-level errors separate from form-level errors. Map stable server error codes to the relevant fields where possible and preserve a general error path for failures that cannot be attributed to one field.

### What makes a form accessible?

Use real labels, semantic controls, keyboard support, clear error associations, meaningful focus behavior, appropriate autocomplete attributes, and status announcements where needed. Error text should be understandable without relying only on color.

## Routing

### What should a route own?

A route should define navigation identity and, where the router supports it, the data/loading/error boundary associated with that navigation. Keep reusable UI components independent of router-specific details when practical.

### How do nested routes help?

They let layouts and parent UI persist while child content changes. This can improve information architecture and avoid rebuilding shared navigation or shell state on every navigation.

### How should protected routes work?

The server must enforce authentication and authorization. Client-side guards are for navigation UX: redirecting unauthenticated users, showing loading state while identity is known, and avoiding flashes of unauthorized UI.

### What is route-level code splitting?

Load the code needed for a route when the route is reached instead of shipping every screen up front. The trade-off is a navigation-time loading boundary, so important routes may benefit from prefetching or framework-level optimization.

### How do you handle a navigation that fails?

Provide a route-level error state with a retry/reload path, preserve enough navigation context for the user to recover, and distinguish application errors from an unavailable backend or failed network request.

## Resilient asynchronous UX

### What states should an API-driven component model?

At minimum: idle/loading, success, empty, failure, and often refreshing or stale-but-visible. Treating everything as loading or error usually produces poor UX and makes recovery ambiguous.

### How do you prevent stale responses from overwriting current state?

Cancel obsolete requests when the API supports cancellation, or track a request ID/version and ignore responses that no longer correspond to the current state. This is especially important for search, filters, and rapidly changing routes.

### Retry or not retry?

Retry transient failures when the operation is safe and the user benefits from recovery. Avoid blindly retrying validation/authentication failures or creating repeated mutations without idempotency. Give the user control when repeated automatic retries would be surprising.

### What is graceful degradation in a frontend?

Keep the most important user journey usable when a non-critical dependency fails. For example, render cached data while a secondary panel fails, rather than turning the entire page into an error screen.

### How should optimistic UI work?

Apply the expected state immediately, record enough information to reconcile or roll back, submit the mutation, and handle conflict/failure explicitly. Optimistic UI is appropriate only when the expected action is reasonably predictable and rollback is understandable.

### What is the difference between loading, empty, and error states?

Loading means the result is not available yet. Empty means the request succeeded but there is nothing to show. Error means the result could not be obtained or processed. These states need different copy and recovery actions.

### How do you handle offline or intermittent connectivity?

Detect connectivity as a hint rather than absolute truth, preserve user input where safe, avoid losing pending work, retry appropriate operations, and communicate whether data is cached, stale, pending, or failed. Critical mutations need explicit synchronization semantics.

## Production debugging scenarios

### A user double-clicks Submit and creates two orders. What do you do?

The frontend should prevent accidental duplicate interaction, but the backend should enforce idempotency for the order operation. Investigate request IDs, server logs, retry behavior, network timing, and whether the client generated or reused an operation key.

### Search results randomly show an older query. Why?

Requests can resolve out of order. Reproduce with artificial latency, inspect request sequencing, then cancel stale requests or gate state updates by the latest query/request ID.

### Users see a blank screen after navigation. What is your workflow?

Check route transitions and chunk loading, browser console errors, route-level exceptions, failed data requests, deployment asset/version mismatches, and error boundaries. A route-level fallback should provide a recovery path rather than leaving an unhandled failure.

### A form loses all entered data after a network error. Is that acceptable?

Usually not. Preserve user input when the data is safe to retain, show the failure close to the action that failed, and allow retry without forcing the user to re-enter everything.

### Senior interviewer follow-ups

- Where should retries live: UI, data-fetching layer, or backend?
- How do you cancel a request with AbortController?
- How do you test stale-response races deterministically?
- When is optimistic UI unsafe?
- How would you design navigation during an authentication refresh?
- What should happen if a route's JavaScript chunk is unavailable after deployment?
- How do accessibility requirements change your error/loading design?

---
# TypeScript + React

### 170. How do you type component props?

Use an explicit object type/interface and type the component parameter. Prefer precise domain types over broad `any`.

### 171. `interface` vs `type` for props?

Both can describe object shapes. `type` is especially flexible for unions/intersections; `interface` supports declaration merging and extension semantics. Choose consistently based on the codebase.

### 172. Should you use `React.FC`?

It is optional, not required. Explicitly typing props often provides simpler and clearer inference. Use the convention that best fits the project.

### 173. How do you type children?

Use the appropriate React node type for arbitrary renderable content or a narrower type when the component expects something more specific.

### 174. How do you type a DOM event?

Use React's event types, such as `React.ChangeEvent<HTMLInputElement>` when explicit annotation is needed. Prefer contextual typing where TypeScript can infer it.

### 175. How do you type refs?

Use the relevant element or value type, e.g. `useRef<HTMLInputElement | null>(null)` when appropriate.

### 176. What is a discriminated union?

A union of object types sharing a literal discriminator, allowing TypeScript to narrow safely based on that property.

### 177. Why avoid `any`?

`any` disables substantial type checking and can hide bugs. Prefer `unknown` for values whose type is not yet known and narrow them safely.

### 178. How do you type API responses?

Define domain types and validate untrusted runtime data at the boundary when correctness matters. TypeScript types alone do not validate JSON at runtime.

### 179. What is type narrowing?

TypeScript refines a broader type into a more specific type using control flow, type guards, discriminators and related checks.

---

# Data Fetching & APIs

### 180. Should every API request use `useEffect`?

No. Depending on the application, route loaders, Server Components, framework data APIs, query libraries or direct event-driven mutations may be better.

### 181. What makes server-state fetching difficult?

Caching, deduplication, invalidation, stale data, retries, race conditions, pagination, optimistic updates and synchronization across views.

### 182. What is optimistic UI?

Update the UI immediately assuming a mutation will succeed, then reconcile with the server result or roll back on failure.

### 183. What is stale-while-revalidate?

Show cached data immediately while fetching a newer version in the background, then update the UI when fresh data arrives.

### 184. How should cancellation work?

Use cancellation mechanisms supported by the underlying API, commonly `AbortController` for Fetch, and ensure stale responses cannot overwrite current state.

### 185. REST vs GraphQL?

REST exposes resources/endpoints using HTTP semantics. GraphQL exposes a typed query language and schema where clients select requested fields. Neither is universally superior; complexity, caching, organizational boundaries and client needs matter.

### 186. How do you handle API errors?

Model loading, success and failure states explicitly; distinguish user-correctable errors, authorization failures, transient failures and unexpected faults; provide useful recovery behavior and observability.

### 187. What is pagination?

Retrieving large datasets in manageable chunks. Common designs include offset/page pagination and cursor-based pagination.

### 188. What is infinite scrolling?

Incrementally loading more content as the user approaches the end of a list. Use virtualization for very large lists and ensure keyboard/accessibility behavior remains usable.

---

# Performance

### 189. How do you optimize a React application?

Measure first. Identify expensive renders, large bundles, slow network requests, excessive JavaScript, long tasks, layout work and unnecessary data processing. Then apply targeted changes and verify them with profiling and real-user metrics.

### 190. What is React Profiler?

React DevTools Profiler helps inspect component rendering and performance behavior. Production diagnosis should combine it with browser performance tools and real-user telemetry.

### 191. What is memoization?

Caching a calculation or identity so equivalent inputs can reuse previous work. Memoization has memory and complexity costs, so it should solve a measured problem.

### 192. `React.memo`?

It can skip re-rendering a component when its props compare equal according to its memoization strategy. It is an optimization, not a correctness mechanism.

### 193. Why can `React.memo` fail to help?

Props may receive new object/function identities every render, the component may consume changing context, its own state may change, or the render may be cheap enough that comparison costs outweigh savings.

### 194. What is code splitting?

Dividing JavaScript into independently loadable chunks, often by route or feature.

### 195. What is lazy loading?

Defer loading a resource or component until it is needed. `React.lazy` supports lazy component loading with Suspense.

### 196. What is virtualization?

Render only the visible subset of a large collection instead of mounting every item.

### 197. How do you optimize a large table?

Virtualize rows/columns where appropriate, avoid unnecessary cell renders, paginate or window data, keep stable identities, and profile actual bottlenecks.

### 198. Why can huge context values hurt performance?

Changing a provider value can cause its consumers to update. Splitting concerns and subscription granularity can reduce unnecessary work.

### 199. What is bundle size?

The amount of JavaScript and other assets delivered to the client. Measure both compressed transfer size and parsed/executed cost.

### 200. What is tree shaking?

A bundler optimization that removes unused statically analyzable exports, especially with ES modules.

### 201. What is the most important React performance rule?

**Do not optimize based on folklore. Measure, form a hypothesis, change one thing, and measure again.**

---

# Accessibility

### 202. What is semantic HTML?

Using elements according to their intended meaning, such as `<button>` for actions and `<nav>` for navigation.

### 203. Why prefer `<button>` over `<div onClick>`?

Buttons provide keyboard interaction, semantics and browser accessibility behavior by default. Recreating those behaviors manually is error-prone.

### 204. What is ARIA?

Accessible Rich Internet Applications attributes supplement semantics when native HTML cannot express the required UI semantics. ARIA should not replace appropriate native HTML.

### 205. What is keyboard accessibility?

All interactive functionality should be usable with a keyboard and have logical focus order and visible focus indication.

### 206. What is focus management?

Explicitly controlling focus when UI changes require it, such as moving focus into a newly opened modal and returning it appropriately when the modal closes.

### 207. How do you make a modal accessible?

Use appropriate dialog semantics, manage focus, prevent inappropriate background interaction, provide an accessible name, support Escape where appropriate and restore focus when closing.

### 208. How should images be handled?

Provide meaningful alternative text when the image conveys information. Decorative images should not create redundant announcements.

### 209. What is WCAG?

Web Content Accessibility Guidelines provide internationally recognized accessibility guidance organized around principles including perceivable, operable, understandable and robust content.

---

# Testing

### 210. Unit vs integration vs end-to-end testing?

Unit tests isolate small units. Integration tests verify interactions among units. End-to-end tests exercise realistic user flows through the application.

### 211. What should React component tests assert?

Prefer user-visible behavior and outcomes over implementation details such as internal state or private functions.

### 212. What is React Testing Library's philosophy?

Test components through the way users interact with and perceive them rather than tightly coupling tests to implementation details.

### 213. What should you test in a form?

Rendering, labels, validation behavior, submission states, success/error handling, keyboard behavior and important accessibility semantics.

### 214. What should you mock?

Mock expensive or nondeterministic external boundaries when appropriate, but avoid mocking so much that tests no longer represent real application behavior.

### 215. What is a flaky test?

A test whose result is nondeterministic despite unchanged code/input. Common causes include timing, shared state, race conditions, network dependencies and improper cleanup.

### 216. How do you test asynchronous UI?

Wait for user-visible outcomes using the testing library's async utilities rather than arbitrary sleep delays.

### 217. What is snapshot testing?

A test records serialized output and compares future output against it. Snapshots can be useful but become low-value when huge and mechanically updated without reviewing semantic changes.

### 218. What is E2E testing useful for?

Validating critical flows such as authentication, checkout, navigation and major integrations from the user's perspective.

---

# Security

### 219. What is XSS?

Cross-site scripting occurs when attacker-controlled content is interpreted as executable script in a user's browser.

### 220. Does React prevent XSS?

React escapes ordinary text interpolations by default, which mitigates many injection cases. Dangerous APIs such as `dangerouslySetInnerHTML` bypass that protection and require trusted/sanitized content.

### 221. What is CSRF?

Cross-Site Request Forgery tricks a user's browser into making an authenticated request to another site. Defenses depend on authentication architecture and can include SameSite cookies, CSRF tokens and origin checks.

### 222. Should JWTs be stored in localStorage?

There is no universal answer, but sensitive tokens in JavaScript-accessible storage are exposed to XSS. HttpOnly, Secure, appropriately SameSite cookies can reduce token exposure to JavaScript. Choose an authentication architecture based on threat model and application requirements.

### 223. Why is client-side authorization insufficient?

Attackers control the client. Every privileged operation must be authorized by the server.

### 224. What is dependency supply-chain risk?

Third-party packages can introduce vulnerabilities, malicious code or compromised transitive dependencies. Use lockfiles, updates, auditing, provenance/signature mechanisms where available and minimize unnecessary dependencies.

### 225. What is DOM-based XSS?

A client-side injection vulnerability where unsafe data reaches a DOM sink and is interpreted as HTML/script by the browser.

---

# Tooling & Build Systems

### 226. What is a bundler?

A tool that analyzes modules and assets and produces deployable output, often performing transformation, code splitting, optimization and asset processing.

### 227. Vite vs Webpack?

Vite emphasizes fast development using native ESM and a production bundling pipeline, while Webpack is a highly configurable mature bundler ecosystem. Exact behavior depends on versions and configuration.

### 228. What is Babel?

A JavaScript transformation tool that can parse and transform syntax through plugins/presets. It can participate in React compilation pipelines.

### 229. What is SWC?

A fast compiler/tooling project written in Rust that can transform JavaScript/TypeScript and power parts of modern frameworks and build pipelines.

### 230. What is ESLint?

A static analysis/linting tool for JavaScript and related ecosystems. React projects commonly use React-specific lint rules.

### 231. Why use `eslint-plugin-react-hooks`?

It detects common violations involving Hook rules and effect dependencies, helping maintain React's assumptions.

### 232. What is a source map?

A mapping from generated code back to original source code, useful for debugging transformed/minified applications.

### 233. What is environment configuration?

Build/deployment-specific configuration such as API endpoints and feature flags. Secrets must not be embedded in browser bundles because client-delivered code is public.

---

# Architecture & Design Patterns

### 234. What is component-driven architecture?

Organizing UI as reusable components with clear ownership, contracts and composition boundaries.

### 235. What is feature-based folder structure?

Group files around business features rather than technical types. This can improve cohesion and scalability, though it is not universally superior.

### 236. Container vs presentational components?

A traditional distinction between components that coordinate data/behavior and components focused primarily on presentation. Modern Hooks and composition often make this split less rigid.

### 237. What is a render prop?

A component receives a function prop and calls it to determine part of its rendered output. It was a common reuse pattern before Hooks became widespread and remains useful in some APIs.

### 238. What is a higher-order component?

A function that takes a component and returns an enhanced component. HOCs remain valid but can introduce wrapper complexity; Hooks/composition are often preferred for new logic reuse.

### 239. What is compound component architecture?

A group of related components that coordinate through shared context or conventions, such as `<Select>`, `<Select.Trigger>` and `<Select.Option>`.

### 240. What is headless UI?

Components expose behavior/state/accessibility while leaving visual presentation to the consumer.

### 241. What is inversion of control in UI components?

Instead of a component hardcoding every detail, consumers provide behavior or rendering decisions through props, children or render functions.

### 242. How do you design reusable components?

Start from stable behavior, define a small API, avoid leaking implementation details, make accessibility part of the contract, support composition and avoid premature generalization.

### 243. How do you avoid a “God component”?

Split by responsibility and ownership, extract reusable logic into Hooks/functions, move data boundaries to appropriate layers and keep components focused on a coherent piece of UI behavior.

---

# Debugging & Production

### 244. A component renders infinitely. How do you debug it?

Look for state updates during render, Effects that update one of their own dependencies, unstable dependencies causing repeated synchronization, external-store loops and parent/child feedback cycles. Reproduce minimally and inspect with React DevTools.

### 245. Why is an Effect running repeatedly?

A dependency may be changing identity every render, the Effect may be updating state that changes a dependency, or the Effect may genuinely need to resynchronize. Do not blindly remove dependencies.

### 246. A child renders too often. What do you check?

Check parent renders, prop identity, context, state, external-store subscriptions and whether the render is actually expensive. Profile before adding memoization.

### 247. The UI freezes while typing. What do you investigate?

Long synchronous JavaScript, expensive rendering, filtering/sorting huge datasets, layout work and unnecessary state propagation. Use browser Performance tools and React profiling.

### 248. API results appear out of order. Why?

Concurrent requests can resolve in a different order from initiation. Cancel stale requests or associate responses with the current request/version.

### 249. Memory usage grows over time. What do you inspect?

Uncleaned subscriptions/listeners, timers, retained closures, caches, detached DOM nodes and third-party libraries. Use browser heap snapshots and allocation profiling.

### 250. Production has a hydration mismatch. What is your workflow?

Identify the component boundary, compare server and client inputs, remove nondeterministic rendering, inspect environment-specific branches, validate data consistency and reproduce with production-like rendering.

### 251. A feature works locally but fails in production. What do you check?

Build configuration, environment variables, base paths, asset URLs, SSR/runtime differences, browser support, caching/CDN behavior, minification, source maps, API CORS/authentication and deployment logs.

### 252. How should frontend observability work?

Collect errors, performance signals, key user journeys and relevant context while respecting privacy. Correlate client events with backend traces where possible.

---

# Frontend System Design

### 253. Design a scalable React dashboard.

Discuss information architecture, route-level code splitting, data-fetching boundaries, caching, pagination/virtualization, state ownership, accessibility, error/loading states, observability and deployment strategy.

### 254. Design a social-media feed.

Discuss cursor pagination, caching, optimistic mutations, real-time updates, virtualization, media loading, accessibility, moderation/security, offline/retry behavior and consistency trade-offs.

### 255. Design an e-commerce product page.

Discuss server/client rendering boundaries, SEO, product data caching, image optimization, variants, cart state, inventory freshness, optimistic UI, accessibility, analytics and failure handling.

### 256. Design a real-time chat application.

Discuss WebSocket/SSE architecture, connection lifecycle, message ordering, optimistic messages, retry/deduplication, unread counts, virtualization, presence and reconnection behavior.

### 257. Design a collaborative editor.

Discuss conflict resolution/CRDT or OT approaches, server synchronization, local optimistic updates, presence, persistence, permissions and offline recovery.

### 258. Design a large data table.

Discuss server-side pagination/filtering/sorting, virtualization, column configuration, stable row IDs, accessibility, keyboard navigation, export behavior and performance budgets.

### 259. Design a design system.

Discuss tokens, primitives, component contracts, accessibility, theming, versioning, documentation, visual regression testing, release strategy and adoption governance.

### 260. How would you migrate a legacy React app?

Start with inventory and risk assessment. Establish tests and observability, define target architecture, migrate incrementally at feature boundaries, control dependencies, measure performance and keep rollback paths.

---

# Coding / Machine-Coding Questions

Be prepared to implement these without relying on a framework's magic.

1. Build a counter with increment/decrement/reset.
2. Build a searchable list.
3. Build a debounced search box.
4. Build an autocomplete/typeahead.
5. Build tabs.
6. Build an accordion.
7. Build a modal with focus management.
8. Build a tooltip.
9. Build a dropdown.
10. Build a toast notification system.
11. Build pagination.
12. Build infinite scrolling.
13. Build a virtualized list.
14. Build a sortable/filterable table.
15. Build a multi-step form.
16. Build a form with validation.
17. Build a file-upload component.
18. Build an image gallery/lightbox.
19. Build a star-rating component.
20. Build a nested comments tree.
21. Build a recursive folder explorer.
22. Build a shopping cart.
23. Build a shopping-cart quantity editor.
24. Build a stopwatch.
25. Build a countdown timer.
26. Build a traffic-light component.
27. Build a progress bar.
28. Build a skeleton-loading system.
29. Build a retryable API hook.
30. Build a custom `useDebounce` Hook.
31. Build a custom `usePrevious` Hook.
32. Build a custom `useInterval` Hook.
33. Build a custom `useFetch` abstraction and discuss its limitations.
34. Build a reusable controlled/uncontrolled input.
35. Build a compound component.
36. Build a modal portal.
37. Build a keyboard-accessible command palette.
38. Build a drag-and-drop list.
39. Build a Kanban board.
40. Build a real-time notification panel.
41. Build a route-aware breadcrumb component.
42. Build an optimistic update flow.
43. Build an error boundary integration.
44. Build a theme switcher.
45. Build a permission-aware UI.
46. Build a feature-flag provider.
47. Build a responsive dashboard.
48. Build a virtualized autocomplete.
49. Build a cached data-fetching layer.
50. Build a small state-management library.

For every machine-coding question, interviewers may additionally ask:

- What happens with 100,000 items?
- What happens with slow network conditions?
- How do you cancel requests?
- How is keyboard accessibility handled?
- How would you test it?
- How would you make it reusable?
- How would you handle errors?
- What would you change for SSR?
- What would you change for React Server Components?
- Where would state live?
- What are the performance bottlenecks?

---

# Senior & Staff Questions

### 261. Why does React prefer pure rendering?

Because React may render work multiple times, interrupt it, restart it or discard it before commit. Pure rendering keeps those scheduling strategies safe.

### 262. When would you choose local state over Redux/Zustand/etc.?

When the state has a local owner and does not require cross-tree synchronization. Global state should earn its complexity.

### 263. When is Context the wrong tool?

When values update extremely frequently, when consumers need fine-grained subscriptions, or when the data is actually server state better handled by a data-fetching/cache architecture.

### 264. How do you evaluate a React library?

API quality, maintenance, ecosystem health, bundle/runtime cost, accessibility, TypeScript support, SSR/RSC compatibility, security, license, dependency graph and migration risk.

### 265. How would you introduce React Compiler into a large application?

Establish a performance baseline, verify compiler compatibility, fix Rules-of-React violations, enable incrementally, monitor compiler diagnostics and production metrics, and test existing memoization carefully.

### 266. How would you migrate from React 18 to React 19?

Read the official upgrade guide/release notes, update dependencies together, run tests and production builds, review behavior changes, validate libraries, then roll out incrementally where possible.

### 267. How would you adopt Server Components?

Identify server-only/data-heavy components, establish framework/bundler support, define client boundaries intentionally, keep sensitive/server-only dependencies out of client modules, and measure bundle/startup impact.

### 268. How do you decide between SSR, SSG, ISR, CSR and RSC?

Choose based on freshness, SEO, personalization, latency, infrastructure, caching, interactivity and data locality. Modern applications can combine multiple strategies.

### 269. How do you define a frontend performance budget?

Choose user-centric metrics and budgets such as Core Web Vitals, JavaScript transfer/execute cost, long tasks, interaction latency and route-level startup time. Validate with lab and real-user data.

### 270. How do you prevent architectural drift?

Document boundaries, enforce conventions with tooling, maintain dependency rules, review architecture-impacting changes, monitor bundle/dependency health and periodically remove obsolete abstractions.

### 271. How do you balance developer experience and runtime performance?

Optimize for the whole system. A complicated architecture that saves a few milliseconds but dramatically increases development cost may be worse than a simpler design. Measure user impact and maintenance cost.

### 272. How do you handle a critical frontend incident?

Contain impact, establish observability, reproduce, identify the smallest safe mitigation, deploy/rollback, communicate clearly, verify recovery and perform a blameless root-cause review with preventative actions.

---

# Behavioral & Project Questions

Prepare concise STAR-style answers backed by concrete technical details.

1. Tell me about your most complex React application.
2. Why did you choose React?
3. Why did you choose your state-management solution?
4. Tell me about a difficult production bug.
5. Tell me about a major performance improvement.
6. Tell me about a failed technical decision.
7. Tell me about a disagreement over architecture.
8. Tell me about a migration you led.
9. Tell me about a security issue you prevented.
10. Tell me about an accessibility improvement you made.
11. Tell me about a difficult API integration.
12. Tell me about a flaky test suite you fixed.
13. Tell me about reducing bundle size.
14. Tell me about a deployment incident.
15. Tell me about mentoring another developer.
16. How do you review React pull requests?
17. How do you decide whether to refactor?
18. How do you handle technical debt?
19. How do you estimate frontend work?
20. How do you communicate technical risk?
21. How do you keep up with React changes?
22. What React feature do you disagree with or find difficult?
23. What is one React misconception you frequently see?
24. What would you change about your last frontend architecture?
25. What is the most consequential frontend decision you made?

---

# Rapid-Fire Questions

Use these for final-round revision.

- Props are read-only? **Yes, treat them as immutable inputs.**
- State updates are immediate? **No, they schedule an update.**
- Does render mean DOM mutation? **No.**
- Are keys props? **No, `key` is special React identity metadata.**
- Should indexes be keys? **Usually no for reorderable/dynamic lists.**
- Can React render `null`? **Yes.**
- Does `useRef` trigger rendering? **Changing `.current` does not.**
- Does `useMemo` guarantee performance improvement? **No.**
- Is `useEffect` for derived values? **Usually no.**
- Is Context global state? **No.**
- Is client-side authorization secure? **No.**
- Does TypeScript validate API JSON at runtime? **No.**
- Is React Compiler a runtime library? **No.**
- Does Server Component mean SSR? **No.**
- Is `'use server'` how you mark Server Components? **No.**
- Can Server Components use `useState`? **No.**
- Can Client Components use server-only database connections? **No.**
- Does React prevent every XSS vulnerability? **No.**
- Does `React.memo` prevent all renders? **No.**
- Is virtualization the same as pagination? **No.**
- Does debounce change React priority? **No.**
- Does a transition delay every update? **No; it marks specific updates as non-urgent.**
- Is `useId` a list-key generator? **No.**
- Is `useEffect` guaranteed to execute exactly once? **No.**
- Is Strict Mode a production performance feature? **No; its extra development checks are development-oriented.**
- Is React a complete backend framework? **No.**

---

# Interview Preparation Matrix

| Level | Must Know | Should Know | Standout |
|---|---|---|---|
| Junior | JS, JSX, components, props, state, events, lists, basic Hooks | Context, forms, testing, accessibility | Debugging and performance fundamentals |
| Mid | All React core, Hooks, state architecture, TypeScript, APIs | SSR, routing, testing strategy, performance | Architecture and production debugging |
| Senior | React internals/mental model, concurrency, SSR/RSC, performance, security | Compiler, advanced testing, observability | System design, migrations, trade-offs |
| Staff | Everything above | Platform architecture and governance | Multi-team architecture, strategy and organizational trade-offs |

---

# Source & Verification Policy

This project should prioritize **primary sources** over blog posts and interview-prep folklore.

## Tier 1 — Primary

- React documentation: https://react.dev/
- React release notes: https://react.dev/blog
- React API reference: https://react.dev/reference/react
- React DOM reference: https://react.dev/reference/react-dom
- React Server Components reference: https://react.dev/reference/rsc
- React source repository: https://github.com/facebook/react
- MDN Web Docs: https://developer.mozilla.org/
- ECMAScript specification: https://tc39.es/ecma262/
- WHATWG HTML: https://html.spec.whatwg.org/
- Fetch Standard: https://fetch.spec.whatwg.org/
- W3C/WAI accessibility guidance: https://www.w3.org/WAI/
- WCAG: https://www.w3.org/TR/WCAG22/
- TypeScript handbook: https://www.typescriptlang.org/docs/

## Tier 2 — High-quality secondary sources

Use reputable framework documentation, browser documentation, standards organizations and well-maintained project documentation.

## Tier 3 — Community material

Blogs, tutorials, Stack Overflow, Reddit and interview websites can be useful for discovering questions, but they should **not** be treated as authoritative evidence for React behavior.

## Verification checklist for every future answer

Before merging a new answer:

- [ ] Check the current React documentation.
- [ ] Check the relevant React release notes if version-sensitive.
- [ ] Check whether the behavior is React core or framework-specific.
- [ ] Check whether the answer describes current React or legacy React.
- [ ] Avoid undocumented implementation details presented as guarantees.
- [ ] Avoid performance claims without measurement/context.
- [ ] Include trade-offs where the question is architectural.
- [ ] Add a primary source when the claim is subtle or version-sensitive.
- [ ] Test code examples when practical.

## Version-sensitive claims

React is actively evolving. Questions involving the following should always be version-checked:

- React Compiler
- Server Components
- Server Functions
- `use`
- Actions/forms
- `<Activity />`
- `useEffectEvent`
- `cacheSignal`
- Partial Pre-rendering
- React DOM server APIs
- hydration behavior
- framework integration
- routing/data APIs

---

# Suggested Repository Evolution

The README is the **epic/index**. As the question bank grows, individual verified answer sets should be split into focused files without losing the README as the canonical map.

Recommended future structure:

```text
README.md
questions/
  01-javascript.md
  02-browser.md
  03-react-fundamentals.md
  04-jsx-rendering.md
  05-props-state.md
  06-hooks.md
  07-effects.md
  08-context-state-management.md
  09-forms.md
  10-suspense-concurrency.md
  11-react-19.md
  12-react-compiler.md
  13-server-components.md
  14-ssr-hydration.md
  15-routing.md
  16-typescript.md
  17-data-fetching.md
  18-performance.md
  19-accessibility.md
  20-testing.md
  21-security.md
  22-tooling.md
  23-architecture.md
  24-debugging.md
  25-system-design.md
  26-machine-coding.md
  27-senior-staff.md
  28-behavioral.md
sources/
  react.md
  javascript.md
  web-platform.md
  typescript.md
```

## Definition of “complete”

No finite list can literally contain every question an interviewer could invent. The goal of this repository is therefore **coverage of the React developer interview surface area**, with continuously maintained, source-backed answers rather than a static claim of mathematical completeness.

---

## Contribution standard

A high-quality contribution should answer:

> **What is it? Why does it exist? How does it work? When should I use it? When should I avoid it? What are the trade-offs? What changed in modern React?**

Avoid:

- cargo-cult rules
- “React always does X” claims without qualification
- obsolete React 16/17 advice presented as current
- framework-specific behavior presented as React core behavior
- performance claims without evidence
- security advice that relies solely on client-side controls
- huge code samples when a minimal example is clearer

---

## ⭐ Goal

Build one of the most **comprehensive, technically rigorous and current React interview references** available on GitHub — useful both for candidates preparing for interviews and experienced engineers evaluating their own depth.

**React is a moving target. Keep the answers verified.**
