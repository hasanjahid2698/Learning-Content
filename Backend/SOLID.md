# 🧱 SOLID Principles

## 🧩 Overview

**SOLID** is an acronym for **five key design principles** that help developers write **clean, maintainable, and scalable** object-oriented code.
They promote **loose coupling**, **high cohesion**, and **flexibility** in software design.

---

## 🧠 The SOLID Principles

### **S — Single Responsibility Principle (SRP)**

**Concept:**
A class should have **only one reason to change** — meaning it should have **one job or responsibility**.

**Why:**

* Improves code readability and testability.
* Reduces the chance of introducing bugs when modifying functionality.

**Example Use:**
Separate business logic, data access, and logging into distinct classes.

---

### **O — Open/Closed Principle (OCP)**

**Concept:**
Software entities (classes, modules, functions) should be **open for extension** but **closed for modification**.

**Why:**

* Encourages adding new features without altering existing, tested code.
* Typically achieved using **abstraction**, **inheritance**, or **interfaces**.

**Example Use:**
Add new payment methods by extending a base `PaymentProcessor` class instead of modifying it.

---

### **L — Liskov Substitution Principle (LSP)**

**Concept:**
Subclasses should be **substitutable for their base classes** without altering the correctness of the program.

**Why:**

* Ensures proper use of inheritance.
* Prevents unexpected behavior when replacing parent types with child types.

**Example Use:**
If `Bird` has a method `Fly()`, then a subclass like `Penguin` that cannot fly should not inherit it — or it should handle the behavior properly.

---

### **I — Interface Segregation Principle (ISP)**

**Concept:**
Clients should **not be forced to depend on methods they do not use**.
It’s better to have **multiple small, specific interfaces** rather than one large, generic one.

**Why:**

* Leads to a more decoupled and modular system.
* Reduces unnecessary dependencies and recompilation.

**Example Use:**
Instead of one large `IWorker` interface, create smaller interfaces like `ICoder`, `ITester`, etc.

---

### **D — Dependency Inversion Principle (DIP)**

**Concept:**

* High-level modules should **not depend on low-level modules**.
* Both should depend on **abstractions (interfaces)**.
* Abstractions should not depend on details — **details should depend on abstractions**.

**Why:**

* Promotes decoupling and easier maintenance.
* Simplifies swapping implementations (e.g., changing databases or APIs).
* Forms the basis of **Dependency Injection (DI)**.

**Example Use:**
A `UserService` depends on an `IUserRepository` interface instead of a specific database implementation.

---

## 💡 Key Interview Takeaway

The **SOLID principles** help create code that is:

* **Understandable** – each class has a clear purpose.
* **Extensible** – new features can be added easily.
* **Maintainable** – changes don’t break existing functionality.

> ✅ **In short:** SOLID promotes **single responsibility**, **extensibility**, and **dependence on abstractions** rather than concrete implementations.



