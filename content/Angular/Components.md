# Components

A component is the main building block of an Angular UI.

It connects:

- TypeScript class
- HTML template
- styles
- metadata
- inputs/outputs
- change detection

## Basic example

```ts
@Component({
  selector: 'app-user-card',
  standalone: true,
  imports: [],
  templateUrl: './user-card.component.html',
  styleUrl: './user-card.component.scss',
})
export class UserCardComponent {}
```

## Component parts

### selector

`selector` defines how the component is used in another template.

```ts
selector: 'app-user-card'
```

Usage:

```html
<app-user-card />
```

## template

The template defines the UI.

```html
<h2>{{ user.name }}</h2>
```

Templates can use:

- interpolation
- property binding
- event binding
- control flow like `@if` and `@for`
- pipes
- child components

## styles

Styles define the component appearance.

```ts
styleUrl: './user-card.component.scss'
```

Angular normally scopes component styles to that component.

## Component class

The class stores state and behavior used by the template.

```ts
export class UserCardComponent {
  isSelected = false;

  select() {
    this.isSelected = true;
  }
}
```

Keep component class focused on UI logic.

Business/data logic usually belongs in:

- services
- facades
- stores
- validators
- pipes

## Inputs

Inputs pass data from parent to child.

```ts
user = input.required<User>();
```

Template:

```html
<app-user-card [user]="user" />
```

Older style:

```ts
@Input() user!: User;
```

Use inputs for data the parent owns.

## Outputs

Outputs send events from child to parent.

Modern style:

```ts
delete = output<string>();

onDelete() {
  this.delete.emit(this.user().id);
}
```

Template:

```html
<app-user-card (delete)="deleteUser($event)" />
```

Older style:

```ts
@Output() delete = new EventEmitter<string>();
```

Use outputs for user actions or child-to-parent communication.

## Component communication

Common communication patterns:

- parent -> child: inputs
- child -> parent: outputs
- sibling/shared state: service, facade, store, signals, RxJS
- route/page data: router, resolver, service

Avoid direct tight coupling between unrelated components.

## Standalone components

Modern Angular components are usually standalone.

```ts
@Component({
  standalone: true,
  imports: [UserAvatarComponent, DatePipe],
})
export class UserCardComponent {}
```

Standalone components import their dependencies directly.

This makes dependencies easier to see.

## Providers in components

A component can provide services locally.

```ts
@Component({
  providers: [UserCardState],
})
export class UserCardComponent {}
```

This creates a new service instance for this component subtree.

Use component providers for local component state.

Do not put app-wide singletons in component providers by accident.

## Lifecycle

Common lifecycle hooks:

```text
constructor
ngOnChanges
ngOnInit
ngAfterContentInit
ngAfterViewInit
ngOnDestroy
```

Use:

- `constructor` for dependency setup only
- `ngOnInit` for initialization
- `ngOnChanges` for input changes
- `ngAfterViewInit` for view child access
- `ngOnDestroy` for cleanup

## Change detection

Components participate in Angular [[Change Detection]].

For performance, use `OnPush` for presentational components:

```ts
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush,
})
export class UserCardComponent {}
```

With `OnPush`, prefer:

- [[Immutability|immutable inputs]]
- signals
- `async` pipe
- stable `@for track`
- no heavy template functions

## Smart vs presentational components

### Smart/page component

Responsible for:

- loading data
- connecting to services/facades
- route params
- page state
- orchestration

### Presentational component

Responsible for:

- displaying data
- emitting user actions
- reusable UI
- minimal business logic

Example:

```text
UserPageComponent -> smart
UserCardComponent -> presentational
```

## Common mistakes

- putting API calls directly in many small components
- making one component handle too many responsibilities
- mutating input objects
- using functions in templates for expensive calculations
- forgetting to clean subscriptions/listeners
- using component providers when root service is needed
- making child components depend directly on parent implementation
- not using `track` in large lists

## Mental model

Ask:

```text
Who owns this state?
Is this component smart or presentational?
Should data come through input, service, route, or store?
Should this dependency be local or app-wide?
Can this component be tested in isolation?
Can Angular skip checking it with OnPush?
```

## Short answer

An Angular component is a UI building block made of a class, template, styles, and metadata. It receives data through inputs, sends events through outputs, participates in change detection, and can import dependencies directly when standalone. Good components are focused, reusable, testable, and keep business/data logic outside the template-heavy UI layer.

## Related

- [[Standalone Components]]
- [[Binding#Template Binding|Template Binding]]
- [[Change Detection]]
- [[Dependency Injection]]
- [[Change Detection#OnPush strategy|OnPush]]
- [[Lifecycle Hooks]]
- [[Signals]]
