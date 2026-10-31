# Transactions in Entity Framework Core

Transactions ensure that a series of database operations are executed as a single unit of work, meaning that either all operations are committed, or none of their changes are committed.

EF Core supports transactions through automatic transaction management, explicit database transactions, and .NET's `TransactionScope`.

## 1. Automatic Transactions

EF Core automatically wraps a single `SaveChanges()` or `SaveChangesAsync()` call in a transaction when the database provider supports transactions.

If all changes succeed, they are committed. If an operation fails, the transaction is rolled back.

**Example:**

```csharp
using var context = new AppDbContext();

context.Users.Add(new User { Name = "John" });
context.SaveChanges();
```

**Use Case:** Simple operations where all changes made by a single `SaveChanges()` call must succeed or fail together.

**Important:** Automatic transactions do not cover multiple separate `SaveChanges()` calls as one unit. Use an explicit transaction when multiple operations must succeed or fail together.


Example:

```csharp
context.Students.Add(student);
context.SaveChanges(); // Transaction 1

context.Courses.Add(course);
context.SaveChanges(); // Transaction 2

context.Enrollments.Add(enrollment);
context.SaveChanges(); // Transaction 3
```

Explanation:

Each `SaveChanges()` call has its own automatic transaction.

If **Transaction 1** succeeds, the student is saved.  
If **Transaction 2** succeeds, the course is saved.  
If **Transaction 3** fails, the enrollment is not saved, but the student and course remain saved.  

**Note:**   If all three operations must succeed or fail together, use an explicit transaction to wrap all SaveChanges() calls.

## 2. Explicit Transactions

When you need to coordinate multiple operations within a single transaction, you can manage the transaction explicitly using:

- **`IDbContextTransaction`** — EF Core's database transaction API.
- **`TransactionScope`** — a .NET feature that manages ambient transactions.

---

### A. Using `IDbContextTransaction`

`IDbContextTransaction` represents a database transaction managed through EF Core.  
It is commonly used with a single `DbContext` when multiple operations must be committed or rolled back together.

**Steps:**

1. Start a transaction using `BeginTransaction()`.
2. Perform database operations.
3. Call `Commit()` if all operations succeed.
4. Dispose of the transaction without committing if an operation fails.

**Example:**

```csharp
using var context = new AppDbContext();

using var transaction = context.Database.BeginTransaction();

context.Users.Add(new User { Name = "Alice" });
context.SaveChanges();

context.Orders.Add(new Order { OrderNumber = "123" });
context.SaveChanges();

transaction.Commit();
```

**Explanation:**

- Both `SaveChanges()` calls execute within the same transaction.
- `Commit()` commits the transaction.
- If an exception occurs before `Commit()`, disposing of the uncommitted transaction rolls it back.

**Advantages:**
- Provides explicit control over the transaction lifecycle.
- Suitable for coordinating multiple operations and `SaveChanges()` calls.
- Can be shared across multiple `DbContext` instances when they share the same underlying database connection and transaction.

---

### B. Using `TransactionScope`

`TransactionScope` is a .NET feature that manages ambient transactions.  
An ambient transaction is a transaction available to operations executing within its scope.

**For more detail** [TransactionScopeOption](./RelatedDocuments/EntityFramework/TransactionScopeOption.md)

**Steps:**

1. Create a `TransactionScope`.
2. Perform database operations.
3. Call `Complete()` when all operations succeed.
4. Dispose of the scope to finish the transaction.

**Example: Synchronous Operations**

```csharp
using System.Transactions;

using (var scope = new TransactionScope())
{
    using (var context1 = new AppDbContext())
    {
        context1.Users.Add(new User { Name = "Bob" });
        context1.SaveChanges();
    }

    using (var context2 = new AppDbContext())
    {
        context2.Orders.Add(new Order { OrderNumber = "456" });
        context2.SaveChanges();
    }

    scope.Complete();
}
```

**Explanation:**

- Both contexts attempt to participate in the same ambient transaction.
- `Complete()` indicates that the scope completed successfully.
- The transaction is committed when the scope is disposed, provided the transaction and all required scopes complete successfully.
- If an operation fails before `Complete()` is called, the transaction is not committed and is rolled back.

**Important:** Both contexts must enlist in the ambient transaction. Provider support is required, and using multiple database connections can cause the transaction to escalate to a **distributed transaction**.

**Distributed Transactions:** If a transaction spans multiple connections or resources, it may escalate to a distributed transaction. Support for distributed transactions depends on the .NET runtime, operating system, database provider, and infrastructure. On supported Windows configurations, **MSDTC (Microsoft Distributed Transaction Coordinator)** may be required.


## 3. Comparison: `IDbContextTransaction` vs. `TransactionScope`

| Feature | `IDbContextTransaction` | `TransactionScope` |
|---|---|---|
| Transaction model | Explicit database transaction | Ambient transaction |
| Typical usage | Operations within one context | Operations across multiple contexts or resources |
| Multiple contexts | Possible when they share the same connection and transaction | Possible when they enlist in the same ambient transaction |
| Transaction control | `Commit()` and `Rollback()` | `Complete()` followed by scope disposal |

