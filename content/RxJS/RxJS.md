# RxJS

RxJS is a library for working with asynchronous data streams.

In Angular, RxJS is commonly used with:

- `HttpClient`
- reactive forms
- router params and events
- component state
- user input streams
- WebSocket/event streams
- cancellation and error handling

## Observable

An Observable is a lazy stream of values over time.

It can emit:

- next values
- an error
- a completion signal

```ts
const users$ = this.http.get<User[]>('/api/users');
```

The `$` suffix is a common convention for Observable values.

## Subscribe

An Observable usually starts when someone subscribes.

```ts
users$.subscribe({
  next: users => console.log(users),
  error: error => console.error(error),
  complete: () => console.log('done'),
});
```

A subscription is the connection between the Observable and the consumer.

## Observable vs Promise

| Feature | Promise | Observable |
|---|---|---|
| Values | one value | zero, one, or many values |
| Execution | eager | lazy |
| Cancellation | not built in | unsubscribe |
| Composition | `.then()` / `async await` | operators |
| Angular usage | one-time async work | HTTP, forms, routing, events, streams |

## Cold Observable

A cold Observable creates a new independent execution for each subscriber.

```ts
const users$ = this.http.get<User[]>('/api/users');

users$.subscribe();
users$.subscribe();
```

With Angular `HttpClient`, each subscription can trigger a separate HTTP request.

Cold Observable examples:

- `HttpClient` requests
- `of(...)`
- `defer(...)`
- many custom Observables

Short idea: cold means each subscriber gets its own execution.

## Hot Observable

A hot Observable shares one execution between subscribers.

Examples:

- `Subject`
- DOM events
- WebSocket stream
- shared Observable with `share()`
- cached stream with `shareReplay()`

Short idea: hot means the source is already active or shared.

## RxJS Operators

RxJS operators are functions used inside `pipe()` to transform, filter, combine, control, or share Observable streams.

```ts
this.searchControl.valueChanges.pipe(
  debounceTime(300),
  distinctUntilChanged(),
  switchMap(query => this.api.search(query))
);
```

## Operator Categories

Creation operators create Observables:

- `of`
- `from`
- `interval`
- `timer`
- `fromEvent`
- `defer`

Transformation operators change values or switch streams:

- `map`
- `scan`
- `switchMap`
- `mergeMap`
- `concatMap`
- `exhaustMap`

Filtering operators control which values continue:

- `filter`
- `debounceTime`
- `distinctUntilChanged`
- `take`
- `takeUntil`
- `first`

Combination operators combine streams:

- `combineLatest`
- `forkJoin`
- `merge`
- `zip`
- `withLatestFrom`

Multicasting operators share execution:

- `share`
- `shareReplay`

## Higher-Order Mapping Operators

Higher-order mapping operators map each source value to an inner Observable.

| Operator | Behavior | Best for |
|---|---|---|
| `mergeMap` | runs inner Observables in parallel | every result matters |
| `switchMap` | cancels previous inner Observable | search, route params, latest request wins |
| `concatMap` | queues inner Observables sequentially | save operations, order matters |
| `exhaustMap` | ignores new emissions while one is active | prevent duplicate submits |

## mergeMap

`mergeMap` subscribes to every inner Observable and lets them run in parallel.

```ts
clicks$.pipe(
  mergeMap(() => this.api.save())
);
```

Use when every request/result matters and concurrency is acceptable.

## switchMap

`switchMap` cancels the previous inner Observable when a new value arrives.

```ts
search$.pipe(
  debounceTime(300),
  switchMap(query => this.api.search(query))
);
```

Use for search, autocomplete, route param loading, and latest-value-wins flows.

## concatMap

`concatMap` queues inner Observables and runs them one after another.

```ts
saveClicks$.pipe(
  concatMap(data => this.api.save(data))
);
```

Use when order must be preserved.

## exhaustMap

`exhaustMap` ignores new source emissions while the current inner Observable is active.

```ts
submitClicks$.pipe(
  exhaustMap(() => this.api.submit())
);
```

Use to prevent duplicate form submissions or button spam.

## Multicasting

Multicasting means multiple subscribers share the same Observable execution.

Without multicasting, every subscription to a cold Observable can create a new execution.

```ts
const users$ = this.http.get<User[]>('/api/users');

users$.subscribe();
users$.subscribe();
```

This can trigger duplicate HTTP requests.

## share

`share()` converts a cold Observable into a shared/hot Observable while subscribers are active.

```ts
const users$ = this.http.get<User[]>('/api/users').pipe(
  share()
);
```

`share()`:

- prevents duplicate execution while there are subscribers
- does not replay old values to late subscribers
- is useful for sharing live work

## shareReplay

`shareReplay()` shares the execution and replays previous emissions to new subscribers.

```ts
const users$ = this.http.get<User[]>('/api/users').pipe(
  shareReplay({ bufferSize: 1, refCount: true })
);
```

`shareReplay()` is commonly used for:

- HTTP response caching
- shared app state
- avoiding repeated requests
- giving late subscribers the latest value

Be careful with long-lived streams because caching forever can keep data in memory.

## Subject

A Subject is both an Observable and an Observer.

It can emit values and also be subscribed to.

```ts
const subject = new Subject<string>();

subject.subscribe(value => console.log(value));
subject.next('hello');
```

## BehaviorSubject

`BehaviorSubject` requires an initial value.

It stores the latest value and immediately emits it to new subscribers.

```ts
const user$ = new BehaviorSubject<User | null>(null);
```

Use for current state, selected item, auth user, or small local stores.

## ReplaySubject

`ReplaySubject` replays a configured number of previous values to new subscribers.

```ts
const messages$ = new ReplaySubject<string>(2);
```

Use when late subscribers need history.

## AsyncSubject

`AsyncSubject` emits only the last value when the stream completes.

```ts
const result$ = new AsyncSubject<number>();

result$.next(1);
result$.next(2);
result$.complete(); // emits 2
```

Use for one-time final results where only the last value matters.

## Subject Comparison

| Type | Initial value | Late subscribers receive | Common use |
|---|---:|---|---|
| `Subject` | no | future values only | events |
| `BehaviorSubject` | yes | latest value | state |
| `ReplaySubject` | no | previous buffered values | history/cache |
| `AsyncSubject` | no | last value on complete | final result |

Prefer exposing Subjects as Observables:

```ts
private readonly userSubject = new BehaviorSubject<User | null>(null);
readonly user$ = this.userSubject.asObservable();
```

This prevents outside code from calling `next`.

## RxJS Error Handling

RxJS error handling depends on the use case.

Common tools:

- `catchError`
- `retry`
- `retryWhen`
- `throwError`
- `finalize`

```ts
this.http.get<User[]>('/api/users').pipe(
  catchError(error => {
    console.error(error);
    return of([]);
  })
);
```

`catchError` intercepts an error and returns a fallback stream, alternative stream, or rethrows a custom error.

`retry` is useful for transient failures like temporary network issues.

`retryWhen` gives more control over retry timing and conditions.

## Schedulers

Schedulers control when work is executed in RxJS.

They influence timing, ordering, and async boundaries.

Common scheduler-related concepts:

- immediate synchronous execution
- queue-like execution
- microtask-like async behavior
- animation-frame work

Schedulers are mostly advanced, but they matter for testing, timing, and performance-sensitive streams.

## Cancellation

Cancellation means stopping async work that is no longer needed.

In RxJS, cancellation usually happens through unsubscription.

```ts
const sub = users$.subscribe();
sub.unsubscribe();
```

Operators can also cancel work:

- `switchMap` cancels previous inner Observable
- `takeUntil` stops when notifier emits
- `takeUntilDestroyed` stops when Angular context is destroyed
- `timeout` fails if work takes too long

## Debounce

Debounce waits until events stop for a period of time.

```ts
searchControl.valueChanges.pipe(
  debounceTime(300),
  switchMap(query => this.api.search(query))
);
```

Use for search inputs, autocomplete, and noisy user input.

## Throttle

Throttle allows one value, then ignores or limits values for a period of time.

```ts
clicks$.pipe(
  throttleTime(1000)
);
```

Use for scroll, resize, repeated clicks, and rate-limited actions.

## timeout

`timeout` errors if an Observable does not emit within the configured time.

```ts
this.http.get('/api/users').pipe(
  timeout(5000)
);
```

Use when a request or stream should not wait forever.

## Subscription Leaks

A subscription leak happens when a subscription remains active after it is no longer needed.

This can cause:

- memory leaks
- duplicate side effects
- unnecessary API calls
- degraded performance

Avoid leaks by using:

- `async` pipe
- `takeUntilDestroyed`
- `take(1)` / `first()`
- higher-order operators like `switchMap`
- clear ownership for every subscription

## takeUntilDestroyed

`takeUntilDestroyed()` automatically completes a stream when the Angular context is destroyed.

```ts
this.userService.users$.pipe(
  takeUntilDestroyed()
).subscribe(users => {
  this.users = users;
});
```

Use it when you manually subscribe inside components, directives, or services with Angular injection context.

## RxJS and Signals Interop

RxJS and Signals solve different but related problems.

- RxJS is strong for async streams, cancellation, events, and composition.
- Signals are strong for local synchronous state and template reactivity.

Interop is useful when an Angular app needs to connect stream-based data with signal-based UI state.

Common direction:

- Observable from API/forms/router -> signal for template state
- signal state -> Observable when stream composition is needed

## Angular Rules of Thumb

- Use Observables for async streams and cancellation.
- Use `async` pipe when the template consumes the stream.
- Use `switchMap` for latest-request-wins behavior.
- Use `concatMap` when order matters.
- Use `exhaustMap` to ignore duplicate submits.
- Use `shareReplay` carefully for caching.
- Expose `Observable`, not `Subject`, from services.
- Every manual subscription needs ownership and cleanup.

## Short Answer

RxJS helps Angular work with asynchronous streams. Observables can emit many values over time, operators transform and control streams, Subjects multicast values, and cancellation happens through unsubscription. In Angular, RxJS is central for HTTP, forms, router data, user events, state flows, and error handling.

## Related Notes

- [[Promises]]
- [[Async Await]]
- [[Call Stack]]
- [[Angular Router]]
- [[Reactive Forms]]
- [[Signals]]
