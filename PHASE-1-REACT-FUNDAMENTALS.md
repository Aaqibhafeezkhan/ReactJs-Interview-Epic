# Phase 1 — React Fundamentals

This phase strengthens the React fundamentals that senior interviews expect you to explain as a mental model rather than as isolated definitions.

## Rendering and identity

### What happens when a React component renders?

**Interview answer:** React calls the component to calculate the next UI description. Rendering is a calculation; it does not mean the DOM is necessarily changed. React can render work that is later abandoned, so render logic should remain pure.

**Mental model:** Think of render as producing a candidate tree. React then reconciles that tree with the previous one and commits only the host changes that are required.

**Practical example:**

```tsx
function Greeting({ name }: { name: string }) {
  return <h1>Hello, {name}</h1>;
}
```

Calling `Greeting` conceptually calculates the element tree. The browser DOM update is a later concern.

**Trade-offs and pitfalls:** Do not perform subscriptions, mutations, network writes, or other externally visible side effects during render.

**Senior follow-up:** Why can React render a component more than once without committing each result?

### Render phase vs commit phase

The render phase determines what should be displayed. The commit phase applies the result to the host environment. This distinction explains why a component can render without causing a visible DOM mutation and why render code must be free of side effects.

### Does a parent render always change the DOM?

No. A parent render can cause child components to be evaluated, but reconciliation may find that the resulting host output is equivalent. Rendering and DOM mutation are different operations.

## Reconciliation and keys

### What is reconciliation?

Reconciliation is React's process of comparing the newly calculated element tree with the previous tree to determine what work is needed before commit. Element type, position, keys, and identity all influence whether existing state and host nodes can be reused.

### Why do keys matter?

Keys give React stable identity among siblings. They let React distinguish an item that moved from an item that was removed and replaced.

```tsx
items.map(item => <Row key={item.id} item={item} />)
```

Prefer stable IDs from the data. Index keys are risky when insertion, deletion, sorting, or filtering can change positions.

### Why can a bad key cause state to appear on the wrong row?

React associates state with a component's position and identity in the rendered tree. If an index key causes identities to shift, React can reuse the state belonging to a different logical item.

## Components, props and composition

### What makes a good React component boundary?

A useful component boundary groups UI and behavior that change together while keeping data flow understandable. Avoid splitting every small expression into a component merely to make the file shorter.

### Props vs state

Props are inputs supplied by a parent. State is data owned by a component that can trigger another render when updated. If a value can be derived from props or existing state during render, storing a second copy often creates synchronization problems.

### Why is composition preferred over inheritance?

React's model naturally supports composition through children and props. Composition keeps relationships explicit and allows a parent to control the structure without coupling components through a class hierarchy.

```tsx
function Panel({ children }: { children: React.ReactNode }) {
  return <section className="panel">{children}</section>;
}
```

## State semantics

### What does a state update actually do?

A state setter requests an update; it does not mutate the current render's state variable. The current render continues to observe its existing snapshot, and a later render receives the updated state.

```tsx
function Counter() {
  const [count, setCount] = useState(0);

  function incrementThreeTimes() {
    setCount(c => c + 1);
    setCount(c => c + 1);
    setCount(c => c + 1);
  }

  return <button onClick={incrementThreeTimes}>{count}</button>;
}
```

Functional updates are important when multiple updates depend on the previous value.

### Why should state be updated immutably?

React relies heavily on identity comparisons. Mutating an existing object can preserve the same reference and make changes harder to detect and reason about. Creating the next state also makes the transition explicit.

```tsx
setUser(previous => ({
  ...previous,
  name: 'Aaqib'
}));
```

### When should derived data not be state?

If `fullName` is always `firstName + lastName`, storing both values creates two sources of truth. Derive it during render unless the value represents independently changing state.

## Purity and common mistakes

### Why must render be pure?

React may call render more than once or start work that it later abandons. Pure rendering means the same inputs produce the same result without mutating external systems.

Common mistakes:

- mutating props or state objects
- performing network writes during render
- subscribing during render
- generating state from duplicated derived values
- using unstable list keys
- assuming every render produces a DOM mutation

### Senior interview scenario: a list loses input state after sorting

First inspect the list keys. If indexes are used, sorting changes which logical item owns each key and React may reuse component state for a different item. Replace the index with a stable domain identifier, then verify whether any intentional remount behavior is also involved.

### Senior interview scenario: a component renders many times

Do not begin by adding `memo`. First identify what is causing the render, whether the result is expensive, whether the parent is recreating identities, and whether the component's architecture can reduce unnecessary work. Optimization should follow measurement rather than render-count anxiety.

## Phase 1 checklist

- Rendering and render/commit mental model
- Reconciliation and stable identity
- Keys and list state preservation
- Component boundaries and composition
- Props, state, and derived data
- Functional state updates
- Immutable state transitions
- Render purity
- Common correctness mistakes
- Senior debugging scenarios
