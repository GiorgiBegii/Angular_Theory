# Lifecycle Hooks

Angular lifecycle hooks are methods that Angular calls at specific moments in a component or directive lifetime.

They help you run code when:

- inputs change
- component initializes
- Angular runs change detection
- projected content is ready
- component view is ready
- component is destroyed

## Hook Order

Common component lifecycle order:

1. `constructor`
2. `ngOnChanges`
3. `ngOnInit`
4. `ngDoCheck`
5. `ngAfterContentInit`
6. `ngAfterContentChecked`
7. `ngAfterViewInit`
8. `ngAfterViewChecked`
9. `ngOnDestroy`

## constructor

`constructor` is a TypeScript class feature, not an Angular lifecycle hook.

Use it for dependency injection and simple class setup.

```ts
constructor(private userService: UserService) {}
```

Do not put initialization logic that depends on Angular inputs or the template here.

## ngOnChanges

Runs when an input-bound property changes.

It runs before `ngOnInit` on the first input change.

```ts
ngOnChanges(changes: SimpleChanges) {
  console.log(changes['userId']);
}
```

Use it when logic depends on `@Input()` / `input()` changes.

## ngOnInit

Runs once after Angular initializes input-bound properties.

Use it for initialization logic:

- load initial data
- setup streams
- initialize component state
- call services based on inputs

```ts
ngOnInit() {
  this.loadUser();
}
```

Do not use it for DOM access that depends on the rendered view. Use `ngAfterViewInit` for that.

## ngDoCheck

Runs during every change detection cycle.

Use it for custom change detection logic when Angular's default input checking is not enough.

Be careful: it can run very often, so expensive logic here can hurt performance.

## ngAfterContentInit

Runs once after projected content is initialized.

Projected content means content passed through `<ng-content>`.

Use it when you need to work with content children after projection is ready.

## ngAfterContentChecked

Runs after Angular checks projected content.

It can run many times, so avoid heavy logic here.

Use it only for advanced cases related to projected content checking.

## ngAfterViewInit

Runs once after the component's view and child views are initialized.

Use it when you need access to template elements or child components.

```ts
@ViewChild('input') input!: ElementRef<HTMLInputElement>;

ngAfterViewInit() {
  this.input.nativeElement.focus();
}
```

Good for:

- `ViewChild`
- DOM-dependent logic
- third-party UI libraries

## ngAfterViewChecked

Runs after Angular checks the component view and child views.

It can run very often.

Avoid changing component state here because it can cause change detection errors or loops.

## ngOnDestroy

Runs before Angular destroys the component or directive.

Use it for cleanup:

- unsubscribe manually created subscriptions
- remove event listeners
- clear timers
- destroy third-party instances

```ts
ngOnDestroy() {
  this.subscription.unsubscribe();
}
```

In modern Angular, prefer `takeUntilDestroyed()` when possible.

## Modern Angular Notes

With signals and modern APIs, some lifecycle usage can be reduced.

Examples:

- use `effect()` for reactive side effects
- use `takeUntilDestroyed()` for subscription cleanup
- use `afterNextRender()` / `afterEveryRender()` for render-related work

Still, lifecycle hooks remain important for understanding Angular component behavior.

## Short Answer

Angular lifecycle hooks are methods called by Angular during component creation, input changes, change detection, view/content initialization, and destruction. `ngOnInit` runs once after Angular sets input-bound properties and is used for initialization logic such as loading data or setting up streams. DOM-dependent logic belongs in `ngAfterViewInit`, and cleanup belongs in `ngOnDestroy`.

## Related Notes

- [[Components]]
- [[Change Detection]]
- [[Dependency Injection]]
