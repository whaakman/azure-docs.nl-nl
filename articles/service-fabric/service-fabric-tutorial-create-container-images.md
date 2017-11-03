---
title: "Maken van installatiekopieën van de container voor Azure Service Fabric | Microsoft Docs"
description: "Informatie over het maken van installatiekopieën van de container voor een Service Fabric-toepassing met meerdere container."
services: service-fabric
documentationcenter: 
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: Docker, Containers, Microservices, Service Fabric, Azure
ms.assetid: 
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/15/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 08b3cc4a52c09ee03818b563794ef9b009d12ef4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-container-images-for-service-fabric"></a>Maken van installatiekopieën van de container voor Service Fabric

Deze zelfstudie maakt deel uit een van een zelfstudie serie die laat zien hoe u containers in Linux Service Fabric-cluster. In deze zelfstudie is een toepassing met meerdere container klaar voor gebruik met Service Fabric. In volgende zelfstudies worden deze installatiekopieën gebruikt als onderdeel van een Service Fabric-toepassing. In deze zelfstudie leert u het volgende: 

> [!div class="checklist"]
> * Toepassing kloonbron vanuit GitHub  
> * Een installatiekopie van een container van de toepassingsbron maken
> * Implementeer een exemplaar van Azure Container register (ACR)
> * Een installatiekopie van een container voor ACR tag
> * Upload de installatiekopie naar ACR

In deze zelfstudie reeks leert u hoe: 

> [!div class="checklist"]
> * Maken van installatiekopieën van de container voor Service Fabric
> * [Bouwen en uitvoeren van een Service Fabric-toepassing met Containers](service-fabric-tutorial-package-containers.md)
> * [Hoe failover en schalen in Service Fabric worden verwerkt](service-fabric-tutorial-containers-failover.md)

## <a name="prerequisites"></a>Vereisten

- Linux-ontwikkelomgeving instellen voor Service Fabric. Volg de instructies [hier](service-fabric-get-started-linux.md) voor het instellen van uw Linux-omgeving. 
- Deze zelfstudie vereist dat u de Azure CLI versie 2.0.4 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 
- Bovendien is het vereist dat u een Azure-abonnement beschikbaar hebt. Ga voor meer informatie over een gratis evaluatieversie [hier](https://azure.microsoft.com/free/).

## <a name="get-application-code"></a>Toepassingscode ophalen

De voorbeeldtoepassing gebruikt in deze zelfstudie is een stemmende app. De toepassing bestaat uit een front-onderdeel en een back-end Redis-exemplaar. De onderdelen worden geleverd in container-installatiekopieën. 

Git gebruiken voor het downloaden van een kopie van de toepassing op uw ontwikkelomgeving.

```bash
git clone https://github.com/Azure-Samples/service-fabric-dotnet-containers.git

cd service-fabric-dotnet-containers/Linux/container-tutorial/
```

De container-tutorial-map bevat een map met de naam 'azure-stem'. Deze map 'azure-stem' bevat de broncode van de front-end- en een Dockerfile om de front-end samen te stellen. De container-tutorial-map bevat ook de map 'redis' waarvoor de Dockerfile het redis-image. Deze mappen bevatten de benodigde activa voor deze zelfstudie. 

## <a name="create-container-images"></a>Maken van installatiekopieën van de container

Voer de volgende opdracht voor het bouwen van de afbeelding voor de web-front-component in de map 'azure-stem'. Met deze opdracht maakt gebruik van de Dockerfile in deze map voor het bouwen van de installatiekopie. 

```bash
docker build -t azure-vote-front .
```

In de map 'redis' en voer de volgende opdracht voor het bouwen van de afbeelding voor de redis-back-end. Met deze opdracht maakt gebruik van de Dockerfile in de map voor het bouwen van de installatiekopie. 

```bash
docker build -t azure-vote-back .
```

Wanneer het voltooid, gebruiken de [docker-installatiekopieën](https://docs.docker.com/engine/reference/commandline/images/) opdracht om te zien van de gemaakte afbeeldingen.

```bash
docker images
```

U ziet dat vier afbeeldingen gedownload of gemaakt. De *azure stem voorgrond* installatiekopie van de toepassing bevat. Deze is afgeleid van een *python* afbeelding van Docker-Hub. De installatiekopie van het Redis is van Docker-Hub gedownload.

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
azure-vote-back              latest              bf9a858a9269        3 seconds ago        107MB
azure-vote-front             latest              052c549a75bf        About a minute ago   708MB
redis                        latest              9813a7e8fcc0        2 days ago           107MB
tiangolo/uwsgi-nginx-flask   python3.6           590e17342131        5 days ago           707MB

```

## <a name="deploy-azure-container-registry"></a>Register met Azure Container implementeren

Voer de [az aanmelding](/cli/azure/login) opdracht zich aanmelden bij uw Azure-account. 

Gebruik vervolgens de [az account](/cli/azure/account#set) opdracht voor het kiezen van uw abonnement te maken van het register van de Azure-Container. 

```bash
az account set --subscription <subscription_id>
```

Wanneer u een Azure Container Registry implementeert, moet u eerst een resourcegroep. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create). In dit voorbeeld wordt een resourcegroep met de naam *myResourceGroup* wordt gemaakt in de *westus* regio. Kies de resourcegroep in een regio in de buurt. 

```bash
az group create --name myResourceGroup --location westus
```

Maken van een Azure-Container register met de [az acr maken](/cli/azure/acr#create) opdracht. De naam van een Container register **moeten uniek zijn**.

```bash
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic --admin-enabled true
```

In de rest van deze zelfstudie gebruiken we 'acrname' als tijdelijke aanduiding voor de container register-naam die u hebt gekozen.

## <a name="log-in-to-your-container-registry"></a>Meld u aan het register van de container bij

Meld u aan bij uw ACR-exemplaar voordat u installatiekopieën aan. Gebruik de [az acr aanmelding](/cli/azure/acr?view=azure-cli-latest#az_acr_login) opdracht om de bewerking te voltooien. Geef de unieke naam krijgen tot het register van de container wanneer deze is gemaakt.

```bash
az acr login --name <acrName>
```

De opdracht retourneert een bericht 'Aanmelding geslaagd' eenmaal is voltooid.

## <a name="tag-container-images"></a>Installatiekopieën van de tag-container

Elke container installatiekopie moet worden gemarkeerd met de naam loginServer van het register. Deze code wordt gebruikt voor routering wanneer container afbeeldingen met een installatiekopie-register worden gepusht.

Als een lijst met huidige afbeeldingen wilt weergeven, gebruikt de [docker-installatiekopieën](https://docs.docker.com/engine/reference/commandline/images/) opdracht.

```bash
docker images
```

Uitvoer:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
azure-vote-back              latest              bf9a858a9269        3 seconds ago        107MB
azure-vote-front             latest              052c549a75bf        About a minute ago   708MB
redis                        latest              9813a7e8fcc0        2 days ago           107MB
tiangolo/uwsgi-nginx-flask   python3.6           590e17342131        5 days ago           707MB
```

Als u de naam van de loginServer, voer de volgende opdracht:

```bash
az acr show --name <acrName> --query loginServer --output table
```

Tag nu de *azure stem voorgrond* installatiekopie met de loginServer van het register van de container. Ook toe te voegen `:v1` aan het einde van de naam van de installatiekopie. Deze code geeft de versie van de installatiekopie.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Tag vervolgens de *azure-stem-back* installatiekopie met de loginServer van het register van de container. Ook toe te voegen `:v1` aan het einde van de naam van de installatiekopie. Deze code geeft de versie van de installatiekopie.

```bash
docker tag azure-vote-back <acrLoginServer>/azure-vote-back:v1
```

Wanneer gecodeerd, docker-installatiekopieën uitvoeren om te controleren of de bewerking.


Uitvoer:

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
azure-vote-back                        latest              bf9a858a9269        22 minutes ago      107MB
<acrName>.azurecr.io/azure-vote-back    v1                  bf9a858a9269        22 minutes ago      107MB
azure-vote-front                       latest              052c549a75bf        23 minutes ago      708MB
<acrName>.azurecr.io/azure-vote-front   v1                  052c549a75bf        23 minutes ago      708MB
redis                                  latest              9813a7e8fcc0        2 days ago          107MB
tiangolo/uwsgi-nginx-flask             python3.6           590e17342131        5 days ago          707MB

```

## <a name="push-images-to-registry"></a>Push-installatiekopieën in register

Push de *azure stem voorgrond* afbeelding in het register. 

In het volgende voorbeeld vervangen door de naam van de loginServer ACR de loginServer uit uw omgeving.

```bash
docker push <acrLoginServer>/azure-vote-front:v1
```

Push de *azure-stem-back* afbeelding in het register. 

In het volgende voorbeeld vervangen door de naam van de loginServer ACR de loginServer uit uw omgeving.

```bash
docker push <acrLoginServer>/azure-vote-back:v1
```

De docker-push-opdrachten nemen een aantal minuten in beslag.

## <a name="list-images-in-registry"></a>Lijst met afbeeldingen in register

U kunt een lijst met afbeeldingen die een pushbericht hebben ontvangen in het register van uw Azure-Container gebruiken de [az acr opslagplaats lijst](/cli/azure/acr/repository#list) opdracht. De opdracht bijwerken met de naam van het ACR-exemplaar.

```bash
az acr repository list --name <acrName> --output table
```

Uitvoer:

```bash
Result
----------------
azure-vote-back
azure-vote-front
```

Bij zelfstudie voltooien is de container-installatiekopie opgeslagen in een persoonlijke Azure-Container register-exemplaar. Deze installatiekopie wordt geïmplementeerd vanaf ACR naar een Service Fabric-cluster in volgende zelfstudies.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie is een toepassing is opgehaald uit Github en container afbeeldingen zijn gemaakt en naar een register gepusht. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * Toepassing kloonbron vanuit GitHub  
> * Een installatiekopie van een container van de toepassingsbron maken
> * Implementeer een exemplaar van Azure Container register (ACR)
> * Een installatiekopie van een container voor ACR tag
> * Upload de installatiekopie naar ACR

Ga naar de volgende zelfstudie voor meer informatie over pakketten containers in een Service Fabric-toepassing met behulp van Yeoman. 

> [!div class="nextstepaction"]
> [Het pakket en containers implementeren als een Service Fabric-toepassing](service-fabric-tutorial-package-containers.md)
