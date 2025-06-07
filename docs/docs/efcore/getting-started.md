# Getting Started

![NuGet Version](https://img.shields.io/nuget/v/Qrtix.RepositoryPattern.EntityFrameworkCore?style=flat&logo=nuget)
![NuGet Downloads](https://img.shields.io/nuget/dt/Qrtix.RepositoryPattern.EntityFrameworkCore?style=flat&logo=nuget)
![GitHub Repo stars](https://img.shields.io/github/stars/carlosjortiz/RepositoryPattern?style=flat&logo=github)

A library for implementing generic repositories and unit of work with Entity Framework Core.
This implementation uses a single instance of the DbContext for all repositories to avoid concurrency issues.

## Table of Contents

- [Installation](#installation)
- [Creating the DbContext](#creating-the-dbcontext)
- [Injecting the Repository Pattern](#injecting-the-repositorypatterns-services)

## Installation

Using the NuGet package manager console within Visual Studio run the following command:

```
Install-Package Ortix.RepositoryPattern.EntityFrameworkCore
```

Or using the .NET Core CLI from a terminal window:

```
dotnet add package Qrtix.RepositoryPattern.EntityFrameworkCore
```

## Creating the DbContext

Create your DbContext inheriting from `Microsoft.EntityFrameworkCore.DbContext`:

```csharp
public class MyDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }  

    protected override void OnModelCreating(ModelBuilder modelBuilder)
	{
		modelBuilder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());
		base.OnModelCreating(modelBuilder);
	}
}
```

Configure your DbContext in the `Program` class:

```csharp
builder.Services.AddDbContext<DbContext, SuinQShopModuleDbContext>(opt =>
    {
        opt.UseSqlServer(builder.Configuration.GetConnectionString("ConnectionString"));
    });
```

## Injecting the RepositoryPattern's Services

Add the `RepositoryPattern` services to the Service Collection:


```csharp
builder.Services.AddRepositoryPattern(options => {
    options.UseEntityFrameworkCore();
});
```

The default scope for injected services is scoped. If you want to change it, refer to the next examples:

Using the same lifetime:
```csharp
builder.Services.AddRepositoryPattern(options => {
    options.UseEntityFrameworCore(ServiceLifetime.Singleton);
});
```

Using individual lifetime:
```csharp
builder.Services.AddRepositoryPattern(options => {
    options.UseEntityFrameworCore(ServiceLifetime.Scoped, SeriviceLifetime.Singleton);
});
```