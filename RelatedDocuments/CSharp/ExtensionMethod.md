# Extension Method Example

```csharp
using System;

public static class IntExtensions
{
    public static int Doubler(this int value)
    {
        return 2 * value;
    }

    public static int AddThree(this int value)
    {
        return value + 3;
    }
}

public class Program
{
    public static void Main()
    {
        int x = 5;
        int result = x.Doubler().AddThree(); // (5 * 2) + 3 = 13
        Console.WriteLine(result);
    }
}
```
As you see in the previous example we added 2 methods to int struct without modifying the original **`int` struct**.