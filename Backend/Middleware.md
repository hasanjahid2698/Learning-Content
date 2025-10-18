# 🧩 ASP.NET Core Middleware

## Overview

**Middleware** in ASP.NET Core are software components arranged in a pipeline to handle **HTTP requests and responses**.
Each middleware performs specific tasks — such as authentication, routing, or logging — and can either:

* **Pass** the request to the next middleware (`await next(context)`), or
* **Short-circuit** the pipeline and return a response immediately.

This modular pipeline design makes request handling **flexible, maintainable, and powerful**.

---

## 🔄 Request Pipeline Concept

Think of the pipeline as an **assembly line**:
Each incoming HTTP request moves through middleware components sequentially.
Each component can inspect, modify, or handle the request before passing it on.
The response then flows back through the same components, allowing further processing or modification.

---

## ⚙️ Key Principles

### 1. **Order Matters**

Middleware execute **in the order they are added** in `Program.cs`.

* Earlier middleware handle requests first and responses last.
* Incorrect ordering can break functionality (e.g., placing `UseAuthorization` before `UseRouting`).

### 2. **next() Delegate**

Each middleware receives a `RequestDelegate next`.
Calling `await next(context)` passes control forward.
If omitted, the pipeline stops early — useful for cases like authentication failure.

---

## 🧱 Common Middleware Examples

| Middleware            | Purpose                                        |
| --------------------- | ---------------------------------------------- |
| `UseExceptionHandler` | Catches unhandled exceptions.                  |
| `UseStaticFiles`      | Serves static content (e.g., CSS, JS, images). |
| `UseRouting`          | Maps incoming requests to endpoints.           |
| `UseAuthentication`   | Identifies the current user.                   |
| `UseAuthorization`    | Checks access permissions.                     |

---

## 🧭 Example: Program.cs

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.UseExceptionHandler("/Error");    // Handles exceptions
app.UseStaticFiles();                  // Serves static assets
app.UseRouting();                      // Enables routing
app.UseAuthorization();                // Applies authorization
app.MapControllers();                  // Maps controller endpoints

app.Run();
```

---

## 💡 Interview Takeaway

Middleware forms a **chain of responsibility** for handling HTTP requests.
The **execution order** is critical, and each component can either **continue** or **terminate** the pipeline.
