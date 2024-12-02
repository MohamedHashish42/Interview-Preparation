# Encapsulation Example

## Encapsulation without getter and setter
```csharp
public class Person
{

    private string name;
    private int age;

    public void SetName(string name)
    {
        this.name = name;
    }
    public string GetName()
    {
        return name;
    }


    public void SetAge(int age)
    {
        if (age < 0)
        {
            throw new ArgumentException("Age cannot be negative.");
        }
        this.age = age;
    }
    public int GetAge()
    {
        return age;
    }
}

```

## Encapsulation with getter and setter
```csharp
 public class Person
{

    private string name;
    private int age;


    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    public int Age
    {
        get { return age; }
        set
        {
            if (value < 0)
            {
                throw new ArgumentException("Age cannot be negative.");
            }
            age = value;
        }
    }
}

```
