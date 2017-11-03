---
title: Resources beheren met de Azure CLI | Microsoft Docs
description: De Azure-opdrachtregelinterface (CLI) gebruiken voor het beheren van Azure-resources en groepen
editor: 
manager: timlt
documentationcenter: 
author: tfitzmac
services: azure-resource-manager
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 10/06/2017
ms.author: tomfitz
ms.openlocfilehash: c68f2a8b6e18dc2d51d8bbb5cd05bc037dc2fadb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>De Azure CLI gebruiken voor het beheren van Azure-resources en resourcegroepen

In dit artikel leert u hoe u uw oplossingen met Azure CLI en Azure Resource Manager beheert. Als u niet bekend met Resource Manager, Zie [overzicht van Resource Manager](resource-group-overview.md). Dit artikel is gericht op beheertaken. U gaat het volgende doen:

1. Een resourcegroep maken
2. Een resource toevoegen aan de resourcegroep
3. Een label toevoegen aan de bron
4. Een query uitvoeren op basis van namen of labelwaarden resources
5. Toepassen en verwijderen van een vergrendeling op de bron
6. Verwijderen van een resourcegroep

In dit artikel wordt niet weergegeven voor het implementeren van een Resource Manager-sjabloon aan uw abonnement. Zie voor die informatie [implementeren van resources met Resource Manager-sjablonen en Azure CLI](resource-group-template-deploy-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, Zie [2.0 voor Azure CLI installeren](/cli/azure/install-azure-cli).

## <a name="set-subscription"></a>Set-abonnement

Als u meer dan één abonnement hebt, kunt u overschakelen naar een ander abonnement. Eerst gaan we kijken alle abonnementen voor uw account.

```azurecli-interactive
az account list
```

Deze retourneert een lijst van uw abonnementen uitgeschakeld en ingeschakeld.

```json
[
  {
    "cloudName": "AzureCloud",
    "id": "<guid>",
    "isDefault": true,
    "name": "Example Subscription One",
    "registeredProviders": [],
    "state": "Enabled",
    "tenantId": "<guid>",
    "user": {
      "name": "example@contoso.org",
      "type": "user"
    }
  },
  ...
]
```

U ziet dat er één abonnement is gemarkeerd als standaardwaarde. Dit abonnement is de huidige context voor bewerkingen. Als u wilt overschakelen naar een ander abonnement, geef de naam van het abonnement met de **az account set** opdracht.

```azurecli-interactive
az account set -s "Example Subscription Two"
```

Als de context van het huidige abonnement, gebruiken **az account weergeven** zonder parameters:

```azurecli-interactive
az account show
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Voordat u implementeert geen bronnen aan uw abonnement, moet u een resourcegroep met de bronnen.

Voor het maken van een resourcegroep gebruikt de **az groep maken** opdracht. Gebruikt de opdracht de **naam** parameter om een naam voor de resourcegroep en de **locatie** parameter om de locatie.

```azurecli-interactive
az group create --name TestRG1 --location "South Central US"
```

De uitvoer is in de volgende indeling:

```json
{
  "id": "/subscriptions/<subscription-id>/resourceGroups/TestRG1",
  "location": "southcentralus",
  "managedBy": null,
  "name": "TestRG1",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

Als u de resourcegroep later nodig, gebruikt u de volgende opdracht:

```azurecli-interactive
az group show --name TestRG1
```

Als u alle resourcegroepen in uw abonnement, gebruikt u:

```azurecli-interactive
az group list
```

## <a name="add-resources-to-a-resource-group"></a>Resources toevoegen aan een resourcegroep

Als u wilt een resource toevoegen aan de resourcegroep, kunt u de **az resource maken** opdracht of een opdracht die specifiek is voor het type resource dat u wilt maken (zoals **az storage-account maken**). Wellicht eenvoudiger te gebruiken van een opdracht die specifiek is voor een resourcetype omdat deze parameters voor de eigenschappen die nodig zijn voor de nieuwe resource bevat. Gebruik **az resource maken**, moet u de eigenschappen in te stellen zonder te worden gevraagd voor hen kennen.

Echter, toevoegen van een resource via script kan veroorzaken toekomstige verwarring omdat de nieuwe resource niet in een Resource Manager-sjabloon bestaat. Sjablonen kunnen u op betrouwbare wijze en herhaaldelijk implementeren van uw oplossing.

De volgende opdracht maakt een opslagaccount. In plaats van de naam die wordt weergegeven in het voorbeeld, Geef een unieke naam voor het opslagaccount. De naam moet tussen 3 en 24 tekens lang zijn en alleen cijfers en kleine letters gebruiken. Als u de naam die wordt weergegeven in het voorbeeld gebruikt, ontvangt u een fout opgetreden omdat deze naam al gebruikt wordt.

```azurecli-interactive
az storage account create -n myuniquestorage -g TestRG1 -l westus --sku Standard_LRS
```

Als u deze bron later nodig, gebruikt u de volgende opdracht:

```azurecli-interactive
az storage account show --name myuniquestorage --resource-group TestRG1
```

## <a name="add-a-tag"></a>Een label toevoegen

Labels kunnen u om uw resources op basis van andere eigenschappen te organiseren. Bijvoorbeeld wellicht verschillende resources in verschillende resourcegroepen die deel uitmaken van dezelfde afdeling. U kunt een label voor afdeling en de waarde toepassen op deze resources om deze te markeren als onderdeel van dezelfde categorie. Of u kunt markeren of een resource wordt gebruikt in een productie- of testomgeving. In dit artikel leert u tags toepassen op slechts één resource, maar in uw omgeving is het waarschijnlijk wel zinvol tags toepassen op al uw resources.

De volgende opdracht geldt twee tags voor uw opslagaccount:

```azurecli-interactive
az resource tag --tags Dept=IT Environment=Test -g TestRG1 -n myuniquestorage --resource-type "Microsoft.Storage/storageAccounts"
```

Labels zijn als een enkel object bijgewerkt. Als u wilt een code toevoegt aan een resource die al tags bevat, moet u eerst de bestaande labels ophalen. Nieuw label toevoegen aan het object dat de bestaande labels bevat en alle tags toepassen op de resource.

```azurecli-interactive
jsonrtag=$(az resource show -g TestRG1 -n myuniquestorage --resource-type "Microsoft.Storage/storageAccounts" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g TestRG1 -n myuniquestorage --resource-type "Microsoft.Storage/storageAccounts"
```

## <a name="search-for-resources"></a>Zoeken naar resources

Gebruik de **az resourcelijst** opdracht voor het ophalen van bronnen voor verschillende zoekcriteria.

* Als u een resource met de naam, bieden de **naam** parameter:

  ```azurecli-interactive
  az resource list -n myuniquestorage
  ```

* Als u alle resources in een resourcegroep, bieden de **resourcegroep** parameter:

  ```azurecli-interactive
  az resource list --resource-group TestRG1
  ```

* Als u de resources met een naam en waarde, bieden de **tag** parameter:

  ```azurecli-interactive
  az resource list --tag Dept=IT
  ```

* Alle resources met een bepaald resourcetype, geeft u de **type resource** parameter:

  ```azurecli-interactive
  az resource list --resource-type "Microsoft.Storage/storageAccounts"
  ```

## <a name="get-resource-id"></a>Resource-ID ophalen

Veel opdrachten nemen een resource-ID als een parameter. Als u de ID voor een resource die en opslaan in een variabele, gebruikt u:

```azurecli-interactive
webappID=$(az resource show -g exampleGroup -n exampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
```

## <a name="lock-a-resource"></a>Vergrendelen van een resource

Als u controleren of een essentiële resource niet per ongeluk is verwijderd of gewijzigd wilt, moet u een vergrendeling toegepast op de resource. U kunt opgeven dat ofwel een **CanNotDelete** of **ReadOnly**.

Als u wilt maken of verwijderen van management vergrendelingen, u moet toegang hebben tot `Microsoft.Authorization/*` of `Microsoft.Authorization/locks/*` acties. Van de ingebouwde rollen worden alleen de eigenaar en de beheerder voor gebruikerstoegang die acties verleend.

Als u wilt toepassen op een vergrendeling, moet u de volgende opdracht gebruiken:

```azurecli-interactive
az lock create --lock-type CanNotDelete --resource-name myuniquestorage --resource-group TestRG1 --resource-type Microsoft.Storage/storageAccounts --name storagelock
```

De vergrendelde resource in het vorige voorbeeld kan niet worden verwijderd, totdat de vergrendeling wordt verwijderd. Gebruik voor het verwijderen van een vergrendeling:

```azurecli-interactive
az lock delete --name storagelock --resource-group TestRG1 --resource-type Microsoft.Storage/storageAccounts --resource-name myuniquestorage
```

Zie voor meer informatie over de instelling vergrendelingen [resources met Azure Resource Manager vergrendelen](resource-group-lock-resources.md).

## <a name="remove-resources-or-resource-group"></a>Verwijder resources of resourcegroep
U kunt een resource of resourcegroep verwijderen. Wanneer u een resourcegroep verwijdert, verwijdert u ook alle resources in die resourcegroep.

* Als u wilt een bron verwijdert uit de resourcegroep, gebruikt u de opdracht delete voor het brontype dat u wilt verwijderen. De opdracht de resource wordt verwijderd, maar de resourcegroep niet verwijdert.

  ```azurecli-interactive
  az storage account delete -n myuniquestorage -g TestRG1
  ```

* U een resourcegroep en de bijhorende resources verwijderen met de **az groep verwijderen** opdracht.

  ```azurecli-interactive
  az group delete -n TestRG1
  ```

Voor beide opdrachten, wordt u gevraagd te bevestigen dat u wilt verwijderen van de resource of resourcegroep.

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het maken van Resource Manager-sjablonen, [Azure Resource Manager-sjablonen ontwerpen](resource-group-authoring-templates.md).
* Zie voor meer informatie over het implementeren van sjablonen, [Implementeer een toepassing met Azure Resource Manager-sjabloon](resource-group-template-deploy-cli.md).
* U kunt bestaande resources verplaatsen naar een nieuwe resourcegroep. Zie voor voorbeelden [Resources verplaatsen naar de nieuwe resourcegroep of abonnement](resource-group-move-resources.md).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](resource-manager-subscription-governance.md).