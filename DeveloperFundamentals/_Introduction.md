# Configuration Fundamentals in .NET

Outline:

1.  Introduction
1.  Importance of application configuration
1.  Creating and setting up an `appsettings.json` File
1.  Creating and setting up a `secrets.json` File
1.  `ConfigurationBuilder` code in a console application
1.  Implement the `IOptions` pattern in a console application
1.  Conclusion

Script:

## Introduction

Welcome to our video presentation on `Configuration Fundamentals in .NET`. In this presentation, we will cover the importance of application configuration, setting up the ConfigurationBuilder in a console application, and using appsettings.json and secrets.json files for configuration. We will also explore what is known as the Options pattern. This pattern helps you structure your configuration keys and values, resulting in cleaner code.
    
## Importance of application configuration

Application configuration is essential for managing various settings and values used by your application. It allows you to separate these settings from your code, making maintenance and deployment easier. Configuration files enable you to store information such as database connection strings, API keys, and other sensitive data securely and separately from your source code.
    
##  ConfigurationBuilder setup in a console application

To begin, let's set up the ConfigurationBuilder in a .NET 7.0 console application. First, install the required NuGet packages:

    - `Microsoft.Extensions.Configuration`
    - `Microsoft.Extensions.Configuration.Json`
    - `Microsoft.Extensions.Configuration.UserSecrets`

## Creating and setting up an `appsettings.json` file

Add a new item to your console app project called `appsettings.json`. This file will contain the configuration settings for your application. The `appsettings.json` file is a JSON file that contains key-value pairs of configuration settings. The appsettings.json file is read at runtime, and its values are made available to the application through the `IConfiguration` interface.

Once you have added the `appsettings.json` file to your project, add the following JSON to the file:
```json
{
	"AppName": "Learning Basic Application Configuration",
	"SecretSettings": {
		"SuperSecret": "<< Placeholder: Value to be stored in secrets.json >>"
	}
}
```

Ensure that your `appsettings.json` file is set to `Copy if Newer` in the `Copy to Output Directory` of the properties window in Visual Studio.

The `.csproj` file will contain the following:
```xml
<ItemGroup>
	<None Update="appsettings.json">
	<CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
	</None>
</ItemGroup>
```

## Creating and Setting Up a `secrets.json` File

Add a `secrets.json` file to your project by right-clicking on the project and selecting `Manage User Secrets`. This file will contain sensitive configuration settings for your application. The `secrets.json` file is a JSON file that contains key-value pairs of configuration settings. The `secrets.json` file is read at runtime, and its values are made available to the application through the `IConfiguration` interface.

The `.csproj` file will now contain a `UserSecretId` element.
```xml
<PropertyGroup>
	...
	<UserSecretsId>xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</UserSecretsId>
</PropertyGroup>
```

Once you have added the `secrets.json` file to your project, add the following JSON to the file:
```json
{
	"SecretSettings": {
		"SuperSecret": "My secret is that I love .Net!"
	}
}
```

Only add the key and value that you want to keep secret. The `secrets.json` file is not checked into source control and is only available on your local machine.

## `ConfigurationBuilder` code in a console application

Now that all the setup is completed, lets write some code:

```csharp
using Microsoft.Extensions.Configuration;

var builder = new ConfigurationBuilder()
	.AddJsonFile("appsettings.json", optional: true)
	.AddUserSecrets<Program>();

var configuration = builder.Build();


Console.WriteLine("Reading directly from configuration");
var appName = configuration["AppName"];
var superSecret = configuration["SecretSettings:SuperSecret"];
Console.WriteLine($"AppName: {appName}");
Console.WriteLine($"SecretSettings - SuperSecret: {superSecret}");
```

Run the application to ensure that you receive the expected output.

```console
AppName: Learning Basic Application Configuration
SecretSettings - SuperSecret: My secret is that I love .Net!
```

There are some important things to note:

- The order in which you add the configuration files is important. The last file added will override any values in the previous files.
- The `AddUserSecrets` method requires a type parameter. The `Program.cs` file implies the existence of the `Program` type. This is used to locate the `secrets.json` file.
- The `AddUserSecrets` method has an `optional` parameter of type `bool`. This parameter is used to determine if the `secrets.json` file is optional. If the file is not found, an exception will be thrown if the parameter is set to `false`. If the parameter is set to `true`, no exception will be thrown if the file is not found. If your application requires other developers to set up secrets, it is recommended that you set this value to `false`. This will signal to other developers to set up their user secrets when they run the application.
- The key for the `SecretSettings` section is `SecretSettings:SuperSecret`. This is because the `SecretSettings` section is nested within the `appsettings.json` file. The `:` is used to denote a nested section.

## Implement the `IOptions` pattern in a console application

The `IOptions` pattern is a way to structure your configuration keys and values, resulting in cleaner code. Using `magic strings` in your application, especially as they grow and become more complex, makes maintenance more difficult.

Install the required NuGet packages:

    - `Microsoft.Extensions.Options`
	- `Microsoft.Extensions.Options.ConfigurationExtensions`

Add a new file to your project called `AppSettings.cs` with the following classes to represent the stucture of the configuration settings.
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
```

Note that the ` = default!` at the end of each of the properties implies that the property will be set at runtime. This is required because the `IOptions` pattern requires that the properties are set at runtime.

Modify the `Program.cs` file to the following:
```csharp
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Options;
using ConfigurationFundamentals;

var builder = new ConfigurationBuilder()
	.AddJsonFile("appsettings.json", optional: true)
	.AddUserSecrets<Program>();

var configuration = builder.Build();


Console.WriteLine("Reading directly from configuration");
var appName = configuration["AppName"];
var superSecret = configuration["SecretSettings:SuperSecret"];
Console.WriteLine($"AppName: {appName}");
Console.WriteLine($"SecretSettings - SuperSecret: {superSecret}");


Console.WriteLine();
Console.WriteLine(new string('=', 50));
Console.WriteLine("Reading from a strongly typed class");
var appSettingsOptions = new OptionsWrapper<AppSettings>(new());
configuration.Bind(appSettingsOptions.Value);
var appSettings = appSettingsOptions.Value;
Console.WriteLine($"AppName: {appSettings.AppName}");
Console.WriteLine($"SecretSettings - SuperSecret: {appSettings.SecretSettings.SuperSecret}");
```

__See Also__:

 - `DependencyInversionFundamentals` for using the options pattern with DI.
 - `AspNetConfigurationFundamentals` for using the options pattern with ASP.NET Core.

##  Conclusion

We have demonstrated the importance of application configuration, setting up the ConfigurationBuilder in a .NET 7.0 console application, creating and using `appsettings.json` and `secrets.json` files, and utilizing the IOptions pattern to provide structure and type safety to our configuration. By effectively managing your application's configuration, you can improve security, maintainability, and deployment flexibility. We hope this presentation has been helpful in understanding the essentials of application configuration in .NET 7.0.