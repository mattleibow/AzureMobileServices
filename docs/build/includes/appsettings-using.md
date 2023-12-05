
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
