# Promises

A Promise represents a value that will be available later.

It is used for one-time asynchronous work.

Common examples:

- HTTP request
- file loading
- delayed operation
- browser API result

## Promise States

A Promise has three states:

| State | Meaning |
|---|---|
| `pending` | still running |
| `fulfilled` | completed successfully |
| `rejected` | failed |

```js
const promise = fetch('/api/users');
```

At first, the Promise is `pending`.

Later it becomes either fulfilled or rejected.

## then, catch, finally

```js
fetch('/api/users')
  .then(response => response.json())
  .then(users => {
    console.log(users);
  })
  .catch(error => {
    console.error(error);
  })
  .finally(() => {
    console.log('Finished');
  });
```

- `then` handles successful result
- `catch` handles errors
- `finally` runs after success or error

## Promise Error Handling

Promise errors can be handled with `.catch()`.

```js
loadUser()
  .then(user => console.log(user))
  .catch(error => console.error(error));
```

With `async / await`, rejected Promises are handled with `try/catch`.

```js
async function saveUser() {
  try {
    await api.saveUser();
  } catch (error) {
    console.error('Save failed', error);
  }
}
```

Use `finally` for cleanup or loading state reset.

```js
try {
  await saveUser();
} catch (error) {
  console.error(error);
} finally {
  this.loading = false;
}
```

Do not silently swallow errors.

```js
try {
  await saveUser();
} catch (error) {
  // bad: ignored
}
```

In Angular:

- Promise errors use `try/catch`
- Observable errors use [[RxJS#RxJS Error Handling|RxJS catchError]]
- HTTP errors are often handled in interceptors
- unexpected app errors can be handled globally

## Promise and Microtask Queue

Promise callbacks do not run immediately.

When a Promise resolves or rejects, callbacks from:

- `.then()`
- `.catch()`
- `.finally()`

are placed into the [[Call Stack#Microtask Queue|Microtask Queue]].

Microtasks run after the current call stack is empty, but before the next macrotask.

```js
console.log('start');

setTimeout(() => {
  console.log('timeout');
}, 0);

Promise.resolve().then(() => {
  console.log('promise');
});

console.log('end');
```

Output:

```text
start
end
promise
timeout
```

The Promise runs before `setTimeout` because microtasks are processed before the next [[Call Stack#Macrotask Queue|Macrotask Queue]] task.

## Promise vs setTimeout

`setTimeout` callback goes to the macrotask queue.

Promise callback goes to the microtask queue.

Microtasks have higher priority.

```js
setTimeout(() => console.log('macro'), 0);
Promise.resolve().then(() => console.log('micro'));

// micro
// macro
```

## async / await

`async / await` is syntax over Promises.

```js
async function loadUsers() {
  const response = await fetch('/api/users');
  return response.json();
}
```

After `await`, the continuation of the function also resumes through the microtask queue.

## Angular Context

Promise timing matters in Angular because:

- Zone.js tracks async work like Promises
- change detection may run after async tasks
- microtask timing affects UI update order
- `async / await` code is Promise-based

In Angular, HTTP usually uses Observables, but Promises still appear in:

- `async / await`
- browser APIs
- guards/resolvers sometimes
- one-time async initialization

## Short Answer

Promises represent one-time async results. When a Promise resolves or rejects, its `.then()`, `.catch()`, and `.finally()` callbacks go into the microtask queue. The event loop processes all microtasks before moving to the next macrotask like `setTimeout`, DOM events, or HTTP callbacks. That is why resolved Promise callbacks run before `setTimeout(..., 0)`.

## Related Notes

- [[Call Stack]]
- [[Call Stack#Microtask Queue|Microtask Queue]]
- [[Call Stack#Macrotask Queue|Macrotask Queue]]
- [[Async Await]]
- [[RxJS#RxJS Error Handling|RxJS Error Handling]]
- [[Zone.js]]
