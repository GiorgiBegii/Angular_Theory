# What is OOP

OOP means Object-Oriented Programming.

It is a programming style where code is organized around objects.

An object contains:

- data as properties
- behavior as methods

```ts
class User {
  constructor(public name: string) {}

  sayHello() {
    return `Hello ${this.name}`;
  }
}
```

The goal of OOP is to make code easier to understand, reuse, maintain, and extend.

## Four Main OOP Principles

Quick reminder:

1. [[What is OOP#Encapsulation|Encapsulation]] - hide internal details and expose only what is needed.
2. [[What is OOP#Inheritance|Inheritance]] - create new classes based on existing ones.
3. [[What is OOP#Polymorphism|Polymorphism]] - use different implementations through the same interface.
4. [[What is OOP#Abstraction|Abstraction]] - focus on what an object does, not how it does it.

## Encapsulation

Encapsulation means hiding internal details and exposing only what is needed.

The object controls how its data can be accessed or changed.

```ts
class BankAccount {
  private balance = 0;

  deposit(amount: number) {
    if (amount <= 0) {
      return;
    }

    this.balance += amount;
  }

  getBalance() {
    return this.balance;
  }
}
```

`balance` is hidden. Other code must use methods like `deposit()` and `getBalance()`.

Angular example:

```ts
@Injectable({ providedIn: 'root' })
export class AuthService {
  private token: string | null = null;

  login(token: string) {
    this.token = token;
  }

  isLoggedIn() {
    return !!this.token;
  }
}
```

Components do not need to know how token logic works internally.

## Inheritance

Inheritance means creating a new class based on an existing class.

The child class can reuse or extend parent behavior.

```ts
class Animal {
  eat() {
    console.log("Eating");
  }
}

class Dog extends Animal {
  bark() {
    console.log("Barking");
  }
}
```

`Dog` can use both `bark()` and `eat()`.

In JavaScript, inheritance is based on [[Prototypes]].

In Angular, inheritance can be useful for shared component logic, but composition is often preferred for flexibility.

## Polymorphism

Polymorphism means using different implementations through the same shape or interface.

```ts
interface PaymentStrategy {
  pay(amount: number): void;
}

class CardPayment implements PaymentStrategy {
  pay(amount: number) {
    console.log(`Pay by card: ${amount}`);
  }
}

class PaypalPayment implements PaymentStrategy {
  pay(amount: number) {
    console.log(`Pay by PayPal: ${amount}`);
  }
}
```

Both classes can be used through the same `PaymentStrategy` interface.

Angular example:

```ts
providers: [
  { provide: PaymentStrategy, useClass: CardPayment }
]
```

The component depends on the abstraction, not the concrete implementation.

## Abstraction

Abstraction means focusing on what something does, not how it does it.

```ts
interface UserRepository {
  getUser(id: string): Observable<User>;
}
```

The caller knows it can get a user, but it does not need to know whether the data comes from HTTP, cache, IndexedDB, or mock data.

Angular example:

```ts
@Injectable()
export class HttpUserRepository implements UserRepository {
  getUser(id: string) {
    return this.http.get<User>(`/api/users/${id}`);
  }
}
```

Abstraction keeps code easier to replace and test.

## OOP in Angular

Angular uses OOP concepts through:

- classes for components and services
- dependency injection
- interfaces and abstract classes
- encapsulated services
- inheritance when needed
- polymorphism through providers

Common Angular examples:

- component class = state + behavior for a view
- service class = reusable business logic
- directive class = reusable DOM behavior
- pipe class = reusable transformation

## Short Answer

OOP organizes code around objects that combine data and behavior. Its main principles are encapsulation, inheritance, polymorphism, and abstraction. In Angular, OOP appears in components, services, directives, pipes, dependency injection, and provider-based abstractions.

## Related Notes

- [[SOLID]]
- [[Classes]]
- [[Prototypes]]
- [[Dependency Injection]]
