# Entity Framework Core Conventions

## Navigation Properties
- **DO NOT initialize navigation collections** to `new List<T>()` or any other value
- Use `= null!` to suppress nullability warnings: `public ICollection<T> NavigationProperty { get; set; } = null!`
- This ensures that if we forget to `.Include()` the required navigation in a query, we get a NullReferenceException instead of a hard-to-catch empty collection bug
- NullReferenceExceptions are easier to identify and debug during development
- The `null!` suppresses compiler warnings while keeping the property uninitialized

## Model Configuration
- **Prefer attribute annotations over Fluent API** for entity configuration
- **Do not add configuration that is implicitly implied** by EF Core conventions:
  - Primary keys (properties named `Id` or `<EntityName>Id`)
  - Foreign keys (properties matching navigation property names)
  - Navigation relationships (automatically inferred from navigation properties)
- Only use Fluent API for complex scenarios that cannot be expressed with attributes

## Common Attributes
- `[Key]` - only needed if primary key doesn't follow naming convention
- `[Required]` - for non-nullable reference types (though `required` keyword is preferred)
- `[MaxLength(n)]` or `[StringLength(n)]` - for string length constraints
- `[Column("name")]` - for custom column names
- `[Table("name")]` - for custom table names
- `[Index(nameof(Property))]` - for indexes (on entity class)
- `[Index(nameof(Property), IsUnique = true)]` - for unique indexes

## Query Materialization
- **Prefer `ToArray()` over `ToList()`** when materializing EF Core queries if the result collection is not supposed to be modified
- `ToArray()` creates an immutable array, preventing accidental modifications
- Use `ToList()` only when you need a mutable collection

## Materialization Examples
```csharp
// Good - using ToArray for readonly results
var usernames = await context.Users
    .Select(u => u.Name)
    .ToArrayAsync();

// Bad - using ToList when modification is not needed
var usernames = await context.Users
    .Select(u => u.Name)
    .ToListAsync();
```

## Existence Checks
- **Prefer `AnyAsync()` over fetching records** when checking for existence
- `AnyAsync()` is more efficient as it stops at the first match and doesn't transfer data
- Only fetch the record if you need to use it after the existence check

## Existence Check Examples
```csharp
// Good - using AnyAsync for existence check
var userExists = await context.Users.AnyAsync(u => u.Email == email);

// Bad - fetching record just to check existence
var user = await context.Users.FirstOrDefaultAsync(u => u.Email == email);
var userExists = user != null;
```

## Development Environment
- **MySQL hostname for development**: `mysql.overwatcher.orb.local`
- Use this hostname when connecting to MySQL container in development environment
- For localhost development, use `mysql.overwatcher.orb.local` as well

## Example
```csharp
// Good - using attributes
[Index(nameof(Username), IsUnique = true)]
public class User
{
    public int Id { get; set; } // No [Key] needed - follows convention
    
    [MaxLength(100)]
    public required string Username { get; set; }
    
    public ICollection<Post> Posts { get; set; } = null!;
}

// Bad - unnecessary Fluent API
modelBuilder.Entity<User>(entity =>
{
    entity.HasKey(e => e.Id); // Unnecessary - follows convention
    entity.Property(e => e.Username).HasMaxLength(100); // Use attribute instead
    entity.HasMany(e => e.Posts)... // Unnecessary - inferred from navigation
});
```