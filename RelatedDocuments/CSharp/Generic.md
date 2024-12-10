# **Generic Example**

## 1. **Generic Class**:
   The `List<T>` class is a generic collection in the .NET Framework.
   ```csharp
   List<int> numbers = new List<int> { 1, 2, 3 };   // List of integers
   List<string> words = new List<string> { "apple", "banana" }; // List of strings
   ```
   In this example, `List<T>` can store any type (`int`, `string`, etc.). The type is specified when the list is declared.

## 2. **Generic Field**:
  ```csharp
  class DataStore<T>
  {
    public T data;
  }
  ```
## 2. **Generic Method**:
   You can define methods that work with any type using type parameters.
   ```csharp
   public static int FindIndex<T>(IList<T> list, T item)
   {
     for (int i = 0; i < list.Count; i++)
     {
         if (list[i].Equals(item))
         {
             return i;
         }
     }

     return -1;
   }

   List<string> fruits = new List<string> { "apple", "banana", "orange" };
   int index = FindIndex(fruits, "banana");
   Console.WriteLine(index); // Output: 1
   ```

## 3. **Generic Constraint**:
   You can apply constraints to generics to specify the types that can be used.
   ```csharp
   public class Repository<T> where T : class
   {
       public void Add(T item)
       {
           // Implementation
       }
   }
   ```
   In this example, the `Repository<T>` class only works with reference types (`where T : class`).
