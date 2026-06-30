# Dependency Injection

Dependency Injection means a class receives the objects or values it needs from outside instead of creating them manually.

Simple idea:

```text
Bad: class creates dependency with new
Good: Angular provides dependency
```

## Why Angular uses DI

Angular uses DI to connect:

- components
- directives
- pipes
- services
- guards
- interceptors
- validators
- configuration values
- feature providers

DI makes Angular code easier to replace, test, configure, and split into features.

## Dependency

A dependency is anything a class needs to work.

Examples:

- `HttpClient`
- `Router`
- `AuthService`
- `UserService`
- API URL string
- feature flag
- logger
- storage adapter

## Without DI

```ts
export class UserComponent {
  private userService = new UserService();
}
```

Problem:

- hard to test
- hard to replace
- class controls object creation
- tight coupling

## With DI

```ts
export class UserComponent {
  constructor(private userService: UserService) {}
}
```

Angular creates or finds `UserService` and gives it to the component.

## Injectable service

```ts
@Injectable({
  providedIn: 'root',
})
export class UserService {
  constructor(private http: HttpClient) {}
}
```

`providedIn: 'root'` means Angular provides one app-level instance.

This is the most common service setup.

## inject() function

Modern Angular also supports `inject()`.

```ts
@Injectable({
  providedIn: 'root',
})
export class UserService {
  private http = inject(HttpClient);
}
```

Component example:

```ts
export class UserPageComponent {
  private userService = inject(UserService);
}
```

Guard example:

```ts
export const authGuard: CanActivateFn = () => {
  const auth = inject(AuthService);
  return auth.isLoggedIn();
};
```

### When to use inject()

Use `inject()` for:

- private dependencies
- functional guards/resolvers/interceptors
- field initializers
- avoiding very large constructors
- inheritance cases where constructor forwarding becomes noisy
- helper functions that run inside injection context

Important TypeScript detail:

```ts
private userService = inject(UserService); // correct
// wrong idea: class fields do not use const
// private const userService = inject(UserService);
```

Class fields do not use `const`.

## Injection context

`inject()` works only inside an Angular injection context.

Valid places:

- class field initializer
- constructor
- service creation
- component/directive creation
- functional guard
- functional resolver
- functional interceptor
- provider factory

Invalid:

```ts
ngOnInit() {
  const service = inject(UserService); // wrong
}
```

## Provider

A provider tells Angular how to create or return a dependency.

Common provider forms:

- `useClass`
- `useValue`
- `useFactory`
- `useExisting`

## useClass

Use a class implementation.

```ts
providers: [
  {
    provide: AuthStorage,
    useClass: LocalStorageAuthStorage,
  },
]
```

Angular creates `LocalStorageAuthStorage` when `AuthStorage` is requested.

## useValue

Use a fixed value.

```ts
providers: [
  {
    provide: API_URL,
    useValue: 'https://api.example.com',
  },
]
```

Good for:

- config
- constants
- mocks in tests

## useFactory

Use a function to create the value.

```ts
providers: [
  {
    provide: API_URL,
    useFactory: () => environment.apiUrl,
  },
]
```

Good when creation depends on runtime/config logic.

## useExisting

Use an existing provider as an alias.

```ts
providers: [
  LoggerService,
  {
    provide: AnalyticsLogger,
    useExisting: LoggerService,
  },
]
```

Both tokens point to the same instance.

## InjectionToken

Use `InjectionToken` when dependency is not a class.

Examples:

- string
- object config
- function
- interface-like abstraction

```ts
export const API_URL = new InjectionToken<string>('API_URL');
```

Use it:

```ts
export class ApiService {
  constructor(@Inject(API_URL) private apiUrl: string) {}
}
```

Provide it:

```ts
providers: [
  {
    provide: API_URL,
    useValue: 'https://api.example.com',
  },
]
```

## Interface problem

TypeScript interfaces disappear at runtime.

This does not work:

```ts
constructor(private storage: AuthStorage) {}
```

if `AuthStorage` is only an interface.

Use `InjectionToken`:

```ts
export interface AuthStorage {
  getToken(): string | null;
}

export const AUTH_STORAGE = new InjectionToken<AuthStorage>('AUTH_STORAGE');
```

Then inject:

```ts
constructor(@Inject(AUTH_STORAGE) private storage: AuthStorage) {}
```

## Injector Hierarchy

Angular DI is hierarchical.

Main injector levels:

```text
Platform injector
Root EnvironmentInjector
Route/feature EnvironmentInjector
ElementInjector for components/directives
NullInjector
```

Angular looks for a provider starting from the current place and moves upward.

## providedIn: root

```ts
@Injectable({
  providedIn: 'root',
})
export class AuthService {}
```

Meaning:

- one root-level instance
- available app-wide
- tree-shakable if unused
- good default for most services

## Component providers

```ts
@Component({
  selector: 'app-user-card',
  providers: [UserCardState],
})
export class UserCardComponent {}
```

This creates a new service instance for that component subtree.

Use when each component instance needs its own local state.

## providers vs viewProviders

`providers` makes dependency available to:

- component
- view children
- projected content

`viewProviders` makes dependency available to:

- component
- view children only

Projected content cannot access `viewProviders`.

## Route-level providers

Modern Angular routes can define providers:

```ts
export const routes: Routes = [
  {
    path: 'admin',
    providers: [AdminFacade],
    loadComponent: () => import('./admin.page').then(m => m.AdminPage),
  },
];
```

This scopes the service to that route/feature.

Good for:

- lazy features
- feature-specific facades
- feature config
- separating admin/user areas

## Provider Scope

Angular service is not always globally singleton.

It depends where it is provided:

| Provider location | Instance behavior |
| --- | --- |
| `providedIn: 'root'` | one app-level instance |
| component `providers` | new instance per component subtree |
| route `providers` | instance scoped to route/feature |
| test providers | instance from TestBed |

Provider scope controls:

- how long a service instance lives
- who can access that instance
- whether state is shared globally or isolated locally

Short idea:

- root provider = shared app-wide state
- route provider = feature-specific state
- component provider = local component subtree state

## Optional dependency

Use optional injection when dependency may not exist.

```ts
constructor(@Optional() private logger: LoggerService | null) {}
```

With `inject()`:

```ts
const logger = inject(LoggerService, { optional: true });
```

If provider is missing, Angular returns `null` instead of throwing.

## Resolution modifiers

Angular can control where DI searches.

Common modifiers:

- `@Self()` - only current injector
- `@SkipSelf()` - start search from parent injector
- `@Host()` - stop at host boundary
- `@Optional()` - return `null` if not found

Example:

```ts
constructor(@Self() private control: NgControl) {}
```

## Multi providers

Multi providers allow many values for one token.

Useful for plugin-like extension points.

```ts
export const VALIDATORS = new InjectionToken<ValidatorFn[]>('VALIDATORS');

providers: [
  {
    provide: VALIDATORS,
    useValue: requiredValidator,
    multi: true,
  },
  {
    provide: VALIDATORS,
    useValue: emailValidator,
    multi: true,
  },
]
```

Injected value becomes an array.

Angular uses this idea in places like interceptors.

## DI and SOLID

DI helps with:

- Dependency Inversion Principle
- Open/Closed Principle
- Single Responsibility Principle
- testing with mocks/fakes
- replacing implementations

Example:

```ts
export const AUTH_STORAGE = new InjectionToken<AuthStorage>('AUTH_STORAGE');
```

Production:

```ts
providers: [{ provide: AUTH_STORAGE, useClass: LocalStorageAuthStorage }]
```

Test:

```ts
providers: [{ provide: AUTH_STORAGE, useClass: MemoryAuthStorage }]
```

Same `AuthService`, different dependency.

## Testing DI

```ts
TestBed.configureTestingModule({
  providers: [
    UserService,
    {
      provide: HttpClient,
      useValue: httpClientMock,
    },
  ],
});
```

Or override a provider:

```ts
TestBed.overrideProvider(API_URL, {
  useValue: 'http://test-api',
});
```

## Common mistakes

- creating services manually with `new`
- putting too much state in root services
- using `providedIn: 'root'` for feature-local state
- forgetting that component providers create new instances
- trying to inject TypeScript interfaces directly
- using `inject()` outside injection context
- duplicating providers and accidentally creating multiple instances
- using component `providers` when app-wide singleton is needed
- using root singleton when route/component scoped service is better

## Mental model

Ask:

```text
Who owns this dependency?
How long should this instance live?
Should it be app-wide, route-scoped, or component-scoped?
Should consumers know the implementation or only the abstraction?
How will I replace it in tests?
```

## Quick summary

| Concept | Meaning |
| --- | --- |
| Dependency | object/value a class needs |
| Provider | recipe for creating/returning dependency |
| Token | key used to find dependency |
| Injector | container that stores providers/instances |
| `providedIn: 'root'` | app-wide tree-shakable provider |
| `InjectionToken` | DI token for non-class values or abstractions |
| `useClass` | provide class implementation |
| `useValue` | provide fixed value |
| `useFactory` | provide value from factory function |
| `useExisting` | alias existing provider |
| multi provider | collect many providers into an array |

## Interview answer

Dependency Injection in Angular is a system where classes ask Angular for dependencies instead of creating them manually. Angular resolves dependencies through injector hierarchies using tokens and providers. Senior Angular developers understand provider scope, `providedIn: 'root'`, route/component providers, `InjectionToken`, `inject()`, multi providers, and how DI supports testing and SOLID design.

## Related

- [[SOLID]]
- [[Creational Patterns]]
- [[Structural Patterns]]
- [[Behavioral Patterns]]
- [[Change Detection]]

## Official docs

- [Dependency injection overview](https://angular.dev/guide/di)
- [Hierarchical injectors](https://angular.dev/guide/di/hierarchical-dependency-injection)
- [InjectionToken](https://angular.dev/api/core/InjectionToken)
