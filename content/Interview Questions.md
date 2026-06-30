# Interview Questions

Questions with answers imported from the provided file. Related notes point back to theory pages.

## Day 1

### Day 1 A · JS/RxJS: What is the difference between `==` and `===`, and how does implicit type coercion work?

**Answer**

== automatically converts one type to another and checking are they equal or not.

=== checking both value and type of the values without converting types.

**Related notes**

- [[Data Types#Destructuring|Destructuring]]
- [[Type Coercion]]

### Day 1 A · Angular: What are standalone components and how do they replace NgModule?

**Answer**

Standalone components are Angular components that don't need to be declared in an NgModule. They manage their own dependencies via imports and can be used directly.

They replace NgModules by removing the need for module declarations and making Angular more component-centric and simpler.

**Related notes**

- [[What is Angular]]
- [[Standalone Components]]
- [[NgModule]]
- [[Components]]

### Day 1 B · JS/RxJS: What primitive types exist in JS, and how do null and undefined differ?

**Answer**

JavaScript primitive types are: string, number, boolean, bigint, symbol, undefined, and null.

undefined means a variable was declared but not assigned a value.

null means an intentional absence of a value (set by developer).

**Related notes**

- [[Data Types]]

### Day 1 B · Angular: Which core files and entities are involved in creating a basic Angular app?

**Answer**

A basic Angular app involves:

- main.ts -> bootstraps the application
- app.component.ts -> root component
- app.component.html -> root template
- app.component.css/scss -> styles
- app.config.ts (or app.module.ts in older Angular) -> app configuration/providers
- index.html -> entry HTML file
- angular.json -> build and project configuration
- tsconfig.json -> TypeScript configuration

**Related notes**

- [[What is Angular]]

## Day 2

### Day 2 A · JS/RxJS: What are scope and closures, and how is the lexical environment formed?

**Answer**

Scope defines where variables are accessible in JavaScript. JS uses lexical scoping, meaning access to variables depends on where functions are declared, not where they are called. Each execution context creates a lexical environment containing local variables and a reference to its parent scope, forming the scope chain.

A closure is created when a function retains access to variables from its outer lexical environment even after the parent function has finished execution. Closures are commonly used in Angular and RxJS for callbacks, event handlers, state encapsulation, and async operations. var is function-scoped, while let and const are block-scoped, which affects how closures behave inside loops and async code.

**Related notes**

- [[Data Types]]
- [[Scope]]
- [[Closures]]
- [[Lexical Environment]]

### Day 2 A · Angular: How do interpolation and property/event bindings work in templates?

**Answer**

**Interpolation** (`{{ }}`) is Angular's one-way binding syntax used to render component data as text in the template. It evaluates expressions in the component context and updates the DOM when change detection runs.

**Property binding** (`[prop]`) binds component values to DOM properties, enabling dynamic updates of element state, for example `[disabled]` and `[src]`. It is also one-way-from component to view.

**Event binding** (`(event)`) listens to DOM events and triggers component methods, enabling communication from view back to component logic.

**Together, these** bindings form Angular's unidirectional data flow within templates, managed by change detection.

**Two-way binding** in Angular combines property binding and event binding into a single syntax using `[( )]`, commonly called banana-in-a-box syntax.

It provides synchronization between the component and the view, meaning changes in the UI automatically update the component state, and updates in the component are immediately reflected in the UI.

Internally, two-way binding is just syntactic sugar for:

- `[property]` -> updates the view from the component
- `(event)` -> updates the component from the view

Example:

`[(ngModel)]="value"` binds an input field to a component property, keeping both in sync.

It is most commonly used in template-driven forms, but in large-scale Angular applications, reactive forms are often preferred for better control and scalability.

**Related notes**

- [[Binding#Template Binding|Template Binding]]
- [[Binding#Interpolation|Interpolation]]
- [[Binding#Property Binding|Property Binding]]
- [[Binding#Event Binding|Event Binding]]
- [[Binding#Two-way Binding|Two-way Binding]]

### Day 2 B · JS/RxJS: How does the this context work, and how do arrow functions differ in handling it?

**Answer**

In JavaScript, this is determined by how a function is called, not where it is defined. In methods, this typically refers to the calling object, but in standalone functions it can be undefined (strict mode) or the global object.

Arrow functions do not have their own this. They lexically capture this from the surrounding scope at the time of definition and never rebind it.

In Angular, this is important for callbacks, RxJS operators, and event handlers: regular functions may lose context when passed around, while arrow functions preserve the component instance context, avoiding manual binding.

**Related notes**

- [[Language Concepts#this|this]]
- [[Language Concepts#Arrow Functions|Arrow Functions]]
- [[Modules and Application Structure#Strict Mode|Strict Mode]]

### Day 2 B · Angular: What are the component lifecycle stages, and what is the responsibility of ngOnInit?

**Answer**

Angular component lifecycle is a sequence of hooks that run from creation to destruction of a component instance. Key stages include: creation (constructor), input initialization (ngOnChanges, ngOnInit), view rendering (ngAfterViewInit), change detection cycles (ngDoCheck, ngAfterContentChecked, ngAfterViewChecked), and cleanup (ngOnDestroy).

ngOnInit runs once after Angular sets all input-bound properties. It is used for component initialization logic such as fetching data, setting up streams/subscriptions, or performing initial state setup. It should not be used for heavy DOM interaction that belongs to later hooks like ngAfterViewInit.

Main lifecycle stages:

- constructor
- ngOnChanges
- ngOnInit
- ngDoCheck
- ngAfterContentInit
- ngAfterContentChecked
- ngAfterViewInit
- ngAfterViewChecked
- ngOnDestroy

**Related notes**

- [[What is Angular]]
- [[Lifecycle Hooks]]

## Day 3

### Day 3 A · JS/RxJS: What is destructuring, and how do rest/spread differ on arrays vs objects?

**Answer**

Destructuring is a clean way to take values from arrays or objects into variables. Arrays use position, objects use property names. ...rest collects remaining values, and ...spread copies or expands values. In arrays it works with elements in order, in objects it works with properties and can merge objects.

**Related notes**

- [[Data Types]]

### Day 3 A · Angular: How do structural directives differ from attribute directives; what roles do @if and @for play?

**Answer**

Structural directives change the DOM layout by adding/removing elements (@if, @for, formerly *ngIf, *ngFor). Attribute directives only change the behavior or styling of existing elements (ngClass, ngStyle).

@if is used for conditional rendering, and @for is used to loop through collections and render items efficiently.

**Related notes**

- [[What is Angular]]
- [[Directives]]
- [[Control Flow Syntax]]

### Day 3 B · JS/RxJS: Which array methods are fundamental for transformations, and why is immutability important?

**Answer**

Core transformation methods are map, filter, and reduce.

map transforms each item into a new array

filter selects items based on a condition

reduce aggregates values into a single result

Immutability matters because you don't mutate original data-this prevents side effects, makes state predictable, and improves change detection (especially in Angular and RxJS).

**Related notes**

- [[Data Types]]
- [[Arrays]]
- [[Immutability]]

### Day 3 B · Angular: What are pipes, and when are pure vs impure pipes appropriate?

**Answer**

Pipes are used to transform data in templates (formatting, filtering, mapping) without changing the underlying value.

Pure pipes run only when input reference changes. They're fast and should be the default (e.g., formatting dates, numbers).

Impure pipes run on every change detection cycle. They're useful when data changes internally without reference change (e.g., filtering a constantly mutating array), but they're expensive and should be avoided unless necessary.

**Related notes**

- [[What is Angular]]
- [[Pipes]]
- [[Change Detection]]

## Day 4

### Day 4 A · JS/RxJS: What is prototypal inheritance, and how are class/extends related to prototypes?

**Answer**

JavaScript uses prototypal inheritance, where objects inherit directly from other objects through the internal prototype chain. When a property or method is accessed, JS first checks the object itself, then walks up the prototype chain until it finds it or reaches Object.prototype.

class and extends in modern JavaScript do not introduce a new inheritance model-they are syntactic sugar over prototypes.

A class is essentially a constructor function with methods placed on its .prototype.

extends sets up the prototype chain between child and parent (linking Child.prototype to Parent.prototype, and Child to Parent for static inheritance).

So in Angular (or any JS context), when you use classes for services/components, you are still working with the same underlying prototype-based inheritance model-just expressed in a more readable and structured syntax.

**Related notes**

- [[Data Types]]
- [[Prototypes]]
- [[Classes]]
- [[What is OOP#Inheritance|Inheritance]]

### Day 4 A · Angular: What is dependency injection in Angular, how does inject() work, and where are providers declared?

**Answer**

Dependency Injection in Angular is a hierarchical runtime system for resolving and providing dependencies instead of manually instantiating them. A class declares what it needs, and Angular's injector tree resolves the correct instance based on provider configuration and scope.

inject() is a context-based alternative to constructor injection. It can only be used within an active injection context (e.g. components, services, interceptors, guards, factory functions). Internally it accesses the current injector, traverses the hierarchy, and returns the closest matching provider instance, or throws if no provider exists.

Providers define how and where dependencies are created. They can be declared at:

providedIn: 'root' (application singleton, tree-shakable)

NgModule providers

Component providers (scoped instance per component subtree)

Application bootstrap (bootstrapApplication providers in standalone apps)

Overall, Angular DI is about controlling object creation, scope, and lifecycle through an injector hierarchy, with inject() providing a functional way to access that system outside constructors.

**Related notes**

- [[What is Angular]]
- [[Dependency Injection]]

### Day 4 B · JS/RxJS: How do Map/Set differ from objects and arrays in common use cases?

**Answer**

Map and Set are more specialized collections compared to objects and arrays.

A Map is a key-value structure where keys can be of any type, and it preserves insertion order. It's typically used when I need frequent additions, deletions, or lookups with dynamic keys.

An Object is mainly for structured data and string-based keys, and it's less predictable for dynamic dictionary-like use cases.

A Set stores only unique values, so I use it when I need to eliminate duplicates or quickly check if a value exists. Compared to arrays, it's more efficient for membership checks but doesn't support array operations like map or filter.

In general, I use objects for data models, arrays for ordered lists and transformations, and Map/Set when I need better performance or specific behavior like uniqueness or flexible key handling.

**Related notes**

- [[Object#Map|Map]]
- [[Arrays#Set|Set]]
- [[Object]]
- [[Arrays]]

### Day 4 B · Angular: What is the injector hierarchy, and how does provider scope affect service instances?

**Answer**

Angular has a hierarchical injector system. Each level of the app can have its own injector, for example root, route/feature, and component injectors.

When a dependency is requested, Angular starts from the current injector and moves upward until it finds a matching provider.

Provider scope controls service lifetime and instance sharing:

- `providedIn: 'root'` -> one application-wide singleton
- route/feature providers -> one instance for that route or feature scope
- component providers -> new instance for that component and its children
- test providers -> instance configured by `TestBed`

The same service can have different instances depending on where it is provided.

This is useful when you need to separate global state from feature-level or component-level state.

In short: injector hierarchy determines **where Angular looks for a dependency**, and provider scope determines **whether the instance is shared globally or created for a smaller context**.

**Related notes**

- [[What is Angular]]
- [[Dependency Injection]]
- [[Dependency Injection#Injector Hierarchy|Injector Hierarchy]]
- [[Dependency Injection#Provider Scope|Provider Scope]]

## Day 5

### Day 5 A · JS/RxJS: How do Promises relate to the event loop's microtask/macrotask queues?

**Answer**

Promises are handled through the microtask queue.

When a Promise resolves or rejects, its `.then()`, `.catch()`, or `.finally()` callbacks are queued as microtasks.

The event loop processes all microtasks before moving to the next macrotask, such as:

- `setTimeout`
- DOM events
- HTTP callbacks

So even if `setTimeout(..., 0)` is used, a resolved Promise callback runs first.

This matters in Angular because change detection, Zone.js tracking, and async operations often rely on microtask timing for predictable UI updates.

**Related notes**

- [[Promises]]
- [[Call Stack]]
- [[Call Stack#Microtask Queue|Microtask Queue]]
- [[Call Stack#Macrotask Queue|Macrotask Queue]]

### Day 5 A · Angular: What capabilities does Angular Router provide, and how is a route configuration defined?

**Answer**

Angular Router provides client-side navigation, lazy loading, route guards, resolvers, nested routes, parameterized routes, redirects, and navigation state management in SPA applications.

Route configuration is defined as an array of Routes, where each route maps a URL path to a component, module, guard, or resolver. It's usually registered with RouterModule.forRoot() in the root module.

**Related notes**

- [[What is Angular]]
- [[Angular Router]]

### Day 5 B · JS/RxJS: How does async/await work, and how should errors be handled in async code?

**Answer**

async/await is syntactic sugar over Promises that makes asynchronous code look synchronous and easier to read. An async function always returns a Promise, and await pauses execution until the Promise resolves or rejects without blocking the event loop.

Errors should typically be handled with try/catch around awaited operations. For parallel async operations, Promise.all() is preferred, but its rejection behavior should also be considered. In Angular, async errors are often handled additionally through RxJS operators or global HTTP interceptors depending on the use case.

**Related notes**

- [[Async Await]]
- [[Promises]]
- [[Promises#Promise Error Handling|Promise Error Handling]]
- [[Call Stack#Microtask Queue|Microtask Queue]]

### Day 5 B · Angular: What is lazy loading, and how do loadComponent and loadChildren differ?

**Answer**

Lazy loading is a routing strategy where feature code is loaded on demand instead of being included in the initial bundle, improving startup performance and reducing initial payload.

loadChildren is used to lazy-load an entire Angular module (or route configuration), typically returning a dynamic import that resolves to a NgModule or route array. It's the classic module-based lazy loading approach.

loadComponent is used in standalone component architecture to lazy-load a single component without a module wrapper. It returns a dynamic import that resolves directly to a component class.

In modern Angular, loadComponent is preferred for standalone apps, while loadChildren is still used for module-based or grouped feature loading.

**Related notes**

- [[What is Angular]]
- [[Angular Router#Lazy Loading|Lazy Loading]]
- [[Angular Router#loadComponent vs loadChildren|loadComponent vs loadChildren]]

## Day 6

### Day 6 A · JS/RxJS: What is an Observable in RxJS, and how is it conceptually different from a Promise?

**Answer**

In RxJS, an Observable is a lazy stream of asynchronous data that can emit multiple values over time. You subscribe to it to receive values, errors, and completion events.

A Promise represents a single asynchronous result that resolves or rejects once.

Key conceptual differences:

Promise -> one value, eager, cannot be canceled.

Observable -> multiple values, lazy, cancellable, composable with operators like map, switchMap, debounceTime, etc.

In Angular, Observables are preferred for things like HttpClient, forms, routing, and real-time streams because they handle async event streams more flexibly than Promises.

**Related notes**

- [[Promises]]
- [[RxJS#Observable|Observable]]

### Day 6 A · Angular: What are guards, and which guard types exist at a high level?

**Answer**

Guards in Angular are used to control navigation and access to routes before a route is activated, loaded, or exited.

High-level guard types:

CanActivate -> checks if a route can be accessed.

CanActivateChild -> protects child routes.

CanDeactivate -> prevents leaving a route (e.g. unsaved changes).

CanLoad -> prevents lazy-loaded modules from loading.

CanMatch -> controls whether a route matches during routing.

Typically used for authentication, authorization, feature flags, and navigation safety.

**Related notes**

- [[What is Angular]]
- [[Angular Router]]
- [[Angular Router#Guards|Route Guards]]

### Day 6 B · JS/RxJS: What are cold vs hot Observables, and how does this affect subscribers?

**Answer**

A cold Observable creates a new independent execution for each subscriber, so every subscription gets its own data stream. A common Angular example is HttpClient, where each subscription triggers a new HTTP request.

A hot Observable shares the same execution and stream between subscribers. Examples are Subject, DOM events, or WebSocket streams.

The key difference is that cold Observables replay the process per subscriber, while hot Observables emit values regardless of who is subscribed, so late subscribers may miss earlier emissions.

**Related notes**

- [[RxJS#Observable|Observable]]
- [[Promises]]
- [[RxJS#Cold Observable|Cold Observable]]
- [[RxJS#Hot Observable|Hot Observable]]

### Day 6 B · Angular: What are resolvers, and when are they justified in routing?

**Answer**

Resolvers in Angular are used to fetch or prepare data before a route is activated. The router waits for the resolver to complete and then provides the data to the component through route data.

They are justified when a page depends on critical data before rendering, such as loading user details or configuration data, to avoid partially loaded screens and simplify component initialization.

**Related notes**

- [[What is Angular]]
- [[Angular Router]]
- [[Angular Router#Resolvers|Resolvers]]

## Day 7

### Day 7 A · JS/RxJS: What are the main categories of RxJS operators (creation, transformation, filtering, combination, multicasting)?

**Answer**

RxJS operators are usually grouped by what they do in the observable pipeline:

Creation operators - create observables from values, events, promises, or APIs.

Examples: of, from, interval, timer.

Transformation operators - transform emitted values or switch observable streams.

Examples: map, switchMap, concatMap, mergeMap.

Filtering operators - control which values continue through the stream.

Examples: filter, debounceTime, distinctUntilChanged, take.

Combination operators - combine multiple observables into one stream.

Examples: combineLatest, forkJoin, merge, zip.

Multicasting operators - share a single execution among multiple subscribers to avoid duplicate side effects.

Examples: share, shareReplay, publish.

In Angular, these categories are heavily used for HTTP flows, reactive forms, route handling, and state management.

**Related notes**

- [[RxJS#RxJS Operators|RxJS Operators]]
- [[RxJS#Multicasting|Multicasting]]

### Day 7 A · Angular: What are Reactive Forms, and how do Typed Forms differ from untyped forms?

**Answer**

Reactive Forms are a model-driven approach for handling forms in Angular.

The form structure, validation, and state are managed in TypeScript using classes like `FormGroup`, `FormControl`, and `FormArray`. They are scalable, testable, and useful for complex enterprise applications.

Typed Forms add strong TypeScript typing to form controls. With typed forms, form values, controls, and `patchValue` / `setValue` operations are type-safe at compile time.

Example difference:

```ts
// Untyped: values are basically any
const form = new FormGroup({});

// Typed: full IntelliSense and compile-time validation
const typedForm = new FormGroup<{
  name: FormControl<string>;
}>({
  name: new FormControl('', { nonNullable: true }),
});
```

Typed forms reduce runtime bugs and improve developer experience in large Angular codebases.

**Related notes**

- [[What is Angular]]
- [[Reactive Forms]]
- [[Reactive Forms#Typed Forms|Typed Forms]]

### Day 7 B · JS/RxJS: How do mergeMap, switchMap, concatMap, and exhaustMap differ in stream behavior?

**Answer**

These are higher-order mapping operators used to handle inner observables differently:

- `mergeMap` - runs inner observables in parallel. Best when every request/result matters and concurrency is acceptable.
- `switchMap` - cancels the previous inner observable when a new value arrives. Ideal for search/autocomplete or route-based HTTP requests.
- `concatMap` - queues inner observables and executes them sequentially. Useful when order must be preserved, like save operations.
- `exhaustMap` - ignores new emissions while the current inner observable is active. Common for preventing duplicate form submissions or button spam.

Main difference is how they handle concurrency, cancellation, and ordering of inner streams.

**Related notes**

- [[RxJS#mergeMap|mergeMap]]
- [[RxJS#switchMap|switchMap]]
- [[RxJS#concatMap|concatMap]]
- [[RxJS#exhaustMap|exhaustMap]]

### Day 7 B · Angular: What are sync vs async validators, and where is the validation strategy defined?

**Answer**

Sync validators run immediately and return either ValidationErrors or null.

Examples: required, minLength, custom synchronous checks.

Async validators run asynchronously and return an Observable or Promise.

Typically used for server-side validation like checking if an email or username already exists.

The validation strategy is defined when creating the form control/group:

second argument -> sync validators

third argument -> async validators

Angular executes sync validators first, and async validators only if sync validation passes.

**Related notes**

- [[What is Angular]]
- [[Reactive Forms#Validation|Validators]]
- [[Reactive Forms#Async Validators|Async Validators]]

## Day 8

### Day 8 A · JS/RxJS: What is a Subject, and how do BehaviorSubject, ReplaySubject, and AsyncSubject differ?

**Answer**

In RxJS, a Subject is both an Observable and an Observer - it can emit values and also be subscribed to, which makes it useful for multicasting and event communication.

Subject

No initial value

Subscribers only receive future emissions

BehaviorSubject

Requires an initial value

Always emits the latest stored value immediately to new subscribers

Common for state management

ReplaySubject

Replays a configurable number of previous values to new subscribers

Useful when late subscribers need history

AsyncSubject

Emits only the last value when the stream completes

Good for one-time async operations/caching

Example use cases in Angular:

Subject -> component event bus

BehaviorSubject -> app/store state

ReplaySubject -> chat/history streams

AsyncSubject -> single API result after completion

**Related notes**

- [[RxJS#Subject|Subject]]
- [[RxJS#BehaviorSubject|BehaviorSubject]]
- [[RxJS#ReplaySubject|ReplaySubject]]
- [[RxJS#AsyncSubject|AsyncSubject]]

### Day 8 A · Angular: Why does ControlValueAccessor exist, and what problem does it solve?

**Answer**

ControlValueAccessor in Angular exists to connect custom form components with Angular Forms API (ngModel, FormControl, formControlName).

It solves the problem of making custom UI components behave like native form controls by providing a bridge between the form model and the view.

It allows Angular to:

- write values to the component
- listen for value changes
- track touched/dirty state
- handle disabled state

Without ControlValueAccessor, custom components can't fully participate in Angular reactive or template-driven forms.

**Related notes**

- [[What is Angular]]
- [[ControlValueAccessor]]
- [[Reactive Forms]]

### Day 8 B · JS/RxJS: What is multicasting, and how do share and shareReplay help?

**Answer**

In RxJS, multicasting means multiple subscribers share the same observable execution instead of creating a new execution per subscriber.

Without multicasting, every subscription to a cold observable (like an HTTP call) triggers a separate execution.

share()

Converts a cold observable into a shared/hot observable

Prevents duplicate executions while subscribers are active

Does not replay old values to late subscribers

shareReplay()

Same as share, but also caches/replays previous emissions to new subscribers

Commonly used for HTTP response caching and shared app state

Example:

Without shareReplay, multiple components subscribing to the same HTTP stream can trigger multiple API calls. With it, the result is shared and replayed from cache.

**Related notes**

- [[RxJS#Multicasting|Multicasting]]
- [[RxJS#share|share]]
- [[RxJS#shareReplay|shareReplay]]

### Day 8 B · Angular: How do reactive forms differ from template-driven forms, and when to choose each?

**Answer**

Reactive Forms

Form model is defined in TypeScript

Explicit, scalable, immutable-style approach

Easier unit testing, dynamic forms, complex validation, RxJS integration

Preferred for enterprise and large-scale apps

Template-Driven Forms

Form logic mainly lives in the template using directives like ngModel

Simpler and less boilerplate

Better for small/simple forms

I'd choose:

Reactive Forms for most production apps, complex workflows, dynamic validation, and maintainability

Template-Driven Forms only for lightweight/simple forms or quick prototypes

**Related notes**

- [[What is Angular]]
- [[Reactive Forms]]
- [[Reactive Forms#Typed Forms|Typed Forms]]
- [[Template-driven Forms]]

## Day 9

### Day 9 A · JS/RxJS: What error-handling strategies exist in RxJS, and when are catchError, retry, retryWhen appropriate?

**Answer**

"RxJS error handling depends on the use case. catchError is used to intercept errors and return a fallback value, alternative stream, or rethrow a custom error. retry is appropriate for transient failures like temporary network issues, automatically resubscribing a fixed number of times. retryWhen provides more control, allowing conditional retries, delays, exponential backoff, or stopping retries based on specific error conditions. In production applications, I typically combine catchError for graceful degradation with retry or retryWhen for recoverable errors."

**Related notes**

- [[Data Types]]
- [[RxJS#RxJS Error Handling|RxJS Error Handling]]

### Day 9 A · Angular: What problems does HttpClient solve, and what are its core concepts (headers, params, response)?

**Answer**

"HttpClient provides a typed, Observable-based API for making HTTP requests and handling responses. It simplifies communication with backend services, supports interceptors, and integrates well with RxJS. Core concepts include HttpHeaders for request metadata, HttpParams for query string parameters, and typed responses for strong compile-time safety and better maintainability."

**Related notes**

- [[What is Angular]]
- [[HTTP]]
- [[HTTP#Angular HttpClient|HttpClient]]

### Day 9 B · JS/RxJS: What are Schedulers in RxJS, and how do they influence execution?

**Answer**

"Schedulers in RxJS control when and where Observable work is executed. They influence execution timing, concurrency, and task scheduling. For example, asyncScheduler schedules work asynchronously, while queueScheduler executes tasks synchronously in a queue. They're useful for controlling performance, testing, and coordinating asynchronous operations."

**Related notes**

- [[Data Types]]
- [[RxJS#Schedulers|Schedulers]]

### Day 9 B · Angular: What are HTTP interceptors, and which scenarios do they cover (auth, logging, retries)?

**Answer**

"HTTP interceptors are middleware-like classes in Angular that sit between HttpClient and the backend, allowing you to inspect and transform requests and responses globally. They're commonly used to attach auth tokens, handle centralized error processing, implement logging, and add retry logic or loading indicators. They keep cross-cutting concerns out of services and promote clean, reusable HTTP logic."

**Related notes**

- [[What is Angular]]
- [[HTTP#HTTP Interceptors|HTTP Interceptors]]

## Day 10

### Day 10 A · JS/RxJS: How are cancellation and time control modeled in RxJS (debounce, throttle, timeout) conceptually?

**Answer**

In RxJS, time-control operators regulate when values are emitted, while cancellation is handled by unsubscribing from streams.

debounceTime() -> waits for a period of silence, then emits the latest value. Common for search inputs.

throttleTime() -> emits the first value, then ignores subsequent values for a time window. Useful for scroll/resize events.

timeout() -> throws an error if no value arrives within the specified duration.

Cancellation -> achieved through unsubscription, often automatically via operators like switchMap(), which cancels the previous inner observable when a new value arrives.

Debounce waits for inactivity, throttle limits emission rate, timeout enforces responsiveness, and cancellation is modeled through Observable unsubscription (e.g., switchMap).

**Related notes**

- [[Data Types]]
- [[RxJS#Cancellation|Cancellation]]
- [[RxJS#Debounce|Debounce]]
- [[RxJS#Throttle|Throttle]]
- [[RxJS#timeout|timeout]]

### Day 10 A · Angular: How does change detection work; how do Default and OnPush differ?

**Answer**

Change detection is Angular's mechanism for synchronizing the UI with application state. It runs a change detection cycle, traverses the component tree, and updates bindings when values change.

Default strategy checks the component and all its descendants whenever Angular detects an async event (user interaction, HTTP response, timer, Promise, etc.). It's simple but can become expensive in large applications.

OnPush strategy optimizes performance by checking a component only when:

An @Input() reference changes.

An event originates from the component or its children.

An observable bound with the async pipe emits.

Change detection is triggered manually (markForCheck(), detectChanges()).

As a senior developer, I prefer OnPush with immutable state and RxJS/signals because it reduces unnecessary checks, improves scalability, and makes data flow more predictable.

**Related notes**

- [[What is Angular]]
- [[Change Detection]]
- [[Change Detection#OnPush strategy|OnPush]]
- [[Immutability]]
- [[Zone.js]]

### Day 10 B · JS/RxJS: What are "subscription leaks," and what conceptual approaches help prevent them?

**Answer**

Subscription leaks occur when an RxJS subscription remains active after the component, service, or consumer no longer needs it. This can cause memory leaks, unnecessary API calls, duplicated side effects, and degraded application performance.

Common prevention approaches:

Use the async pipe in templates, which automatically manages subscriptions.

Prefer higher-order operators (switchMap, mergeMap, etc.) instead of nested subscriptions.

Unsubscribe when the lifecycle ends using takeUntil, takeUntilDestroyed (Angular), or other completion-based operators.

Use operators like take(1) or first() for one-time values.

Ensure long-lived streams are intentionally managed and completed when appropriate.

As a rule, every subscription should have a clear lifecycle and ownership strategy.

**Related notes**

- [[Data Types]]
- [[RxJS#Subscription Leaks|Subscription Leaks]]
- [[RxJS#takeUntilDestroyed|takeUntilDestroyed]]

### Day 10 B · Angular: What roles do trackBy and unidirectional data flow play in template performance?

**Answer**

trackBy improves *ngFor/@for performance by helping Angular identify items with a unique key instead of object reference. This allows Angular to reuse existing DOM elements and update only changed items, reducing unnecessary DOM creation and rendering.

Unidirectional data flow means data moves from parent to child through inputs, while events flow back through outputs. This predictable flow makes change detection more efficient, simplifies debugging, and reduces unintended side effects.

Together, trackBy minimizes DOM updates, while unidirectional data flow makes change detection predictable and performant, especially in large applications.

**Related notes**

- [[What is Angular]]
- [[Change Detection#trackBy / track|trackBy]]
- [[Change Detection#Unidirectional Data Flow|Unidirectional Data Flow]]
- [[Change Detection]]

## Day 11

### Day 11 A · JS/RxJS: How do unknown, any, and never fit into TypeScript's type system, and what is type narrowing?

**Answer**

any disables type checking - TypeScript allows any operation on it, so it should be avoided when possible.

unknown is a safer version of any. You can assign anything to it, but before using it, you must narrow the type with checks like typeof, instanceof, or custom type guards.

never represents a value that can never exist, typically used for functions that always throw errors or for exhaustive switch-case checks.

Type narrowing is the process of reducing a broad type to a more specific one through runtime checks, allowing TypeScript to provide better type safety.

**Related notes**

- [[Data Types]]
- [[Language Concepts#this|this]]
- [[Language Concepts#Arrow Functions|Arrow Functions]]
- [[Language Concepts#unknown|unknown]]
- [[Language Concepts#any|any]]
- [[Language Concepts#never|never]]
- [[Language Concepts#Type Narrowing|Type Narrowing]]

### Day 11 A · Angular: What are Angular Signals, and what are the purposes of signal, computed, and effect?

**Answer**

Angular Signals are a reactive state management feature that enables fine-grained UI updates without relying on RxJS for local state.

signal - stores reactive state.

computed - derives values from other signals.

effect - executes side effects when signal values change.

Signals automatically track dependencies and update only affected parts of the application, improving performance and code readability.

**Related notes**

- [[What is Angular]]
- [[Signals]]
- [[Signals#computed|computed]]
- [[Signals#effect|effect]]

### Day 11 B · JS/RxJS: How can RxJS and Signals interoperate conceptually, and when is it useful?

**Answer**

RxJS and Signals solve different problems. RxJS is best for asynchronous streams and events (HTTP, WebSockets, user actions), while Signals are best for managing local reactive UI state.

They can work together by converting Observables to Signals and Signals to Observables. A common pattern is using RxJS for data fetching and Signals for storing and displaying the state in components.

Interview answer:

"RxJS handles async streams, Signals handle reactive state. They can interoperate through conversions, allowing RxJS to manage data flow and Signals to efficiently update the UI."

**Related notes**

- [[Data Types]]
- [[Signals]]
- [[Signals#computed|computed]]
- [[Signals#effect|effect]]
- [[RxJS#RxJS and Signals Interop|RxJS and Signals Interop]]
- [[RxJS#Observable|Observable]]

### Day 11 B · Angular: Which local state patterns are suitable: service singleton, ComponentStore, Signals-based store?

**Answer**

Service singleton - simple shared data, auth user, settings, selected language.

Signals store - modern Angular local/feature state, derived values, UI state.

ComponentStore - more complex RxJS-heavy state, effects, loading/error flows, pagination.

In modern Angular, I usually prefer Signals for local UI state and RxJS/ComponentStore when the state is stream-heavy or has complex async logic.

**Related notes**

- [[What is Angular]]
- [[Signals]]
- [[Signals#computed|computed]]
- [[Signals#effect|effect]]
- [[State Management]]
- [[State Management#ComponentStore|ComponentStore]]
- [[Signals#Signals Store|Signals Store]]

## Day 12

### Day 12 A · JS/RxJS: What are ESM modules, and how do export/import work in the browser environment?

**Answer**

ESM is the standard JavaScript module system. It allows files to expose code with export and consume it with import. In the browser, modules are loaded using `<script type="module">`. They run in strict mode, have their own scope, are deferred by default, and dependencies are resolved through explicit module specifiers. In Angular, we write ESM all the time, but the bundler usually resolves paths, optimizes dependencies, and produces browser-ready bundles.

**Related notes**

- [[Data Types]]
- [[Modules and Application Structure#ESM|ESM]]
- [[Modules and Application Structure#Modules|Modules]]
- [[Modules and Application Structure#Strict Mode|Strict Mode]]
- [[Modules and Application Structure#Application Structure|Application Structure]]
- [[Modules and Application Structure#Path Aliases|Path Aliases]]

### Day 12 A · Angular: How do @if/@for differ from *ngIf/*ngFor, and why were they introduced?

**Answer**

@if and @for are Angular's new built-in control flow syntax introduced in Angular 17.

*ngIf and *ngFor are directives.

@if and @for are built directly into Angular's template compiler, so Angular can generate more optimized code.

**Related notes**

- [[What is Angular]]
- [[Directives]]
- [[Control Flow Syntax]]

### Day 12 B · JS/RxJS: Why is runtime data validation useful, and how does it relate to TypeScript types?

**Answer**

TypeScript types exist only during compilation and are removed at runtime. Because of that, data coming from external sources such as APIs, localStorage, user input, or WebSockets cannot be trusted. Runtime validation checks that the actual data matches the expected shape before the application uses it. TypeScript gives compile-time safety, while runtime validation provides runtime safety. Together they help prevent bugs and unexpected runtime errors.

**Related notes**

- [[Data Types]]
- [[TypeScript Types#Runtime Validation|Runtime Validation]]
- [[TypeScript Types]]

### Day 12 B · Angular: What are attribute directives, and what are common scenarios for their use?

**Answer**

Attribute directives change the appearance or behavior of an existing DOM element without creating or removing it from the DOM. Common use cases include applying styles, highlighting elements, handling permissions, showing validation states, or adding custom UI behavior. Angular built-in examples are NgClass and NgStyle. I typically use attribute directives when the same behavior needs to be reused across multiple components.

**Related notes**

- [[What is Angular]]
- [[Directives]]

## Day 13

### Day 13 A · JS/RxJS: Which JS/TS application structuring approaches help scalability (modules, layers, aliases)?

**Answer**

To make applications scalable, I organize code into feature-based modules and clear layers such as UI, services, data access, and shared utilities. I use ES modules to keep code encapsulated and reusable, path aliases to avoid long relative imports, and enforce dependency boundaries between layers. This improves maintainability, reduces coupling, and makes the codebase easier to navigate as the application grows.

**Related notes**

- [[Data Types]]
- [[Modules and Application Structure#ESM|ESM]]
- [[Modules and Application Structure#Modules|Modules]]
- [[Modules and Application Structure#Application Structure|Application Structure]]
- [[Modules and Application Structure#Path Aliases|Path Aliases]]

### Day 13 A · Angular: How to structure a monorepo with Nx (feature/ui/data-access/util), and why enforce dependency boundaries?

**Answer**

In Nx, I usually structure Angular code by domain and layer: feature for smart components/pages, ui for reusable presentational components, data-access for API/state logic, and util for pure helpers. Apps should compose features, features can use UI and data-access, UI should stay dumb and not call APIs, and util should not depend on Angular-specific business logic.

**Related notes**

- [[What is Angular]]
- [[Angular Architecture#Nx|Nx]]
- [[Angular Architecture#Monorepo|Monorepo]]
- [[Angular Architecture#Dependency Boundaries|Dependency Boundaries]]

### Day 13 B · JS/RxJS: Which principles for designing operator chains improve maintainability (readability, consistency, side-effect isolation)?

**Answer**

For maintainable RxJS chains, I keep the pipeline readable, consistent, and predictable. I use the right flattening operator for the behavior: switchMap for canceling previous requests, concatMap for ordered execution, mergeMap for parallel work, and exhaustMap for ignoring repeated triggers.

I keep side effects inside tap, transformations inside map, error handling close to the async operation with catchError, and cleanup inside finalize. I also avoid deeply nested subscriptions and prefer composing streams declaratively. A good RxJS chain should read like a data flow, not like hidden imperative logic.

**Related notes**

- [[Data Types]]
- [[RxJS#RxJS Operators|RxJS Operators]]
- [[Signals]]
- [[Signals#computed|computed]]
- [[Signals#effect|effect]]

### Day 13 B · Angular: How do route-level providers differ from component-level and app-level providers?

**Answer**

and app-level providers?

Angular has a hierarchical dependency injection system. The provider scope determines how long an instance lives and who can access it.

App-level providers (providedIn: 'root' or bootstrap providers) create a singleton instance shared across the entire application.

Route-level providers create an instance when a route is activated and destroy it when leaving that route. They are useful for feature-specific services, stores, or state that should not be shared with other routes.

Component-level providers create a new instance for that component and its children. Each component instance gets its own service instance.

**Related notes**

- [[What is Angular]]
- [[Dependency Injection]]
- [[Dependency Injection#Route-level providers|Route-level Providers]]
- [[Dependency Injection#Component providers|Component Providers]]
- [[Dependency Injection#providedIn: root|App-level Providers]]

## Day 14

### Day 14 A · JS/RxJS: What conceptual approaches exist for testing RxJS (marble thinking, side-effect isolation)?

**Answer**

RxJS testing verifies stream behavior over time. Marble thinking represents emissions, timing, completion, and errors visually, which makes async stream behavior easier to reason about. Side effects should be isolated in `tap`, services, or effects so the main operator chain remains testable. Good RxJS tests cover success, error, completion, and cancellation behavior.

**Related notes**

- [[Data Types]]
- [[Signals]]
- [[Signals#computed|computed]]
- [[Signals#effect|effect]]

### Day 14 A · Angular: What testing levels exist in Angular, and what is the purpose of each (conceptually)?

**Answer**

Angular testing has several levels. Unit tests check isolated logic like services, pipes, validators, and pure functions. Component tests verify templates, inputs, outputs, bindings, and user interaction. Integration tests check multiple Angular pieces working together. E2E tests verify full user flows in the browser. The goal is to test behavior at the cheapest reliable level.

**Related notes**

- [[What is Angular]]
- [[Angular Testing]]
- [[Angular Testing#Unit Testing|Unit Testing]]
- [[Angular Testing#Integration Testing|Integration Testing]]
- [[Angular Testing#E2E Testing|E2E Testing]]

### Day 14 B · JS/RxJS: What is backpressure in reactive systems, and why is rate control important?

**Answer**

Backpressure means the producer emits faster than the consumer can process. In frontend apps this can happen with scroll, resize, input, WebSocket, or rapid user actions. Rate control protects performance and prevents duplicated work. Common tools are debounceTime, 	hrottleTime, uditTime, buffering, cancellation with switchMap, and limiting concurrency.

**Related notes**

- [[Data Types]]

### Day 14 B · Angular: What XSS risks exist on the frontend, and what is the role of sanitization and safe APIs?

**Answer**

XSS happens when attacker-controlled content executes as JavaScript in the browser. Risks come from unsafe HTML rendering, direct DOM manipulation, third-party scripts, and bypassing framework protections. Angular sanitizes many bindings by default, but innerHTML, `bypassSecurityTrust...`, and external HTML must be handled carefully. Sanitization removes dangerous content, and safe APIs reduce the chance of executable input reaching the DOM.

**Related notes**

- [[What is Angular]]
- [[Security#XSS|XSS]]
- [[Security#Sanitization|Sanitization]]
- [[Security]]

## Day 15

### Day 15 A · JS/RxJS: What are the pros and cons of SWR/caching patterns in reactive data access?

**Answer**

SWR means stale-while-revalidate: show cached data immediately, then refresh in the background. It improves perceived speed and reduces unnecessary requests. The downside is complexity: cache invalidation, stale data, loading state, and error handling become harder. It works best for mostly-read data where slightly stale UI is acceptable.

**Related notes**

- [[Data Types]]

### Day 15 A · Angular: Which route preloading strategies exist, and when are they justified?

**Answer**

Route preloading loads lazy route code before the user opens it. Strategies include no preloading, preload all, and custom selective preloading based on route data, user role, network, or predicted navigation. It is justified when it improves perceived navigation speed without hurting startup performance or wasting bandwidth.

**Related notes**

- [[What is Angular]]
- [[Angular Performance#Route Preloading|Route Preloading]]
- [[Angular Router]]

### Day 15 B · JS/RxJS: How to reason about retries with backoff conceptually, and when should retries be avoided?

**Answer**

Retries are useful for temporary failures like network glitches. Backoff waits longer between attempts to avoid overwhelming the server. Retries should be limited and observable. Avoid retries for validation errors, authentication/authorization errors, bad requests, and non-idempotent operations where repeating the action can duplicate side effects.

**Related notes**

- [[Data Types]]

### Day 15 B · Angular: What approaches exist for global error handling, and how does it align with HTTP error handling?

**Answer**

Global error handling catches unexpected application errors and reports them to logging or monitoring tools. HTTP error handling usually belongs near the request layer, often through interceptors for auth errors, retries, mapping server errors, or global messages. Expected business errors should be handled locally. A good architecture separates expected recoverable errors from unexpected system failures.

**Related notes**

- [[What is Angular]]
- [[Production Readiness#Global Error Handling|Global Error Handling]]
- [[HTTP#HTTP Error Handling|HTTP Error Handling]]

## Day 16

### Day 16 A · JS/RxJS: Why are AbortController and cancel signals important on the web platform, and how do they relate to RxJS cancellation?

**Answer**

AbortController provides a standard way to cancel web operations like `fetch`. It prevents outdated requests from continuing after the result is no longer needed. RxJS models cancellation through unsubscription. Operators like switchMap cancel previous inner streams, and when those streams wrap abortable APIs, unsubscription can trigger an abort signal.

**Related notes**

- [[Data Types]]
- [[RxJS#Cancellation|Cancellation]]
- [[RxJS#Debounce|Debounce]]
- [[RxJS#Throttle|Throttle]]
- [[RxJS#timeout|timeout]]
- [[Signals]]
- [[Signals#computed|computed]]
- [[Signals#effect|effect]]

### Day 16 A · Angular: How to organize i18n in an app, and how does Angular i18n compare to libraries like Transloco?

**Answer**

i18n should be organized with clear translation keys, feature/domain structure, fallback handling, and consistent naming. Angular built-in i18n is strong for compile-time translation and template extraction. Libraries like Transloco are more flexible for runtime language switching, lazy-loaded translation files, and dynamic keys. The choice depends on whether the app needs runtime switching and how dynamic translations are.

**Related notes**

- [[What is Angular]]
- [[i18n]]
- [[i18n#Transloco|Transloco]]

### Day 16 B · JS/RxJS: Which principles of designing public TS APIs matter for DX and long-term stability?

**Answer**

Good public TypeScript APIs should be small, explicit, stable, and strongly typed. Avoid leaking implementation details. Prefer clear names, narrow types, useful generics, readonly data where possible, and backward-compatible changes. Public APIs should define errors, cancellation, completion, and expected usage clearly.

**Related notes**

- [[Data Types]]
- [[TypeScript Types#Public TypeScript API Design|Public TypeScript API Design]]

### Day 16 B · Angular: Which accessibility (a11y) aspects are critical in components: roles, ARIA attributes, focus management?

**Answer**

Accessible components need semantic HTML first, then ARIA only when needed. Important parts are keyboard navigation, visible focus states, focus trapping/restoration for dialogs, correct roles, labels, aria-expanded/selected/disabled states, and screen-reader-friendly errors. Components should be usable without a mouse.

**Related notes**

- [[What is Angular]]
- [[Accessibility]]
- [[Accessibility#ARIA|ARIA]]
- [[Accessibility#Focus Management|Focus Management]]

## Day 17

### Day 17 A · JS/RxJS: Which logging and introspection approaches help debug RxJS pipelines?

**Answer**

RxJS pipelines can be debugged with targeted `tap` logs, clear stream names, and logging lifecycle events like subscribe, emit, error, complete, and unsubscribe. For complex timing, marble diagrams help. Avoid random logs everywhere; log at stream boundaries and side-effect points so data flow stays understandable.

**Related notes**

- [[Data Types]]

### Day 17 A · Angular: What is zone-based change detection, why is zone.js used, and what does "zoneless" mode imply?

**Answer**

Zone-based change detection uses Zone.js to patch async browser APIs and notify Angular when async work finishes. Angular then runs change detection to update the UI. Zoneless mode removes that automatic async tracking and relies on explicit update signals such as Signals, async pipe emissions, input changes, template events, and markForCheck().

**Related notes**

- [[What is Angular]]
- [[Change Detection]]
- [[Change Detection#OnPush strategy|OnPush]]
- [[Zone.js]]
- [[Zoneless]]

### Day 17 B · JS/RxJS: How to conceptually separate domain streams from UI streams in an application?

**Answer**

Domain streams represent business data and rules, such as current user, permissions, orders, or entities. UI streams represent view state, such as selected tab, search text, dialog state, loading indicators, and form interaction. Keep domain streams in services/facades/stores and UI streams closer to components.

**Related notes**

- [[Data Types]]

### Day 17 B · Angular: Which performance techniques are recommended: code splitting, change optimization, data caching?

**Answer**

Recommended techniques include lazy loading/code splitting, OnPush or signal-driven change detection, stable 	rack/	rackBy, avoiding heavy template functions, caching repeat data, preloading likely routes, image optimization, and reducing bundle size. The main idea is to load less, render less, and avoid repeating expensive work.

**Related notes**

- [[What is Angular]]
- [[Angular Performance]]
- [[Angular Performance#Code Splitting|Code Splitting]]
- [[Angular Performance#Caching|Caching]]

## Day 18

### Day 18 A · JS/RxJS: How do Promises and reactive streams complement each other in complex scenarios?

**Answer**

Promises are good for one-time async results. Observables are better for streams of values over time. In complex apps, Promises can represent single initialization tasks while RxJS handles events, user input, HTTP flows, cancellation, retries, and composition. They complement each other when one-time async work must be integrated into a larger reactive flow.

**Related notes**

- [[Promises]]
- [[RxJS#Observable|Observable]]
- [[Call Stack#Microtask Queue|Microtask Queue]]
- [[Call Stack#Macrotask Queue|Macrotask Queue]]

### Day 18 A · Angular: What is Angular Universal (SSR), what is hydration, and why use transfer state?

**Answer**

Angular SSR renders initial HTML on the server so users and crawlers receive content faster. Hydration connects the server-rendered HTML to the client-side Angular app without recreating the whole DOM. TransferState passes server-fetched data to the browser so the client does not immediately repeat the same request.

**Related notes**

- [[What is Angular]]
- [[SSR]]
- [[SSR#Hydration|Hydration]]
- [[SSR#TransferState|TransferState]]

### Day 18 B · JS/RxJS: What risks do global side effects pose in operator chains, and how can they be isolated?

**Answer**

Global side effects inside RxJS chains make behavior hidden, hard to test, and hard to reuse. They can cause duplicated requests, unexpected state changes, and order-dependent bugs. Isolate side effects in `tap`, effects/services, or facade methods. Keep transformation operators like map and `filter` pure.

**Related notes**

- [[Data Types]]
- [[RxJS#RxJS Operators|RxJS Operators]]
- [[Signals]]
- [[Signals#computed|computed]]
- [[Signals#effect|effect]]

### Day 18 B · Angular: What conceptual considerations matter when integrating UI kits (PrimeNG/Tailwind): theming and responsiveness?

**Answer**

When integrating UI kits, define theming, spacing, responsive rules, and override strategy clearly. PrimeNG provides ready-made components and behavior, while Tailwind gives utility-based styling control. Avoid random mixing. Decide how dark mode, tokens, responsiveness, and component customization should work across the app.

**Related notes**

- [[What is Angular]]
- [[UI Kits]]
- [[UI Kits#PrimeNG|PrimeNG]]
- [[UI Kits#Tailwind CSS|Tailwind CSS]]
- [[UI Kits#Responsiveness|Responsive Design]]

## Day 19

### Day 19 A · JS/RxJS: Which security considerations matter for client-side JS (CSP, external HTML, secrets)?

**Answer**

Client-side security includes avoiding unsafe HTML injection, using CSP, validating external content, and never storing real secrets in frontend code. Tokens and sensitive data must be handled carefully. Third-party scripts increase attack surface. Frontend checks improve UX, but real authorization and data protection must happen on the backend.

**Related notes**

- [[Data Types]]

### Day 19 A · Angular: How are environments and build configurations organized, and what are "budgets" for?

**Answer**

Angular environments and build configurations separate settings for development, staging, and production, such as API URLs, flags, and optimization options. Build configurations control optimization, source maps, file replacements, and output settings. Budgets define bundle or asset size limits to catch performance regressions.

**Related notes**

- [[What is Angular]]
- [[Production Readiness#Environment Handling|Environment Configuration]]
- [[Production Readiness#CI/CD|Build Configurations]]
- [[Angular Performance#Budgets|Budgets]]

### Day 19 B · JS/RxJS: Which architectural boundaries help localize "effects" and event exchange between modules?

**Answer**

Good boundaries keep side effects inside services, facades, stores, or effects layers instead of spreading them through components. Features should communicate through public APIs, events, routes, or shared state contracts rather than direct internal imports. This reduces coupling and makes event exchange easier to trace.

**Related notes**

- [[Data Types]]
- [[Signals]]
- [[Signals#computed|computed]]
- [[Signals#effect|effect]]
- [[Modules and Application Structure#ESM|ESM]]
- [[Modules and Application Structure#Modules|Modules]]
- [[Modules and Application Structure#Application Structure|Application Structure]]
- [[Modules and Application Structure#Path Aliases|Path Aliases]]

### Day 19 B · Angular: Which i18n key and structure practices improve maintainability in large projects?

**Answer**

Use consistent key naming, group translations by feature/domain, avoid duplicate keys with different meanings, and keep interpolation clear. Prefer stable semantic keys for large apps. Lazy-load feature translations when useful and document naming rules so translation updates are easier to review.

**Related notes**

- [[What is Angular]]
- [[i18n]]
- [[i18n#Transloco|Transloco]]

## Day 20

### Day 20 A · JS/RxJS: What belongs to a theoretical quality checklist for reactive code (readability, time control, cancellation, errors, testability)?

**Answer**

A reactive quality checklist includes readable stream names, correct operator choice, clear cancellation policy, controlled timing, explicit error handling, isolated side effects, no nested subscriptions, subscription ownership, and testability. Each stream should have a clear purpose, lifecycle, and behavior under success, error, and cancellation.

**Related notes**

- [[Data Types]]
- [[RxJS#Cancellation|Cancellation]]
- [[RxJS#Debounce|Debounce]]
- [[RxJS#Throttle|Throttle]]
- [[RxJS#timeout|timeout]]

### Day 20 A · Angular: How to formulate an Angular architectural checklist (routing, DI boundaries, state, forms, HTTP, errors, performance, i18n, a11y, SSR)?

**Answer**

An Angular architecture checklist should cover routing, lazy loading, provider scopes, DI boundaries, state ownership, forms strategy, HTTP/interceptor design, error handling, performance, i18n, accessibility, SSR/hydration, and testing. The goal is to make major architectural decisions explicit and consistent.

**Related notes**

- [[What is Angular]]
- [[i18n]]
- [[i18n#Transloco|Transloco]]
- [[Accessibility]]
- [[Accessibility#ARIA|ARIA]]
- [[Accessibility#Focus Management|Focus Management]]
- [[SSR]]
- [[SSR#Hydration|Hydration]]
- [[SSR#TransferState|TransferState]]
- [[Angular Architecture]]
- [[Production Readiness#Production Checklist|Production Checklist]]

### Day 20 B · JS/RxJS: What principles should a public reactive API follow (expose Observable rather than Subject, avoid external next, define completion/error semantics, cancellation policy, backpressure expectations)?

**Answer**

Backpressure means the producer emits faster than the consumer can process. In frontend apps this can happen with scroll, resize, input, WebSocket, or rapid user actions. Rate control protects performance and prevents duplicated work. Common tools are debounceTime, 	hrottleTime, uditTime, buffering, cancellation with switchMap, and limiting concurrency.

**Related notes**

- [[Data Types]]
- [[RxJS#Observable|Observable]]
- [[Promises]]
- [[RxJS#Subject|Subject]]
- [[RxJS#BehaviorSubject|BehaviorSubject]]
- [[RxJS#ReplaySubject|ReplaySubject]]
- [[RxJS#AsyncSubject|AsyncSubject]]
- [[RxJS#Cancellation|Cancellation]]
- [[RxJS#Debounce|Debounce]]
- [[RxJS#Throttle|Throttle]]
- [[RxJS#timeout|timeout]]

### Day 20 B · Angular: What is a production-readiness & operations checklist for Angular apps (CI/CD, environment handling, feature flags, logging/monitoring, error reporting, source-maps policy, performance & accessibility audits)?

**Answer**

Accessible components need semantic HTML first, then ARIA only when needed. Important parts are keyboard navigation, visible focus states, focus trapping/restoration for dialogs, correct roles, labels, aria-expanded/selected/disabled states, and screen-reader-friendly errors. Components should be usable without a mouse.

**Related notes**

- [[What is Angular]]
- [[Accessibility]]
- [[Accessibility#ARIA|ARIA]]
- [[Accessibility#Focus Management|Focus Management]]
- [[Production Readiness]]
- [[Production Readiness#CI/CD|CI CD]]
- [[Production Readiness#Logging and Monitoring|Monitoring]]
- [[Production Readiness#Source Maps|Source Maps]]
