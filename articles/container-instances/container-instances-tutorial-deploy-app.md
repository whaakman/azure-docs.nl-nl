---
title: Zelfstudie voor Azure Containerexemplaren - app implementeren
description: Zelfstudie voor Azure Containerexemplaren - app implementeren
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: mmacy
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 2858f20cd9da469d5983e2bef9176f5922349196
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="deploy-a-container-to-azure-container-instances"></a>Een container implementeren naar Azure Containerexemplaren

Dit is de laatste van een zelfstudie drie delen. In vorige gedeelten [is gemaakt met een installatiekopie van een container](container-instances-tutorial-prepare-app.md) en [naar een Azure Container Registry gepusht](container-instances-tutorial-prepare-acr.md). Deze sectie is de zelfstudie voltooid door het implementeren van de container op exemplaren van Azure-Container. Stappen voltooid omvatten:

> [!div class="checklist"]
> * Implementatie van de container van het Azure-Container register met de Azure CLI
> * De toepassing bekijken in de browser
> * De container logboeken bekijken

## <a name="before-you-begin"></a>Voordat u begint

Deze zelfstudie vereist dat u de Azure CLI versie 2.0.20 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli).

Voor deze zelfstudie hebt u een Docker-ontwikkelomgeving nodig. Docker biedt pakketten die eenvoudig Docker op elke configureren op elk [Mac](https://docs.docker.com/docker-for-mac/)-, [Windows](https://docs.docker.com/docker-for-windows/)- of [Linux](https://docs.docker.com/engine/installation/#supported-platforms)-systeem.

Azure Cloud-Shell is dan de Docker-onderdelen die nodig zijn voor het voltooien van elke stap in deze zelfstudie niet opgenomen. Daarom raden we aan een lokale installatie van de Azure CLI en Docker-ontwikkelomgeving.

## <a name="deploy-the-container-using-the-azure-cli"></a>De container met de Azure CLI implementeren

De Azure CLI kunt de implementatie van een container voor exemplaren van Azure-Container in één opdracht. Omdat de container-installatiekopie wordt gehost in het persoonlijke Azure-Container register, moet u de referenties voor toegang tot dit opnemen. Indien nodig, kunt u ze opvragen, zoals hieronder wordt weergegeven.

Container register login-server (update met de registernaam van uw):

```azurecli
az acr show --name <acrName> --query loginServer
```

Container register wachtwoord:

```azurecli
az acr credential show --name <acrName> --query "passwords[0].value"
```

Voer de volgende opdracht voor het implementeren van uw installatiekopie container uit het register van de container met een resource-aanvraag van het CPU-kern 1 en 1 GB geheugen. Vervang `<acrLoginServer>` en `<acrPassword>` met de waarden die u hebt ontvangen van de vorige twee opdrachten.

```azurecli
az container create --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public -g myResourceGroup
```

Binnen enkele seconden, moet u een initiële reactie ontvangen van Azure Resource Manager. U kunt de status van de implementatie weergeven, met [az container weergeven](/cli/azure/container#az_container_show):

```azurecli
az container show --name aci-tutorial-app --resource-group myResourceGroup --query instanceView.state
```

Herhaal de `az container show` opdracht totdat de status van verandert *in behandeling* naar *met*, die minder dan een minuut duren. Wanneer de container is *met*, gaat u verder met de volgende stap.

## <a name="view-the-application-and-container-logs"></a>Bekijk de logboeken toepassingen en container

Zodra de implementatie is geslaagd, weergegeven van de container openbaar IP-adres met de [az container weergeven](/cli/azure/container#az_container_show) opdracht:

```bash
az container show --name aci-tutorial-app --resource-group myResourceGroup --query ipAddress.ip
```

Voorbeeld van uitvoer:`"13.88.176.27"`

Overzicht van de actieve toepassing, gaat u naar het openbare IP-adres in uw favoriete browser.

![Hallo wereld-app in de browser][aci-app-browser]

U kunt ook de uitvoer van de container bekijken:

```azurecli
az container logs --name aci-tutorial-app -g myResourceGroup
```

Uitvoer:

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u niet langer de bronnen die u in deze zelfstudie reeks hebt gemaakt, kunt u uitvoeren de [az groep verwijderen](/cli/azure/group#delete) opdracht om te verwijderen van de resourcegroep en alle resources die deze bevat. Deze opdracht verwijdert u de container-registersleutel die u hebt gemaakt, evenals de actieve container en alle gerelateerde resources.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt voltooid u het proces voor het implementeren van uw containers op Azure Containerexemplaren. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * Implementatie van de container van het Azure-Container register met de Azure CLI
> * De toepassing bekijken in de browser
> * De container logboeken bekijken

<!-- LINKS -->
[prepare-app]: ./container-instances-tutorial-prepare-app.md

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
