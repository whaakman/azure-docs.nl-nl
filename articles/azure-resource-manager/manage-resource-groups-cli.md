---
title: Azure Resource Manager-groepen beheren met behulp van Azure CLI | Microsoft Docs
description: Azure CLI gebruiken om uw Azure Resource Manager-groepen te beheren.
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
ms.openlocfilehash: c9e6cc68cbd629642849d323d4271722f3232aa3
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56825204"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-azure-cli"></a>Azure Resource Manager-resourcegroepen beheren met behulp van Azure CLI

Informatie over het gebruik van Azure CLI met [Azure Resource Manager](resource-group-overview.md) voor het beheren van uw Azure-resourcegroepen. Zie voor het beheren van Azure-resources, [Azure-resources beheren met behulp van Azure CLI](./manage-resources-cli.md).

Andere artikelen over het beheren van resourcegroepen:

- [Azure-resourcegroepen beheren met behulp van de Azure-portal](./manage-resources-portal.md)
- [Azure-resourcegroepen beheren met behulp van Azure PowerShell](./manage-resources-powershell.md)

## <a name="what-is-a-resource-group"></a>Wat is een resourcegroep

Een resourcegroep is een container met gerelateerde resources voor een Azure-oplossing. De resourcegroep kan alle resources voor de oplossing bevatten of enkel de resources die u als groep wilt beheren. U bepaalt hoe resources worden toegewezen aan resourcegroepen op basis van wat voor uw organisatie het meest zinvol is. Over het algemeen resources die aan dezelfde resourcegroep dezelfde levenscyclus delen, zodat u kunt eenvoudig implementeren, bijwerken en als een groep verwijderen toevoegen.

De resourcegroep slaat metagegevens op over de resources. Dat is de reden waarom u moet aangeven waar die metagegevens moeten worden opgeslagen als u een locatie voor de resourcegroep opgeeft. In verband met nalevingsvereisten moet u er mogelijk voor zorgen dat uw gegevens worden opgeslagen in een bepaalde regio.

De resourcegroep slaat metagegevens op over de resources. Wanneer u een locatie voor de resourcegroep opgeeft, bent u op te geven waar die metagegevens worden opgeslagen.

## <a name="create-resource-groups"></a>Maken van resourcegroepen

De volgende CLI-script wordt een resourcegroep, en geeft vervolgens de resourcegroep.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
az group create --name $resourceGroupName --location $location
```

## <a name="list-resource-groups"></a>Een lijst met resourcegroepen maken

De volgende CLI-script geeft een lijst van de resourcegroepen onder uw abonnement.

```azurecli-interactive
az group list
```

Een resourcegroep ophalen:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group show --name $resourceGroupName
```

## <a name="delete-resource-groups"></a>Resourcegroepen verwijderen

De volgende CLI-script wordt een resourcegroep verwijderd:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

Zie voor meer informatie over hoe Azure Resource Manager orders voor het verwijderen van resources, [verwijderen voor groep van Azure Resource Manager-resource](./resource-group-delete.md).

## <a name="deploy-resources-to-an-existing-resource-group"></a>Resources implementeren op een bestaande resourcegroep

Zie [resources implementeren op een bestaande resourcegroep](./manage-resources-cli.md#deploy-resources-to-an-existing-resource-group).

## <a name="deploy-a-resource-group-and-resources"></a>Een resourcegroep en resources implementeren

U kunt een resourcegroep maken en implementeren van resources aan de groep met behulp van Resource Manager-sjabloon. Zie voor meer informatie, [resourcegroep maken en implementeren van resources](./deploy-to-subscription.md#create-resource-group-and-deploy-resources).

## <a name="move-to-another-resource-group-or-subscription"></a>Verplaatsen naar een andere resourcegroep of abonnement

U kunt de resources in de groep verplaatsen naar een andere resourcegroep. Zie voor meer informatie, [resources verplaatsen](./manage-resources-cli.md#move-resources).

## <a name="lock-resource-groups"></a>LOCK-resourcegroepen

Vergrendeling wordt voorkomen dat andere gebruikers in uw organisatie per ongeluk verwijderen of aanpassen van kritieke resources, zoals Azure-abonnement, resourcegroep of resource. 

Het volgende script Hiermee vergrendelt u een resourcegroep, zodat de resourcegroep kan niet worden verwijderd.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az lock create --name LockGroup --lock-type CanNotDelete --resource-group $resourceGroupName  
```

Het volgende script haalt alle vergrendelingen voor een resourcegroep:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az lock list --resource-group $resourceGroupName  
```

Het volgende script wordt een vergrendeling verwijderd:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the lock name:" &&
read lockName &&
az lock delete --name $lockName --resource-group $resourceGroupName
```

Zie voor meer informatie [Resources vergrendelen met Azure Resource Manager](resource-group-lock-resources.md).

## <a name="tag-resource-groups"></a>Tag-resourcegroepen

U kunt tags toepassen op resourcegroepen en resources voor uw activa logische manier te organiseren. Zie voor meer informatie, [met tags voor het ordenen van uw Azure-resources](./resource-group-using-tags.md#azure-cli).

## <a name="export-resource-groups-to-templates"></a>Resourcegroepen naar sjablonen exporteren

Na het instellen van uw resourcegroep is, is het raadzaam om de Resource Manager-sjabloon voor de resourcegroep weer te geven. De sjabloon exporteren biedt twee voordelen:

- Automatiseer toekomstige implementaties van de oplossing omdat de sjabloon de volledige infrastructuur bevat.
- Meer informatie over de sjabloonsyntaxis van de door te kijken op JavaScript Object Notation (JSON) die uw oplossing aangeeft.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group export --name $resourceGroupName  
```

Het script wordt de sjabloon op de console weergegeven.  De JSON kopiëren en opslaan als een bestand.

Zie voor meer informatie, [resourcegroep exporteren](./manage-resource-groups-portal.md#export-resource-groups-to-templates).

## <a name="manage-access-to-resource-groups"></a>Toegang tot resourcegroepen beheren

[Op rollen gebaseerde toegangsbeheer (RBAC)](../role-based-access-control/overview.md) is de manier waarop u de toegang tot resources in Azure beheert. Zie voor meer informatie, [beheren van toegang via RBAC en Azure CLI](../role-based-access-control/role-assignments-cli.md).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer Azure Resource Manager [overzicht van Azure Resource Manager](./resource-group-overview.md).
- Zie voor meer informatie de syntaxis van de Resource Manager-sjabloon, [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](./resource-group-authoring-templates.md).
- Zie voor meer informatie over het ontwikkelen van sjablonen, de [zelfstudies met stapsgewijze instructies](/azure/azure-resource-manager/).
- Als u de Azure Resource Manager-sjabloon schema's, Zie [sjabloonverwijzing](/azure/templates/).