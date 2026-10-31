# Isolation Levels in EF Core

## What Is `IsolationLevel`?

`IsolationLevel` determines how concurrent transactions interact when reading and modifying data.   
It controls the visibility of changes made by other transactions and helps manage concurrency issues such as dirty reads, non-repeatable reads, and phantom reads.

Entity Framework Core does not implement isolation behavior itself. It relies on the underlying database engine, such as SQL Server, to enforce the selected isolation level.

## Common Isolation Levels

- **Read Uncommitted**
- **Read Committed**
- **Repeatable Read**
- **Serializable**
- **Snapshot**

---
### 1. Read Uncommitted

**Definition:**  
Allows a transaction to read uncommitted changes made by other transactions.  
This can result in dirty reads if those changes are subsequently rolled back.

**Use Case:**  
May be appropriate when approximate results are acceptable and dirty reads are explicitly tolerated.   
However, it should be avoided when data accuracy is important.

---

### 2. Read Committed

**Definition:**  
Allows a transaction to read only committed data, preventing dirty reads.   
However, another transaction may modify or insert data between queries, resulting in non-repeatable reads or phantom reads.

**Read Committed** is the **default** isolation level in SQL Server, although the behavior of reads depends on whether locking or row versioning is used.

**Use Case:**  
Suitable for most business applications that require committed data without the additional locking restrictions of stricter isolation levels.

---

### 3. Repeatable Read

**Definition:**    
Ensures that data read by a transaction cannot be modified by another transaction until the current transaction completes.   
However, other transactions may insert new rows matching the original query conditions, allowing phantom reads.

**Use Case:**    
Useful when a transaction needs previously read rows to remain unchanged while it performs additional operations.

---

### 4. Serializable

**Definition:**  
The strictest standard isolation level. It ensures that concurrent transactions behave as if they were executed sequentially, preventing dirty reads, non-repeatable reads, and phantom reads.


**Use Case:**  
Useful when concurrent operations must preserve strict consistency, such as financial business rules.

**Trade-off:** It can increase blocking, reduce concurrency, and increase the risk of deadlocks.

---

### 5. Snapshot

**Definition:**  
Uses row versioning to provide a transaction-level consistent view of committed data as it existed when the transaction started.

Unlike locking-based isolation levels, Snapshot allows reads to access earlier row versions rather than waiting for concurrent modifications to finish.

**SQL Server Requirement:**  
The database option `ALLOW_SNAPSHOT_ISOLATION` must be enabled before transactions can use the `Snapshot` isolation level.

**Use Case:**  
Useful for applications with concurrent reads and writes that require a consistent view of data throughout the entire transaction, while reducing read-related blocking.

**Important:** Snapshot prevents the three standard isolation anomalies discussed below, but it is not equivalent to Serializable. Certain concurrency anomalies, such as write skew, can still occur.


## Concurrency Issues

The following problems can occur when transactions execute concurrently, depending on the isolation level.

### 1. Dirty Reads

**Definition:**  
A transaction reads changes made by another transaction that has not yet committed.  
If the other transaction rolls back, the first transaction has read data that was never committed.

**Example:**  
1. Transaction A updates an employee's salary but does not commit.
2. Transaction B reads the updated salary.
3. Transaction A rolls back.
4. Transaction B has read a value that was never committed.


---

### 2. Non-Repeatable Reads

**Definition:**  
A transaction reads the same row twice but gets different values because another transaction modified and committed that row between the reads.

**Example:**

1. Transaction A reads an employee's salary as 5,000.
2. Transaction B updates the salary to 6,000 and commits.
3. Transaction A reads the same row again and gets 6,000.

---

### 3. Phantom Reads  

**Definition:**  
A transaction executes the same query multiple times but gets different sets of rows because another transaction inserts or deletes rows that match the query conditions.

**Example:**  
1. Transaction A selects all employees with `Salary > 5000`.
2. Transaction B inserts an employee with a salary of 6,000 and commits.
3. Transaction A executes the same query again and sees the additional employee.


**Quick Memory Note:**  
- **Dirty Read:** Reading uncommitted changes.
- **Non-Repeatable Read:** The same row returns different values.
- **Phantom Read:** The same query returns a different set of rows.



## How Isolation Levels Prevent Concurrency Issues

The following table summarizes the three standard isolation anomalies under SQL Server's standard isolation-level behavior.

| Isolation Level | Dirty Reads | Non-Repeatable Reads | Phantom Reads |
|---|---|---|---|
| **Read Uncommitted** | Yes | Yes | Yes |
| **Read Committed** | No | Yes | Yes |
| **Repeatable Read** | No | No | Yes |
| **Serializable** | No | No | No |
| **Snapshot** | No | No | No |

**Important:** Although Snapshot prevents these three anomalies, it does not guarantee the same behavior as Serializable in every concurrency scenario.



## How to Set `IsolationLevel` in EF Core

You can specify the isolation level when starting a database transaction.

### Example 1: Using `BeginTransaction()`

```csharp
using var transaction = context.Database.BeginTransaction(
    System.Data.IsolationLevel.Serializable);

// Perform database operations

context.SaveChanges();

transaction.Commit();
```

**Explanation:**  
- `BeginTransaction()` starts a transaction using the specified isolation level.
- `SaveChanges()` persists the changes within the transaction.
- `Commit()` commits the transaction.
- If the transaction is disposed without being committed, it is rolled back.

### Example 2: Using `TransactionScope`

```csharp
using System.Transactions;

using var scope = new TransactionScope(
    TransactionScopeOption.Required,
    new TransactionOptions
    {
        IsolationLevel = System.Transactions.IsolationLevel.ReadCommitted
    });

context.SaveChanges();

scope.Complete();
```

**Explanation:**

- `TransactionScopeOption.Required` uses the ambient transaction if one exists; otherwise, it creates one.
- `TransactionOptions` specifies the isolation level.
- `Complete()` indicates that the operations succeeded and the transaction can commit when the scope is disposed.

**Note:** When using asynchronous operations, configure `TransactionScopeAsyncFlowOption.Enabled`.

## Important Note

**Choosing an isolation level involves balancing data consistency, concurrency, and performance.**
The appropriate choice depends on the application's business requirements and the consequences of concurrent operations.