# TransactionScopeOption
## **What is `TransactionScopeOption` in .NET?**

**`TransactionScopeOption`** is an enum in .NET that determines whether a `TransactionScope` joins an existing transaction, 
creates a new one, or suppresses transactions. It controls transaction participation when using `TransactionScope`.



## **Values of `TransactionScopeOption`**

1. **`Required`** (Default):
   - Joins an existing ambient transaction if one exists.
   - If no ambient transaction exists, it creates a new transaction.
   - Suitable for most scenarios where you want operations to participate in a transaction if one is already active.

   **Example**:
   ```csharp
   using (var scope = new TransactionScope(TransactionScopeOption.Required))
   {
       // Will join an existing transaction or create a new one
       SaveData();
       scope.Complete();  // Commit the transaction
   }
   ```

2. **`RequiresNew`**:
   - Always creates a new transaction, even if there is an active ambient transaction.
   - Useful when you need operations to run in a separate transaction, independent of any existing transaction.

   **Example**:
   ```csharp
   using (var scope = new TransactionScope(TransactionScopeOption.RequiresNew))
   {
       // Creates a new transaction, independent of any existing one
       SaveData();
       scope.Complete();  // Commit the transaction
   }
   ```

3. **`Suppress`**:
   - Does not participate in any transaction, even if an ambient transaction exists.
   - Useful when you want operations to execute outside of any transaction, such as for logging or non-critical work.

   **Example**:
   ```csharp
   // Main transaction for money transfer
   using (var scope = new TransactionScope())
   {
       try
       {
           DeductMoney(fromAccount, amount);            
           AddMoney(toAccount, amount);
           
           // Log the transfer - We want this to happen regardless of transaction success/failure
           using (var logScope = new TransactionScope(TransactionScopeOption.Suppress))
           {
               LogTransfer(fromAccount, toAccount, amount);
               // `scope.Complete()` is unnecessary here since no transaction is used
           }
           
           scope.Complete(); // Complete the main money transfer transaction
       }
       catch (Exception ex)
       {
           // Transaction will automatically rollback if we don't call Complete()
       }
   }
   ```



## **When to Use Each Option**

| **Option**        | **Scenario**                                                                                       |
|--------------------|---------------------------------------------------------------------------------------------------|
| **Required**       | Default option, use when you want to participate in an existing transaction if one exists.        |
| **RequiresNew**    | Use when you need a separate transaction, independent of any existing transactions.               |
| **Suppress**       | Use when you want to perform operations outside of any transactional context.                     |

