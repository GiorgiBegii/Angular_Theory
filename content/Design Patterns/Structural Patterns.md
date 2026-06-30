**Focus on how classes and objects are composed.**

### Adapter

Adapter is used when two pieces of code have different interfaces, and we need a small layer that makes them work together without changing either side.

Angular uses the Adapter pattern in **ControlValueAccessor**, which allows custom components to work with Angular Forms. More generally, adapters are often used to convert API responses or third-party libraries into a format the application expects.

### Facade

Facade provides a simple interface over a more complex system. Instead of a component talking to many services directly, it talks to one facade service that handles everything. 

Angular commonly uses the Facade pattern by placing business logic in a facade service. Components communicate with the facade instead of multiple services, which simplifies the component and hides system complexity. 

### Decorator 

Decorator adds extra behavior or metadata to a class, method, property, or parameter without changing the original code structure. 

Angular heavily uses the Decorator pattern. Decorators such as @Component, @Injectable, @Input, and @Output add metadata that tells Angular how a class or property should behave without modifying the class logic itself.

### Proxy 

Proxy is an object that stands in front of another object and controls access to it. It can add extra behavior such as logging, caching, security checks, or request modification. 

Angular uses the Proxy pattern through HTTP Interceptors. Interceptors sit between the application and HTTP requests, allowing us to add authentication, logging, error handling, or other behavior without changing the code that makes the request. 

### Composite 

Composite is used when individual objects and groups of objects should be treated the same way. 

Angular uses the Composite pattern in Reactive Forms. FormControl, FormGroup, and FormArray form a tree structure and can be treated through the common AbstractControl API. The Angular component hierarchy is also a good example of a composite structure.

### Flyweight 

Flyweight is used when we have many similar objects and want to save memory by sharing common data instead of storing it in every object. 

Flyweight reduces memory usage by sharing common data between many objects. Angular applies similar ideas internally by sharing component metadata, templates, and singleton services instead of duplicating them for every instance.

