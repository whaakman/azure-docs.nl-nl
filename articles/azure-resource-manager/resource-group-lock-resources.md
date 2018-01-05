---
title: Azure-resources om te voorkomen dat wijzigingen vergrendelen | Microsoft Docs
description: "Voorkomen dat gebruikers bijwerken of verwijderen van essentiële Azure-resources door het toepassen van een vergrendeling voor alle gebruikers en rollen."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 53c57e8f-741c-4026-80e0-f4c02638c98b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: tomfitz
ms.openlocfilehash: e25de0366126ceee988eb253b66d18c9b8b62e1f
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="lock-resources-to-prevent-unexpected-changes"></a>Resources om te voorkomen dat onverwachte wijzigingen vergrendelen 

Als beheerder, moet u wellicht een abonnement, resourcegroep of resource om te voorkomen dat andere gebruikers in uw organisatie per ongeluk verwijderen of wijzigen van kritieke bronnen vergrendelen. U kunt de vergrendeling op instellen **CanNotDelete** of **ReadOnly**. 

* **CanNotDelete** betekent geautoriseerde gebruikers kunnen nog steeds lezen en wijzigen van een resource, maar de resource kan niet worden verwijderd. 
* **Alleen-lezen** betekent geautoriseerde gebruikers kunnen een resource lezen, maar ze niet verwijderen of bijwerken van de resource. Toepassen van deze vergrendeling is vergelijkbaar met alle gemachtigde gebruikers beperken tot de machtigingen verleend door de **lezer** rol. 

## <a name="how-locks-are-applied"></a>Hoe vergrendelingen worden toegepast

Wanneer u een vergrendeling op een bovenliggend bereik toepast, nemen alle resources binnen dat bereik de dezelfde vergrendeling. Zelfs resources die u later toevoegen overnemen de vergrendeling van het bovenliggende item. De meest beperkende vergrendeling in de overname voorrang.

In tegenstelling tot rollen gebaseerd toegangsbeheer kunt u management vergrendelingen toepassen van een beperking voor alle gebruikers en rollen. Zie voor meer informatie over het instellen van machtigingen voor gebruikers en rollen [toegangsbeheer op basis van rollen in Azure](../active-directory/role-based-access-control-configure.md).

Vergrendelingen van Resource Manager alleen van toepassing op bewerkingen die in de vlak management, die uit de bewerkingen die worden verzonden optreden bestaat naar `https://management.azure.com`. De vergrendelingen beperken niet hoe resources hun eigen functies uitvoeren. Wijzigingen in de resourcedefinitie zijn beperkt, maar de bewerkingen van resources zijn niet beperkt. Bijvoorbeeld, een vergrendeling van het kenmerk alleen-lezen voor een SQL-Database wordt voorkomen dat u de database wijzigen of verwijderen, maar dit voorkomt niet dat u maken, bijwerken of verwijderen van gegevens in de database. Gegevenstransacties worden toegestaan, omdat deze bewerkingen niet worden verzonden naar `https://management.azure.com`.

Toepassen van **ReadOnly** kan leiden tot onverwachte resultaten optreden omdat bepaalde bewerkingen die lijkt lezen bewerkingen daadwerkelijk extra acties vereist. Bijvoorbeeld, als een **ReadOnly** vergrendeling van een opslagaccount wordt voorkomen dat alle gebruikers weergeven van de sleutels. De lijst met sleutels opnieuw wordt verwerkt door een POST-aanvraag omdat de geretourneerde sleutels zijn beschikbaar voor schrijfbewerkingen. Voor een ander voorbeeld plaatsen van een **ReadOnly** vergrendelen op een App Service-bron wordt voorkomen dat Visual Studio Server Explorer-bestanden voor de resource worden weergegeven omdat die interactie voor toegang voor schrijven vereist.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Wie kunt maken of verwijderen van vergrendelingen in uw organisatie
Als u wilt maken of verwijderen van management vergrendelingen, u moet toegang hebben tot `Microsoft.Authorization/*` of `Microsoft.Authorization/locks/*` acties. Van de ingebouwde rollen alleen **eigenaar** en **beheerder voor gebruikerstoegang** deze acties worden verleend.

## <a name="portal"></a>Portal
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a>Template
Het volgende voorbeeld ziet een sjabloon die wordt gemaakt van een app service-plan, een website en een vergrendeling op de website. Het brontype van de vergrendeling is het brontype van de resource te vergrendelen en **providers/vergrendelingen**. De naam van de vergrendeling wordt gemaakt door de naam van de resource met cookievalidatie **/Microsoft.Authorization/** en de naam van de vergrendeling.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

Voor het implementeren van deze voorbeeldsjabloon met PowerShell gebruiken:

```powershell
New-AzureRmResourceGroup -Name sitegroup -Location southcentralus
New-AzureRmResourceGroupDeployment -ResourceGroupName sitegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/lock.json -hostingPlanName plan0103
```

Voor het implementeren van deze voorbeeldsjabloon met Azure CLI gebruiken:

```azurecli
az group create --name sitegroup --location southcentralus
az group deployment create --resource-group sitegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/lock.json --parameters hostingPlanName=plan0103
```

## <a name="powershell"></a>PowerShell
U vergrendeling resources met Azure PowerShell geïmplementeerd met behulp van de [nieuw AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) opdracht.

Als u wilt vergrendelen van een resource, geef de naam van de bron, het resourcetype en de naam van de resourcegroep.

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

Als u wilt vergrendelen van een resourcegroep, geef de naam van de resourcegroep.

```powershell
New-AzureRmResourceLock -LockName LockGroup -LockLevel CanNotDelete -ResourceGroupName exampleresourcegroup
```

Als u informatie over een vergrendeling, gebruikt [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock). Als u alle vergrendelingen in uw abonnement, gebruikt u:

```powershell
Get-AzureRmResourceLock
```

Als u alle vergrendelingen voor een resource, gebruikt u:

```powershell
Get-AzureRmResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

Als u alle vergrendelingen voor een resourcegroep, gebruikt u:

```powershell
Get-AzureRmResourceLock -ResourceGroupName exampleresourcegroup
```

Gebruik voor het verwijderen van een vergrendeling:

```powershell
$lockId = (Get-AzureRmResourceLock -ResourceGroupName exampleresourcegroup -ResourceName examplesite -ResourceType Microsoft.Web/sites).LockId
Remove-AzureRmResourceLock -LockId $lockId
```

## <a name="azure-cli"></a>Azure-CLI

U vergrendeling resources met Azure CLI geïmplementeerd met behulp van de [az vergrendeling maken](/cli/azure/lock#create) opdracht.

Als u wilt vergrendelen van een resource, geef de naam van de bron, het resourcetype en de naam van de resourcegroep.

```azurecli
az lock create --name LockSite --lock-type CanNotDelete --resource-group exampleresourcegroup --resource-name examplesite --resource-type Microsoft.Web/sites
```

Als u wilt vergrendelen van een resourcegroep, geef de naam van de resourcegroep.

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete --resource-group exampleresourcegroup
```

Als u informatie over een vergrendeling, gebruikt [az vergrendeling lijst](/cli/azure/lock#list). Als u alle vergrendelingen in uw abonnement, gebruikt u:

```azurecli
az lock list
```

Als u alle vergrendelingen voor een resource, gebruikt u:

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite --namespace Microsoft.Web --resource-type sites --parent ""
```

Als u alle vergrendelingen voor een resourcegroep, gebruikt u:

```azurecli
az lock list --resource-group exampleresourcegroup
```

Gebruik voor het verwijderen van een vergrendeling:

```azurecli
lockid=$(az lock show --name LockSite --resource-group exampleresourcegroup --resource-type Microsoft.Web/sites --resource-name examplesite --output tsv --query id)
az lock delete --ids $lockid
```

## <a name="rest-api"></a>REST-API
U kunt vergrendelen geïmplementeerde resources met de [REST-API voor beheer van vergrendelingen](https://docs.microsoft.com/rest/api/resources/managementlocks). De REST-API kunt u maken en verwijderen van vergrendelingen en informatie ophalen over bestaande vergrendelingen.

Voer voor het maken van een vergrendeling:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

Het bereik kan worden voor een abonnement, resourcegroep of resource. De naam van de vergrendeling is wat u wilt aanroepen van de vergrendeling. Gebruik voor de api-versie **01-01-2015**.

Opnemen in de aanvraag een JSON-object waarmee de eigenschappen voor de vergrendeling.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het logisch ordenen van uw resources [met labels om uw resources te organiseren](resource-group-using-tags.md)
* Als u wilt wijzigen welke resourcegroep een resource bevindt zich in, Zie [resources verplaatsen naar een nieuwe resourcegroep](resource-group-move-resources.md)
* U kunt beperkingen en conventies toepassen voor uw abonnement met aangepast beleid. Zie [Wat is Azure Policy?](../azure-policy/azure-policy-introduction.md) voor meer informatie.
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](resource-manager-subscription-governance.md).

