# **yield Examples**

## **1. Simple Example: Returning a Sequence**
```csharp
IEnumerable<int> GetNumbers()
{
    yield return 1; // Returns 1 and pauses
    yield return 2; // Returns 2 and pauses
    yield return 3; // Returns 3 and pauses
}

// Usage:
foreach (var number in GetNumbers())
{
    Console.WriteLine(number);
}
```

**Output:**
```
1
2
3
```


## **2. Using `yield` in a Loop**
```csharp
IEnumerable<int> GenerateEvenNumbers(int limit)
{
    for (int i = 0; i <= limit; i += 2)
    {
        yield return i; // Return the current value of `i` and pause
    }
}

// Usage:
foreach (var even in GenerateEvenNumbers(10))
{
    Console.WriteLine(even);
}
```

**Output:**
```
0
2
4
6
8
10
```

## **3. `yield break`**
The `yield break` statement is used to end the iteration early.

```csharp
IEnumerable<int> GenerateNumbers(int limit)
{
    for (int i = 0; i <= limit; i++)
    {
        if (i == 5)
        {
            yield break; // Stop iteration when `i` reaches 5
        }
        yield return i;
    }
}

// Usage:
foreach (var num in GenerateNumbers(10))
{
    Console.WriteLine(num);
}
```

**Output:**
```
0
1
2
3
4
```
