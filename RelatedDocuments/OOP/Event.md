# **Event Examples**

## 1. **Simple Example**

### Event Publisher:
```csharp
public class Publisher
{
    public event EventHandler MyEvent;

    // Method to raise the event
    public void RaiseEvent()
    {
        Console.WriteLine("Raising event...");
        MyEvent?.Invoke(this, EventArgs.Empty);
    }
}
```

### Event Subscriber:
```csharp
public class Subscriber
{
    public void OnMyEvent(object sender, EventArgs e)
    {
        Console.WriteLine("Event handled by Subscriber.");
    }
}
```

### Using the Event:
```csharp
class Program
{
    static void Main()
    {
        Publisher publisher = new Publisher();
        Subscriber subscriber = new Subscriber();

        // Subscribe to the event
        publisher.MyEvent += subscriber.OnMyEvent;

        // Raise the event
        publisher.RaiseEvent();
    }
}
```
**Output:**
```
Raising event...
Event handled by Subscriber.
```

## 2. **Example using Custom Event Argument:**
### Custom Event Argument:
```csharp
public class MyCustomEventArgs : EventArgs
{
    public string Message { get; set; }

    public MyCustomEventArgs(string message)
    {
        Message = message;
    }
}
```

### Event Publisher:
```csharp
public class Publisher
{
    public event EventHandler<MyCustomEventArgs> MyEvent;

    // Method to raise the event
    public void RaiseEvent(string message)
    {
        Console.WriteLine("Raising event...");
        MyEvent?.Invoke(this, new MyCustomEventArgs(message)); // Invoke event with custom args
    }
}
```

### Event Subscriber:
```csharp
public class Subscriber
{
    public void OnMyEvent(object sender, MyCustomEventArgs e)
    {
        Console.WriteLine($"Event handled by Subscriber. Message: {e.Message}");
    }
}
```

### Using the Event:
```csharp
class Program
{
    static void Main()
    {
        Publisher publisher = new Publisher();
        Subscriber subscriber = new Subscriber();

        // Subscribe to the event
        publisher.MyEvent += subscriber.OnMyEvent;

        // Raise the event
        publisher.RaiseEvent("Hello from custom event!");
    }
}
```

**Output:**
```
Raising event...
Event handled by Subscriber. Message: Hello from custom event!
```


## 3. **More Practical Example**
Suppose we have a Student class that contains an event, StatusChanged, which is triggered whenever the student's status changes. This class includes a property Status, which, when set to a new value, checks if the value differs from the current one. If the status is different, it triggers the OnStatusChanged method, which raises the event to notify any subscribers.

In this scenario, we have two subscribers: the Accounting class and the Transportation class. Both of these classes listen for the StatusChanged event and implement their respective handlers (OnStudentStatusChanged). 

### Event Publisher:
```csharp
public class Student
{
    public event EventHandler<string> StatusChanged;

    private string _status;
    public string Status
    {
        get => _status;
        set
        {
            if (_status != value)
            {
                _status = value;
                OnStatusChanged(value);
            }
        }
    }

    protected virtual void OnStatusChanged(string newStatus)
    {
        StatusChanged?.Invoke(this, newStatus);
    }
}
```
### Event Subscribers:
```csharp
public class Accounting
{
    public void OnStudentStatusChanged(object sender, string newStatus)
    {
        Console.WriteLine($"[Accounting] Student's status changed to: {newStatus}. Updating tuition fees.");
    }
}
public class Transportation
{
    public void OnStudentStatusChanged(object sender, string newStatus)
    {
        Console.WriteLine($"[Transportation] Student's status changed to: {newStatus}. Updating bus information.");
    }
}
```
### Using the Event:
```csharp
class Program
{
    static void Main(string[] args)
    {
        var student = new Student();

        // Create entities that subscribe to the event
        var accounting = new Accounting();
        var transportation = new Transportation();

        // Subscribe to the event
        student.StatusChanged += accounting.OnStudentStatusChanged;
        student.StatusChanged += transportation.OnStudentStatusChanged;

        // Change the student's status
        student.Status = "Passed"; // Event triggered: student passed
        student.Status = "Failed"; // Event triggered: student failed
    }
}
```

### Output:
```
[Accounting] Student's status changed to: Passed. Updating tuition fees.
[Transportation] Student's status changed to: Passed. Updating bus information.
[Accounting] Student's status changed to: Failed. Updating tuition fees.
[Transportation] Student's status changed to: Failed. Updating bus information.
```
