# **L**iskov Substitution Principle (LSP)


## ⚠️ Without LSP 

```csharp
public class Apple
{
    public virtual string GetColor()
    {
        return "Red";
    }
}

public class Orange : Apple
{
    public override string GetColor()
    {
        return "Orange";
    }
}

Apple apple = new Orange();
Console.WriteLine(apple.GetColor()); // Output: Orange
```

### Why this violates LSP:

> The `Orange` class inherits from `Apple`, suggesting that an `Orange` **is a kind of** `Apple`. This violates Liskov Substitution Principle because any code using `Apple` should safely use any derived class without unexpected behavior. But in this case, substituting `Apple` with `Orange` results in incorrect assumptions, for example, we might expect all `Apple` instances to return `"Red"` for their color.



## ✅ With LSP (Correct Design)

```csharp
public abstract class Fruit
{
    public abstract string GetColor();
}
```

```csharp
public class Apple : Fruit
{
    public override string GetColor()
    {
        return "Red";
    }
}

public class Orange : Fruit
{
    public override string GetColor()
    {
        return "Orange";
    }
}
```

```csharp
Fruit fruit = new Orange();
Console.WriteLine(fruit.GetColor()); // Output: Orange

fruit = new Apple();
Console.WriteLine(fruit.GetColor()); // Output: Red
```

### Why this adheres to LSP:

> We now have a common abstraction (`Fruit`) and two unrelated, independent implementations (`Apple`, `Orange`). Any code working with a `Fruit` can safely operate on any subclass without relying on specific behavior or properties unique to one type.
