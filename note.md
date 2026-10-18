# React practice notes — four-year experience preparation

This repository is a practice path, not a checklist to memorize. For each challenge, implement the happy path, handle failures and edge cases, then explain your decisions as if you were reviewing the code with a teammate.

## How to use the practice app

1. Pick a challenge from the curriculum and build it without copying a solution.
2. Review the example code and interact with its preview; treat it as a starting pattern, not a full answer.
3. Write down the state shape, who owns it, and which values are derived.
4. Include loading, empty, error, and success states when the feature needs them.
5. Test keyboard use and accessible names as well as the visual result.
6. Profile before optimizing. Record what you measured and whether the change helped.
7. Mark the practice complete only when you can explain the trade-offs and failure cases.

## Suggested four-week refresh

| Week | Focus | Practice |
| --- | --- | --- |
| 1 | Components, TypeScript, state, rendering, forms, accessibility | Profile cards, controlled forms, searchable lists, accordion |
| 2 | Hooks, effects, async behavior, context, reducers, routing | Data fetching, debounce, theme context, cart reducer, issue routes |
| 3 | Testing, performance, resilience, reusable APIs | Component tests, profiling, error boundary, compound components |
| 4 | Data flows, architecture, communication | Optimistic updates, pagination, design system, issue-tracker capstone |

Adjust the pace to your schedule. Prioritize understanding and a small number of complete features over rushing through every item.

## Core concepts to be able to explain

### Components, props, and rendering

- Components describe UI for the current props and state; rendering should stay pure.
- Props flow down; events and callbacks communicate intent back up.
- Use stable keys that identify items across insertion, deletion, and reordering.
- Prefer composition to large components with many unrelated modes.
- Keep render branches explicit for loading, empty, error, and success states.

### State and data flow

- Keep one source of truth and lift state only to the nearest common owner.
- Treat state as immutable. Use functional updates when the next value depends on the previous value.
- Store essential state; derive values such as totals, filtered lists, and validation summaries.
- Use local component state for local UI, URL state for shareable navigation, and a deliberate shared-state solution for cross-feature needs.
- `useReducer` is useful when transitions are related, numerous, or easier to express as actions.

### Effects and async work

- An effect synchronizes with an external system (for example a subscription, timer, or browser API).
- Do not use an effect just to calculate a value that can be derived while rendering.
- Clean up subscriptions and timers. Abort or ignore obsolete requests so stale responses cannot win.
- Handle HTTP failures explicitly; `fetch` does not reject solely because the server returned a non-OK status.
- Distinguish client UI state from server state; caching, retries, deduplication, and invalidation may justify a query library.

### Controlled and uncontrolled inputs

| Feature | Controlled | Uncontrolled |
| --- | --- | --- |
| Value ownership | React state | DOM |
| Common API | `value` + `onChange` | `defaultValue` + `ref` |
| Reading values | Available in state on every render | Read from the DOM when needed |
| Live validation and dependent UI | Straightforward | Requires additional coordination |
| Typical use | Dynamic forms and conditional fields | Simple forms or integrations with DOM-based libraries |

Choose based on the form's needs rather than treating one approach as universally better. React Hook Form can use uncontrolled inputs to reduce rerenders; Zod can provide runtime validation and an inferred TypeScript type.

### Hooks and context

- Call hooks at the top level of a component or custom hook, never inside conditions or loops.
- Custom hooks share stateful logic; each call has its own state.
- Context helps distribute a value to consumers, but changing provider values can rerender those consumers.
- Keep providers close to where the value is needed and split context when update frequency or ownership calls for it.
- `useMemo` and `useCallback` are performance tools; measure before adding them.
- Use refs for imperative DOM access or mutable values that should not trigger rendering. Use `useId` for stable accessibility relationships, not list keys.
- Use `useSyncExternalStore` when React must subscribe to state owned outside React; otherwise prefer state, reducers, or context.

### Performance

- Start with React DevTools Profiler and a reproducible interaction.
- Find the expensive render or large DOM before choosing a fix.
- Consider stable component boundaries, avoiding needless state updates, and list virtualization for large collections.
- Use transitions to mark non-urgent updates; keep controlled text-input updates urgent. `useDeferredValue` is not a time-based debounce.
- Lazy-load meaningful route or feature boundaries; make the loading state intentional.
- Compare production behavior and bundle output, not only development impressions.

### Server rendering and hydration

- Hydration attaches client-side behavior to HTML rendered on the server; initial client markup must match server output.
- Avoid time, randomness, and browser-only values in the initial render unless they are made deterministic.
- Follow the chosen framework's server/client component and data-loading model rather than assuming every component runs only in the browser.

### Accessibility and quality

- Prefer semantic HTML and native controls; use ARIA to clarify behavior, not to replace it.
- Ensure visible focus, labels, keyboard access, and meaningful status/error announcements.
- Test user-observable behavior and important failure paths; avoid testing implementation details.
- Keep error boundaries around meaningful failure domains and provide a recovery path.
- Validate untrusted runtime data. A TypeScript type does not validate an API response or parsed JSON.

## Four-year interview preparation

For each design or code exercise, practice answering:

1. What is the source of truth, and why does each state value live here?
2. What happens for loading, empty, error, retry, and rapid repeated interaction?
3. How does the feature behave with keyboard navigation and assistive technology?
4. What would you test, at which layer, and what would you mock?
5. What performance bottleneck did you measure, and what evidence supports the optimization?
6. What trade-off did you choose, what alternatives did you reject, and when would you revisit it?

At this level, interviewers often value clear reasoning, realistic edge cases, maintainability, and collaboration as much as API recall. Be ready to review code, describe a migration or production incident, and explain how you would keep a feature reliable as requirements grow.

## Run the project

```bash
npm install
npm run dev
```

Run `npm run build` and `npm run lint` before sharing changes.
