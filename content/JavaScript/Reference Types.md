# Reference Types

Reference types are JavaScript values that are objects or object-like structures.

Common reference types:

- object
- array
- function
- date
- map
- set

Reference values are stored and compared by reference:

```js
const a = { name: "Ana" };
const b = a;

b.name = "Nino";

console.log(a.name); // "Nino"
```

Two objects with the same shape are not equal unless they are the same reference:

```js
{} === {}; // false
```

## Related Notes

- [[Data Types]]
- [[Data Types#Primitive Types|Primitive Types]]
