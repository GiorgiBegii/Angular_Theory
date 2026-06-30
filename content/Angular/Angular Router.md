# Angular Router

Angular Router manages navigation in single-page Angular applications.

It lets the browser URL decide which component or feature should be displayed.

## Main Capabilities

Angular Router provides:

- client-side navigation
- route configuration
- nested routes
- parameterized routes
- redirects
- lazy loading
- route guards
- resolvers
- route-level providers
- navigation state
- active link styling
- preloading strategies

## Route Configuration

Routes are usually defined as an array of `Routes`.

```ts
export const routes: Routes = [
  {
    path: '',
    component: HomePage,
  },
  {
    path: 'users/:id',
    component: UserDetailsPage,
  },
  {
    path: 'admin',
    loadComponent: () =>
      import('./admin/admin.page').then(m => m.AdminPage),
  },
  {
    path: '**',
    redirectTo: '',
  },
];
```

Each route maps a URL path to something Angular should render or load.

Common route properties:

- `path`
- `component`
- `loadComponent`
- `loadChildren`
- `redirectTo`
- `children`
- `canActivate`
- `canMatch`
- `resolve`
- `providers`

## Registering Routes

In standalone Angular, routes are usually registered in `app.config.ts`:

```ts
export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(routes),
  ],
};
```

In older NgModule-based Angular, routes are usually registered with:

```ts
RouterModule.forRoot(routes)
```

Feature modules often used:

```ts
RouterModule.forChild(routes)
```

## Router Outlet

`router-outlet` marks where routed components should render.

```html
<router-outlet />
```

When the URL changes, Angular Router matches the route and displays the matching component inside `router-outlet`.

## Router Links

Use `routerLink` for navigation in templates.

```html
<a routerLink="/users">Users</a>
```

Active route styling:

```html
<a routerLink="/users" routerLinkActive="active">Users</a>
```

## Route Params

Parameterized route:

```ts
{
  path: 'users/:id',
  component: UserDetailsPage,
}
```

Read parameter:

```ts
const id = this.route.snapshot.paramMap.get('id');
```

or reactively:

```ts
this.route.paramMap.subscribe(params => {
  const id = params.get('id');
});
```

## Lazy Loading

Lazy loading loads code only when the route is visited.

Standalone component lazy loading:

```ts
{
  path: 'settings',
  loadComponent: () =>
    import('./settings/settings.page').then(m => m.SettingsPage),
}
```

Route config lazy loading:

```ts
{
  path: 'admin',
  loadChildren: () =>
    import('./admin/admin.routes').then(m => m.ADMIN_ROUTES),
}
```

Lazy loading improves initial bundle size and startup performance.

## loadComponent vs loadChildren

`loadComponent` lazy-loads a single standalone component.

Use it when one route maps directly to one standalone page/component.

```ts
{
  path: 'profile',
  loadComponent: () =>
    import('./profile/profile.page').then(m => m.ProfilePage),
}
```

`loadChildren` lazy-loads a route configuration or an NgModule.

Use it when a feature has:

- child routes
- multiple pages
- route-level providers
- feature-level organization

```ts
{
  path: 'admin',
  loadChildren: () =>
    import('./admin/admin.routes').then(m => m.ADMIN_ROUTES),
}
```

Older module-based Angular often used `loadChildren` to load an NgModule:

```ts
{
  path: 'admin',
  loadChildren: () =>
    import('./admin/admin.module').then(m => m.AdminModule),
}
```

Short rule:

- single standalone page -> `loadComponent`
- feature area / route tree -> `loadChildren`

## Guards

Guards decide whether navigation is allowed.

Common guard types:

- `canActivate` - checks if a route can be entered.
- `canActivateChild` - checks if child routes can be entered.
- `canDeactivate` - checks if the user can leave the current route.
- `canMatch` - checks if a route is allowed to match, often used for auth or feature flags.

Example:

```ts
{
  path: 'admin',
  canActivate: [authGuard],
  loadComponent: () =>
    import('./admin/admin.page').then(m => m.AdminPage),
}
```

Common use cases:

- authentication
- authorization
- feature flags
- preventing unsaved form navigation
- blocking unavailable routes

In modern Angular, guards are often written as functions:

```ts
export const authGuard: CanActivateFn = () => {
  const authService = inject(AuthService);
  return authService.isLoggedIn();
};
```

## Resolvers

Resolvers load required data before activating a route.

```ts
{
  path: 'users/:id',
  component: UserDetailsPage,
  resolve: {
    user: userResolver,
  },
}
```

Use resolvers when the page should not render until required data is available.

Good use cases:

- critical page data
- configuration needed before render
- SEO/SSR-friendly data loading
- avoiding partially loaded screens

Avoid resolvers when:

- data is optional
- loading state is acceptable
- request can happen inside component state logic

Resolver example:

```ts
export const userResolver: ResolveFn<User> = route => {
  const userService = inject(UserService);
  return userService.getUser(route.paramMap.get('id')!);
};
```

## Route-Level Providers

Routes can provide services only for a specific route or feature.

```ts
{
  path: 'admin',
  providers: [AdminFacade],
  loadComponent: () =>
    import('./admin/admin.page').then(m => m.AdminPage),
}
```

This is useful for feature-scoped state and feature-specific services.

## Short Answer

Angular Router provides client-side navigation, nested routes, route parameters, redirects, lazy loading, guards, resolvers, route-level providers, and navigation state. Route configuration is an array of `Routes`, where each route maps a URL path to a component, lazy-loaded component, child route config, redirect, guard, or resolver. Modern standalone Angular registers routes with `provideRouter(routes)`, while older NgModule-based apps use `RouterModule.forRoot(routes)`.

## Related Notes

- [[What is Angular]]
- [[Dependency Injection#Provider Scope|Provider Scope]]
- [[Change Detection]]
