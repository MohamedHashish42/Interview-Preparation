# **O**pen/Closed Principle (OCP)

## ⚠️ Without OCP

```csharp
public class InvoiceWithout
{
    public double GetInvoiceDiscount(double amount, InvoiceType invoiceType)
    {
        double finalAmount = 0;

        if (invoiceType == InvoiceType.ProposedInvoice)
        {
            finalAmount = amount - 50;
        }
        else if (invoiceType == InvoiceType.FinalInvoice)
        {
            finalAmount = amount - 100;
        }

        // Need to modify this method if a new type is added
        return finalAmount;
    }
}

public enum InvoiceType
{
    ProposedInvoice,
    FinalInvoice
}
```


### Why this violates OCP:

> This implementation violates the Open/Closed Principle because any time a new invoice type is introduced, we must modify the `GetInvoiceDiscount` method. The class is **not closed for modification**.


## ✅ With OCP (Correct Approach)

### Define an Interface

```csharp
public interface IInvoiceDiscount
{
    double GetInvoiceDiscount(double amount);
}
```

### Implement Invoice Types

```csharp
public class ProposedInvoiceDiscount : IInvoiceDiscount
{
    public double GetInvoiceDiscount(double amount)
    {
        return amount - 50;
    }
}

public class FinalInvoiceDiscount : IInvoiceDiscount
{
    public double GetInvoiceDiscount(double amount)
    {
        return amount - 100;
    }
}
```

### Use a Calculator

```csharp
public class InvoiceDiscountCalculator
{
    public double ApplyDiscount(double amount, IInvoiceDiscount invoiceDiscount)
    {
        return invoiceDiscount.GetInvoiceDiscount(amount);
    }
}
```

### Why this adheres to OCP:

> In this design, the core logic remains unchanged. To support new types of discounts, we simply create new classes that implement `IInvoiceDiscount`. This adheres to the Open/Closed Principle by being **open for extension but closed for modification**.

