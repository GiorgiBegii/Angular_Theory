# SOLID in Angular

SOLID is a set of design principles that help Angular code stay clean, testable, and easy to change.

In Angular, SOLID mostly applies to:
- components
- services
- directives
- pipes
- guards
- interceptors
- facades
- state management

## S - Single Responsibility Principle

One class should have one main responsibility.

### Angular meaning

A component should not do everything.

Bad component responsibilities:
- render UI
- call API
- format data
- validate business rules
- manage complex state
- handle permissions

Better split:
- component - UI and user interaction
- service - API/data logic
- facade - page-level orchestration
- pipe - formatting
- validator - validation
- guard - route access logic

### Bad

```ts
export class UserPageComponent {
  users = [];

  constructor(private http: HttpClient) {}

  loadUsers() {
    this.http.get('/api/users').subscribe(users => {
      this.users = users as any[];
    });
  }

  formatUserName(user: any) {
    return `${user.firstName} ${user.lastName}`.toUpperCase();
  }

  canDelete(user: any) {
    return user.role !== 'admin';
  }
}
```

### Better

```ts
export class UserPageComponent {
  users$ = this.userFacade.users$;

  constructor(private userFacade: UserFacade) {}

  deleteUser(id: string) {
    this.userFacade.deleteUser(id);
  }
}
```

### Remember

Component = view logic.  
Service/facade = business and data logic.

### Patterns used

- [[Structural Patterns#Facade|Facade]] - component talks to one facade instead of many services.
- [[Structural Patterns#Adapter|Adapter]] - separates external/API shape from app/domain shape.
- [[Behavioral Patterns#Mediator|Mediator]] - shared service coordinates communication between components.
- [[Behavioral Patterns#Command|Command]] - UI triggers an action, but execution logic stays outside the component.

### Angular example

SRP is often protected by the **Facade pattern**:

```ts
export class UserPageComponent {
  users$ = this.userFacade.users$;

  constructor(private userFacade: UserFacade) {}
}
```

The component is responsible for UI.  
The facade is responsible for user-page logic.

## O - Open/Closed Principle

Code should be open for extension, but closed for constant modification.

### Angular meaning

When adding new behavior, avoid editing one large `if/switch` class again and again.

Use:
- interfaces
- injection tokens
- strategy services
- multi providers
- polymorphism

### Bad

```ts
getDiscount(type: string) {
  if (type === 'student') return 20;
  if (type === 'vip') return 30;
  if (type === 'employee') return 50;
  return 0;
}
```

Every new discount changes the same method.

### Better

```ts
export interface DiscountStrategy {
  type: string;
  getDiscount(): number;
}

@Injectable()
export class StudentDiscountStrategy implements DiscountStrategy {
  type = 'student';
  getDiscount() {
    return 20;
  }
}
```

Now a new discount can be added as a new class instead of modifying old logic.

### Remember

Add new behavior by adding new code, not rewriting stable code.

### Patterns used

- [[Behavioral Patterns#Strategy|Strategy]] - add new behavior as a new strategy class.
- [[Creational Patterns#Factory Method|Factory Method]] - choose which implementation to create/use.
- [[Creational Patterns#Abstract Factory|Abstract Factory]] - switch a whole family of related implementations.
- [[Structural Patterns#Decorator|Decorator]] - add behavior without changing the original class.
- [[Behavioral Patterns#Chain of Responsibility|Chain of Responsibility]] - add new handler/interceptor without rewriting old handlers.

### Angular example

HTTP interceptors follow OCP and Chain of Responsibility:

```ts
export const authInterceptor: HttpInterceptorFn = (req, next) => {
  const cloned = req.clone({
    setHeaders: { Authorization: 'Bearer token' },
  });

  return next(cloned);
};
```

To add logging, caching, or error handling, create another interceptor.  
Existing API services do not need to change.

## L - Liskov Substitution Principle

If code expects a base type or interface, every implementation should work without breaking expectations.

### Angular meaning

If several services implement the same interface, they must behave consistently.

Example:

```ts
export interface AuthStorage {
  getToken(): string | null;
  setToken(token: string): void;
  clearToken(): void;
}
```

Valid implementations:
- `LocalStorageAuthStorage`
- `SessionStorageAuthStorage`
- `MemoryAuthStorage`

All must respect the same behavior:
- `getToken()` returns token or `null`
- `setToken()` saves token
- `clearToken()` removes token

### Bad

```ts
export class BrokenAuthStorage implements AuthStorage {
  getToken() {
    throw new Error('Not supported');
  }
}
```

This breaks the contract.

### Remember

If it implements the interface, it must behave like the interface promises.

### Patterns used

- [[Behavioral Patterns#Strategy|Strategy]] - every strategy must respect the same contract.
- [[Structural Patterns#Adapter|Adapter]] - different external systems can be adapted to the same interface.
- [[Creational Patterns#Factory Method|Factory Method]] - factory can return different implementations safely only if they are substitutable.
- [[Structural Patterns#Composite|Composite]] - child controls should behave consistently through the same base API.

### Angular example

Reactive Forms use a Composite-like structure:

```ts
FormControl
FormGroup
FormArray
```

They all work through `AbstractControl`.

If code expects `AbstractControl`, each control type must follow the same basic rules:
- has value
- has validation state
- can be enabled/disabled
- can be marked touched/dirty

## I - Interface Segregation Principle

Do not force code to depend on methods it does not use.

### Angular meaning

Avoid huge services/interfaces that expose unrelated features.

Bad:

```ts
export interface UserService {
  getUsers(): Observable<User[]>;
  createUser(user: User): Observable<User>;
  exportReport(): void;
  sendEmail(): void;
  checkPermissions(): boolean;
}
```

Better:

```ts
export interface UserReader {
  getUsers(): Observable<User[]>;
}

export interface UserWriter {
  createUser(user: User): Observable<User>;
}

export interface UserPermissionChecker {
  checkPermissions(): boolean;
}
```

### Component example

A list component may need only reading:

```ts
constructor(private userReader: UserReader) {}
```

It should not depend on writing, reports, or email methods.

### Remember

Small focused APIs are easier to test, replace, and understand.

### Patterns used

- [[Structural Patterns#Facade|Facade]] - exposes only the operations a component needs.
- [[Structural Patterns#Adapter|Adapter]] - creates a smaller clean API over a large external API.
- [[Behavioral Patterns#Mediator|Mediator]] - components communicate through focused methods/events instead of knowing each other directly.
- [[Structural Patterns#Proxy|Proxy]] - controls access to a large object/service through a narrower interface.

### Angular example

Instead of injecting a huge service into every component:

```ts
constructor(private userService: UserService) {}
```

Create focused facades:

```ts
constructor(private userListFacade: UserListFacade) {}
```

The list page gets only list-related operations.  
It does not know about create, export, email, or admin actions.

## D - Dependency Inversion Principle

High-level code should depend on abstractions, not concrete implementations.

### Angular meaning

Components and services should not create dependencies with `new`.

Bad:

```ts
export class UserComponent {
  private api = new UserApiService();
}
```

Better:

```ts
export class UserComponent {
  constructor(private userApi: UserApiService) {}
}
```

Even better for replaceable implementations:

```ts
export const AUTH_STORAGE = new InjectionToken<AuthStorage>('AUTH_STORAGE');

export class AuthService {
  constructor(@Inject(AUTH_STORAGE) private storage: AuthStorage) {}
}
```

Provider:

```ts
providers: [
  {
    provide: AUTH_STORAGE,
    useClass: LocalStorageAuthStorage,
  },
]
```

Testing becomes easy:

```ts
providers: [
  {
    provide: AUTH_STORAGE,
    useClass: MemoryAuthStorage,
  },
]
```

### Remember

Angular DI is the main tool for Dependency Inversion.

### Patterns used

- [[Dependency Injection]] - Angular provides dependencies from outside.
- [[Creational Patterns#Factory Method|Factory Method]] - provider/factory decides which implementation to create.
- [[Creational Patterns#Abstract Factory|Abstract Factory]] - provide a group of related services depending on environment/config.
- [[Creational Patterns#Singleton|Singleton]] - Angular services with `providedIn: 'root'` usually behave as app-wide singletons.
- [[Structural Patterns#Proxy|Proxy]] - HTTP interceptors or wrapper services stand between caller and real implementation.

### Angular example

Angular provider factory:

```ts
export const API_URL = new InjectionToken<string>('API_URL');

providers: [
  {
    provide: API_URL,
    useFactory: () => environment.apiUrl,
  },
]
```

The app depends on the `API_URL` abstraction.  
The actual value comes from configuration.

## Quick Angular Summary

| Principle | Angular version | Helpful patterns |
| --- | --- | --- |
| SRP | Keep components, services, pipes, guards, and validators focused | Facade, Adapter, Mediator, Command |
| OCP | Add behavior with strategies, providers, and polymorphism | Strategy, Factory Method, Decorator, Chain of Responsibility |
| LSP | Implementations must respect the same interface contract | Strategy, Adapter, Factory Method, Composite |
| ISP | Prefer small focused services/interfaces | Facade, Adapter, Mediator, Proxy |
| DIP | Use Angular DI and InjectionToken instead of hard-coded dependencies | Dependency Injection, Factory Method, Abstract Factory, Singleton, Proxy |

## Related

- [[What is OOP]]
- [[Dependency Injection]]
- [[Creational Patterns]]
- [[Structural Patterns]]
- [[Behavioral Patterns]]

