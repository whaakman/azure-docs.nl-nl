---
title: "Privé-Docker-containerregister maken - Azure CLI | Microsoft Docs"
description: "Aan de slag met het maken en beheren van privé-Docker-containerregisters met Azure CLI 2.0"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: c7cdb1b13bf32388d18c2a25af28337a81861c1e
ms.contentlocale: nl-nl
ms.lasthandoff: 07/12/2017

---

# <a name="create-a-managed-container-registry-using-the-azure-cli"></a>Een beheerd containerregister maken met de Azure CLI

Azure Container Registry is een beheerde service voor Docker-containerregisters die wordt gebruikt voor het opslaan van installatiekopieën van persoonlijke Docker-containers. In deze handleiding vindt u instructies voor het maken van een beheerd exemplaar van Azure Container Registry met behulp van de Azure CLI.

Beheerde Azure-containerregisters zijn nog in preview en zijn niet in alle regio's beschikbaar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze Quickstart gebruikmaken van Azure CLI versie 2.0.4 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *westcentralus*.

```azurecli-interactive 
az group create --name myResourceGroup --location westcentralus
```

## <a name="create-a-container-registry"></a>Een containerregister maken

Maak een ACR-exemplaar met de opdracht [az acr create](/cli/azure/acr#create).

> [!NOTE]
> Geef bij het maken van een register een unieke domeinnaam op het hoogste niveau op, bestaande uit alleen letters en cijfers.

 De registernaam in de voorbeelden is *myContainerRegistry1*, maar u moet deze vervangen door uw eigen unieke naam.

```azurecli
az acr create --name myContainerRegistry1 --resource-group myResourceGroup --sku Managed_Standard
```

Wanneer het register is gemaakt, is de uitvoer vergelijkbaar met het volgende:

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-29T04:50:28.607134+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry1",
  "location": "westcentralus",
  "loginServer": "mycontainerregistry1.azurecr.io",
  "name": "myContainerRegistry1",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Managed_Standard",
    "tier": "Managed"
  },
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

## <a name="log-in-to-acr-instance"></a>Aanmelden bij ACR-exemplaar

Voordat u installatiekopieën van containers gaat pushen en pullen, moet u zich aanmelden bij het ACR-exemplaar. Gebruik hiervoor de opdracht [az acr login](/cli/azure/acr#login).

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

De opdracht retourneert een bericht dat de aanmelding is gelukt.

## <a name="use-azure-container-registry"></a>Azure Container Registry gebruiken

### <a name="list-container-images"></a>Containerinstallatiekopieën opvragen

Gebruik de `az acr`-CLI-opdrachten om query's toe te passen op de installatiekopieën en tags in een opslagplaats.

> [!NOTE]
> Container Registry ondersteunt momenteel de opdracht `docker search` niet voor het toepassen van query's op installatiekopieën en tags.

### <a name="list-repositories"></a>Opslagplaatsen weergeven

In het volgende voorbeeld worden de opslagplaatsen in een register weergegeven in JSON-indeling (JavaScript Object Notation):

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a>Tags weergeven

In het volgende voorbeeld worden de tags in de opslagplaats **samples/nginx** weergegeven in JSON-indeling:

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een beheerd exemplaar van Azure Container Registry gemaakt met behulp van de Azure CLI.

> [!div class="nextstepaction"]
> [Uw eerste installatiekopie pushen met de Docker-CLI](container-registry-get-started-docker-cli.md)

