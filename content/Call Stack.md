The **Call Stack** is a [[Data structures |LIFO]] (Last In, First Out) data structure used by the JavaScript engine to keep track of function execution.

- Every function call creates a **stack frame**.
- Frames are pushed onto the stack when functions are called.
- Frames are removed when functions return.
- Only one frame executes at a time because JavaScript is single-threaded.

```js
function first() {  
second();  
}  
  
function second() {  
third();  
}  
  
function third() {  
console.log('Hello');  
}  
  
first();  
```

Execution:
```
Call Stack
------------
first()
second()
third()
console.log()
```

# Stack Frames

Each frame contains:

- Local variables
- Function arguments
- Return address
- `this`
- Scope information

Example:

```js
function add(a, b) {  
const sum = a + b;  
return sum;  
}  
  
add(2, 3);
```

Frame:

```
add()  
---------  
a = 2  
b = 3  
sum = 5  
return address
```

# Microtask Queue
Microtasks are **high-priority tasks**.

Examples:

- `Promise.then()`
- `Promise.catch()`
- `Promise.finally()`
- `queueMicrotask()`
- `MutationObserver`

# Macrotask Queue
Macrotasks are lower-priority tasks.

Examples:

- `setTimeout()`
- `setInterval()`
- DOM events (click, keyup, etc.)
- HTTP callbacks
- MessageChannel
- postMessage

# [[Zone.js]] and the Call Stack

Zone.js patches:

- setTimeout
- Promise
- Events
- XHR

When async work finishes:

```
callback
↓
stack empty
↓
Zone.js
↓
Angular change detection
```

In zoneless Angular, change detection is triggered by Signals or explicit APIs instead.