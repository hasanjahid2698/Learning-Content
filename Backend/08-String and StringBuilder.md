# 🔤 String vs StringBuilder in C#

The choice between **`String`** and **`StringBuilder`** in C# is a key performance consideration when performing string manipulations.
The main difference lies in **immutability**.

---

## 🧱 `System.String`

### **Concept**

A `string` in C# is **immutable** — once created, its value **cannot be changed**.

### **How It Works**

When you perform operations like concatenation (`+`), the **.NET runtime** creates a **new string object** in memory and discards the old one.

Each modification results in **a new memory allocation**, making repeated operations inefficient.

### **Performance**

* Fine for a small number of modifications.
* **Very inefficient** for loops or repeated concatenations.
* Causes **excessive memory allocations** and **GC pressure**.

### **Example — String Inefficiency**

```csharp
string s = "start";
// This loop creates 1001 string objects in memory!
// ("start", "start0", "start01", "start012", ...)
for (int i = 0; i < 1000; i++)
{
    s = s + i;
}
```

⚠️ Every iteration creates a **new string** → **1000+ allocations!**

---

## ⚙️ `System.Text.StringBuilder`

### **Concept**

A `StringBuilder` is **mutable** — it can change its contents without creating new objects.

### **How It Works**

* Maintains an **internal character buffer**.
* When you append data, it modifies this buffer directly.
* If the buffer becomes too small, it expands automatically — but far less frequently than a string reallocation.

### **Performance**

* Ideal for repeated string concatenation.
* **More memory-efficient** and **significantly faster** in loops.

### **Example — StringBuilder Efficiency**

```csharp
// Only one StringBuilder object is created
StringBuilder sb = new StringBuilder("start");
for (int i = 0; i < 1000; i++)
{
    // The internal buffer is modified in place
    sb.Append(i);
}
string result = sb.ToString(); // Final string created once at the end
```

✅ Efficient and optimized — only one main allocation.

---

## 📊 Summary Comparison

| Feature          | `String`                           | `StringBuilder`                         |
| ---------------- | ---------------------------------- | --------------------------------------- |
| **Mutability**   | Immutable                          | Mutable                                 |
| **Memory Usage** | Creates new object for each change | Reuses internal buffer                  |
| **Performance**  | Slower for repeated concatenations | Much faster for repeated concatenations |
| **Use Case**     | Small, fixed strings               | Large or frequently modified strings    |
| **Namespace**    | `System`                           | `System.Text`                           |

---

## 🧠 Key Interview Takeaway

* **`String`**: Immutable — every modification creates a new object.
* **`StringBuilder`**: Mutable — modifies an internal buffer directly.
* Use `StringBuilder` when performing **repeated or large-scale string manipulations** (e.g., loops, concatenations).
