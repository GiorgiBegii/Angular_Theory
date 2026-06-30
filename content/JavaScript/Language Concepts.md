# Language Concepts

This note groups small JavaScript and TypeScript concepts that often appear together in interviews.

## this

`this` is a special value that points to the current execution context.

In regular JavaScript functions, `this` is determined by how the function is called.

```js
const user = {
  name: "Ana",
  sayName() {
    console.log(this.name);
  }
};

user.sayName(); // "Ana"
```

`this` is not decided by where the function is written. It is decided by the call-site.

## Lost this

`this` can be lost when a method is passed as a callback.

```js
const fn = user.sayName;
fn(); // this is lost
```

The function is no longer called as `user.sayName()`, so `this` is not `user`.

In strict mode, a standalone function can have `this === undefined`.

```js
"use strict";

function show() {
  console.log(this);
}

show(); // undefined
```

## Arrow Functions

Arrow functions are a shorter syntax for writing functions.

```js
const sum = (a, b) => a + b;
```

Arrow functions do not have their own `this`.

They capture `this` from the surrounding lexical scope.

```ts
export class UserComponent {
  name = "Ana";

  loadUser() {
    setTimeout(() => {
      console.log(this.name);
    }, 1000);
  }
}
```

Regular functions can lose component context:

```ts
setTimeout(function () {
  console.log(this.name);
}, 1000);
```

In Angular and RxJS callbacks, arrow functions are commonly used because they keep access to the component instance.

```ts
this.userService.getUser().subscribe(user => {
  this.user = user;
});
```

## any

`any` disables TypeScript type checking for a value.

```ts
let value: any = 10;

value.toUpperCase();
value.notExistingMethod();
```

TypeScript allows almost anything with `any`, so it removes type safety.

Use `any` only when:

- migrating legacy code
- temporarily working with unknown third-party data
- there is no better type yet

Prefer `unknown` when the value must be checked before use.

## unknown

`unknown` is a safer version of `any`.

Anything can be assigned to `unknown`, but it cannot be used until the type is narrowed.

```ts
let value: unknown = "hello";

value.toUpperCase(); // error

if (typeof value === "string") {
  value.toUpperCase(); // ok
}
```

Use `unknown` for untrusted data:

- API responses before validation
- `JSON.parse`
- external messages
- caught errors

## never

`never` represents a value that can never exist.

Common cases:

- function always throws
- function never returns
- exhaustive checks

```ts
function fail(message: string): never {
  throw new Error(message);
}
```

Exhaustive switch:

```ts
type Status = "loading" | "success" | "error";

function assertNever(value: never): never {
  throw new Error(`Unexpected value: ${value}`);
}
```

`never` helps TypeScript prove that all cases are handled.

## Type Narrowing

Type narrowing means reducing a broad type to a more specific type using runtime checks.

```ts
function print(value: string | number) {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else {
    console.log(value.toFixed(2));
  }
}
```

Common narrowing tools:

- `typeof`
- `instanceof`
- `in`
- equality checks
- discriminated unions
- custom type guards

Custom type guard:

```ts
function isUser(value: unknown): value is User {
  return typeof value === "object" && value !== null && "id" in value;
}
```

## Short Answer

Regular functions get `this` from how they are called, while arrow functions capture `this` from the surrounding scope. In TypeScript, `any` disables type checking, `unknown` requires narrowing before use, `never` represents impossible values, and type narrowing uses runtime checks to safely work with specific types.

## Related Notes

- [[Functions]]
- [[Scope]]
- [[Lexical Environment]]
- [[Modules and Application Structure#Strict Mode|Strict Mode]]
- [[TypeScript Types]]
