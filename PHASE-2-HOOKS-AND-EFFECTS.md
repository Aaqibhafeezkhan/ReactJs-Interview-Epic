# Phase 2 — Hooks and Effects

## Rules of Hooks
Hooks must be called at the top level of React function components or custom Hooks. Conditional, loop, or nested calls break the stable call order React uses to associate Hook state with a call site.

## Effects as synchronization
An Effect synchronizes rendered state with an external system such as a subscription, timer, browser API, or imperative widget. It is not a general-purpose lifecycle hook. Use an event handler for work caused directly by an interaction.

## Dependencies and stale closures
The dependency list describes reactive values read by the Effect. Omitting a changing dependency can make the Effect observe a stale render. When a state update depends on previous state, use a functional updater.

## Cleanup and async races
Cleanup should undo subscriptions and timers and should invalidate or cancel obsolete asynchronous work. AbortController is appropriate for fetch requests that support cancellation.

## Custom Hooks
Custom Hooks extract reusable stateful behavior while preserving the Rules of Hooks. Keep their public API small and focused on one reusable concern.

## Common senior traps
- Using Effects to derive data that can be calculated during render.
- Omitting dependencies only to silence lint feedback.
- Starting subscriptions without cleanup.
- Allowing an obsolete async result to overwrite newer state.
- Treating memoization as a correctness mechanism.

## Senior follow-ups
- Why can development tooling exercise Effect setup and cleanup more than once?
- When should an Effect be removed in favor of render logic or an event handler?
- How do you diagnose an Effect loop?
- How would you cancel or invalidate a stale request?

## Phase 2 checklist
- Rules of Hooks
- Effect synchronization model
- Event handlers versus Effects
- Dependency arrays and stale closures
- Cleanup and async races
- Custom Hooks
- Senior debugging scenarios
