# Create a key-value configuration

TODO add some docs here

## Prerequisites

[!INCLUDE [azure-cli-prereqs](../../includes/azure-cli-prereqs.md)]

## Creating a configuration

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
