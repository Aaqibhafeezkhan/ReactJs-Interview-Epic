## Goal

Make ReactJs-Interview-Epic a deep, practical React interview reference that is structured for progressive learning and senior-level discussion rather than a flat question list.

## Phases

### Phase 0 — Content baseline
- [x] Inventory current topics/questions and identify duplication.
- [x] Define answer-depth and navigation conventions.
- Completed via [Issue #5](https://github.com/Aaqibhafeezkhan/ReactJs-Interview-Epic/issues/5) and [PR #6](https://github.com/Aaqibhafeezkhan/ReactJs-Interview-Epic/pull/6)

### Phase 1 — React fundamentals
- [x] Strengthen rendering, reconciliation, component, props, and state coverage.
- [x] Add practical examples and common mistakes.

Completed via [Issue #11](https://github.com/Aaqibhafeezkhan/ReactJs-Interview-Epic/issues/11) and [PR #12](https://github.com/Aaqibhafeezkhan/ReactJs-Interview-Epic/pull/12)

### Phase 2 — Hooks and effects
- [x] Cover hook semantics, dependencies, custom hooks, and async behavior.
- [x] Add production failure scenarios and follow-ups.

Implemented in [Issue #9](https://github.com/Aaqibhafeezkhan/ReactJs-Interview-Epic/issues/9) and [PR #10](https://github.com/Aaqibhafeezkhan/ReactJs-Interview-Epic/pull/10)

### Phase 3 — State and architecture
- [x] Cover Context, Redux/state libraries, server state, and boundaries.
- [x] Explain trade-offs and architecture choices.

Completed via [Issue #13](https://github.com/Aaqibhafeezkhan/ReactJs-Interview-Epic/issues/13) and [PR #14](https://github.com/Aaqibhafeezkhan/ReactJs-Interview-Epic/pull/14)

### Phase 4 — Performance
- [x] Cover memoization, rendering diagnosis, profiling, and common performance traps.
- [x] Add debugging scenarios.

Completed via [Issue #15](https://github.com/Aaqibhafeezkhan/ReactJs-Interview-Epic/issues/15) and [PR #16](https://github.com/Aaqibhafeezkhan/ReactJs-Interview-Epic/pull/16)

### Phase 5 — Forms, routing and resilience
- [x] Cover production forms, validation, routing, error/loading states, request lifecycle handling, and resilient UX.
- [x] Add stale-response handling, retries, optimistic UI, accessibility, production debugging, and senior follow-ups.

Completed via [Issue #17](https://github.com/Aaqibhafeezkhan/ReactJs-Interview-Epic/issues/17) and [PR #18](https://github.com/Aaqibhafeezkhan/ReactJs-Interview-Epic/pull/18)

### Phase 6 — TypeScript and testing
- [ ] Strengthen React + TypeScript patterns.
- [ ] Add testing strategy, mocks, component and integration examples.

### Phase 7 — Accessibility and modern React
- [ ] Cover accessibility and semantic React practices.
- [ ] Add modern server/rendering architecture topics.

### Phase 8 — Frontend system design
- [ ] Add architecture/design prompts.
- [ ] Cover scaling, caching, data flow, performance, and trade-offs.

### Phase 9 — Senior scenarios and debugging
- [ ] Add production incidents, debugging exercises, and interviewer follow-ups.
- [ ] Distinguish junior/mid/senior expectations where useful.

### Phase 10 — Content QA and release readiness
- [ ] Review technical correctness and currentness.
- [ ] Remove shallow/duplicate material.
- [ ] Verify examples and navigation.
- [ ] Mark the guide release-ready.

## Definition of Done

The repository is a navigable, technically current React interview reference with substantial answers, runnable examples where useful, trade-offs, follow-ups, scenarios, and clear senior-level depth.


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
