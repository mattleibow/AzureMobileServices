# Create a Cosmos DB database

TODO add more docs about all this.

## Prerequisites

[!INCLUDE [azure-cli-prereqs](../../../includes/azure-cli-prereqs.md)]

## Create a resource group

In order to create a Cosmos DB account, we need a resource group.

**Read more**

* [Read more about creating resource groups](../../../azure/resource-groups.md).

## Create an account

A Cosmos DB Account is the top level resource in Azure Cosmos DB. It is the entry point to our Cosmos DB instances.

The following features are defined at the Cosmos DB Account level:

* A unique DNS name used to connect with database instances like  
  `https://{account}.documents.azure.com/`
* Global distribution capabilities _(you can define how many regions you want to distribute your data across)_.
* The default consistency level for all resources in the account _(you can override this for individual resources)_.


> [!NOTE]
> A Cosmos DB account name must be lowercase and have a maximum length of 44 characters.

#### [Azure CLI](#tab/azure-cli)

To create a new Azure Cosmos DB for NoSQL account with default settings, run:

```
az cosmosdb create \
  --resource-group <rg-name> \
  --name <account-name>
```
> For example, to create a Cosmos DB account named `mymobiledataaccount` in an existing resource group named `MyMobileResources`:
>
> ```
> az cosmosdb create \
>   --resource-group "MyMobileResources" \
>   --name "mymobiledataaccount"
> ```
---

## Create a database

Create an Azure Cosmos DB database using the API for NoSQL.

#### [Azure CLI](#tab/azure-cli)

To create a new database with default settings, run:

```
az cosmosdb sql database create \
  --resource-group <rg-name> \
  --account-name <account-name> \
  --name <db-name>
```
> For example, to create a database named `MyMobileData` in an existing Cosmos DB account `mymobiledataaccount`:
>
> ```
> az cosmosdb sql database create \
>   --resource-group "MyMobileResources" \
>   --account-name "mymobiledataaccount" \
>   --name "MyMobileData"
> ```
---

## Create a container

Create an Azure Cosmos DB container with a partition key and default settings.

#### [Azure CLI](#tab/azure-cli)

To create a new container with default settings, run:

```
az cosmosdb sql container create \
  --resource-group <rg-name> \
  --account-name <account-name> \
  --database-name <db-name> \
  --name <conatiner-name> \
  --partition-key-path <partition-key-path>
```
> For example, to create a container named `MyBooks` in an existing database `MyMobileData`:
>
> ```
> az cosmosdb sql container create \
>   --resource-group "MyMobileResources" \
>   --account-name "mymobiledataaccount" \
>   --database-name "MyMobileData" \
>   --name "MyBooks" \
>   --partition-key-path "/publish_year"
> ```
---
