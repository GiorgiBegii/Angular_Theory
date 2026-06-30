# Angular Testing

Angular testing checks whether components, services, routing, forms, HTTP flows, and user behavior work as expected.

Simple idea:

```text
test the smallest useful behavior
use bigger tests only when integration matters
```

## Unit Testing

Unit tests check one small piece of logic in isolation.

Good targets:

- services
- pure functions
- pipes
- validators
- guards
- small component methods

Example purpose:

```text
Given input X
when function/service runs
then result should be Y
```

Unit tests should be fast, focused, and easy to understand.

## Component Testing

Component tests check component behavior together with its template.

They are useful for:

- inputs
- outputs
- template bindings
- conditional rendering
- user clicks
- form interactions
- child component integration when needed

Good component tests focus on visible behavior, not private implementation details.

## Integration Testing

Integration tests check multiple Angular pieces working together.

Examples:

- component + service
- component + reactive form
- route + guard
- HTTP service + interceptor
- parent component + child component

Use integration tests when the risk is in the connection between pieces.

## E2E Testing

E2E tests check complete user flows in a real browser-like environment.

Examples:

- login flow
- checkout flow
- create/edit/delete entity
- navigation between pages
- permission-based access

E2E tests give high confidence but are slower and more expensive than unit tests.

## Testing Pyramid

Common balance:

- many unit tests
- some component/integration tests
- fewer E2E tests

Short rule:

```text
test most logic cheaply
test critical flows end-to-end
```

## TestBed

`TestBed` creates an Angular testing module/environment.

It can configure:

- components
- providers
- imports
- mocks
- router testing
- HTTP testing

```ts
TestBed.configureTestingModule({
  imports: [UserComponent],
  providers: [
    { provide: UserService, useValue: userServiceMock },
  ],
});
```

## Mocks and Fakes

Use mocks/fakes to control dependencies.

Good mock:

- small
- explicit
- returns predictable values
- represents only what the test needs

Avoid over-mocking Angular itself. Mock app dependencies, not the framework.

## Async Testing

Angular tests often deal with async work:

- Promises
- Observables
- HTTP requests
- timers
- router navigation

Use the simplest tool that fits:

- `async/await` for Promises
- Observable helpers for streams
- HTTP testing controller for HTTP
- fake timers only when timing is important

## What To Test

Test behavior:

- what user sees
- what output/event happens
- what service returns
- what navigation is allowed
- what validation result is produced

Avoid testing:

- private method details
- framework internals
- exact implementation when behavior is enough

## Short Answer

Angular testing has several levels. Unit tests check isolated logic like services, pipes, validators, and pure functions. Component tests verify templates, inputs, outputs, bindings, and user interaction. Integration tests check multiple Angular pieces working together. E2E tests verify full user flows in the browser. The goal is to test behavior at the cheapest reliable level.

## Related Notes

- [[Dependency Injection]]
- [[Reactive Forms]]
- [[Angular Router]]
- [[HTTP]]
- [[RxJS]]
