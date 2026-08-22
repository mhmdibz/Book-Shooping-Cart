# 📚 Book Shopping Cart

A full-featured online bookstore built with **ASP.NET Core MVC (.NET 10)**, supporting customer shopping, admin management, stock tracking, and order processing — all in one clean, role-based web application.

---

## 🖼️ Overview

Book Shopping Cart is a multi-role e-commerce web application that allows customers to browse books, manage their cart, and place orders — while giving admins full control over inventory, genres, stock levels, and order fulfillment. The app exposes 30+ controller actions across its MVC controllers.

---

## ✨ Features

### 👤 Customer
- Browse and search books by genre
- Add/remove items from shopping cart
- View cart with real-time item count
- Checkout with delivery & payment details
- Track order history and status

### 🛠️ Admin
- Add, update, and delete books (with image upload)
- Manage book genres
- Manage stock levels per book
- View and update order statuses
- Sales reports — including Top N Selling Books (via Stored Procedure)
- Admin operations panel

---

## 🔧 Technical Highlights

- **Database-level search instead of in-memory filtering** — the book catalog query is built incrementally with `IQueryable`, chaining `Where` clauses conditionally based on `search term` and `genreId`, and is only materialized at the final `ToListAsync()`. This means filtering happens in SQL Server, not by pulling every book into memory first.
- **Read-optimized queries** — catalog and listing queries use `AsNoTracking()` together with field projection (selecting only the columns a view needs) to reduce EF Core's change-tracking overhead and the amount of data pulled per request.
- **Atomic checkout** — placing an order touches multiple related writes: creating the `Order`, creating its `OrderDetails`, validating and deducting `Stock`, and clearing the user's cart. All of these are wrapped in a single database transaction, so a failure partway through rolls back every step instead of leaving the order half-committed.
- **Stock validation before deduction** — available quantity is checked against the requested quantity *before* stock is decremented, preventing an order from being placed for more copies than are actually in stock.

---

## 🏗️ Tech Stack

| Layer | Technology |
|---|---|
| Framework | ASP.NET Core MVC (.NET 10) |
| ORM | Entity Framework Core 10 |
| Database | SQL Server |
| Authentication | ASP.NET Core Identity |
| Authorization | Role-Based (Admin / User) |
| UI | Razor Views + Bootstrap |
| File Storage | Local file system (image uploads) |
| Pattern | Repository Pattern + Dependency Injection |

---

## 🗂️ Project Structure

```
BookShoppingCart-Mvc/
└── BookShoppingCartMvcUI/
    ├── Controllers/          # MVC Controllers (Book, Cart, Genre, Stock, Order, Reports)
    ├── Models/               # Domain models (Book, Order, CartDetail, Stock ...)
    │   └── DTOs/             # Data Transfer Objects
    ├── Repositories/         # Repository interfaces and implementations
    ├── Data/
    │   ├── ApplicationDbContext.cs
    │   └── DbSeeder.cs       # Seeds roles, admin user, and default genres
    ├── Areas/Identity/       # Scaffolded ASP.NET Identity pages
    ├── Constants/            # Roles and PaymentMethods enums
    ├── Shared/               # FileService for image upload/delete
    ├── Migrations/           # EF Core database migrations
    ├── Views/                # Razor Views per controller
    ├── Program.cs
    └── appsettings.json
```

---

## ⚙️ Getting Started

### Prerequisites

- [.NET 10 SDK](https://dotnet.microsoft.com/download)
- SQL Server (local or remote)
- Visual Studio 2022+ or VS Code

### 1. Clone the repository

```bash
git clone https://github.com/mhmdibz/Book-Shooping-Cart.git
cd Book-Shooping-Cart
```

### 2. Configure the database connection

Open `BookShoppingCartMvcUI/appsettings.json` and update the connection string:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=YOUR_SERVER;Database=BookShoppingCartDb;Trusted_Connection=True;TrustServerCertificate=True;"
  }
}
```

### 3. Run the application

```bash
cd BookShoppingCartMvcUI
dotnet run
```

> On first run, the app automatically applies all pending migrations and seeds default data (roles, admin user, and genres).

### 4. Login as Admin

| Field | Value |
|---|---|
| Email | `admin@gmail.com` |
| Password | `Admin@123` |

> Demo credentials for local evaluation only — seeded by `DbSeeder` on first run.

---

## 🗃️ Database Schema

Key entities and their relationships:

```
Book ──────┬── Genre
           ├── Stock          (1-to-1)
           ├── CartDetail     (many-to-many via ShoppingCart)
           └── OrderDetail    (many-to-many via Order)

Order ─────┬── OrderStatus
           ├── OrderDetail
           └── IdentityUser

ShoppingCart ── CartDetail ── Book
```

---

## 🔐 Roles & Authorization

| Role | Access |
|---|---|
| **Admin** | Full access — books, genres, stock, orders, reports |
| **User** | Browse books, manage own cart, place & view own orders |
| **Guest** | Browse books only (no cart or checkout) |

Roles and the default admin account are automatically seeded on first startup via `DbSeeder`.

---

## 📦 NuGet Packages

```xml
Microsoft.AspNetCore.Identity.EntityFrameworkCore     10.0.0
Microsoft.AspNetCore.Identity.UI                      10.0.0
Microsoft.EntityFrameworkCore.SqlServer               10.0.0
Microsoft.EntityFrameworkCore.Tools                   10.0.0
Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore  10.0.0
Microsoft.AspNetCore.Components.QuickGrid.EntityFrameworkAdapter  10.0.0
```

---

## 📊 Reports

The Reports section uses a **SQL Server Stored Procedure** to fetch the Top N best-selling books, demonstrating raw SQL integration alongside EF Core.

---

## 🖼️ Image Upload

- Supported formats: `.jpg`, `.jpeg`, `.png`
- Max file size: **1 MB**
- Handled by `FileService` which saves/deletes files from the server's `wwwroot/images` folder

---

## 🚀 Roadmap / Future Improvements

- [ ] Add unit & integration tests (xUnit + Moq)
- [ ] Integrate structured logging (Serilog)
- [ ] Add payment gateway integration
- [ ] Expose a REST API layer
- [ ] Migrate to Clean Architecture

---

## Author

**Mohamed Ibrahim Zaki**
Backend Engineer (ASP.NET Core) & Computer Science Student
[GitHub](https://github.com/mhmdibz) · [LinkedIn](https://www.linkedin.com/in/mohamed-ibrahim-dev-eg/)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
