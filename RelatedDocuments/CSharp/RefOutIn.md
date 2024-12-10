# **ref, out and in Examples**

## ref Example
```csharp
public void Modify(ref int x)
{
    x = x + 10;  // Modifies the original variable
}

int number = 5;
Modify(ref number);
Console.WriteLine(number);  // Output: 15
```

## out Example
```csharp
public void Calculate(out int result)
{
    result = 50;  // Must assign a value before the method returns
}

int number;
Calculate(out number);
Console.WriteLine(number);  // Output: 50
```

## in Example:
```csharp
public void Display(in int x)
{
    Console.WriteLine(x);  // Can read x, but cannot modify it
    // x = 10;  // Error: Cannot assign to 'x' because it is a read-only parameter
}

int number = 5;
Display(in number);
Console.WriteLine(number);  // Output: 5
```