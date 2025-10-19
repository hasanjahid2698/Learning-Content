# 🔹 ViewData vs ViewBag vs TempData (ASP.NET Core MVC)

In ASP.NET Core MVC, these three mechanisms are used to **pass data from a controller to a Razor view**.
While they achieve similar goals, they differ in **type, lifetime, and use cases**.

---

## 1. ViewData

* **Type:** `ViewDataDictionary` (dictionary of string keys and object values)
* **How it works:** Set a value using a **string key** in the controller. In the view, retrieve it using the same key and **cast to the original type**.
* **Lifetime:** Lasts **only for the current request**; discarded after the view is rendered.

### **Example**

```csharp
// Controller
public IActionResult Index()
{
    ViewData["Message"] = "Hello from ViewData!";
    return View();
}

// View
<h2>@( (string)ViewData["Message"] )</h2>
```

---

## 2. ViewBag

* **Type:** `dynamic` object
* **How it works:** A **dynamic wrapper around ViewData**. Values can be accessed like properties, avoiding string keys and casting.
* **Lifetime:** Same as ViewData (**current request only**)
* **Behind the Scenes:** `ViewBag.Message` actually sets `ViewData["Message"]`. They are **interchangeable**.

### **Example**

```csharp
// Controller
public IActionResult Index()
{
    ViewBag.Message = "Hello from ViewBag!";
    return View();
}

// View
<h2>@ViewBag.Message</h2>
```

---

## 3. TempData

* **Type:** `ITempDataDictionary`
* **How it works:** Like a dictionary, but **persists data across requests**, usually via session or cookies. After the value is read, it is **marked for deletion**.
* **Lifetime:** **One subsequent request** (ideal for redirects).
* **Use Case:** One-time messages, e.g., “Your record was saved successfully.”

### **Example**

```csharp
// Controller
[HttpPost]
public IActionResult Create(Product product)
{
    // ... save product ...
    TempData["StatusMessage"] = "Product created successfully!";
    return RedirectToAction("Index"); 
}

// Index View
@if (TempData["StatusMessage"] != null)
{
    <div class="alert alert-success">@TempData["StatusMessage"]</div>
}
```

---

## 📊 Summary Table

| Feature      | ViewData             | ViewBag                 | TempData                               |
| ------------ | -------------------- | ----------------------- | -------------------------------------- |
| **Type**     | `ViewDataDictionary` | `dynamic`               | `ITempDataDictionary`                  |
| **Access**   | String keys + cast   | Property-like (dynamic) | String keys                            |
| **Lifetime** | Current request only | Current request only    | Next request (after redirect)          |
| **Use Case** | Pass data to view    | Pass data to view       | One-time messages / redirect scenarios |

---

## 🧠 Key Interview Takeaway

* **ViewData & ViewBag:** Pass data **during a single request**. ViewBag is a **dynamic wrapper** around ViewData.
* **TempData:** Persists data **across a redirect**, making it ideal for **one-time messages**.

