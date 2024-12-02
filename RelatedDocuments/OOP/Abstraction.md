# Abstraction Example

## Using Abstract Class

```csharp
public abstract class Vehicle
{
    public abstract void StartEngine();
    public abstract void Drive();
    public void Honk()
    {
        Console.WriteLine("Honking...");
    }
}

public class Car : Vehicle
{
    public override void StartEngine()
    {
        Console.WriteLine("Car engine started");
    }
     public override void Drive()
    {
        Console.WriteLine("Car is driving...");
    }
}
```


## Using Interface
```csharp
public interface IVehicle
{
    void StartEngine();
    void Drive();
}

public class Car : IVehicle
{
    public void StartEngine()
    {
        Console.WriteLine("Car engine started");
    }

    public void Drive()
    {
        Console.WriteLine("Car is driving...");
    }
}

```
