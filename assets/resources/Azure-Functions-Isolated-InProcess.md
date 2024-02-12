---
layout: page
title: Azure Functions Isolated vs. In-Process
parent: Resources 
permalink: /assets/Resources/Misc/Azure-Functions-Isolated-InProcess/
---
Below you will find my comments on Isolated vs. In Process Azure Functions as well as some common patterns to be aware of and to avoid and why.

## Execuitive Summary

Isolated Azure Functions, a new model introduced in Azure Functions v3.0, offer several benefits over the traditional In-Process model. Key benefits include improved performance, better dependency management through out-of-the-box support for Dependency Injection (DI), enhanced stability due to isolation from the host process, and language independence.

However, these advantages come with their own challenges. Isolated Azure Functions may have longer cold start times due to running in a separate process. They can also introduce complexity in managing inter-process communication and ensuring data consistency.

It is crucial to avoid calling services or using anti-patterns during startup, as this can lead to unexpected behavior and performance issues. While DI is a powerful feature, it must be used correctly to avoid common pitfalls such as service location and captive dependencies.

Microsoft has not announced plans to discontinue support for In-Process Azure Functions, but they are actively encouraging users to move to the isolated model, suggesting that this will be the focus of their future development and support efforts.

In conclusion, while Isolated Azure Functions present certain challenges, they offer significant advantages over In-Process Azure Functions. Teams should weigh these factors and make an informed decision based on the specific needs of their project.

## Important Notes:
### Service Location:
1. Service location is a pattern where the Dependency Injection container is used as a service locator, rather than using it to inject dependencies.
2. This pattern can lead to tightly coupled code, as classes directly ask for their dependencies from the container, making them aware of the container's existence.
3. It can make the code harder to test, as dependencies are not explicitly defined and can't be easily substituted with mocks or stubs.
4. This pattern is considered an anti-pattern in modern software development, as it goes against the principles of Dependency Injection.
5. To avoid this, dependencies should be injected into classes through constructors, properties, or methods, rather than being directly requested from the container.

### Captive Dependencies:
1. A captive dependency is a scenario where a singleton service depends on a scoped or transient service.
2. This can lead to unexpected behavior, as the singleton service is expected to be stateless and long-lived, while the scoped or transient service may have a shorter lifetime.
3. The scoped or transient service can become 'captive' by the singleton service, leading to potential issues like stale data or incorrect state.
4. This scenario is considered a pitfall of Dependency Injection, as it can lead to subtle bugs that are hard to diagnose.
5. To avoid this, ensure that services with longer lifetimes do not depend on services with shorter lifetimes. For example, a singleton service should not depend on a scoped or transient service.

### Transient Services
Transient services are created each time they're requested from the service container. This lifetime works best for lightweight, stateless services.

### Scoped Services
Scoped services are created once per client request (connection). Use this lifetime for services that need to maintain state within a single client interaction.
  
### Singleton Services
Singleton services are created the first time they're requested and then every subsequent request will use the same instance. Use this lifetime for services that are stateless or need to maintain state across multiple client interactions.
Remember, it's crucial to match the lifetime to the nature of the service to avoid issues like captive dependencies.

### Patterns to Avoid or to Consider
- Service Location, Captive Dependenceies
- Invoking services during startup
- Don't override Azure Functions Host Functionality
- Don't override IConfiguraton
  Overriding IConfiguration can render Azure Functions Host ineffective. The below code showcases this anti-pattern:

        ~~~
            // BAD CODE  
            public class Startup : FunctionsStartup
            {
                public override void Configure(IFunctionsHostBuilder builder)
                {
                       // Override the default IConfiguration.
                       // NEVER CALL AddSingleton<IConfiguration>!!!!!
                       builder.Services.AddSingleton<IConfiguration>( 
                          new ConfigurationBuilder()
                          .AddEnvironmentVariables()
                          .Build()
                         );
                }
            }
       ~~~

  Please refer to the Azure Functions documentation on customizing configuration sources. Here’s the correct way to update IConfiguration:

     ~~~
            // GOOD CODE
            public class Startup : FunctionsStartup
            {
                // Use ConfigureAppConfiguration for update IConfiguration
                public override void ConfigureAppConfiguration(IFunctionsConfigurationBuilder builder)
                {
                    FunctionsHostBuilderContext context = builder.GetContext();
            
                    builder.ConfigurationBuilder
                        .AddJsonFile(Path.Combine(context.ApplicationRootPath, "appsettings.json"), optional: true, reloadOnChange: false)
                        .AddJsonFile(Path.Combine(context.ApplicationRootPath, $"appsettings.{context.EnvironmentName}.json"), optional: true, reloadOnChange: false)
                        .AddEnvironmentVariables();
                }
                
                public override void Configure(IFunctionsHostBuilder builder)
                {
                }
            }
     ~~~
     
- Isolated Worker Overrides
  Similar missteps can occur with dotnet-isolated. Here’s a problematic example:

     ~~~
            // BAD CODE
            var host = new HostBuilder()
                .ConfigureFunctionsWorkerDefaults()
                .ConfigureServices(services =>
                {
                    // NEVER CALL AddSingleton<IConfiguration>()!
                    services.AddSingleton<IConfiguration>(
                        new ConfigurationBuilder()
                            .AddEnvironmentVariables()
                            .Build());
                })
                .Build();
            
            host.Run();
     ~~~

  This code often results in exceptions that terminate the isolated process, making diagnosis challenging.

- Improper Key Vault Configuration
  For those using isolated workers, Key Vault configurations should be managed on the host side, not the worker side. Misusing the Key Vault might lead to excessive 
  requests, surpassing the set threshold. Here’s what you should avoid:

     ~~~
            // BAD CODE
            
            var host = new HostBuilder()
                        .ConfigureFunctionsWorkerDefaults()
                        .ConfigureAppConfiguration(config =>{
                             var azureKeyVaultURL = Environment.GetEnvironmentVariable("AzureKeyVaultURL");
                            var azureKeyVaultADAppID = Environment.GetEnvironmentVariable("AzureKeyVaultMIAppID");
            
                            config
                               .SetBasePath(Environment.CurrentDirectory)
                               .AddAzureKeyVault(new Uri(azureKeyVaultURL), new ManagedIdentityCredential(azureKeyVaultADAppID))
                               .AddEnvironmentVariables()
                            .Build();
                        })
            .Build()
     ~~~

  For best practices with Key Vault, always use Key Vault Reference, see documentation for details.

- Avoid Instantiating HTTP or other network Clients Repeatedly
A common mistake observed in Azure Functions is the repeated instantiation of network clients within the code path. This is not limited to C# but applies to other programming languages as well. The primary concern here is the unnecessary creation of new outbound connections without reusing them.

For instance, consider the following C# code:
   ~~~
      // Not recommended
      using var client = new HttpClient();
   ~~~

This pattern results in a new connection every time, and if this behavior continues unchecked, you might eventually encounter a Socket Exception. This is particularly problematic in a Serverless environment like Azure Functions, where network resources are often shared among multiple customers. It’s worth noting that this issue isn’t exclusive to HttpClient. If you're using other network clients like Azure SDK, it's advisable to refrain from creating a new client for every single request.

**Solution**

Consider making your client static or, better yet, employ Dependency Injection (DI) as a best practice. This ensures resource optimization and helps avoid potential network-related exceptions. 
For a deeper dive into implementing this solution, refer to: 

[Use dependency injection in .NET Azure Functions | Microsoft Learn](https://learn.microsoft.com/en-us/azure/azure-functions/functions-dotnet-dependency-injection){:target="_blank"} .

- Not Leveraging async/await in C#
  When working with network clients or other operations that can be asynchronous in nature, it’s crucial to use the async/await pattern. While it might seem fine for simpler applications, in production settings where operations like file I/O or calls to external systems are common, ignoring the asynchronous pattern can lead to issues.

  Specifically, not using the async/await pattern can cause your function to block a thread during an external call, leading to thread starvation. An important note: avoid using Thread.Sleep() in Azure Functions at all costs. Instead, pair async/await with await Task.Delay(), as this is a more efficient approach that doesn't block the thread.

  Here’s a simple example without async/await:

    ~~~
          // Not recommended (If you have external call or IO operation)
          
          public static class SimpleExample
          {
              [FunctionName("QueueTrigger")]
              public static void Run(
                  [QueueTrigger("myqueue-items")] string myQueueItem, 
                  ILogger log)
              {
                  log.LogInformation($"C# function processed: {myQueueItem}");
              }
          }
    ~~~

  Integrating async/await is simple. You just need to modify the method signature and utilize the asynchronous methods with await.  Here is an example using async/await:

    ~~~
          // Recommended
          
          public static class AsyncExample
          {
              [FunctionName("BlobCopy")]
              public static async Task RunAsync(
                  [BlobTrigger("sample-images/{blobName}")] Stream blobInput,
                  [Blob("sample-images-copies/{blobName}", FileAccess.Write)] Stream blobOutput,
                  CancellationToken token,
                  ILogger log)
              {
                  log.LogInformation($"BlobCopy function processed.");
                  await blobInput.CopyToAsync(blobOutput, 4096, token); // external call
              }
          }
   ~~~

- Use the Diagnose and Solve Problems tools in Visual Studio and take action
  This tool points you directly to the root of the problem. And it doesn’t stop here. Alongside diagnosing the issue, it offers actionable recommendations to rectify it. This dual capability — to both identify and guide remediation — makes it an indispensable asset.  

  It’s crucial to adhere to best practices. Doing so not only enhances the efficiency and reliability of your functions but also prevents potential pitfalls associated with common anti-patterns. I highly recommend folling the recommendations outlined on [Azure Functions best practices | Microsoft Learn](https://learn.microsoft.com/en-us/azure/azure-functions/functions-best-practices?tabs=csharp){:target="_blank"}. 
