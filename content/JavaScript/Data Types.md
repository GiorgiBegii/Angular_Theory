# Data Types

JavaScript values are split into two big groups:

- Primitive types - simple immutable values, stored and compared by value.
- Reference types - objects stored and compared by reference.

Use this note as the overview for both primitive and reference values.

## Primitive Types

Primitive values are:
- Immutable
- Stored by value
- Compared by value

#### 1. Number

Represents both integers and floating-point numbers.
```js
let age = 25;
let price = 10.5;
```

There is **only one numeric type** (excluding `BigInt`).
```js
typeof 5 // "number"typeof 5.2 
// "number"
```

**Special numeric values NaN Means Not a Number**.
```js
Number("abc") // NaN
```

```js
0 / 0 // NaN
```

Interesting fact:
```js
NaN === NaN // false
```

Correct check:
```js
Number.isNaN(value)
```


#### 2. BigInt

For integers larger than `MAX_SAFE_INTEGER`.
```js
const n = 12345678901234567890n;
```

or

```js
BigInt("12345678901234567890")
```

Cannot mix with Number.
```js
1n + 1 // TypeError
```

Must convert first.

#### 3. String

Represents text.
```js
const name = "John";
```

Strings are immutable.
```js
let str = "Hello";
str[0] = "A";
console.log(str);// Hello
```

---

**Template literals**
```js
`Hello ${name}`
```

Supports
- interpolation
- multiline strings

---

**Common methods**
```js
lengthincludes()
startsWith()
endsWith()
slice()
substring()
replace()
replaceAll()
trim()
split()
toUpperCase()
toLowerCase()
```

#### 4. Boolean

Only two values.
```
true
false
```

Used in conditions.

---

**Truthy values**
```js
{}
[]
"hello"
1
-1
Infinity
Symbol()
42n
```

---

**Falsy values**

There are exactly **8 falsy values**:

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

#### 5. Undefined

Means:
> variable exists but has no value.
```js
let x;
console.log(x); // undefined
```

Also:
```js
const obj = {};
obj.age; // undefined
```

#### 6. Null

Represents an intentional absence of value.
```js
const user = null;
```

Difference:

**undefined**
> not assigned

**null**
> assigned empty value

---

**Strange behavior**

```js
typeof null // "object"
```

Historical JavaScript bug.

#### 7. Symbol

Creates unique identifiers.
```js
const id = Symbol("id");

obj = {
 id = 123
}
```

Every Symbol is unique.
```js
Symbol() === Symbol()
// false
```

Useful for
- hidden object properties
- metadata
- preventing property name collisions
- implementing well-known symbols (e.g., `Symbol.iterator`)


### Reference Types

#### 1. [[Object]]

```js
const user = {    
name: "John"
};
```

#### 2. [[Arrays]]

```js
const numbers = [1,2,3];
```

#### 3. Function

Functions are objects.

```js
function hello() {}
```

```js
typeof hello // "function"
```

#### 4. Date

```js
new Date()
```

#### 5. Map

Key-value collection.
```js
const map = new Map();
```

Keys may be any type.

#### 6. Set

Unique values.
```js
const set = new Set();
```

# typeof Results

|Expression|Result|
|---|---|
|`typeof 5`|`"number"`|
|`typeof 5n`|`"bigint"`|
|`typeof true`|`"boolean"`|
|`typeof "abc"`|`"string"`|
|`typeof undefined`|`"undefined"`|
|`typeof Symbol()`|`"symbol"`|
|`typeof {}`|`"object"`|
|`typeof []`|`"object"`|
|`typeof function(){}`|`"function"`|
|`typeof null`|`"object"` (legacy bug)|
[[Boxing and Unboxing]]

[[Immutability]]

## Destructuring

Destructuring is a clean way to take values from arrays or objects into variables.

## Array Destructuring

Arrays use position.

```js
const numbers = [10, 20, 30];

const [first, second] = numbers;

console.log(first); // 10
console.log(second); // 20
```

Skip values with commas:

```js
const [first, , third] = numbers;
```

## Object Destructuring

Objects use property names.

```js
const user = {
  id: 1,
  name: "Ana"
};

const { id, name } = user;
```

Rename while destructuring:

```js
const { name: userName } = user;
```

Default value:

```js
const { role = "user" } = user;
```

## Rest

Rest collects remaining values.

Array rest:

```js
const [first, ...rest] = [1, 2, 3, 4];

console.log(rest); // [2, 3, 4]
```

Object rest:

```js
const { id, ...profile } = user;
```

`profile` contains the remaining object properties.

## Spread

Spread copies or expands values.

Array spread:

```js
const a = [1, 2];
const b = [...a, 3];
```

Object spread:

```js
const baseUser = { id: 1, name: "Ana" };
const updatedUser = { ...baseUser, name: "Nino" };
```

In arrays, spread works with elements in order.

In objects, spread works with properties and can merge objects.

## Short Answer

Destructuring takes values from arrays or objects into variables. Arrays destructure by position, objects destructure by property name. Rest collects remaining values, while spread copies or expands values. In arrays it works with ordered elements, and in objects it works with properties.
