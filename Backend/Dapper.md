# ⚡ Dapper

## 🧩 What is Dapper?

**Dapper** is a **simple, fast, and lightweight micro ORM (Object-Relational Mapper)** for .NET.
It helps developers interact with databases using **C# objects** instead of raw SQL everywhere.

### 🆚 ORM Comparison

* **Full ORMs (e.g., Entity Framework Core):**

  * Feature-rich (auto SQL generation, change tracking, migrations).
  * More complex and has performance overhead.

* **Micro ORMs (e.g., Dapper):**

  * Focused mainly on **mapping query results** to C# objects.
  * Extremely fast with minimal overhead.

---

## 🚀 Why Use Dapper?

### ⚡ Performance

* Often called the **“king of micro ORMs”** for its speed.
* Nearly as fast as raw **ADO.NET DataReader**, which is the baseline for database performance in .NET.

### 🎯 Control

* You write the **SQL manually**, giving **full control** and **optimization** freedom.
* Unlike full ORMs, there’s **no auto-generated SQL** that could impact efficiency.

### 🧠 Simplicity

* Dapper is just a **set of extension methods** for `IDbConnection`.
* Easy to learn and integrate — minimal configuration and setup.

---

## ⚙️ How It Works

1. Open a database connection.
2. Write a SQL query.
3. Use Dapper extension methods like `Query`, `QueryFirstOrDefault`, or `Execute`.
4. Dapper automatically maps query results to C# object properties **based on matching column names**.

---

## 💻 Example: Using Dapper

```csharp
// 1. Define your C# model
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}

// 2. Use Dapper in your repository
public class ProductRepository
{
    private readonly IDbConnection _dbConnection;

    public ProductRepository(IDbConnection dbConnection)
    {
        _dbConnection = dbConnection;
    }

    public async Task<IEnumerable<Product>> GetAllProductsAsync()
    {
        // 3. Write your SQL query
        var sql = "SELECT Id, Name, Price FROM Products";

        // 4. Execute and map results
        var products = await _dbConnection.QueryAsync<Product>(sql);
        return products;
    }

    public async Task InsertProductAsync(Product product)
    {
        var sql = "INSERT INTO Products (Name, Price) VALUES (@Name, @Price)";
        
        // ExecuteAsync is used for INSERT, UPDATE, DELETE
        await _dbConnection.ExecuteAsync(sql, new { product.Name, product.Price });
    }
}
```

✅ In this example:
Dapper maps the **`Id`, `Name`, and `Price`** columns from the SQL result to the corresponding **properties** of the `Product` class.
It also handles **parameterized queries** safely to prevent SQL injection.

---

## 💡 Key Interview Takeaway

* **Dapper** is a **high-performance micro ORM** ideal for scenarios where you need **speed** and **SQL control**.
* Faster and simpler than Entity Framework, but requires **manual SQL writing**.
* Best suited for applications where **performance and precision** are more important than abstraction.

