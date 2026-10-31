# TransactionScopeOption

## What Is `TransactionScopeOption` in .NET?

`TransactionScope` is a .NET feature that manages ambient transactions.  
**An ambient transaction** is a transaction available to operations executing within its scope.

`TransactionScopeOption` is a .NET enum that determines how a `TransactionScope` interacts with an existing ambient transaction.   
It specifies whether the scope joins an existing transaction, creates a new transaction, or executes without an ambient transaction.

## Values of `TransactionScopeOption`

### 1. `Required`

**Definition:**
- Joins the existing ambient transaction if one exists.
- Creates a new transaction if no ambient transaction exists.

**Use Case:**  
Suitable for most scenarios where operations should participate in an existing transaction when available.

**Example:**

```csharp
using (var scope = new TransactionScope(
    TransactionScopeOption.Required))
{
    SaveData();

    scope.Complete();
}
```

If an ambient transaction exists, the scope participates in it. Otherwise, it creates a new transaction.

**Important:** Calling `Complete()` indicates that the scope completed successfully. The transaction is committed when the relevant scope is disposed, provided all required scopes complete successfully.

---

### 2. `RequiresNew`

**Definition:**
- Always creates a new transaction, even if an ambient transaction already exists.
- The existing ambient transaction is temporarily suspended while the new transaction is active.

**Use Case:**

Useful when an operation must commit independently of an existing transaction.

**Example:**

```csharp
using (var scope = new TransactionScope(
    TransactionScopeOption.RequiresNew))
{
    SaveData();

    scope.Complete();
}
```

**Important:** The new transaction can commit independently. If the outer transaction subsequently rolls back, the changes committed by the new transaction generally remain committed.

---

### 3. `Suppress`

**Definition:**
- Suppresses the ambient transaction within the scope.
- Operations executed within the scope do not automatically participate in the previously active ambient transaction.

**Use Case:**

Useful when an operation should execute outside the current ambient transaction, such as performing logging that should not be rolled back with the main transaction.

**Example:**

```csharp
using (var scope = new TransactionScope(
    TransactionScopeOption.Suppress))
{
    LogMessage("Operation executed");
}
```

**Important:**
- Calling `Complete()` is unnecessary when the scope does not create or participate in a transaction.
- `Suppress` does not prevent an operation from explicitly creating its own transaction.
  ```Csharp
     using (var scope = new TransactionScope(
    TransactionScopeOption.Suppress))
    {
        using (var newScope = new TransactionScope())
        {
            SaveLog();
            newScope.Complete();
        }
    }
   ```


## When to Use Each Option

| Option | Behavior | Typical Use Case |
|---|---|---|
| `Required` | Joins an existing ambient transaction or creates a new one. | Operations that should participate in the current transaction when available. |
| `RequiresNew` | Always creates a new transaction. | Operations that must commit independently of the outer transaction. |
| `Suppress` | Executes without the existing ambient transaction. | Operations that should not participate in the current transaction. |

## Key Takeaways

- **`Required`** → Join an existing transaction or create a new one.
- **`RequiresNew`** → Always create a new transaction.
- **`Suppress`** → Suppress the ambient transaction.
- **`Complete()`** → Indicates successful completion of a scope; it does not immediately commit the transaction.