# Lexical Environment

A lexical environment is the internal structure JavaScript uses to store variables and connect scopes.

Simple idea:

> Lexical environment = local variables + link to parent environment.

## What It Contains

Each lexical environment has:

- **environment record** - variables, functions, parameters declared in the current scope.
- **outer reference** - link to the parent lexical environment.

```js
const globalValue = "global";

function outer() {
  const outerValue = "outer";

  function inner() {
    const innerValue = "inner";
    console.log(innerValue, outerValue, globalValue);
  }
}
```

For `inner()`, JavaScript has access to:

- `innerValue` from inner lexical environment
- `outerValue` from outer lexical environment
- `globalValue` from global lexical environment

This chain is called the [[Scope]] chain.

## Created When

A lexical environment is created when JavaScript enters:

- global code
- a function
- a block with `let` / `const`
- a module

```js
function test() {
  const a = 1;

  if (true) {
    const b = 2;
  }
}
```

Here JavaScript creates:

- function lexical environment for `test`
- block lexical environment for the `if` block

## Lexical Means Written Position

JavaScript decides variable access based on where the function is written.

```js
const name = "Global";

function sayName() {
  console.log(name);
}

function run() {
  const name = "Local";
  sayName();
}

run(); // "Global"
```

`sayName()` uses the environment where it was created, not where it was called.

## Closures

A closure happens when a function keeps access to its outer lexical environment.

```js
function createCounter() {
  let count = 0;

  return function increment() {
    count++;
    return count;
  };
}

const counter = createCounter();

counter(); // 1
counter(); // 2
```

`increment()` still remembers `count` because it keeps a reference to the lexical environment of `createCounter()`.

## Short Answer

A lexical environment is how JavaScript stores variables for a scope. It contains the current scope's variables and a reference to the parent scope. This is what makes lexical scope, scope chain, and closures work.

## Related Notes

- [[Scope]]
- [[Closures]]
- [[Hoisting]]
