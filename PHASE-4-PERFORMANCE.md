# Phase 4 — Performance

Performance work should start with measurement, not with sprinkling memoization everywhere. The interview goal is to explain where time is spent, identify the bottleneck, and choose the smallest change that improves the user-visible outcome.

## 1. React work and browser work

### Q1. What does a React performance problem actually mean?
It means some user-visible work is taking too long or happening too often. The bottleneck may be React rendering, DOM updates, JavaScript computation, layout/paint, network activity, or a combination.

### Q2. Does every React render mean the DOM changed?
No. React can re-run components and reconcile the result without producing meaningful host updates. Measure both render activity and browser work before deciding what to optimize.

### Q3. What should you measure?
Look at the user-facing symptom first: interaction latency, initial load, route transition time, rendering duration, dropped frames, or resource size. Then use React DevTools Profiler and browser performance tools to identify the expensive path.

### Q4. Why is premature optimization dangerous?
It adds complexity before the bottleneck is understood. Extra memoization can make code harder to read, create stale dependency mistakes, and still fail to solve the actual problem.

## 2. React.memo

### Q5. What does React.memo do?
React.memo lets React reuse a component's previous rendered result when its props are considered equal. It can reduce unnecessary work for a component whose parent renders frequently but whose relevant inputs remain stable.

### Q6. Is React.memo always faster?
No. React still has to compare props, and the comparison itself has a cost. It helps when the avoided render work is materially more expensive than the comparison overhead.

### Q7. When is React.memo useful?
It is a candidate when profiling shows repeated renders of an expensive component caused by unchanged props. Stable component boundaries and predictable props make it more effective.

### Q8. Is React.memo a correctness feature?
No. Correct code must not depend on memoization. Treat it as a performance optimization that can be removed without changing application behavior.

## 3. useMemo and useCallback

### Q9. What does useMemo do?
useMemo can cache the result of a calculation between renders when its dependencies have not changed. It is appropriate for genuinely expensive calculations when measurement justifies the retained value and dependency complexity.

### Q10. What does useCallback do?
useCallback can preserve a function identity between renders when its dependencies have not changed. It is mainly useful when function identity affects another optimization or subscription boundary.

### Q11. Why can useCallback be unnecessary?
Creating a function is usually cheap. Adding useCallback introduces dependency tracking and another layer of indirection. Use it when stable identity has a purpose, not as a default rule.

### Q12. Can useMemo fix a stale value bug?
No. A memoization hook with incorrect dependencies can itself create stale results. Dependencies are part of correctness for the cached calculation.

## 4. Stable identities and component boundaries

### Q13. Why can unstable object props hurt performance?
If a parent creates a new object on every render, a memoized child may see changed identity even when the logical values are equivalent. First question the data flow; only stabilize identities when it solves a measured problem.

### Q14. How do component boundaries affect performance?
A large component can cause unrelated work to be recalculated together. Splitting a component around meaningful state and rendering boundaries can reduce the scope of updates and clarify ownership.

### Q15. Should every child be memoized?
No. Memoize boundaries that have a measurable reason to avoid repeated work. Blanket memoization creates noise and can hide the actual architecture problem.

## 5. Lists and large datasets

### Q16. Why can large lists become slow?
Large lists increase rendering, reconciliation, DOM, layout, and event-handling costs. The right fix depends on which part is actually expensive.

### Q17. What is list virtualization?
Virtualization renders only the items near the visible viewport instead of mounting the entire collection. It reduces DOM and rendering work for very large lists.

### Q18. When would you not virtualize?
For small lists, virtualization can add complexity without a meaningful benefit. It can also complicate accessibility, measurement, keyboard navigation, and dynamic row sizing.

### Q19. How do keys affect list performance?
Stable keys help React preserve identity during insertions, deletions, and reordering. Unstable or random keys can cause unnecessary remounts and destroy local state.

## 6. Expensive computation

### Q20. Where should expensive computation live?
First determine whether it is truly required during rendering. If the computation is derived from inputs and expensive, it may be memoized or moved to a more appropriate boundary such as a worker or server depending on the workload.

### Q21. What is a common mistake with expensive filters?
Filtering or sorting a large dataset on every keystroke can block interaction. Debouncing the input may reduce request frequency, but client-side computation still needs profiling.

### Q22. When would you use a Web Worker?
A worker can move CPU-heavy JavaScript away from the main thread. It is useful for substantial computation where responsiveness matters and the data-transfer cost is acceptable.

## 7. Context and rendering fan-out

### Q23. How can Context create performance problems?
A frequently changing context value can invalidate many consumers at once. The architectural fix may be to split context responsibilities or change the ownership boundary rather than adding memoization everywhere.

### Q24. Does React.memo stop context-driven updates?
Not by itself. A component reading a context can still update when that context value changes. The right fix is usually to narrow context subscriptions or move the changing value to a better state boundary.

## 8. Profiling methodology

### Q25. How would you investigate a slow dashboard interaction?
First reproduce the exact interaction and define the symptom, such as input lag or a long commit. Record a React Profiler trace and a browser performance trace, identify the dominant work, change one variable, and measure again.

### Q26. What makes a profiler result actionable?
It should identify a repeatable user action, an expensive component or browser task, and a plausible causal path. A long list of timings without a hypothesis does not automatically tell you what to change.

### Q27. How do you know an optimization worked?
Compare the same workload before and after the change. Use user-facing metrics where possible and ensure the optimization did not shift the cost elsewhere or create correctness regressions.

## 9. Browser-level performance

### Q28. What is a long task?
A long task is a main-thread task that occupies enough time to delay other work, including responsive input and rendering opportunities. Large JavaScript computations are a common cause.

### Q29. What are layout and paint concerns in React apps?
React can trigger DOM changes that cause the browser to perform layout and paint. A React render may be inexpensive while the resulting DOM work or CSS effects are expensive, so browser profiling matters.

### Q30. How does code splitting help?
Code splitting can reduce the JavaScript needed for an initial route by loading some modules only when needed. The trade-off is additional requests, latency, and coordination around loading states.

### Q31. When does lazy loading help?
It is useful when a feature is not required for the initial user experience. Large rarely used routes or components are stronger candidates than small frequently used primitives.

## 10. Common performance traps

### Trap: memoizing everything
Measure first. If there is no meaningful avoided work, remove the optimization.

### Trap: deriving huge arrays on every render
Check whether the calculation can be reduced, moved, memoized, paginated, or performed against a smaller dataset.

### Trap: using context for high-frequency granular state
Narrow the provider boundary or consider a subscription model that lets consumers observe only the slice they need.

### Trap: fixing a slow API with frontend memoization
Caching an expensive UI calculation does not fix network, backend, or database latency. Trace the whole request path.

### Trap: blaming React for browser jank
Inspect long tasks, layout, paint, image decoding, and third-party scripts. The bottleneck may be outside React.

## 11. Debugging scenarios

### Scenario: typing into a search box lags
Profile the keystroke. Check synchronous filtering, component fan-out, expensive derived data, and network work triggered on every keypress. Separate urgent input updates from non-urgent work where appropriate.

### Scenario: a memoized table still re-renders all rows
Inspect prop identity, context usage, parent state boundaries, and whether the table actually needs to render all rows. Memoization cannot help if relevant inputs change every time.

### Scenario: a dashboard becomes slower as more widgets are added
Look for broad state/context subscriptions and duplicated calculations. Measure which widgets respond to each update and introduce narrower boundaries where they provide real benefit.

### Scenario: initial page load is slow
Separate server latency, document delivery, JavaScript download, parsing, execution, and rendering. Reduce the dominant cost rather than assuming the answer is React.memo.

### Scenario: scrolling a large table drops frames
Profile main-thread work during scrolling. Check DOM size, row rendering, synchronous handlers, layout, and virtualization before changing component memoization.

## 12. Senior trade-offs

### Memoization vs simpler code
Memoization can reduce repeated work but adds dependency and identity complexity. Prefer it when measurements show a material win.

### Virtualization vs straightforward rendering
Virtualization improves very large collections but adds complexity around accessibility, dynamic sizes, and scroll behavior. Use it when scale justifies the complexity.

### Client optimization vs backend optimization
A fast component cannot compensate for a slow API. Establish whether the dominant latency is client, network, or server before optimizing.

### Splitting components vs over-fragmenting
Meaningful boundaries can reduce update scope. Excessive fragmentation can increase prop plumbing and cognitive load. Optimize around state and rendering responsibilities.

### Performance vs freshness
Caching and memoization trade memory and freshness for speed. The correct policy depends on the data and user expectation.

## Senior follow-ups

- How would you prove a React.memo change improved a production interaction?
- When does Context become a rendering bottleneck?
- How would you profile a React application that is visually smooth but CPU-heavy?
- What would you inspect when React Profiler looks fine but the UI still drops frames?
- How would you decide between virtualization and pagination?
- When would you move computation to a Web Worker or the server?
- How would you prioritize performance work when the slow path affects only a small user segment?
- What performance budget would you define for a large operations dashboard?

## Definition of Done

A candidate should be able to diagnose React and browser performance problems with measurement, explain memoization and rendering trade-offs, reason about large lists and context fan-out, and optimize the actual bottleneck rather than applying generic performance folklore.
