---
title: Zelfstudie voor Azure Containerexemplaren - app implementeren
description: Zelfstudie voor Azure Containerexemplaren deel 3 van 3 - toepassing implementeren
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 471caa1b24dc7017c70782c072b2068f9635244b
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="deploy-a-container-to-azure-container-instances"></a>Een container implementeren naar Azure Containerexemplaren

Dit is de laatste zelfstudie in een reeks drie delen. Eerder in de reeks [is gemaakt met een installatiekopie van een container](container-instances-tutorial-prepare-app.md) en [naar een Azure Container Registry gepusht](container-instances-tutorial-prepare-acr.md). In dit artikel is de zelfstudie reeks voltooid door het implementeren van de container op exemplaren van Azure-Container.

In deze zelfstudie hebt u:

> [!div class="checklist"]
> * De container van het Azure-Container register met de Azure CLI implementeren
> * De toepassing in de browser weergeven
> * Bekijk de logboeken van de container

## <a name="before-you-begin"></a>Voordat u begint

Deze zelfstudie vereist dat u de Azure CLI versie 2.0.23 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u wilt installeren of upgraden, Zie [2.0 voor Azure CLI installeren][azure-cli-install].

Voor deze zelfstudie hebt voltooid, moet u een Docker-ontwikkelomgeving die lokaal zijn geïnstalleerd. Docker biedt pakketten die eenvoudig Docker op elke configureren [Mac][docker-mac], [Windows][docker-windows], of [Linux] [ docker-linux] system.

Azure Cloud-Shell is dan de Docker-onderdelen die nodig zijn voor het voltooien van elke stap in deze zelfstudie niet opgenomen. U moet de Azure CLI en Docker-ontwikkelomgeving installeren op uw lokale computer om deze zelfstudie te voltooien.

## <a name="deploy-the-container-using-the-azure-cli"></a>De container met de Azure CLI implementeren

De Azure CLI kunt de implementatie van een container voor exemplaren van Azure-Container in één opdracht. Omdat de container-installatiekopie wordt gehost in het persoonlijke Azure-Container register, moet u de referenties voor toegang tot dit opnemen. De referenties met de volgende Azure CLI-opdrachten ophalen.

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
az container create --resource-group myResourceGroup --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public --ports 80
```

Binnen enkele seconden, moet u een initiële reactie ontvangen van Azure Resource Manager. U kunt de status van de implementatie weergeven, met [az container weergeven][az-container-show]:

```azurecli
az container show --resource-group myResourceGroup --name aci-tutorial-app --query instanceView.state
```

Herhaal de [az container weergeven] [ az-container-show] opdracht totdat de status van verandert *in behandeling* naar *met*, die minder dan een minuut duren. Wanneer de container is *met*, gaat u verder met de volgende stap.

## <a name="view-the-application-and-container-logs"></a>Bekijk de logboeken toepassingen en container

Zodra de implementatie is geslaagd, weergegeven van de container openbaar IP-adres met de [az container weergeven] [ az-container-show] opdracht:

```bash
az container show --resource-group myResourceGroup --name aci-tutorial-app --query ipAddress.ip
```

Voorbeeld van uitvoer:`"13.88.176.27"`

Overzicht van de actieve toepassing, gaat u naar het openbare IP-adres in uw favoriete browser.

![Hallo wereld-app in de browser][aci-app-browser]

U kunt ook de uitvoer van de container bekijken:

```azurecli
az container logs --resource-group myResourceGroup --name aci-tutorial-app
```

Uitvoer:

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u niet langer de bronnen die u in deze zelfstudie reeks hebt gemaakt, kunt u uitvoeren de [az groep verwijderen] [ az-group-delete] opdracht om te verwijderen van de resourcegroep en alle resources die deze bevat. Deze opdracht verwijdert u de container-registersleutel die u hebt gemaakt, evenals de actieve container en alle gerelateerde resources.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt voltooid u het proces voor het implementeren van uw containers op Azure Containerexemplaren. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * De container van het Azure-Container register met de Azure CLI geïmplementeerd
> * De toepassing weergegeven in de browser.
> * De logboeken van de container weergegeven

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-container-show]: /cli/azure/container#az_container_show
[az-group-delete]: /cli/azure/group#az_group_delete
[azure-cli-install]: /cli/azure/install-azure-cli
[prepare-app]: ./container-instances-tutorial-prepare-app.md