# Angular Architecture

Angular architecture is how an app is organized so features can grow without becoming tightly coupled.

Simple idea:

```text
clear boundaries
clear ownership
clear data flow
```

## Application Layers

Common layers:

- page/container components
- presentational components
- services/facades
- data-access/API services
- state stores
- shared UI/utilities
- route configuration

Each layer should have a clear responsibility.

## Dependency Boundaries

Dependency boundaries define what can import what.

Good direction:

```text
feature -> shared/core
shared -> no feature imports
core -> app-wide infrastructure
```

Bad direction:

```text
feature A imports internals from feature B
shared imports app-specific feature code
everything imports everything
```

Healthy boundaries make features easier to move, test, lazy-load, and refactor.

## Monorepo

A monorepo keeps multiple apps/libs/packages in one repository.

Useful when teams share:

- UI libraries
- utilities
- domain models
- data-access libraries
- design system code
- multiple Angular apps

The risk is uncontrolled coupling. A monorepo still needs clear boundaries.

## Nx

Nx is a common monorepo tool used with Angular.

It helps with:

- workspace libraries
- dependency graph
- affected builds/tests
- caching
- code generation
- enforcing module boundaries

Nx is useful when the codebase has multiple apps/libs or needs strong architecture rules.

## Feature Structure

Feature folders often contain:

```text
users/
+-- data-access/
+-- ui/
+-- pages/
+-- users.routes.ts
+-- users.store.ts
```

Keep code close to the feature that owns it. Promote code to shared only when it is truly reusable.

## Public APIs

A feature/library should expose only what other code needs.

Good:

```ts
export { UserCardComponent } from './ui/user-card.component';
export { USER_ROUTES } from './users.routes';
```

Avoid importing deep internal files from other features.

## Architecture Checklist

Check:

- routing is feature-based
- DI scope matches ownership
- state has clear owner
- forms are typed and validated
- HTTP concerns are centralized
- errors are handled consistently
- performance uses lazy loading and OnPush/signals
- i18n/a11y/SSR needs are considered
- feature boundaries are enforced

## Short Answer

Angular architecture is about organizing routing, DI, state, forms, HTTP, errors, performance, i18n, accessibility, and SSR around clear boundaries. A scalable app keeps features isolated, shared code truly reusable, dependencies flowing in predictable directions, and infrastructure concerns centralized.

## Related Notes

- [[Angular Router]]
- [[Dependency Injection]]
- [[State Management]]
- [[Angular Performance]]
- [[Production Readiness]]
- [[Modules and Application Structure]]
