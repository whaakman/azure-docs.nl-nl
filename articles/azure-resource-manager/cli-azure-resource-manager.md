---
title: Resources beheren met de Azure CLI | Microsoft Docs
description: De Azure-opdrachtregelinterface (CLI) gebruiken voor het beheren van Azure-resources en groepen
editor: ''
manager: timlt
documentationcenter: ''
author: tfitzmac
services: azure-resource-manager
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: conceptual
ms.date: 10/06/2017
ms.author: tomfitz
ms.openlocfilehash: dd111c33cbd348a05ed0f0c04f7325347612e54d
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39259839"
---
# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>De Azure CLI gebruiken voor het beheren van Azure-resources en resourcegroepen

In dit artikel leert u hoe u voor het beheren van uw oplossingen met Azure CLI en Azure Resource Manager. Als u niet bekend bent met Resource Manager, Zie [overzicht van Resource Manager](resource-group-overview.md). In dit artikel is gericht op beheertaken. U gaat het volgende doen:

1. Een resourcegroep maken
2. Een resource toevoegen aan de resourcegroep
3. Een label toevoegen aan de resource
4. Query uitvoeren op basis van namen of tagwaarden-resources
5. Toepassen en verwijderen van een vergrendeling op de resource
6. Een resourcegroep verwijderen

In dit artikel wordt niet weergegeven voor het implementeren van een Resource Manager-sjabloon aan uw abonnement. Voor die informatie, Zie [resources implementeren met Resource Manager-sjablonen en Azure CLI](resource-group-template-deploy-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en de CLI lokaal gebruikt, Zie [Azure CLI installeren](/cli/azure/install-azure-cli).

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

U ziet dat er één abonnement is gemarkeerd als standaardwaarde. De huidige context voor bewerkingen van dit abonnement is. Als u wilt overschakelen naar een ander abonnement, bieden u de naam van het abonnement met de **az account set** opdracht.

```azurecli-interactive
az account set -s "Example Subscription Two"
```

Als u wilt weergeven van de huidige context van het abonnement, gebruikt u **az account show** zonder een parameter:

```azurecli-interactive
az account show
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Voordat u implementeert alle resources die aan uw abonnement, moet u een resourcegroep met de resources maken.

U kunt een resourcegroep maken met de opdracht **az group create**. De opdracht maakt gebruik van de **naam** parameter opgeven voor een naam op voor de resourcegroep en de **locatie** parameter opgeven voor de locatie.

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

Als u ophalen van de resourcegroep later wilt, gebruikt u de volgende opdracht uit:

```azurecli-interactive
az group show --name TestRG1
```

Voor alle resourcegroepen in uw abonnement, gebruikt u:

```azurecli-interactive
az group list
```

## <a name="add-resources-to-a-resource-group"></a>Resources toevoegen aan een resourcegroep

Als u wilt een resource toevoegen aan de resourcegroep, kunt u de **az resource maken** opdracht of een opdracht die specifiek is voor het type resource dat u maakt (zoals **az storage-account maken**). Wellicht eenvoudiger te gebruiken van een opdracht die specifiek is voor een resourcetype omdat deze parameters voor de eigenschappen die nodig zijn voor de nieuwe resource bevat. Gebruik **az resource maken**, moet u de eigenschappen instellen zonder te worden gevraagd voor hen weten.

Echter, toevoegen van een resource via script kan verwarring toekomstige omdat de nieuwe resource niet in een Resource Manager-sjabloon bestaat. Sjablonen kunnen u uw oplossing betrouwbaar en herhaaldelijk te implementeren.

De volgende opdracht maakt u een opslagaccount. In plaats van met de naam weergegeven in het voorbeeld, Geef een unieke naam voor het opslagaccount. De naam moet tussen 3 en 24 tekens lang zijn en alleen cijfers en kleine letters. Als u de naam die wordt weergegeven in het voorbeeld gebruiken, ontvangt u een fout opgetreden omdat deze naam al gebruikt wordt.

```azurecli-interactive
az storage account create -n myuniquestorage -g TestRG1 -l westus --sku Standard_LRS
```

Als u deze resource later ophalen wilt, gebruikt u de volgende opdracht uit:

```azurecli-interactive
az storage account show --name myuniquestorage --resource-group TestRG1
```

## <a name="add-a-tag"></a>Een label toevoegen

Met tags kunt u uw resources op basis van verschillende eigenschappen te organiseren. Bijvoorbeeld, kunnen er verschillende resources in verschillende resourcegroepen die deel uitmaken van dezelfde afdeling. U kunt een afdeling tag en waarde toepassen op deze resources om ze te markeren als behorend tot de dezelfde categorie. Of u kunt markeren of een resource wordt gebruikt in een productie- of testomgeving. In dit artikel hebt u tags toepassen op slechts één bron, maar in uw omgeving het meest waarschijnlijke verstandig om tags toepassen op al uw resources.

De volgende opdracht is van toepassing twee labels op uw storage-account:

```azurecli-interactive
az resource tag --tags Dept=IT Environment=Test -g TestRG1 -n myuniquestorage --resource-type "Microsoft.Storage/storageAccounts"
```

Labels worden bijgewerkt als een enkel object. Als u wilt een code toevoegen aan een resource die reeds labels bevat, moet u eerst de bestaande tags ophalen. De nieuwe tag toevoegen aan het object met de bestaande tags en alle tags toepassen op de resource.

```azurecli-interactive
jsonrtag=$(az resource show -g TestRG1 -n myuniquestorage --resource-type "Microsoft.Storage/storageAccounts" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g TestRG1 -n myuniquestorage --resource-type "Microsoft.Storage/storageAccounts"
```

## <a name="search-for-resources"></a>Zoeken naar resources

Gebruik de **az resource list** opdracht om op te halen van resources voor verschillende zoekcriteria.

* Als u een resource met de naam, bieden de **naam** parameter:

  ```azurecli-interactive
  az resource list -n myuniquestorage
  ```

* Als u alle resources in een resourcegroep, bieden de **resourcegroep** parameter:

  ```azurecli-interactive
  az resource list --resource-group TestRG1
  ```

* Als u alle resources met een naam van de tag en waarde, bieden de **tag** parameter:

  ```azurecli-interactive
  az resource list --tag Dept=IT
  ```

* Op alle resources met een bepaald resourcetype, bieden de **resourcetype** parameter:

  ```azurecli-interactive
  az resource list --resource-type "Microsoft.Storage/storageAccounts"
  ```

## <a name="get-resource-id"></a>Resource-ID ophalen

Veel opdrachten worden een resource-ID als parameter. Voor de ID voor een resource en opslaan in een variabele, gebruikt u:

```azurecli-interactive
webappID=$(az resource show -g exampleGroup -n exampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
```

## <a name="lock-a-resource"></a>Een resource vergrendelen

Wanneer u nodig hebt om te controleren of een essentiële bron niet per ongeluk is verwijderd of gewijzigd, moet u een vergrendeling toepassen naar de resource. U kunt opgeven of een **CanNotDelete** of **ReadOnly**.

Als u wilt maken of verwijderen van beheervergrendelingen, u moet toegang hebben tot `Microsoft.Authorization/*` of `Microsoft.Authorization/locks/*` acties. Van de ingebouwde rollen, worden alleen de eigenaar en beheerder van gebruikerstoegang personen deze acties verleend.

Als u wilt een vergrendeling toepassen, moet u de volgende opdracht gebruiken:

```azurecli-interactive
az lock create --lock-type CanNotDelete --resource-name myuniquestorage --resource-group TestRG1 --resource-type Microsoft.Storage/storageAccounts --name storagelock
```

De vergrendelde resource in het vorige voorbeeld kan niet worden verwijderd, totdat de vergrendeling is verwijderd. Gebruik het volgende te verwijderen van een vergrendeling:

```azurecli-interactive
az lock delete --name storagelock --resource-group TestRG1 --resource-type Microsoft.Storage/storageAccounts --resource-name myuniquestorage
```

Zie voor meer informatie over de instelling wordt vergrendeld, [resources met Azure Resource Manager vergrendelen](resource-group-lock-resources.md).

## <a name="remove-resources-or-resource-group"></a>Resources of resourcegroep verwijderen
U kunt een resource of resourcegroep verwijderen. Wanneer u een resourcegroep verwijdert, verwijdert u ook alle resources in die resourcegroep.

* Als een resource verwijderen uit de resourcegroep, gebruikt u de opdracht voor het brontype dat u wilt verwijderen. De opdracht de resource wordt verwijderd, maar Verwijder de resourcegroep niet.

  ```azurecli-interactive
  az storage account delete -n myuniquestorage -g TestRG1
  ```

* Voer de opdracht **az group delete** uit om de resourcegroep en alle bijbehorende resources te verwijderen.

  ```azurecli-interactive
  az group delete -n TestRG1
  ```

Voor beide opdrachten, wordt u gevraagd om te bevestigen dat u wilt verwijderen van de resource of resourcegroep.

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het maken van Resource Manager-sjablonen, [Azure Resource Manager-sjablonen ontwerpen](resource-group-authoring-templates.md).
* Zie voor meer informatie over het implementeren van sjablonen, [een toepassing implementeren met Azure Resource Manager-sjabloon](resource-group-template-deploy-cli.md).
* U kunt bestaande resources verplaatsen naar een nieuwe resourcegroep. Zie voor voorbeelden van [verplaatsen van Resources aan de nieuwe resourcegroep of abonnement](resource-group-move-resources.md).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](/azure/architecture/cloud-adoption-guide/subscription-governance).