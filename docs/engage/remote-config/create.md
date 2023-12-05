# Create an App Configuration store

TODO add some docs here

## Prerequisites

[!INCLUDE [azure-cli-prereqs](../../includes/azure-cli-prereqs.md)]

## Create a resource group

In order to create a App Configuration store, we need a resource group.

**Read more**

* [Read more about creating resource groups](../../azure/resource-groups.md).


## Create an App Configuration store

Create a new store with the az group create command and replace the placeholder <name> with a unique resource name for your App Configuration store.

#### [Azure CLI](#tab/azure-cli)

To create a new app configuration store, run:

```
az appconfig create \
  --resource-group <rg-name> \
  --location <rg-location> \
  --name <name>
```
> For example, to create an app config named `MyMobileConfig` in an existing resource group named `MyMobileResources`:
>
> ```
> az appconfig create \
>   --resource-group "MyMobileResources" \
>   --location "southafricanorth" \
>   --name "MyMobileConfig"
> ```
---

## Retrieve connection string

Since our app is just going to be reading from the configuration, we can retrieve the "Primary Read Only" connection string.

#### [Azure CLI](#tab/azure-cli)

To retrieve the connection strings for the app configuration, run:

```
az appconfig credential list \
  --resource-group <rg-name> \
  --name <name>
```
> For example, to retrieve the connection strings for an app config named `MyMobileConfig` in hte resource group named `MyMobileResources`:
>
> ```
> az appconfig credential list \
>   --resource-group "MyMobileResources" \
>   --name "MyMobileConfig"
> ```

The result of the query will be a json array with all the connection strings:

```
[
  {
    "connectionString": "Endpoint=https://<name>.azconfig.io;Id=<id>;Secret=<secret>",
    "id": "<id>",
    "lastModified": "YYYY-MM-DDTHH:mm:ss+00:00",
    "name": "Primary",
    "readOnly": false,
    "value": "<secret>"
  },
  {
    "connectionString": "Endpoint=https://<name>.azconfig.io;Id=<id>;Secret=<secret>",
    "id": "<id>",
    "lastModified": "YYYY-MM-DDTHH:mm:ss+00:00",
    "name": "Secondary",
    "readOnly": false,
    "value": "<secret>"
  },
  {
    "connectionString": "Endpoint=https://<name>.azconfig.io;Id=<id>;Secret=<secret>",
    "id": "<id>",
    "lastModified": "YYYY-MM-DDTHH:mm:ss+00:00",
    "name": "Primary Read Only",
    "readOnly": true,
    "value": "<secret>"
  },
  {
    "connectionString": "Endpoint=https://<name>.azconfig.io;Id=<id>;Secret=<secret>",
    "id": "<id>",
    "lastModified": "YYYY-MM-DDTHH:mm:ss+00:00",
    "name": "Secondary Read Only",
    "readOnly": true,
    "value": "<secret>"
  }
]
```

---
