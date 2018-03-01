---
title: Zelfstudie voor Azure Container Instances - App implementeren
description: Zelfstudie voor Azure Container Instances deel 3 van 3 - App implementeren
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 99bd03bf4c3ca2d7b1ced51ebfe8be669f271c1c
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2018
---
# <a name="deploy-a-container-to-azure-container-instances"></a>Een container implementeren in Azure Container Instances

Dit is de laatste zelfstudie in een reeks van drie. U hebt eerder [een containerinstallatiekopie gemaakt](container-instances-tutorial-prepare-app.md) en [gepusht naar een Azure Container Registry](container-instances-tutorial-prepare-acr.md). In dit artikel wordt de zelfstudie afgerond door de container te implementeren in Azure Container Instances.

In deze zelfstudie hebt u:

> [!div class="checklist"]
> * De container van de Azure Container Registry implementeren met behulp van de Azure CLI
> * De toepassing in de browser bekijken
> * De containerlogboeken bekijken

## <a name="before-you-begin"></a>Voordat u begint

Voor deze zelfstudie moet u de versie Azure CLI 2.0.27 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

Er moet lokaal een Docker-ontwikkelomgeving zijn geïnstalleerd om deze zelfstudie te voltooien. Docker biedt pakketten die eenvoudig Docker configureren op elk [Mac][docker-mac]-, [Windows][docker-windows]- of [Linux][docker-linux]-systeem.

Azure Cloud Shell bevat niet de vereiste Docker-onderdelen die nodig zijn om elke stap in deze zelfstudie te voltooien. U moet Azure CLI en de Docker-ontwikkelomgeving op uw lokale computer installeren om deze zelfstudie te voltooien.

## <a name="deploy-the-container-using-the-azure-cli"></a>De container implementeren met behulp van de Azure CLI

Dankzij de Azure CLI kan een container met één opdracht worden geïmplementeerd in Azure Container Instances. Aangezien de containerinstallatiekopie wordt gehost in de persoonlijke Azure Container Registry, moet u de referenties opnemen die vereist zijn om hier toegang toe te krijgen. Haal de referenties op met de volgende Azure CLI-opdrachten.

Aanmeldingsserver van containerregister (werk bij met de naam van uw register):

```azurecli
az acr show --name <acrName> --query loginServer
```

Wachtwoord van containerregister:

```azurecli
az acr credential show --name <acrName> --query "passwords[0].value"
```

Voer de volgende opdracht uit om de containerinstallatiekopie van het containerregister te implementeren met een resource-aanvraag van 1 CPU-kern en 1 GB geheugen. Vervang `<acrLoginServer>` en `<acrPassword>` door de waarden die u hebt opgehaald met de vorige twee opdrachten. Vervang `<acrName>` door de naam van het containerregister.

```azurecli
az container create --resource-group myResourceGroup --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-username <acrName> --registry-password <acrPassword> --dns-name-label aci-demo --ports 80
```

Binnen een paar seconden zou u een eerste reactie moeten ontvangen van Azure Resource Manager. De waarde `--dns-name-label` moet uniek zijn voor de Azure-regio waar u de containerinstallatiekopie maakt. Werk de waarde in het vorige voorbeeld bij als u een foutbericht over het **DNS-naamlabel** krijgt wanneer u de opdracht uitvoert.

Gebruik [az container show][az-container-show] om de status van de implementatie te bekijken:

```azurecli
az container show --resource-group myResourceGroup --name aci-tutorial-app --query instanceView.state
```

Herhaal de opdracht [az container show][az-container-show] totdat de status verandert van *In behandeling* in *Wordt uitgevoerd*. Als het goed is, duurt dit minder dan een minuut. Wanneer de container de status *Wordt uitgevoerd* heeft, gaat u verder met de volgende stap.

## <a name="view-the-application-and-container-logs"></a>De toepassings- en containerlogboeken bekijken

Wanneer de implementatie is geslaagd, geeft u de volledig gekwalificeerde domeinnaam (FQDN) van de container weer met de opdracht [az container show][az-container-show]:

```bash
az container show --resource-group myResourceGroup --name aci-tutorial-app --query ipAddress.fqdn
```

Voorbeelduitvoer: `"aci-demo.eastus.azurecontainer.io"`

Als u de actieve toepassing wilt bekijken, navigeert u in uw browser naar de weergegeven DNS-naam:

![Hello world-app in browser][aci-app-browser]

U kunt ook de logboekuitvoer van de container bekijken:

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

Als u de resources die u in de zelfstudiereeks hebt gemaakt, niet langer nodig hebt, kunt u de opdracht [az group delete][az-group-delete] uitvoeren om de resourcegroep en alle resources daarin te verwijderen. Met deze opdracht verwijdert u het containerregister dat u hebt gemaakt, evenals de actieve container en alle gerelateerde resources.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het proces voor het implementeren van uw containers naar Azure Container Instances doorlopen. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * De container van de Azure Container Registry implementeren met behulp van de Azure CLI
> * De toepassing in de browser bekijken
> * De containerlogboeken ophalen

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
