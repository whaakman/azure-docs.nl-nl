---
title: Azure Resource Manager-groepen beheren met behulp van Azure PowerShell | Microsoft Docs
description: Azure PowerShell gebruiken om uw Azure Resource Manager-groepen te beheren.
services: azure-resource-manager
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jgao
ms.openlocfilehash: 5197358e3bd8a3052fbf71cafc2f1e3acda46b26
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721143"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-azure-powershell"></a>Azure Resource Manager-resourcegroepen beheren met behulp van Azure PowerShell

Informatie over het gebruik van Azure PowerShell gebruiken met [Azure Resource Manager](resource-group-overview.md) voor het beheren van uw Azure-resourcegroepen. Zie voor het beheren van Azure-resources, [Azure-resources beheren met behulp van Azure PowerShell](./manage-resources-powershell.md).

Andere artikelen over het beheren van resourcegroepen:

- [Azure-resourcegroepen beheren met behulp van de Azure-portal](./manage-resources-portal.md)
- [Azure-resourcegroepen beheren met behulp van Azure CLI](./manage-resources-cli.md)

## <a name="what-is-a-resource-group"></a>Wat is er een resourcegroep

Een resourcegroep is een container met gerelateerde resources voor een Azure-oplossing. De resourcegroep kan alle resources voor de oplossing bevatten of enkel de resources die u als groep wilt beheren. U bepaalt hoe resources worden toegewezen aan resourcegroepen op basis van wat voor uw organisatie het meest zinvol is. Over het algemeen resources die aan dezelfde resourcegroep dezelfde levenscyclus delen, zodat u kunt eenvoudig implementeren, bijwerken en als een groep verwijderen toevoegen.

De resourcegroep slaat metagegevens op over de resources. Dat is de reden waarom u moet aangeven waar die metagegevens moeten worden opgeslagen als u een locatie voor de resourcegroep opgeeft. In verband met nalevingsvereisten moet u er mogelijk voor zorgen dat uw gegevens worden opgeslagen in een bepaalde regio.

De resourcegroep slaat metagegevens op over de resources. Wanneer u een locatie voor de resourcegroep opgeeft, bent u op te geven waar die metagegevens worden opgeslagen.

## <a name="create-resource-groups"></a>Maken van resourcegroepen

De volgende PowerShell-script wordt een resourcegroep, en geeft vervolgens de resourcegroep.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location

Get-AzResourceGroup -Name $resourceGroupName
```

## <a name="list-resource-groups"></a>Lijst met resourcegroepen

De volgende PowerShell-script geeft een lijst van de resourcegroepen onder uw abonnement.

```azurepowershell-interactive
Get-AzResourceGroup
```

Een resourcegroep ophalen:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"

Get-AzResourceGroup -Name $resourceGroupName
```

## <a name="delete-resource-groups"></a>Resourcegroepen verwijderen

De volgende PowerShell-script wordt een resourcegroep verwijderd:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"

Remove-AzResourceGroup -Name $resourceGroupName
```

Zie voor meer informatie over hoe Azure Resource Manager orders voor het verwijderen van resources, [verwijderen voor groep van Azure Resource Manager-resource](./resource-group-delete.md).

## <a name="deploy-resources-to-an-existing-resource-group"></a>Resources implementeren op een bestaande resourcegroep

Zie [resources implementeren op een bestaande resourcegroep](./manage-resources-powershell.md#deploy-resources-to-an-existing-resource-group).

Zie voor het valideren van implementatie van een resourcegroep, [Test AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/Az.Resources/Test-AzResourceGroupDeployment?view=azps-1.3.0).

## <a name="deploy-a-resource-group-and-resources"></a>Een resourcegroep en resources implementeren

U kunt een resourcegroep maken en implementeren van resources aan de groep met behulp van Resource Manager-sjabloon. Zie voor meer informatie, [resourcegroep maken en implementeren van resources](./deploy-to-subscription.md#create-resource-group-and-deploy-resources).

## <a name="redeploy-when-deployment-fails"></a>Opnieuw implementeren wanneer de implementatie mislukt

Deze functie wordt ook wel bekend als *Rollback bij fout*. Zie voor meer informatie, [opnieuw implementeren wanneer de implementatie mislukt](./resource-group-template-deploy.md#redeploy-when-deployment-fails).

## <a name="move-to-another-resource-group-or-subscription"></a>Verplaatsen naar een andere resourcegroep of abonnement

U kunt de resources in de groep verplaatsen naar een andere resourcegroep. Zie voor meer informatie [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](./resource-group-move-resources.md).

## <a name="lock-resource-groups"></a>LOCK-resourcegroepen

Vergrendeling wordt voorkomen dat andere gebruikers in uw organisatie per ongeluk verwijderen of aanpassen van kritieke resources, zoals Azure-abonnement, resourcegroep of resource. 

Het volgende script Hiermee vergrendelt u een resourcegroep, zodat de resourcegroep kan niet worden verwijderd.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"

New-AzResourceLock -LockName LockGroup -LockLevel CanNotDelete -ResourceGroupName $resourceGroupName 
```

Het volgende script haalt alle vergrendelingen voor een resourcegroep:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"

Get-AzResourceLock -ResourceGroupName $resourceGroupName 
```

Zie voor meer informatie [Resources vergrendelen met Azure Resource Manager](resource-group-lock-resources.md).

## <a name="tag-resource-groups"></a>Tag-resourcegroepen

U kunt tags toepassen op resourcegroepen en resources voor uw activa logische manier te organiseren. Zie voor meer informatie, [met tags voor het ordenen van uw Azure-resources](./resource-group-using-tags.md#powershell).

## <a name="export-resource-groups-to-templates"></a>Resourcegroepen naar sjablonen exporteren

Na het instellen van de resourcegroep, kunt u een Resource Manager-sjabloon voor de resourcegroep bekijken. De sjabloon exporteren biedt twee voordelen:

- Automatiseer toekomstige implementaties van de oplossing omdat de sjabloon de volledige-infrastructuur bevat.
- Meer informatie over de sjabloonsyntaxis van de door te kijken op JavaScript Object Notation (JSON) die uw oplossing aangeeft.

Gebruiken voor het exporteren van alle resources in een resourcegroep, de [Export-AzResourceGroup](/powershell/module/az.resources/Export-AzResourceGroup) cmdlet en geef de naam van de resourcegroep.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"

Export-AzResourceGroup -ResourceGroupName $resourceGroupName
```

De sjabloon wordt opgeslagen als een lokaal bestand.

In plaats van alle resources in de resourcegroep exporteren, kunt u selecteren welke resources om te exporteren.

Voor het exporteren van een resource, doorgeven die resource-ID.

```azurepowershell-interactive
$resource = Get-AzResource `
  -ResourceGroupName <resource-group-name> `
  -ResourceName <resource-name> `
  -ResourceType <resource-type>
Export-AzResourceGroup `
  -ResourceGroupName <resource-group-name> `
  -Resource $resource.ResourceId
```

Voor het exporteren van meer dan één resource, geeft u de resource-id's in een matrix.

```azurepowershell-interactive
Export-AzResourceGroup `
  -ResourceGroupName <resource-group-name> `
  -Resource @($resource1.ResourceId, $resource2.ResourceId)
```

Bij het exporteren van de sjabloon, kunt u opgeven of de parameters worden gebruikt in de sjabloon. Standaard parameters voor de namen van voorbeeldresources zijn opgenomen, maar ze geen standaardwaarde hebben. U moet de waarde van deze parameter doorgeven tijdens implementatie.

```json
"parameters": {
  "serverfarms_demoHostPlan_name": {
    "defaultValue": null,
    "type": "String"
  },
  "sites_webSite3bwt23ktvdo36_name": {
    "defaultValue": null,
    "type": "String"
  }
}
```

De parameter wordt gebruikt in de bron voor de naam.

```json
"resources": [
  {
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2016-09-01",
    "name": "[parameters('serverfarms_demoHostPlan_name')]",
    ...
  }
]
```

Als u de `-IncludeParameterDefaultValue` parameter bij het exporteren van de sjabloon, de sjabloonparameter bevat een standaardwaarde die is ingesteld op de huidige waarde. U kunt deze standaardwaarde gebruiken of de standaardwaarde overschrijven door te geven in een andere waarde.

```json
"parameters": {
  "serverfarms_demoHostPlan_name": {
    "defaultValue": "demoHostPlan",
    "type": "String"
  },
  "sites_webSite3bwt23ktvdo36_name": {
    "defaultValue": "webSite3bwt23ktvdo36",
    "type": "String"
  }
}
```

Als u de `-SkipResourceNameParameterization` parameter bij het exporteren van de sjabloon, parameters voor de resourcenamen niet zijn opgenomen in de sjabloon. In plaats daarvan de naam van de resource rechtstreeks op de resource toe aan de huidige waarde is ingesteld. U kunt de naam van de tijdens de implementatie niet aanpassen.

```json
"resources": [
  {
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2016-09-01",
    "name": "demoHostPlan",
    ...
  }
]
```

Zie voor meer informatie, [één of meerdere resources exporteren naar de sjabloon in Azure portal](./export-template-portal.md).

## <a name="manage-access-to-resource-groups"></a>Toegang tot resourcegroepen beheren

[Op rollen gebaseerde toegangsbeheer (RBAC)](../role-based-access-control/overview.md) is de manier waarop u de toegang tot resources in Azure beheert. Zie voor meer informatie, [toegang met RBAC en Azure PowerShell beheren](../role-based-access-control/role-assignments-powershell.md).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer Azure Resource Manager [overzicht van Azure Resource Manager](./resource-group-overview.md).
- Zie voor meer informatie de syntaxis van de Resource Manager-sjabloon, [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](./resource-group-authoring-templates.md).
- Zie voor meer informatie over het ontwikkelen van sjablonen, de [zelfstudies met stapsgewijze instructies](/azure/azure-resource-manager/).
- Als u de Azure Resource Manager-sjabloon schema's, Zie [sjabloonverwijzing](/azure/templates/).