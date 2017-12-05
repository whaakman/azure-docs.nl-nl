---
title: Quick Start - een persoonlijke Docker-register maken in Azure met Azure CLI
description: Snel informatie over het maken van een persoonlijke register van de Docker-container met de Azure CLI.
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: tysonn
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: azurecli
ms.topic: quicksart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/16/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 6804a3e6933a467fc57b960676ada800a9ed22a4
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="create-a-container-registry-using-the-azure-cli"></a>Een containerregister maken met de Azure-CLI

Azure Container Registry is een beheerde service voor Docker-containerregisters die wordt gebruikt voor het opslaan van installatiekopieën van persoonlijke Docker-containers. Deze handleiding gegevens maken van een Azure Container register-exemplaar met de Azure CLI.

Deze snelstartgids vereist dat u de Azure CLI versie 2.0.20 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli).

U moet ook Docker lokaal geïnstalleerd hebben. Docker biedt pakketten die eenvoudig Docker op elke configureren op elk [Mac](https://docs.docker.com/docker-for-mac/)-, [Windows](https://docs.docker.com/docker-for-windows/)- of [Linux](https://docs.docker.com/engine/installation/#supported-platforms)-systeem.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Een containerregister maken

In deze snelstartgids maken we een *Basic* register. Azure Container register is beschikbaar in diverse verschillende SKU's, een korte beschrijving van de volgende tabel. Zie voor uitgebreide informatie over elk [Container register SKU's](container-registry-skus.md).

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Maak een ACR-exemplaar met de opdracht [az acr create](/cli/azure/acr#create).

De naam van het register **moeten uniek zijn**. In het volgende voorbeeld *myContainerRegistry007* wordt gebruikt. Hiermee worden bijgewerkt naar een unieke waarde.

```azurecli
az acr create --name myContainerRegistry007 --resource-group myResourceGroup --sku Basic
```

Wanneer het register is gemaakt, is de uitvoer vergelijkbaar met het volgende:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-09-08T22:32:13.175925+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry007",
  "location": "eastus",
  "loginServer": "myContainerRegistry007.azurecr.io",
  "name": "myContainerRegistry007",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "mycontainerregistr223140"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

In de rest van deze snelstartgids, gebruiken we `<acrname>` als een tijdelijke aanduiding voor de naam van de container-register.

## <a name="log-in-to-acr"></a>Aanmelden bij ACR

Voordat u installatiekopieën van containers gaat pushen en pullen, moet u zich aanmelden bij het ACR-exemplaar. Gebruik hiervoor de opdracht [az acr login](/cli/azure/acr#login).

```azurecli
az acr login --name <acrname>
```

De opdracht retourneert een bericht dat de aanmelding is gelukt.

## <a name="push-image-to-acr"></a>Push-installatiekopie naar ACR

Voor het pushen van een afbeelding met een Azure-Container register, moet u eerst een afbeelding hebben. Indien nodig, voer de volgende opdracht voor het ophalen van een installatiekopie van een vooraf gemaakte van Docker-Hub.

```bash
docker pull microsoft/aci-helloworld
```

De afbeelding moet worden gemarkeerd met de naam van de ACR aanmelding-server. Voer de volgende opdracht om te retourneren van de aanmeldingsnaam van de server van het ACR-exemplaar.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Label van de installatiekopie met de [docker-tag](https://docs.docker.com/engine/reference/commandline/tag/) opdracht. Vervang  *<acrLoginServer>*  met de aanmeldingsnaam van de server van uw ACR-exemplaar.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Gebruik tot slot [docker push](https://docs.docker.com/engine/reference/commandline/push/) voor de push-installatiekopie van het naar het ACR-exemplaar. Vervang  *<acrLoginServer>*  met de aanmeldingsnaam van de server van uw ACR-exemplaar.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="list-container-images"></a>Containerinstallatiekopieën opvragen

Het volgende voorbeeld worden de opslagplaatsen in een register:

```azurecli
az acr repository list -n <acrname> -o table
```

Uitvoer:

```bash
Result
----------------
aci-helloworld
```

Het volgende voorbeeld bevat de labels van de **aci helloworld** opslagplaats.

```azurecli
az acr repository show-tags -n <acrname> --repository aci-helloworld -o table
```

Uitvoer:

```bash
Result
--------
v1
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, kunt u de [az groep verwijderen](/cli/azure/group#delete) opdracht om te verwijderen van de resourcegroep, ACR-exemplaar en alle installatiekopieën van de container.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids kunt u een Azure Container Registry gemaakt met de Azure CLI. Als u gebruiken van Azure Container register met exemplaren van Azure-Container wilt, blijven de zelfstudie exemplaren van Azure-Container.

> [!div class="nextstepaction"]
> [Zelfstudie voor Azure Containerexemplaren](../container-instances/container-instances-tutorial-prepare-app.md)