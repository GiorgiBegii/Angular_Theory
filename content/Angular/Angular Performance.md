# Angular Performance

Angular performance means loading less code, rendering less work, and avoiding repeated expensive operations.

Simple idea:

```text
load less
check less
re-render less
repeat less work
```

## Code Splitting

Code splitting breaks the app into smaller JavaScript chunks.

In Angular, this usually happens through lazy routes:

- `loadComponent`
- `loadChildren`

Benefits:

- smaller initial bundle
- faster startup
- feature code loads only when needed

See [[Angular Router#Lazy Loading|Lazy Loading]].

## Route Preloading

Preloading loads lazy routes in the background after the app starts.

It balances:

- fast initial load
- faster later navigation

Use preloading for likely-to-be-used routes, not every heavy feature automatically.

## Caching

Caching stores expensive or repeated results.

Examples:

- HTTP response caching
- memoized derived data
- `shareReplay` for shared Observable results
- browser/CDN caching for static assets
- TransferState for SSR data reuse

Use caching when data is reused and freshness rules are clear.

## Change Optimization

Important tools:

- `OnPush`
- Signals
- `async` pipe
- stable `@for track`
- immutable updates
- avoiding heavy template functions

See [[Change Detection]].

## Rendering Rules

Avoid:

- calling heavy functions in templates
- recreating arrays/objects unnecessarily
- missing `track` in large lists
- impure pipes without a strong reason
- unnecessary subscriptions

Prefer:

- computed values
- pure pipes
- stable references
- lazy loading
- small presentational components

## Data Caching

Cache only when:

- repeated requests happen
- data does not change often
- stale data is acceptable or controlled
- invalidation strategy is clear

For RxJS HTTP caching, `shareReplay` is common, but use it carefully for long-lived streams.

## Budgets

Budgets are Angular build limits for bundle or asset sizes.

They help catch performance regressions during build/CI.

Examples:

- initial bundle too large
- component style too large
- lazy chunk too large

Budgets make performance visible before production.

## Short Answer

Recommended Angular performance techniques include lazy loading/code splitting, route preloading, OnPush or signal-driven change detection, stable `track`/`trackBy`, caching repeated data, avoiding heavy template work, image optimization, and bundle budgets. The main idea is to load less, render less, and avoid repeating expensive work.

## Related Notes

- [[Angular Router]]
- [[Angular Router#Lazy Loading|Lazy Loading]]
- [[Change Detection]]
- [[Change Detection#trackBy / track|trackBy]]
- [[Signals]]
- [[RxJS#shareReplay|shareReplay]]
