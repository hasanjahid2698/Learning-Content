# 🔁 `ref` vs `out` Keywords in C#

In C#, arguments are typically **passed by value**, meaning the method receives a **copy** of the variable.
The **`ref`** and **`out`** keywords are used to **pass arguments by reference**, allowing the method to operate directly on the original variable.

When passed by reference, the method receives a **reference (pointer)** to the variable — not a copy.

---

## ⚙️ `ref` Keyword

### **Rule**

The variable **must be initialized** before it is passed to the method.

### **Behavior**

* The method can **read and modify** the value.
* It’s a **two-way communication**: data flows *into* and *out of* the method.

### **Use Case**

When you want a method to **modify an existing variable**.

### **C# Example**

```csharp
void IncrementValue(ref int number)
{
    // I can read and modify the value of 'number'
    number = number + 5; 
}

// Must be initialized before passing
int myVar = 10; 
IncrementValue(ref myVar); 

Console.WriteLine(myVar); // Output: 15
```

✅ **Explanation:**
The method receives a reference to `myVar` and modifies its original value directly.

---

## 📤 `out` Keyword

### **Rule**

The variable **does not need to be initialized** before being passed.

### **Behavior**

* The method **must assign** a value to the variable **before returning**.
* The variable’s existing value **cannot be read** inside the method.
* It’s a **one-way communication**: data flows *out of* the method.

### **Use Case**

When you want a method to **return multiple values**.
A common example is the `int.TryParse` method.

### **C# Example**

```csharp
void GetCoordinates(out int x, out int y)
{
    // Must assign values before the method exits
    x = 100;
    y = 200;
}

// No need to initialize before calling
int locationX; 
int locationY;
GetCoordinates(out locationX, out locationY);

Console.WriteLine($"X: {locationX}, Y: {locationY}"); // Output: X: 100, Y: 200
```

✅ **Explanation:**
The method assigns values to the output variables before returning, which are then accessible to the caller.

---

## 📊 Summary Table

| **Feature**         | **`ref`**                       | **`out`**                            |
| ------------------- | ------------------------------- | ------------------------------------ |
| **Initialization**  | Must be initialized before call | Does not need to be initialized      |
| **Value in Method** | Can be **read and written**     | Must be **written** before returning |
| **Primary Purpose** | Modify an existing variable     | Return multiple values from a method |

---

## 🧠 Key Interview Takeaway

Both **`ref`** and **`out`** pass variables **by reference**.
The key differences are:

* **Initialization:** `ref` requires initialization; `out` does not.
* **Usage:** `ref` is for **modifying** an existing value; `out` is for **returning** new values.
