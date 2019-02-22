---
title: Azure-resources om te voorkomen dat wijzigingen vergrendelen | Microsoft Docs
description: Voorkomen dat gebruikers bijwerken of verwijderen van essentiële Azure-resources door het toepassen van een vergrendeling voor alle gebruikers en rollen.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 53c57e8f-741c-4026-80e0-f4c02638c98b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: tomfitz
ms.openlocfilehash: 83518825c91cdd727b3d4fb9ecc86d51dea8fc26
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56649166"
---
# <a name="lock-resources-to-prevent-unexpected-changes"></a>Resources vergrendelen om onverwachte wijzigingen te voorkomen 

Als beheerder moet u mogelijk een abonnement, resourcegroep of resource om te voorkomen dat andere gebruikers in uw organisatie per ongeluk verwijderen of aanpassen van kritieke resources vergrendelen. U kunt de vergrendeling instellen op **CanNotDelete** of **ReadOnly**. In de portal, de vergrendelingen worden genoemd **verwijderen** en **alleen-lezen** respectievelijk.

* **CanNotDelete** betekent geautoriseerde gebruikers kunnen nog steeds lezen en wijzigen van een resource, maar ze kunnen de resource niet verwijderen. 
* **Alleen-lezen** betekent geautoriseerde gebruikers kunnen een resource lezen, maar ze niet verwijderen of bijwerken van de resource. Toepassen van deze vergrendeling is vergelijkbaar met alle gemachtigde gebruikers beperken tot de machtigingen die zijn verleend door de **lezer** rol. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="how-locks-are-applied"></a>Hoe vergrendelingen worden toegepast

Wanneer u een vergrendeling op een overkoepelend bereik toepast, nemen alle resources binnen dat bereik de dezelfde vergrendeling. Zelfs resources die u later toevoegen overnemen de vergrendeling van het bovenliggende item. De meest beperkende vergrendeling in de overname voorrang.

In tegenstelling tot de op rollen gebaseerd toegangsbeheer gebruikt u beheervergrendelingen voor het toepassen van een beperking voor alle gebruikers en rollen. Zie voor meer informatie over het instellen van machtigingen voor gebruikers en rollen, [Azure Role-based Access Control](../role-based-access-control/role-assignments-portal.md).

Resource Manager-vergrendelingen gelden alleen voor bewerkingen die in de beheerlaag, die uit bewerkingen die worden verzonden optreden bestaat naar `https://management.azure.com`. De vergrendelingen beperken niet hoe resources hun eigen functies uitvoeren. Wijzigingen van resources zijn beperkt, maar de bewerkingen van resources zijn niet beperkt. Bijvoorbeeld, een alleen-lezenvergrendeling op een SQL-Database wordt voorkomen dat u verwijderen of aanpassen van de database, maar deze niet te voorkomen dat u het maken, bijwerken of verwijderen van gegevens in de database. Gegevenstransacties zijn toegestaan, omdat deze bewerkingen worden niet naar verzonden `https://management.azure.com`.

Toepassen van **ReadOnly** kan leiden tot onverwachte resultaten omdat bepaalde bewerkingen die kunnen worden gelezen bewerkingen daadwerkelijk extra acties vereist. Bijvoorbeeld, als u plaatst een **ReadOnly** vergrendeling op een storage-account wordt voorkomen dat alle gebruikers van de aanbieding van de sleutels. De lijst met sleutels bewerking via een POST-aanvraag wordt verwerkt, omdat de geretourneerde sleutels zijn beschikbaar voor schrijfbewerkingen. Voor een ander voorbeeld: als u plaatst een **alleen-lezen** vergrendeling op een App Service-resource voorkomt dat Visual Studio Server Explorer weergeven van bestanden voor de resource omdat die interactie schrijftoegang is vereist.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Wie kunt maken of verwijderen van vergrendelingen in uw organisatie
Als u wilt maken of verwijderen van beheervergrendelingen, u moet toegang hebben tot `Microsoft.Authorization/*` of `Microsoft.Authorization/locks/*` acties. Van de ingebouwde rollen worden deze acties alleen toegekend aan **Eigenaar** en **Administrator voor gebruikerstoegang**.

## <a name="portal"></a>Portal
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a>Template

Wanneer u een Resource Manager-sjabloon voor het implementeren van een vergrendeling, kunt u verschillende waarden gebruiken voor de naam en type afhankelijk van het bereik van de vergrendeling.

Bij het toepassen van een vergrendeling op een **resource**, gebruikt u de volgende indelingen:

* naam: `{resourceName}/Microsoft.Authorization/{lockName}`
* type- `{resourceProviderNamespace}/{resourceType}/providers/locks`

Bij het toepassen van een vergrendeling op een **resourcegroep** of **abonnement**, gebruikt u de volgende indelingen:

* naam: `{lockName}`
* type- `Microsoft.Authorization/locks`

Het volgende voorbeeld ziet een sjabloon die wordt gemaakt van een app service-plan, een website en een vergrendeling op de website. Het brontype van de vergrendeling is het brontype van de resource moet worden vergrendeld en **/providers/vergrendelingen**. De naam van de vergrendeling is gemaakt door de resourcenaam van de met **/Microsoft.Authorization/** en de naam van de vergrendeling.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "hostingPlanName": {
            "type": "string"
        }
    },
    "variables": {
        "siteName": "[concat('ExampleSite', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "apiVersion": "2016-09-01",
            "type": "Microsoft.Web/serverfarms",
            "name": "[parameters('hostingPlanName')]",
            "location": "[resourceGroup().location]",
            "sku": {
                "tier": "Free",
                "name": "f1",
                "capacity": 0
            },
            "properties": {
                "targetWorkerCount": 1
            }
        },
        {
            "apiVersion": "2016-08-01",
            "name": "[variables('siteName')]",
            "type": "Microsoft.Web/sites",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
            ],
            "properties": {
                "serverFarmId": "[parameters('hostingPlanName')]"
            }
        },
        {
            "type": "Microsoft.Web/sites/providers/locks",
            "apiVersion": "2016-09-01",
            "name": "[concat(variables('siteName'), '/Microsoft.Authorization/siteLock')]",
            "dependsOn": [
                "[resourceId('Microsoft.Web/sites', variables('siteName'))]"
            ],
            "properties": {
                "level": "CanNotDelete",
                "notes": "Site should not be deleted."
            }
        }
    ]
}
```

Zie voor een voorbeeld van het instellen van een vergrendeling op een resourcegroep, [een resourcegroep maken en een vergrendeling van het](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/create-rg-lock-role-assignment).

## <a name="powershell"></a>PowerShell
U vergrendeling geïmplementeerde resources met Azure PowerShell met behulp van de [New-AzResourceLock](/powershell/module/az.resources/new-azresourcelock) opdracht.

Als u wilt vergrendelen van een resource, geef de naam van de resource, het resourcetype en de naam van de resourcegroep.

```azurepowershell-interactive
New-AzResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

Als u wilt vergrendelen van een resourcegroep, geef de naam van de resourcegroep.

```azurepowershell-interactive
New-AzResourceLock -LockName LockGroup -LockLevel CanNotDelete -ResourceGroupName exampleresourcegroup
```

Voor informatie over een vergrendeling gebruikt [Get-AzureRmResourceLock](/powershell/module/az.resources/get-azresourcelock). Voor alle vergrendelingen in uw abonnement, gebruikt u:

```azurepowershell-interactive
Get-AzResourceLock
```

Voor alle vergrendelingen voor een resource, gebruikt u:

```azurepowershell-interactive
Get-AzResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

Voor alle vergrendelingen voor een resourcegroep, gebruikt u:

```azurepowershell-interactive
Get-AzResourceLock -ResourceGroupName exampleresourcegroup
```

Als u wilt een vergrendeling voor verwijderen, gebruikt u:

```azurepowershell-interactive
$lockId = (Get-AzResourceLock -ResourceGroupName exampleresourcegroup -ResourceName examplesite -ResourceType Microsoft.Web/sites).LockId
Remove-AzResourceLock -LockId $lockId
```

## <a name="azure-cli"></a>Azure-CLI

U vergrendeling geïmplementeerde resources met Azure CLI met behulp van de [az lock maken](/cli/azure/lock#az-lock-create) opdracht.

Als u wilt vergrendelen van een resource, geef de naam van de resource, het resourcetype en de naam van de resourcegroep.

```azurecli
az lock create --name LockSite --lock-type CanNotDelete --resource-group exampleresourcegroup --resource-name examplesite --resource-type Microsoft.Web/sites
```

Als u wilt vergrendelen van een resourcegroep, geef de naam van de resourcegroep.

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete --resource-group exampleresourcegroup
```

Voor informatie over een vergrendeling gebruikt [az lock list](/cli/azure/lock#az-lock-list). Voor alle vergrendelingen in uw abonnement, gebruikt u:

```azurecli
az lock list
```

Voor alle vergrendelingen voor een resource, gebruikt u:

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite --namespace Microsoft.Web --resource-type sites --parent ""
```

Voor alle vergrendelingen voor een resourcegroep, gebruikt u:

```azurecli
az lock list --resource-group exampleresourcegroup
```

Als u wilt een vergrendeling voor verwijderen, gebruikt u:

```azurecli
lockid=$(az lock show --name LockSite --resource-group exampleresourcegroup --resource-type Microsoft.Web/sites --resource-name examplesite --output tsv --query id)
az lock delete --ids $lockid
```

## <a name="rest-api"></a>REST-API
U kunt vergrendelen, geïmplementeerde resources met de [REST-API voor beheervergrendelingen](https://docs.microsoft.com/rest/api/resources/managementlocks). De REST-API kunt u maken en verwijderen van de vergrendelingen en gegevens worden opgehaald over bestaande vergrendelingen.

Voer het volgende voor het maken van een vergrendeling:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

Het bereik wordt mogelijk een abonnement, resourcegroep of resource. De naam van de vergrendeling is wat u wilt aanroepen van de vergrendeling. Gebruik voor api-versie, **2016-09-01**.

Opnemen in de aanvraag, een JSON-object dat Hiermee geeft u de eigenschappen van de vergrendeling.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het organiseren van uw resources logisch, [tags gebruiken om uw resources te organiseren](resource-group-using-tags.md)
* U kunt beperkingen en conventies toepassen voor uw abonnement met aangepast beleid. Zie [Wat is Azure Policy?](../governance/policy/overview.md) voor meer informatie.
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](/azure/architecture/cloud-adoption-guide/subscription-governance).

