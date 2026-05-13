# How does Entity Framework implement **Transactions**

Entity Framework Transactions ensure that a series of database operations are executed as a single unit of work. If any operation fails, the entire transaction is rolled back to maintain data integrity.  


Transactions can be managed automatically by Entity Framework or explicitly using `DbContext.Database.BeginTransaction()`.


## **1. Automatic Transactions**

Entity Framework automatically wraps **`SaveChanges()`** in a transaction. If all operations succeed, the changes are committed; otherwise, they are rolled back automatically.

Use this approach for simple operations where no manual transaction control is needed.

**Example**:
```csharp
using (var context = new AppDbContext())
{
    context.Users.Add(new User { Name = "John" });
    context.SaveChanges(); // Wrapped in an implicit transaction
}
```

## **2. Explicit Transactions**

For more complex scenarios involving multiple `SaveChanges()` calls or operations spanning multiple contexts, you can explicitly manage transactions using:

- **`IDbContextTransaction`** — EF Core's built-in transaction API
- **`TransactionScope`** — a higher-level ambient transaction manager from .NET

---

### **First: `IDbContextTransaction`** (EF Core)

An EF Core–specific mechanism for managing transactions within a single `DbContext`.

**Steps**:
1. Start a transaction with `BeginTransaction()`.
2. Perform operations.
3. Commit or rollback the transaction.

**Example**:
```csharp
using (var context = new AppDbContext())
{
    using (var transaction = context.Database.BeginTransaction())
    {
        try
        {
            context.Users.Add(new User { Name = "Alice" });
            context.SaveChanges();

            context.Orders.Add(new Order { OrderNumber = "123" });
            context.SaveChanges();

            transaction.Commit(); // Commit if everything succeeds
        }
        catch (Exception)
        {
            transaction.Rollback(); // Rollback on failure
            throw;
        }
    }
    // If Commit() is never called or an exception occurs,
    // the transaction is rolled back when disposed
}
```

**Advantages**:
- Fine-grained control over the transaction's lifecycle.
- Works within a single `DbContext`.



### **Second: `TransactionScope`**

A .NET feature that manages ambient transactions, allowing multiple `DbContext` instances or even non-EF resources (e.g., other databases or services) to participate in a single transaction.

**Steps**:
1. Wrap the operations in a `TransactionScope`.
2. Call `scope.Complete()` when everything succeeds — otherwise the transaction rolls back automatically.

**Example (Synchronous)**:
```csharp
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

    scope.Complete(); // Commit the transaction
}
// If an exception occurs or Complete() is never called, the transaction rolls back automatically
```

**Example (Asynchronous)**:
```csharp
using (var scope = new TransactionScope(TransactionScopeAsyncFlowOption.Enabled))
{
    using (var context1 = new AppDbContext())
    {
        context1.Users.Add(new User { Name = "Bob" });
        await context1.SaveChangesAsync();
    }

    using (var context2 = new AppDbContext())
    {
        context2.Orders.Add(new Order { OrderNumber = "456" });
        await context2.SaveChangesAsync();
    }

    scope.Complete();
}
// If an exception occurs or Complete() is never called, the transaction rolls back automatically
```

**Advantages**:
- Allows multiple `DbContext` instances or other resources to participate in the same transaction.
- Simplifies distributed transaction management.
- Supports async operations when `TransactionScopeAsyncFlowOption.Enabled` is set.

> **Note:** If the transaction spans multiple databases or external resources, **Microsoft Distributed Transaction Coordinator (MSDTC)** may be required. If all operations target the same SQL Server instance, MSDTC is **not** needed.


## **Comparison: `IDbContextTransaction` vs `TransactionScope`**

| Feature | `IDbContextTransaction` | `TransactionScope` |
|---|---|---|
| Scope | Single `DbContext` only | Multiple `DbContext` instances or resources |
| Simplicity | Straightforward for single-context use | Higher-level abstraction; easier for distributed scenarios |
| .NET Compatibility | EF Core: `IDbContextTransaction`. <br> EF 6: `DbContextTransaction` (same concept, slightly different API). | Works in both EF Core and EF 6. <br> requires MSDTC for cross-resource transactions. |
| Async Support | Fully supported | Requires `TransactionScopeAsyncFlowOption.Enabled` |

---

## **Conclusion**
Entity Framework handles transactions either implicitly via `SaveChanges()` or explicitly via `IDbContextTransaction` and `TransactionScope`. Choose based on your scenario:

- **Automatic transactions** — sufficient for simple, single `SaveChanges()` calls.
- **`IDbContextTransaction`** — use when you need fine-grained control within a single `DbContext`.
- **`TransactionScope`** — use for cross-context or distributed transactions, always enable `TransactionScopeAsyncFlowOption.Enabled` for async code.

If your transaction spans multiple databases or external resources, evaluate whether MSDTC is required based on your infrastructure.
