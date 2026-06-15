ASP.NET MVC – CHECKLIST RESUMIDO
1. Criar Projeto
 MVC Project (.NET 9)
 Authentication: Individual Accounts
2. Model
 Criar classe em Models
 Definir Id + campos
3. DbContext
 Adicionar DbSet no ApplicationDbContext
public DbSet<Category> Categories { get; set; }
4. Migration
 Package Manager Console
add-migration InitialCreate
update-database
5. Scaffold (CRUD)
 Controllers → Add → Scaffold
 MVC Controller + EF
 Selecionar Model + DbContext
6. Testar CRUD
 Run project
 Create / Read / Edit / Delete
7. Views
 Verificar @model
 Ajustar se necessário
8. Security
 [Authorize] se necessário
 Roles se pedido
