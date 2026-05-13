# Isolation Levels in Entity Framework

## What Is `IsolationLevel` in Entity Framework?

`IsolationLevel` determines how transactions interact with each other during concurrent database operations. It controls how data is read and locked between transactions, helping manage concurrency issues such as dirty reads, non-repeatable reads, and phantom reads.

Entity Framework itself does not implement isolation behavior directly. Instead, it relies on the underlying database engine (such as Microsoft SQL Server) to enforce the selected isolation level.

## Common Isolation Levels

* **Read Uncommitted**
* **Read Committed**
* **Repeatable Read**
* **Serializable**
* **Snapshot**

---

### 1️⃣ Read Uncommitted

🔹 **Definition**:
Transactions can read uncommitted changes made by other transactions. This means data may be dirty, inconsistent, or later rolled back.

🔹 **Use Case**:
Useful when performance is more important than absolute accuracy, such as logging, analytics, or reporting systems where temporary inconsistencies are acceptable.

---

### 2️⃣ Read Committed (Default in SQL Server & EF)

🔹 **Definition**:
A transaction can only read committed data, ensuring it never sees uncommitted changes from other transactions.

🔹 **Use Case**:
This is the default isolation level in Microsoft SQL Server and is suitable for most business applications.


---

### 3️⃣ Repeatable Read

🔹 **Definition**:
A transaction locks rows it reads, ensuring that the same rows cannot be modified by other transactions until the current transaction completes.

🔹 **Use Case**:
Useful when data stability is important, such as financial calculations or report generation where repeated reads must remain consistent.

---

### 4️⃣ Serializable (Most Strict)

🔹 **Definition**:
Serializable provides the highest isolation level by ensuring transactions behave as if they were executed sequentially (as if they were running one after the other).

🔹 **Use Case**:
Used when absolute consistency is required, such as banking systems, inventory management, and critical business operations.


---

### 5️⃣ Snapshot (Optimistic Concurrency)

🔹 **Definition**:
Snapshot isolation uses row versioning so that each transaction reads a consistent snapshot of the data as it existed when the transaction started.


🔹 **Use Case**:
Useful in high-concurrency systems such as e-commerce applications, dashboards, and real-time reporting systems.

> **Important:**
> Snapshot isolation is not fully equivalent to `Serializable` in all concurrency scenarios, even though it prevents the common read anomalies.



## Concurrency Issues

These are common concurrency problems that may occur depending on the selected isolation level.


### 1️⃣ Dirty Reads

🔹 **Definition**:
A transaction reads uncommitted changes made by another transaction. If the other transaction rolls back, the first transaction has read invalid data.

🔹 **Example**:

1. Transaction A updates a row but does not commit yet.
2. Transaction B reads the updated value.
3. Transaction A rolls back the change.
4. Transaction B now has incorrect data.


---

### 2️⃣ Non-Repeatable Reads

🔹 **Definition**:
A transaction reads the same row multiple times, but the value changes between reads because another transaction modified and committed the row.

🔹 **Example**:

1. Transaction A reads a row.
2. Transaction B updates and commits the same row.
3. Transaction A reads the row again and sees a different value.


---

### 3️⃣ Phantom Reads

🔹 **Definition**:
A transaction executes the same query multiple times and gets different sets of rows because another transaction inserted or deleted matching rows.

🔹 **Example**:

1. Transaction A selects all employees with `Salary > 5000`.
2. Transaction B inserts a new employee with `Salary = 6000` and commits.
3. Transaction A executes the query again and sees an additional row.

> **Quick Memory Note**
>
> * **Non-Repeatable Read** → The same row changes between reads.
> * **Phantom Read** → New rows appear or existing rows disappear between queries.


# How Isolation Levels Prevent Concurrency Issues

| Isolation Level      | Dirty Reads | Non-Repeatable Reads | Phantom Reads |
| -------------------- | ----------- | -------------------- | ------------- |
| **Read Uncommitted** | ❌           | ❌                    | ❌             |
| **Read Committed**   | ✅           | ❌                    | ❌             |
| **Repeatable Read**  | ✅           | ✅                    | ❌             |
| **Serializable**     | ✅           | ✅                    | ✅             |
| **Snapshot**         | ✅           | ✅                    | ✅             |



## How to Set `IsolationLevel` in Entity Framework

You can specify the isolation level when starting a transaction.



### Example: Using `BeginTransaction()`

```csharp
using (var transaction = context.Database.BeginTransaction(
    System.Data.IsolationLevel.Serializable))
{
    try
    {
        // Database operations

        context.SaveChanges();

        transaction.Commit();
    }
    catch
    {
        transaction.Rollback();
        throw;
    }
}
```

---

### Example: Using `TransactionScope`

```csharp
using System.Transactions;

using (var scope = new TransactionScope(
    TransactionScopeOption.Required,
    new TransactionOptions
    {
        IsolationLevel = IsolationLevel.ReadCommitted
    }))
{
    // Database operations

    context.SaveChanges();

    scope.Complete();
}
```


## Conclusion

Understanding isolation levels is important for balancing performance, concurrency, and data consistency in database applications.

Each isolation level provides different trade-offs between:

* Data accuracy
* Concurrency
* Locking behavior
* System performance

Choosing the appropriate isolation level depends on your application's business requirements and concurrency needs.

* **Read Uncommitted** → Highest performance but allows dirty reads.
* **Read Committed** → Default and suitable for most applications.
* **Repeatable Read** → Ensures stable repeated reads.
* **Serializable** → Provides the strictest consistency guarantees.
* **Snapshot** → Offers high concurrency with consistent reads using row versioning.
