# Indexer Example

```csharp

public class SampleCollection
{
    private string[] items = new string[10];

    // Indexer declaration
    public string this[int index]
    {
        get { return items[index]; }
        set { items[index] = value; }
    }
}

class Program
{
    static void Main()
    {
        var collection = new SampleCollection();
        collection[0] = "Hello";
        Console.WriteLine(collection[0]);  // Output: Hello
    }
}
```

