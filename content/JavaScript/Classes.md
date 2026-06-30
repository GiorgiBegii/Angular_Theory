# Classes

Classes are modern JavaScript syntax for creating objects with shared behavior.

They make constructor functions and prototypes easier to read and write.

## Basic Class

```js
class User {
  constructor(name) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }
}

const user = new User("Ana");
user.sayName(); // "Ana"
```

`constructor` runs when a new instance is created.

Methods are shared through the prototype:

```js
User.prototype.sayName;
```

## Classes Use Prototypes

A class is mostly syntax over JavaScript's prototype system.

This:

```js
class User {
  sayName() {}
}
```

is conceptually similar to:

```js
function User() {}

User.prototype.sayName = function () {};
```

So classes are not a separate inheritance model. They still use [[Prototypes]].

## extends

`extends` creates inheritance between classes.

```js
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

`Dog` inherits methods from `Animal`.

Under the hood:

- `Dog.prototype` links to `Animal.prototype`
- instances of `Dog` can use methods from both `Dog.prototype` and `Animal.prototype`

## super

`super` calls the parent class constructor or method.

```js
class User {
  constructor(name) {
    this.name = name;
  }
}

class Admin extends User {
  constructor(name, role) {
    super(name);
    this.role = role;
  }
}
```

Use `super()` before accessing `this` in a child constructor.

## Static Members

Static methods belong to the class itself, not to instances.

```js
class MathUtil {
  static sum(a, b) {
    return a + b;
  }
}

MathUtil.sum(2, 3); // 5
```

## Angular Context

Angular uses classes for:

- components
- services
- directives
- pipes
- guards
- interceptors

```ts
@Component({
  selector: 'app-user',
  templateUrl: './user.component.html'
})
export class UserComponent {}
```

In Angular, classes help organize state and behavior, while decorators add Angular metadata.

## Short Answer

Classes are modern syntax for creating objects and sharing behavior. They use JavaScript prototypes under the hood. Class methods are placed on `.prototype`, and `extends` connects child and parent prototypes. In Angular, components and services are written as classes, but they still follow JavaScript's prototype-based object model.

## Related Notes

- [[Prototypes]]
- [[Functions#Constructor Functions|Constructor Functions]]
- [[Language Concepts#this|this]]
- [[What is OOP]]
