# Class 6.2 Summary – Local Authentication Done

This class was about **adding ASP.NET Core Identity (Authentication System)** and connecting the Store page to the **real database instead of fake/mock data**.

---

## 🔥 Big Picture

```text
User
 ↓
Website
 ↓
Identity (Login System)
 ↓
Database
 ↓
Categories / Products
```

Before:

```text
User
 ↓
Website
 ↓
Fake Categories in Controller
```

---

## 1. Add ASP.NET Identity Packages

Added in `.csproj`:

```xml
<PackageReference Include="Microsoft.AspNetCore.Identity.EntityFrameworkCore" />
<PackageReference Include="Microsoft.AspNetCore.Identity.UI" />
```

### Why?

ASP.NET Identity provides built-in:

- Login
- Register
- Password system
- Users & Roles

---

## Identity Tables Created Automatically

```text
AspNetUsers
AspNetRoles
AspNetUserRoles
AspNetUserClaims
```

---

## 2. Create ApplicationUser

```csharp
using Microsoft.AspNetCore.Identity;

namespace TechJockeys.Data;

public class ApplicationUser : IdentityUser
{
}
```

### Why?

Allows extending user data later:

```csharp
public class ApplicationUser : IdentityUser
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

---

## 3. Scaffold Identity Pages

Generated automatically:

```text
Areas/
 └── Identity/
      └── Pages/
```

Includes:

- Login
- Register
- Logout
- Error pages

---

## 4. Error Page

Simple error view:

```html
<h1>Error</h1>
```

---

## Error Model

```csharp
public class ErrorModel : PageModel
{
    public string? RequestId { get; set; }

    public bool ShowRequestId =>
        !string.IsNullOrEmpty(RequestId);

    public void OnGet()
    {
        RequestId =
            Activity.Current?.Id ??
            HttpContext.TraceIdentifier;
    }
}
```

---

## 5. Allow Anonymous

```csharp
[AllowAnonymous]
```

Used so error pages work without login.

---

## 6. View Imports

```csharp
@using Microsoft.AspNetCore.Identity
@using TechJockeys.Data
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
```

---

## 7. Dependency Injection (IMPORTANT)

```csharp
private readonly ApplicationDbContext _context;

public StoreController(ApplicationDbContext context)
{
    _context = context;
}
```

### What happens?

ASP.NET automatically provides DbContext.

---

## 8. Replace Fake Data with Database

Before:

```csharp
var categories = new List<Category>();

for(int i=1; i<16; i++)
{
    categories.Add(new Category
    {
        CategoryId = i,
        Name = "Category " + i
    });
}
```

---

After:

```csharp
var categories =
    _context.Category
        .OrderBy(c => c.Name)
        .ToList();
```

---

## SQL Equivalent

```sql
SELECT *
FROM Category
ORDER BY Name
```

---

## 9. Pass Data to View

```csharp
return View(categories);
```

---

## In View

```csharp
@model IEnumerable<Category>

@foreach (var category in Model)
{
    <p>@category.Name</p>
}
```

---

## 10. UI Improvement (Bootstrap Cards)

Before:

```html
<li>Category</li>
```

After:

```html
<div class="card">
    <h3>Category</h3>
</div>
```

---

## Grid System

```text
12 columns total
12 / 4 = 3 cards per row
```

---

## 11. Connection String

```json
"ConnectionStrings": {
  "DefaultConnection": "Server=...;Database=...;User Id=...;Password=...;TrustServerCertificate=True;"
}
```

---

# 💡 KEY CONCEPTS FOR EXAM

## Identity
- Login system built-in ASP.NET

## ApplicationUser
- Custom user class

## Dependency Injection
- ASP.NET passes DbContext automatically

## DbContext
- Connects app to database

## LINQ
- Queries database

```csharp
.Where()
.OrderBy()
.ToList()
```

---

# 🚀 FINAL SUMMARY

```text
Identity added
ApplicationUser created
Scaffold Identity pages
Error page configured
StoreController now uses DbContext
Fake data removed
Database data loaded
UI improved with Bootstrap cards
```
