# Remote App Configuration

Azure App Configuration provides a service to centrally manage application settings and feature flags. Modern programs, especially programs running in a cloud, generally have many components that are distributed in nature. Spreading configuration settings across these components can lead to hard-to-troubleshoot errors during an application deployment. Use App Configuration to store all the settings for your application and secure their accesses in one place.

**Read more**

* [Read more about Azure App Configuration](https://learn.microsoft.com/azure/azure-app-configuration).

## Prerequisites

[!INCLUDE [resource-groups](../../../includes/azure-cli-prereqs.md)]

## Create a resource group

In order to create a Cosmos DB account, we need a resource group.

**Read more**

* [Read more about creating resource groups](../../../azure/resource-groups.md).


## Create an App Configuration store

Create a new store with the az group create command and replace the placeholder <name> with a unique resource name for your App Configuration store.

#### [Azure CLI](#tab/azure-cli)

To create a new app configuration store, run:

```
az appconfig create \
  --resource-group <rg-name> \
  --location centralus \
  --name <name>
```
> For example, to create a Cosmos DB account named `mymobiledataaccount` in an existing resource group named `MyMobileResources`:
>
> ```
> az cosmosdb create \
>   --resource-group "MyMobileResources" \
>   --name "mymobiledataaccount"
> ```
---
