# **Using Example**

Consider working with a file stream:
```csharp
using (var stream = new FileStream("example.txt", FileMode.Open))
{
    // Perform file operations
    StreamReader reader = new StreamReader(stream);
    Console.WriteLine(reader.ReadToEnd());
}
// The FileStream is automatically closed and disposed of here
```
In this example, `FileStream` implements `IDisposable`. The `using` statement ensures that the stream is closed and the resources are released as soon as the operations inside the `using` block are completed.

### **Equivalent Code Without `using`**:
```csharp
FileStream stream = null;
try
{
    stream = new FileStream("example.txt", FileMode.Open);
    StreamReader reader = new StreamReader(stream);
    Console.WriteLine(reader.ReadToEnd());
}
finally
{
    if (stream != null)
        stream.Dispose();
}
```
Without `using`, the code becomes longer and requires manual handling of the `Dispose` method, typically in a `finally` block.