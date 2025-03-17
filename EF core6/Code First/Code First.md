# Code First Migration
## intro
Code First is a tool that migrates local database `localDB` by generating the code in `Migration` folder.

## How to use
### command line
The simplest way is to type command in Nuget Console in VS.

Step 1:

[open Nuget Manager Console](https://github.com/40843245/Visual-Studio/blob/main/Nuget%20Manager%20Console/How%20to/How%20to%20open%20Nuget%20Manager%20Console%20in%20VS%3F.md)

Step 2:

To enable migration, type these commands in Nuget Manager Console.

```
Enable-Migrations -ContextTypeName <contexTypeName>
```

where

`<contexTypeName>` is the name of context type that will be migrated.

> [!WARNING]
> If you want to overwrite the an existing migration, use `--Force` option.

> [!NOTE]
> If you want to auto migrate it, use `-EnableAutomaticMigrations`.

Step 3: 

To add migration, type these commands in Nuget Manager Console.

```
Add-Migration <migratedName>
```

where

`<migratedName>` can be any descriptive name you want.

> [!TIP]
> To make the project more readable and maintainable, I recommend that you specify `<migratedName>` with a descriptive and readable name.

Step 3:

Update the database by these commands.

```
Update-Database
```

If it's okay, you should see like this

<img width="437" alt="image" src="https://github.com/user-attachments/assets/f7152b95-6a87-4a77-9cb7-5dc1682ac248" />

## examples
### example 1
For example, I want to automatically migrate context `MediaManagerSystemContext` class.

<img width="239" alt="image" src="https://github.com/user-attachments/assets/792ccb00-97d4-4720-a136-3963f976ece1" />

`MediaManagerSystemContext.cs` under `..\MediaManagerSystem\Data` folder.

```
using System.Data.Entity;

namespace MediaManagerSystem.Data
{
    public class MediaManagerSystemContext : DbContext
    {
        // You can add custom code to this file. Changes will not be overwritten.
        // 
        // If you want Entity Framework to drop and regenerate your database
        // automatically whenever you change your model schema, please use data migrations.
        // For more information refer to the documentation:
        // http://msdn.microsoft.com/en-us/data/jj591621.aspx
    
        public MediaManagerSystemContext() : base("name=MediaManagerSystemContext")
        {
        }

        public DbSet<Models.FormBean.account_info> account_info { get; set; }

    }
}
```

Step 1:

I can enable migrate `MediaManagerSystemContext` through these commands in Nuget Manager Console.

```
Enable-Migrations -EnableAutomaticMigrations -ContextTypeName MediaManagerSystemContext
```

<img width="469" alt="image" src="https://github.com/user-attachments/assets/2f1e79ff-8e5b-4ea5-9460-a882291dcbd8" />

After executing the command, it will generate `Migration` folder and `Configuration.cs` file.

<img width="232" alt="image" src="https://github.com/user-attachments/assets/7d73897f-cc56-4d04-b217-df484aa63cc7" />

`Configuration.cs` in `..\MediaSystemManager\Migration` folder.

```
using System.Data.Entity.Migrations;

namespace MediaManagerSystem.Migrations
{
    internal sealed class Configuration : DbMigrationsConfiguration<Data.MediaManagerSystemContext>
    {
        public Configuration()
        {
            AutomaticMigrationsEnabled = true;
            ContextKey = "MediaManagerSystem.Data.MediaManagerSystemContext";
        }

        protected override void Seed(Data.MediaManagerSystemContext context)
        {
            //  This method will be called after migrating to the latest version.

            //  You can use the DbSet<T>.AddOrUpdate() helper extension method 
            //  to avoid creating duplicate seed data.
        }
    }
}
```

Step 2:

Then I can add migration to `MediaManagerSystemContext` through these commands.

```
Add-Migration MediaManagerSystemContextMigration
```

<img width="520" alt="image" src="https://github.com/user-attachments/assets/401c0eda-9047-4411-8fb8-931e5bdea5e2" />

Step 3:

Then I update the database `MediaManagerSystemContext` by these commands. 

```
Update-Database
```

<img width="437" alt="image" src="https://github.com/user-attachments/assets/7fa41c1c-1378-4d94-bc20-d4c72cf3622f" />

## reference
MSDS [Code First Migrations(in EF core6)](https://learn.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/?redirectedfrom=MSDN)
