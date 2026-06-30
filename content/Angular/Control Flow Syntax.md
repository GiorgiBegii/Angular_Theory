# Control Flow Syntax

Control flow syntax is Angular's modern template syntax for rendering conditional blocks and lists.

It includes:

- `@if`
- `@else if`
- `@else`
- `@for`
- `@empty`
- `@switch`
- `@case`
- `@default`

## Why It Exists

Before modern control flow, Angular used structural directives:

```html
<div *ngIf="isLoggedIn">Welcome</div>

<li *ngFor="let user of users">
  {{ user.name }}
</li>
```

Modern Angular can use:

```html
@if (isLoggedIn) {
  <div>Welcome</div>
}

@for (user of users; track user.id) {
  <li>{{ user.name }}</li>
}
```

`@if` and `@for` are easier to read, more explicit, and built into Angular's template compiler.

## @if

`@if` conditionally renders part of the template.

```html
@if (user) {
  <app-user-card [user]="user" />
} @else {
  <p>No user selected</p>
}
```

Use `@if` when the UI depends on a condition.

## @for

`@for` renders a list.

```html
@for (user of users; track user.id) {
  <app-user-card [user]="user" />
}
```

The `track` expression helps Angular identify items efficiently when the list changes.

Use stable unique values like:

- `user.id`
- `item.key`
- another unique identifier

## @empty

`@empty` renders fallback content when the list is empty.

```html
@for (user of users; track user.id) {
  <app-user-card [user]="user" />
} @empty {
  <p>No users found</p>
}
```

## @switch

`@switch` renders different blocks based on one value.

```html
@switch (status) {
  @case ('loading') {
    <p>Loading...</p>
  }
  @case ('error') {
    <p>Error</p>
  }
  @default {
    <p>Ready</p>
  }
}
```

## Difference From Structural Directives

`*ngIf` and `*ngFor` are structural directives.

`@if` and `@for` are built-in control flow syntax.

They solve similar UI problems, but the implementation is different:

- structural directives are directive-based
- control flow syntax is compiler-level template syntax

## Short Answer

Control flow syntax is Angular's modern way to write conditional rendering and list rendering. `@if` replaces many `*ngIf` use cases, and `@for` replaces many `*ngFor` use cases. They are built into the template compiler, are easier to read, and make tracking list items explicit with `track`.

## Related Notes

- [[Directives]]
- [[Binding]]
- [[Change Detection]]
