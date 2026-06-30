# Standalone Components

Standalone components are Angular components that do not need to be declared in an `NgModule`.

They can directly import the dependencies they need.

## Basic example

```ts
@Component({
  selector: 'app-user-card',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './user-card.component.html',
})
export class UserCardComponent {}
```

The important part is:

```ts
standalone: true
```

## What problem they solve

Before standalone components, components had to be declared inside an `NgModule`.

Old style:

```ts
@NgModule({
  declarations: [UserCardComponent],
  imports: [CommonModule],
  exports: [UserCardComponent],
})
export class UserModule {}
```

Standalone style:

```ts
@Component({
  standalone: true,
  imports: [CommonModule],
})
export class UserCardComponent {}
```

The component owns its own dependencies.

## imports

Standalone components use `imports` directly.

You can import:

- other standalone components
- standalone directives
- standalone pipes
- Angular modules like `CommonModule`
- router directives
- form modules

Example:

```ts
@Component({
  standalone: true,
  imports: [
    CommonModule,
    RouterLink,
    ReactiveFormsModule,
    UserAvatarComponent,
  ],
})
export class UserProfileComponent {}
```

## providers

A standalone component can also define providers.

```ts
@Component({
  standalone: true,
  providers: [UserCardState],
})
export class UserCardComponent {}
```

This creates a provider scope for that component and its children.

Use this when each component instance needs its own local service/state.

## Standalone app bootstrap

Standalone apps can start without `AppModule`.

```ts
bootstrapApplication(AppComponent, {
  providers: [
    provideRouter(routes),
    provideHttpClient(),
  ],
});
```

Instead of putting app configuration in `AppModule`, modern Angular often uses:

- `main.ts`
- `app.config.ts`
- `ApplicationConfig`
- provider functions like `provideRouter()` and `provideHttpClient()`

## Routing with standalone components

Standalone components work naturally with the router.

```ts
export const routes: Routes = [
  {
    path: 'users',
    loadComponent: () =>
      import('./user-page.component').then(m => m.UserPageComponent),
  },
];
```

`loadComponent` lazy-loads one standalone component.

## loadComponent vs loadChildren

`loadComponent`:

- lazy-loads a single standalone component
- good for simple route pages
- less boilerplate

`loadChildren`:

- lazy-loads a route tree
- good for larger feature areas
- can include child routes and route-level providers

Example:

```ts
{
  path: 'admin',
  loadChildren: () =>
    import('./admin.routes').then(m => m.adminRoutes),
}
```

## How standalone replaces NgModule

Standalone components do not remove all module concepts, but they remove the need for most feature/component `NgModule` declarations.

Old Angular architecture:

```text
Component -> declared in NgModule -> NgModule imports dependencies
```

Modern standalone architecture:

```text
Component -> imports dependencies directly
```

This makes dependencies easier to see and simplifies lazy loading.

## Benefits

- less boilerplate
- easier lazy loading
- clearer component dependencies
- simpler feature structure
- works well with functional providers
- better fit for modern Angular APIs

## Common mistakes

- forgetting to add required directives/pipes/components to `imports`
- importing too much into every component
- putting global services in component `providers` by accident
- mixing module-based and standalone architecture without clear rules
- using `loadComponent` for a large feature that should be a route tree

## Mental model

Ask:

```text
Is this a single page/component? -> loadComponent
Is this a feature area with child routes/providers? -> loadChildren
Should this service be local to the component or app-wide?
Are component dependencies visible in imports?
```

## Short answer

Standalone components are Angular components that declare their own dependencies using `imports` and do not need to be declared in an `NgModule`. They make Angular more component-focused, reduce boilerplate, and work well with modern APIs like `bootstrapApplication`, `provideRouter`, and `loadComponent`.

## Related

- [[What is Angular]]
- [[NgModule]]
- [[Dependency Injection]]
- [[Angular Router#Lazy Loading|Lazy Loading]]
- [[Angular Router#loadComponent vs loadChildren|loadComponent]]
- [[Angular Router#loadComponent vs loadChildren|loadChildren]]
- [[Angular Router]]
- [[What is Angular#App Startup|app.config.ts]]
