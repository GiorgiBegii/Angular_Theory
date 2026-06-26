### Primitive Types

Primitive values are:
- Immutable
- Stored by value
- Compared by value

#### 1. Number

Represents both integers and floating-point numbers.
```
let age = 25;let price = 10.5;
```

There is **only one numeric type** (excluding `BigInt`).
```
typeof 5 // "number"typeof 5.2 // "number"
```

**Special numeric values NaN Means Not a Number**.
```
Number("abc") // NaN
```

```
0 / 0 // NaN
```

Interesting fact:
```
NaN === NaN // false
```

Correct check:
```
Number.isNaN(value)
```


#### 2. BigInt

For integers larger than `MAX_SAFE_INTEGER`.
```
const n = 12345678901234567890n;
```

or

```
BigInt("12345678901234567890")
```

Cannot mix with Number.
```
1n + 1 // TypeError
```

Must convert first.

#### 3. String

Represents text.
```
const name = "John";
```

Strings are immutable.
```
let str = "Hello";str[0] = "A";console.log(str);// Hello
```

---

**Template literals**
```
`Hello ${name}`
```

Supports
- interpolation
- multiline strings

---

**Common methods**
```
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
truefalse
```

Used in conditions.

---

**Truthy values**
```
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

```
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
```
let x;console.log(x); // undefined
```

Also:
```
const obj = {};obj.age; // undefined
```

#### 6. Null

Represents an intentional absence of value.
```
const user = null;
```

Difference:

**undefined**
> not assigned

**null**
> assigned empty value

---

**Strange behavior**

```
typeof null // "object"
```

Historical JavaScript bug.

#### 7. Symbol

Creates unique identifiers.
```
const id = Symbol("id");

obj = {
 id = 123
}
```

Every Symbol is unique.
```
Symbol() === Symbol()// false
```

Useful for
- hidden object properties
- metadata
- preventing property name collisions
- implementing well-known symbols (e.g., `Symbol.iterator`)


### Reference Types

#### 1. [[Object]]

```
const user = {    
name: "John"
};
```

#### 2. Array

```
const numbers = [1,2,3];
```

#### 3. Function

Functions are objects.

```
function hello() {}
```

```
typeof hello // "function"
```

#### 4. Date

```
new Date()
```

#### 5. Map

Key-value collection.
```
const map = new Map();
```

Keys may be any type.

#### 6. Set

Unique values.
```
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