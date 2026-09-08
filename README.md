# CarRentalManagement

A small full-stack car-rental demo built on .NET 8: a Blazor WebAssembly client (SPA) with an ASP.NET Core server that provides hosting, authentication, and data persistence. It’s structured as a three-project solution (Client / Server / Shared) for building and experimenting with Blazor + Identity + EF Core patterns.

## Stack

- **Languages:** C#, HTML, CSS, small JavaScript
- **Framework / runtime:** .NET 8 — Blazor WebAssembly (Client) + ASP.NET Core server (Server)
- **Notable libraries:** Blazor WebAssembly, Microsoft.AspNetCore.ApiAuthorization.IdentityServer, Microsoft.EntityFrameworkCore (SQLite + SQL Server providers), Microsoft.AspNetCore.Identity.EntityFrameworkCore, Azure.Identity

## Repository layout

```
CarRentalManagement/                          Blazor WebAssembly client project (Client)
  App.razor
  Program.cs                                  client entry (CarRentalManagement/Program.cs)
  CarRentalManagement.Client.csproj
  Components/                                  UI components
  Pages/                                       client pages
  wwwroot/                                     client static assets (libman.json present)

CarRentalManagement.Server/                   ASP.NET Core server project (hosts API and likely the client)
  Program.cs                                  server startup
  CarRentalManagement.Server.csproj
  Pages/                                       server Razor pages / endpoints
  Data/                                        data access layer (EF Core context & migrations)
  Models/                                      server-side models
  wwwroot/                                     static files served by the server
  appsettings.json
  appsettings.Development.json
  tempkey.jwk                                  development signing key (IdentityServer)

CarRentalManagement.Shared/                   shared DTOs/models used by client and server
  CarRentalManagement.Shared/                  shared class library project

CarRentalManagement.sln                       Visual Studio solution file
.gitignore, .gitattributes
```

How it fits together:
- The solution contains three projects: a Blazor WASM client (CarRentalManagement/), an ASP.NET Core server (CarRentalManagement.Server/) and a Shared library (CarRentalManagement.Shared/) with DTOs/models used by both. The client registers OIDC authentication in Program.cs and is intended to be hosted together with the server (server project references the Shared project and includes ApiAuthorization/IdentityServer packages). EF Core providers for both SQLite and SQL Server are referenced, so the server handles persistence and Identity.

## Quick start

From a machine with the .NET 8 SDK installed:

```bash
git clone https://github.com/James-Hippolite/CarRentalManagement.git
cd CarRentalManagement
dotnet restore
dotnet build
cd CarRentalManagement.Server
dotnet run
```

This runs the server which hosts the client and configures local authentication (IdentityServer). The client binds OIDC options from configuration under the "Local" section (see CarRentalManagement/Program.cs).

### Notes
- Check CarRentalManagement.Server/appsettings.Development.json and user secrets for connection strings and auth provider settings. The server includes a development JWK (tempkey.jwk) used for signing in development.
- The server project references EF Core providers for SQLite and SQL Server — set the appropriate connection string before running if you need a persistent database.

## Next steps / Questions you might ask
- Where is the DbContext and database schema defined (CarRentalManagement.Server/Data)? Can you show the DbContext and migrations?
- Where is the OIDC "Local" provider configuration stored (appsettings.Development.json or user secrets)?
- Should I add a Dockerfile and GitHub Actions workflow to build and run the Server+Client? I can add one targeting .NET 8.
