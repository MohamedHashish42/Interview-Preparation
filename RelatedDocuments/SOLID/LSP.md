# **L**iskov Substitution Principle (LSP)

## First Example
### ⚠️ LSP Violation
```csharp
public class Bird
{
    public virtual void Fly()
    {
        Console.WriteLine("Flying...");
    }

    public virtual void Eat()
    {
        Console.WriteLine("Eating...");
    }
}

public class Penguin : Bird
{
    public override void Fly()
    {
        Console.WriteLine("I can't fly, but I'm still a Bird!");
    }
}
```

```csharp
Bird bird = new Penguin();
bird.Fly(); //💥 Program runs but behavior is wrong (logical violation)
```

#### **Why this violates LSP:**


> The code assumes that every `Bird` can fly, but `Penguin` cannot.
>
> Using `Penguin` instead of `Bird` breaks the expected behavior because not all birds can fly. This violates the **Liskov Substitution Principle**, which says that subclasses must be usable in place of their parent without changing correct program behavior.





### ✅ With LSP (Correct Design)

```csharp
public interface IBird
{
    void Eat();
}

public interface IFlyable
{
    void Fly();
}
```

```csharp
public class Sparrow : IBird, IFlyable
{
    public void Eat()
    {
        Console.WriteLine("Eating...");
    }

    public void Fly()
    {
        Console.WriteLine("Flying...");
    }
}

public class Penguin : IBird
{
    public void Eat()
    {
        Console.WriteLine("Eating...");
    }
}
```

```csharp
IFlyable flyableBird = new Sparrow();
flyableBird.Fly(); // ✅ Works

IBird penguin = new Penguin(); // ✅ Works safely
```

#### 💡 **Why this adheres to LSP:**

> We removed the incorrect assumption that all birds can fly by separating behaviors into smaller, specific interfaces.

* ✅ Each class only implements behavior it actually supports
* ✅ No subclass violates expected behavior
* ✅ Objects can be substituted without breaking the program




## Second Example
### ⚠️ LSP Violation

```csharp
public class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }

    public int GetArea()
    {
        return Width * Height;
    }
}

public class Square : Rectangle
{
    public override int Width
    {
        set
        {
            base.Width = value;
            base.Height = value;
        }
    }

    public override int Height
    {
        set
        {
            base.Width = value;
            base.Height = value;
        }
    }
}
```

```csharp
Rectangle rect = new Square();
rect.Width = 5;
rect.Height = 10;

Console.WriteLine(rect.GetArea()); // 💥 Unexpected result (100 instead of 50)
```



#### **Why this violates LSP:**

> The code assumes that `Rectangle` has independent `Width` and `Height`.
>
> `Square` breaks this assumption by forcing both values to be the same.
>
> This violates the **Liskov Substitution Principle** because using `Square` instead of `Rectangle` changes the expected behavior of the program.



### ✅ With LSP (Correct Design)

```csharp
public interface IShape
{
    int GetArea();
}
```

```csharp
public class Rectangle : IShape
{
    public int Width { get; set; }
    public int Height { get; set; }

    public int GetArea()
    {
        return Width * Height;
    }
}
```

```csharp
public class Square : IShape
{
    public int Side { get; set; }

    public int GetArea()
    {
        return Side * Side;
    }
}
```

```csharp
IShape shape1 = new Rectangle { Width = 5, Height = 10 };
Console.WriteLine(shape1.GetArea()); // 50

IShape shape2 = new Square { Side = 5 };
Console.WriteLine(shape2.GetArea()); // 25
```

#### 💡 **Why this adheres to LSP:**

> We removed the incorrect assumption that a square must behave like a rectangle with independent dimensions.

* ✅ Each class defines its own correct behavior
* ✅ No subclass changes expected behavior of the parent
* ✅ Objects are fully substitutable without breaking logic


