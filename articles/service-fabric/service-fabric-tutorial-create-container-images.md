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
ms.openlocfilehash: 9ea5be818cfc104c243ce31cc0e2d0f10135259f
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
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
git clone https://github.com/Azure-Samples/service-fabric-containers.git

cd service-fabric-containers/Linux/container-tutorial/
```

De oplossing bevat twee mappen en een ' docker-compse.yml' bestand. De map 'azure-stem' bevat de Python frontend service samen met de Dockerfile gebruikt voor het samenstellen van de installatiekopie. De map 'Voting' bevat het Service Fabric-toepassingspakket dat is geïmplementeerd op het cluster. Deze mappen bevatten de benodigde activa voor deze zelfstudie.  

## <a name="create-container-images"></a>Maken van installatiekopieën van de container

In de **azure stem** map en voer de volgende opdracht voor het bouwen van de afbeelding voor de web-front-component. Met deze opdracht maakt gebruik van de Dockerfile in deze map voor het bouwen van de installatiekopie. 

```bash
docker build -t azure-vote-front .
```

Met deze opdracht kan enige tijd duren, omdat de vereiste afhankelijkheden moeten worden opgehaald uit de Docker-Hub. Wanneer het voltooid, gebruiken de [docker-installatiekopieën](https://docs.docker.com/engine/reference/commandline/images/) opdracht om te zien van de gemaakte afbeeldingen.

```bash
docker images
```

U ziet dat twee installatiekopieën gedownload of gemaakt. De *azure stem voorgrond* installatiekopie van de toepassing bevat. Deze is afgeleid van een *python* afbeelding van Docker-Hub.

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
azure-vote-front             latest              052c549a75bf        About a minute ago   708MB
tiangolo/uwsgi-nginx-flask   python3.6           590e17342131        5 days ago           707MB

```

## <a name="deploy-azure-container-registry"></a>Register met Azure Container implementeren

Voer de **az aanmelding** opdracht zich aanmelden bij uw Azure-account. 

```bash
az login
```

Gebruik vervolgens de **az account** opdracht voor het kiezen van uw abonnement te maken van het register van de Azure-Container. U moet de abonnement-ID van uw Azure-abonnement in plaats van < subscription_id > invoeren. 

```bash
az account set --subscription <subscription_id>
```

Wanneer u een Azure Container Registry implementeert, moet u eerst een resourcegroep. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

Een resourcegroep maken met de opdracht **az group create**. In dit voorbeeld wordt een resourcegroep met de naam *myResourceGroup* wordt gemaakt in de *westus* regio.

```bash
az group create --name <myResourceGroup> --location westus
```

Maken van een Azure-Container register met de **az acr maken** opdracht. Vervang \<acrName > met de naam van de container-registersleutel die u in uw abonnement wilt maken. Deze naam moet alfanumerieke en uniek zijn. 

```bash
az acr create --resource-group <myResourceGroup> --name <acrName> --sku Basic --admin-enabled true
```

In de rest van deze zelfstudie gebruiken we 'acrName' als tijdelijke aanduiding voor de container register-naam die u hebt gekozen. Controleer deze waarde. 

## <a name="log-in-to-your-container-registry"></a>Meld u aan het register van de container bij

Meld u aan bij uw ACR-exemplaar voordat u installatiekopieën aan. Gebruik de **az acr aanmelding** opdracht om de bewerking te voltooien. Geef de unieke naam krijgen tot het register van de container wanneer deze is gemaakt.

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
azure-vote-front             latest              052c549a75bf        About a minute ago   708MB
tiangolo/uwsgi-nginx-flask   python3.6           590e17342131        5 days ago           707MB
```

Als u de naam van de loginServer, voer de volgende opdracht:

```bash
az acr show --name <acrName> --query loginServer --output table
```

Hiermee wordt een tabel met de volgende resultaten. Dit resultaat wordt gebruikt voor het label uw **azure stem voorgrond** afbeelding voordat u deze in het register van de container in de volgende stap.

```bash
Result
------------------
<acrName>.azurecr.io
```

Tag nu de *azure stem voorgrond* installatiekopie met de loginServer van het register van de container. Ook toe te voegen `:v1` aan het einde van de naam van de installatiekopie. Deze code geeft de versie van de installatiekopie.

```bash
docker tag azure-vote-front <acrName>.azurecr.io/azure-vote-front:v1
```

Wanneer gecodeerd, docker-installatiekopieën uitvoeren om te controleren of de bewerking.


Uitvoer:

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                       latest              052c549a75bf        23 minutes ago      708MB
<acrName>.azurecr.io/azure-vote-front   v1                  052c549a75bf       23 minutes ago      708MB
tiangolo/uwsgi-nginx-flask             python3.6           590e17342131        5 days ago          707MB

```

## <a name="push-images-to-registry"></a>Push-installatiekopieën in register

Push de *azure stem voorgrond* afbeelding in het register. 

In het volgende voorbeeld vervangen door de naam van de loginServer ACR de loginServer uit uw omgeving.

```bash
docker push <acrName>.azurecr.io/azure-vote-front:v1
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