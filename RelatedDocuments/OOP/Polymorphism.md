# Polymorphism Examples

## Using Inheritance

```csharp
public class Color
{
    public virtual void Display()
    {
        Console.WriteLine("This is a generic color");
    }
}

public class Red : Color
{
    public override void Display()
    {
        Console.WriteLine("This is red color");
    }
}

public class Blue : Color
{
    public override void Display()
    {
        Console.WriteLine("This is blue color");
    }
}


```

### Demonstrating Polymorphism  

```csharp

Color color1 = new Red();
color1.Display(); // Output: This is red color

Color color2 = new Blue();
color2.Display(); // Output: This is blue color

```

Or

```csharp
List<Color> colors = new List<Color>
{
    new Red(),
    new Blue()
};

foreach (var color in colors)
{
    color.Display(); // Calls the specific implementation for each color.
}
```

## Using Interfaces

```csharp
public class IColor
{
    public void Display();
}

public class Red : IColor
{
    public override void Display()
    {
        Console.WriteLine("This is red color");
    }
}

public class Blue : IColor
{
    public override void Display()
    {
        Console.WriteLine("This is blue color");
    }
}
```
### Demonstrating Polymorphism  
```csharp
List<IColor> colors = new List<IColor>
{
    new Red(),
    new Blue()
};

foreach (var color in colors)
{
    color.Display(); // Calls the specific implementation for each color.
}
```


