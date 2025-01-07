# ASP .NET Core

---

### Middleware:

Middleware is software that's assembled into an app pipeline to handle requests and responses.

Each middleware component:

- Chooses whether to pass the request to the next component in the pipeline.
- Can perform work before and after the next component in the pipeline.

Request delegates are configured using `Run`, `Map`, and `Use` extension methods. An individual request delegate can be specified  in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class. These reusable classes and in-line  anonymous methods are *middleware*, also called *middleware components*.

![](/home/soham/TechnicalNotes/images/asp_dotnet_core_notes/middleware.png)

Exception-handling delegates should be called early in the pipeline, so  they can catch exceptions that occur in later stages of the pipeline.

The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.

```c#
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.Run(async context =>
{
    await context.Response.WriteAsync("Hello world!");
});

app.Run();
```

