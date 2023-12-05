# Key-Value configurations

TODO add some docs here

## Create a key-value configuration

Add a key-value to the App Configuration store using the az appconfig kv set command. Replace the placeholder <name> with the name of the App Configuration store:


#### [Azure CLI](#tab/azure-cli)

To create a new app configuration store, run:

```
az appconfig kv set \
  --name <store-name> \
  --key <key> \
  --value <value>
```
> For example, to create an app config named `MyMobileConfig` in an existing resource group named `MyMobileResources`:
>
> ```
> az appconfig kv set \
>   --name "MyMobileConfig" \
>   --key "MyMobileApp:HomePage:WelcomeMessage" \
>   --value "Welcome to My Books"
> ```
---

## Consume a key-value configuration

To consume the app configuration, we can use the "Primary Read Only" connection string from Azure.

This connection string should be stored in the apps settings or environment variables.

#### [.NET MAUI](#tab/dotnet-maui)

1. Install the `Microsoft.Extensions.Configuration.AzureAppConfiguration` NuGet package.
2. Add the following namespaces to your `MauiProgram.cs`:

   ```cs
   using Microsoft.Extensions.Configuration;
   using Microsoft.Extensions.Configuration.AzureAppConfiguration;
   ```

3. Connect

var connectionString = Environment.GetEnvironmentVariable("ConnectionString");

var builder = new ConfigurationBuilder();
builder.AddAzureAppConfiguration(connectionString);

var config = builder.Build();
Console.WriteLine(config["TestApp:Settings:Message"] ?? "Hello world!");


---

