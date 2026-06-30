# Binding

Binding is how Angular connects component data and template UI.

Short idea:

- component -> template: show data
- template -> component: react to user actions
- component <-> template: keep value synced both ways

## Template Binding

Template binding means using Angular template syntax to connect HTML with component properties, methods, and events.

Common binding types:

- interpolation: `{{ value }}`
- property binding: `[property]="value"`
- event binding: `(event)="handler()"`
- two-way binding: `[(ngModel)]="value"`

## Interpolation

Interpolation (`{{ }}`) is Angular's one-way binding syntax used to render component data as text in the template.

It evaluates expressions in the component context and updates the DOM when change detection runs.

```html
<h1>{{ title }}</h1>
<p>Hello, {{ userName }}</p>
```

```ts
title = "Dashboard";
userName = "Ana";
```

## Property Binding

Property binding (`[prop]`) binds component values to DOM properties, enabling dynamic updates of element state.

Examples:

- `[disabled]`
- `[src]`

It is also one-way-from component to view.

```html
<img [src]="avatarUrl" />
<button [disabled]="isLoading">Save</button>
<app-user-card [user]="selectedUser" />
```

```ts
avatarUrl = "/avatar.png";
isLoading = true;
selectedUser = { id: 1, name: "Ana" };
```

## Event Binding

Event binding (`(event)`) listens to DOM events and triggers component methods, enabling communication from view back to component logic.

Together, these bindings form Angular's unidirectional data flow within templates, managed by change detection.

```html
<button (click)="save()">Save</button>
<input (input)="onSearch($event)" />
```

```ts
save() {
  // save data
}

onSearch(event: Event) {
  const value = (event.target as HTMLInputElement).value;
}
```

Common events:

- `(click)`
- `(input)`
- `(change)`
- `(submit)`
- `(keyup)`

## Two-way Binding

Two-way binding in Angular combines property binding and event binding into a single syntax using `[( )]`, commonly called banana-in-a-box syntax.

It provides synchronization between the component and the view, meaning changes in the UI automatically update the component state, and updates in the component are immediately reflected in the UI.

Internally, two-way binding is just syntactic sugar for:

- `[property]` -> updates the view from the component
- `(event)` -> updates the component from the view

Example:

```html
<input [(ngModel)]="name" />
```

```ts
name = "Ana";
```

`[(ngModel)]="value"` binds an input field to a component property, keeping both in sync.

It is most commonly used in template-driven forms, but in large-scale Angular applications, reactive forms are often preferred for better control and scalability.

## Direction

| Syntax | Direction | Example |
|---|---|---|
| `{{ value }}` | component -> template | text rendering |
| `[property]` | component -> template | `[disabled]="isLoading"` |
| `(event)` | template -> component | `(click)="save()"` |
| `[(value)]` | both directions | `[(ngModel)]="name"` |

## Short Answer

Angular binding connects component state with the template. Interpolation renders text, property binding passes values to properties or inputs, event binding reacts to user actions, and two-way binding keeps a component value and UI value synchronized.

## Related Notes

- [[Components]]
- [[Reactive Forms]]
- [[Directives]]
