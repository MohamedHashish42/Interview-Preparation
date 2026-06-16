# **Inheritance and Composition Examples**

## Inheritance Example
```csharp
public class Animal
{
    public void Eat() => Console.WriteLine("Eating...");
}

public class Dog : Animal // Dog is an Animal
{
    public void Bark() => Console.WriteLine("Barking...");
}
```

## Composition Example
### **Concrete Composition or Direct Composition (Tightly Coupled)**
```csharp
public class Engine
{
    public void Start() => Console.WriteLine("Engine started...");
}

public class Car // Car has an Engine
{
    private Engine _engine = new Engine();

    public void StartCar()
    {
        _engine.Start();
        Console.WriteLine("Car started...");
    }
}
```


### **Interface-Based Composition (Loosely Coupled)**  

```csharp

public interface IEngine
{
    void Start();
}


public class GasolineEngine : IEngine
{
    public void Start() => Console.WriteLine("Gasoline engine started...");
}

public class ElectricEngine : IEngine
{
    public void Start() => Console.WriteLine("Electric motor powered up...");
}

public class Car
{
    private readonly IEngine _engine;

    public Car(IEngine engine)
    {
        _engine = engine;
    }

    public void StartCar()
    {
        _engine.Start();
        Console.WriteLine("Car started...");
    }
}
```