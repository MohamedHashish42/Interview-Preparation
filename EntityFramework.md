
# Entity Framework 


- [What is **ORM (Object-Relational Mapper)**?](#what-is-orm-object-relational-mapper)
- [What is **Entity Framework**?](#what-is-entity-framework)
- [What Are the Main Approaches **(Code First, Database First, and Model First)** to Using Entity Framework?](#what-are-the-main-approaches-code-first-database-first-and-model-first-to-using-entity-framework)
- [What is **DbContext** in Entity Framework?](#what-is-dbcontext-in-entity-framework)
- [How to Override Configuration in Entity Framework?](#how-to-override-configuration-in-entity-framework)
- [What Are Delete Behaviors (Cascade, Restrict, SetNull, NoAction) in EF Core?](#what-are-delete-behaviors-cascade-restrict-setnull-noaction-in-ef-core)
- [What Are **Lazy Loading, Eager Loading, and Explicit Loading**?](#what-are-lazy-loading-eager-loading-and-explicit-loading)
- [What is the **N+1** Problem in EF, and How Do You Mitigate It?](#what-is-the-n1-problem-in-ef-and-how-do-you-mitigate-it)
- [What is the Difference Between **AsNoTracking** and **Normal Tracking** Queries in EF?](#what-is-the-difference-between-asnotracking-and-normal-tracking-queries-in-ef)
- [What is the Difference Between **SaveChanges** and **SaveChangesAsync**?](#what-is-the-difference-between-savechanges-and-savechangesasync)
- [What is **Migrations**?](#what-is-migrations)
- [What is **Transaction** How Does Entity Framework Implement **it**?](#what-is-transaction-how-does-entity-framework-implement-it)
- [What is **TransactionScopeOption** in Entity Framework?](#what-is-transactionscopeoption-in-entity-framework)
- [What is **IsolationLevel** in Entity Framework?](#what-is-isolationlevel-in-entity-framework)
- [What Are **Table-Per-Hierarchy (TPH)**, **Table-Per-Type (TPT)**, and **Table-Per-Concrete Class (TPC)** Inheritance Strategies in EF?](#what-are-table-per-hierarchy-tph-table-per-type-tpt-and-table-per-concrete-class-tpc-inheritance-strategies-in-ef)
- [What Are **Split Queries**, and Why Are They Useful in EF Core?](#what-are-split-queries-and-why-are-they-useful-in-ef-core)
- [What Are **Global Query Filters** in EF Core, and How Are They Used?](#what-are-global-query-filters-in-ef-core-and-how-are-they-used)



## What Is **ORM (Object-Relational Mapper)**?
An **ORM (Object-Relational Mapper)** is a programming technique used to interact with relational databases through object-oriented programming. It bridges the **gap** between the object-oriented world (classes, inheritance, associations) and the relational world (tables, rows, foreign keys), allowing developers to work with database data as objects without writing raw SQL queries.

## What Is **Entity-Framework**?
**Entity Framework (EF)** is an open-source **ORM (Object-Relational Mapper)** framework for .NET applications supported by Microsoft. It enables developers to work with data using objects of domain specific classes without focusing on the underlying database.


### Entity Framework Features
- **Cross-platform**:   
  EF Core is a cross-platform framework which can run on Windows, Linux and Mac.

- **Transactions**:   
  EF performs automatic transaction management while querying or saving data. It also provides options to customize transaction management.

- **Querying**:    
  EF supports LINQ queries and translates them into database-specific languages (e.g., SQL). Raw SQL queries are also allowed.

- **Change Tracking**:  
  EF keeps track of changes occurred to instances of your entities (Property values) which need to be submitted to the database.

- **Caching**:   
  EF includes **first-level caching** (also called the identity map) by default. Within the same `DbContext` instance, if you query an entity by its **primary key** and then query it again, EF returns the already tracked entity from memory instead of sending another query to the database. 
**For more detail** [Caching](./RelatedDocuments/EntityFramework/Caching.md)

- **Migrations**:
   EF supports database schema management with migration commands via NuGet Package Manager Console or CLI.

- **Configuration**:   
  Entity Framework (EF) follows a **conventions over configuration** approach, where it automatically applies a set of default rules to configure the model. However, developers can override these conventions using **data annotations** or the **Fluent API** to customize the model's configuration according to specific requirements.


## What are the main approaches **(Code First, Database First, and Model First)** to using Entity Framework?
The main approaches to using Entity Framework are:

1. **Code-First**:  
   - Developers define classes (entities) in code, and Entity Framework generates the database schema based on these classes.
   - Ideal for projects starting without a predefined database.  
   - Allows customization of the database using **Fluent API** or **Data Annotations**.  

2. **Database-First**:  
   - The database is designed first, and EF generates the corresponding entity classes and DbContext based on the existing schema.  
   - Suitable for projects with an already existing database.  

3. **Model-First**:  
   - Developers create a visual Entity Data Model (EDM) using the EF Designer in Visual Studio, and EF generates both the database schema and code.  
   - Ideal for developers who prefer a graphical approach to designing the data model.  
   - ⚠️ **Note**: Model-First is only supported in **EF 6** (the classic version). **EF Core does not support it** — EF Core only supports Code-First and Database-First approaches.

## What is **DbContext** in Entity Framework?

**DbContext** is the primary class in Entity Framework that acts as a **bridge** between your application and the database. It is responsible for:

1. **Database Connection Management** – Handles the connection to the database.
2. **CRUD Operations** – Enables querying, inserting, updating, and deleting data using LINQ.
3. **Change Tracking** – Tracks changes made to entities and saves them to the database when `SaveChanges()` is called.
4. **Model Configuration** – Configures entity mappings, relationships, and constraints using Fluent API or data annotations.
5. **Unit of Work Behavior** – Coordinates changes across multiple entities in a single save operation.

Typically, a custom context class inherits from `DbContext`:

```csharp
public class AppDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }
    public DbSet<Order> Orders { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer("YourConnectionString");
    }
}
```

This context allows your application to interact with the `Products` and `Orders` tables in the database.

## How to override configuration in entity framework?
In Entity Framework, you can override configuration using:

### 1. **Fluent API**
- Defined in the `OnModelCreating` method of your `DbContext`.
- Provides greater flexibility and is suitable for complex configurations.
- Example:
  ```csharp
  protected override void OnModelCreating(ModelBuilder modelBuilder)
  {
      modelBuilder.Entity<Employee>()
          .Property(e => e.Name)
          .HasMaxLength(100)
          .IsRequired();
  }
  ```

### 2. **Data Annotations**
- Defined using attributes directly on entity classes or properties.
- Simpler and more declarative but less flexible.
- Example:
  ```csharp
  public class Employee
  {
      [MaxLength(100)]
      [Required]
      public string Name { get; set; }
  }
  ```

### Key Differences:
- **Flexibility**: Fluent API offers more customization options than Data Annotations.
- **Separation of Concerns**: Fluent API keeps configuration separate from the entity class, while Data Annotations mix configuration with the entity code.
- **Complexity**: Data Annotations are easier for simple configurations, but Fluent API is better for advanced scenarios.

### Conclusion
- No matter which approach you choose for overriding conventions (Fluent API or Data Annotations), just stick to that and don't mix them, Entity framework doesn't stop you from mixing but it complicates your maintenance because every time you want to check a configuration you have to look in two places (class definition & DbContext)

- If you're working on an enterprise application it's better to just stick to Fluent API, we will have a little bit more code but you have better separation and you can configure all kind of relationships.


## What Are **Delete Behaviors (Cascade, Restrict, SetNull, NoAction)** in EF Core?

**Delete Behavior** in EF Core defines what happens to **dependent (child) entities** in the database when their **principal (parent) entity** is deleted. It controls how EF handles the foreign key relationship on delete.


| Delete Behavior | Database FK Constraint | EF Change Tracker Action |
|-----------------|------------------------|--------------------------|
| **Cascade**     | `ON DELETE CASCADE`    | Deletes children automatically |
| **Restrict**    | `ON DELETE RESTRICT`   | Throws exception if children exist |
| **SetNull**     | `ON DELETE SET NULL`   | Sets FK to `NULL` on children |
| **NoAction**    | `ON DELETE NO ACTION`  | Does nothing deletion may fail at DB save time due to FK constraint <br> or succeed if no related rows exist|

### 💡 The key difference (Restrict vs NoAction)

* **Restrict** = “You are not allowed to start deleting this” 🚫
* **NoAction** = “Go ahead… I’ll check later and fail you if needed” ⏳


## What are **Lazy Loading, Eager Loading and Explicit Loading**?
**Lazy Loading**, **Eager Loading**, and **Explicit Loading** are techniques used in Entity Framework **to load related data (navigation properties)**.  
Here's a comparison:



### **1. Lazy Loading**
- **Definition**: Related data is automatically loaded from the database **only when it is accessed** for the first time.
- **Mechanism**: Proxy classes are used to intercept property access and trigger a query to load the data.
- **Pros**: Efficient if related data is not always needed, as it minimizes initial queries.
- **Cons**: Can lead to the **N+1 query problem** if accessing many related entities in a loop.
- **Example**:
  ```csharp
  var customer = context.Customers.First();  // Loads only Customer
  var orders = customer.Orders;  // Loads Orders when accessed
  ```
#### Rules To Apply

1. **Enable Lazy Loading in Your `DbContext`**:
   ```csharp
   public class MyDbContext : DbContext
   {
      public DbSet<MyEntity> MyEntities { get; set; }

      protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
      {
            optionsBuilder.UseLazyLoadingProxies();
      }
   }
2. **Make Navigation Properties Virtual**:
  In order to enable lazy loading, navigation properties need to be marked as `virtual`. This allows Entity Framework to create a proxy for the entity to intercept the navigation and load it lazily.

   ```csharp
   public class MyEntity
   {
      public int Id { get; set; }
         
      // Navigation property marked as virtual for lazy loading
       public virtual ICollection<RelatedEntity> RelatedEntities { get; set; }
   }
   ```


### **2. Eager Loading**
- **Definition**: Related data is loaded **immediately** along with the main entity in a **single query**.
- **Mechanism**: Achieved using the `Include()` method.
- **Pros**: Reduces the number of queries, as all required data is fetched at once.
- **Cons**: May fetch more data than necessary if some relationships are not used.
- **Example**:
  ```csharp
  var customers = context.Customers
                         .Include(c => c.Orders)
                         .ToList();  // Loads Customers and related Orders
  ```

---

### **3. Explicit Loading**
- **Definition**: Related data is loaded **manually** on demand by explicitly calling the `Load()` method.
- **Mechanism**: Requires explicitly triggering queries for related data using `Load()`.
- **Pros**: Provides fine-grained control over what data is loaded.
- **Cons**: Requires more code and manual management.
- **Example**:
  ```csharp
  var customer = context.Customers.First();  // Loads only Customer

  // Explicit Loading (Manually loads Orders)
  context.Entry(customer).Collection(c => c.Orders).Load();  
  //Or
  context.Orders.Where(o => o.CustomerId == customer.Id).Load(); 
  ```


### Summary Table:

| Feature           | Lazy Loading                    | Eager Loading                  | Explicit Loading              |
|--------------------|----------------------------------|---------------------------------|--------------------------------|
| **When Loaded**    | When accessed                  | At query time                  | Manually triggered            |
| **Query Count**    | Multiple queries (on demand)   | Single query                   | Multiple queries (manual)     |
| **Control**        | Automatic                      | Predefined at query time       | Full manual control           |
| **Performance**    | May cause N+1 query problem    | Efficient if data is needed    | Efficient for selective data  |

## What is the **N+1** problem in EF, and how do you mitigate it?

The **N+1 problem** in Entity Framework (EF) occurs when a query retrieves an entity and then executes **N additional queries** to load related data for each entity. This typically happens with **Lazy Loading** when iterating over a collection of entities with related navigation properties.



### Example of the N+1 Problem:
```csharp
var customers = context.Customers.ToList();  // (1 query) to load all customers
foreach (var customer in customers)
{
    var orders = customer.Orders; // Executes an additional query for each customer (N queries)
}
```
- **Total Queries**: 1 query for `Customers` + N queries for `Orders`.


### Impact:
- **Performance Issues**: Multiple database queries (N+1 queries) increase response time and database load, especially with large datasets.

### Mitigation Techniques:

1. **Eager Loading**:
   - Use the `Include()` method to load related data in a **single query**.
   - Example:
     ```csharp
     var customers = context.Customers.Include(c => c.Orders).ToList();  // Single query for Customers + Orders
     ```

2. **Explicit Loading**:
   - Manually load related data for a specific entity when needed, rather than triggering N lazy loads in a loop.

## What is the difference between **AsNoTracking** and **Normal Tracking** queries in EF?
The difference between `AsNoTracking` and normal tracking queries in Entity Framework (EF) lies in how EF manages the state of the retrieved entities in its **change tracker**.


### **Normal Tracking Queries**
- **Definition**: By default, EF **tracks the state** of retrieved entities in the **DbContext**. This allows it to detect changes and automatically update the database when `SaveChanges()` is called.
- **Use Case**: When you need to modify entities and save changes back to the database.

**Example**:
```csharp
var product = context.Products.First();  // Entity is tracked
product.Price = 20;  // Change is tracked
context.SaveChanges();  // EF generates an UPDATE query
```

- **Performance Impact**: 
  - Tracking adds overhead, as EF keeps all retrieved entities in memory and monitors their changes.
  - Suitable when you need to update or delete data.

---

### **`AsNoTracking` Queries**
- **Definition**: Disables tracking for the queried entities. EF does not monitor changes or keep these entities in memory.
- **Use Case**: Use `AsNoTracking` when you only need to **read data** (e.g., for reporting or displaying in the UI) and don’t need to modify or save it.

**Example**:
```csharp
var products = context.Products.AsNoTracking().ToList();  // No tracking
```

- **Behavior**:
  - Changes made to the entity after retrieval are not tracked.
  - `SaveChanges()` will not persist modifications unless the entity is manually attached to the `DbContext`.

**Performance Impact**:
  - Improves query performance, especially for large datasets, as EF skips the overhead of tracking.


### **Key Differences**:

| Feature                | Normal Tracking Queries                  | `AsNoTracking` Queries                |
|------------------------|------------------------------------------|---------------------------------------|
| **State Tracking**     | Tracks changes to entities.             | No tracking; entities are detached.  |
| **Performance**        | Slightly slower due to tracking overhead.| Faster as tracking is skipped.       |
| **Use Case**           | Modify and save changes to entities.     | Read-only operations (e.g., reporting).|
| **Memory Usage**       | Higher, as entities are stored in memory.| Lower, as entities are not tracked.  |

## What is the difference between **SaveChanges** and **SaveChangesAsync**?

### **Difference Between `SaveChanges` and `SaveChangesAsync`**

| **Aspect**              | **`SaveChanges`**                         | **`SaveChangesAsync`**                     |
|-------------------------|-------------------------------------------|--------------------------------------------|
| **Definition**           | A synchronous method that saves changes to the database. | An asynchronous method that saves changes to the database. |
| **Thread Blocking**      | Blocks the calling thread until the operation completes. | Does not block the calling thread; allows it to perform other tasks while waiting for the operation to complete. |
| **Use Case**             | Use in scenarios where the application does not require asynchronous execution, such as console apps or non-scalable tasks. | Use in scenarios where scalability and responsiveness are important, such as web APIs or GUI applications. |
| **Performance**          | Suitable for simple, single-threaded operations. | Improves scalability by freeing up threads during database I/O operations. |
| **Syntax**               | No `await` keyword is required.          | Requires `await` keyword with an `async` method. |

---

### **Examples**

#### **1. Using `SaveChanges`**
```csharp
using (var context = new AppDbContext())
{
    context.Users.Add(new User { Name = "Alice" });
    context.SaveChanges();  // Synchronous
}
```

- Blocks the thread until the changes are saved to the database.
- Typically used in applications where scalability is not a concern.

---

#### **2. Using `SaveChangesAsync`**
```csharp
using (var context = new AppDbContext())
{
    context.Users.Add(new User { Name = "Bob" });
    await context.SaveChangesAsync();  // Asynchronous
}
```

- Non-blocking, allows other tasks to run while waiting for the operation to complete.
- Recommended for modern applications like web APIs or desktop apps where responsiveness is critical. 
 
### **Important Note**

At first glance, using `await` with `SaveChangesAsync()` may seem similar to synchronous code because execution pauses until the save operation finishes. However, the key difference is that the thread is **not blocked** while waiting.

#### How `SaveChangesAsync()` Works with `await`

1. **Calling `SaveChangesAsync()`**

   When `SaveChangesAsync()` is called, Entity Framework starts saving the changes to the database.

   Since database communication is an I/O operation that may take time, the method does not keep the current thread busy while waiting for the database response.

2. **What `await` Does**

   The `await` keyword pauses the execution of the current method until the operation completes.

   However, instead of blocking the thread, the runtime frees it to handle 
   other work (such as processing additional requests in a web application) 
   while waiting for the database response.

3. **When the Operation Finishes**

   Once the database operation completes, execution resumes automatically from the line after `await`.

   This makes the code appear sequential, even though the thread was free during the waiting period.

#### Example

```csharp 
await context.SaveChangesAsync();

Console.WriteLine("Changes saved");
```

In this example:

* `"Changes saved"` will not execute until the save operation finishes.
* But the thread itself is not blocked while waiting for the database response.

#### Key Difference

* `SaveChanges()` blocks the thread until the operation completes.
* `SaveChangesAsync()` with `await` pauses the method without blocking the thread.

That is why asynchronous code improves scalability and responsiveness, especially in web applications.

## What is **Migrations**?
**Migrations** in Entity Framework are a feature that automatically tracks and applies changes to the database schema, keeping it in sync with the evolving data model.

### **Generated Migration File**:
A migration file contains:
1. **`Up` Method**: Defines the changes to apply (e.g., adding a table or column).
2. **`Down` Method**: Defines how to revert the changes (e.g., dropping a table or column).

### **Benefits of Using Migrations**:
1. **Automated Schema Changes**: Simplifies database updates as the model evolves.
2. **Consistency**: Ensures all developers and environments have the same schema.
3. **Version Control**: Provides a history of schema changes for auditing or rollback.
4. **Customizable**: You can modify the generated migration scripts for specific requirements.

### **Common Commands in EF Core Migrations**:
| Command                              | Description                                           |
|--------------------------------------|-------------------------------------------------------|
| `dotnet ef migrations add <name>`    | Adds a new migration based on model changes.          |
| `dotnet ef database update`          | Updates the database to the latest migration.         |
| `dotnet ef migrations remove`        | Removes the last migration (if not applied to the DB).|
| `dotnet ef migrations list`          | Lists all available migrations.                      |
| `dotnet ef database update <name>`   | Updates the database to a specific migration.         |

## What is **Transaction** How Does Entity Framework Implement **it**?

Entity Framework Transactions ensure that a series of database operations are executed as a single unit of work. If any operation fails, the entire transaction is rolled back to maintain data integrity.   
 
Transactions can be managed automatically by Entity Framework or explicitly using `DbContext.Database.BeginTransaction()`.

**For more detail** [How does Entity Framework implement **Transactions**](./RelatedDocuments/EntityFramework/How-Does-Entity-Framework-Implement-Transactions.md)


## What is **TransactionScopeOption** in Entity Framework?

**`TransactionScopeOption`** is an enum in .NET that determines whether a `TransactionScope` joins an existing transaction, 
creates a new one, or suppresses transactions. It controls transaction participation when using `TransactionScope`.  

**For more detail** [TransactionScopeOption](./RelatedDocuments/EntityFramework/TransactionScopeOption.md)

## What is **IsolationLevel** in Entity Framework?
**IsolationLevel** in Entity Framework determines how transactions interact with each other by controlling how data is locked and read during concurrent operations. It defines the level of isolation from other transactions to prevent issues like dirty reads, non-repeatable reads, and phantom reads.  

**For more detail** [IsolationLevel](./RelatedDocuments/EntityFramework/IsolationLevel.md)


## What are **Table-Per-Hierarchy (TPH)**, **Table-Per-Type (TPT)** and **Table-Per-Concrete Class (TPC)** inheritance strategies in EF?
Entity Framework supports different inheritance strategies for mapping object-oriented classes to relational database tables. The 3 main strategies are **Table-Per-Hierarchy (TPH)**, **Table-Per-Type (TPT)** and **Table-Per-Concrete Class (TPC)**.

1. **Table-Per-Hierarchy (TPH) – Default**  
In TPH, a **single database table** is used to store data for all classes in the inheritance hierarchy.

2. **Table-Per-Type (TPT)**  
In TPT, each class in the inheritance hierarchy is mapped to a **separate table** in the database **which contains only the properties of that class and is linked via primary/foreign keys to its ancestors**.

3. **Table-Per-Concrete Class (TPC)**     
In TPC, each concrete class in the inheritance hierarchy is mapped to its **own table** in the database **which contains all properties of the class and its ancestors**.  


**For more detail** [Inheritance Strategies](./RelatedDocuments/EntityFramework/InheritanceStrategies.md)


## What are **Split Queries**, and Why Are They Useful in EF Core?

**Split queries** fetch related data using multiple queries instead of a single query with joins. This approach can improve performance and reduce data redundancy for complex relationships.

### **How Split Queries Work**

* **Default Behavior**: EF Core typically uses **single queries** with joins for `Include` operations. For complex relationships, this can produce large result sets due to Cartesian explosion.
* **Split Queries**: EF Core executes separate queries for each part of the data. For example, for an `Order` entity with related `OrderItems`:

  1. Fetch all `Orders` in one query.
  2. Fetch all `OrderItems` in another query.

> ✅ Result: Each `Order` appears only once in the first query, so **data is not repeated** for each related `OrderItem`.

### **Why Split Queries Are Useful**

1. **Avoids Cartesian Explosion** – Reduces redundant rows when fetching one-to-many or many-to-many relationships.
2. **Improved Performance** – Smaller, focused queries can be faster than one large query for big datasets.

### **How to Use Split Queries**

Enable split queries per query using `AsSplitQuery()`:

```csharp
var orders = await context.Orders
    .Include(o => o.OrderItems)
    .AsSplitQuery()
    .ToListAsync();
```

Or configure globally in `OnModelCreating`:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.UseQuerySplittingBehavior(QuerySplittingBehavior.SplitQuery);
}
```

### **When to Use Split Queries**
* Large result sets caused by joins.
* High-cardinality relationships.
  
### **Trade-offs**

**Pros**:
* Improves performance for High-cardinality relationships.

**Cons**:
* Multiple database round trips may increase latency.
* Less beneficial for small or simple queries.

## What are **global query filters** in EF Core, and how are they used?

**Global query filters** are filters applied at the **model level** to automatically include specific filtering logic in all queries for a given entity. These filters are particularly useful for implementing features like **soft deletes**, **multi-tenancy**.


### **Key Points**
1. **Applied Globally**:
   - Once defined, the filter is applied automatically to all queries for the entity (e.g., `DbSet` queries, relationships, and `Include` statements).
   - You don't need to explicitly add filtering logic in every query.

2. **Defined in `OnModelCreating`**:
   - Filters are configured in the `OnModelCreating` method of your `DbContext` using the `HasQueryFilter` method.


### **Example: Soft Deletes**

#### **Entity Model**:
```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public bool IsDeleted { get; set; }
}
```

#### **Configuring the Global Query Filter**:
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Product>().HasQueryFilter(p => !p.IsDeleted);
}
```

- This ensures that all queries on the `Product` entity automatically exclude rows where `IsDeleted = true`.

#### **Query Behavior**:
```csharp
var products = context.Products.ToList(); // Excludes soft-deleted rows (IsDeleted = true).
```


### **Example: Multi-Tenancy**

#### **Entity Model**:
```csharp
public class Order
{
    public int Id { get; set; }
    public string Description { get; set; }
    public int TenantId { get; set; }
}
```

#### **Configuring the Global Query Filter**:
```csharp
public class AppDbContext : DbContext
{
    private readonly int _tenantId;

    public AppDbContext(DbContextOptions options, int tenantId) : base(options)
    {
        _tenantId = tenantId;
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Order>().HasQueryFilter(o => o.TenantId == _tenantId);
    }
}
```

- The `_tenantId` is injected at runtime, so the filter automatically includes the tenant's ID in all queries.

#### **Query Behavior**:
```csharp
var orders = context.Orders.ToList(); // Returns only orders for the current tenant.
```


### **How to Bypass Global Query Filters**

There may be cases where you want to ignore the global filter (e.g., querying soft-deleted records).

#### **Using `IgnoreQueryFilters`**:
```csharp
var allProducts = context.Products.IgnoreQueryFilters().ToList();
```

- This explicitly bypasses the global `HasQueryFilter` logic.




