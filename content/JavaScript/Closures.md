# What is a Closure?

> **A closure is a function that remembers and can access variables from the lexical scope in which it was created, even after that outer function has finished executing.**

Example:
```js
function outer() {    
	const message = "Hello";    
	function inner() {        
		console.log(message);    
	}    
	return inner;
}

const fn = outer();
fn();
```

Output:

```
Hello
```

Even though `outer()` has finished, `inner()` still has access to `message`.

---

# Why Does It Work?

When a function is created, JavaScript stores a reference to its **Lexical Environment**.

```
outer()
message = "Hello"
↓
returns inner()
↓
inner still remembers
message = "Hello"
```

The variables are **not copied**. The closure keeps a reference to the original lexical environment.

---

# Lexical Scope

Closures are based on **lexical scope**.

A function can access:
- its own variables
- variables from parent scopes
- global variables

```js
const country = "USA";

function outer() {    
	const city = "New York";    
	function inner() {        
		console.log(country);        
		console.log(city);    
	}    
	inner();
}
```

Output

```
USA
New York
```

The scope is determined by **where the function is defined**, not where it is called.

---

# Basic Example

```js
function counter() {    
	let count = 0;    
	return function () {        
		count++;        
		console.log(count);   
	};
}

const increment = counter();
increment();
increment();
increment();
```

Output

```
1
2
3
```

`count` remains alive because the returned function closes over it.

---

# Memory Visualization

```
Stack
increment      
│      
▼
HeapFunction
↓
Lexical Environment
count = 3
```

The lexical environment stays in memory as long as the closure is reachable.

---

# Multiple Closures

```js
function counter() {    
	let count = 0;    
	return {        
		inc() {            
			count++;        
		},        
		get() {            
			return count;        
		}    
	};
}

const c = counter();
c.inc();
c.inc();

console.log(c.get());
```

Output

```
2
```

Both methods share the same `count`.

---

# Independent Closures

```js
const a = counter();
const b = counter();

a.inc();
a.inc();

b.inc();

console.log(a.get());
console.log(b.get());
```

Output

```
2
1
```

Each call creates a new lexical environment.

---

# Private Variables

Closures provide data hiding.

```js
function createUser(name) {    
	let password = "12345"; 
	   
	return {        
		getName() {            
			return name;        
		}    
	};
}

const user = createUser("John");

console.log(user.getName());
```

You cannot access:

```js
user.password
```

↓

```
undefined
```

The password is private.

---

# Closures with Callbacks

```js
function greet(name) {    
	return function () {        
		console.log("Hello " + name);    
	};
}
const helloJohn = greet("John");
setTimeout(helloJohn, 1000);
```

The callback still remembers `name`.

---

# Closures in Loops

Classic interview question.

### Wrong

```js
for (var i = 0; i < 3; i++) {    
	setTimeout(() => {        
		console.log(i);    
	}, 100);
}
```

Output
```
3
3
3
```

There is only **one** `i`, shared by all callbacks.

---

### Correct (let)

```js
for (let i = 0; i < 3; i++) {    
	setTimeout(() => {        
		console.log(i);    
	}, 100);
}
```

Output

```
0
1
2
```

Each iteration creates a new binding for `i`.

---

### Correct (Closure)

```js
for (var i = 0; i < 3; i++) {    
	(function (index) {        
		setTimeout(() => {            
			console.log(index);        
		}, 100);    
	})(i);
}
```

Output

```
0
1
2
```

The IIFE creates a new closure for each iteration.

---

# Closures and Objects

```js
function person(name) {    
	return {        
		sayHello() {            
			console.log(name);        
		}    
	};
}
```

The object method remembers `name`.

---

# Memoization

Closures store cached results.

```js
function memo() {    
	const cache = {};    
	return function (n) {        
		if (cache[n]) {            
			return cache[n];        
		}        
		cache[n] = n * 2;        
		return cache[n];    
	};
}
const double = memo();
double(5);
double(5);
```

The second call uses the cached value.

---

# Module Pattern

Before ES Modules, closures were commonly used to create modules.

```js
const counter = (() => {    
	let count = 0;    
	return {        
		increment() {            
			count++;        
		},   
		     
		value() {            
			return count;        
		}    
	};
})();
```

`count` cannot be modified directly from outside.

---

# Closures and Garbage Collection

Variables captured by a closure are **not** garbage collected while the closure is still reachable.

```js
function test() {    
	const bigArray = new Array(1000000);    
	return function () {        
		console.log(bigArray.length);    
	};
}
```

The array stays in memory until the returned function becomes unreachable.

---

# Potential Memory Leaks

Closures can accidentally keep large objects alive.

```js
function create() {    
	const hugeObject = loadHugeData();    
	return function () {        
		console.log("clicked");    
	};
}
```

Even if `hugeObject` isn't used inside the returned function, it may remain alive because the closure retains the surrounding environment. Modern JavaScript engines can sometimes optimize unused captured variables, but you shouldn't rely on that behavior. Avoid capturing unnecessary data in long-lived closures.

---

# Real-World Uses

Closures are used in:

- Event listeners
- Callbacks
- `setTimeout()`
- `setInterval()`
- Promises
- Async functions
- React Hooks
- Angular services and factories
- RxJS operators
- Currying
- Memoization
- Debouncing
- Throttling
- Module pattern

---

# Common Interview Questions

### What is a closure?

A function that remembers variables from the scope where it was created, even after that scope has finished executing.

---

### Why are closures useful?

They allow:

- private state
- encapsulation
- callbacks
- function factories
- memoization
- currying
- module patterns

---

### Why doesn't the outer variable disappear?

Because the returned function still references the lexical environment, preventing it from being garbage collected.

---

### Do closures copy variables?

No.

They keep **references** to variables in their lexical environment.

Example:

```js
let count = 0;
function test() {    
	console.log(count);
}
count = 5;test();
```

Output
```
5
```

The closure sees the current value of `count`, not a copy taken when the function was created.