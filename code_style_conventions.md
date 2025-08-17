# Code Style and Conventions

## C# Code Style
- **Namespace**: File-scoped namespaces (C# 10+ style)
- **Access Modifiers**: implicit whenever possible
- **Naming Conventions**:
    - Private fields: Underscore prefix with camelCase (e.g., `_logger`)
    - Public properties/methods: PascalCase
    - Parameters: camelCase
    - Classes: PascalCase

## Project Configuration
- **Target Framework**: .NET 9.0
- **Language Version**: Latest C# features available
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

## Async Method Naming
- **DO NOT use the `Async` suffix** unless a synchronous version of the same method already exists or will be created
- Only use `Async` suffix when you need to distinguish between sync and async versions of the same method
- Most modern C# codebases are async-first, making the suffix redundant

## Exception Handling
- **Only add try/catch blocks when you need to handle specific exceptions**
- **DO NOT catch exceptions just to log and rethrow** - global exception handlers will handle logging
- Only catch exceptions when you can:
  - Recover from the error
  - Transform the exception
  - Add meaningful context that isn't available higher up
  - Provide a fallback behavior
