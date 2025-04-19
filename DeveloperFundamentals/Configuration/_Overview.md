# Configuration Fundamentals in .NET

## Introduction

We will cover the importance of application configuration, setting up the ConfigurationBuilder in a console application, and using appsettings.json and secrets.json files for configuration.
    
## Importance of application configuration

Application configuration is essential for managing various settings and values used by your application. It allows you to separate these settings from your code, making maintenance and deployment easier. Configuration files enable you to store information such as database connection strings, API keys, and other sensitive data securely and separately from your source code.
    
##  ConfigurationBuilder setup in a console application

To begin, let's set up the ConfigurationBuilder in a .NET 7.0 console application. First, install the required NuGet packages:

    - `Microsoft.Extensions.Configuration`
    - `Microsoft.Extensions.Configuration.Json`
    - `Microsoft.Extensions.Configuration.UserSecrets`

## Creating and setting up an `appsettings.json` file

Add a new item to your console app project called `appsettings.json`. This file will contain the configuration settings for your application. The `appsettings.json` file is a JSON file that contains key-value pairs of configuration settings. The `appsettings.json` file is read at runtime, and its values are made available to the application through the `IConfiguration` interface.

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

Only add the key and value that you want to keep secret. The `secrets.json` file is not checked into source control and is only available on your local machine. This implies that each developer will have to set up their own `secrets.json` file independently.

## `ConfigurationBuilder` code in a console application

Now that all the setup is completed, lets write some code:

```csharp
using Microsoft.Extensions.Configuration;

var builder = new ConfigurationBuilder()
	.AddJsonFile("appsettings.json", optional: false)
	.AddUserSecrets<Program>(optional: false);

var configuration = builder.Build();

Console.WriteLine("Reading directly from configuration");
var appName = configuration["AppName"];
var superSecret = configuration["SecretSettings:SuperSecret"];
Console.WriteLine($"AppName: {appName}");
Console.WriteLine($"SuperSecret: {superSecret}");
```

Run the application to ensure that you receive the expected output.

```console
Reading directly from configuration
AppName: Learning Basic Application Configuration
SuperSecret: My secret is that I love .Net!
```

There are some important things to note:

- The order in which you add the configuration files is important. The last file added will override any values in the previous files.
- The `AddUserSecrets` method requires a type parameter. The `Program.cs` file implies the existence of the `Program` type. This is used to locate the `secrets.json` file.
- The `AddUserSecrets` method has an `optional` parameter of type `bool`. This parameter is used to determine if the `secrets.json` file is optional. If the file is not found, an exception will be thrown if the parameter is set to `false`. If the parameter is set to `true`, no exception will be thrown if the file is not found. If your application requires other developers to set up secrets, it is recommended that you set this value to `false`. This will signal to other developers to set up their user secrets when they run the application.
- The key for the `SecretSettings` section is `SecretSettings:SuperSecret`. This is because the `SecretSettings` section is nested within the `appsettings.json` file. The `:` is used to denote a nested section.

##  Conclusion

We have demonstrated the importance of application configuration, setting up the ConfigurationBuilder in a .NET console application, creating and using `appsettings.json` and `secrets.json` files. By effectively managing your application's configuration, you can improve security, maintainability, and deployment flexibility.