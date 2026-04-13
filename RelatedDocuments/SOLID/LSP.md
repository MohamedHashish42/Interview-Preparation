# **L**iskov Substitution Principle (LSP)


## ⚠️ LSP Violation
```csharp
public class Bird
{
    public virtual void Fly()
    {
        Console.WriteLine("Flying...");
    }
}

public class Penguin : Bird
{
    public override void Fly()
    {
        throw new Exception("Penguins can't fly!");
    }
}
```

```csharp
Bird bird = new Penguin();
bird.Fly(); // 💥 Runtime Exception
```

### Why this violates LSP:

> The code assumes that every `Bird` can fly. However, `Penguin` breaks this expectation by throwing an exception.
>
> Substituting `Bird` with `Penguin` breaks the expected behavior and causes the program to fail, which is a clear violation of the **Liskov Substitution Principle**.


## ✅ With LSP (Correct Design)

```csharp
public interface IBird
{
}

public interface IFlyable
{
    void Fly();
}
```

```csharp
public class Sparrow : IBird, IFlyable
{
    public void Fly()
    {
        Console.WriteLine("Flying...");
    }
}

public class Penguin : IBird
{
    // No Fly method ✅
}
```

```csharp
IFlyable bird = new Sparrow();
bird.Fly(); // ✅ Works

IBird penguin = new Penguin(); // ✅ Works safely
```

### 💡 Why this adheres to LSP:

> We removed the incorrect assumption that all birds can fly by separating behaviors into smaller, specific interfaces.

* ✅ Each class only implements behavior it actually supports
* ✅ No subclass violates expected behavior
* ✅ Objects can be substituted without breaking the program
