# **params Example**
```csharp
public class Example
{
    public void PrintNumbers(params int[] numbers)
    {
        foreach (var num in numbers)
        {
            Console.WriteLine(num);
        }
    }
}

class Program
{
    static void Main()
    {
        var example = new Example();

        // Calling with different numbers of arguments
        example.PrintNumbers(1, 2, 3);  // Output: 1 2 3
        example.PrintNumbers(10, 20);   // Output: 10 20
        example.PrintNumbers();         // No output (empty array)
    }
}
```
