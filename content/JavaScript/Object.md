An object is the **most important data structure in JavaScript**. Almost everything in JavaScript is an object or behaves like one.

# 1. What is an Object?

An object is a collection of **key-value pairs**.
```
const user = {    
	name: "John",    
	age: 25
};
```
Keys are called **properties**.

Values can be anything.
```
{    
	string    
	number    
	boolean    
	array    
	object    
	function
}
```

# 2. How Objects Are Stored

Objects are stored in the **Heap**.
```js
const user = {    
	name: "John"
};
```

Variable contains only a reference.
```
Stack
user 

â”‚ 
â–¼

Heap
{   
	name: "John"
}
```

# 3. Object Creation

**Object literal**
```js
const user = {};
```

**Object constructor**
```js
const user = new Object();
```

**Constructor function**
```js
function User(name) {    
	this.name = name;
}

const user = new User("John");
```

**Class**
```js
class User {    
	constructor(name) {        
		this.name = name;    
	}
}
```

**Object.create()**
```
const admin = Object.create(user);
```
Creates an object with a specified prototype.

# 4. Properties

Each property has:
- key
- value
- attributes

Property attributes:
- value
- writable
- enumerable
- configurable

Example:
```js
Object.defineProperty(user, "id", {    
	value: 1,    
	writable: false,    
	enumerable: false,    
	configurable: false
});
```

# 5. this

The most important rule is:

> **`this` is NOT determined by where a function is written.**
> 
> **`this` is determined by how the function is called**

### Rule 1. Method call

When a function is called as a property of an object:
```js
object.method();
```

`this` is that object.
```js
const user = {  
	name: "John",  
	sayHello() {    
		console.log(this);    
		console.log(this.name);  
	}
};

user.sayHello();
```

Output
```js
{  
	name: "John",  
	sayHello: Æ’
}
	
John
```

Here:
```js
user.sayHello();
^^^^
```

Because `user` called the function:
```js
this === user
```

Think of it like:
```
user.sayHello()
â†“
Who called sayHello?
â†“
user
â†“
this = user
```

---

### Rule 2. Different object calls the same function

```js
function sayHello() {    
	console.log(this.name);
}

const user1 = {    
	name: "John",    
	sayHello
};

const user2 = {    
	name: "Alice",    
	sayHello
};

user1.sayHello();
user2.sayHello();
```

Output
```js
John
Alice
```

Exactly the same function.

Different caller.

```
user1.sayHello()
â†“
this = user1
```

```
user2.sayHello()
â†“
this = user2
```

---

### Rule 3. Regular function call

```js
function greet() {    
	console.log(this);
}
greet();
```

In [[Modules and Application Structure#Strict Mode|Strict Mode]]:
```js
undefined
```

Without strict mode (browser):
```js
window
```

No object called the function.

So there is no object for `this`.

---

### Rule 4. Save method into variable

```js
const user = {    
	name: "John",    
	sayHello() {        
		console.log(this.name);    
	}
};

const fn = user.sayHello;fn();
```

Output
```js
undefined
```

Why?

Originally
```
user.sayHello()
â†“
this = user
```

After assigning
```js
const fn = user.sayHello;
```

You call
```js
fn();
```

Now nobody owns the function.
```js
fn()
â†“
no object
â†“
this = undefined
```

Many developers incorrectly think the function "remembers" the object. It doesn't.

---

### Rule 5. Nested object

```js
const company = {    
	name: "Google",    
	employee: {        
		name: "John",        
		print() {            
			console.log(this.name);        
		}    
	}
};

company.employee.print();
```

Output
```
John
```

Not Google.

Why?

```js
company.employee.print()
â†“
Who called print()?
â†“
employee
â†“
this = employee
```

Only the object immediately before the dot matters.

---

### Rule 6. Arrow functions

Arrow functions **do not create their own `this`**.

They take `this` from the surrounding scope (lexical `this`).
```js
const user = {    
	name: "John",    
	regular() {        
		console.log(this.name);    
	},    
	arrow: () => {        
		console.log(this.name);    
	}
};

user.regular();
user.arrow();
```

Output
```
Johnundefined
```

Why?

The arrow function wasn't called with its own `this`. It inherited `this` from where it was created (the surrounding scope), not from `user`.

---

### 7.Why arrows are useful

Without arrow functions:

```js
const user = {    
	name: "John",    
	start() {        
		setTimeout(function () {            
			console.log(this.name);        
		}, 1000);    
	}
};
user.start();
```

Output
```
undefined
```

Because:
```
setTimeout
â†“
calls your function
â†“
this = undefined
```

---

Using an arrow function:
```js
const user = {    
	name: "John",    
	start() {        
		setTimeout(() => {            
			console.log(this.name);        
		}, 1000);    
	}
};
user.start();
```

Output
```
John
```

What happens?
```
user.start()
â†“
this = user
â†“
Arrow function created
â†“
Arrow captures current this (user)
â†“
Later setTimeout executes arrow
â†“
Arrow still uses user
```

The arrow doesn't get a new `this`; it reuses the one from `start()`.

---

**Visual example**

```js
const person = {    
	name: "John",    
	sayHello() {        
		console.log(this.name);        
			setTimeout(() => {            
			console.log(this.name);        
		}, 1000);    
	}
};
person.sayHello();
```

Execution:

```
person.sayHello()
â†“
this = person
â†“
console.log(this.name)
â†“
John
â†“
Arrow function created
â†“
Arrow remembers this (person)
â†“
setTimeout executes arrow
â†“
console.log(this.name)
â†“
John
```

---

### 8.`call`, `apply`, and `bind`

You can explicitly choose what `this` should be.

```js
function greet() {    
	console.log(this.name);
}
const user = {    
	name: "John"
};
greet.call(user);
```

Output
```
John
```

Here you're telling JavaScript:

> "Execute `greet`, but use `user` as `this`."

Similarly:
```js
greet.apply(user);
```
<span style="color:#ff5555;">Does the same thing as <code>call()</code>, but accepts arguments as an array.</span>

```js
const bound = greet.bind(user);
bound();
```

`bind()` doesn't call the function immediately. It creates a **new function** that will always use `user` as `this`.

# 6. Prototype Chain

One of the most important topics.

Every object has an internal prototype.

```
user
â†“
User.prototype
â†“
Object.prototype
â†“
null
```

Property lookup walks the chain.

# 7. Prototype vs **proto**

Know the difference.

```
prototype
```

Exists only on constructor functions/classes.

```
__proto__
```

Reference from an object to its prototype.

# 8. Destructuring

```js
const { name, age } = user;
```

Renaming
```js
const { name: fullName } = user;
```

Default values
```js
const {    
	age = 18
} = user;
```

Rest
```js
const {    
	name,    
	...rest
} = user;
```


# 9. Spread Operator (`...`)

**Spread = unpack (expand) properties or elements into a new object/array.**

### Copy

```js
const copy = { ...user };
```

ðŸ‘‰ "Take everything from `user` and put it into a new object."

---

### Merge

```js
const result = {
    ...a,
    ...b
};
```

ðŸ‘‰ Copy everything from `a`, then everything from `b`.

**Rule:** **Last property wins.**

```js
const a = { age: 25 };
const b = { age: 30 };

const result = { ...a, ...b };

// { age: 30 }
```

---

### Update (Immutable)

```js
const updated = {
    ...user,
    age: 30
};
```

ðŸ‘‰ Copy everything, then replace `age`.

---

### Remember

- âœ… Creates a **shallow copy**.
- âœ… Copies **own enumerable properties**.
- âŒ Doesn't deep copy nested objects.

# 10. Getters & Setters

**Getter = compute a value when reading a property.**

**Setter = run logic when writing a property.**

```js
const user = {
    first: "John",
    last: "Doe",

    get fullName() {
        return `${this.first} ${this.last}`;
    },

    set fullName(value) {
        [this.first, this.last] = value.split(" ");
    }
};
```

---

### Getter (Read)

```js
console.log(user.fullName);
```

ðŸ‘‰ Executes `get fullName()`.

Returns:

```text
John Doe
```

---

### Setter (Write)

```js
user.fullName = "Alice Smith";
```

ðŸ‘‰ Executes `set fullName()`.

Result:

```js
user.first // "Alice"
user.last  // "Smith"
```

---

### Remember

- âœ… **Getter** â†’ runs when **reading** a property.
- âœ… **Setter** â†’ runs when **writing** a property.
- âœ… Lets you expose a property while hiding the implementation.

**Memory trick:**

> **Get = Read ðŸ“–**  
> **Set = Write âœï¸**

## Map

`Map` is a key-value collection.

It is useful when you need dynamic keys or keys that are not only strings.

```js
const userVisits = new Map();

const user = { id: 1 };

userVisits.set(user, 5);
userVisits.get(user); // 5
```

Map keys can be any type:

- string
- number
- object
- function
- symbol

```js
const map = new Map();

map.set("name", "Ana");
map.set(1, "numeric key");
map.set({ id: 1 }, "object key");
```

Use `Map` when:

- keys are dynamic
- keys are not only strings
- frequent add/delete/lookups are needed
- insertion order matters
- you need dictionary-like behavior

Use plain objects when:

- you model structured data
- keys are known property names
- data shape matters

```js
const userModel = {
  id: 1,
  name: "Ana"
};
```

Short idea: object is best for data shape, `Map` is best for dynamic key-value storage.
