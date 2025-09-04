# **I**nterface Segregation Principle (ISP)

## ⚠️ Without ISP

```csharp
public interface IWorker
{
    void Work();
    void TakeBreak();
}
```

```csharp
public class HumanWorker : IWorker
{
    public void Work()
    {
        Console.WriteLine("Human is working.");
    }

    public void TakeBreak()
    {
        Console.WriteLine("Human is taking a break.");
    }
}
```
```csharp
public class RobotWorker : IWorker
{
    public void Work()
    {
        Console.WriteLine("Robot is working.");
    }

    public void TakeBreak()
    {
        throw new NotImplementedException("Robots don't need breaks.");
    }
}
```

### Why this violates ISP:

> The `RobotWorker` class is forced to implement `TakeBreak()` even though robots don't need breaks. Throwing a `NotImplementedException` is a clear sign that the interface includes methods irrelevant to certain implementations. This increases coupling and reduces flexibility.

---

## ✅ With ISP

```csharp
public interface IWork
{
    void Work();
}

public interface ITakeBreak
{
    void TakeBreak();
}
```

```csharp
public class HumanWorker : IWork, ITakeBreak
{
    public void Work()
    {
        Console.WriteLine("Human is working.");
    }

    public void TakeBreak()
    {
        Console.WriteLine("Human is taking a break.");
    }
}
```
```csharp
public class RobotWorker : IWork
{
    public void Work()
    {
        Console.WriteLine("Robot is working.");
    }
}
```

### Why this adheres to ISP:

> We’ve split the original interface into smaller ones (`IWork` and `ITakeBreak`), so each class only implements the functionality it needs. This reduces unnecessary dependencies and promotes low coupling making the system more maintainable and scalable.
