# Arrays

An array is an ordered collection of values.

Arrays are reference types, so they are compared by reference, not by content.

```js
const a = [1, 2];
const b = [1, 2];

a === b; // false
```

## Basic Usage

```js
const users = ["Ana", "Nino", "Gio"];

users[0]; // "Ana"
users.length; // 3
```

Arrays can contain any value:

```js
const mixed = [1, "text", true, { id: 1 }];
```

## Fundamental Transformation Methods

### map

`map` transforms every item and returns a new array.

```js
const numbers = [1, 2, 3];

const doubled = numbers.map(number => number * 2);

console.log(doubled); // [2, 4, 6]
```

Use `map` when the output array should have the same number of items.

### filter

`filter` keeps only items that match a condition.

```js
const users = [
  { name: "Ana", active: true },
  { name: "Nino", active: false }
];

const activeUsers = users.filter(user => user.active);
```

Use `filter` when you want fewer items.

### reduce

`reduce` converts an array into one result.

```js
const numbers = [10, 20, 30];

const total = numbers.reduce((sum, number) => sum + number, 0);

console.log(total); // 60
```

Use `reduce` for totals, grouping, maps, or building a new structure.

## Mutating vs Non-Mutating Methods

Mutating methods change the original array:

- `push`
- `pop`
- `shift`
- `unshift`
- `splice`
- `sort`
- `reverse`

Non-mutating methods return a new value:

- `map`
- `filter`
- `reduce`
- `slice`
- `concat`
- `toSorted`
- `toReversed`
- `toSpliced`

## Immutable Updates

Do not mutate state directly:

```js
users.push(newUser);
```

Create a new array instead:

```js
users = [...users, newUser];
```

Update item:

```js
users = users.map(user =>
  user.id === updatedUser.id ? updatedUser : user
);
```

Remove item:

```js
users = users.filter(user => user.id !== deletedUserId);
```

## Angular / RxJS Context

Arrays are common in Angular templates:

```html
@for (user of users; track user.id) {
  <app-user-card [user]="user" />
}
```

Immutable array updates are important because:

- `OnPush` change detection works better with new references
- pure pipes run when references change
- RxJS streams are easier to reason about
- state updates become predictable

## Set

`Set` is a collection of unique values.

```js
const ids = new Set([1, 2, 2, 3]);

console.log(ids); // Set(3) { 1, 2, 3 }
```

Use `Set` when you need:

- unique values
- fast membership checks
- removing duplicates

```js
const uniqueTags = [...new Set(tags)];
```

Check if value exists:

```js
ids.has(2); // true
```

Compared to arrays:

- array is better for ordered transformations like `map`, `filter`, `reduce`
- set is better for uniqueness and membership checks

## Short Answer

Arrays are ordered collections and reference types. The most important transformation methods are `map`, `filter`, and `reduce`. `map` transforms items, `filter` selects items, and `reduce` builds one result. In Angular and RxJS, immutable array updates are preferred because they avoid side effects and create new references for predictable UI updates.

## Related Notes

- [[Data Types]]
- [[Reference Types]]
- [[Immutability]]
- [[Functions#Higher Order Functions|Higher Order Functions]]
- [[Control Flow Syntax#@for|@for]]
