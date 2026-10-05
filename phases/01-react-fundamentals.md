# Phase 1 — React Fundamentals

This phase builds the core React mental model needed before discussing advanced Hooks, state libraries, performance, Server Components or frontend system design.

## 1. Rendering mental model

### Q1. What happens when React renders a component?
React calls the component to produce React elements. It compares the resulting tree with the previous rendered result and determines what work is needed. The render phase should be treated as a pure calculation. The commit phase applies the necessary host-environment changes.

A render does not mean React immediately mutates the DOM.

### Q2. Why should rendering be pure?
Modern React may render work more than once, interrupt it, restart it or discard it before commit. Pure rendering makes this safe. Side effects belong in the appropriate event or effect mechanisms rather than during render.

### Q3. Does every render update the DOM?
No. A component can render while React determines that the resulting host output does not require a DOM change. Rendering and committing host mutations are separate concepts.

## 2. Components, props and identity

### Q4. What is a React component?
A component is a reusable unit that receives inputs and returns UI. In modern React, function components are the normal model.

### Q5. Are props mutable?
Treat props as immutable inputs. A component should not mutate the object it receives from its parent. If a value needs to change, the owner of that state should update it and pass the new value down.

### Q6. What determines component identity?
React uses the position and key information in the rendered tree to determine whether an existing component instance can be reused. Changing identity can cause state to be reset.

### Q7. Why are keys important?
Keys give list items stable identity among siblings. They allow React to distinguish insertion, deletion and reordering correctly. A key should describe the item's stable identity, not merely its current position.

### Q8. Why can index keys be problematic?
If a list is reordered, inserted into or filtered, an index can now refer to a different logical item. React may preserve state with the wrong item. Index keys can be acceptable for truly static lists whose order never changes.

## 3. State

### Q9. What is state?
State is data owned by a component or another state abstraction that participates in rendering. Updating state schedules a new render; it should not be thought of as directly mutating the current render's value.

### Q10. Why doesn't setting state immediately change the current value?
A render is a snapshot. Event handlers and other functions created during that render close over that snapshot. A state update requests a future render with the new state.

### Q11. Why use functional state updates?
When the next state depends on the previous state, the updater form makes that dependency explicit and composes correctly across multiple queued updates.

### Q12. What is derived state?
Derived state is a value that can be calculated from existing props/state. Storing it separately often creates synchronization problems. Prefer deriving it during render unless computation is genuinely expensive or the value represents independent state.

### Q13. What is state ownership?
Put state at the lowest common ancestor that needs to coordinate it, while keeping purely local state local. Lift state when siblings need shared ownership; avoid global state merely for convenience.

## 4. Reconciliation and keys

### Q14. What is reconciliation?
It is React's process of comparing the current rendered element tree with the previous one to determine what should be preserved, updated or removed.

### Q15. What happens when element types change?
Changing the type at a position can cause the previous component subtree to be replaced, which can reset state below that point. Stable structure and intentional keys are therefore important when state preservation matters.

### Q16. Are keys passed as props?
No. key is special React metadata used for reconciliation. If a component needs an ID as an input, pass it explicitly as another prop.

## 5. State updates and batching

### Q17. Are state updates synchronous?
Do not describe them as simple synchronous assignments. State setters schedule updates. React may batch updates and choose when to render/commit them according to its scheduling model.

### Q18. Why can multiple state updates appear to use the same old value?
Because each handler sees the state snapshot from its render. When successive updates depend on prior state, use functional updaters.

### Q19. What is batching?
React can group multiple state updates so they result in fewer renders/commits. Interview answers should focus on the observable guarantee rather than relying on old React-version folklore about exactly which event boundaries batch.

## 6. Controlled and uncontrolled components

### Q20. Controlled vs uncontrolled input?
A controlled input gets its current value from React state and reports changes back through an event handler. An uncontrolled input keeps its current value in the DOM and can be accessed with a ref when needed.

Controlled inputs are useful when UI behavior depends continuously on the value. Uncontrolled inputs can be simpler for forms where React does not need to mirror every keystroke.

### Q21. What is state colocation?
Keep state close to the components that use it. Colocation reduces unnecessary coupling and makes ownership easier to understand.

## 7. Common mistakes

### Mistake: storing every computed value in state
This creates two sources of truth. Prefer deriving values from existing inputs.

### Mistake: using useEffect to respond to a click
If something should happen because a user clicked a button, put it in the event handler. Effects synchronize with external systems; they are not a general replacement for event logic.

### Mistake: using random keys
Random keys destroy stable identity and can cause unnecessary remounting and state loss.

### Mistake: mutating state objects
Mutating an existing object can make updates harder to reason about and can defeat identity-based change detection. Prefer producing a new value when state changes.

### Mistake: putting all state into Context
Context is a dependency-passing mechanism, not automatically a complete state-management architecture. Frequently changing or highly granular state may be better modeled locally or with a store/data-fetching solution.

## 8. Debugging scenarios

### Scenario: A list item's input state moves to another row
Check the keys first. If indexes are used and the list can reorder/filter/insert, the key is likely not representing stable identity.

### Scenario: UI shows stale state inside an event handler
Remember that the handler observes the render snapshot from which it was created. If the operation depends on the latest queued state, use a functional updater or restructure the logic.

### Scenario: A component remounts unexpectedly
Inspect its type and key at the relevant tree position. Changing keys intentionally resets state; unstable keys can cause accidental remounts.

### Scenario: A component renders more often than expected
First establish what actually changed. Check parent renders, state updates, context updates and subscriptions. Do not jump directly to React.memo; determine the source and whether the extra render has measurable impact.

## 9. Senior-level trade-offs

### Local state vs global state
Prefer local ownership. Global state is justified when multiple distant consumers genuinely share client state or coordination requirements.

### Derived value vs stored value
Derive when possible. Store only when the value has independent lifecycle/ownership or deriving it is materially inappropriate.

### Context vs state library
Context works well for relatively stable dependencies and low-frequency shared values. A dedicated state library may provide better subscriptions, selectors, debugging and update isolation for complex client state.

### Client state vs server state
Server state has ownership, caching, freshness, synchronization and retry semantics that differ from ordinary client UI state. Treating server data as generic global state often creates unnecessary complexity.

## 10. Interview follow-ups

- Why can a parent render without every child producing DOM changes?
- How does a key affect state preservation?
- Why is index-as-key sometimes acceptable?
- Why is derived state often a code smell?
- What exactly does a state setter schedule?
- Why can functional updaters solve stale-state sequencing?
- When would you choose an uncontrolled input?
- Why is Context not the same thing as Redux?
- How would you debug unexpected remounting?
- What state would you colocate in a complex dashboard?
- How would you design state ownership for a reusable component?
- Which claims about reconciliation are implementation details versus public guarantees?

## Definition of Done

A candidate should be able to explain rendering versus commit, component identity, keys, state snapshots, state ownership, controlled/uncontrolled inputs and common architectural mistakes without relying on cargo-cult React rules.
