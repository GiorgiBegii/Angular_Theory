# Async Await

`async / await` is syntax over Promises.

It makes asynchronous code look more like synchronous code.

## async

An `async` function always returns a Promise.

```js
async function getUser() {
  return { id: 1, name: "Ana" };
}

getUser().then(user => console.log(user));
```

Even if you return a normal value, JavaScript wraps it in a Promise.

```js
async function getNumber() {
  return 10;
}

getNumber(); // Promise<number>
```

## await

`await` waits for a Promise to settle.

```js
async function loadUser() {
  const response = await fetch('/api/users/1');
  const user = await response.json();

  return user;
}
```

`await` pauses the async function, but it does not block the event loop.

Other JavaScript work can continue while the Promise is pending.

After `await`, the rest of the async function continues through the [[Call Stack#Microtask Queue|Microtask Queue]].

## Error Handling

Use `try/catch` around awaited operations.

```js
async function loadUser() {
  try {
    const response = await fetch('/api/users/1');
    return await response.json();
  } catch (error) {
    console.error('Failed to load user', error);
    throw error;
  }
}
```

If an awaited Promise rejects, JavaScript jumps to `catch`.

## Sequential Await

Sequential awaits run one after another.

```js
const user = await loadUser();
const orders = await loadOrders(user.id);
```

Use this when the second operation depends on the first.

## Parallel Await

Use `Promise.all` when operations are independent.

```js
const [user, settings] = await Promise.all([
  loadUser(),
  loadSettings(),
]);
```

This is faster because both operations start together.

Important: `Promise.all` rejects if any Promise rejects.

```js
try {
  const [user, settings] = await Promise.all([
    loadUser(),
    loadSettings(),
  ]);
} catch (error) {
  console.error(error);
}
```

## Promise.allSettled

Use `Promise.allSettled` when you want all results, even if some fail.

```js
const results = await Promise.allSettled([
  loadUser(),
  loadSettings(),
]);
```

Each result has a status:

- `fulfilled`
- `rejected`

## Angular Context

Angular HTTP uses Observables by default, so async errors are often handled with RxJS operators.

```ts
this.http.get<User>('/api/user').pipe(
  catchError(error => {
    return throwError(() => error);
  })
);
```

But `async / await` still appears in:

- browser APIs
- one-time initialization
- guards/resolvers in some codebases
- test setup
- converting Promise-based APIs

For Angular apps:

- use `try/catch` for Promise-based async code
- use RxJS `catchError` for Observable streams
- use HTTP interceptors for cross-cutting HTTP error handling
- use global error handling for unexpected application errors

## Common Mistakes

Do not forget `await`:

```js
const user = loadUser(); // Promise, not user
```

Do not use sequential awaits when work can run in parallel:

```js
const user = await loadUser();
const settings = await loadSettings();
```

Prefer:

```js
const [user, settings] = await Promise.all([
  loadUser(),
  loadSettings(),
]);
```

Do not swallow errors silently:

```js
try {
  await saveUser();
} catch (error) {
  // bad: ignored
}
```

## Short Answer

`async / await` is syntax over Promises. An `async` function always returns a Promise, and `await` pauses the async function until the Promise resolves or rejects without blocking the event loop. Errors from awaited Promises should be handled with `try/catch`. For independent async operations, use `Promise.all`, but remember it rejects when any Promise fails.

## Related Notes

- [[Promises]]
- [[Promises#Promise Error Handling|Promise Error Handling]]
- [[Call Stack#Microtask Queue|Microtask Queue]]
- [[RxJS#RxJS Error Handling|RxJS Error Handling]]
