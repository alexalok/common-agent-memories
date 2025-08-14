# FastEndpoints Usage Conventions

## Endpoint Structure
- Use `ExecuteAsync` instead of `HandleAsync` for endpoint implementation
- Create nested `RequestDto` and `ResponseDto` classes inside the endpoint class
- If validation is needed, create a nested `Validator` class inside each DTO class
- Use primary constructors for dependency injection

## Route Parameter Mapping
- **FastEndpoints can map route parameters directly to DTO properties**
- Use the same property names as route parameters for automatic mapping
- No need to manually extract route parameters with `Route<T>()`

## Empty DTOs
- **Use `EmptyRequest` when request body is empty**
- **Use `EmptyResponse` when response body is empty**
- Prefer these over creating empty DTO classes

## Endpoint Example
```csharp
public class UpdateDeviceEndpoint(IDeviceService _deviceService) : Endpoint<UpdateDeviceEndpoint.RequestDto, EmptyResponse>
{
    public override void Configure()
    {
        Put("/api/devices/{deviceId}");
        AllowAnonymous();
    }

    public override async Task<EmptyResponse> ExecuteAsync(RequestDto req, CancellationToken ct)
    {
        // req.DeviceId is automatically mapped from route parameter
        var deviceId = req.DeviceId;
        // Implementation here
        return new EmptyResponse();
    }

    public class RequestDto
    {
        public required Guid DeviceId { get; set; } // Mapped from route parameter
        public string? PushToken { get; set; }
        public bool? PushTokenIsSandbox { get; set; }
        public string? BattleNetUsername { get; set; }
    }
}
```

## Key Points
- Always use nested DTOs for better organization
- Validators are optional and only needed when validation is required
- Use primary constructors for cleaner dependency injection
- Use appropriate HTTP status codes and response patterns
- Prefer `EmptyRequest`/`EmptyResponse` over empty DTO classes
- Route parameters are automatically mapped to DTO properties

## DTO Property Requirements
- **Use `[Required]` DataAnnotation and `required` keyword** for mandatory properties
- **Only provide default values for optional properties** (when client can skip sending them)
- **Remove default values for required properties** - use `required` keyword instead
- FastEndpoints supports DataAnnotations validation
- The `required` keyword resolves uninitialized property warnings

## DTO Examples
```csharp
public class RequestDto
{
    [Required]
    public required string AgentName { get; set; } // Required, no default

    public int Count { get; set; } = 10; // Optional with default value
}

public class ResponseDto
{
    public required IReadOnlyList<string> Friends { get; set; } // Required, no default
}
```