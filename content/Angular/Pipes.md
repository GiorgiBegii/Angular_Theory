# Pipes

Pipes transform values in Angular templates.

They are used for formatting or simple view-level transformations without changing the original data.

```js
{{ user.name | uppercase }}
{{ today | date: 'shortDate' }}
{{ price | currency }}
```

## What Pipes Are For

Use pipes for presentation logic:

- format dates
- format numbers
- format currency
- transform text
- display fallback values
- small reusable template transformations

Pipes should not contain business logic, API calls, or heavy calculations.

## Built-in Pipes

Common Angular pipes:

- `date`
- `currency`
- `decimal`
- `percent`
- `uppercase`
- `lowercase`
- `titlecase`
- `json`
- `async`

Example:

```js
{{ createdAt | date: 'medium' }}
{{ amount | currency: 'USD' }}
```

## Custom Pipe

```ts
@Pipe({
  name: 'initials',
  standalone: true
})
export class InitialsPipe implements PipeTransform {
  transform(fullName: string): string {
    return fullName
      .split(' ')
      .map(part => part[0])
      .join('');
  }
}
```

Usage:

```js
{{ user.fullName | initials }}
```

## Pure Pipes

Pure pipes are the default.

They run only when Angular detects a pure change:

- primitive value changes
- object reference changes
- array reference changes

```ts
@Pipe({
  name: 'activeUsers',
  standalone: true,
  pure: true
})
export class ActiveUsersPipe implements PipeTransform {
  transform(users: User[]): User[] {
    return users.filter(user => user.active);
  }
}
```

Pure pipes are fast and should be preferred.

They work best with [[Immutability|immutable]] data:

```ts
this.users = [...this.users, newUser];
```

If you mutate the same array reference, a pure pipe may not run:

```ts
this.users.push(newUser);
```

## Impure Pipes

Impure pipes run on every change detection cycle.

```ts
@Pipe({
  name: 'activeUsers',
  standalone: true,
  pure: false
})
export class ActiveUsersPipe implements PipeTransform {
  transform(users: User[]): User[] {
    return users.filter(user => user.active);
  }
}
```

They can detect internal mutations, but they are expensive.

Use impure pipes only when:

- data mutates without reference change
- transformation must update every change detection cycle
- the data set is very small
- there is no better state-management approach

Avoid impure pipes for large lists or expensive transformations.

## async Pipe

`async` pipe subscribes to an `Observable` or `Promise` in the template.

```html
@if (user$ | async; as user) {
  <app-user-card [user]="user" />
}
```

It:

- subscribes automatically
- updates the template when a value arrives
- unsubscribes automatically when the view is destroyed
- works well with `OnPush`

Prefer `async` pipe over manual subscriptions in components when the value is only used in the template.

## Pure vs Impure

| Type | When it runs | Performance | Preferred? |
|---|---|---|---|
| Pure pipe | input reference/value changes | fast | yes |
| Impure pipe | every change detection cycle | expensive | rarely |

## Common Mistakes

Do not use pipes for API calls:

```ts
transform(id: number) {
  return this.http.get(`/users/${id}`);
}
```

Do not put heavy filtering/sorting in a pipe for large lists.

Prefer calculating data in the component, service, signal, selector, or RxJS stream.

## Angular Performance Notes

Pipes are connected to [[Change Detection]].

Pure pipes are efficient because Angular can skip them when input references do not change.

Impure pipes can become a performance problem because Angular calls them very often.

For scalable Angular apps:

- prefer pure pipes
- keep pipe logic simple
- use immutable data
- avoid impure pipes unless necessary
- avoid heavy work in templates

## Short Answer

Pipes transform data for display in Angular templates without changing the original value. Pure pipes run only when an input value or reference changes, so they are fast and should be the default. Impure pipes run on every change detection cycle, which allows them to react to internal mutations but makes them expensive. Use pure pipes for formatting and simple transformations; avoid impure pipes unless the use case is small and justified.

## Related Notes

- [[Change Detection]]
- [[Immutability]]
- [[Binding]]
