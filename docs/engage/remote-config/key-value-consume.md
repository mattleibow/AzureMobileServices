# Consume a key-value configuration

TODO add some docs here

## Prerequisites

* The app is set up to read configuration files. [Enable configuration in the app](../../build/configuration.md).

## Configuration files

This connection string should be stored in the apps settings or environment variables. It needs to be compiled into the app and then loaded at runtime.

To consume the Azure app configuration, we can use the "Primary Read Only" connection string from Azure. We only need a read only configuration because our app does not need to update the Azure values.

If you are using an app that supports `appsettings.json`, add the connection string to your `appsettings.json` file. In addition to the connection string, it is good practice to also include default values for each of the app configurations as there may be issues retrieveing them later.

```json
{
    "ConnectionStrings":
    {
        "MyAppConfig": "Endpoint=https://<account>.azconfig.io;Id=<id>;Secret=<secret>"
    },
    "MyAppConfigDefaults":
    {
        "MyMobileApp:HomePage:WelcomeMessage":  "Default Welcome"
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

2. Add the following namespaces to your main program builder file:

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

### Asynchronous

Loading configurations asynchronously is always better as it allows apps to be responsive, but this adds an extra layer of complexity. The additional work is contained into a single class, however, it does require the app to work differently and respond to changes once the data is loaded.


1. Install the `Microsoft.Extensions.Configuration.AzureAppConfiguration` NuGet package.

2. Create a new file in your project to handle the asynchronous app configuration loading:

    ```cs
    using System.Diagnostics.CodeAnalysis;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    using Microsoft.Extensions.Primitives;

    namespace Microsoft.Extensions.Configuration.AzureAppConfiguration;

    public class AsyncAzureAppConfig : IConfiguration
    {
        private readonly Action<AzureAppConfigurationOptions> actionOptions;

        private Task<IConfigurationRoot>? configTask;
        private IConfigurationRoot? configurationRoot;
        private IConfiguration? defaultConfiguration;

        public AsyncAzureAppConfig(Action<AzureAppConfigurationOptions> action)
        {
            actionOptions = action;
        }

        public AsyncAzureAppConfig(string connectionString)
            : this(options => options.Connect(connectionString))
        {
        }

        public async Task LoadAsync(bool reload = false)
        {
            if (reload || configurationRoot is not null)
                return;

            configTask ??= Task.Run(() => Build(actionOptions));
            configurationRoot = await configTask;
            configTask = null;
        }

        public void SetDefaults(IConfiguration? defaults)
        {
            defaultConfiguration = defaults;
        }

        public string? this[string key]
        {
            get => TryLoaded(out var config) ? config[key] : default;
            set => OnlyWhenLoaded()[key] = value;
        }

        public IEnumerable<IConfigurationSection> GetChildren() =>
            OnlyWhenLoaded().GetChildren();

        public IChangeToken GetReloadToken() =>
            OnlyWhenLoaded().GetReloadToken();

        public IConfigurationSection GetSection(string key) =>
            OnlyWhenLoaded().GetSection(key);

        private IConfiguration OnlyWhenLoaded() =>
            TryLoaded(out var config) ? config : throw new InvalidOperationException("Configuration is not yet ready, make sure to call LoadAsync() first.");

        private bool TryLoaded([NotNullWhen(true)] out IConfiguration? configuration)
        {
            configuration = configurationRoot ?? defaultConfiguration;
            return configuration is not null;
        }

        private static IConfigurationRoot Build(Action<AzureAppConfigurationOptions> action) =>
            new ConfigurationBuilder()
                .AddAzureAppConfiguration(action)
                .Build();
    }
    ```

3. Add the following namespaces to your main program builder file:

    ```cs
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

4. Load the Azure app configuration into the app:

    ```cs
    // Retrieve the connection string
    var connectionString = builder.Configuration.GetConnectionString("MyAppConfig");

    // Retrieve the default values for the remote configuration
    var defaults = builder.Configuration.GetRequiredSection("MyAppConfigDefaults");

    // Load configuration from Azure App Configuration
    var appConfig = new AsyncAzureAppConfig(connectionString!);
    appConfig.SetDefaults(defaults);
    builder.Services.AddSingleton(appConfig);

    // The rest of the existing code in MauiProgram.cs
    // ...
    ```

5. Configuration values can be read asynchronously and the application can wait for the configuration to load and then update accordingly:

    ```cs
    public class MyViewModel
    {
        private readonly AsyncAzureAppConfig config;

        public MyViewModel(AsyncAzureAppConfig configuration)
        {
            config = configuration;

            configuration.LoadAsync()
                .ContinueWith(task =>
                {
                    if (task.IsCompletedSuccessfully)
                        Console.WriteLine($"Success: {MySetting}");
                    else
                        Console.WriteLine($"Failed: {task.Exception}");
                });
        }

        public string MySetting =>
            config["MyMobileApp:HomePage:WelcomeMessage"] ?? "<unavailable>";
    }
    ```
