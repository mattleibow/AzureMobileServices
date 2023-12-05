
1. Load the settings as an `object?`:

    ```cs
    var data = AppContext.GetData("MySetting");
    if (data is string value)
    {
        // do something with the string value
    }
    ```

2. If the setting is just a boolean value, then there is a convenient `TryGetSwitch`:

    ```cs
    var wasFound = AppContext.TryGetSwitch("MySetting", out var isEnabled);
    ```
