
# C# Concepts and Key Features



- [What is **C#** and how does it differ from **.NET**?](#what-is-c-and-how-does-it-differ-from-net)
- [What is the difference between **Value Type** and **Reference Type**?](#what-is-the-difference-between-value-type-and-reference-type)
- [Why does **string** in C# behave like a value type despite being a reference type?](#why-does-string-in-c-behave-like-a-value-type-despite-being-a-reference-type)
- [What is **Boxing** and **Unboxing**?](#what-is-boxing-and-unboxing)
- [What is the difference between **Passing By Value** and **Passing By Reference**?](#what-is-the-difference-between-passing-by-value-and-passing-by-reference)
- [What are the **out**, **ref**, and **in** keywords?](#what-are-the-out-ref-and-in-keywords)
- [What is the difference between **Struct** and **Class**?](#what-is-the-difference-between-struct-and-class)
- [What is the difference between **Constants** and **Readonly** variables?](#what-is-the-difference-between-constants-and-readonly-variables)
- [What is the difference between **Properties** and **Fields**?](#what-is-the-difference-between-properties-and-fields)
- [What are **Generics**, and why are they useful?](#what-are-generics-and-why-are-they-useful)
- [What is the difference between **String** and **StringBuilder**?](#what-is-the-difference-between-string-and-stringbuilder)
- [What is the **Lambda Expression**?](#what-is-the-lambda-expression)
- [What is the **Delegate**?](#what-is-the-delegate)
- [What are **Action**, **Func** and **Predicate**?](#what-are-action-func-and-predicate)
- [What are **Extension Methods**?](#what-are-extension-methods)
- [What is the **IDisposable** interface, and its purpose?](#what-is-the-idisposable-interface-and-its-purpose)
- [What is the purpose of the **using** statement?](#what-is-the-purpose-of-the-using-statement)
- [What is the difference between **++i** (Pre-Increment) and **i++** (Post-Increment)?](#what-is-the-difference-between-i-pre-increment-and-i-post-increment)
- [What is the difference between **dynamic** and **object**?](#what-is-the-difference-between-dynamic-and-object)
- [What is the **Indexer**?](#what-is-the-indexer)
- [What is the **params** modifier?](#what-is-the-params-modifier)
- [What is the **Tuple**?](#what-is-the-tuple)
- [What is the **var** keyword?](#what-is-the-var-keyword)
- [What is the **yield** keyword?](#what-is-the-yield-keyword)
- [What are **Foreground** and **Background** Threads?](#what-are-foreground-and-background-threads)
- [What is the **ThreadPool**?](#what-is-the-threadpool)
- [What are **async/await**?](#what-are-asyncawait)
- [What is the **Task**?](#what-is-the-task)
- [What is the difference between **Task.Run** and **Thread.Start**?](#what-is-the-difference-between-taskrun-and-threadstart)


## What is C# and how does it differ from .NET? 
C# is a **language** used to write programs, while .NET is a **framework** that provides the environment and tools needed to run those programs. 

.NET supports other languages such as VB.NET and F#. 
 

## What is the difference between **value type** and **reference type**?  

| **Feature**                     | **Value Type**                                                                                     | **Reference Type**                                                                                                    |
|----------------------------------|----------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **Memory Location**              | Typically stored on the **stack**. However, if a value type is a field inside a class, it is stored on the **heap** as part of the class instance. | The **object** itself is stored on the **heap**, while the **reference** (pointer to the object) is stored on the **stack** (for local variables) or on the **heap** (for fields inside objects). |
| **Copying Behavior**             | When assigned to another variable, a **copy of the value** is made, resulting in two independent copies. | When assigned to another variable, a **copy of the reference (pointer)** is made, meaning both variables point to the same object. |
| **Nullability**                  | Cannot be `null` unless explicitly made nullable (e.g., `int?`).                                    | Can be `null`, meaning it may not point to any object.                                                                  |
| **Default Initialization**       | Default values are assigned (e.g., `0`, `false`).                                                  | Default value is `null`, indicating it does not point to any object.                                                   |
| **Examples**                     | `int`, `float`, `bool`, `struct`, `enum`.                                                          | `class`, `string`, `array`, `interface`, `delegate`.                                                                  |

### Example
```csharp
class MemoryDemo
{
    // Value type field (Stored on the heap as part of the class instance)
    public int valueField; 

    // Reference type field (Stored on the heap)
    public string text; 

    // Reference type field pointing to another object (Stored on the heap)
    public Engine engine; 

    public void ShowMemoryLocations()
    {
        // Local value type (Stored on the stack)
        int localValue = 100; 

        // Local reference type (Reference is on the stack, object is on the heap)
        Person person = new Person { Name = "Mohamed" }; 
    }
}

```
### In summary:
- **Value types** are typically stored on the stack and hold data directly and create independent copies when assigned.  
- **Reference types** are stored on the **heap**, while the **reference** (pointer to the object) is stored on the **stack** (for local variables) or on the **heap** (for fields inside objects), assigning one variable to another shares the same object.
  

<p align="center">
    <img src="./RelatedDocuments/CSharp/Figures/ValueTypeVSRefType.PNG" alt="JIT">
</p>



## Why does **string** in C# behave like a value type despite being a reference type?
In C#, `string` is a **reference type**, but it behaves like a **value type** because it is **immutable**. This means any modification to a `string` creates a new instance rather than modifying the original one. This behavior makes it seem like a value type since operations on `string` do not affect the original instance.

### Example:
```csharp
string s1 = "Hello";
string s2 = s1;
s1 = "World";

Console.WriteLine(s2); // Output: Hello
```

## What is **Boxing** and **Unboxing**?
### **Boxing**:
- **Boxing** is the process of converting a **value type** (e.g., `int`, `bool`) into a **reference type** (i.e., `object` or an interface).
- Boxing occurs automatically (**implicit** cast).

### **Unboxing**:
- **Unboxing** is the process of converting a **reference type** (object) back to a **value type**.
- Requires an **explicit** cast.

### Example of Both:
```csharp
int num = 5; // value type
object obj = num; // boxing
int unboxedNum = (int)obj; // unboxing
```

## What is the difference between **Passing By Value** and **Passing By Reference**? 


| **Feature**                 | **Pass by Value**                                                           | **Pass by Reference**                                         |
| --------------------------- | --------------------------------------------------------------------------- | ------------------------------------------------------------- |
| **Behavior**                | A **copy** of the variable is passed                                        | A **reference** to the original variable is passed            |
| **Effect on Original Data** | Changes inside the method **do not affect** the original variable           | Changes inside the method **do affect** the original variable |
| **Usage**                   | Default for **value types** and **reference types** (but note nuance below) | Use `ref`, `out`, or `in` modifiers                           |
| **Example**                 | `void ModifyValue(int x)`                                                   | `void ModifyValue(ref int x)`                                 |

### 🔍 Clarification:

* In C#, **all arguments are passed by value by default**, whether they are **value types** (like `int`, `struct`) or **reference types** (like `class`).
* For **reference types**, the **reference itself** is passed **by value**, meaning the method receives a copy of the reference. So, changes to the object **are reflected outside**, but **reassigning** the reference inside the method does **not** affect the original.   
  ```csharp
  class Person
  {
      public string Name { get; set; }
  }

  class Program
  {
      static void ModifyPerson(Person p)
      {   
          /*
           This line modifies the state (Name property) of the object that 'p' points to.
           Since 'p' is a reference to the same object that 'person' (in Main) refers to,
           the change will be visible outside this method.           
          */          
          p.Name = "Alice";

          /*
            This line creates a new Person object in the heap with Name = "Bob".
            Then it changes the local reference variable 'p' to point to this new object.
            However, this reassignment affects only the local copy of the reference (p),
            not the original 'person' variable in Main. So the caller still points to the 
            original object.
          */         
          p = new Person { Name = "Bob" };
      }

      static void Main()
      {
          Person person = new Person { Name = "John" };

          ModifyPerson(person);

          Console.WriteLine(person.Name); // Output: Alice
      }
  }
  ``` 


## What are the **out**, **ref** and **in** keywords

In C#, the **out**, **ref**, and **in** keywords are used to pass arguments by reference to methods, but they have distinct behaviors


| Keyword | `ref`                                             | `out`                                        | `in`                                         |
|---------|--------------------------------------------------|---------------------------------------------|---------------------------------------------|
| Purpose | Passes by reference for both **read and write**  | Passes by reference for **output**         | Passes by reference for **read only**      |
| Condition | Must be initialized before being passed to the method. | Must be assigned a value in the method.    | Must be initialized before being passed to the method. |

[Ref, Out and In Examples](./RelatedDocuments/CSharp/RefOutIn.md)
            
## What is the difference between **struct** and **class**? 
| Feature               | **Class**                                               | **Struct**                                          |
|-----------------------|---------------------------------------------------------|-----------------------------------------------------|
| **Type**              | Reference type                                          | Value type                                          |
| **Inheritance**       | Supports inheritance                                    | Does not support inheritance                        |
| **Default Constructor** | Can have a custom parameterless constructor (if explicitly defined)| Cannot define custom parameterless constructors (compiler provides one that initializes all fields) |
| **Use Cases**         | Ideal for complex data and objects with many fields     | Best for small, lightweight data structures that don't require inheritance |



### Examples

#### Class
```csharp
public class Car
{
    public string Make { get; set; }
    public string Model { get; set; }
}
```

#### Struct
```csharp
public struct Point
{
    public int X { get; set; }
    public int Y { get; set; }
}
```



## What is the difference between **Constants** and **Readonly** variables?

| Feature               | **`const`**               | **`readonly`**      |
|-----------------------|--------------------------------------|------------------------------------------|
| **Initialization**    | 	At declaration only | At declaration or in constructor |
| **Modifiable**        | Not modifiable after initialization    | Not modifiable after initialization         |
| **Value Set Time**    | Compile-time                        | Compile-time or Runtime (usually in the constructor)    |
| **Declaration Inside Method**      | yes         | No     |
|**Using with static modifier**|No (because const is implicit Static)|Yes|
| **Usage**             | Fixed values like `PI`, `MaxValue`  | Values set once, such as configuration data |
| **Examples**          | `public const int Max = 100;`       | `public readonly int Max;`<br>`Max = value;` |

## What is the difference between **Properties** and **Fields**?
| Aspect | Properties | Fields |
|--------|------------|--------|
| Definition | A member that provides a mechanism to read, write, <br> or compute the value of a private Variables | A variable that holds data directly |
| Encapsulation | High – can include access rules, validation, and logic | Low – typically private and only stores data |
| Syntax | Use `get` and `set` accessors | Declared as variables within a class |
| Access Control | Can have different access levels for `get` and `set`<br>`public int Age { get; private set; }` | Single access modifier for the whole field<br>`private int age;` |
| Inheritance | Can be virtual and overridden | Cannot be overridden |
| Interfaces | Can be declared in interfaces | Cannot be declared in interfaces |

## What are generics and why are they useful?
- Generic means not specific to a particular data type.
- Allows you to define classes, fields, methods, or interfaces **with type parameters** and this in turn improve code reusability.

### Example
[Using Example](./RelatedDocuments/CSharp/Generic.md) 

## What is the difference between **string** and **StringBuilder**? 

| Feature                  | `string`                                   | `StringBuilder`                             |
|--------------------------|--------------------------------------------|--------------------------------------------|
| **Mutability**           | Immutable (Each modification creates a new string instance, which can lead to performance overhead) | Mutable (can be modified in place)        |
| **Performance**          | Less efficient for multiple modifications (creates new objects) | More efficient for frequent modifications  |
| **Memory Allocation**    | Creates new string instances on each change | Reuses memory, minimizing allocations       |
| **Usage Scenario**       | Best for Short strings, few modifications  | Best for Many modifications, especially in loops |
| **Example of Concatenation** | `result += "Hello";` (creates new string each time) | `sb.Append("Hello");` (appends without creating new objects) |


## What is the **lambda expression**?

A **lambda expression** in C# is a concise way to define anonymous functions (functions without a name) that can contain **expressions or statements**. It’s often used with **LINQ** and **delegates**, making code simpler and more readable.

**Key Points:**  
1. **Syntax**: The syntax of a lambda expression is   
    ```csharp
      (input parameters) => expression
      (input parameters) => { statements }
    ```
2. **Usage**: Commonly used for short, inline functions with LINQ, and delegates.
3. **Readability**: Provides a clear, shorthand way to define behavior, especially in collection processing like `.Where()`, `.Select()`, `.OrderBy()`, etc..

### Expression Lambda
Expression lambda contains a single expression after the `=>` symbol. It’s ideal for simple operations and directly returns the result of the expression.

```csharp
// Expression lambda to add two numbers
Func<int, int, int> add = (x, y) => x + y;
Console.WriteLine(add(3, 4)); // Output: 7
```

### Statement Lambda
```csharp
Func<int, int, int> multiplyOrAdd = (x, y) =>
{
    if (x > y)
        return x * y;
    else
        return x + y;
};
```

##  What is the **delegate**?
A delegate in C# **is a type that represents a reference to a method**.   
It allows methods to be passed as parameters.   
Delegates can point to methods with a **matching signature** and **return type**.

## What are **Action**, **Func** and **Predicate**?
In C#, `Action`, `Func`, and `Predicate` are delegate types provided by the language to encapsulate method references.

### **Action<T1, T2, ...>:**

- `Action` is a **generic delegate type** that represents a method with up to **16 input parameters** of types `T1, T2, ..., Tn`   
and **no return value (void)**.

```csharp
// Represents an action that takes two integers and prints their sum
Action<int, int> printSumAction = (a, b) => Console.WriteLine(a + b);
printSumAction(3, 5); // Prints 8
```

### **Func<T1, T2, ..., TResult>:**

- `Func` is a **generic delegate type** that represents a method with up to **16 input parameters** of types `T1, T2, ..., Tn`   
and **returns a result of type `TResult`**.

```csharp
// Represents a function that takes two integers and returns their sum
Func<int, int, int> addFunction = (a, b) => a + b;
int result = addFunction(3, 5); // return 8
```

### **Predicate <T>**

- `Predicate` is a **generic delegate type** that represents a method that takes **one input** parameter and **returns a Boolean value**.

- `Predicate<T>` is equivalent to `Func<T, bool>`.

```csharp
// Represents an action that takes an integer and checks even or not 
Predicate<int> isEven = x => x % 2 == 0;
isEven(2); //return true
```


## What are **extension methods**?
Extension methods in C# enable you to add methods to existing types without modifying the original type or creating a derived type.   

They enable a more fluent syntax for function composition.
### Syntax:
- Extension methods are declared as **`static` methods** in a **`static` class**.
- The first parameter in the method must include the **`this` keyword** followed by the **type to be extended**.


Here’s an example of how you can achieve this:  
[Extension Method Example](./RelatedDocuments/CSharp/ExtensionMethod.md)

## What is the **IDisposable** interface, and its purpose?
### Definition
 **`IDisposable`** is an interface used to **release unmanaged resources** in a **deterministic way**.  
#### **Definition Breakdown**  
**Unmanaged resources:** Resources not managed by the CLR, such as file handles, database connections, or memory allocated through native code.
**deterministic:** The **`Dispose`** method is called  **explicitly** to release resources when an object is no longer needed.


### Syntax
1. **Without Using**
   ```csharp
     var resource = new ResourceType();
     try
     {
        // Code that uses the resource
     }
     finally
     {
       resource.Dispose();
     }
    ```
  
2. **Wit Using**  
   **`using` statement** is used to ensure that `Dispose` is called automatically, improving code safety and readability.  

   ```csharp
      using (var resource = new ResourceType())
      {
       // Code that uses the resource
      }
       // The Dispose method is called automatically here
   ```

## What is the purpose of the **using** statement? 

The **`using` statement** in C# is primarily used for managing **resource disposal**. It ensures that **unmanaged resources**, such as file handles, network connections, or database connections, are properly released when they are no longer needed.   
The `using` statement provides a convenient way to work with objects that implement the **`IDisposable`** interface, automatically calling their `Dispose` method once the code block is exited, even if an exception occurs.

### **Purpose of `using` Statement**:
- **Automatic Resource Management**: Ensures that resources are properly disposed of without requiring explicit code to do so.
- **Simplifies Code**: Reduces boilerplate code for disposing of objects and handling exceptions.
- **Prevents Memory Leaks**: Guarantees that unmanaged resources are cleaned up, reducing the risk of memory leaks.
  

### **Syntax**:
```csharp
using (var resource = new ResourceType())
{
    // Code that uses the resource
}
// The Dispose method is called automatically here
```

In this context, **`ResourceType`** must implement the **`IDisposable`** interface. After the `using` block completes, the `Dispose` method is called automatically to release resources.

### Example
[Using Example](./RelatedDocuments/CSharp/Using.md) 


### **`using` Directive vs. `using` Statement**:
- **`using` Directive**: Used at the top of a C# file to **import namespaces** (e.g., `using System;`).
- **`using` Statement**: Used to **manage resources** within a block of code.

### Summary:
The `using` statement in C# is used for **automatic resource management**, ensuring that objects implementing the `IDisposable` interface are properly disposed of, helping to manage memory effectively and prevent resource leaks.


## What is the difference between **++i** (Pre-Increment) and **i++** (Post-Increment)

### 1. **Post-increment (`i++`)**  
The increment happens **after** the value is used in the expression.

#### Example:
```csharp
int i = 5;
int result = i++;  // result = 5, i becomes 6
```

### 2. **Pre-increment (`++i`)**
The increment happens **before** the value is used.

#### Example:
```csharp
int i = 5;
int result = ++i;  //  result = 6, i becomes 6,
```

## What is the difference between **dynamic**, and **object**?

In C#, both `dynamic `and `object` are used to represent types that can hold any data type, but they have key differences in behavior, usage, and purpose. Here’s a breakdown:



| Feature          | `dynamic`          | `object`                |
|------------------|--------------------|-------------------------|
| **Definition**                            | A type resolved entirely at **runtime**.                                                                   | The **base type** for all types in .NET.                                     |
| **Casting to retrieve the original type** | **Not required** — members and operations resolved at runtime.                                                 | **Required** — explicit cast needed to access type-specific members.                   |
| **Compile-Time Checking**                 | **No** compile-time type checking — errors occur at runtime.                                                   | **Yes** — compile-time checking ensures type safety.                                   |
| **Use Case**                              | When working with data whose type is unknown at compile time (e.g., COM, reflection, JSON, dynamic languages). | When storing any type in a general-purpose container with compile-time safety.         |
| **Example**                               | `dynamic name = "Mohamed";`<br> `var Length = name.Length;`<br>(Compiles, but may fail at runtime.)                           | `object name = "Mohamed";`<br> `var Length = ((string)name).Length;`<br>(Requires explicit cast to access string props.) |

### Key Notes:
- **`dynamic`** is flexible but risky, as runtime errors can occur if the operations are invalid for the resolved type.
- **`object`** is the base type of all .NET types, often used in scenarios requiring general-purpose storage but less convenient due to casting requirements.


## What is the Indexer? 
An indexer in C# is a **special type of property that allows an object to be indexed**  
in the same way as an array using square brackets [].  

### Benefits of Using Indexers
- **Simplifies Syntax:** Allows objects to be accessed using array-like syntax, making code cleaner and easier to read.
- **Improves Encapsulation:** Enables controlled access to internal data without exposing underlying data structures.
- **Supports Multiple Parameters:** Indexers can take multiple parameters, enabling multidimensional access patterns.

  

[Indexer Example](./RelatedDocuments/CSharp/Indexer.md) 

## What is the **params** modifier?
The `params` modifier in C# allows a method to accept a **variable number of arguments** as a **single array parameter**.   
It lets you pass **zero or more arguments** of a specified type without explicitly creating an array. 


[Params Example](./RelatedDocuments/CSharp/Params.md) 


### Benefits:
- You can call the method with **any number of arguments**, or even **none**.
- Removes the need to manually create an array when calling the method.

### Restrictions:
- The `params` parameter **must be the last parameter** in the method signature.
- Only **one** `params` parameter is allowed per method.


## What is the **tuple**?


A **tuple** is a convenient way to **group multiple values in a lightweight data structure without defining a new class**.  
Tuples are useful for **temporary data structures** and **methods that need to return multiple values**.


### Basic Tuple (Pre-C# 7.0)
```csharp
// Creating a tuple with two elements
var person = Tuple.Create("Mohamed", 30);
Console.WriteLine(person.Item1); // Output: John
Console.WriteLine(person.Item2); // Output: 30
```

### Named Tuple (C# 7.0+)
```csharp
// Creating a named tuple
var employee = (Name: "Ahmed", Age: 25, Position: "Developer");
Console.WriteLine(employee.Name);   // Output: Alice
Console.WriteLine(employee.Age);    // Output: 25
Console.WriteLine(employee.Position); // Output: Developer
```
## What is the **var** keyword? 
InC#, **`var`** is a keyword that lets you declare a **local variable without explicitly specifying its type**, the compiler infers the type from the assigned value.

### Key Features of `var`:
1. **Type Inference**:  
   The type of the variable is determined at **compile time** by the compiler based on the assigned value.  
   ```csharp
   var name = "John"; // Compiler infers type as string
   var age = 30;      // Compiler infers type as int
   ```

2. **Strongly Typed**:  
   Once type is inferred, the type cannot change.  
   ```csharp
   var number = 10;   // Compiler infers int
   number = "text";   // Error: Cannot assign a string to an int
   ```

3. **Local Scope Only**:  
   `var` can only be used for local variables (inside methods, loops, etc.), not for class fields or properties.  
   ```csharp
   var localVariable = 50; // Valid
   // var globalField;      // Invalid for class-level fields
   ```

4. **Cannot Be Null Without Explicit Type**:  
   You cannot declare `var` with a `null` value unless explicitly typed (e.g., `var name = (string)null;`).

5. **Readability and Simplicity**:  
   Often used in situations where the type is either **obvious** or **complex** (e.g., LINQ queries).  
   ```csharp
   var result = from x in list where x > 5 select x; // Complex types
   ```




### When to Use `var`
1. When the type is **obvious** from the context:
   ```csharp
   var name = "John";  // string is obvious
   ```

2. When working with **anonymous types**:
   ```csharp
   var person = new { Name = "John", Age = 30 }; // Anonymous type
   ```

3. When the type is **long or complex** (e.g., LINQ queries):
   ```csharp
   var results = from item in collection where item.Age > 18 select item;
   ```


### When to Avoid `var`:
1. When it **reduces readability** or the type isn't obvious:
   ```csharp
   var x = GetData();  // What type is GetData() returning?
   ```

2. When explicitly specifying the type improves clarity for future maintenance.

In summary, `var` simplifies code while retaining strong typing but should be used carefully to maintain readability and clarity.

## What is the **yield** keyword?
The `yield` keyword in C# is used to create iterator methods, which return sequences of values one at a time. These methods return `IEnumerable` or `IEnumerable<T>`and allow for efficient on-demand iteration.



### **How `yield` Works**
When you use `yield`, it pauses the execution of a method and remembers its state. The next time the enumerator is called, execution resumes from where it left off.



### **Key Features of `yield`:**
1. Simplifies code for generating sequences (e.g., custom iterators).
2. Defers execution until the values are actually iterated.
3. Reduces memory usage since the entire collection is not stored in memory at once.


### **When to Use `yield`:**
1. When generating large sequences lazily (e.g., reading lines from a file).
2. When you don’t need to store all items in memory at once.
3. When creating custom iteration logic.

### **`yield`** Examples
[Yield Example](./RelatedDocuments/CSharp/Yield.md) 

### **Advantages of `yield`:**
1. **Simpler Code:** Eliminates the need for manual implementation of enumerators.
2. **Lazy Evaluation:** Only generates the next value when needed.
3. **Efficient Memory Usage:** Does not allocate memory for the entire collection upfront.


### **Disadvantages of `yield`:**
1. Debugging can be slightly harder because of deferred execution.
2. State management within the iterator can be complex if the method has many variables.


## What are **Foreground** and **Background** Threads?
### Foreground threads
In C#, **foreground threads** keep the application running until they finish execution.  
The application will not terminate as long as any foreground thread is still running.

#### Key Points:
1. **Thread Behavior**: Foreground threads are essential to the application's lifecycle—they keep it alive.
2. **Termination**: The application exits **only after all** foreground threads complete.
3. **Use Case**: Used for critical tasks that must finish before the app exits (e.g., main logic, UI).

#### Example:
```csharp
Thread foregroundThread = new Thread(() =>
{
    // Some foreground work
    Console.WriteLine("Foreground thread running");
});
foregroundThread.IsBackground = false; // Default is foreground thread
foregroundThread.Start();
```
As long as this thread is running, the application will not exit.
### Background threads

**Background threads** do **not** prevent the application from terminating.  
They are often used for tasks like I/O or background processing that aren’t essential to app shutdown.


#### Key Points:

1. **Thread Behavior**: Run in the background, don’t block app termination.
2. **Termination**: Automatically stopped when all foreground threads finish.
3. **Use Case**: Ideal for non-critical tasks that can be abandoned if the app exits.

#### Example:
```csharp
Thread backgroundThread = new Thread(() =>
{
    // Some background work
    Console.WriteLine("Background thread running");
});
backgroundThread.IsBackground = true; // Set the thread as background
backgroundThread.Start();
```
This thread will stop automatically when all foreground threads have finished.
## What is the **ThreadPool**?

The **ThreadPool** in C# is a pool of **managed worker threads** provided by the runtime to execute tasks concurrently.  
Instead of creating a new thread for each task, the ThreadPool **reuses existing threads**, improving performance and reducing resource overhead.

### Key Points:

- **Thread Type**: ThreadPool threads are typically **background threads**, so they don’t block application termination.
- **Limitations**: The pool has a **maximum number of threads**, which the runtime can adjust dynamically
- **Automatic Management**: The .NET runtime manages the pool creating, reusing, and releasing threads based on demand.


### How it Works:

1. **Task Queue**: Submitted tasks are queued.
2. **Worker Threads**: Worker threads are Idle threads that pick up tasks from the queue and execute them.
3. **Thread Reuse**: After a task completes, the thread returns to the pool for reuse.
4. **Dynamic Scaling**: The runtime adjusts the pool size based on workload and system resources.


✅ This efficient way of handling tasks in a multithreaded environment helps reduce resource consumption and improves application responsiveness.

### Example:
```csharp
using System;
using System.Threading;

class Program
{
    static void Main()
    {
        // Queue a task to the ThreadPool
        ThreadPool.QueueUserWorkItem(TaskMethod);
        
        // Wait for a key press to prevent immediate exit
        Console.ReadLine();
    }

    static void TaskMethod(object state)
    {
        Console.WriteLine("Task is being executed by a thread from the ThreadPool.");
    }
}
```

In the above example:
- `ThreadPool.QueueUserWorkItem` places a task in the ThreadPool queue.
- A worker thread from the pool picks up the task and executes it.

## What are **async/await**?
`async` and `await` are keywords in C# used to write **asynchronous** code.  
They enable **non-blocking operations**, allowing the program to continue executing without waiting for a long-running task to complete.

`async`: Marks a method as **asynchronous** and indicates that it can contain `await` expressions.

`await`: Pauses the execution of the `async` method until the awaited `Task` completes, ****without blocking the main thread**.*.

### Example
[async/await Example](./RelatedDocuments/CSharp/AsyncAwait/AsyncAwait.md) 

## What is the **Task**?
- In C#, a **Task** represents an **asynchronous** operation.   
- It is used to run code in the background and allows for **non-blocking** execution. 
- The `Task` class is part of the **System.Threading.Tasks** namespace and is commonly used with the `async` and `await` keywords to handle asynchronous methods. 

### Task Types:
1. **`Task`**: Represents an operation with no return value (like void).
2. **`Task<T>`**: Represents an operation that returns a result of type `T`.

### Task Methods:
- `Task.Run(task)`: Starts a new task on a thread pool thread to run code asynchronously in the background
- `Task.WhenAny(tasks)`: Completes when **any** task completes.
- `Task.WhenAll(tasks)`: Completes when **all** tasks complete.
- `Task.Delay(time)`: Creates a task that completes after a delay — useful for simulating timeouts or pauses.

### Task Status:
- `Created`: The task has been initialized but not yet started.
- `WaitingToRun`: The task is scheduled and waiting to be assigned to a thread.
- `Running`: The task is currently executing.
- `RanToCompletion`: The task has finished successfully.
- `Faulted`: The task has thrown an exception and failed.
- `Canceled`: The task was canceled before it completed.


### Example of Task:
```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        // Task: No return value
        Task task1 = Task.Run(() =>
        {
            Console.WriteLine("Task 1 running...");
        });

        // Task<T>: Returns a result
        Task<int> task2 = Task.Run(() =>
        {
            Console.WriteLine("Task 2 running...");
            return 42;
        });

        // Delay
        await Task.Delay(1000); // wait for 1 second

        // WhenAny: Completes when the first task finishes
        Task firstFinished = await Task.WhenAny(task1, task2);
        Console.WriteLine("First task finished");

        // WhenAll: Waits for all tasks to complete
        await Task.WhenAll(task1, task2);
        Console.WriteLine($"All tasks completed. Result from task2: {task2.Result}");
    }
}
```


## What is the difference between **Task.Run** and **Thread.Start**?

| **Aspect**            | **Task.Run**                                                                 | **Thread.Start**                                                   |
|------------------------|------------------------------------------------------------------------------|----------------------------------------------------------------------|
| **Abstraction Level**  | High-level abstraction (part of Task Parallel Library - TPL).                | Low-level construct for directly managing threads.                  |
| **Ease of Use**        | Easier to use, integrates well with `async/await`.                          | Requires manual thread management.                                  |
| **Performance**        | Uses Thread Pool (efficient thread reuse).                                  | Creates a new OS thread each time (more resource-intensive).         |
| **Return Type**        | Returns a `Task`, supports results, and exception handling.        | Returns `void`, no direct support for results or exceptions.         |
| **Use Cases**          | Best for CPU-bound or asynchronous work, with chaining or awaiting results. | Suitable for long-running operations or low-level thread control.    |
| **Thread Management**  | Managed by the runtime, no need to manually manage lifecycle.                | Requires explicit thread lifecycle management.                       |

### Example Code:

| **Aspect**              | **Example Code**                                                                                                          |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------|
| **Task.Run**            | `await Task.Run(() => { Console.WriteLine("Running with Task"); });`                                                      |
| **Thread.Start**        | `var thread = new Thread(() => { Console.WriteLine("Running with Thread"); }); thread.Start();`                           |