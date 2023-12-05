# Configuration

https://learn.microsoft.com/core/extensions/configuration

## Using `appsettings.json`

https://learn.microsoft.com/aspnet/core/fundamentals/configuration

.NET MAUI uses the same configuration system as ASP.NET, except that many of the features are opt-in to reduce app size and increase start-up performance.

One such feature that we may want to use is settings files, such as `appsettings.json`. This feature is enabled via the `JsonConfigurationProvider`.

### Adding an `appsettings.json`

#### [.NET MAUI](#tab/dotnet-maui)

To enable `appsettings.json` in .NET MAUI apps, we need to do a few things:

1. Create a new `appsettings.json` file in the `Resources/Raw` folder of your project:
    > The full path is `<project-dir>/Resources/Raw/appsettings.json`
    ```json
    {
        "MySetting": "MyValue"
    }
    ```

2. Install the `Microsoft.Extensions.Configuration.Json` NuGet package.
3. Add the following namespaces to your `MauiProgram.cs` file:

    ```cs
    using Microsoft.Extensions.Configuration;
    ```

4. Load the `appsettings.json` file into the configuration manager by using `AddJsonStream` in your `MauiProgram.cs` file:

    ```cs
    var stream = FileSystem.OpenAppPackageFileAsync("appsettings.json").Result;
    builder.Configuration.AddJsonStream(stream);
    ```

#### [ASP.NET](#tab/dotnet-aspnet)

The default web app builder automatically reads the `appsettings.json` file, so nothong needs to be done.

---

### Consuming `appsettings.json`

Once your app has loaded the configurations, it is easy to use:

```cs
public class MyViewModel
{
    private readonly IConfiguration config;

    public MyViewModel(IConfiguration configuration)
    {
        config = configuration;
    }

    public string MySetting =>
        config["MySetting"] ?? "<unavailable>";
}
```

## Using .NET Runtime configuration settings

Loading files on app startup may have performance implications, so an alternative is to make use of the .NET Runtime configuration settings.

These settings are only boolean values, so can only be "on" or "off".

https://learn.microsoft.com/core/runtime-config

### Adding .NET Runtime configuration settings

Create a new `runtimeconfig.template.json` file in the project root folder:

> The full path is `<project-dir>/runtimeconfig.template.json`

```json
{
    "configProperties":
    {
        "MySetting": "MyValue"
    }
}
```

### Consuming .NET Runtime configuration settings

Load the settings as an `object?`:

```cs
var data = AppContext.GetData("MySetting");
if (data is string value)
{
    // do something with the string value
}
```

If the setting is just a boolean value, then there is a convenient `TryGetSwitch`:

```cs
var wasFound = AppContext.TryGetSwitch("MySetting", out var isEnabled);
```
