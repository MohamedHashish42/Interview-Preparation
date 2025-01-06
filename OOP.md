

# Object-Oriented Programming (OOP)

- [What is **OOP** and What are its four pillars](#what-is-oop-and-what-are-its-four-pillars)
- [What is the difference between **Class** and **Object**?](#what-is-the-difference-between-class-and-object)
- [What is the difference between **Instructor** and **Destructor**?](#what-is-the-difference-between-instructor-and-destructor)
- [What is **Constructor Chaining**?](#what-is-constructor-chaining)
- [What are **Getters** and **Setters**?](#what-are-getters-and-setters)
- [What are **Access Modifiers**?](#what-are-access-modifiers)
- [What is **Event**?](#what-is-event)
- [What are the differences between **Inheritance** and **Composition**?](#what-are-the-differences-between-inheritance-and-composition)
- [What is the **this** keyword?](#what-is-the-this-keyword)
- [What is the **base** keyword?](#what-is-the-base-keyword)
- [What is the **static** modifier?](#what-is-the-static-modifier)
- [What is the **virtual** modifier?](#what-is-the-virtual-modifier)
- [What is the **abstract** modifier?](#what-is-the-abstract-modifier)
- [What is the difference between **virtual** and **abstract**?](#what-is-the-difference-between-virtual-and-abstract)
- [What is the **sealed** modifier?](#what-is-the-sealed-modifier)
- [What are the **is** and **as** keywords?](#what-are-the-is-and-as-keywords)
- [What are **Downcasting** and **Upcasting**?](#what-are-downcasting-and-upcasting)
- [What is the **Method Signature**?](#what-is-the-method-signature)
- [What is the **Method Overloading**?](#what-is-the-method-overloading)
- [What is the **Method Overriding**?](#what-is-the-method-overriding)
- [What is the difference between **abstract class** and **interface**?](#what-is-the-difference-between-abstract-class-and-interface)




## What is **OOP** and What are its four pillars?
**Object-Oriented Programming (OOP)** is a programming paradigm based on the concept of **objects**, which contain data (properties) and behavior (methods). 

The main principles of OOP are:

1. [**Encapsulation**](./RelatedDocuments/OOP/Encapsulation.md): Bundling data (properties) and behavior (methods) within a single unit called a class.  
It also restricts direct access to some of an object's components, which helps prevent unintended interference or misuse of the data.
2. [**Inheritance**](./RelatedDocuments/OOP/Inheritance.md): Allowing a class to inherit properties and methods from another class.
3. [**Polymorphism**](./RelatedDocuments/OOP/Polymorphism.md): Allowing objects of different types to be treated as instances of the same base type.
This is commonly achieved through:  
**Inheritance:**  
Derived classes override  methods of a base class. For example, a Shape base class might have a Draw() method, and derived classes like Circle and Rectangle provide their specific implementations of Draw().  
**Interfaces:**  
Classes can implement a common interface, ensuring they all provide specific methods regardless of their internal implementation. For instance, multiple classes implementing an IDrawable interface can be used interchangeably where IDrawable is expected.  

4. [**Abstraction**](./RelatedDocuments/OOP/Abstraction.md): Hiding the complexity of the system and showing only essential features.


## What is the difference between **Class** and **Object**? 
A **class** is a blueprint or template for creating objects. It defines data (properties) and behavior (methods) that the objects created from the class will have. It does not hold data itself.

An **object** is an instance of a class. It holds actual data and can use the methods in defined in its class.


## What is the difference between **instructor** and **destructor**?
**Constructor**:
- A special method in a class used to initialize objects.
- It is automatically called when an object is created.
- Can be overloaded (multiple constructors with different parameters).  

**Destructor**:
- A special method used to clean up resources before an object is destroyed.
- It is automatically called by the garbage collector.
- Cannot be overloaded and is rarely used in C# due to automatic memory management.

## What is **Constructor Chaining**?
Constructor chaining is a technique where one constructor calls another constructor in the same class or in a base class to reuse code and avoid duplication.

### Usage 
The **this** keyword is used to chain constructors within the same class, while **base** is used to call a constructor in a base class.

### Example
**Constructor Chaining Within the Same Class**
```csharp
public class Car
{
    public string Make { get; set;}
    public string Model { get; set;}

    public Car(string make, string model)
    {
        Make = make;
        Model = model;
    }

    public Car(string make) : this(make, "Unknown")
    {
    }
}

```

**Constructor Chaining Within a Base Class:**
```csharp
public class Vehicle
{
    public string Type { get; set;}
    public Vehicle(string type)
    {
      Type = type;
    }
}

public class Car : Vehicle
{
    public string Make { get; set;}
    public Car(string make) : base("Car")
    {
       Make = make;
    }
}
```


## What are **getters** and **setters**?

A **getter** and **setter** (also known as accessors) are special methods that allow you to control access to a class's properties. They define how a property's value can be read and modified.

Here's a basic example:

```csharp
public class Person
{
    private string name;
    public string Name
    {
        get { return name; } 
        set { name = value; }
    }
}
```
You can also write this using the more concise auto-property syntax:

```csharp
public class Person
{
    public string Name { get; set; }
}
```


### Example:
```csharp
public class Person
{
    private int age;
    public int Age
    {
        get { return age; }
        set 
        { 
            if (value >= 0)
                age = value;
            else
                throw new ArgumentException("Age cannot be negative");
        }
    }
}
```



## What are **access modifiers**?

**Access modifiers** in C# are keywords that determine the **accessibility** of member or a type. They control which parts of the program can see or use a particular member.

### Common Access Modifiers in C#:

1. **public**:
   **Accessible from anywhere** in the program, including other classes and assemblies.

2. **private**:
   **Accessible only within the same class**. It is the most restrictive access level.

3. **protected**:
   **Accessible within the same class and derived (child) classes**.

4. **internal**:
   **Accessible within the same assembly** but not from other assemblies.

5. **protected internal**:
   **Accessible within the same assembly or in derived classes, even if they are in different assemblies**.
   ```Csharp
      public class Car 
      {
          protected internal string model;
      }
   ```

6. **private protected**:
   **Accessible within the same class and by derived classes within the same assembly**.
    ```Csharp
      public class Car 
      {
         private protected string model;
      }
   ```
## What is **Event**?
An event in C# is a way for a class to notify other classes or objects when something of interest happens. Event is based on delegate which define the method signature that the event handlers (subscriber methods) must match, allowing one or more methods (event handlers) to be invoked in response to an action.

### Key Features of Events:
1. **Encapsulation:**  
   Events provide a mechanism for classes to expose notifications without revealing the internal logic.
   
2. **Publisher-Subscriber Model:**  
   - A **publisher** defines an event and decides when to raise it.
   - **Subscribers** (other objects) attach their methods (event handlers) to handle the event.

### Syntax:

#### Declaring an Event:

```csharp
public event EventHandler MyEvent;
```

#### Note 

In C# you can use  EventHandler or Custom Event Arguments
**EventHandler:**  
   - Standard delegate type for events.
   - Signature: `void Handler(object sender, EventArgs e)`

**Custom Event Arguments:**  
   To pass additional data with events, use a custom class inheriting from `EventArgs`:

#### Raising an Event:
```csharp
protected virtual void OnMyEvent()
{
    MyEvent?.Invoke(this, EventArgs.Empty);
}
```

#### Subscribing to an Event:
Other classes can subscribe to the event:
```csharp
publisher.MyEvent += SubscriberMethod;
```

#### Unsubscribing from an Event:
To detach a method:
```csharp
publisher.MyEvent -= SubscriberMethod;
```

### Examples
[Event Examples](./RelatedDocuments/OOP/Event.md) 

## What are the differences between **Inheritance** and **Composition**?
Inheritance and composition are two fundamental ways to achieve code reuse and establish relationships between classes in object-oriented programming, but they follow different approaches

I'll convert the key differences into a clear table format: 

| Aspect | Inheritance | Composition |
|--------|------------|-------------|
| Relationship Type | "is-a" relationship (Dog is an Animal) | "has-a" relationship (Car has an Engine) |
| Coupling | Tight coupling between parent and child classes | Loose coupling with more flexibility |
| Extensibility | Limited to single inheritance (no multiple inheritance in C#) | Can include multiple objects of different types |
| Code Reuse | Child class reuses and can override parent's behavior | Class reuses behavior by delegating to contained objects |
| Runtime Behavior | Cannot change parent class at runtime | Can change composed objects at runtime (good for dependency injection) |

### Inheritance Example
```csharp
public class Animal
{
    public void Eat() => Console.WriteLine("Eating...");
}

public class Dog : Animal // Dog is an Animal
{
    public void Bark() => Console.WriteLine("Barking...");
}
```

### Composition Example
 **Concrete Composition or Direct Composition (Tightly Coupled)**
```csharp
public class Engine
{
    public void Start() => Console.WriteLine("Engine started...");
}

public class Car // Car has an Engine
{
    private Engine _engine = new Engine();

    public void StartCar()
    {
        _engine.Start();
        Console.WriteLine("Car started...");
    }
}
```

or

**Interface-Based Composition (Loosely Coupled)**  

```csharp

public interface IEngine
{
    void Start();
}


public class GasolineEngine : IEngine
{
    public void Start() => Console.WriteLine("Gasoline engine started...");
}

public class ElectricEngine : IEngine
{
    public void Start() => Console.WriteLine("Electric motor powered up...");
}

public class Car
{
    private readonly IEngine _engine;

    public Car(IEngine engine)
    {
        _engine = engine;
    }

    public void StartCar()
    {
        _engine.Start();
        Console.WriteLine("Car started...");
    }
}
```


## What is the **this** keyword?
The **this** keyword in C# is a reference to the current instance of the class.  
It is used within a class to refer to the instance members (fields, methods, properties, or constructors) of that class.


### Key Points:
1. **Access Instance Members**: The `this` keyword is used to distinguish between instance members and parameters or local variables with the same name.
2. **Constructor Chaining**: It can be used to call another constructor in the same class, enabling constructor chaining.
3. **Pass Current Instance**: It can be passed as an argument to other methods or constructors to refer to the current object.
## What is the **base** keyword?
The **base** keyword in C# is used to access members of the **base class** from within a **derived class**.

### Key Points:
1. **Access Base Class Members**: The `base` keyword allows a derived class to refer to members of its base class.
2. **Constructor Chaining**: It is commonly used to call a constructor of the base class when creating an instance of a derived class.
3. **Method Overriding**: It can be used within an overridden method to call the base class implementation of that method.

## What is the **static** modifier?
The `static` modifier in C# is used to declare members that belong to the type itself rather than to any specific instance of the type. 

 Static members are shared across all instances of a class, meaning there's only one copy in memory. This is useful for resources or operations that need to be common for all instances, such as configuration settings or utility functions.

 ```csharp
   var Date = DateTime.Now; //(Now) is static member
   Console.WriteLine(); //(WriteLine) is static member
 ```

## What is the **virtual** modifier?
The virtual keyword is used to declare a method, property, indexer, or event in a base class that can be overridden in derived classes.

This enables **polymorphism**, allowing a derived class to provide a different implementation of the method or member.

### Key Points:
-  Declaring a member as `virtual` means that it can be overridden in any derived class using the `override` keyword.
- The `virtual` member must have a definition in the base class.
- Derived classes do not have to override a `virtual` member; they can use the base class implementation.

## What is the **abstract** modifier?
The **abstract** modifier is used to define incomplete classes and methods that serve as a blueprint for other classes.   
An abstract class cannot be instantiated on its own, and any method marked as abstract must be implemented by derived (subclassed) classes.


## What is the difference between **virtual** and **abstract**?
| Aspect | `virtual` | `abstract` |
|---------|-----------|------------|
| Implementation | Must have a default implementation | Cannot have an implementation |
| Overriding | Optional - derived classes may override | Mandatory - derived classes must override |
| Class Type | Can be in any class | Can only be in abstract classes |
| Object Creation | Class can be instantiated | Class cannot be instantiated |
| Usage Scenario | When default behavior is needed but might be overridden | When derived classes must implement their own behavior |
| Need for 'override' keyword | Yes, when overriding | Yes, when implementing |
| Can be sealed | Yes | No |
| Method Example | `virtual void Start() { /* code */ }` | `abstract void Start();` |
| Base Class Example | `class Animal { }` | `abstract class Animal { }` |
## What is the **sealed** modifier?
**sealed** is used to  prevents inheritance of a class or prevents further overriding of a method.

## What are the **is** and **as** keywords?
The `is` and `as` keywords in C# are used for type checking and safe type casting.

### `is` Keyword:
- **Purpose**: Checks if an object is of a specified type.
- **Usage**: Returns `true` if the object is of the specified type; otherwise, `false`.
- **Example**:
    ```csharp
    object obj = "Hello, World!";
    if (obj is string)
    {
        Console.WriteLine("The object is a string.");
    }
    ```

### `as` Keyword:
- **Purpose**: Performs a **safe cast** of an object to a specified type.  
       **Safe casting** means converting an object to a specified type without causing an exception if the conversion fails. Instead of throwing an exception, safe casting will return a null reference.
- **Usage**: Returns the object cast to the specified type if the cast is valid; otherwise, it returns `null` without throwing an exception.
- **Example**:
    ```csharp
    object obj = "Hello, World!";
    string str = obj as string;

    if (str != null)
    {
        Console.WriteLine("The object was successfully cast to a string.");
    }
    else
    {
        Console.WriteLine("The object is not a string.");
    }
    ```

## What are **Downcasting** and **Upcasting**?
**Upcasting and downcasting** are concepts in object-oriented programming related to type conversion within an inheritance hierarchy. 

### Upcasting:
- **Definition**: Converting a derived class reference or object to its base class type.
- **Purpose**: Allows treating an instance of a derived class as an instance of its base class. This is useful for achieving polymorphism.
- **Casting Type**: Upcasting is safe and implicit (do not need explicit type casting).
- **In Practical terms**: In practical terms that means whenever a method requires an object of a given type as a parameter we can pass an object of that type or type that derives from that type.
  
- **Example**:
  ```csharp
  Dog myDog = new Dog();
  Animal myAnimal = myDog; // Upcasting to base class
  myAnimal.MakeSound(); // Can only access methods and properties available in Animal
  ```


### Downcasting:
- **Definition**: Converting a base class reference back to a derived class type.
- **Purpose**: Allows accessing members specific to the derived class after upcasting.
- **Casting Type**: Downcasting is explicit (Need explicit  casting)
- **In Practical terms**: In practical terms that means whenever a method requires an object of a given type as a parameter we can access members of a class derived from  this given type.

- **Example**:
  ```csharp
  Animal myAnimal = new Animal(); 
  Dog myDog = (Dog)myAnimal; // Downcasting to derived class
  myDog.Bark(); // Accesses method specific to Dog
  ```

### Note
Some time we can get an `InvalidCastException` if the object is not of the correct type. To avoid this, you can use the `is` or `as` operators as the following:

  ```csharp
  if (myAnimal is Dog)
  {
      Dog myDog = (Dog)myAnimal;
      myDog.Bark();
  }

  // Using 'as' operator
  Dog myDog = myAnimal as Dog;
  if (myDog != null)
  {
      myDog.Bark();
  }
  ```
## What is the **Method Signature**?
A **method signature** in C# refers to the unique identity of a method. It includes:

1. **Method Name**: The name of the method.
2. **Parameter List**: The number, types, and order of parameters.

### Important Points:
- **Return Type** is **not** part of the method signature.
- **Modifiers** like `static`, `virtual`, or `override` are also **not** part of the method signature.

## What is the **Method Overloading**?

Method overloading is a feature that allows a class to have multiple methods with the same name but different parameter lists (i.e., different type, number, or order of parameters). This allows methods to perform similar functions but with different types of input.

## What is the **Method Overriding**?

**Method overriding** is a feature that allows a subclass to provide a specific implementation for a method that is already defined in its base (parent) class. The method in the derived class must have the same signature as the method in the base class. This enables runtime polymorphism, where the method that gets called is determined at runtime based on the actual type of the object.

### Key Points:
- **Same Method Signature**: The overriding method must have the same name, return type, and parameters as the method in the base class.
- **`virtual` and `override` Keywords**:
  - The method in the base class must be marked with the `virtual` or `abstract` keyword.
  - The method in the derived class must be marked with the `override` keyword.
- **Run-time Polymorphism**: Method overriding is an example of run-time (or dynamic) polymorphism.

## What is the difference between **abstract class** and **interface**? 
| Feature                     | **Abstract Class**                                     | **Interface**                                    |
|-----------------------------|-------------------------------------------------------|--------------------------------------------------|
| **Declaration**              | `abstract` keyword                                   | `interface` keyword                              |
| **Methods**                  | Can have both method declaration and implementation parts | Can only have method declarations (C# 8.0+ allows default implementations) |
| **Fields/Properties**        | Can have fields, properties, and methods              | Cannot have fields (only properties and methods)  |
| **Multiple Inheritance**     | A class can inherit from only **one** class.| A class can implement multiple interfaces         |
| **Constructors**             | Can have constructors      | Cannot have constructors                          |
| **Access Modifiers**         | Members can have different types of access modifiers | All members are implicitly `public`               |
| **When to Use**              | Use when classes share common method implementations | Use when only behavior (no implementation) needs to be defined |
| **Example**                  | ```abstract class Animal { public abstract void Speak(); public void Eat() { } }``` | ```interface IAnimal { void Speak(); }``` |