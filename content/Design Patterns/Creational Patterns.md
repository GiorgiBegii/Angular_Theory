**Focus on object creation.**

### Singleton 

 Singleton means there is only one instance of a service in the application, and all parts of the app use the same instance.

### Factory Method

Factory Method is a way to move object creation into a separate place instead of creating objects directly with new everywhere. The code asks for an object, and the factory decides which implementation to create and return.

Angular uses Factory Method through its Dependency Injection system. Components ask for a service, and Angular decides how to create and provide that service. This keeps object creation separate from the code that uses the object and makes the application easier to maintain and extend.

### Abstract Factory

Abstract Factory is used when we need a group of related objects that should work together. Instead of deciding which object to create in many places, we have one place that provides the whole set. This makes it easy to switch between different implementations without changing the code that uses them.

### Builder

Builder is useful when an object has many options. Instead of creating everything at once, we add the options one by one and get the final object at the end. This makes the code easier to read and maintain.

### Prototype

Angular doesn't directly use the Prototype pattern in everyday development. The Prototype pattern is about creating new objects by copying existing ones. In Angular, we usually create new objects with object spread (...), Object.assign, or by mapping data rather than using explicit clone methods.

