# Inheritance  Example

```csharp
public class Vehicle
{
    public string Brand { get; set; }
    public int Year { get; set; }

    public Vehicle(string brand, int year)
    {
        Brand = brand;
        Year = year;
    }

    public void DisplayInfo()
    {
        Console.WriteLine($"Brand: {Brand}, Year: {Year}");
    }
}

// Derived class
public class Car : Vehicle
{
    public int NumberOfDoors { get; set; }

    public Car(string brand, int year, int numberOfDoors) : base(brand, year)
    {
        NumberOfDoors = numberOfDoors;
    }

    public void Honk()
    {
        Console.WriteLine($"{Brand} car honks: Beep beep!");
    }
}

```
