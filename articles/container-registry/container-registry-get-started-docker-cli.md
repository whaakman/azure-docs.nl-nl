---
title: "Docker-installatiekopieën in Azure Container Registry | Microsoft Azure"
description: "Docker-installatiekopieën pushen naar en ophalen van een Azure-containerregister met de Docker-CLI"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: 64fbe43f-fdde-4c17-a39a-d04f2d6d90a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2016
ms.author: stevelas
translationtype: Human Translation
ms.sourcegitcommit: f299cff22d00a1c765a32838647818d18f3df85d
ms.openlocfilehash: df15eebf0052aa4713263a810df605fa1016c306

---
# <a name="push-your-first-image-to-a-container-registry-using-the-docker-cli"></a>Uw eerste installatiekopie naar een containerregister pushen met de Docker-CLI
Een Azure-containerregister slaat persoonlijke [Docker](http://hub.docker.com)-containerinstallatiekopieën op en beheert ze, vergelijkbaar met de manier waarop [Docker Hub](https://hub.docker.com/) openbare Docker-installatiekopieën opslaat. U gebruikt de [Docker-Command-Line Interface](https://docs.docker.com/engine/reference/commandline/cli/) (Docker-CLI) voor [aanmelden](https://docs.docker.com/engine/reference/commandline/login/), [pushen](https://docs.docker.com/engine/reference/commandline/push/), [ophalen](https://docs.docker.com/engine/reference/commandline/pull/) en andere bewerkingen voor uw containerregister. 

Zie voor meer achtergrondinformatie en concepten [Wat is Azure Container Registry?](container-registry-intro.md)


> [!NOTE]
> Container Registry is momenteel in de preview-versie.
> 
> 

## <a name="prerequisites"></a>Vereisten
* **Azure-containerregister**: maak een containerregister in uw Azure-abonnement. Gebruik bijvoorbeeld [Azure Portal](container-registry-get-started-portal.md) of de [Azure-CLI 2.0 (Preview)](container-registry-get-started-azure-cli.md).
* **Docker-CLI**: installeer de [Docker-engine](https://docs.docker.com/engine/installation/) om uw lokale computer als een Docker-host in te stellen en de Docker-CLI-opdrachten te gebruiken.

## <a name="log-in-to-a-registry"></a>Aanmelden bij een register
Voer `docker login` uit om u bij uw containerregister aan te melden met uw [registerreferenties](container-registry-authentication.md).

In het volgende voorbeeld worden de id en het wachtwoord van een [service-principal](../active-directory/active-directory-application-objects.md) van Azure Active Directory doorgegeven. U hebt bijvoorbeeld een service-principal aan uw register toegewezen voor een automatiseringsscenario. 

```
docker login myregistry-contoso.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

> [!TIP]
> Geef de registernaam op die aan alle vereisten voldoet (zonder hoofdletters). In dit voorbeeld is het `myregistry-contoso.azurecr.io`.

## <a name="steps-to-pull-and-push-an-image"></a>Stappen voor ophalen en pushen van een installatiekopie
In het volgende voorbeeld wordt een Nginx-installatiekopie gedownload uit het openbare Docker Hub-register, getagd voor uw persoonlijke Azure-containerregister, gepusht naar uw register en vervolgens weer opgehaald.

**1. De officiële Docker-installatiekopie ophalen voor Nginx**

Haal eerst de openbare Nginx-installatiekopie op naar uw lokale computer.

```
docker pull nginx
```
**2. De Nginx-container starten**

Met de volgende opdracht wordt de lokale Nginx-container interactief gestart (zodat u uitvoer van Nginx kunt zien) en wordt luisteren via poort 8080 gestart. De container die wordt uitgevoerd, wordt verwijderd zodra deze is gestopt.

```
docker run -it --rm -p 8080:80 nginx
```

Ga naar [http://localhost:8080](http://localhost:8080) om de container die wordt uitgevoerd, weer te geven. Er wordt een venster zoals het volgende weergegeven.

![Nginx op lokale computer](./media/container-registry-get-started-docker-cli/nginx.png)

Druk op [CTRL]+[C] om de container die wordt uitgevoerd, te stoppen.

**3. Een alias van de installatiekopie in uw register maken**

Met de volgende opdracht maakt u een alias van de installatiekopie met een volledig gekwalificeerd pad naar uw register. In dit voorbeeld wordt de naamruimte `samples` gespecificeerd om overbodige items in de hoofdmap van het register te voorkomen.

```
docker tag nginx myregistry-exp.azurecr.io/samples/nginx
```  

**4. De installatiekopie naar uw register pushen**

```
docker push myregistry-contoso.azurecr.io/samples/nginx
``` 

**5. De installatiekopie vanuit uw register ophalen**

```
docker pull myregistry-contoso.azurecr.io/samples/nginx
``` 

**6. De Nginx-container vanuit uw register starten**

```
docker run -it --rm -p 8080:80 myregistry-exp.azurecr.io/samples/nginx
```

Ga naar [http://localhost:8080](http://localhost:8080) om de container die wordt uitgevoerd, weer te geven.

Druk op [CTRL]+[C] om de container die wordt uitgevoerd, te stoppen.

**6. De installatiekopie verwijderen**

```
docker rmi myregistry-contoso.azurecr.io/samples/nginx
```



## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes kent, kunt u uw register gaan gebruiken. Begin bijvoorbeeld met het implementeren van containerinstallatiekopieën op een [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/)-cluster.






<!--HONumber=Jan17_HO4-->


