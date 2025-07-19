# Language Integrated Query (LINQ)

- [What Is **Linq**?](#what-is-linq)
- [What is the difference between **IEnumerable** and **IQueryable**?](#what-is-the-difference-between-ienumerable-and-iqueryable)
- [What is the difference between **Deferred** and **Immediate** execution?](#what-is-the-difference-between-deferred-and-immediate-execution)
- [What are the two main syntax styles to write LINQ queries?](#what-are-the-two-main-syntax-styles-to-write-linq-queries)
- [What is the difference between **LINQ GroupBy** and **SQL GROUP BY**?](#what-is-the-difference-between-linq-groupby-and-sql-group-by)
- [What is the difference between **Any**, **All**, and **Contains**?](#what-is-the-difference-between-any-all-and-contains)
- [What is the difference between **Select** and **SelectMany**?](#what-is-the-difference-between-select-and-selectmany)
- [What is the difference between **First**, **FirstOrDefault**, **Single**, and **SingleOrDefault**?](#what-is-the-difference-between-first-firstordefault-single-and-singleordefault)
- [What is **ToLookup** in LINQ, and how is it different from **GroupBy**?](#what-is-tolookup-in-linq-and-how-is-it-different-from-groupby)
- [How do you perform multiple levels of sorting?](#how-do-you-perform-multiple-levels-of-sorting)
## What Is Linq?
**LINQ (Language Integrated Query)** a feature in .NET that allows querying data from various sources (e.g., collections, databases, XML) using a **uniform query syntax**.  

It eliminates the mismatch between the programming language and database providers by using **a uniform query syntax**.

The Result of Linq Query can be:  
1. **IEnumerable**  
2. **IQueryable**


## What is the difference between **IEnumerable** and **IQueryable**?
| **Feature**               | **IEnumerable**       | **IQueryable**                                      |
|---------------------------|-----------------------|-----------------------------------------------------|
| **Namespace**             | `System.Collections.Generic` | `System.Linq`            |
| **Execution**             | Deferred but always executed in-memory(client-side).   | Deferred but executed on the data source (e.g., SQL Server).(server-side). |
| **Query Processing**      | Processes the query in memory after retrieving all data.   | Translates queries into the data source's query language (e.g., SQL). |
| **Support Lazy Loading**  | No.   | Yes. |
| **Performance**           | Less efficient for filtering/operations on large datasets. | More efficient because filtering happens at the data source. |     
| **Purpose**               | In-memory data.  | For querying data from out-of-memory sources like databases. |
| **Use Case**              | Small or already-loaded data sets.                         | Large datasets from external data sources.          |                 |
| **Example**               | Iterating over an in-memory `List<T>` or `Array`.          | Querying a database using Entity Framework or LINQ to SQL. |

### Example
```csharp
   var coursesQuery = dbContext.Courses          
                         .AsEnumerable()    
                         .Where(c => c.Level == Level.Intermediate);

   var courses = coursesQuery.ToList();
```
#### Example breakdowns
- In this example the execution happens only when you call `.ToList()`;
- this part will run **on server side**  
  `dbContext.Courses` is translated into a query and executed on the database.   
  ```csharp
  dbContext.Courses
  ```
- this part will run **on client side** 
  ```csharp        
  .AsEnumerable()    
  .Where(c => c.Level == Level.Intermediate)
  ```

## What is the difference between **Deferred** and **Immediate** execution? 
| **Aspect**               | **Deferred Execution**                                      | **Immediate Execution**                                    |
|--------------------------|------------------------------------------------------------|----------------------------------------------------------|
| **Definition**            | Query execution is delayed until the query results are accessed (e.g., via `foreach`, `ToList()` or other enumerations).   | Query execution happens immediately upon invocation  (e.g., via `ToList`, `Count`, `First`, `Sum`)|            |
| **Performance Impact**    | Executes only when needed, allowing optimization.          | Executes immediately, even if the result is not used.    |
| **Usage Scenario**        | Suitable for large datasets or chaining operations.        | Used when you need the result immediately in memory.     |

### Important Note
As you see in previous comparison we can use methods like  `ToList()` in both **Deferred** and **Immediate** execution, the following 2 examples will explain how :

**Deferred Example**
```csharp
   var coursesQuery = dbContext.Courses          
                               .Where(c => c.Level == Level.Intermediate);
   // Do something here and after that run the following
   var courses = coursesQuery.ToList();
```
**Immediate Example**
```csharp
   var coursesQuery = dbContext.Courses          
                               .Where(c => c.Level == Level.Intermediate)
                               .ToList();
```
### In Summary
`ToList()` is the method that transitions from deferred to actual execution, but the difference lies in when it is called in the flow of the code.

## What are the two main syntax styles to write LINQ queries? 
In LINQ, there are **two main syntaxes** for writing queries:

### 1. **Query Syntax (Query Operators)**  
- SQL-like syntax.
- Uses keywords such as `from`, `where`, `select`, `group by`, `order by`, etc.
- Example:
  ```csharp
  var result = from user in users
               where user.Age > 18
               select user;
  ```

### 2. **Method Syntax (Extension Methods)** 
- Functional programming style. 
- Uses LINQ methods like `Where()`, `Select()`, `OrderBy()`, etc., which are extension methods in the `System.Linq` namespace.
- Often combined with lambda expressions.
- Example:
  ```csharp
  var result = users.Where(user => user.Age > 18).Select(user => user);
  ```

## What is the difference between **LINQ GroupBy** and **SQL GROUP BY**?
| **Aspect**              | **LINQ `GroupBy`**                                                                                   | **SQL `GROUP BY`**                                                         |
|-------------------------|-----------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| **Purpose**             | Groups data into collections of keys and values (key-value pairs).                                   | Groups data for aggregation (e.g., `SUM`, `COUNT`, etc.).                  |
| **Output**              | Produces a collection of grouped items (an `IGrouping<TKey, TElement>`).                             | Produces aggregated rows as flat data.                                     |
| **Aggregation**         | Aggregation happens **explicitly** (e.g., calling `.Count()`, `.Sum()`, etc., on the grouped items).  | Aggregation happens **implicitly** (e.g., `COUNT`, `SUM`, etc.).           |
| **Behavior**            | Returns full groups as objects, which you can further manipulate in C#.                              | Only returns aggregated values as defined in the query.                    |

### **Examples: LINQ vs. SQL**

#### 1. **Grouping Data Without Aggregation**
- **LINQ Example**: Groups items but does not aggregate. You can iterate over each group.

  ```csharp
   var students = new List<Student>
   {
       new Student { Name = "Alice", Grade = "A" },
       new Student { Name = "Bob", Grade = "B" },
       new Student { Name = "Charlie", Grade = "A" },
       new Student { Name = "Diana", Grade = "B" }
   };

  var groupedStudents = students.GroupBy(s => s.Grade);

  foreach (var group in groupedStudents)
  {
      Console.WriteLine($"Grade: {group.Key}");
      foreach (var student in group)
      {
          Console.WriteLine($" - {student.Name}");
      }
  }
  ```

  **Output:**
    ```
     Grade: A
      - Alice
      - Charlie
     Grade: B
      - Bob
      - Diana
    ```


#### 2. **Grouping Data With Aggregation**
- **LINQ Example**: Groups and aggregates explicitly.

  ```csharp
  var groupedCounts = students
       .GroupBy(s => s.Grade)
       .Select(g => new { Grade = g.Key, Count = g.Count() });

  foreach (var group in groupedCounts)
  {
    Console.WriteLine($"Grade: {group.Grade}, Count: {group.Count}");
  }
  ```

  **Output:**
  ```
   Grade: A, Count: 2
   Grade: B, Count: 2
  ```

- **SQL Equivalent**: 
  ```sql
  SELECT Grade, COUNT(*) AS Count
  FROM Students
  GROUP BY Grade;
  ```

  **Output:**
  ```
  Grade | Count
   A    |   2
   B    |   2
  ```

## What is the difference between **Any**,  **All** and **Contains**? 

| Feature                | `Any`                                    | `All`                                  | `Contains`                              |
|------------------------|-------------------------------------------|-----------------------------------------|-------------------------------------------|
| **Purpose**            | Checks if **any element** in a collection meets a **specified condition** or if the collection has **any elements**. | Checks if **all elements** in the collection **match** a condition. | Checks if a **specific value** is present in the collection. |
| **Returns**            | Returns **true** if **at least one element** matches the condition, otherwise **false**. | Returns **true** if **all elements** match the condition, otherwise **false**. | Returns **true** if the collection contains the **specified element**, otherwise **false**. |
| **Syntax**             | `collection.Any(predicate)` , `collection.Any()`             | `collection.All(predicate)`             | `collection.Contains(value)`              |
| **Example**            | `numbers.Any(n => n > 10)`                | `numbers.All(n => n > 10)`               | `numbers.Contains(5)`                     |


## What is the difference between  **Select** and **SelectMany**? 
| Feature            | `Select`                               | `SelectMany`                             |
|--------------------|------------------------------------------|--------------------------------------------|
| **Purpose**        | Projects each element into a new form, **keeping nested collections as separate**. | Projects each element into a new form and **flattens nested collections** into a single collection. |
| **Returns**        | A collection of **collections** if the result is a nested structure. | A **single flattened collection** of elements. |
| **Use Cases**      | Use when you want to **preserve the hierarchy** of collections. | Use when you need to **flatten** multiple collections into one. |
| **Example**        | `var nested = libraries.Select(l => l.Books); // Returns IEnumerable<IEnumerable<Book>> ` | `var flat = people.SelectMany(p => p.Books); // Returns IEnumerable<Book>` |


## What is the difference between **First**, **FirstOrDefault**, **Single**, and **SingleOrDefault**?
Sure! Here is the transposed version of the table:

| Feature                 | **`First`**                                      | **`FirstOrDefault`**                                   | **`Single`**                                      | **`SingleOrDefault`**                             |
|-------------------------|----------------------------------------------------|----------------------------------------------------------|----------------------------------------------------|-----------------------------------------------------|
| **Description**         | Returns the **first element** that matches the condition. | Returns the **first element** that matches the condition or **default value**  if no match. | Returns **the only element** that matches a specified condition. | Returns **the only element** that matches a specified condition or **default value** if no match. |
| **Behavior on No Match** | Throws an **exception**.                          | Returns **default value**.                                | Throws an **exception**.                            | Returns **default value**.                           |
| **Behavior on Multiple Matches** | Returns **first** matching element.            | Returns **first** matching element.                      | Throws an **exception**. | Throws an **exception** . |

## What is `ToLookup` in LINQ, and how is it different from `GroupBy`?
**ToLookup** and **GroupBy** in LINQ both group elements based on a key, but they differ in key ways:

| Feature              | **`ToLookup`**                                    | **`GroupBy`**                                      |
|-----------------------|-----------------------------------------------------|-----------------------------------------------------|
| **Execution**      | Immediate                                               | Deferred                                                |
| **Return Type**  | **`ILookup<TKey, TElement>`**.            |  **`IEnumerable<IGrouping<TKey, TElement>>`**. |
| **Mutability**          | Read-only                              | Allows query transformations |
| **Best For**            | Key-based access to grouped elements   | Complex grouping with additional processing |
| **Example Access**      | `lookupGroup['a']`                          | `groupByGroup.Where(g => g.Key == 'a')`|

### Examples
[ToLookupAndGroupBy](./RelatedDocuments/LINQ/ToLookupAndGroupBy.md)


## How do you perform multiple levels of sorting?
To perform multiple levels of sorting, you can chain `ThenBy` or `ThenByDescending` methods after OrderBy or OrderByDescending
```csharp
var sortedPeople = people.OrderBy(p => p.LastName)
                         .ThenBy(p => p.FirstName)
                         .ToList();
```