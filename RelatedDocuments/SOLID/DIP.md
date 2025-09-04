
# **D**ependency Inversion Principle (DIP)


## ⚠️ Without DIP

```csharp
public class Employee
{
    public int ID { get; set; }
    public string Name { get; set; }
    public string Department { get; set; }
    public double Salary { get; set; }
}
```

```csharp
public class EmployeeDataAccess
{
    public Employee GetEmployeeDetails(int id)
    {
        // Simulated DB call
        return new Employee
        {
            ID = id,
            Name = "Pranaya",
            Department = "IT",
            Salary = 10000
        };
    }
}
```
```csharp
public class EmployeeBusinessLogic
{
    private readonly EmployeeDataAccess _employeeDataAccess;

    public EmployeeBusinessLogic()
    {
        _employeeDataAccess = new EmployeeDataAccess(); // tightly coupled
    }

    public Employee GetEmployeeDetails(int id)
    {
        return _employeeDataAccess.GetEmployeeDetails(id);
    }
}
```

### Why this violates DIP:

> `EmployeeBusinessLogic` (high-level) directly depends on `EmployeeDataAccess` (low-level). This causes:  

* ❌ **Tight coupling** 
* ❌ **Hard to test** (can't mock `EmployeeDataAccess`)
* ❌ **Low flexibility for future changes**  (e.g., switching to a file or API source)


## ✅ With DIP

```csharp
public interface IEmployeeDataAccess
{
    Employee GetEmployeeDetails(int id);
}
```

```csharp
public class EmployeeDataAccess : IEmployeeDataAccess
{
    public Employee GetEmployeeDetails(int id)
    {
        return new Employee
        {
            ID = id,
            Name = "Pranaya",
            Department = "IT",
            Salary = 10000
        };
    }
}
```

```csharp
public class EmployeeBusinessLogic
{
    private readonly IEmployeeDataAccess _employeeDataAccess;

    // Constructor injection
    public EmployeeBusinessLogic(IEmployeeDataAccess employeeDataAccess)
    {
        _employeeDataAccess = employeeDataAccess;
    }

    public Employee GetEmployeeDetails(int id)
    {
        return _employeeDataAccess.GetEmployeeDetails(id);
    }
}
```
### Why this adheres to DIP:

> Now both `EmployeeBusinessLogic` and `EmployeeDataAccess` depend on the **abstraction** `IEmployeeDataAccess`.

* ✔️ **Loosely coupled**: High-level module doesn't care how data is fetched
* ✔️ **Testable**: You can inject a mock or fake implementation during unit tests
* ✔️ **Flexible**: You can swap data access sources (e.g., API, file, in-memory) without touching `EmployeeBusinessLogic`
* ✔️ **Follows SOLID** principles, particularly **Open/Closed** and **DIP**

