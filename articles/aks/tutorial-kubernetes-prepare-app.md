---
title: Kubernetes op Azure zelfstudie - App voorbereiden
description: AKS zelfstudie - App voorbereiden
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 60e0feb1e45ac5d9f35eac9667eaf9004d77e86a
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="prepare-application-for-azure-container-service-aks"></a>Voorbereiden van de toepassing Azure Container Service (AKS)

In deze zelfstudie, deel 1 van acht, is een toepassing met meerdere container klaar voor gebruik in Kubernetes. Stappen voltooid omvatten:  

> [!div class="checklist"]
> * Toepassingsbron klonen vanuit GitHub  
> * Maken van een installatiekopie van een container van de toepassingsbron
> * De toepassing testen in een lokale Docker-omgeving

Zodra de stappen zijn voltooid, is de volgende toepassing toegankelijk in uw lokale ontwikkelomgeving.

![Afbeelding van Kubernetes-cluster in Azure](./media/container-service-tutorial-kubernetes-prepare-app/azure-vote.png)

In volgende zelfstudies leert is de installatiekopie van de container geüpload naar een Azure Container Registry, en voer in een cluster AKS.

## <a name="before-you-begin"></a>Voordat u begint

In deze zelfstudie wordt ervan uitgegaan dat u een basiskennis hebt van Docker-kernconcepten zoals containers, containerinstallatiekopieën en Docker-basisopdrachten. Indien nodig, Zie [aan de slag met Docker] [ docker-get-started] voor een primer op de basisprincipes van de container. 

Voor deze zelfstudie hebt u een Docker-ontwikkelomgeving nodig. Docker biedt pakketten die eenvoudig Docker op elke configureren [Mac][docker-for-mac], [Windows][docker-for-windows], of [Linux] [ docker-for-linux] system.

Azure Cloud-Shell is dan de Docker-onderdelen die nodig zijn voor het voltooien van elke stap in deze zelfstudie niet opgenomen. Daarom wordt u aangeraden een volledige Docker-ontwikkelomgeving.

## <a name="get-application-code"></a>Toepassingscode ophalen

De voorbeeldtoepassing gebruikt in deze zelfstudie is een eenvoudige stemmende app. De toepassing bestaat uit een front-onderdeel en een back-end Redis-exemplaar. Het webonderdeel wordt verpakt in een installatiekopie van een aangepaste container. Een installatiekopie van een ongewijzigd van Docker-Hub maakt gebruik van het Redis-exemplaar.  

Git gebruiken voor het downloaden van een kopie van de toepassing op uw ontwikkelomgeving.

```console
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Wijzig de mappen zodat u uit de gekloonde directory werkt.

```console
cd azure-voting-app-redis
```

In de map is de broncode van de toepassing, een vooraf gemaakte Docker compose bestands- en een manifestbestand Kubernetes. Deze bestanden worden gebruikt in de zelfstudie set. 

## <a name="create-container-images"></a>Maken van installatiekopieën van de container

[Docker Compose] [ docker-compose] kan worden gebruikt voor het automatiseren van de build buiten de container-installatiekopieën en de implementatie van toepassingen met meerdere container.

Voer de `docker-compose.yaml` bestand dat u wilt maken van de installatiekopie van de container, de installatiekopie van het Redis downloaden en de toepassing niet starten.

```console
docker-compose up -d
```

Wanneer het voltooid, gebruiken de [docker-installatiekopieën] [ docker-images] opdracht om te zien van de gemaakte afbeeldingen.

```console
docker images
```

U ziet dat drie afbeeldingen gedownload of gemaakt. De `azure-vote-front` installatiekopie bevat van de toepassing en maakt gebruik van de `nginx-flask` installatiekopie als basis. De `redis` installatiekopie wordt gebruikt om een Redis-exemplaar te starten.

```
REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Voer de [docker ps] [ docker-ps] opdracht om te zien van de actieve containers.

```console
docker ps
```

Uitvoer:

```
CONTAINER ID        IMAGE             COMMAND                  CREATED             STATUS              PORTS                           NAMES
82411933e8f9        azure-vote-front  "/usr/bin/supervisord"   57 seconds ago      Up 30 seconds       443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
b68fed4b66b6        redis             "docker-entrypoint..."   57 seconds ago      Up 30 seconds       0.0.0.0:6379->6379/tcp          azure-vote-back
```

## <a name="test-application-locally"></a>Toepassing lokaal testen

Blader naar http://localhost: 8080 om te zien van de toepassing uitgevoerd.

![Afbeelding van Kubernetes-cluster in Azure](./media/container-service-tutorial-kubernetes-prepare-app/azure-vote.png)

## <a name="clean-up-resources"></a>Resources opschonen

Nu dat de functionaliteit van de toepassing is gevalideerd, kunnen de actieve containers worden gestopt en verwijderd. Installatiekopieën van de container niet verwijderen. De `azure-vote-front` afbeelding is geüpload naar een Azure Container register-exemplaar in de volgende zelfstudie.

Voer de volgende als u wilt stoppen van de actieve containers.

```console
docker-compose stop
```

Verwijder de gestopte containers en -resources met de volgende opdracht.

```console
docker-compose down
```

Is voltooid hebt u de installatiekopie van een container waarin de toepassing Azure stem.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt een toepassing is getest en installatiekopieën van de container gemaakt voor de toepassing. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * De toepassingsbron klonen vanuit GitHub  
> * De installatiekopie van een container van toepassingsbron gemaakt
> * De toepassing getest in een lokale Docker-omgeving

Ga verder met de volgende zelfstudie voor meer informatie over het opslaan van installatiekopieën van de container in een Azure Container Registry.

> [!div class="nextstepaction"]
> [Push-installatiekopieën in Azure Container register][aks-tutorial-prepare-acr]

<!-- LINKS - external -->
[docker-compose]: https://docs.docker.com/compose/
[docker-for-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-for-mac]: https://docs.docker.com/docker-for-mac/
[docker-for-windows]: https://docs.docker.com/docker-for-windows/
[docker-get-started]: https://docs.docker.com/get-started/
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-ps]: https://docs.docker.com/engine/reference/commandline/ps/

<!-- LINKS - internal -->
[aks-tutorial-prepare-acr]: ./tutorial-kubernetes-prepare-acr.md