# Phase 3 — State and Architecture

This phase focuses on choosing the right state model as a React application grows. The goal is not to memorize state-management libraries, but to explain ownership, boundaries, subscriptions, and trade-offs.

## 1. State ownership

### Q1. What is state ownership?
State ownership means identifying which part of the application is responsible for creating, updating, and interpreting a piece of state. Keep state at the lowest practical owner and lift it only when multiple components genuinely need coordinated access.

### Q2. Local state vs shared state?
Local state is best for UI concerns that belong to one component or a small subtree. Shared state is justified when multiple consumers need the same state, coordinate transitions, or must observe a common lifecycle.

### Q3. What is state colocation?
Colocation means keeping state close to the components that use it. It reduces unnecessary provider/store dependencies, limits update propagation, and makes ownership easier to understand.

### Q4. When should you lift state?
Lift state to the nearest common owner when sibling or peer components must coordinate it. Do not lift state simply because another component might need it later.

### Q5. What is a single source of truth?
A single source of truth means one authoritative representation owns a value. If the same fact is stored independently in several places, the application now needs synchronization logic and can drift into contradictory states.

## 2. Context

### Q6. What problem does Context solve?
Context passes a value through a component tree without manually threading the value through every intermediate component. It is useful for dependencies or state that is naturally scoped to a subtree.

### Q7. Is Context a state-management library?
No. Context is a mechanism for making a value available to descendants. The value can be theme, locale, a service, configuration, or state managed by useState/useReducer/a store.

### Q8. How does a Context provider boundary help architecture?
A provider makes the lifetime and visibility of a dependency explicit. A narrowly scoped provider can prevent unrelated parts of an application from depending on the same state and makes testing or replacement easier.

### Q9. What is Context update fan-out?
When a provider value changes, consumers subscribed to that context may need to render again. A provider containing frequently changing, unrelated values can therefore cause broad update propagation.

### Q10. How do you reduce Context churn?
Split contexts by responsibility, keep provider values stable when appropriate, colocate providers with their consumers, and avoid putting every application concern into one global context.

### Q11. Should you memoize every Context value?
No. Memoization should address a measured identity/change problem. It is not a substitute for choosing a better state boundary.

## 3. Reducers

### Q12. When is useReducer useful?
useReducer is useful when state transitions are related, involve multiple fields, or become easier to describe as explicit actions. It can make complex state transitions more predictable than scattered setter calls.

### Q13. What makes a good reducer?
A reducer should be deterministic: given the same state and action, it returns the same next state. Keep side effects outside the reducer and make actions describe meaningful state transitions.

### Q14. What should an action represent?
Prefer actions that describe an event or domain change, such as `requestSubmitted`, `itemAdded`, or `saveFailed`, rather than actions that expose low-level implementation details.

### Q15. Reducer vs multiple useState calls?
Multiple useState calls are often clearer for independent values. A reducer becomes attractive when transitions span several values or when the state machine is easier to reason about centrally.

### Q16. Can reducers perform API calls?
Do not put asynchronous side effects inside a reducer. Dispatch an action around the async lifecycle and perform the external work in the appropriate event/effect/service layer.

## 4. Context + reducer

A common application pattern is Context for dependency propagation plus useReducer for state transitions.

```tsx
type Action =
  | { type: "added"; id: string }
  | { type: "removed"; id: string };

type State = { selected: string[] };

function reducer(state: State, action: Action): State {
  switch (action.type) {
    case "added":
      return state.selected.includes(action.id)
        ? state
        : { selected: [...state.selected, action.id] };
    case "removed":
      return { selected: state.selected.filter(id => id !== action.id) };
  }
}
```

The important interview point is the separation of responsibilities: the reducer defines transitions, while Context defines who can access the state/dispatch pair.

## 5. External stores

### Q17. Why would an application use an external store?
An external store can provide shared state outside the React component tree, with its own update and subscription model. This is useful when many parts of an application need coordinated client state or when a store already represents an external source of truth.

### Q18. What is useSyncExternalStore?
It is React's API for safely subscribing to an external store. The key interview idea is that React is told how to subscribe and how to read the current snapshot instead of the component manually wiring ad-hoc subscriptions.

### Q19. Why not just call setState from a subscription callback?
A naive subscription can interact poorly with concurrent rendering and can create tearing or lifecycle bugs. useSyncExternalStore gives React a defined contract for external snapshots and subscriptions.

### Q20. What should an external store own?
Only the state that benefits from being externalized. Keep transient UI state local and avoid turning the store into a dumping ground for every value the application happens to use.

## 6. Redux-style architecture

### Q21. What problem does Redux solve?
Redux provides a predictable centralized state model based on a store, actions, and reducers, with explicit state transitions and subscription semantics. Its value is strongest when shared client state is complex enough to benefit from centralized coordination and tooling.

### Q22. Why is Redux Toolkit preferred for modern Redux code?
Redux Toolkit reduces boilerplate and provides standard patterns for slices, immutable updates, store configuration, and async workflows. In an interview, explain the architectural reason for using a centralized store instead of treating Redux itself as the goal.

### Q23. Context vs Redux?
Context primarily solves value propagation through a tree. Redux is a state architecture with a store, update model, selectors/subscriptions, middleware, and tooling. Context can carry Redux or another store, so the two are not mutually exclusive.

### Q24. When is Redux overkill?
When state is mostly local, changes are simple, and few distant consumers need coordination. Adding a global store can increase indirection and cognitive load without solving a real problem.

### Q25. What are selectors?
Selectors are functions that derive or read a useful slice of store state. Good selectors can limit what a component subscribes to and keep view code independent from the store's internal shape.

## 7. Client state vs server state

### Q26. Is fetched API data the same as UI state?
Not necessarily. Server state has an external owner and additional concerns such as freshness, caching, invalidation, retries, synchronization, and concurrent updates.

### Q27. Why can putting server data into a generic global store become problematic?
The store now has to implement concerns that belong to data-fetching infrastructure: cache identity, stale data handling, refetching, retry policy, request deduplication, and invalidation. A dedicated server-state approach can keep these semantics explicit.

### Q28. What belongs in client UI state?
Examples include modal visibility, selected tabs, draft form state, local filters, expanded rows, and temporary interaction state.

### Q29. What belongs in server state?
Data whose source of truth is an API or remote system, such as user profiles, product lists, orders, or account balances. The application may cache it locally, but it does not own the authoritative value.

### Q30. Can both kinds of state interact?
Yes. A filter in client state can determine which server query is requested. The UI should still keep ownership and lifecycle semantics distinct.

## 8. Derived and duplicated state

### Q31. Should derived values be stored globally?
Usually no. If a value can be computed from existing state and props, derive it rather than storing a second source of truth.

### Q32. What is a common architecture smell with global state?
Putting values into global state only because they are technically accessible there. Global state should represent genuinely shared state, not merely convenient state.

### Q33. Why can duplicated state cause production bugs?
Two copies can be updated through different paths, creating temporary or permanent disagreement. Bugs then appear as synchronization failures rather than simple rendering mistakes.

## 9. Debugging scenarios

### Scenario: updating one Context value causes half the page to render
Check provider scope and whether unrelated state has been combined under one context. Split state by responsibility and measure the actual update path before adding memoization.

### Scenario: users see stale values after switching accounts
Check ownership and reset boundaries. Account-scoped state should not accidentally survive the account transition through a long-lived global store or provider.

### Scenario: a global store keeps growing
Inventory each value. Remove transient UI state, derived data, and server-cache responsibilities that do not belong in the store.

### Scenario: two tabs disagree about selected data
Determine whether the state is local UI state, browser-shared state, or server-owned state. Cross-tab synchronization is an architectural requirement, not something that automatically follows from choosing a state library.

### Scenario: a reducer becomes a thousand-line switch
Look for multiple unrelated domains sharing one reducer. Split state by domain or move complex workflows behind smaller domain reducers/state machines.

## 10. Senior trade-offs

### Local state vs global store
Local state minimizes coupling and is easier to reason about. A global store is justified when shared coordination, persistence, or cross-cutting client state materially benefits from centralized semantics.

### Context vs external store
Context is simple and excellent for scoped dependencies. An external store can provide more granular subscriptions and an independent state lifecycle when a large shared graph of client state requires it.

### Reducer vs event handlers
Reducers make transitions explicit and testable. For simple independent state, a reducer can introduce unnecessary ceremony.

### Single store vs multiple stores
A single store can create a recognizable application boundary, while multiple focused stores can reduce coupling. Choose boundaries around domains and update patterns rather than ideology.

## Senior follow-ups

- How would you prevent a Context provider from becoming a global god-object?
- When is a reducer a better model than several useState calls?
- Why is server state different from client state?
- How does useSyncExternalStore change the contract for external subscriptions?
- How would you migrate a prop-drilled feature to Context without making everything global?
- When would you reject Redux even in a large React application?
- How would you debug a state bug caused by stale cache rather than stale React state?
- What state would you keep local in a large trading or operations dashboard?

## Definition of Done

A candidate should be able to choose and justify local state, lifted state, Context, reducers, external stores, or Redux-style architecture based on ownership, update frequency, coupling, lifecycle, and operational needs rather than library preference.
