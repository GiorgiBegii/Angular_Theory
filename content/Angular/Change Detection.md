# Change Detection

Change Detection is the process Angular uses to keep the template/DOM synchronized with component state.

Simple idea:

```text
state changes
Angular checks bindings
DOM updates if values changed
```

## What Angular checks

Angular checks template expressions and bindings:

```js
{{ user.name }}
[disabled]="isSaving"
@if (isLoggedIn) {}
@for (item of items; track item.id) {}
```

If the value used by the template changed, Angular updates the DOM.

## Change Detection tree

Angular application is a component tree.

Change detection also works as a tree:

```text
AppComponent
+-- HeaderComponent
+-- UserPageComponent
|   +-- UserListComponent
|   +-- UserCardComponent
+-- FooterComponent
```

When Angular runs change detection, it decides which components/subtrees need to be checked.

## Strategies

Angular has two important strategies:

- `Default` / eager checking
- `OnPush`

## Default strategy

Default strategy checks the component and its children whenever Angular runs change detection.

```ts
@Component({
  selector: 'app-user',
  templateUrl: './user.component.html',
  changeDetection: ChangeDetectionStrategy.Default,
})
export class UserComponent {}
```

Default is simple, but in large apps it may check more components than needed.

## OnPush strategy

`OnPush` tells Angular to skip a component subtree until Angular knows it may be dirty.

```ts
@Component({
  selector: 'app-user-card',
  templateUrl: './user-card.component.html',
  changeDetection: ChangeDetectionStrategy.OnPush,
})
export class UserCardComponent {}
```

In modern Angular, `OnPush` is important because it makes UI updates more predictable and improves runtime performance in large component trees.

> Note: Angular v22 docs mention `OnPush` as the default strategy in Angular v22+. In older Angular versions, `Default` was the normal default unless `OnPush` was configured manually.

## When OnPush component updates

An `OnPush` component is checked when:

- an `@Input()` / `input()` receives a new value from template binding
- an event happens inside the component subtree
- an `async` pipe emits a new value
- a signal used in the template changes
- `ChangeDetectorRef.markForCheck()` is called
- a view that was marked dirty is attached

## Input reference rule

With `OnPush`, object reference matters.

Bad:

```ts
this.user.name = 'Giorgi';
```

The object is mutated, but the reference is the same.

Better:

```ts
this.user = {
  ...this.user,
  name: 'Giorgi',
};
```

Now Angular receives a new object reference.

## Signals and Change Detection

Signals work very well with `OnPush`.

```ts
count = signal(0);

increment() {
  this.count.update(value => value + 1);
}
```

Template:

```js
{{ count() }}
```

When a signal is read in an `OnPush` component template, Angular tracks it. When the signal changes, Angular marks that component for update.

## Async pipe

Prefer `async` pipe instead of manual subscription in components.

```ts
users$ = this.userService.getUsers();
```

```html
@for (user of users$ | async; track user.id) {
  <app-user-card [user]="user" />
}
```

Why it is good:

- subscribes automatically
- unsubscribes automatically
- marks component for check when value emits
- works well with `OnPush`

## Manual subscription problem

Problem:

```ts
users: User[] = [];

ngOnInit() {
  this.userService.getUsers().subscribe(users => {
    this.users = users;
  });
}
```

This can work, but senior Angular code must consider:

- unsubscribe / `takeUntilDestroyed`
- error handling
- loading state
- whether view is marked for check
- avoiding nested subscriptions

Better:

```ts
users$ = this.userService.getUsers();
```

Use `async` pipe in template.

## ChangeDetectorRef

`ChangeDetectorRef` gives manual control over change detection.

### markForCheck()

Marks an `OnPush` view as dirty so Angular checks it later.

Use when something changed but Angular did not automatically know.

```ts
constructor(private cdr: ChangeDetectorRef) {}

loadFromExternalCallback() {
  externalLibrary.onChange(value => {
    this.value = value;
    this.cdr.markForCheck();
  });
}
```

### detectChanges()

Runs change detection immediately for this view and its children.

Use carefully.

```ts
this.cdr.detectChanges();
```

Good for local/manual checks, especially with detached views.

### detach()

Removes the view from the normal change detection tree.

```ts
this.cdr.detach();
```

Useful for very expensive read-only sections that update too often.

### reattach()

Adds the view back to the normal change detection tree.

```ts
this.cdr.reattach();
```

## markForCheck vs detectChanges

| API | Meaning | When to use |
| --- | --- | --- |
| `markForCheck()` | schedule this view for checking later | most common with `OnPush` |
| `detectChanges()` | check this view immediately | local/manual update |
| `detach()` | remove view from CD tree | expensive component |
| `reattach()` | add view back to CD tree | resume normal updates |

## Zone.js and Change Detection

Traditionally, Angular used [[Zone.js]] to know when async work finishes.

Examples:

- click event
- HTTP response
- `setTimeout`
- `Promise`
- `setInterval`

Zone.js patches async browser APIs and notifies Angular that change detection may be needed.

## Zone pollution

Zone pollution means too many async events trigger unnecessary change detection.

Common sources:

- `mousemove`
- `scroll`
- `resize`
- animations
- charts
- third-party libraries

Fix with `NgZone.runOutsideAngular()`:

```ts
constructor(private ngZone: NgZone) {}

ngOnInit() {
  this.ngZone.runOutsideAngular(() => {
    window.addEventListener('scroll', this.onScroll);
  });
}
```

When UI must update, go back into Angular:

```ts
this.ngZone.run(() => {
  this.showButton = true;
});
```

## Zoneless Angular

[[Zoneless]] Angular means Angular does not depend on Zone.js to detect async work.

Instead, Angular relies on explicit notifications:

- signal update used in template
- `async` pipe emission
- `markForCheck()`
- component input update
- template/host listener
- attaching a dirty view

Angular v21+ is zoneless by default. In Angular v20, zoneless can be enabled with:

```ts
bootstrapApplication(AppComponent, {
  providers: [provideZonelessChangeDetection()],
});
```

In zoneless apps, code must be more explicit. If an external callback changes state and Angular does not know, use signals or `markForCheck()`.

## Performance rules

### Use OnPush for presentational components

```ts
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush,
})
export class UserCardComponent {}
```

Best for:

- cards
- rows
- list items
- reusable UI components
- pure/presentational components

### Use track in @for

Bad:

```html
@for (user of users; track $index) {
  <app-user-card [user]="user" />
}
```

Better:

```html
@for (user of users; track user.id) {
  <app-user-card [user]="user" />
}
```

Stable tracking prevents unnecessary DOM recreation.

## trackBy / track

`trackBy` in `*ngFor` and `track` in modern `@for` help Angular identify list items with a stable key.

Old syntax:

```html
<li *ngFor="let user of users; trackBy: trackByUserId">
  {{ user.name }}
</li>
```

```ts
trackByUserId(index: number, user: User) {
  return user.id;
}
```

Modern syntax:

```html
@for (user of users; track user.id) {
  <app-user-card [user]="user" />
}
```

Use stable unique identifiers:

- `user.id`
- `item.key`
- database id
- slug or unique code

Avoid `track $index` for dynamic lists where items can be inserted, removed, or reordered.

Stable tracking allows Angular to reuse DOM nodes and update only changed items instead of recreating the whole list.

## Unidirectional Data Flow

Unidirectional data flow means data moves in one predictable direction.

In Angular:

- parent passes data down through inputs
- child sends events up through outputs
- state changes create a new render/update path

```html
<app-user-card
  [user]="selectedUser"
  (selected)="selectUser($event)"
/>
```

This makes change detection easier to reason about because data ownership is clearer.

Together, stable `track` and unidirectional data flow improve template performance:

- `track` reduces unnecessary DOM recreation
- unidirectional data flow reduces unpredictable side effects
- `OnPush` works better with immutable inputs and clear ownership

### Avoid functions in template

Bad:

```js
{{ getFullName(user) }}
```

This function may run many times during change detection.

Better:

```ts
fullName = computed(() => `${this.user().firstName} ${this.user().lastName}`);
```

```html
{{ fullName() }}
```

Or calculate data before binding.

### Avoid heavy pipes unless pure

Pure pipes are cached based on input references.

Impure pipes run often and can hurt performance.

### Keep state immutable

Prefer:

```ts
this.users = this.users.map(user =>
  user.id === id ? { ...user, active: true } : user
);
```

Avoid:

```ts
this.users[index].active = true;
```

## Common mistakes

- mutating `@Input()` objects with `OnPush`
- manual subscriptions without cleanup
- using functions directly in templates
- forgetting `track` in large lists
- calling `detectChanges()` too often
- using `Default` strategy everywhere in a large app
- running scroll/mousemove listeners inside Angular zone
- changing state from third-party callbacks without `markForCheck()` or signals

## Mental model

Do not think:

```text
Angular magically updates everything.
```

Think:

```text
What changed?
Who knows it changed?
Which component should be checked?
Can Angular skip the rest?
```

## Interview answer

Angular change detection synchronizes component state with the DOM. Angular checks template bindings and updates the DOM when values change. For scalable apps, senior developers use `OnPush`, immutable state, signals, `async` pipe, stable `@for track`, and avoid expensive template work. They understand when Angular marks views dirty, how `ChangeDetectorRef` works, and how zoneless Angular makes change detection more explicit.

## Related

- [[Zone.js]]
- [[Zoneless]]
- [[Signals]]
- [[Data Types]]
- [[Object]]
- [[Functions]]
- [[Immutability]]

## Official docs

- [Skipping component subtrees](https://angular.dev/best-practices/skipping-subtrees)
- [ChangeDetectorRef](https://angular.dev/api/core/ChangeDetectorRef)
- [Zoneless](https://angular.dev/guide/zoneless)
- [Signals](https://angular.dev/guide/signals)
