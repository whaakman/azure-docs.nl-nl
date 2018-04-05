---
title: 'Zelfstudie: Kubernetes in Azure - app voorbereiden'
description: 'Zelfstudie: AKS - app voorbereiden'
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 8a2c2e53ed04cf00cc02135c5e5f82ded18fc2bc
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="tutorial-prepare-application-for-azure-container-service-aks"></a>Zelfstudie: toepassing voorbereiden voor AKS (Azure Container Service)

In deze zelfstudie, deel een van acht, wordt een toepassing met meerdere containers voorbereid voor gebruik in Kubernetes. Dit zijn de uitgevoerde stappen:  

> [!div class="checklist"]
> * Toepassingsbron klonen vanuit GitHub  
> * Een containerinstallatiekopie maken uit de toepassingsbron
> * De toepassing testen in een lokale Docker-omgeving

Na voltooiing is de volgende toepassing toegankelijk in uw lokale ontwikkelomgeving.

![Afbeelding van Kubernetes-cluster in Azure](./media/container-service-tutorial-kubernetes-prepare-app/azure-vote.png)

In de volgende zelfstudies wordt de containerinstallatiekopie geüpload naar een Azure Container Registry, en vervolgens uitgevoerd in een AKS-cluster.

## <a name="before-you-begin"></a>Voordat u begint

In deze zelfstudie wordt ervan uitgegaan dat u een basiskennis hebt van Docker-kernconcepten zoals containers, containerinstallatiekopieën en Docker-basisopdrachten. Zie, indien nodig, [Aan de slag met Docker][docker-get-started] voor een uitleg van de basisprincipes van containers. 

Voor deze zelfstudie hebt u een Docker-ontwikkelomgeving nodig. Docker biedt pakketten die eenvoudig Docker configureren op elk [Mac][docker-for-mac]-, [Windows][docker-for-windows]- of [Linux][docker-for-linux]-systeem.

Azure Cloud Shell bevat niet de vereiste Docker-onderdelen die nodig zijn om elke stap in deze zelfstudie te voltooien. Daarom raden wij u aan een volledige Docker-ontwikkelomgeving te gebruiken.

## <a name="get-application-code"></a>Toepassingscode ophalen

De voorbeeldtoepassing die wordt gebruikt in deze zelfstudie, is een eenvoudige stem-app. De toepassing bestaat uit een front-endwebonderdeel en een back-end-Redis-exemplaar. Het webonderdeel is verpakt in een aangepaste containerinstallatiekopie. Het Redis-exemplaar gebruikt een ongewijzigde installatiekopie van Docker Hub.  

Gebruik git om een kopie van de toepassing te downloaden naar de ontwikkelomgeving.

```console
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Wijzig de mappen zodat u vanuit de gekloonde map werkt.

```console
cd azure-voting-app-redis
```

In de map bevinden zich de broncode van de toepassing, een vooraf gemaakt Docker Compose-bestand en een Kubernetes-manifestbestand. Deze bestanden worden gebruikt in de alle delen van de zelfstudie. 

## <a name="create-container-images"></a>Containerinstallatiekopieën maken

[Docker Compose][docker-compose] kan worden gebruikt om de build te automatiseren op basis van containerinstallatiekopieën en de implementatie van toepassingen met meerdere containers.

Voer het bestand `docker-compose.yaml` uit om de containerinstallatiekopie te maken, download de Redis-installatiekopie en start de toepassing.

```console
docker-compose up -d
```

Wanneer dit is voltooid, gebruikt u de opdracht [docker images][docker-images] om de gemaakte installatiekopieën te bekijken.

```console
docker images
```

U ziet dat er drie installatiekopieën zijn gedownload of gemaakt. De `azure-vote-front`-installatiekopie bevat de toepassing en gebruikt de `nginx-flask`-installatiekopie als basis. De `redis`-installatiekopie wordt gebruikt om een Redis-exemplaar te starten.

```
REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Voer de opdracht [docker ps][docker-ps] uit om de actieve containers te zien.

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

Blader naar http://localhost:8080 om de actieve toepassing te zien.

![Afbeelding van Kubernetes-cluster in Azure](./media/container-service-tutorial-kubernetes-prepare-app/azure-vote.png)

## <a name="clean-up-resources"></a>Resources opschonen

Nu de functionaliteit van de toepassing is gevalideerd, kunnen de actieve containers worden gestopt en verwijderd. Verwijder de containerinstallatiekopieën niet. De `azure-vote-front`-installatiekopie wordt in de volgende zelfstudie geüpload naar een Azure Container Registry-exemplaar.

Voer de volgende opdracht uit om de actieve containers te stoppen.

```console
docker-compose stop
```

Verwijder de gestopte containers en resources met de volgende opdracht.

```console
docker-compose down
```

Na voltooiing hebt u een containerinstallatiekopie die de Azure Vote-toepassing bevat.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie is een toepassing getest en zijn containerinstallatiekopieën gemaakt voor de toepassing. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * De toepassingsbron klonen vanuit GitHub  
> * Containerinstallatiekopie maken uit de toepassingsbron
> * De toepassing testen in een lokale Docker-omgeving

Ga verder met de volgende zelfstudie voor meer informatie over het opslaan van installatiekopieën van de container in een Azure Container Registry.

> [!div class="nextstepaction"]
> [Installatiekopieën naar Azure Container Registry pushen][aks-tutorial-prepare-acr]

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