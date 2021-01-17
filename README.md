# Creating-an-Entity-Data-Model
20487D_MOD02_DEMO_LESSON2


# Lesson 2: Creating an Entity Data Model

### Demonstration: Creating an Entity Type, DbContext, and DbInitializer

1. Open the Command Prompt window.

2. Create a new **ASP.NET Core Web API** project. At the command prompt, paste the following command, and then press Enter:

   ```bash
    dotnet new console --name MyFirstEF --output [Repository Root]\Allfiles\Mod02\DemoFiles\MyFirstEF\Starter
   ```

3. After the project is created, to change the directory, at the command prompt, run the following command:

   ```bash
    cd [Repository Root]\Allfiles\Mod02\DemoFiles\MyFirstEF\Starter
   ```

4. To use **Entity Framework Core**, install the following package from the command prompt:

   ```base
    dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version=2.1.1
    dotnet restore
   ```

5. To Open the project in Microsoft Visual Studio Code, paste the following command, and then press Enter:

   ```bash
    code .
   ```

6. To add a new folder named **Models**, right-click in the Explorer pane, and then select **New Folder**.

7. Right-click the **Models** folder, select **New File**, in the box on the top, type **Product.cs**, and then press Enter.

8. In **Product.cs**, paste the following code:

   ```cs
    namespace MyFirstEF.Models
    {
        public class Product
        {
            public int Id { get; set; }
            public string Name { get; set; }
        }
    }
   ```

9. Right-click the **Models** folder, select **New File**, in the box on the top, type **Store.cs**, and then press Enter.

10. In **Store.cs**, paste the following code:

    ```cs
     using System.Collections.Generic;
    
     namespace MyFirstEF.Models
     {
         public class Store
         {
              public int Id { get; set; }
              public string Name { get; set; }
              public IEnumerable<Product> Products { get; set; }
         }
     }
    ```

11. To add a new folder named **Database**, right-click in the Explorer pane, and then select **New Folder**.

12. Right-click the **Database** folder, select **New File**, in the box on the top, type **MyDbContext.cs**, and then press Enter.

13. In **MyDbContext.cs**, add the following using statements:

    ```cs
     using Microsoft.EntityFrameworkCore;
     using MyFirstEF.Models;
    ```

14. In **MyDbContext.cs**, add the following code:

    ```cs
     namespace MyFirstEF.Database
     {
         public class MyDbContext : DbContext
         {
             public DbSet<Product> Products { get; set; }
             public DbSet<Store> Stores { get; set; }
    
             protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
             {
                 optionsBuilder.UseSqlServer(@"Server=.\SQLEXPRESS;Database=MyFirstEF;Trusted_Connection=True;");
             }
         }
     }
    ```

15. Right-click the **Database** folder, select **New File**, in the box on the top, type **DbInitializer.cs**, and then press Enter.

16. In **DbInitializer.cs**, add the following code:

    ```cs
     namespace MyFirstEF.Database
     {
         public static class  DbInitializer
         {
             public static void Initialize(MyDbContext context)
             {
                 context.Database.EnsureCreated();
                 // Code to create initial data
             }
         }
     }
    ```

17. Navigate to **Program.cs** and add the following using statement:

    ```cs
     using MyFirstEF.Database;
    ```

18. In **Program.cs**, replace the **main** method with the following code:

    ```cs
     static void Main(string[] args)
     {
         using(var context = new MyDbContext())
         {
             DbInitializer.Initialize(context);
         }
         Console.WriteLine("Database created");
     }
    ```

19. To run the application, in command prompt, run the following command:

    ```bash
    dotnet run
    ```

20. Open **Azure Data Studio**.

21. Click **New Connection**. The **Connection** window will appear.

22. In the **Server** box, type **.\SQLEXPRESS**, and then click **Connect**.

23. On the **Server** blade, expand **.\sqlexpress**, and then expand **Database**.

24. Ensure the **MyFirstEF** database appears.

25. In **Server**, expand the **MyFirstEF** node, and then expand the **Tables** node.

26. Notice that both classes defined in the **MyFirstEF** project appear as tables, **dbo.Stores** and **dbo.Products**.

    >**Note**: Database tables are usually named in the plural form, which is why Entity Framework changed the names of the generated tables from Store and Product to Stores and Products. The dbo prefix is the name of the schema in which the tables were created.

27. Expand the **dbo.Products** and **dbo.Stores** tables, and then expand the **Columns** node in each of them to see that both the tables have **Id** and **Name** columns, similar to their corresponding class properties.

28. Close **Azure Data Studio**.

29. Close all open windows.
