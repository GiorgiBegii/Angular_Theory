# State Management

State management means deciding where application data lives, who can change it, and how the UI reacts to changes.

Simple idea:

```text
state has an owner
updates are explicit
views react to state
```

## Local Component State

Use local component state for values used only by one component.

Examples:

- dialog open/closed
- selected tab
- local filter text
- temporary UI flags

Signals are a good fit for local synchronous UI state.

## Service Singleton

A root service can hold app-wide shared state.

Good for:

- current user
- auth state
- settings
- selected language
- app configuration

Be careful: root services live for the whole app lifetime, so they should not hold feature-local state by accident.

## Signals Store

A Signals-based store keeps state in signals and derived values in `computed`.

Good for:

- local feature state
- selected item
- filters
- derived UI values
- simple shared state

See [[Signals#Signals Store|Signals Store]].

## ComponentStore

ComponentStore is useful for more complex RxJS-heavy local or feature state.

Good for:

- async effects
- loading/error state
- pagination
- cancellation
- stream composition
- feature-level state with clear update methods

Use it when plain signals/services become too informal or stream-heavy.

## RxJS State

RxJS state is useful when data is naturally stream-based.

Examples:

- router params
- form value changes
- WebSocket events
- polling
- cancellation
- search/autocomplete

Use RxJS for async streams and Signals for current UI state when that split is cleaner.

## Good State Rules

- one clear owner for each piece of state
- avoid duplicated source of truth
- keep updates explicit
- keep derived state derived, not manually duplicated
- expose read-only state to consumers
- keep async side effects in services/stores/facades
- reset feature state when feature lifetime ends

## Short Answer

Suitable local state patterns depend on complexity. A service singleton is good for simple shared app state. Signals are good for modern local or feature UI state and derived values. ComponentStore is useful for RxJS-heavy feature state with effects, loading/error flows, pagination, or complex async logic.

## Related Notes

- [[Signals]]
- [[Signals#computed|computed]]
- [[Signals#Signals Store|Signals Store]]
- [[RxJS]]
- [[Dependency Injection#Provider Scope|Provider Scope]]
- [[Angular Architecture]]
