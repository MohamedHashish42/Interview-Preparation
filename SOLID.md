
# SOLID Principles

- [What Are SOLID Principles?](#what-are-solid-principles)
- [Why Are SOLID Principles Important in Object-Oriented Design?](#why-are-solid-principles-important-in-object-oriented-design)

## What Are SOLID Principles?
SOLID principles are five design principles that improve software design by making it more maintainable, flexible, and scalable.

### 1. **S**ingle Responsibility Principle (SRP):
Software entities (classes, modules) should have **only one reason to change**.

#### **What “one reason to change” really means?**  

A class should only be changed for **one specific type of responsibility or role**.

In other words:

> Things that change for the same reason should be kept together, and things that change for different reasons should be separated.

#### **Simple example:**

⚠️ If a class handles:

* user data
* database saving
* sending emails

Then it has **multiple reasons to change** (database rules, email rules, user logic).

✅ But if each part is separated:

* User class → user data
* Repository → database changes
* Email service → email logic

Then each one changes only when **its own job/role changes**.


### 2. **O**pen/Closed Principle (OCP)
Software entities (classes, modules) should be **open for extension** but **closed for modification**.  
[Check With and Without Examples](./RelatedDocuments/SOLID/OCP.md)  

### 3. **L**iskov Substitution Principle (LSP)
Subtypes should be substitutable for their base types **without altering the expected behavior of the program**.  
[Check With and Without Examples](./RelatedDocuments/SOLID/LSP.md)  


### 4. **I**nterface Segregation Principle (ISP)
Clients should not be forced to depend on interfaces they don't use. Instead, create **smaller and more specific interfaces**.  
[Check With and Without Examples](./RelatedDocuments/SOLID/ISP.md)  


### 5. **D**ependency Inversion Principle (DIP)
High-level modules should not depend on low-level modules, both should depend on abstractions.  
[Check With and Without Examples](./RelatedDocuments/SOLID/DIP.md)  
#### **What is Abstraction?**

In simple words, abstraction means **hiding implementation details while exposing only the essential behavior**.

In programming, abstraction is achieved using **interfaces, abstract classes, or APIs** , not only abstract classes or interfaces.

In programming, abstraction is achieved by defining contracts such as **interfaces**, **abstract classes**, or **APIs that specify behavior without exposing implementation details**.

## Why are SOLID principles important in object-oriented design?
The **SOLID principles** are important in object-oriented design because they promote:

* **Maintainability**: Code is easier to update and modify without breaking other parts.
* **Scalability**: Systems can grow and evolve with minimal changes.
* **Testability**: Smaller, focused classes and methods are easier to unit test.
* **Reusability**: Well-designed components can be reused across projects.
* **Flexibility**: Code adapts more easily to changing requirements.
* **Loose Coupling**: Components are less dependent on each other.

In short, SOLID principles lead to **cleaner, more robust, and manageable code**.

