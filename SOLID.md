# SOLID Principles
- [What Are SOLID Principles?](#what-are-solid-principles)
- [Why Are SOLID Principles Important in Object-Oriented Design?](#why-are-solid-principles-important-in-object-oriented-design)

## What Are SOLID Principles?
SOLID principles are five design principles that improve software design by making it more maintainable, flexible, and scalable:

### 1. **S**ingle Responsibility Principle (SRP):
Software entities (classes, modules) should have **only one** responsibility to do.

### 2. **O**pen/Closed Principle (OCP)
Software entities (classes, modules) should be **open for extension** but **closed for modification**.  
[Check With and Without Examples](./RelatedDocuments/SOLID/OCP.md)  

### 3. **L**iskov Substitution Principle (LSP)
Subtypes should be substitutable for their base types without altering the correctness of the program.  
[Check With and Without Examples](./RelatedDocuments/SOLID/LSP.md)  


### 4. **I**nterface Segregation Principle (ISP)
Clients should not be forced to depend on interfaces they don't use. create smaller, specific interfaces.  
[Check With and Without Examples](./RelatedDocuments/SOLID/ISP.md)  


### 5. **D**ependency Inversion Principle (DIP)
High-level modules should not depend on low-level modules; both should depend on abstractions.  
[Check With and Without Examples](./RelatedDocuments/SOLID/DIP.md)  
### What is Abstraction?
In simple words, we can say that Abstraction means something which is **non-concrete**.   
So, abstraction in programming means we need to create either **an interface** or **abstract class** which is **non-concrete** so that we can not create an instance of it.

## Why are SOLID principles important in object-oriented design?
The **SOLID principles** are important in object-oriented design because they promote:

* **Maintainability**: Code is easier to update and modify without breaking other parts.
* **Scalability**: Systems can grow and evolve with minimal changes.
* **Testability**: Smaller, focused classes and methods are easier to unit test.
* **Reusability**: Well-designed components can be reused across projects.
* **Flexibility**: Code adapts more easily to changing requirements.

In short, SOLID principles lead to **cleaner, more robust, and manageable code**.

