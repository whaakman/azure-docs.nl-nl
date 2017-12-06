---
title: Zelfstudie voor Azure Containerexemplaren - register van Azure-Container voorbereiden
description: Zelfstudie voor Azure Containerexemplaren - register van Azure-Container voorbereiden
services: container-instances
author: neilpeterson
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 11/20/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 6e69b7a3540ce90743f9dd75664b118751f4a63b
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="deploy-and-use-azure-container-registry"></a>Implementeren en gebruiken van Azure Container register

Dit maakt deel uit van een zelfstudie drie delen. In de [vorige stap](container-instances-tutorial-prepare-app.md), een installatiekopie van een container is gemaakt voor een eenvoudige webtoepassing geschreven in [Node.js](http://nodejs.org). In deze zelfstudie kunt u de installatiekopie naar een Azure Container register push. Als u de installatiekopie van de container niet hebt gemaakt, terug naar [zelfstudie 1 – afbeelding van de container maken](container-instances-tutorial-prepare-app.md).

Het register van de Container Azure is een register op basis van Azure, persoonlijke voor installatiekopieën van de Docker-container. Deze zelfstudie wordt begeleid bij het implementeren van een Azure Container register-exemplaar en een installatiekopie van een container te pushen. Stappen voltooid omvatten:

> [!div class="checklist"]
> * Een exemplaar van Azure Container register implementeren
> * Afbeelding van de container voor Azure Container register tagging
> * Installatiekopie uploaden naar Azure Container register

In volgende zelfstudies leert implementeren u de container uit uw persoonlijke register naar Azure Containerexemplaren.

## <a name="before-you-begin"></a>Voordat u begint

Deze zelfstudie vereist dat u de Azure CLI versie 2.0.21 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli).

Voor deze zelfstudie hebt u een Docker-ontwikkelomgeving nodig. Docker biedt pakketten die eenvoudig Docker op elke configureren op elk [Mac](https://docs.docker.com/docker-for-mac/)-, [Windows](https://docs.docker.com/docker-for-windows/)- of [Linux](https://docs.docker.com/engine/installation/#supported-platforms)-systeem.

Azure Cloud-Shell is dan de Docker-onderdelen die nodig zijn voor het voltooien van elke stap in deze zelfstudie niet opgenomen. Daarom raden we aan een lokale installatie van de Azure CLI en Docker-ontwikkelomgeving.

## <a name="deploy-azure-container-registry"></a>Register met Azure Container implementeren

Wanneer u een Azure Container Registry implementeert, moet u eerst een resourcegroep. Een Azure-resourcegroep is een logische verzameling in welke Azure resources worden geïmplementeerd en beheerd.

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create). In dit voorbeeld wordt een resourcegroep met de naam *myResourceGroup* wordt gemaakt in de *eastus* regio.

```azurecli
az group create --name myResourceGroup --location eastus
```

Maken van een Azure container register met de [az acr maken](/cli/azure/acr#create) opdracht. De containernaam register **moeten uniek zijn** in Azure, en 5 50 alfanumerieke tekens moeten bevatten. Vervang `<acrName>` met een unieke naam voor het register:

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Bijvoorbeeld, een Azure container registry maken met de naam *mycontainerregistry082*:

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

In de rest van deze zelfstudie gebruiken we `<acrName>` als tijdelijke aanduiding voor de container register-naam die u hebt gekozen.

## <a name="container-registry-login"></a>Container register aanmelding

U moet zich aanmelden met uw ACR-exemplaar voordat u installatiekopieën aan. Gebruik de [az acr aanmelding](/cli/azure/acr#az_acr_login) opdracht om de bewerking te voltooien. U moet opgeven dat de unieke naam die is opgegeven in het register van de container wanneer deze is gemaakt.

```azurecli
az acr login --name <acrName>
```

De opdracht retourneert een `Login Succeeded` bericht eenmaal is voltooid.

## <a name="tag-container-image"></a>De installatiekopie van de tag-container

Voor het implementeren van de installatiekopie van een container van een persoonlijke register, de afbeelding moet worden gemarkeerd met de `loginServer` naam van het register.

Als een lijst met huidige afbeeldingen wilt weergeven, gebruikt de `docker images` opdracht.

```bash
docker images
```

Uitvoer:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

Als u de naam van de loginServer, voer de volgende opdracht. Vervang `<acrName>` met de naam van het register van de container.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Voorbeelduitvoer:

```
Result
------------------------
mycontainerregistry082.azurecr.io
```

Tag de *aci-zelfstudie-app* installatiekopie met de loginServer van het register van de container. Ook toe te voegen `:v1` aan het einde van de naam van de installatiekopie. Deze code geeft het versienummer van de installatiekopie. Vervang `<acrLoginServer>` met het resultaat van de `az acr show` opdracht u zojuist hebt uitgevoerd.

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

Push de *aci-zelfstudie-app* afbeelding in het register met de `docker push` opdracht. Vervang `<acrLoginServer>` met de volledige aanmeldingsnaam voor de server die u in de vorige stap verkrijgen.

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

U kunt een lijst met afbeeldingen die een pushbericht hebben ontvangen in het register van uw Azure-Container gebruiken de [az acr opslagplaats lijst](/cli/azure/acr/repository#list) opdracht. De opdracht bijwerken met de naam van de container-register.

```azurecli
az acr repository list --name <acrName> --output table
```

Uitvoer:

```azurecli
Result
----------------
aci-tutorial-app
```

En gebruik vervolgens de labels voor een specifieke installatiekopie vindt de [az acr opslagplaats weergeven-labels](/cli/azure/acr/repository#show-tags) opdracht.

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

In deze zelfstudie een Azure Container Registry is voorbereid voor gebruik met Azure Containerexemplaren en de installatiekopie van de container is gepusht. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * Een exemplaar van het register van Azure-Container geïmplementeerd
> * Een installatiekopie van een container voor Azure Container register met tags
> * Een installatiekopie van een Azure Container register geüpload

Ga naar de volgende zelfstudie voor meer informatie over het implementeren van de container in Azure maken met Azure Containerexemplaren.

> [!div class="nextstepaction"]
> [Implementeer containers naar Containerexemplaren Azure](./container-instances-tutorial-deploy-app.md)
