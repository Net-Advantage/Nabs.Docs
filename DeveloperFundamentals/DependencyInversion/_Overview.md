# Dependency Inversion Fundamentals in .NET

Outline:

1. Introduction
1. Understanding Dependency Inversion or Inversion of Control (IoC)
1. Types of Dependency Inversion
1. Lifetime of an object
1. Setting up Dependency Injection
1. Resolving Dependencies

Script:

> If you already understand the Theory of Dependency Inversion, skip to the `IServiceCollection` for the code samples.

## Introduction

Welcome to our video presentation on `Dependency Inversion Fundamentals in .NET`. In this presentation, we will cover the importance of dependency inversion, what it is, the different types, and the lifetime of an object. Using a console application, we will show how to register your services and then how to resolve dependencies. 

## Understanding Dependency Inversion or Inversion of Control (IoC)

Inversion of Control (IoC) is a software design principle that aims to increase the modularity, flexibility, and testability of a system by inverting the traditional control flow of a program. It is a key concept in object-oriented programming and is often used in conjunction with dependency injection and the use of frameworks.

Every program has a starting point from which the rest of the application flows. In .NET, this is the `Main` method. The `Main` method serves as the entry point of the application and initiates the program. It is responsible for creating the objects needed to run the application and calling the necessary methods. This is called tight coupling, as the `Main` method is tightly coupled to the objects it creates and the methods it calls. This design is not ideal because it makes the application inflexible and difficult to test.

We can `invert` the control flow by using a framework, called a `container`, to create the objects and call the methods. This is why we use the term `Inversion of Control`. The framework is responsible for creating the objects and calling the methods, and is loosely coupled to the objects and methods that it creates and calls. This design is more desirable because it makes the application flexible and easier to test.

Inversion of control is essential for developer productivity. By decoupling the system, multiple individuals and teams can independently work on various parts of the system. By separating the system into discrete concerns, work can be parallelized or assigned to people with different sets of domain knowledge to apply their individual expertise to the system. This approach is often referred to as `separation of concerns`.

## Types of Dependency Inversion

There are the three types of dependency inversion techniques used in software design to achieve Inversion of Control (IoC). These techniques help in creating modular, flexible, and testable applications.

1. Dependency Injection:
Dependency Injection is a technique where an object's dependencies are provided by an external entity instead of the object itself creating them. There are two common forms of Dependency Injection:

	a. Constructor Injection:
	In Constructor Injection, dependencies are provided to an object via its constructor. When the object is instantiated, the required dependencies are passed as arguments to the constructor. This approach ensures that the object has all its dependencies before it starts executing.

	b. Property Injection:
	Property Injection, also known as Setter Injection, involves providing dependencies to an object through setter methods or properties. Unlike Constructor Injection, this approach allows for the possibility of changing dependencies after the object has been instantiated.

2. Service Locator:
The Service Locator pattern involves using a centralized registry to manage the creation and retrieval of service instances. Components can request a specific service from the service locator, which handles the creation and management of the service instance. This approach provides a level of indirection, making it easier to swap or modify components without affecting the rest of the application.

3. Event Driven:
Event Driven programming is a paradigm where the flow of control is determined by the occurrence of specific events, such as user actions or changes in the application state. Components can subscribe to and react to these events, without needing to be explicitly aware of the control flow. This approach promotes loose coupling between components, making it easier to develop and maintain applications.

In summary, Dependency Injection, Service Locator, and Event Driven programming are three types of dependency inversion techniques used to achieve Inversion of Control in software design. By using these techniques, developers can create more modular, flexible, and testable applications.

> In .NET, the most common technique used for Dependency Injection is Constructor Injection. This technique involves providing dependencies to an object via its constructor. When the object is instantiated, the required dependencies are passed as arguments to the constructor. This approach ensures that the object has all its dependencies before it starts executing.

While the Service Locator pattern is a valid means for resolving dependencies, it is not recommended for use in .NET applications. The Service Locator pattern is considered an anti-pattern because it introduces a level of indirection that can make it difficult to understand and maintain the code. Instead, it is recommended to use Constructor Injection to provide dependencies to an object.

## Lifetime of an object

Let's discuss the lifetime of an object as it pertains to Dependency Injection (DI). Understanding object lifetimes is crucial when working with DI, as it helps to manage resources effectively and ensures that the application behaves as expected.

When using Dependency Injection, objects are instantiated and managed by a container or an external entity. The container is responsible for creating, injecting, and disposing of objects as required. There are typically three types of lifetimes associated with objects in a DI container:

1. Transient:
Transient lifetime means that a new instance of the object is created each time it is requested from the DI container. This is useful when the object has a short lifespan or is stateless, ensuring that each consumer receives a brand-new instance, thus avoiding any shared state or side effects. Transient objects are typically lightweight and do not hold expensive resources.

2. Scoped:
Scoped lifetime indicates that an object is created once per scope, usually tied to a specific context, such as a single web request or a unit of work. Scoped objects are shared within that scope but are not shared across different scopes. This is useful when the object needs to maintain state or resources within a specific context. When the scope ends, the DI container disposes of the scoped objects, releasing any associated resources.

3. Singleton:
Singleton lifetime means that only one instance of the object is created and shared across the entire application for the duration of the application's lifetime. Singleton objects are useful when the object needs to maintain a global state or manage shared resources, such as a configuration object or a database connection pool. Since the singleton object persists for the entire application lifetime, it is essential to ensure that it is thread-safe and does not hold resources unnecessarily.

> It is important to note that for a simple console applications like this one, we will not demonstrate `Scoped` lifetime. It will be covered in depth in the `ASP.NET Dependency Inversion Fundamentals` lesson.

In summary, understanding the lifetime of objects in Dependency Injection is crucial for effective resource management and application behavior. By choosing the appropriate lifetime for an object - Transient, Scoped, or Singleton - developers can optimize performance, manage resources, and avoid unexpected issues related to shared state or resource contention.

## Setting up Dependency Injection

Before you start install the required Nuget package:

```powershell
Microsoft.Extensions.DependencyInjection
```

Create the following files for each class:

### TheSingletonService

This object will be instantiated once for the entire lifetime of the application. Each time it is requested the same intance will be retured.

Create a file called `TheSingletonService.cs` with the following code:

```csharp
namespace DependencyInversionFundamentals;

public class TheSingletonService
{
	private readonly DateTime _createdOn = DateTime.UtcNow;
	
	public void Output()
	{
		Console.WriteLine($"TheSingletonService created on: {_createdOn}");
		Console.WriteLine($"TheSingletonService called on: {DateTime.UtcNow}");
	}
}
```

### TheTransientService

This object will be instantiated anew each time it is requested.

Create a file called `TheTransientService.cs` with the following code:

```csharp
namespace DependencyInversionFundamentals;

public class TheTransientService
{
	private readonly DateTime _createdOn = DateTime.UtcNow;

	public void Output()
	{
		Console.WriteLine($"TheTransientService created on: {_createdOn}");
		Console.WriteLine($"TheTransientService called on: {DateTime.UtcNow}");
	}
}
```

### TheHostService

This object will depend on `TheSingletonService` and `TheTransientService`.

Create a file called `TheHostService.cs` with the following code:

```csharp
namespace DependencyInversionFundamentals;

public class TheHostService
{
	private readonly TheSingletonService _theSingletonService;
	private readonly TheTransientService _theTransientService;

	public TheHostService(
		TheSingletonService theSingletonService,
		TheTransientService theTransientService)
	{
		_theSingletonService = theSingletonService;
		_theTransientService = theTransientService;
	}

	public void Output()
	{
		_theSingletonService.Output();
		_theTransientService.Output();
	}
}
```

### Setting up the `IServiceCollection`

The `IServiceCollection` is used to make your container aware of all the dependencies in your application.

In the `Program.cs` file add the following code:

```csharp
using DependencyInversionFundamentals;
using Microsoft.Extensions.DependencyInjection;

var services = new ServiceCollection();
services.AddTransient<TheHostService>();
services.AddSingleton<TheSingletonService>();
services.AddTransient<TheTransientService>();
```

This code sets up the `ServiceCollection` and then added each of the dependencies in your application.

### Setting up the `IServiceProvider`

The `IServiceProvider` is used to resolve the dependencies in your application. Before using it, the IServiceCollection must be built. This is done by calling the `BuildServiceProvider` method.

In the `Program.cs` file add the following code:

```csharp
var serviceProvider = services.BuildServiceProvider();
```

## Resolving dependencies

In this example, we will resolve the `TheHostService` object and then call the `Output` method. As you can see we are using the Service Locator pattern to resolve the `TheHostService` object.

> As previously mentioned, the Service Locator pattern is considered an anti-pattern and should not be used in .NET applications. Instead, it is recommended to use Constructor Injection to provide dependencies to an object.

Add the following code to the `Program.cs` file:

```csharp
var theHostService = serviceProvider.GetRequiredService<TheHostService>();
theHostService.Output();
```

I recommend that you use the `GetRequiredService` method to resolve dependencies. This method will throw an exception if the dependency cannot be resolved. This is useful for debugging and will help you identify any issues with your dependencies.

Note that we only need to explicitly resolve the `TheHostService` object. The `TheSingletonService` and `TheTransientService` objects will be resolved automatically by the DI container.

Calling the	`Output` method will output the following:

```powershell
TheSingletonService created on: 22/04/2023 9:37:53 pm
TheSingletonService called on: 22/04/2023 9:37:53 pm
TheTransientService created on: 22/04/2023 9:37:53 pm
TheTransientService called on: 22/04/2023 9:37:53 pm
```

> The date and time will be different for you.

Add the following code to the `Program.cs` file:

```csharp
Console.WriteLine("Waiting 1 second...");
await Task.Delay(1000);

theHostService = serviceProvider.GetRequiredService<TheHostService>();
theHostService.Output();
```

This will cause the application to wait for 1 second and then resolve the `TheHostService` object again. The output will be:

```powershell
TheSingletonService created on: 22/04/2023 9:37:53 pm
TheSingletonService called on: 22/04/2023 9:37:53 pm
TheTransientService created on: 22/04/2023 9:37:53 pm
TheTransientService called on: 22/04/2023 9:37:53 pm
Waiting 1 second...
TheSingletonService created on: 22/04/2023 9:37:53 pm
TheSingletonService called on: 22/04/2023 9:37:54 pm
TheTransientService created on: 22/04/2023 9:37:54 pm
TheTransientService called on: 22/04/2023 9:37:54 pm
```

As you can see, the `TheSingletonService` object was only created once and the `TheTransientService` object was created twice.

See the 1 second difference between the `TheSingletonService` and `TheTransientService` objects.

## Summary

In this lesson, we learned about:
- Dependency Inversion and why it is important.
- The Service Locator pattern and how it can be used to resolve dependencies in .NET applications.
- The different lifetimes of objects in Dependency Injection and how they can be used to optimize performance and manage resources.
