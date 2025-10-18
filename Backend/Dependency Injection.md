# ⚙️ Service Lifetime Scopes in ASP.NET Core (Dependency Injection)

## 🧩 Overview

**Dependency Injection (DI)** is a built-in feature in ASP.NET Core that manages how and when service instances are created and disposed.
When registering a service, you specify its **lifetime**, which determines **how long the service instance lives** within the application.

There are **three main service lifetimes**:
👉 **Singleton**, **Scoped**, and **Transient**.

---

## 🏗️ 1. Singleton

**Lifetime:**
A single instance is created once and reused for the entire lifetime of the application.

**When to Use:**

* Application-wide state or configuration
* Caching mechanisms
* Logging services
* Services that are expensive to initialize but thread-safe

**Caution:**
Since the same instance is shared across multiple threads and requests, **thread-safety is critical** to prevent race conditions.

---

## 🔁 2. Scoped

**Lifetime:**
A new instance is created **once per HTTP request** and shared across all components handling that request.

**When to Use:**

* Database contexts (e.g., `DbContext` in EF Core)
* Repository or Unit of Work patterns
* Services maintaining per-request state

**Why:**
Ensures each request operates in isolation without sharing state across requests.

---

## ⚡ 3. Transient

**Lifetime:**
A **new instance** is created **every time** the service is requested — even within the same request.

**When to Use:**

* Lightweight and stateless services
* Services that must remain isolated from others
* Short-lived objects that are cheap to create

---

## 💻 Example: Registering Services

```csharp
var builder = WebApplication.CreateBuilder(args);

// Registering services with different lifetimes
builder.Services.AddSingleton<IMySingletonService, MySingletonService>();
builder.Services.AddScoped<IMyScopedService, MyScopedService>();
builder.Services.AddTransient<IMyTransientService, MyTransientService>();

var app = builder.Build();
```

---

## 📋 Summary Table

| Lifetime      | Instance per Application | Instance per Request | Instance per Injection |
| ------------- | ------------------------ | -------------------- | ---------------------- |
| **Singleton** | ✅ One                    | ✅ Same as app        | ✅ Same as app          |
| **Scoped**    | ❌                        | ✅ One                | ✅ Same as request      |
| **Transient** | ❌                        | ❌ Many               | ✅ New every time       |

---

## 💡 Interview Takeaway

* **Singleton:** One instance for the **entire application**.
* **Scoped:** One instance **per HTTP request** (most common for web apps).
* **Transient:** A **new instance** each time it’s requested.

➡️ **Scoped** is ideal for most business and database services since it maintains request-level consistency without sharing state globally.
