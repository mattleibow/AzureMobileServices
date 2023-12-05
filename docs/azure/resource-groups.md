# Resource Groups

A resource group is a container that holds related resources for an Azure solution. The resource group can include all the resources for the solution, or only those resources that you want to manage as a group. You decide how you want to add resources to resource groups based on what makes the most sense for your organization. Generally, add resources that share the same lifecycle to the same resource group so you can easily deploy, update, and delete them as a group.

The resource group stores metadata about the resources. When you specify a location for the resource group, you're specifying where that metadata is stored. For compliance reasons, you may need to ensure that your data is stored in a particular region.

**Read more**
* [Read more about resource groups](https://learn.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-cli).
* [Read more about regions](https://azure.microsoft.com/explore/global-infrastructure/geographies/).
* [Read more about which services are available in which regions](https://azure.microsoft.com/explore/global-infrastructure/products-by-region).

#### [Azure CLI](#tab/azure-cli)

To list supported regions for the current subscription, run:
```
az account list-locations
```
---

Once you have your preferred location for the resource group, you can create it:

#### [Azure CLI](#tab/azure-cli)

To create a resource group, run:
```
az group create \
  --name <rg-name> \
  --location <rg-location>
```

> For example, to create a resource group named `MyMobileResources` in `southafricanorth` (Johannesburg, South Africa North):
>
> ```
> az group create \
>   --name "MyMobileResources" \
>   --location "southafricanorth"
> ```
---
