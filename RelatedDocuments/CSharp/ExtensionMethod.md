# Extension Method Example

```csharp
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

 public static void Main()
 {
   int x = 5;
   Int result = x.Doubler().AddThree();
   Console.WriteLine(result);
}
```
As you see in the previous example we added 2 methods to int struct without modifying the original int struct.