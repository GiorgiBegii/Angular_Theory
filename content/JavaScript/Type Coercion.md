# Type Coercion

Type coercion means JavaScript converts a value from one type to another.

There are two types:

- **Explicit coercion** - developer converts the value manually.
- **Implicit coercion** - JavaScript converts the value automatically.

## Explicit coercion

Explicit coercion is clear because we call conversion functions ourselves.

```js
Number("5"); // 5
String(10); // "10"
Boolean(1); // true
Boolean(""); // false
```

Use explicit coercion when you want readable and predictable code.

## Implicit coercion

Implicit coercion happens automatically during operations.

```js
"5" + 1; // "51"
"5" - 1; // 4
true + 1; // 2
false + 1; // 1
```

Important:

- `+` can mean string concatenation or numeric addition.
- `-`, `*`, `/` usually convert values to numbers.

## == vs ===

`==` compares values after type coercion.

```js
"5" == 5; // true
false == 0; // true
null == undefined; // true
```

`===` compares value and type without coercion.

```js
"5" === 5; // false
false === 0; // false
null === undefined; // false
```

Prefer `===` in most real code because it is more predictable.

## Truthy and falsy

When JavaScript expects a boolean, it converts values to `true` or `false`.

Falsy values:

```js
false
0
-0
0n
""
null
undefined
NaN
```

Everything else is truthy.

Examples:

```js
Boolean([]); // true
Boolean({}); // true
Boolean("0"); // true
Boolean("false"); // true
```

## Common mistakes

```js
[] == false; // true
"" == false; // true
"0" == false; // true
```

These are confusing because `==` applies coercion rules.

Better:

```js
value === false
value === 0
value === ""
```

## Senior reminder

Use coercion intentionally.

Good:

```js
const page = Number(query.page ?? 1);
```

Risky:

```js
if (query.page == 1) {}
```

In production code, prefer explicit conversion and strict equality.

## Short answer

Type coercion is JavaScript's conversion of values between types. It can be explicit, like `Number("5")`, or implicit, like `"5" == 5`. `==` allows coercion, while `===` does not. In most cases, `===` and explicit conversions are safer and easier to understand.

## Related

- [[Data Types]]
- [[Data Types#Primitive Types|Primitive Types]]
