---
title: Zelfstudie voor Azure Containerexemplaren - voorbereiden van uw app
description: 'Azure Containerexemplaren zelfstudie, deel 1 van 3: een app voorbereiden voor implementatie naar Containerexemplaren Azure'
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: fc16be80e776d1472be775fa32354ba157d16545
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="create-container-for-deployment-to-azure-container-instances"></a>Een container voor implementatie in Azure Container Instances maken

Met Azure Container Instances kunt u Docker-containers implementeren in de Azure-infrastructuur zonder virtuele machines in te richten of gebruik te maken van een service op een hoger niveau. In deze zelfstudie maakt u een kleine webtoepassing in Node.js bouwen en in een container die kan worden uitgevoerd met Azure Container instanties in een pakket.

In dit artikel deel uit van een van de reeks u:

> [!div class="checklist"]
> * Klonen van de broncode van de toepassing van GitHub
> * Een installatiekopie van een container van toepassingsbron maken
> * De afbeelding te testen in een lokale Docker-omgeving

Uw installatiekopie uploaden naar een Azure Container Registry in volgende zelfstudies en vervolgens implementeren op Azure Containerexemplaren.

## <a name="before-you-begin"></a>Voordat u begint

Deze zelfstudie vereist dat u de Azure CLI versie 2.0.23 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u wilt installeren of upgraden, Zie [2.0 voor Azure CLI installeren][azure-cli-install].

Deze zelfstudie wordt ervan uitgegaan dat een basiskennis van core Docker-concepten zoals containers, installatiekopieën van de container en basic `docker` opdrachten. Indien nodig, Zie [aan de slag met Docker] [ docker-get-started] voor een primer op de basisprincipes van de container.

Voor deze zelfstudie hebt voltooid, moet u een Docker-ontwikkelomgeving die lokaal zijn geïnstalleerd. Docker biedt pakketten die eenvoudig Docker op elke configureren [Mac][docker-mac], [Windows][docker-windows], of [Linux] [ docker-linux] system.

Azure Cloud-Shell is dan de Docker-onderdelen die nodig zijn voor het voltooien van elke stap in deze zelfstudie niet opgenomen. U moet de Azure CLI en Docker-ontwikkelomgeving installeren op uw lokale computer om deze zelfstudie te voltooien.

## <a name="get-application-code"></a>Toepassingscode ophalen

Het voorbeeld in deze zelfstudie bevat een eenvoudige webtoepassing die is ingebouwd in [Node.js][nodejs]. De app dient een statische HTML-pagina en ziet er als volgt uit:

![Zelfstudie-app weergegeven in browser][aci-tutorial-app]

Gebruik git om het voorbeeld te downloaden:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-the-container-image"></a>De containerinstallatiekopie bouwen

Het Dockerfile dat is opgegeven in de voorbeeldopslagplaats laat zien hoe de container wordt gebouwd. Het starten van een [officiële Node.js installatiekopie] [ docker-hub-nodeimage] op basis van [Alpine Linux][alpine-linux], een kleine distributie die zeer geschikt voor gebruik met containers. Vervolgens worden de toepassingsbestanden naar de container gekopieerd, afhankelijkheden geïnstalleerd met behulp van het de Node Package Manager, en wordt ten slotte de toepassing gestart.

```Dockerfile
FROM node:8.9.3-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

Gebruik de [docker-build] [ docker-build] opdracht voor het maken van de installatiekopie van de container als tagging *aci-zelfstudie-app*:

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

De uitvoer van de [docker-build] [ docker-build] opdracht is vergelijkbaar met de volgende (afgekapt voor leesbaarheid):

```bash
Sending build context to Docker daemon  119.3kB
Step 1/6 : FROM node:8.9.3-alpine
8.9.3-alpine: Pulling from library/node
88286f41530e: Pull complete
84f3a4bf8410: Pull complete
d0d9b2214720: Pull complete
Digest: sha256:c73277ccc763752b42bb2400d1aaecb4e3d32e3a9dbedd0e49885c71bea07354
Status: Downloaded newer image for node:8.9.3-alpine
 ---> 90f5ee24bee2
...
Step 6/6 : CMD node /usr/src/app/index.js
 ---> Running in f4a1ea099eec
 ---> 6edad76d09e9
Removing intermediate container f4a1ea099eec
Successfully built 6edad76d09e9
Successfully tagged aci-tutorial-app:latest
```

Gebruik de [docker-installatiekopieën] [ docker-images] opdracht om te zien van de installatiekopie van het ingebouwde:

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

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png

<!-- LINKS - External -->
[alpine-linux]: https://alpinelinux.org/
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
[azure-cli-install]: /cli/azure/install-azure-cli
