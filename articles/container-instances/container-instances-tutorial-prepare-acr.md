---
title: Zelfstudie voor Azure Containerexemplaren - register van Azure-Container voorbereiden
description: Azure Containerexemplaren zelfstudie deel 2 van 3 - Azure-Container register voorbereiden
services: container-instances
author: neilpeterson
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: c0aad1f9bbaac9a456b34f75633faba92f57f498
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="deploy-and-use-azure-container-registry"></a>Implementeren en gebruiken van Azure Container register

Dit maakt deel uit van een zelfstudie drie delen. In de [vorige stap](container-instances-tutorial-prepare-app.md), een installatiekopie van een container is gemaakt voor een eenvoudige webtoepassing geschreven in [Node.js][nodejs]. In deze zelfstudie kunt u de installatiekopie naar een Azure Container register push. Als u de installatiekopie van de container niet hebt gemaakt, terug naar [zelfstudie 1 – afbeelding van de container maken](container-instances-tutorial-prepare-app.md).

Het register van de Container Azure is een Azure, persoonlijke registry voor installatiekopieën van de Docker-container. Deze zelfstudie wordt u begeleid bij het implementeren van een Azure Container register-exemplaar en een installatiekopie van een container te pushen.

In dit artikel deel twee van de reeks u:

> [!div class="checklist"]
> * Implementeer een exemplaar van Azure Container register
> * Label van de installatiekopie van een container voor uw Azure-container-register
> * Upload de installatiekopie aan het register

In het volgende artikel, de laatste zelfstudie in de reeks implementeert u de container uit uw persoonlijke register in Azure Containerexemplaren.

## <a name="before-you-begin"></a>Voordat u begint

Deze zelfstudie vereist dat u de Azure CLI versie 2.0.23 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u wilt installeren of upgraden, Zie [2.0 voor Azure CLI installeren][azure-cli-install].

Voor deze zelfstudie hebt voltooid, moet u een Docker-ontwikkelomgeving die lokaal zijn geïnstalleerd. Docker biedt pakketten die eenvoudig Docker op elke configureren [Mac][docker-mac], [Windows][docker-windows], of [Linux] [ docker-linux] system.

Azure Cloud-Shell is dan de Docker-onderdelen die nodig zijn voor het voltooien van elke stap in deze zelfstudie niet opgenomen. U moet de Azure CLI en Docker-ontwikkelomgeving installeren op uw lokale computer om deze zelfstudie te voltooien.

## <a name="deploy-azure-container-registry"></a>Register met Azure Container implementeren

Wanneer u een Azure Container Registry implementeert, moet u eerst een resourcegroep. Een Azure-resourcegroep is een logische verzameling in welke Azure resources worden geïmplementeerd en beheerd.

Een resourcegroep maken met de opdracht [az group create][az-group-create]. In dit voorbeeld wordt een resourcegroep met de naam *myResourceGroup* wordt gemaakt in de *eastus* regio.

```azurecli
az group create --name myResourceGroup --location eastus
```

Maken van een Azure container register met de [az acr maken] [ az-acr-create] opdracht. De containernaam register **moeten uniek zijn** in Azure, en 5 50 alfanumerieke tekens moeten bevatten. Vervang `<acrName>` met een unieke naam voor het register:

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Bijvoorbeeld, een Azure container registry maken met de naam *mycontainerregistry082*:

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

In de rest van deze zelfstudie gebruiken we `<acrName>` als tijdelijke aanduiding voor de container register-naam die u hebt gekozen.

## <a name="container-registry-login"></a>Container register aanmelding

U moet zich aanmelden met uw exemplaar van Azure Container register voordat u installatiekopieën aan. Gebruik de [az acr aanmelding] [ az-acr-login] opdracht om de bewerking te voltooien. U moet opgeven dat de unieke naam die u hebt opgegeven voor de container register wanneer u het hebt gemaakt.

```azurecli
az acr login --name <acrName>
```

De opdracht retourneert een `Login Succeeded` bericht eenmaal is voltooid.

## <a name="tag-container-image"></a>De installatiekopie van de tag-container

Voor het implementeren van de installatiekopie van een container van een persoonlijke register, moet u de installatiekopie met labelen de `loginServer` naam van het register.

Als een lijst met huidige afbeeldingen wilt weergeven, gebruikt de [docker-installatiekopieën] [ docker-images] opdracht.

```bash
docker images
```

Uitvoer:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

Als u de naam van de loginServer, voer de [az acr weergeven] [ az-acr-show] opdracht. Vervang `<acrName>` met de naam van het register van de container.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Voorbeelduitvoer:

```
Result
------------------------
mycontainerregistry082.azurecr.io
```

Tag de *aci-zelfstudie-app* installatiekopie met de loginServer van het register van de container. Ook toe te voegen `:v1` aan het einde van de naam van de installatiekopie. Deze code geeft het versienummer van de installatiekopie. Vervang `<acrLoginServer>` met het resultaat van de [az acr weergeven] [ az-acr-show] opdracht u zojuist hebt uitgevoerd.

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

Uitvoeren zodra gecodeerd, `docker images` om te controleren of de bewerking.

```bash
docker images
```

Uitvoer:

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a>Push-installatiekopie in Azure Container register

Push de *aci-zelfstudie-app* afbeelding in het register met de [docker push] [ docker-push] opdracht. Vervang `<acrLoginServer>` met de volledige aanmeldingsnaam voor de server die u in de vorige stap verkrijgen.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

De `push` bewerking duurt een paar seconden enkele minuten duren, afhankelijk van uw internetverbinding en de uitvoer ziet er ongeveer als volgt:

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

## <a name="list-images-in-azure-container-registry"></a>Lijst met afbeeldingen in Azure Container register

U kunt een lijst met afbeeldingen die een pushbericht hebben ontvangen in het register van uw Azure-Container gebruiken de [az acr opslagplaats lijst] [ az-acr-repository-list] opdracht. De opdracht bijwerken met de naam van de container-register.

```azurecli
az acr repository list --name <acrName> --output table
```

Uitvoer:

```azurecli
Result
----------------
aci-tutorial-app
```

En gebruik vervolgens de labels voor een specifieke installatiekopie vindt de [az acr opslagplaats weergeven-labels] [ az-acr-repository-show-tags] opdracht.

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

In deze zelfstudie kunt u een Azure Container Registry voorbereid voor gebruik met Azure Containerexemplaren en gepusht van een installatiekopie van een container in het register. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * Een exemplaar van het register van Azure-Container geïmplementeerd
> * Een installatiekopie van een container voor Azure Container register met tags
> * Een installatiekopie van een Azure Container register geüpload

Ga naar de volgende zelfstudie voor meer informatie over het implementeren van de container in Azure maken met Azure Containerexemplaren.

> [!div class="nextstepaction"]
> [Implementeer containers naar Containerexemplaren Azure](./container-instances-tutorial-deploy-app.md)

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
