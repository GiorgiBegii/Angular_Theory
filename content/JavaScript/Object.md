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

│ 
▼

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
	sayHello: ƒ
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
↓
Who called sayHello?
↓
user
↓
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
↓
this = user1
```

```
user2.sayHello()
↓
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

In strict mode:
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
↓
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
↓
no object
↓
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
↓
Who called print()?
↓
employee
↓
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
↓
calls your function
↓
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
↓
this = user
↓
Arrow function created
↓
Arrow captures current this (user)
↓
Later setTimeout executes arrow
↓
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
↓
this = person
↓
console.log(this.name)
↓
John
↓
Arrow function created
↓
Arrow remembers this (person)
↓
setTimeout executes arrow
↓
console.log(this.name)
↓
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
↓
User.prototype
↓
Object.prototype
↓
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

👉 "Take everything from `user` and put it into a new object."

---

### Merge

```js
const result = {
    ...a,
    ...b
};
```

👉 Copy everything from `a`, then everything from `b`.

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

👉 Copy everything, then replace `age`.

---

### Remember

- ✅ Creates a **shallow copy**.
- ✅ Copies **own enumerable properties**.
- ❌ Doesn't deep copy nested objects.

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

👉 Executes `get fullName()`.

Returns:

```text
John Doe
```

---

### Setter (Write)

```js
user.fullName = "Alice Smith";
```

👉 Executes `set fullName()`.

Result:

```js
user.first // "Alice"
user.last  // "Smith"
```

---

### Remember

- ✅ **Getter** → runs when **reading** a property.
- ✅ **Setter** → runs when **writing** a property.
- ✅ Lets you expose a property while hiding the implementation.

**Memory trick:**

> **Get = Read 📖**  
> **Set = Write ✏️**