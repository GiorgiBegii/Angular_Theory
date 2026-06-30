# Functions

A function is a reusable block of code that can receive input, execute logic, and return a result.

Functions are first-class values in JavaScript. That means they can be stored in variables, passed as arguments, returned from other functions, and used as object methods.

## Function Declaration

A function declaration is created with the `function` keyword.

```js
function sum(a, b) {
  return a + b;
}

sum(2, 3); // 5
```

Function declarations are hoisted, so they can be called before they appear in the file.

```js
sayHello();

function sayHello() {
  console.log("Hello");
}
```

## Function Expression

A function expression stores a function in a variable.

```js
const sum = function (a, b) {
  return a + b;
};
```

Function expressions are not fully hoisted like function declarations.

```js
sum(2, 3); // ReferenceError

const sum = function (a, b) {
  return a + b;
};
```

## Arrow Functions

Arrow functions are a shorter syntax for writing functions.

```js
const sum = (a, b) => {
  return a + b;
};
```

Short form:

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

Here the arrow function keeps `this` from `UserComponent`.

Regular functions can lose `this`:

```ts
export class UserComponent {
  name = "Ana";

  loadUser() {
    setTimeout(function () {
      console.log(this.name);
    }, 1000);
  }
}
```

In this example, `this` does not point to the component instance.

## Constructor Functions

Constructor functions are older JavaScript patterns for creating objects before `class` syntax became common.

They are called with `new`.

```js
function User(name) {
  this.name = name;
}

const user = new User("Ana");

console.log(user.name); // "Ana"
```

When a function is called with `new`, JavaScript:

1. creates a new empty object
2. sets `this` to that new object
3. links the object to the function's prototype
4. returns the object automatically

Methods are usually added through `prototype`:

```js
function User(name) {
  this.name = name;
}

User.prototype.sayName = function () {
  console.log(this.name);
};

const user = new User("Ana");
user.sayName(); // "Ana"
```

Modern JavaScript usually uses `class` syntax instead:

```js
class User {
  constructor(name) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }
}
```

`class` is mostly cleaner syntax over the prototype system.

## this Context

In regular functions, `this` is determined by how the function is called.

```js
const user = {
  name: "Ana",
  sayName() {
    console.log(this.name);
  }
};

user.sayName(); // "Ana"
```

If the method is detached, `this` can be lost.

```js
const fn = user.sayName;
fn(); // lost this
```

Arrow functions are different because they do not create their own `this`.

## Angular / RxJS Usage

In Angular, arrow functions are commonly useful in callbacks, event handlers, and RxJS subscriptions because they preserve the component instance context.

```ts
this.userService.getUser().subscribe(user => {
  this.user = user;
});
```

Avoid regular functions when you need component `this`:

```ts
this.userService.getUser().subscribe(function (user) {
  this.user = user; // wrong context
});
```

## Callback Functions

A callback is a function passed into another function to be called later.

```js
function loadUser(callback) {
  const user = { name: "Ana" };
  callback(user);
}

loadUser(user => {
  console.log(user.name);
});
```

Callbacks are common in event handlers, timers, array methods, and async code.

```js
setTimeout(() => {
  console.log("Done");
}, 1000);
```

## Higher Order Functions

A higher order function is a function that:

- receives another function as an argument
- returns another function
- or both

```js
const numbers = [1, 2, 3];

const doubled = numbers.map(number => number * 2);
```

`map` is a higher order function because it receives a callback.

Common examples:

- `map`
- `filter`
- `reduce`
- `forEach`
- RxJS operators

## Pure Functions

A pure function:

- returns the same output for the same input
- does not change outside state
- has no side effects

```js
function add(a, b) {
  return a + b;
}
```

Not pure:

```js
let total = 0;

function addToTotal(value) {
  total += value;
}
```

Pure functions are easier to test, reuse, and reason about.

In Angular, pipes, validators, reducers, and utility functions should often be pure.

## Recursion

Recursion is when a function calls itself.

It needs:

- base case - when to stop
- recursive case - call itself with smaller/different input

```js
function factorial(n) {
  if (n === 1) {
    return 1;
  }

  return n * factorial(n - 1);
}
```

Use recursion for tree-like data, nested comments, menus, routes, and hierarchical structures.

## Memoization

Memoization caches function results so repeated calls with the same input are faster.

```js
function memoize(fn) {
  const cache = new Map();

  return function (value) {
    if (cache.has(value)) {
      return cache.get(value);
    }

    const result = fn(value);
    cache.set(value, result);
    return result;
  };
}
```

Useful when a function is expensive and called many times with the same arguments.

## Currying

Currying transforms a function with multiple arguments into a chain of functions.

```js
const add = a => b => a + b;

add(2)(3); // 5
```

It is useful when you want to create specialized functions.

```js
const hasRole = role => user => user.role === role;

const isAdmin = hasRole("admin");
```

## Partial Application

Partial application means fixing some arguments now and passing the rest later.

```js
function multiply(a, b) {
  return a * b;
}

const double = multiply.bind(null, 2);

double(5); // 10
```

Currying creates a chain of one-argument functions. Partial application pre-fills some arguments.

## call, apply, bind

`call`, `apply`, and `bind` control what `this` points to.

```js
function greet(message) {
  console.log(`${message}, ${this.name}`);
}

const user = { name: "Ana" };

greet.call(user, "Hello");
```

`apply` is similar to `call`, but arguments are passed as an array.

```js
greet.apply(user, ["Hello"]);
```

`bind` does not call the function immediately. It returns a new function with fixed `this`.

```js
const boundGreet = greet.bind(user);

boundGreet("Hello");
```

## Short Answer

Functions are reusable blocks of code and first-class values in JavaScript. Function declarations are hoisted, function expressions are assigned to variables, arrow functions use shorter syntax and capture `this` lexically, and constructor functions create objects when called with `new`.

Important function patterns include callbacks, higher order functions, pure functions, recursion, memoization, currying, partial application, and manual `this` control with `call`, `apply`, and `bind`.

For `this`: regular functions get `this` from how they are called, while arrow functions do not have their own `this`; they capture it from the surrounding scope. In Angular, this is important because arrow functions keep access to the component instance inside callbacks and RxJS handlers.

## Related Notes

- [[Language Concepts#this|this]]
- [[Scope]]
- [[Lexical Environment]]
- [[Hoisting]]
- [[Object]]
- [[Modules and Application Structure#Strict Mode|Strict Mode]]
- [[Closures]]
