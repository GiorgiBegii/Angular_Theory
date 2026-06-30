**Focus on communication and responsibilities between objects.**

### Observer 

Observer is a pattern where one object publishes changes, and other objects can subscribe and react when those changes happen. 

Angular uses the Observer pattern through RxJS Observables. A service or component can emit data, and multiple subscribers can listen and react to those changes. Common examples are HttpClient, router events, and reactive forms.

### Strategy 

Strategy is used when we have multiple ways to do the same task and want to choose the appropriate one without changing the code that uses it. 

Strategy allows us to switch between different implementations of the same behavior. In Angular, route guards and validators are good examples because Angular can use different implementations while the code that uses them stays the same. 

### Command 

Command turns an action into an object or function that can be executed later. It separates the code that requests an action from the code that performs it. 

Command encapsulates an action so it can be executed later by another part of the system. In Angular, NgRx actions are a good example because components dispatch actions and the store handles the actual work. 

### Mediator 

Mediator is used when multiple objects need to communicate, but instead of talking directly to each other, they communicate through a central object. 

Mediator centralizes communication between objects. In Angular, a shared service with RxJS is a common example: components communicate through the service instead of directly depending on each other. 

### State 

State is used when an object's behavior changes depending on its current state. Instead of writing many if or switch statements, we organize the behavior by state.

State allows an object or UI to behave differently based on its current state. In Angular, we commonly use this idea for loading, success, error, authentication, and other UI states where behavior changes according to the current status. 

### Chain of Responsibility 

Chain of Responsibility passes a request through a sequence of handlers. Each handler can process it or pass it to the next one. 

Chain of Responsibility passes a request through multiple handlers until it is processed or rejected. In Angular, HTTP Interceptors are the best example because each interceptor can modify the request, handle it, or pass it to the next interceptor in the chain. 

### Template Method 

Template Method defines the overall process in a base class, while child classes provide the specific steps. The flow stays the same, but some parts can be customized. 

Template Method defines a fixed workflow while allowing specific steps to be implemented differently. In Angular, lifecycle hooks are a good example because Angular controls the component lifecycle and developers implement the required hook methods to customize behavior. 

### Iterator 

Iterator provides a way to go through a collection of items one by one without needing to know how the collection is stored internally. 

Iterator provides a standard way to traverse a collection without exposing its internal structure. In Angular, *ngFor (or @for) is a common example because it iterates through collections and renders items one by one.

### Visitor 

Visitor allows us to add new operations to existing objects without changing those objects. 

Visitor allows adding new operations to existing objects without modifying their classes. Angular doesn't have a strong built-in Visitor example, but traversing form trees (FormGroup, FormControl, FormArray) and applying operations such as validation or serialization follows a similar idea.

