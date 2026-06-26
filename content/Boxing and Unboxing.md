# What is Boxing?

> **Boxing is the automatic conversion of a primitive value into its corresponding object wrapper.**

Primitive:

```js
const name = "John";
```

When you do:

```js
name.toUpperCase();
```

JavaScript internally behaves approximately like this:

```js
const temp = new String("John");temp.toUpperCase();
```

The wrapper object exists only temporarily.

---

# Primitive Wrapper Objects

|Primitive|Wrapper Object|
|---|---|
|string|`String`|
|number|`Number`|
|boolean|`Boolean`|
|bigint|`BigInt`|
|symbol|`Symbol`|

There are **no wrapper objects** for:

- `null`
- `undefined`

---

# Example

```js
const str = "hello";
console.log(str.length);
console.log(str.toUpperCase());
```

Although `str` is a primitive, both work because JavaScript temporarily boxes it.

Internally:

```js
const temp = new String("hello");
temp.length;
temp.toUpperCase();
```

Then `temp` is discarded.

---

# Unboxing

> **Unboxing is extracting the primitive value from its wrapper object.**

```js
const num = new Number(5);
console.log(num.valueOf());
```

Output
```
5
```

`valueOf()` returns the primitive value.

---

# Why Does Boxing Exist?

Without boxing:

```js
const name = "John";
name.toUpperCase();
```

would fail because primitives don't actually have methods.

Instead, JavaScript creates a temporary wrapper object that provides those methods.

---

# Wrapper Objects vs Primitives

Primitive:

```js
const a = "hello";
```

Object:

```js
const b = new String("hello");
```

They look similar but are **not the same type**.

```js
typeof a;
```

↓

```js
"string"
```

```js
typeof b;
```

↓

```js
"object"
```

---

# Equality

```js
const a = "5";
const b = new String("5");
console.log(a == b);
```

↓

```js
true
```

Loose equality converts the wrapper object to its primitive value.

Strict equality:

```js
console.log(a === b);
```

↓

```js
false
```

Different types:

- primitive
- object

---

# Wrapper Object Truthiness

Very common interview question.

```js
if (new Boolean(false)) {    
	console.log("Runs");
}
```

Output

```
Runs
```

Why?

Because **all objects are truthy**, regardless of the value they wrap.

---

# Avoid Creating Wrapper Objects

Avoid:

```js
const str = new String("hello");
const num = new Number(5);
const bool = new Boolean(true);
```

Prefer:

```js
const str = "hello";
const num = 5;
const bool = true;
```

Wrapper objects can lead to confusing behavior with equality and truthiness.

---

# Can You Add Properties to a Primitive?

```js
const name = "John";
name.age = 20;
console.log(name.age);
```

Output

```
undefined
```

Why?

Internally:

```js
const temp = new String("John");
temp.age = 20;
```

The temporary object is immediately discarded, so the property is lost.

---

# Memory Visualization

```
Primitive"John"
↓
Temporary BoxingString Object
↓
call toUpperCase()
↓
Wrapper destroyed
↓
Primitive remains
```

---

# Common Methods Provided by Wrapper Objects

String

```js
"text".toUpperCase();
"text".includes("t");
```

Number

```js
(123).toFixed(2);
(10).toString();
```

Boolean

```js
true.toString();
```

---
