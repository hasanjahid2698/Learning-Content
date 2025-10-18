# 🔹 Interface vs Abstract Class (C#)

In C#, both **interfaces** and **abstract classes** provide **abstraction** — they define contracts that other classes can conform to.
However, they serve **different design purposes** and behave differently in inheritance and implementation.

---

## 🧩 Interface

* **What it is:** A **pure contract** — defines *what* a class can do, but not *how* it does it.
* **Inheritance:** A class or struct can **implement multiple interfaces**, enabling a form of multiple inheritance.
* **Members:**

  * Cannot contain fields or constructors.
  * Contains only member signatures (methods, properties, events).
  * All members are **implicitly public**.
  * Modern C# supports **default implementations**, but primary intent remains abstraction.
* **Relationship:** Defines a **“can-do”** relationship.
  Example: both `Car` and `Bicycle` can implement `IMovable`.

### **C# Example**

```csharp
public interface ILogger
{
    void LogMessage(string message);
    void LogError(string error);
}

public class FileLogger : ILogger
{
    public void LogMessage(string message) { /* write to file */ }
    public void LogError(string error) { /* write to file */ }
}
```

---

## 🧱 Abstract Class

* **What it is:** A **base class** that is incomplete and must be inherited from.
  It can include both **abstract (unimplemented)** and **concrete (implemented)** members.
* **Inheritance:** A class can **inherit from only one** abstract (or any) class.
* **Members:**

  * Can include fields, constructors, methods, and properties.
  * Can have **fully implemented methods** shared among subclasses.
* **Relationship:** Defines an **“is-a”** relationship.
  Example: `CheckingAccount` and `SavingsAccount` are both `BankAccount`s.

### **C# Example**

```csharp
public abstract class Shape
{
    // Concrete member shared by all subclasses
    public string Color { get; set; }

    // Abstract member that MUST be implemented by subclasses
    public abstract double CalculateArea(); 
}

public class Circle : Shape
{
    public double Radius { get; set; }
    public override double CalculateArea() => Math.PI * Radius * Radius;
}
```

---

## 📊 Summary Table

| **Feature**               | **Interface**                            | **Abstract Class**                                   |
| ------------------------- | ---------------------------------------- | ---------------------------------------------------- |
| **Multiple Inheritance**  | ✅ Yes — a class can implement many.      | ❌ No — can inherit from only one.                    |
| **Member Implementation** | ❌ No (except default methods in C# 8+).  | ✅ Can contain both abstract and implemented members. |
| **Fields / Constructors** | ❌ Not allowed.                           | ✅ Allowed.                                           |
| **Relationship**          | "Can-do" (e.g., `IEnumerable`)           | "Is-a" (e.g., `Stream`)                              |
| **Primary Purpose**       | Define a contract for unrelated classes. | Share common code among related classes.             |

---

## 🧠 Key Interview Takeaway

* Use an **interface** when defining a **contract** that can be implemented by *any class*, regardless of its inheritance hierarchy.
* Use an **abstract class** when creating a **common base** that provides **shared code and structure** for a group of closely related classes.
