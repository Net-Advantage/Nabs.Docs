# Logging Message Template

If you know what the issues are you can skip directly to the last section: `Semantic Logging with Source Generators`.

## The Trouble

It has been my observation that most developers approach logging as follows:

```csharp

public Person CreatePerson(string username)
{
    var person = new Person()
    {
        Id = Guid.NewGuid(),
        Username = username
    };

    _logger.LogInformation(message: $"New person created: {person.Username}");
}
```

This approach uses string interpolation to contruct the "message". However, you may have noticed that you receive the following warning [CA2254: Template should be a static expression](https://learn.microsoft.com/en-nz/dotnet/fundamentals/code-analysis/quality-rules/ca2254).

Examining the source code for the above extension method we discover the reason for this warning:

```csharp
//
// Summary:
//     Formats and writes an informational log message.
//
// Parameters:
//   logger:
//     The Microsoft.Extensions.Logging.ILogger to write to.
//
//   message:
//     Format string of the log message in message template format. Example: "User {User}
//     logged in from {Address}"
//
//   args:
//     An object array that contains zero or more objects to format.
public static void LogInformation(this ILogger logger, string? message, params object?[] args)
{
    logger.Log(LogLevel.Information, message, args);
}
```

The Parameters comment describes the `message` argument as being the `Format string of the log message in message template format. Example: "User {User} logged in from {Address}"`. The operative words where are `Format string` and `message template`. It was never the intention for developers to just blindly dump the output into the log methods.

## Structured or Semantic Logging

Structured logging is an improved approach to logging. This involves:
- turning the "message" into a template.
- use the params arguments to provide the values for the template.

```csharp
_logger.LogInformation(message: "Person Created: {username}", person.Username);
```

## Semantic Logging with Source Generators

The best approach is to use a formal approach to logging by declaring a logging method for each template your application will need.

You can so this by add classes to your project with and extension methods similar to this one:

```csharp
public static partial class GlobalLogging
{
    [LoggerMessage(EventId = 1,
    Level = LogLevel.Information,
    Message = "Person Created: Username: {Username}")]
    public static partial void LogPersonCreated(this ILogger logger, string username);
}
```

The logging source generators will generate the necessary code that avoids all the logging memory and performance issues.

The way you use this is:

```csharp
_logger.LogPersonCreated(person.Username);
```

My view on this topic is that we create an inventory of logs.