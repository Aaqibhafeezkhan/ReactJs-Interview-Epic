# React Interview Content Standard

## Purpose

This repository is a progressive React interview reference, not a flat trivia dump. New material should help a candidate explain React's mental model, browser/runtime behavior, production consequences, and architecture trade-offs.

## Canonical answer shape

For substantive questions, use:

1. **Interview answer** — 2–5 sentences that can be spoken naturally.
2. **Mental model / mechanism** — what React or the browser is actually doing.
3. **Practical example** — a small React/TypeScript example when code clarifies the idea.
4. **Trade-offs and pitfalls** — performance, correctness, accessibility, security, or maintainability implications.
5. **Senior follow-up** — one or two natural interviewer follow-ups.

Rapid-fire questions can remain concise. Do not add filler just to satisfy the structure.

## Content levels

- **Foundation:** components, JSX, props, state, rendering, events, and basic browser concepts.
- **Practical:** Hooks, effects, forms, routing, data fetching, testing, TypeScript, accessibility, and common bugs.
- **Senior:** rendering behavior, concurrency, performance diagnosis, server/client boundaries, security, architecture, and production debugging.
- **Staff/system design:** application boundaries, scaling, caching, delivery architecture, migration, reliability, observability, and trade-offs across teams.

## Canonical topic taxonomy

Use the existing README structure as the primary navigation:

- JavaScript prerequisites
- Browser/web fundamentals
- React fundamentals
- JSX/rendering
- Props/state/data flow
- Hooks
- Effects
- Refs/DOM
- Context/state architecture
- Forms
- Suspense/transitions/scheduling
- React 19+
- React Compiler
- Server Components/Server Functions
- SSR/hydration/streaming
- Routing
- TypeScript + React
- Data fetching/APIs
- Performance
- Accessibility
- Testing
- Security
- Tooling/build systems
- Architecture/design patterns
- Debugging/production
- Frontend system design
- Coding/machine coding
- Senior/staff
- Behavioral/project questions
- Rapid-fire revision

Cross-topic questions should have one canonical home. Link to related material instead of maintaining duplicated explanations.

## Duplication policy

Before adding a question:

1. Search the repository for the concept and likely wording.
2. Improve an existing question when it already tests the same idea.
3. Keep separate questions only when they test meaningfully different levels or scenarios.
4. Cross-reference canonical explanations instead of copying large sections.

Known overlap risks include rendering/reconciliation explanations, effects versus event-driven logic, memoization/performance, server/client boundaries, data-fetching/caching, and production debugging appearing in multiple broad sections and scenario/drill documents. Later phases should consolidate these where useful.

## Modern React and version-awareness

The repository currently targets modern React and records React 19.2 as the documented baseline. Version-sensitive behavior must identify the relevant React generation when it materially affects the answer.

Prefer current primary React documentation for API semantics. Distinguish stable public APIs from framework-specific behavior, experimental features, and implementation details.

Do not teach legacy patterns as current best practice without explicitly labeling them as legacy or explaining why an interviewer may still ask about them.

## Code examples

Examples should be:

- minimal and focused on the concept
- valid modern React/TypeScript where applicable
- explicit about client/server boundaries when relevant
- accessible by default for UI examples
- free of unnecessary framework boilerplate
- accompanied by the behavior or trade-off the interviewer should notice

Avoid examples that hide dependency-array mistakes, stale closures, unsafe DOM usage, insecure rendering, or unnecessary memoization.

## Baseline inventory

The current README is a large question bank covering JavaScript/browser prerequisites, React core, Hooks/effects, state architecture, forms, concurrency, React 19+, Compiler, Server Components/Server Functions, SSR/hydration/streaming, routing, TypeScript, data fetching, performance, accessibility, testing, security, tooling, architecture, production debugging, system design, coding exercises, senior/staff topics, behavioral/project questions, and rapid-fire revision.

The repository also contains dedicated advanced-drill and interview-scenario material. Phase 0 intentionally does not rewrite that content. It establishes the conventions for the substantive React-fundamentals work that follows.

## Review checklist for future content

- Is the React behavior technically accurate for the documented baseline?
- Can the answer be spoken clearly in an interview?
- Does the explanation use the right mental model rather than implementation folklore?
- Is the example minimal and realistic?
- Are common mistakes and trade-offs covered where useful?
- Are accessibility and security implications mentioned when relevant?
- Are version-sensitive claims identified?
- Is the question genuinely distinct from existing material?
- Does the answer belong in the chosen canonical section?
