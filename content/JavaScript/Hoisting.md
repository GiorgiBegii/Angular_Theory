# What is Hoisting?

**Hoisting is JavaScript's behavior of processing declarations before executing the code.**

It does **not** physically move code. During the **creation phase** of an execution context, JavaScript scans the code and allocates memory for variables and functions.

Execution happens later.

```js
console.log(a);
var a = 10;
```

Output:

```
undefined
```

Internally JavaScript behaves approximately like:

```js
var a;
console.log(a);
a = 10;
```

The code is **not rewritten**—this is simply how the engine initializes declarations before execution.

---

# Execution Context

Hoisting happens during the **Creation Phase** of an Execution Context.

Execution Context has two phases:

```
Execution Context
1. Creation Phase    
   • Create Lexical Environment    
   • Create Variable Environment    
   • Allocate memory    
   • Hoist declarations
2. Execution Phase    
   • Execute code line by line    
   • Assign values    
   • Call functions
```

Everything about hoisting comes from these two phases.

---
# What Gets Hoisted?

|Declaration|Hoisted|Initial Value|
|---|---|---|
|var|✅ Yes|`undefined`|
|let|✅ Yes|Uninitialized (TDZ)|
|const|✅ Yes|Uninitialized (TDZ)|
|function declaration|✅ Yes|Entire function|
|function expression|Variable only|Depends on `var`, `let`, or `const`|
|class|✅ Yes|Uninitialized (TDZ)|
|import|✅ Yes|Available before module code runs|

---
# var Hoisting

```js
console.log(a);
var a = 5;
```

Output
```
undefined
```

Memory after Creation Phase
```
Global Environment 
a → undefined
```

Execution
```js
a = 5
```

---

# let Hoisting

```js
console.log(a);
let a = 5;
```

Output
```
ReferenceError
```

`let` **is hoisted**, but it is **not initialized**.

It stays inside the **Temporal Dead Zone**.

Memory
```
a → <uninitialized>
```

---

# const Hoisting

```js
console.log(a);
const a = 5;
```

Output
```
ReferenceError
```

Exactly like `let`.

Difference:

`const` must also receive a value during initialization.

```js
const a;
```
↓
```
SyntaxError
```

---

# Temporal Dead Zone (TDZ)

The TDZ is the period between entering a scope and the point where a `let`, `const`, or `class` declaration is initialized.

```
Scope begins
↓
TDZ
↓
let a = 5;
↓
Variable initialized
```

Example
```js
{    
	console.log(a);    
	let a = 10;
}
```

Output
```
ReferenceError
```

After initialization
```js
{    
	let a = 10;    
	console.log(a);
}
```

↓

```
10
```

---

# Why Does TDZ Exist?

To prevent accidental access before initialization.

Without TDZ:
```js
let total = price * quantity;
```

If `price` wasn't initialized yet, bugs would be harder to detect.

TDZ makes these mistakes fail immediately.

---

# Function Declaration Hoisting

Entire function is created during the Creation Phase.

```js
sayHello();
function sayHello() {    
	console.log("Hello");
}
```

Output
```
Hello
```

Memory
```
sayHello → function
```

---

# Function Expression

```js
sayHello();
var sayHello = function () {};
```

Creation Phase

```
sayHello → undefined
```

Execution

```js
sayHello();
```

↓

```
TypeErrorundefined is not a function
```

Later
```js
sayHello = function(){}
```

---

# Function Expression with let

```js
sayHello();
let sayHello = function () {};
```

Output

```
ReferenceError
```

Because `let` is inside TDZ.

---

# Arrow Functions

```js
hello();
const hello = () => {};
```

Output

```
ReferenceError
```

Arrow functions are **not special**.

They behave like the variable they're assigned to.

---

# Class Hoisting

Classes are hoisted but remain in TDZ.

```js
const user = new User();
class User {}
```

Output

```
ReferenceError
```

---

# Hoisting Inside Functions

```js
function test() {    
	console.log(a);    
	var a = 5;
}
test();
```

Output
```
undefined
```

Creation Phase for the function
```
Function Environmenta → undefined
```

---

# Shadowing

```js
let a = 1;
function test() {    
	console.log(a);    
	let a = 2;
}
test();
```

Output
```
ReferenceError
```

The inner `let a` shadows the outer one. Even though the outer `a` exists, the inner binding is in the TDZ until initialized.

---

# Block Scope

```js
{    
	var a = 1;
}
console.log(a);
```

↓

```
1
```

`var` ignores block scope.

---

```js
{    
	let a = 1;
}
console.log(a);
```

↓

```
ReferenceError
```

---

# Global Object

```js
var a = 5;
```

In classic browser scripts
```js
window.a
```

↓

```
5
```

But
```js
let b = 10;
const c = 20;
```

```
window.b
```

↓

```
undefined
```

In ES modules, even top-level `var` does **not** become a property of the global object.

---
