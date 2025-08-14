# Code Style and Conventions

## C# Code Style
- **Namespace**: File-scoped namespaces (C# 10+ style)
- **Nullable**: Nullable reference types are enabled project-wide
- **Implicit Usings**: Enabled for cleaner code
- **Access Modifiers**: Explicit access modifiers used (public, private, protected)
- **Naming Conventions**:
    - Private fields: Underscore prefix with camelCase (e.g., `_logger`)
    - Public properties/methods: PascalCase
    - Parameters: camelCase
    - Classes: PascalCase

## Project Configuration
- **Target Framework**: .NET 9.0
- **Language Version**: Latest C# features available
- **AOT Compilation**: Enabled for performance
- **Invariant Globalization**: Enabled (culture-independent)

## Logging Pattern
- Use structured logging with ILogger<T>
- Check log level before logging: `if (_logger.IsEnabled(LogLevel.Information))`
- Use structured parameters in log messages

## Async/Await Pattern
- Use async/await for asynchronous operations
- Follow TAP (Task-based Asynchronous Pattern)
- Use CancellationToken for cancellable operations

## Dependency Injection
- **Use primary constructors** whenever possible (C# 12+)
- Do not create explicit properties for constructor arguments
- Name injected dependencies as fields with underscore prefix (_camelCase)
- Constructor injection pattern
- Register services in Program.cs using builder.Services

## Collection Return Types
- **Prefer generic interfaces over concrete implementations** for method return types
- Use `IEnumerable<T>` if the method doesn't need to materialize the result
- Use `IReadOnlyCollection<T>`, `IReadOnlyList<T>`, or similar readonly interfaces when materialization is required
- Avoid returning concrete types like `List<T>`, `Array<T>`, etc.

## Collection Return Examples
```csharp
// Good - using interfaces
IEnumerable<string> GetUsernames() => _users.Select(u => u.Name);
IReadOnlyList<User> GetActiveUsers() => _users.Where(u => u.IsActive).ToList();

// Bad - using concrete types
List<User> GetActiveUsers() => _users.Where(u => u.IsActive).ToList();
string[] GetUsernames() => _users.Select(u => u.Name).ToArray();
```

## Primary Constructor Example
```csharp
// Good - using primary constructor
public class DeviceService(AppDbContext _dbContext) : IDeviceService
{
    public async Task<bool> UpdateDevice(Guid id)
    {
        var device = await _dbContext.Devices.FindAsync(id);
        // ...
    }
}

// Bad - unnecessary explicit constructor
public class DeviceService : IDeviceService
{
    private readonly AppDbContext _dbContext;
    
    public DeviceService(AppDbContext dbContext)
    {
        _dbContext = dbContext;
    }
}
```csharp
// Good - using primary constructor
public class DeviceService(AppDbContext _dbContext) : IDeviceService
{
    public async Task<bool> UpdateDevice(Guid id)
    {
        var device = await _dbContext.Devices.FindAsync(id);
        // ...
    }
}

// Bad - unnecessary explicit constructor
public class DeviceService : IDeviceService
{
    private readonly AppDbContext _dbContext;
    
    public DeviceService(AppDbContext dbContext)
    {
        _dbContext = dbContext;
    }
}
```

## Async Method Naming
- **DO NOT use the `Async` suffix** unless a synchronous version of the same method already exists or will be created
- Only use `Async` suffix when you need to distinguish between sync and async versions of the same method
- Most modern C# codebases are async-first, making the suffix redundant

## Examples
```csharp
// Good - no Async suffix needed
Task<User> GetUser(int id);
Task<bool> UpdatePushToken(Guid deviceId, string token);

// Only use Async when both versions exist
User GetUser(int id);  // sync version
Task<User> GetUserAsync(int id);  // async version
```