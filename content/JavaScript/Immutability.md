# Immutability

Immutability means **do not change existing data directly**.

Instead, create a new value, object, or array with the updated data.

## Primitive Values

Primitive values are already immutable.

```js
let name = "Ana";

name.toUpperCase();

console.log(name); // "Ana"
```

The string did not change. JavaScript returned a new string.

## Objects

Do not mutate an existing object:

```js
user.name = "Nino";
```

Create a new object:

```js
user = {
  ...user,
  name: "Nino"
};
```

## Arrays

Do not mutate an existing array:

```js
users.push(newUser);
```

Create a new array:

```js
users = [...users, newUser];
```

Update an item immutably:

```js
users = users.map(user =>
  user.id === updatedUser.id ? updatedUser : user
);
```

Remove an item immutably:

```js
users = users.filter(user => user.id !== deletedUserId);
```

## Shallow Immutability

Object spread and array spread create shallow copies.

```js
const newUser = {
  ...user,
  profile: {
    ...user.profile,
    city: "Tbilisi"
  }
};
```

Nested objects must also be copied if they change.

## Why It Matters

Immutability helps because it:

- prevents hidden side effects
- makes state predictable
- makes debugging easier
- works well with pure functions
- improves Angular `OnPush` change detection
- works well with signals, RxJS, and store patterns

## Angular Example

Bad:

```ts
this.users.push(newUser);
```

Better:

```ts
this.users = [...this.users, newUser];
```

With `OnPush`, changing the array reference helps Angular detect that data changed.

## RxJS Example

When emitting state, prefer new objects or arrays:

```ts
this.usersSubject.next([...users, newUser]);
```

This avoids mutating shared state that other subscribers may already use.

## Short Answer

Immutability means updating data by creating new values instead of changing existing ones. It applies to arrays, objects, state, Angular inputs, RxJS emissions, and store data. It prevents side effects, makes state predictable, and helps Angular detect changes through new references.

## Related Notes

- [[Data Types]]
- [[Arrays]]
- [[Object]]
- [[Reference Types]]
- [[Change Detection]]
- [[Functions#Pure Functions|Pure Functions]]

