# **Sealed Examples**
## Example 1: Sealed Class

```csharp
sealed class Car
{
}

class BMW : Car // Compile-time error
{
}
```

### Explanation

`BMW` cannot inherit from `Car` because `Car` is marked as `sealed`.

## Example 2: Sealed Method

```csharp
class Animal
{
    public virtual void MakeSound()
    {
        Console.WriteLine("Animal Sound");
    }
}

class Dog : Animal
{
    public sealed override void MakeSound()
    {
        Console.WriteLine("Bark");
    }
}

class Bulldog : Dog
{
    // Compile-time error
    public override void MakeSound()
    {
        Console.WriteLine("Loud Bark");
    }
}
```

### Explanation

`Dog` overrides `MakeSound()` and marks it as `sealed`, so derived classes cannot override it again.

