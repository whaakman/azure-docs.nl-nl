---
title: Zelfstudie voor Azure Container Instances - Azure Container Registry voorbereiden
description: Zelfstudie voor Azure Container Instances, deel 2 van 3 - Azure Container Registry voorbereiden
services: container-instances
author: neilpeterson
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 94ecba44b8281460da4518c146aab814d2eaa850
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="deploy-and-use-azure-container-registry"></a>Azure Container Registry implementeren en gebruiken

Dit is deel 2 van een zelfstudie met drie delen. In de [vorige stap](container-instances-tutorial-prepare-app.md) hebt u een containerinstallatiekopie gemaakt voor een eenvoudige webtoepassing geschreven in [Node.js][nodejs]. In deze zelfstudie pusht u de installatiekopie naar een Azure Container Registry. Als u de containerinstallatiekopie niet hebt gemaakt, gaat u terug naar [Zelfstudie 1: Containerinstallatiekopieën maken](container-instances-tutorial-prepare-app.md).

Azure Container Registry is een op Azure gebaseerd, persoonlijk register voor Docker-containerinstallatiekopieën. Deze zelfstudie legt u stapsgewijs uit hoe u een Azure Container Registry-exemplaar implementeert en hoe u een containerinstallatiekopie naar dit exemplaar pusht.

In dit artikel, deel twee uit de reeks, voert u de volgende handelingen uit:

> [!div class="checklist"]
> * Een Azure Container Registry-exemplaar implementeren
> * Een containerinstallatiekopie taggen voor het Azure Container Registry
> * De installatiekopie uploaden naar het register

In het volgende artikel, de laatste zelfstudie in de reeks, implementeert u de container uit het persoonlijke register naar Azure Container Instances.

## <a name="before-you-begin"></a>Voordat u begint

Voor deze zelfstudie moet u de versie Azure CLI 2.0.23 of later uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

Er moet lokaal een Docker-ontwikkelomgeving zijn geïnstalleerd om deze zelfstudie te voltooien. Docker biedt pakketten die eenvoudig Docker configureren op elk [Mac][docker-mac]-, [Windows][docker-windows]- of [Linux][docker-linux]-systeem.

Azure Cloud Shell bevat niet de vereiste Docker-onderdelen die nodig zijn om elke stap in deze zelfstudie te voltooien. U moet Azure CLI en de Docker-ontwikkelomgeving op uw lokale computer installeren om deze zelfstudie te voltooien.

## <a name="deploy-azure-container-registry"></a>Azure Container Registry implementeren

Wanneer u een Azure Container Registry implementeert, hebt u eerst een resourcegroep nodig. Een resourcegroep is een logische verzameling waarin Azure-resources worden geïmplementeerd en beheerd.

Een resourcegroep maken met de opdracht [az group create][az-group-create]. In dit voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt in de regio *eastus*.

```azurecli
az group create --name myResourceGroup --location eastus
```

Maak een Azure Container Registry met de opdracht [az acr create][az-acr-create]. De containerregisternaam moet uniek zijn binnen Azure en mag 5 tot 50 alfanumerieke tekens bevatten. Vervang `<acrName>` door een unieke naam voor het register:

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Bijvoorbeeld, om een Azure Container Registry te maken met de naam *mycontainerregistry082*:

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

In de rest van deze zelfstudie gebruiken we `<acrName>` als tijdelijke aanduiding voor de gekozen containerregisternaam.

## <a name="container-registry-login"></a>Aanmelden bij het containerregister

U moet u aanmelden bij het Azure Container Registry-exemplaar voordat u installatiekopieën kunt pushen. Gebruik de opdracht [az acr login][az-acr-login] om de bewerking te voltooien. U moet de unieke naam van het containerregister opgeven die u hebt gekozen toen u het maakte.

```azurecli
az acr login --name <acrName>
```

Met de opdracht wordt een `Login Succeeded`-bericht geretourneerd nadat deze is voltooid.

## <a name="tag-container-image"></a>Containerinstallatiekopieën taggen

Als u de containerinstallatiekopie wilt implementeren vanuit een persoonlijk register, moet u de installatiekopie taggen met de `loginServer`-naam van het register.

Als u een lijst met huidige installatiekopieën wilt weergeven, gebruikt u de opdracht [docker images][docker-images].

```bash
docker images
```

Uitvoer:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

Voer de volgende opdracht [az acr show][az-acr-show] uit om de loginServer-naam op te halen. Vervang `<acrName>` door de naam van het containerregister.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Voorbeelduitvoer:

```
Result
------------------------
mycontainerregistry082.azurecr.io
```

Tag de installatiekopie *aci-tutorial-app* met de loginServer van het containerregister. Voeg bovendien `:v1` toe aan het eind van de installatiekopienaam. Deze tag geeft het versienummer van de installatiekopie aan. Vervang `<acrLoginServer>` door het resultaat van de opdracht [az acr show][az-acr-show] die u zojuist hebt uitgevoerd.

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

Voer na het taggen de opdracht `docker images` uit om de bewerking te controleren.

```bash
docker images
```

Uitvoer:

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a>Installatiekopie pushen naar Azure Container Registry

Push de installatiekopie *aci-tutorial-app* naar het register met de opdracht [docker push][docker-push]. Vervang `<acrLoginServer>` door de volledige naam van de aanmeldingsserver die u in de vorige stap hebt verkregen.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

De bewerking `push` duurt minimaal enkele seconden tot maximaal enkele minuten afhankelijk van de internetverbinding, en de uitvoer ziet er ongeveer als volgt uit:

```bash
The push refers to a repository [mycontainerregistry082.azurecr.io/aci-tutorial-app]
3db9cac20d49: Pushed
13f653351004: Pushed
4cd158165f4d: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:ed67fff971da47175856505585dcd92d1270c3b37543e8afd46014d328f05715 size: 1576
```

## <a name="list-images-in-azure-container-registry"></a>Installatiekopieën vermelden in Azure Container Registry

Gebruik de opdracht [az acr repository list][az-acr-repository-list] om een lijst met installatiekopieën te retourneren die naar het Azure Container Registry zijn gepusht. Werk de opdracht bij met de naam van het containerregister.

```azurecli
az acr repository list --name <acrName> --output table
```

Uitvoer:

```azurecli
Result
----------------
aci-tutorial-app
```

Gebruik vervolgens de opdracht [az acr repository show-tags][az-acr-repository-show-tags] om de tags voor een specifieke installatiekopie weer te geven.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

Uitvoer:

```azurecli
Result
--------
v1
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een Azure Container Registry voorbereid voor gebruik met Azure Container Instances en een containerinstallatiekopie naar het register gepusht. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * Er is een Azure Container Registry-exemplaar geïmplementeerd
> * Er is een containerinstallatiekopie getagd voor Azure Container Registry
> * Er is een installatiekopie naar Azure Container Registry geüpload

Ga naar de volgende zelfstudie voor meer informatie over het implementeren van de container in Azure met behulp van Azure Container Instances.

> [!div class="nextstepaction"]
> [Containers implementeren in Azure Container Instances](./container-instances-tutorial-deploy-app.md)

<!-- LINKS - External -->
[docker-build]: https://docs.docker.com/engine/reference/commandline/build/
[docker-get-started]: https://docs.docker.com/get-started/
[docker-hub-nodeimage]: https://store.docker.com/images/node
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/
[nodejs]: http://nodejs.org

<!-- LINKS - Internal -->
[az-acr-create]: /cli/azure/acr#az_acr_create
[az-acr-login]: /cli/azure/acr#az_acr_login
[az-acr-repository-list]: /cli/azure/acr/repository#az_acr_list
[az-acr-repository-show-tags]: /cli/azure/acr/repository#az_acr_repository_show_tags
[az-acr-show]: /cli/azure/acr#az_acr_show
[az-group-create]: /cli/azure/group#az_group_create
[azure-cli-install]: /cli/azure/install-azure-cli
