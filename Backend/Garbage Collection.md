# 🧹 Garbage Collection (GC) — Overview

## What is Garbage Collection?

**Garbage Collection (GC)** is the automatic memory management feature of the **.NET runtime**.
In languages without GC (like **C++**), developers must manually allocate and deallocate memory — forgetting to free memory can lead to **memory leaks**.

In **.NET**, the **Garbage Collector (GC)** automatically tracks memory usage and **reclaims memory** that is no longer being used by the application.

---

## ⚙️ How It Works: The Mark and Compact Algorithm

1. **Mark:**
   The GC starts from the application's *roots* (such as global objects, static fields, and local variables on the current thread's stack) and traverses the object graph.
   It **marks all reachable objects** as *live*.

2. **Sweep / Compact:**

   * Any object **not marked** is considered *garbage* because it is unreachable.
   * The GC **frees memory** occupied by these garbage objects.
   * To reduce **memory fragmentation**, it may also **compact live objects** by moving them together in memory.

---

## 🧬 Generational Garbage Collection

To improve performance, the .NET GC is **generational**, based on the idea that **young objects die young**, while **older objects live longer**.

The managed heap is divided into **three generations**:

| Generation | Description                                                     | Frequency            | Common Objects                      |
| ---------- | --------------------------------------------------------------- | -------------------- | ----------------------------------- |
| **Gen 0**  | All new, short-lived objects are allocated here.                | Very frequent (fast) | Temporary objects, method variables |
| **Gen 1**  | Objects that survive Gen 0 are promoted here; acts as a buffer. | Moderate             | Mid-lived objects                   |
| **Gen 2**  | Objects that survive Gen 1; long-lived objects.                 | Rare (slow, full GC) | Singletons, static objects          |

📝 **Note:**
A **Gen 2 collection** is a *full garbage collection* and can cause noticeable application pauses.
The GC tries to minimize these by running them **infrequently**.

---

## 🧰 Unmanaged Resources

The GC only manages **memory allocated by the .NET runtime** (managed memory).
It does **not** clean up **unmanaged resources** such as:

* Database connections
* File handles
* Network sockets

To properly clean up unmanaged resources:

* Implement the **`IDisposable`** interface.
* Use a **`try-finally`** block or the **`using`** statement to ensure the `Dispose()` method is called.

```csharp
using (var connection = new SqlConnection(connectionString))
{
    connection.Open();
    // Use the connection
} // connection.Dispose() is automatically called here
```

---

## 🧩 Key Interview Takeaway

* The **Garbage Collector** automatically **reclaims memory** from unreachable objects.
* It is **generational (Gen 0, 1, 2)** — optimizing performance by collecting short-lived objects more frequently.
* **Unmanaged resources** must be handled **manually** using the **`IDisposable`** pattern.
