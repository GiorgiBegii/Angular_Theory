# Prototypes

JavaScript uses prototypes for inheritance.

Every object can have another object as its prototype. If JavaScript cannot find a property or method on the object itself, it looks up the prototype chain.

## Prototype Chain

```js
const user = {
  name: "Ana"
};

console.log(user.toString());
```

`toString` is not defined directly on `user`, so JavaScript looks up the prototype chain and finds it on `Object.prototype`.

Lookup order:

1. object itself
2. object's prototype
3. next prototype
4. `Object.prototype`
5. `null`

## prototype vs __proto__

`prototype` belongs to functions and classes.

It is used when creating objects with `new`.

```js
function User(name) {
  this.name = name;
}

User.prototype.sayName = function () {
  console.log(this.name);
};

const user = new User("Ana");
user.sayName(); // "Ana"
```

`__proto__` is the internal prototype link of an object.

```js
user.__proto__ === User.prototype; // true
```

Prefer `Object.getPrototypeOf()` instead of using `__proto__` directly.

```js
Object.getPrototypeOf(user) === User.prototype; // true
```

## Prototypal Inheritance

Prototypal inheritance means objects inherit from other objects.

```js
const animal = {
  eat() {
    console.log("Eating");
  }
};

const dog = Object.create(animal);

dog.bark = function () {
  console.log("Barking");
};

dog.eat(); // "Eating"
```

`dog` does not have `eat`, so JavaScript finds it on `animal`.

## Constructor Functions

Before `class`, constructor functions were commonly used for object creation.

```js
function User(name) {
  this.name = name;
}

User.prototype.sayName = function () {
  console.log(this.name);
};
```

Methods are placed on `User.prototype` so all created objects share the same method instead of copying it into every object.

## Classes Use Prototypes

JavaScript classes do not create a new inheritance model.

They are cleaner syntax over prototypes.

```js
class User {
  constructor(name) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }
}
```

This method is stored on `User.prototype`:

```js
User.prototype.sayName;
```

So `class` is easier syntax, but the runtime model is still prototype-based.

## extends

`extends` sets up the prototype chain between classes.

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

const dog = new Dog();

dog.eat(); // from Animal.prototype
```

`Dog.prototype` inherits from `Animal.prototype`.

## Angular Context

Angular services, components, directives, and pipes are TypeScript classes.

```ts
@Injectable()
export class UserService {
  getUser() {}
}
```

Even though we write `class`, JavaScript still uses prototypes under the hood.

This matters for understanding:

- class methods
- inheritance
- `extends`
- `this`
- method sharing
- how JavaScript objects resolve methods

## Short Answer

JavaScript uses prototypal inheritance. Objects can inherit properties and methods through the prototype chain. `class` and `extends` are modern syntax over this prototype system: class methods are stored on `.prototype`, and `extends` links child and parent prototypes.

## Related Notes

- [[Classes]]
- [[Object]]
- [[Functions#Constructor Functions|Constructor Functions]]
- [[Language Concepts#this|this]]
- [[What is OOP]]
