
### **Caching in Entity Framework Core**

EF includes **first-level caching** (also called the identity map) caching by default. Within the same `DbContext` instance, if you query an entity by its **primary key** and then query it again, EF returns the already tracked entity from memory instead of sending another query to the database. 

Example:

```csharp
using var context = new AppDbContext();

var user1 = context.Users.Find(1); // Hits the database
var user2 = context.Users.Find(1); // Returned from DbContext cache

Console.WriteLine(object.ReferenceEquals(user1, user2)); // True
```

Here, the second `Find()` call does not query the database again—it returns the same tracked entity from the context’s cache.

---

### **Important Note**
The built-in caching behavior in EF Core is limited to **tracked entities**, not entire query results.  

This means EF Core does **not** automatically store and reuse the results of LINQ queries. Every LINQ query is translated into SQL and executed against the database each time it runs.

Example:

```csharp
var usersA = context.Users.Where(u => u.Age > 20).ToList(); // Executes DB query
var usersB = context.Users.Where(u => u.Age > 20).ToList(); // Executes DB query again
```

Even though both queries are identical, EF Core sends two separate requests to the database because query result sets are not cached by default.

---

### **Summary**
- EF Core **remembers tracked entities** within the same `DbContext`.  
- EF Core **does not remember query results**—identical LINQ queries will always hit the database again.  
- If you need caching for query results, you must implement it yourself or use a **second-level caching library**.

