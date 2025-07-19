# Indexer Examples
## Simple Example

```csharp

public class SampleCollection
{
    private string[] items = new string[10];

    // Indexer declaration
    public string this[int index]
    {
        get { return items[index]; }
        set { items[index] = value; }
    }
}

class Program
{
    static void Main()
    {
        var collection = new SampleCollection();
        collection[0] = "Hello";
        Console.WriteLine(collection[0]);  // Output: Hello
    }
}
```

## Multiple Parameters Example
This example demonstrates how to use a multi-parameter indexer in C# by leveraging a tuple as the key in a dictionary.

```csharp

// Simulated entity model
public class Grade
{
    public int StudentId { get; set; }
    public int SubjectId { get; set; }
    public string GradeValue { get; set; }
}



// Simulated DbContext
public static class FakeDbContext
{
    // Simulate async EF call
    public static Task<List<Grade>> GetGradesAsync()
    {
        var grades = new List<Grade>
        {
            new Grade { StudentId = 1, SubjectId = 101, GradeValue = "A" },
            new Grade { StudentId = 1, SubjectId = 102, GradeValue = "B+" },
            new Grade { StudentId = 2, SubjectId = 101, GradeValue = "B" },
        };

        return Task.FromResult(grades);
    }
}

// Class with multi-parameter indexer
public class StudentGrades
{
    private readonly Dictionary<(int studentId, int subjectId), string> _grades = new();

    public string this[int studentId, int subjectId]
    {
        get => _grades.TryGetValue((studentId, subjectId), out var grade) ? grade : "N/A";
        set => _grades[(studentId, subjectId)] = value;
    }
}

// Entry point
public class Program
{
    public static async Task Main()
    {
        // Simulate: var allGrades = await _context.Grades.ToListAsync();
        var allGrades = await FakeDbContext.GetGradesAsync();

        var studentGrades = new StudentGrades();

        // Simulate: foreach (var g in allGrades) ...
        foreach (var g in allGrades)
        {
            studentGrades[g.StudentId, g.SubjectId] = g.GradeValue;
        }

        // Test lookups
        Console.WriteLine("Student 1, Subject 101: " + studentGrades[1, 101]); // A
        Console.WriteLine("Student 1, Subject 102: " + studentGrades[1, 102]); // B+
        Console.WriteLine("Student 2, Subject 101: " + studentGrades[2, 101]); // B
    }
}
```

This technique is best for read-heavy, stable, and performance-sensitive operations where DB hits must be minimized.   
Use it when you need to quickly access multi-keyed data.

