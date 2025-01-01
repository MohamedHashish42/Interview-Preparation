# ToLookup GroupBy Examples


### `ToLookup` Example
```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };

// Immediate execution
var lookup = numbers.ToLookup(n => n % 2 == 0 ? "Even" : "Odd");

Console.WriteLine("Lookup created:");
foreach (var group in lookup)
{
    Console.WriteLine($"{group.Key}: {string.Join(", ", group)}");
}

// The following won't work because it is (Read-only) :
// lookup.Where(...)  
// lookup.Select(...) 
```

#### Output

```
Odd: 1, 3, 5
Even: 2, 4, 6
```



### `GroupBy` Example
```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };

// Deferred execution
var groups = numbers.GroupBy(n => n % 2 == 0 ? "Even" : "Odd");

// Nothing happens until we enumerate the result.
Console.WriteLine("Enumerating GroupBy:");
foreach (var group in groups)
{
    Console.WriteLine($"{group.Key}: {string.Join(", ", group)}");
}

// The following will work groups because GroupBy allows query transformations:
// groups.Where(...)  
// groups.Select(...) 

```

#### Output

```
Odd: 1, 3, 5
Even: 2, 4, 6
```
