# Implement the `IOptions` pattern in a console application

The `IOptions` pattern is a way to structure your configuration keys and values, resulting in cleaner code. Using `magic strings` in your application, especially as they grow and become more complex, makes maintenance more difficult.

Install the required NuGet packages:

    - `Microsoft.Extensions.Options`
	- `Microsoft.Extensions.Options.ConfigurationExtensions`
    - `FluentValidation`

Add a new file to your project called `AppSettings.cs` with the following classes to represent the stucture of the configuration settings. It also adds an `AbstractValidator` that defines the rules 
```csharp
namespace ConfigurationFundamentals;

public class AppSettings
{
    public string AppName { get; set; } = default!;
    public SecretSettings SecretSettings { get; set; } = default!;
}

public class SecretSettings
{
    public string SuperSecret { get; set; } = default!;
}

public class AppSettingsValidator : AbstractValidator<AppSettings>
{
    public AppSettingsValidator()
    {
        RuleFor(x => x.AppName)
            .NotEmpty()
            .WithMessage("AppName is required.");
        RuleFor(x => x.SecretSettings)
            .NotNull()
            .WithMessage("SecretSettings is required.");
        RuleFor(x => x.SecretSettings.SuperSecret)
            .NotEmpty()
            .WithMessage("SuperSecret is required.");
    }
}
```

Note that the ` = default!` at the end of each of the properties implies that the property will be set at runtime.

Modify the `Program.cs` file to the following:
```csharp
using ConfigurationFundamentals;
using FluentValidation;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Options;

var builder = new ConfigurationBuilder()
    .AddJsonFile("appsettings.json", optional: false)
    .AddUserSecrets<Program>(optional: false);

var configuration = builder.Build();

var services = new ServiceCollection();
services.Configure<AppSettings>(configuration);
services.AddOptions<AppSettings>()
    .Validate(appSettings =>
    {
        var validator = new AppSettingsValidator();
        var result = validator.Validate(appSettings);
        return result.IsValid;
    })
    .ValidateOnStart();

var serviceProvider = services.BuildServiceProvider();

var appSettings = serviceProvider.GetRequiredService<IOptions<AppSettings>>()!.Value;
Console.WriteLine("Reading from a strongly typed class");
Console.WriteLine($"AppName: {appSettings.AppName}");
Console.WriteLine($"SecretSettings - SuperSecret: {appSettings.SecretSettings.SuperSecret}");
```
 ##  Conclusion

We have demonstrated the use of the IOptions pattern to provide structure, type safety, and validation to our configuration.