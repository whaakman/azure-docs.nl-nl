---
title: Azure-resources beheren met behulp van Azure CLI | Microsoft Docs
description: Azure CLI en Azure Resource Manager gebruiken om uw resources te beheren.
services: azure-resource-manager
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jgao
ms.openlocfilehash: c7a863c017bbd7440704f024ee362c3a0d252891
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56825113"
---
# <a name="manage-azure-resources-by-using-azure-cli"></a>Azure-resources beheren met behulp van Azure CLI

Informatie over het gebruik van Azure CLI met [Azure Resource Manager](resource-group-overview.md) voor het beheren van uw Azure-resources. Zie voor het beheren van resourcegroepen, [beheren Azure-resourcegroepen met behulp van Azure CLI](./manage-resource-groups-cli.md).

Andere artikelen over het beheren van resources:

- [Azure-resources beheren met behulp van Azure portal](./manage-resources-portal.md)
- [Azure-resources beheren met behulp van Azure CLI](./manage-resources-cli.md)

## <a name="deploy-resources-to-an-existing-resource-group"></a>Resources implementeren op een bestaande resourcegroep

U kunt Azure-resources implementeren via Azure PowerShell of een Resource Manager-sjabloon voor het maken van Azure-resources implementeren.

### <a name="deploy-a-resource"></a>Implementeren van een resource

Het volgende script maakt een storage-account.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
echo "Enter the storage account name:" &&
read storageAccountName &&
az storage account create --resource-group $resourceGroupName --name $storageAccountName --location $location --sku Standard_LRS --kind StorageV2 &&
az storage account show --resource-group $resourceGroupName --name $storageAccountName 
```

### <a name="deploy-a-template"></a>Een sjabloon implementeren

Het volgende script maakt een Quickstart-sjabloon voor het maken van een opslagaccount te implementeren. Zie voor meer informatie [Snelstart: Azure Resource Manager-sjablonen maken met behulp van Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md?tabs=PowerShell).

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
az group deployment create --resource-group $resourceGroupName --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

Zie voor meer informatie, [resources implementeren met Resource Manager-sjablonen en Azure CLI](./resource-group-template-deploy-cli.md).

## <a name="deploy-a-resource-group-and-resources"></a>Een resourcegroep en resources implementeren

U kunt een resourcegroep maken en implementeren van resources aan de groep. Zie voor meer informatie, [resourcegroep maken en implementeren van resources](./deploy-to-subscription.md#create-resource-group-and-deploy-resources).

## <a name="deploy-resources-to-multiple-subscriptions-or-resource-groups"></a>Resources implementeren op meerdere abonnementen of resourcegroepen

Normaal gesproken implementeren u alle resources in uw sjabloon één resourcegroep bestaan. Er zijn echter scenario's waarin u wilt een set met resources samen te implementeren, maar plaats deze in verschillende resourcegroepen of abonnementen. Zie voor meer informatie, [implementeren-Azure-resources voor meerdere abonnementen of resourcegroepen](./resource-manager-cross-resource-group-deployment.md).

## <a name="delete-resources"></a>Resources verwijderen

Het volgende script toont hoe u een opslagaccount verwijdert.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
az storage account delete --resource-group $resourceGroupName --name $storageAccountName 
```

Zie voor meer informatie over hoe Azure Resource Manager orders voor het verwijderen van resources, [verwijderen voor groep van Azure Resource Manager-resource](./resource-group-delete.md).

## <a name="move-resources"></a>Resources verplaatsen

Het volgende script laat zien hoe een storage-account verwijderen uit een resourcegroep naar een andere resourcegroep.

```azurecli-interactive
echo "Enter the source Resource Group name:" &&
read srcResourceGroupName &&
echo "Enter the destination Resource Group name:" &&
read destResourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
storageAccount=$(az resource show --resource-group $srcResourceGroupName --name $storageAccountName --resource-type Microsoft.Storage/storageAccounts --query id --output tsv) &&
az resource move --destination-group $destResourceGroupName --ids $storageAccount
```

Zie voor meer informatie [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](resource-group-move-resources.md).

## <a name="lock-resources"></a>Resources vergrendelen

Vergrendeling wordt voorkomen dat andere gebruikers in uw organisatie per ongeluk verwijderen of aanpassen van kritieke resources, zoals Azure-abonnement, resourcegroep of resource. 

Het volgende script wordt een storage-account vergrendeld, zodat het account kan niet worden verwijderd.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
az lock create --name LockSite --lock-type CanNotDelete --resource-group $resourceGroupName --resource-name $storageAccountName --resource-type Microsoft.Storage/storageAccounts 
```

Het volgende script haalt alle vergrendelingen voor een opslagaccount:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
az lock list --resource-group $resourceGroupName --resource-name $storageAccountName --resource-type Microsoft.Storage/storageAccounts --parent ""
```

Het volgende script wordt een vergrendeling van een storage-account verwijderd:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
lockId=$(az lock show --name LockSite --resource-group $resourceGroupName --resource-type Microsoft.Storage/storageAccounts --resource-name $storageAccountName --output tsv --query id)&&
az lock delete --ids $lockId
```

Zie voor meer informatie [Resources vergrendelen met Azure Resource Manager](resource-group-lock-resources.md).

## <a name="tag-resources"></a>Resources taggen

Helpt bij het organiseren van uw resourcegroep en resources logisch-tagging. Zie voor meer informatie, [met tags voor het ordenen van uw Azure-resources](./resource-group-using-tags.md#azure-cli).

## <a name="manage-access-to-resources"></a>Toegang tot resources beheren

[Op rollen gebaseerde toegangsbeheer (RBAC)](../role-based-access-control/overview.md) is de manier waarop u de toegang tot resources in Azure beheert. Zie voor meer informatie, [beheren van toegang via RBAC en Azure CLI](../role-based-access-control/role-assignments-cli.md).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer Azure Resource Manager [overzicht van Azure Resource Manager](./resource-group-overview.md).
- Zie voor meer informatie de syntaxis van de Resource Manager-sjabloon, [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](./resource-group-authoring-templates.md).
- Zie voor meer informatie over het ontwikkelen van sjablonen, de [zelfstudies met stapsgewijze instructies](/azure/azure-resource-manager/).
- Als u de Azure Resource Manager-sjabloon schema's, Zie [sjabloonverwijzing](/azure/templates/).