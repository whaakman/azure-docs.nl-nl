---
title: Zelfstudie voor Azure Containerexemplaren - voorbereiden van uw app
description: Een app voorbereiden voor implementatie in Azure Container Instances
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 11/20/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 6a168b600833c7e4544da7a5f5f4b7a0af73e0b5
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="create-container-for-deployment-to-azure-container-instances"></a>Een container voor implementatie in Azure Container Instances maken

Met Azure Container Instances kunt u Docker-containers implementeren in de Azure-infrastructuur zonder virtuele machines in te richten of gebruik te maken van een service op een hoger niveau. In deze zelfstudie maakt u een kleine webtoepassing in Node.js bouwen en in een container die kan worden uitgevoerd met Azure Container instanties in een pakket. We behandelen de volgende onderwerpen:

> [!div class="checklist"]
> * Toepassingsbron klonen vanuit GitHub
> * Containerinstallatiekopieën van de toepassingsbron maken
> * De installatiekopieën testen in een lokale Docker-omgeving

Uw installatiekopie uploaden naar een Azure Container Registry in volgende zelfstudies en vervolgens implementeren op Azure Containerexemplaren.

## <a name="before-you-begin"></a>Voordat u begint

Deze zelfstudie vereist dat u de Azure CLI versie 2.0.21 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli).

Deze zelfstudie wordt ervan uitgegaan dat een basiskennis van core Docker-concepten zoals containers, installatiekopieën van de container en basic `docker` opdrachten. Zie, indien nodig, [Aan de slag met Docker]( https://docs.docker.com/get-started/) voor een uitleg van de basisprincipes van containers.

Voor deze zelfstudie hebt u een Docker-ontwikkelomgeving nodig. Docker biedt pakketten die eenvoudig Docker op elke configureren op elk [Mac](https://docs.docker.com/docker-for-mac/)-, [Windows](https://docs.docker.com/docker-for-windows/)- of [Linux](https://docs.docker.com/engine/installation/#supported-platforms)-systeem.

Azure Cloud-Shell is dan de Docker-onderdelen die nodig zijn voor het voltooien van elke stap in deze zelfstudie niet opgenomen. Daarom raden we aan een lokale installatie van de Azure CLI en Docker-ontwikkelomgeving.

## <a name="get-application-code"></a>Toepassingscode ophalen

Het voorbeeld in deze zelfstudie bevat een eenvoudige webtoepassing die is gebouwd in [Node.js](http://nodejs.org). De app dient een statische HTML-pagina en ziet er als volgt uit:

![Zelfstudie-app weergegeven in browser][aci-tutorial-app]

Gebruik git om het voorbeeld te downloaden:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-the-container-image"></a>De containerinstallatiekopie bouwen

Het Dockerfile dat is opgegeven in de voorbeeldopslagplaats laat zien hoe de container wordt gebouwd. Het begint met een [officiële Node.js-installatiekopie][dockerhub-nodeimage] op basis van [Alpine Linux](https://alpinelinux.org/), een kleine distributie die zeer geschikt voor gebruik met containers. Vervolgens worden de toepassingsbestanden naar de container gekopieerd, afhankelijkheden geïnstalleerd met behulp van het de Node Package Manager, en wordt ten slotte de toepassing gestart.

```Dockerfile
FROM node:8.2.0-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

Gebruik de opdracht `docker build` voor het maken van de containerinstallatiekopie door deze te taggen als *aci-tutorial-app*:

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

De uitvoer van de `docker build` opdracht is vergelijkbaar met de volgende (afgekapt voor leesbaarheid):

```bash
Sending build context to Docker daemon  119.3kB
Step 1/6 : FROM node:8.2.0-alpine
8.2.0-alpine: Pulling from library/node
88286f41530e: Pull complete
84f3a4bf8410: Pull complete
d0d9b2214720: Pull complete
Digest: sha256:c73277ccc763752b42bb2400d1aaecb4e3d32e3a9dbedd0e49885c71bea07354
Status: Downloaded newer image for node:8.2.0-alpine
 ---> 90f5ee24bee2
...
Step 6/6 : CMD node /usr/src/app/index.js
 ---> Running in f4a1ea099eec
 ---> 6edad76d09e9
Removing intermediate container f4a1ea099eec
Successfully built 6edad76d09e9
Successfully tagged aci-tutorial-app:latest
```

Gebruik de `docker images` om de gebouwde installatiekopie te bekijken:

```bash
docker images
```

Uitvoer:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

## <a name="run-the-container-locally"></a>De container lokaal uitvoeren

Voer voordat u probeert de container te implementeren naar Azure Container Instances de container lokaal uit om te controleren of deze werkt. Met de schakeloptie `-d` wordt de container op de achtergrond uitgevoerd en met `-p` kunt u een willekeurige poort toewijzen aan poort 80 in de container.

```bash
docker run -d -p 8080:80 aci-tutorial-app
```

Open de browser en ga naar http://localhost:8080 om te bevestigen dat de container wordt uitgevoerd.

![De app lokaal uitvoeren in de browser][aci-tutorial-app-local]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een containerinstallatiekopie gemaakt die kan worden geïmplementeerd naar Azure Container Instances. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * De toepassingsbron van GitHub gekloond
> * Installatiekopieën van het gemaakte container van toepassingsbron
> * De container lokaal getest

Ga verder met de volgende zelfstudie voor meer informatie over het opslaan van installatiekopieën van de container in een Azure Container Registry.

> [!div class="nextstepaction"]
> [Installatiekopieën pushen naar Azure Container Registry](./container-instances-tutorial-prepare-acr.md)

<!-- LINKS -->
[dockerhub-nodeimage]: https://store.docker.com/images/node

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png