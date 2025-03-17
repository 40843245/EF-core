# Code First
## intro
Code First is a tool that migrates local database `localDB` by generating the code in `Migration` folder.

## How to use
### command line
The simplest way is to type command in Nuget Console in VS.

Step 1:

[open Nuget Manager Console](https://github.com/40843245/Visual-Studio/blob/main/Nuget%20Manager%20Console/How%20to/How%20to%20open%20Nuget%20Manager%20Console%20in%20VS%3F.md)

Step 2:

To enable migration, type command in Nuget Manager Console.

```
Enable-Migrations -ContextTypeName <contexTypeName>
```

where

`<contexTypeName>` is the name of context type that will be migrated.

> [!WARNING]
> If you want to overwrite the an existing migration, use `--Force` option.

> [!NOTE]
> If you want to auto migrate it, use `-EnableAutomaticMigrations`.

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

I can type these command in Nuget Manager Console.

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
