# Scope

Scope defines **where a variable can be accessed** in code.

JavaScript uses **lexical scope**. That means scope is decided by where code is written, not where a function is called.

## Main Types

### Global Scope

Available everywhere in the file or runtime.

```js
const appName = "Dashboard";

function printName() {
  console.log(appName);
}
```

### Function Scope

Variables declared inside a function are available only inside that function.

```js
function login() {
  const token = "abc";
  console.log(token);
}

console.log(token); // ReferenceError
```

### Block Scope

`let` and `const` are scoped to the nearest block.

```js
if (true) {
  const user = "Ana";
}

console.log(user); // ReferenceError
```

`var` is not block-scoped. It is function-scoped.

```js
if (true) {
  var count = 1;
}

console.log(count); // 1
```

## Scope Chain

When JavaScript looks for a variable, it checks:

1. current scope
2. parent scope
3. next parent scope
4. global scope

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

`inner()` can access its own variables, variables from `outer()`, and global variables.

## Angular Example

```ts
export class UserComponent {
  userName = "Ana";

  loadUser() {
    const requestId = 1;
    console.log(this.userName);
    console.log(requestId);
  }
}
```

- `userName` belongs to the component instance.
- `requestId` exists only inside `loadUser()`.
- Template can access component properties, but not local variables inside methods.

## Common Mistakes

Do not expect block variables to exist outside the block:

```js
if (isAdmin) {
  const role = "admin";
}

console.log(role); // ReferenceError
```

Be careful with `var` in loops because it is function-scoped:

```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}

// 3, 3, 3
```

Use `let` for block scope:

```js
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}

// 0, 1, 2
```

## Short Answer

Scope defines where variables are visible. JavaScript uses lexical scope, so variable access depends on where functions and blocks are written. If a variable is not found in the current scope, JavaScript searches parent scopes through the scope chain.

## Related Notes

- [[Lexical Environment]]
- [[Closures]]
- [[Hoisting]]
- [[Modules and Application Structure#Strict Mode|Strict Mode]]
