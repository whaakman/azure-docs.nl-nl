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
ms.date: 06/27/2017
ms.author: tomfitz
ms.openlocfilehash: 44c87b00f4fc63dbfd45a07d9a8cddc5eaf1a65c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
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
Het volgende voorbeeld ziet een sjabloon die wordt gemaakt van een vergrendeling op een opslagaccount. Het opslagaccount op waarop u wilt toepassen van de vergrendeling is opgegeven als parameter. De naam van de vergrendeling wordt gemaakt door de naam van de resource met cookievalidatie **/Microsoft.Authorization/** en de naam van de vergrendeling, in dit geval **myLock**.

Het opgegeven type is specifiek voor het brontype. Voor opslag, stelt u het type 'Microsoft.Storage/storageaccounts/providers/locks'.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="powershell"></a>PowerShell
U vergrendeling resources met Azure PowerShell geïmplementeerd met behulp van de [nieuw AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) opdracht.

Als u wilt vergrendelen van een resource, geef de naam van de bron, het resourcetype en de naam van de resourcegroep.

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite `
  -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

Als u wilt vergrendelen van een resourcegroep, geef de naam van de resourcegroep.

```powershell
New-AzureRmResourceLock -LockName LockGroup -LockLevel CanNotDelete `
  -ResourceGroupName exampleresourcegroup
```

Als u informatie over een vergrendeling, gebruikt [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock). Als u alle vergrendelingen in uw abonnement, gebruikt u:

```powershell
Get-AzureRmResourceLock
```

Als u alle vergrendelingen voor een resource, gebruikt u:

```powershell
Get-AzureRmResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

Als u alle vergrendelingen voor een resourcegroep, gebruikt u:

```powershell
Get-AzureRmResourceLock -ResourceGroupName exampleresourcegroup
```

Azure PowerShell biedt andere opdrachten voor vergrendelingen werken, zoals [Set AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) bijwerken van een vergrendeling en [verwijderen AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) een vergrendeling te verwijderen.

## <a name="azure-cli"></a>Azure CLI

U vergrendeling resources met Azure CLI geïmplementeerd met behulp van de [az vergrendeling maken](/cli/azure/lock#create) opdracht.

Als u wilt vergrendelen van een resource, geef de naam van de bron, het resourcetype en de naam van de resourcegroep.

```azurecli
az lock create --name LockSite --lock-type CanNotDelete \
  --resource-group exampleresourcegroup --resource-name examplesite \
  --resource-type Microsoft.Web/sites
```

Als u wilt vergrendelen van een resourcegroep, geef de naam van de resourcegroep.

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete \
  --resource-group exampleresourcegroup
```

Als u informatie over een vergrendeling, gebruikt [az vergrendeling lijst](/cli/azure/lock#list). Als u alle vergrendelingen in uw abonnement, gebruikt u:

```azurecli
az lock list
```

Als u alle vergrendelingen voor een resource, gebruikt u:

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite \
  --namespace Microsoft.Web --resource-type sites --parent ""
```

Als u alle vergrendelingen voor een resourcegroep, gebruikt u:

```azurecli
az lock list --resource-group exampleresourcegroup
```

Azure CLI biedt andere opdrachten voor vergrendelingen werken, zoals [az vergrendeling update](/cli/azure/lock#update) bijwerken van een vergrendeling en [az vergrendeling verwijderen](/cli/azure/lock#delete) een vergrendeling te verwijderen.

## <a name="rest-api"></a>REST API
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
* Zie voor meer informatie over het werken met resource vergrendelingen [vergrendeling omlaag Your Azure Resources](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
* Zie voor meer informatie over het logisch ordenen van uw resources [met labels om uw resources te organiseren](resource-group-using-tags.md)
* Als u wilt wijzigen welke resourcegroep een resource bevindt zich in, Zie [resources verplaatsen naar een nieuwe resourcegroep](resource-group-move-resources.md)
* U kunt beperkingen en conventies toepassen voor uw abonnement met aangepast beleid. Zie voor meer informatie [Beleid gebruiken voor het beheren van resources en toegang](resource-manager-policy.md).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](resource-manager-subscription-governance.md).

