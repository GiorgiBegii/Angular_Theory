# Signals

Signals are Angular's reactive primitive for synchronous state.

Simple idea:

```text
state changes
Angular knows exactly who used that state
only affected views/computed values update
```

Signals are best for local UI state, derived state, and values that the template reads directly.

## signal

`signal()` creates writable reactive state.

```ts
count = signal(0);

increment() {
  this.count.update(value => value + 1);
}
```

Read a signal by calling it:

```ts
this.count();
```

In a template:

```html
{{ count() }}
```

Use `set()` when replacing the value:

```ts
this.count.set(10);
```

Use `update()` when the new value depends on the previous value:

```ts
this.count.update(value => value + 1);
```

## computed

`computed()` creates derived state from other signals.

```ts
firstName = signal('Giorgi');
lastName = signal('Begi');

fullName = computed(() => `${this.firstName()} ${this.lastName()}`);
```

`computed` is:

- read-only
- derived from signals
- memoized
- recalculated only when its dependencies change

Use `computed` instead of manually storing values that can be calculated from existing state.

Bad:

```ts
firstName = signal('Giorgi');
lastName = signal('Begi');
fullName = signal('Giorgi Begi');
```

Better:

```ts
fullName = computed(() => `${this.firstName()} ${this.lastName()}`);
```

## effect

`effect()` runs side-effect code when the signals it reads change.

```ts
effect(() => {
  console.log('Count changed:', this.count());
});
```

Use `effect` for side effects:

- logging
- syncing with localStorage
- calling imperative browser APIs
- connecting to non-Angular libraries

Do not use `effect` to calculate normal derived state. Use `computed` for that.

Bad:

```ts
effect(() => {
  this.fullName.set(`${this.firstName()} ${this.lastName()}`);
});
```

Better:

```ts
fullName = computed(() => `${this.firstName()} ${this.lastName()}`);
```

## Dependency Tracking

Signals automatically track dependencies.

```ts
fullName = computed(() => {
  return `${this.firstName()} ${this.lastName()}`;
});
```

Angular knows `fullName` depends on:

- `firstName`
- `lastName`

When `firstName` changes, `fullName` becomes dirty. When `lastName` changes, `fullName` becomes dirty. Other unrelated signals do not affect it.

## Signals and Change Detection

Signals work very well with [[Change Detection#OnPush strategy|OnPush]].

When a signal is read in an Angular template, Angular tracks that dependency. When the signal changes, Angular marks only the affected view for update.

```ts
count = signal(0);
```

```html
<button (click)="count.update(value => value + 1)">
  {{ count() }}
</button>
```

This gives Angular more precise information than "something async happened, check everything."

## Signals vs RxJS

Signals and RxJS solve different problems.

| Use case | Prefer |
| --- | --- |
| local component state | Signals |
| derived UI values | `computed` |
| template state | Signals |
| HTTP streams | RxJS |
| router/form/event streams | RxJS |
| cancellation | RxJS |
| complex async composition | RxJS |

Short rule:

```text
RxJS = async streams over time
Signals = current reactive state
```

They can work together. Common pattern:

```text
RxJS loads data
Signals store/display current UI state
```

See [[RxJS#RxJS and Signals Interop|RxJS and Signals Interop]].

## toSignal and toObservable

`toSignal()` converts an Observable into a signal.

Useful when the template wants current state from an Observable source.

```ts
users = toSignal(this.userService.users$, {
  initialValue: [],
});
```

`toObservable()` converts a signal into an Observable.

Useful when signal state must enter an RxJS pipeline.

```ts
query$ = toObservable(this.query);
```

Use conversion at boundaries. Do not randomly mix Signals and RxJS inside the same logic.

## Signal Inputs

Angular supports signal-based component inputs.

```ts
user = input.required<User>();
```

Read it like a signal:

```ts
this.user();
```

Signal inputs make input values reactive and work naturally with `computed`.

```ts
userName = computed(() => this.user().name);
```

## model

`model()` is used for two-way binding with signals.

```ts
value = model('');
```

Parent usage:

```html
<app-search [(value)]="searchText" />
```

Use `model()` when a component owns an editable value and needs to expose two-way binding.

## linkedSignal

`linkedSignal()` creates writable state that is connected to another reactive source.

Common use case:

- reset selected item when the source list changes
- keep editable state connected to incoming data
- derive initial value but still allow local writes

Simple idea:

```text
computed = derived and read-only
linkedSignal = derived initial/source value but writable
```

## resource

`resource()` is used for async state connected to signals.

It can represent:

- loading
- value
- error
- reload behavior

Useful when async work depends on signal state, for example loading users by selected id.

For stream-heavy async logic, RxJS is still often clearer.

## Signals Store

A Signals-based store is a service or feature state object that stores state in signals and exposes derived values with `computed`.

```ts
@Injectable({ providedIn: 'root' })
export class UserStore {
  private readonly users = signal<User[]>([]);
  private readonly selectedId = signal<string | null>(null);

  readonly allUsers = this.users.asReadonly();

  readonly selectedUser = computed(() => {
    const id = this.selectedId();
    return this.users().find(user => user.id === id) ?? null;
  });

  setUsers(users: User[]) {
    this.users.set(users);
  }

  selectUser(id: string) {
    this.selectedId.set(id);
  }
}
```

Good for:

- local feature state
- selected item state
- filters/search state
- derived UI state
- simple shared state

Use RxJS or ComponentStore when state is stream-heavy, has complex cancellation, polling, retries, or many async workflows.

## Best Practices

- Keep writable signals private when possible.
- Expose read-only signals from services/stores.
- Use `computed` for derived values.
- Use `effect` only for side effects.
- Keep signal updates immutable.
- Avoid writing to signals from inside `computed`.
- Avoid using `effect` as a replacement for normal data flow.
- Convert between RxJS and Signals only at clear boundaries.
- Use Signals with `OnPush` for predictable UI updates.

## Common Mistakes

- using `effect` to derive state instead of `computed`
- mutating arrays/objects inside a signal without replacing the reference
- mixing RxJS and Signals everywhere without clear ownership
- creating too much global state for simple component state
- exposing writable signals directly from services
- doing HTTP logic directly inside components when a service/store should own it

## Short Answer

Angular Signals are reactive state values. `signal` stores state, `computed` derives state, and `effect` runs side effects when signal values change. Signals automatically track dependencies, update affected views efficiently, work well with `OnPush`, and are best for local or feature UI state. RxJS is still better for async streams, cancellation, and complex event flows.

## Related Notes

- [[Change Detection]]
- [[Change Detection#OnPush strategy|OnPush]]
- [[RxJS]]
- [[RxJS#RxJS and Signals Interop|RxJS and Signals Interop]]
- [[State Management]]
- [[State Management#ComponentStore|ComponentStore]]
- [[Immutability]]
