# Design Patterns

This repository provides a **high-level overview** of common Design Patterns used in software development.

For a **full explanation with detailed examples**, check the complete repository here:  
👉 [Design Patterns](https://github.com/MohamedHashish42/Design_Patterns/tree/master)
  
---    


[🔥Most Used in Real Projects](#most-used-in-real-projects)  
[What are Creational Design Patterns?](#what-are-creational-design-patterns)  
[What are Structural Design Patterns?](#what-are-structural-design-patterns)  
[What are Behavioral Design Patterns?](#what-are-behavioral-design-patterns)  
[Additional Design Concepts](#additional-design-concepts)  


## 🔥Most Used in Real Projects
These patterns are widely used in real-world backend applications and enterprise systems.  

### Dependency Injection

Dependency Injection is a technique where dependencies are provided to a class from the outside rather than created inside it.

**Why it matters:**
- Reduces coupling between components  
- Improves testability and maintainability  

---

### Repository Pattern
Provides a collection-like interface for accessing domain objects while abstracting the underlying data access logic from the business layer.

---

### Unit of Work
Coordinates changes across one or more repositories within a single transaction.

It ensures that all operations are committed together, or rolled back together, to maintain data consistency.

**Relationship with Repository Pattern:**
- Repository handles data access per entity
- Unit of Work manages transactions across multiple repositories
- Together they provide a clean and consistent data access layer


## What are Creational Design Patterns?
According to [Wikipedia](https://en.wikipedia.org/wiki/Creational_pattern), in software engineering, creational design patterns are design patterns that deal with object creation mechanisms i.e. trying to create objects in a manner that is suitable to a given situation.

In simple words, we can say that the Creational Design Pattern deals with Object Creation and Initialization. This Design Pattern gives the programmer more flexibility in deciding which objects need to be created for a given situation.

### Examples
#### 1. Factory Design Pattern


The Factory Pattern centralizes object creation in a single place. Instead of creating objects directly using constructors, a factory method decides which object to create based on input parameters.

**Key Idea:**
Creation logic is handled inside one class using conditional logic (e.g., `if` / `switch`).

**When to use:**

* When the type of object depends on runtime input
* When you want to hide object creation details from the client

---

#### 2. Factory Method Design Pattern
The Factory Method Pattern defines a common interface for object creation, but allows subclasses to decide which concrete class to instantiate.

**Key Idea:**
Creation logic is delegated to subclasses instead of being handled in a single class.

**When to use:**
* When a class cannot know which objects it needs to create
* When you want to follow the Open/Closed Principle (extend without modifying existing code)

**🔍 Difference Between Factory and Factory Method**  

* **Factory Pattern:** uses conditional logic inside one class to decide which object to create
* **Factory Method Pattern:** moves the creation responsibility to subclasses, eliminating conditional logic
---

#### 3. Abstract Factory Design Pattern
According to Gang of Four Definition: “The Abstract Factory Design Pattern provides a way to encapsulate a group of individual factories that have a common theme without specifying their concrete classes“.

---

#### 4. Builder Design Pattern
According to the Gang of Four’s definition, Builder Design Pattern “Separate the construction of a complex object from its representation so that the same construction process can create different representations.”


---

#### 5. Prototype Design Pattern
The Prototype Pattern creates new objects by cloning an existing instance.

✔ Useful when object creation is expensive  
✔ Avoids rebuilding complex objects from scratch

---

#### 6. Singleton Design Pattern
Singleton is a Design Pattern that ensures that only one instance of a particular class is going to be created and then provide simple global access to that instance for the entire application.


## What are Structural Design Patterns?
Structural design patterns are those that simplify the design of large object structures by identifying relationships between them.

They describe common ways of composing classes and objects so that they become repeatable as solutions

### Examples
#### 1. Adapter Design Pattern
The Adapter Design Pattern works as a bridge between two incompatible interfaces.   
This design pattern involves a single class called adapter which is responsible for communication between two independent or incompatible interfaces.

---

#### 2. Facade Design Pattern
As per the GoF definition, the Facade Design Pattern states that you need to provide a unified interface to a set of interfaces in a subsystem.  
The Facade Design Pattern defines a higher-level interface that makes the subsystem easier to use.

---

#### 3. Decorator Design Pattern
The Decorator Design Pattern allows us to dynamically add new functionalities to an existing object without altering or modifying its structure and this design pattern acts as a wrapper to the existing class.

A decorator is an object that adds features to another object.

---

#### 4. Bridge Design Pattern
As per the GoF definitions, the Bridge Pattern “Decouples an abstraction from its implementation so that the two can vary independently”

In the Bridge Design Pattern, there are two layers.

The first layer is the Abstraction layer

and the second layer is the Implementation Layer.

If I make any changes in the Implementation Layer, then it won’t affect the Abstraction Layer.

Similarly, if I make any changes in the Abstraction Layer, then it won’t affect the Implementation layer.

---

#### 5. Composite Design Pattern
As per the GoF definitions, the Composite Design Pattern states that “Compose objects into tree structures to represent part-whole hierarchies. Composite let clients treat individual objects and compositions of objects uniformly”.

The Composite Pattern is used where we need to treat a group of objects in a similar way as a single object.

---

#### 6. Proxy Design Pattern
According to the GoF definitions, the Proxy Design Pattern provides a surrogate (act on behalf of other) 
or placeholder for another object to control access to it. Proxy means “in place of” or “representing” or “on behalf of”

---

#### 7. Flyweight Design Pattern
A flyweight is a shared object that can be used in multiple contexts simultaneously.

The Flyweight Design Pattern is used when there is a need to create a large number of objects of almost similar nature.  
A large number of objects consumes a large amount of memory and the Flyweight design pattern provides a solution for reducing the load on memory by sharing objects.



## What are Behavioral Design Patterns?

### Examples
#### 1. Observer Design Pattern
According to GoF, the Observer design Pattern should “Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically”.

---

#### 2. Chain of Responsibility Design Pattern
According to GoF Definitions, “Avoid coupling the sender of a request to its receiver by giving more than one receiver object a chance to handle the request. Chain the receiving objects and pass the request along the chain until an object handle it”.

In simple words, we can say that the chain of responsibility design pattern creates a chain of receiver objects for a given request. Each receiver object contains a reference to another receiver. If one receiver cannot handle the request then it passes the same request to the next receiver and so on.

Sometimes one receiver is enough to handle the request in the chain and other times we need more than one receiver to handle the request.

---

#### 3. State Design Pattern
According to Gang of Four Definitions, the State Design Pattern “allows an object to alter its behavior when its internal state changes” In simple words, we can say that the State Pattern is a design pattern that allows an object to completely change its behavior depending upon its current internal state.

---

#### 4. Template Method Design Pattern
According to Gang of Four Definitions, The Template Method Design Pattern “Define the skeleton of an algorithm in an operation, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm’s structure”

in other words, it defines a sequence of steps of an algorithm and allows the subclasses to override the steps but do not allow changing the sequence. The Key to the Template Design Pattern is that we put the general logic in the abstract parent class and let the child classes define the specifics.

---

#### 5. Visitor Design Pattern
Represent an operation to be performed on the elements of an object structure.

Visitor lets you define a new operation without changing the classes of the elements on which it operates.

---

#### 6. Strategy Design Pattern
According to Gang of Four Definitions, Strategy Design Pattern “defines a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the algorithm vary independently from clients that use it.”

The Strategy Design Pattern is used when we have multiple algorithms (solutions) for a specific task and the client selects the actual implementation to be used at runtime.

---

#### 7. Interpreter Design Pattern
According to GOF Interpreter Design Pattern “defines a representation for its grammar along with an interpreter that uses the representation to interpret sentences in the language.”

---

#### 8. Mediator Design Pattern
According to the Gang of Four’s definition, Mediator Design Pattern “defines an object that encapsulates how a set of objects interact. Mediator promotes loose coupling by keeping objects from referring to each other explicitly, and it lets you vary their interaction independently.”


**Simplification of the above GoF definition:**
The Mediator Design Pattern is used to reduce the communication complexity between multiple objects. This design pattern provides a mediator object and that mediator object normally handles all the communication complexities between different objects. The Mediator object acts as the communication center for all objects. That means when an object needs to communicate with another object, then it does not call the other object directly, instead, it calls the mediator object and it is the responsibility of the mediator object to route the message to the destination object.

---

#### 9. Memento Design Pattern
The Memento Design Pattern is used to restore an object to its previous state. That means if you want to perform some kind of undo or rollback operation in your application then you need to use the Memento Design Pattern.


## Additional Design Concepts

### Fluent Interface
Fluent Interface is a design style that improves code readability by enabling method chaining.   
It is often used with the Builder pattern but can be applied more broadly. It is not one of the GoF design patterns.


