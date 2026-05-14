# 📚 Book Shopping Cart

A full-featured online bookstore built with **ASP.NET Core MVC (.NET 10)**, supporting customer shopping, admin management, stock tracking, and order processing — all in one clean, role-based web application.

---

## 🖼️ Overview

Book Shopping Cart is a multi-role e-commerce web application that allows customers to browse books, manage their cart, and place orders — while giving admins full control over inventory, genres, stock levels, and order fulfillment.

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
git clone https://github.com/your-username/BookShoppingCart-Mvc.git
cd BookShoppingCart-Mvc
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

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
