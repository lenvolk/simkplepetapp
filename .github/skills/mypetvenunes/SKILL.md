---
name: mypetvenunes
description: Domain expertise for MyPetVenues, a Blazor WebAssembly pet-friendly venue discovery platform. Use this skill when refactoring, updating, debugging, or extending the application. Provides architecture patterns (SOLID services, DI), component conventions (Razor + scoped CSS), CSS theming system (light/dark mode with CSS variables), and data models (Venue, User, Booking, Review). Triggers on tasks involving MyPetVenues pages, components, services, models, or styling.
---

# MyPetVenues Development Skill

Domain knowledge for the MyPetVenues Blazor WebAssembly application.

## Quick Reference

| Aspect | Details |
|--------|---------|
| **Stack** | Blazor WASM, .NET 9, C# 13 |
| **Build** | `dotnet build MyPetVenues/MyPetVenues.csproj` |
| **Run** | `dotnet run --project MyPetVenues/MyPetVenues.csproj` |
| **Theme** | Pink gradient, light/dark mode |
| **Font** | Plus Jakarta Sans |

## Project Structure

```
MyPetVenues/
├── Program.cs          # DI registration
├── Components/         # Reusable: VenueCard, StarRating, PetBadge, etc.
├── Layout/             # MainLayout, Header, Footer
├── Models/             # Venue, User, Booking, Review + enums
├── Pages/              # Home, Venues, VenueDetail, Profile, BookVenue
├── Services/           # IVenueService, IUserService, IBookingService, IThemeService
└── wwwroot/css/app.css # Global styles + CSS variables
```

## Key Conventions

### Service Pattern
- Interface + Mock implementation
- Follow the existing registrations in `MyPetVenues/Program.cs`: Theme, Venue, and Booking are Singleton; User is Scoped
- These are client-side WASM lifetimes, not server-wide or per-request lifetimes; Theme and Booking retain mutable in-memory state
- Async methods returning `Task<T>`

### Component Pattern
- `[Parameter, EditorRequired]` for required props
- `EventCallback` for output events without data; `EventCallback<T>` when passing data
- Scoped CSS file per component using CSS variables

### Styling Pattern
- All colors via CSS variables (`--accent-primary`, `--card-bg`, etc.)
- Theme toggle via `.dark-mode`/`.light-mode` class on `.app-container`
- Card hover: `translateY(-8px)` + shadow increase

## References

Load these based on task:

- **Architecture work**: See [references/architecture.md](references/architecture.md) for services, DI, routes
- **Component work**: See [references/components.md](references/components.md) for component catalog
- **Styling work**: See [references/styling.md](references/styling.md) for CSS variables, theme system
- **Data layer work**: See [references/models.md](references/models.md) for model schemas

## Common Tasks

### Add a new component
1. Create `Components/NewComponent.razor`
2. Create `Components/NewComponent.razor.css`
3. Use CSS variables from app.css
4. Follow parameter conventions (see components.md)

### Add a new page
1. Create `Pages/NewPage.razor` with `@page "/route"`
2. Inject needed services
3. Create `Pages/NewPage.razor.css`
4. Add navigation link in Header.razor

### Add a new service
1. Create `Services/NewService.cs` under `MyPetVenues/`, colocating the interface and mock implementation as existing services do
2. Keep mock data in memory unless the task explicitly calls for backend integration
3. Register in `Program.cs` with a lifetime appropriate for client-side WASM (see architecture reference); do not split existing service files as incidental cleanup

### Modify theme colors
1. Edit CSS variables in `wwwroot/css/app.css`
2. Update both `:root` (light) and `.dark-mode` sections

## Validation and Scope

- Treat current source files as the reference for signatures, routes, and registrations; check the relevant implementation before copying an example from this skill.
- Do not manually edit generated `bin/` or `obj/` files. Change their source inputs instead.
- After application code changes, run `dotnet build MyPetVenues/MyPetVenues.csproj` from the repository root and the narrowest available tests for the changed behavior.
- For UI changes, run the app and verify the affected workflow at desktop and mobile sizes, in both light and dark mode. Check text overflow, image loading, keyboard access, and browser console errors.
- For route or query changes, verify direct navigation and navigation between different parameters on the same page component, including loading, empty, and not-found states where applicable.
- Report which checks ran and any checks that could not run. For skill-only documentation changes, validate metadata and reference links; an application build is not required.
