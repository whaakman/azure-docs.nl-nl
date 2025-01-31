---
title: Snelstartgids - Docker-container implementeren in Azure Container Instances - CLI
description: In deze snelstartgids hebt u de Azure CLI gebruiken om te snel een beperkte web-app die wordt uitgevoerd in een geïsoleerde Azure containerexemplaar implementeren
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 03/21/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 8e504a081f8685107871aed920077dd75a70dfa7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "65908105"
---
# <a name="quickstart-deploy-a-container-instance-in-azure-using-the-azure-cli"></a>Quickstart: Een containerinstantie in Azure met behulp van de Azure CLI implementeren

Gebruik Azure Container Instances om uit te voeren zonder server Docker-containers in Azure met de eenvoud en snelheid. Een toepassing implementeren in een container-exemplaar op aanvraag wanneer u niet nodig voor een volledige container-orchestration-platform, zoals Azure Kubernetes Service hebt.

In deze snelstartgids gebruikt u de Azure CLI een geïsoleerde Docker-container implementeren en de toepassing beschikbaar met een volledig gekwalificeerde domeinnaam (FQDN). Een paar seconden nadat u een met één implementatieopdracht uitvoeren, kunt u bladeren naar de toepassing die wordt uitgevoerd in de container:

![App die is geïmplementeerd in Azure Container Instances, weergegeven in de browser][aci-app-browser]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account][azure-account] voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

U kunt de Azure Cloud Shell of een lokale installatie van de Azure CLI gebruiken om deze Quick Start uit te voeren. Als u wilt gebruiken op het bestand lokaal op, versie 2.0.55 of later wordt aanbevolen. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Azure Container Instances moeten, zoals alle Azure-resources, worden geïmplementeerd in een resourcegroep. Met resourcegroepen kunt u gerelateerde Azure-resources ordenen en beheren.

Maak eerst een resourcegroep met de naam *myResourceGroup* op de locatie *eastus* met behulp van de volgende [az group create][az-group-create]-opdracht:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Een container maken

Nu u een resourcegroep hebt, kunt u een container in Azure uitvoeren. Als u een containerinstantie met de Azure CLI wilt maken, geeft u de naam van een resourcegroep, de naam van een containerinstantie en een Docker-containerinstallatiekopie op voor de opdracht [az container create][az-container-create]. In deze snelstartgids gebruikt u de openbare `mcr.microsoft.com/azuredocs/aci-helloworld` installatiekopie. Deze installatiekopie bevat een kleine web-app die is geschreven in Node.js en die een statische HTML-pagina dient.

Als u uw containers beschikbaar wilt maken op internet, moet u een of meer poorten om te openen of een DNS-naamlabel opgeven, of beide. In deze quickstart implementeert u een container met een DNS-naamlabel zodat de web-app openbaar bereikbaar is.

Voer een vergelijkbaar met de volgende opdracht om een containerexemplaar te starten. Stel een `--dns-name-label` waarde die uniek is binnen de Azure-regio waar u het exemplaar maakt. Als u een foutbericht 'DNS-naamlabel niet beschikbaar' ontvangt, probeert u een ander DNS-naamlabel.

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image mcr.microsoft.com/azuredocs/aci-helloworld --dns-name-label aci-demo --ports 80
```

Binnen enkele seconden krijgt u een reactie van de Azure-CLI die aangeeft dat de implementatie is voltooid. Controleer de status ervan met de opdracht [az container show] [ az-container-show]:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
```

Wanneer u de opdracht uitvoert, worden de FQDN (Fully Qualified Domain Name) en de inrichtingsstatus van de container weergegeven.

```console
$ az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
FQDN                               ProvisioningState
---------------------------------  -------------------
aci-demo.eastus.azurecontainer.io  Succeeded
```

Als `ProvisioningState` van de container **Voltooid** is, gaat u naar de FQDN in de browser. U moet nu een webpagina zien die lijkt op de volgende. U hebt een toepassing geïmplementeerd die wordt uitgevoerd in een Docker-container voor Azure.

![Schermafbeelding van browser met toepassing die wordt uitgevoerd in een exemplaar van een Azure-container][aci-app-browser]

Als de toepassing niet meteen wordt weergegeven, moet u mogelijk een paar seconden wachten terwijl DNS wordt doorgegeven. Probeer vervolgens de browser te vernieuwen.

## <a name="pull-the-container-logs"></a>De containerlogboeken ophalen

Wanneer u problemen moet oplossen voor een container of de toepassing die daarmee wordt uitgevoerd, of als u alleen de uitvoer wilt zien, kunt u eerst de logboeken van de containerinstantie bekijken.

Haal de logboeken van de containerinstantie op met de opdracht [az container logs][az-container-logs]:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

De uitvoer geeft de logboeken voor de container weer, evenals de HTTP GET-aanvragen die worden gegenereerd wanneer u de toepassing in uw browser weergeeft.

```console
$ az container logs --resource-group myResourceGroup --name mycontainer
listening on port 80
::ffff:10.240.255.55 - - [21/Mar/2019:17:43:53 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
::ffff:10.240.255.55 - - [21/Mar/2019:17:44:36 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
::ffff:10.240.255.55 - - [21/Mar/2019:17:44:36 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
```

## <a name="attach-output-streams"></a>Uitvoerstromen koppelen

Behalve het bekijken van de logboeken kunt u ook uw lokale standaarduitvoer- en standaardgegevensstromen met fouten koppelen aan die van de container.

Voer eerst de opdracht [az container attach][az-container-attach] uit om uw lokale console aan de uitvoerstromen van de container te koppelen:

```azurecli-interactive
az container attach --resource-group myResourceGroup --name mycontainer
```

Vernieuw de browser na het koppelen een paar keer om wat extra uitvoer te genereren. Wanneer u klaar bent, ontkoppelt u ten slotte de console met `Control+C`. De uitvoer ziet er als volgt uit:

```console
$ az container attach --resource-group myResourceGroup --name mycontainer
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2019-03-21 17:27:20+00:00) pulling image "mcr.microsoft.com/azuredocs/aci-helloworld"
(count: 1) (last timestamp: 2019-03-21 17:27:24+00:00) Successfully pulled image "mcr.microsoft.com/azuredocs/aci-helloworld"
(count: 1) (last timestamp: 2019-03-21 17:27:27+00:00) Created container
(count: 1) (last timestamp: 2019-03-21 17:27:27+00:00) Started container

Start streaming logs:
listening on port 80

::ffff:10.240.255.55 - - [21/Mar/2019:17:43:53 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
::ffff:10.240.255.55 - - [21/Mar/2019:17:44:36 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
::ffff:10.240.255.55 - - [21/Mar/2019:17:44:36 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
::ffff:10.240.255.55 - - [21/Mar/2019:17:47:01 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
::ffff:10.240.255.56 - - [21/Mar/2019:17:47:12 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met de container, verwijdert u deze met de opdracht [az container delete][az-container-delete]:

```azurecli-interactive
az container delete --resource-group myResourceGroup --name mycontainer
```

Om te controleren of de container is verwijderd, voert u de opdracht [az container list](/cli/azure/container#az-container-list) uit:

```azurecli-interactive
az container list --resource-group myResourceGroup --output table
```

De container **mycontainer** mag dan niet worden weergegeven in de uitvoer van de opdracht. Als u geen andere containers in de resourcegroep hebt, wordt er geen uitvoer weergegeven.

Als u klaar bent met de *myResourceGroup*-resourcegroep en alle resources die deze bevat, verwijdert u deze met de opdracht [az group delete] [ az-group-delete]:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start, kunt u een Azure containerexemplaar gemaakt met behulp van een installatiekopie van de openbare Microsoft. Als u zelf een containerinstallatiekopie wilt bouwen en deze wilt implementeren met behulp van een privé Azure Container-register, gaat u verder met de zelfstudie voor Azure Container Instances.

> [!div class="nextstepaction"]
> [Zelfstudie voor Azure Container Instances](./container-instances-tutorial-prepare-app.md)

Zie de quickstarts voor [Azure Kubernetes Service (AKS)][container-service] als u opties wilt uitproberen voor het uitvoeren van containers in een orchestration-systeem op Azure.

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - External -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[azure-account]: https://azure.microsoft.com/free/
[node-js]: https://nodejs.org

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az-container-attach
[az-container-create]: /cli/azure/container#az-container-create
[az-container-delete]: /cli/azure/container#az-container-delete
[az-container-list]: /cli/azure/container#az-container-list
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli-install]: /cli/azure/install-azure-cli
[container-service]: ../aks/kubernetes-walkthrough.md
