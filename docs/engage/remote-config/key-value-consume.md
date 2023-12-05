# Consume a key-value configuration

TODO add some docs here

## Prerequisites

* The app is set up to read configuration files. [Enable configuration in the app](../../build/configuration.md).

## Configuration files

This connection string should be stored in the apps settings or environment variables. It needs to be compiled into the app and then loaded at runtime.

To consume the Azure app configuration, we can use the "Primary Read Only" connection string from Azure. We only need a read only configuration because our app does not need to update the Azure values.

If you are using an app that supports `appsettings.json`, add the connection string to your `appsettings.json` file:

```json
{
    "ConnectionStrings": {
        "MyAppConfig": "Endpoint=https://<account>.azconfig.io;Id=<id>;Secret=<secret>"
    }
}
```

## Reading configuration files

There are 2 ways to load app configurations:

1. Blocking at startup
2. Asynchronous on-demand

In most mobile scenarios, the app startup should not be blocked under any circumstances. However, on servers and desktop where connectivity is gaurenteed and configuration is required at startup, blocking operations may be permitted.

### Blocking

Blocking app startup is never a good idea, but it is the simplest:

1. Install the `Microsoft.Extensions.Configuration.AzureAppConfiguration` NuGet package.

2. Add the following namespaces to your main program builder:

    ```cs
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

3. Load the Azure app configuration into the app:

    ```cs
    // Retrieve the connection string
    var connectionString = builder.Configuration.GetConnectionString("MyAppConfig");

    // Load configuration from Azure App Configuration
    builder.Configuration.AddAzureAppConfiguration(connectionString);

    // The rest of the existing code in MauiProgram.cs
    // ...
    ```

4. Configuration values can be read like any other configuration values as they are loaded into the same configuration instance:

    ```cs
    public class MyViewModel
    {
        private readonly IConfiguration config;

        public MyViewModel(IConfiguration configuration)
        {
            config = configuration;
        }

        public string WelcomeMessage =>
            config["MyMobileApp:HomePage:WelcomeMessage"] ?? "<unavailable>";
    }
    ```

## Asynchronous
