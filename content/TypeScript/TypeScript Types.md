# TypeScript Types

TypeScript types describe what values are allowed and help catch mistakes before runtime.

Simple idea:

```text
JavaScript runs the code
TypeScript checks the shape before it runs
```

## any

`any` turns off type checking for a value.

```ts
let value: any = 123;
value.toUpperCase(); // TypeScript allows it
```

Use `any` only when necessary because it removes safety.

## unknown

`unknown` means the value can be anything, but you must check it before using it.

```ts
let value: unknown = 'hello';

if (typeof value === 'string') {
  value.toUpperCase();
}
```

Prefer `unknown` over `any` for external data.

## never

`never` represents a value that should never exist.

Common cases:

- function always throws
- function never returns
- exhaustive checks

```ts
function fail(message: string): never {
  throw new Error(message);
}
```

## Type Narrowing

Type narrowing means reducing a broad type to a more specific type using checks.

Examples:

- `typeof`
- `instanceof`
- `in`
- equality checks
- custom type guards

```ts
function print(value: string | number) {
  if (typeof value === 'string') {
    value.toUpperCase();
  }
}
```

## Runtime Validation

TypeScript types disappear at runtime.

That means API responses, localStorage data, URL params, and external input still need runtime validation when trust matters.

Use runtime validation for:

- backend responses
- user input
- third-party data
- config files
- persisted storage

## Public TypeScript API Design

A public TypeScript API should be easy to use correctly and hard to misuse.

Good API types:

- expose clear inputs/outputs
- avoid unnecessary `any`
- use `unknown` for untrusted input
- model impossible states as impossible
- keep generics understandable
- document completion/error/cancellation behavior for reactive APIs

## Short Answer

TypeScript types catch mistakes before runtime. `any` disables type safety, `unknown` forces checking before use, `never` represents impossible values, and type narrowing safely turns broad types into specific types. Because TypeScript disappears at runtime, important external data still needs runtime validation.

## Related Notes

- [[Language Concepts]]
- [[RxJS]]
- [[HTTP]]
- [[Data Types]]
