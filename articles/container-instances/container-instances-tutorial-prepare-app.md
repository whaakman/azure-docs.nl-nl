---
title: Een groep met meerdere containers maken met Azure Container Instances | Azure Docs
description: Een groep met meerdere containers maken en deze implementeren in Azure Container Instances vanuit het Azure Container Registry
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: 
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2017
ms.author: seanmck
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: a678700884b612cad6281eb8f3b74ce63a0ebb69
ms.openlocfilehash: b5f16f0c4b15c02bbc0ee8a9fb9424467a39ee4d
ms.contentlocale: nl-nl
ms.lasthandoff: 07/26/2017

---

# <a name="create-container-for-deployment-to-azure-container-instances"></a>Een container voor implementatie in Azure Container Instances maken

Met Azure Container Instances kunt u Docker-containers implementeren in de Azure-infrastructuur zonder virtuele machines in te richten of gebruik te maken van een service op een hoger niveau. In deze zelfstudie maakt u een eenvoudige webtoepassing in Node.js en verpakt u deze in een container die kan worden uitgevoerd met behulp van Azure Container Instances. Het volgende wordt besproken:

> [!div class="checklist"]
> * Toepassingsbron klonen vanuit GitHub  
> * Containerinstallatiekopieën van de toepassingsbron maken
> * De installatiekopieën testen in een lokale Docker-omgeving

In volgende zelfstudies uploadt u uw installatiekopie naar een Azure Container Registry, en implementeert u deze vervolgens in Azure Container Instances.

## <a name="before-you-begin"></a>Voordat u begint

In deze zelfstudie wordt ervan uitgegaan dat u een basiskennis hebt van Docker-kernconcepten zoals containers, containerinstallatiekopieën en Docker-basisopdrachten. Zie, indien nodig, [Aan de slag met Docker]( https://docs.docker.com/get-started/) voor een uitleg van de basisprincipes van containers. 

Voor deze zelfstudie hebt u een Docker-ontwikkelomgeving nodig. Docker biedt pakketten die eenvoudig Docker op elke configureren op elk [Mac](https://docs.docker.com/docker-for-mac/)-, [Windows](https://docs.docker.com/docker-for-windows/)- of [Linux](https://docs.docker.com/engine/installation/#supported-platforms)-systeem.

## <a name="get-application-code"></a>Toepassingscode ophalen

Het voorbeeld in deze zelfstudie bevat een eenvoudige webtoepassing die is gebouwd in [Node.js](http://nodejs.org). De app dient een statische HTML-pagina en ziet er als volgt uit:

![Zelfstudie-app weergegeven in browser][aci-tutorial-app]

Gebruik git om het voorbeeld te downloaden:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-the-container-image"></a>De containerinstallatiekopie bouwen

Het Dockerfile dat is opgegeven in de voorbeeldopslagplaats laat zien hoe de container wordt gebouwd. Het begint met een [officiële Node.js-installatiekopie] [ dockerhub-nodeimage] op basis van [Alpine Linux](https://alpinelinux.org/), een kleine distributie die zeer geschikt voor gebruik met containers. Vervolgens worden de toepassingsbestanden naar de container gekopieerd, afhankelijkheden geïnstalleerd met behulp van het de Node Package Manager, en wordt ten slotte de toepassing gestart.

```
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
> * De toepassingsbron klonen vanuit GitHub  
> * Containerinstallatiekopieën van de toepassingsbron maken
> * De container lokaal testen

Ga verder met de volgende zelfstudie voor meer informatie over het opslaan van installatiekopieën van de container in een Azure Container Registry.

> [!div class="nextstepaction"]
> [Installatiekopieën pushen naar Azure Container Registry](./container-instances-tutorial-prepare-acr.md)

<!-- LINKS -->
[dockerhub-nodeimage]: https://hub.docker.com/r/library/node/tags/8.2.0-alpine/

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png
