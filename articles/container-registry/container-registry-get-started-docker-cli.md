---
title: Docker-installatiekopie naar persoonlijke Azure container registry pushen
description: Docker-installatiekopieën pushen naar en ophalen van een privécontainerregister in Azure met de Docker-CLI
services: container-registry
author: dlepow
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 01/23/2019
ms.author: danlep
ms.custom: seodec18, H1Hack27Feb2017
ms.openlocfilehash: d2aa05d6648a76b02e09c660d7e6f6f30988db53
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57542162"
---
# <a name="push-your-first-image-to-a-private-docker-container-registry-using-the-docker-cli"></a>Uw eerste installatiekopie naar een Docker-containerregister pushen met de Docker-CLI

Een Azure-containerregister slaat persoonlijke [Docker](https://hub.docker.com)-containerinstallatiekopieën op en beheert ze, vergelijkbaar met de manier waarop [Docker Hub](https://hub.docker.com/) openbare Docker-installatiekopieën opslaat. U kunt de [Docker-opdrachtregelinterface](https://docs.docker.com/engine/reference/commandline/cli/) (Docker CLI) voor [aanmelding](https://docs.docker.com/engine/reference/commandline/login/), [push](https://docs.docker.com/engine/reference/commandline/push/), [pull](https://docs.docker.com/engine/reference/commandline/pull/), en andere bewerkingen voor uw container het register.

In de volgende stappen maakt u een officiële downloaden [Nginx-installatiekopie](https://store.docker.com/images/nginx) uit de openbare Docker Hub-register, tag deze voor uw persoonlijke Azure container registry, naar uw register pushen en deze vervolgens te halen uit het register.

## <a name="prerequisites"></a>Vereisten

* **Azure-containerregister**: maak een containerregister in uw Azure-abonnement. Gebruik bijvoorbeeld de [Azure-portal](container-registry-get-started-portal.md) of de [Azure CLI](container-registry-get-started-azure-cli.md).
* **Docker CLI** -u moet ook Docker lokaal zijn geïnstalleerd. Docker biedt pakketten die eenvoudig Docker configureren op elke [macOS] [docker-mac], [Windows] [docker-windows] of [Linux] [docker-linux]-systeem.

## <a name="log-in-to-a-registry"></a>Aanmelden bij een register

Er zijn [verschillende manieren om te verifiëren](container-registry-authentication.md) naar uw persoonlijke container registry. De aanbevolen methode als u werkt in een opdrachtregel doen is met de Azure CLI-opdracht [az acr login](/cli/azure/acr?view=azure-cli-latest#az-acr-login). Bijvoorbeeld, zich aanmelden bij een register met de naam *myregistry*:

```azurecli
az acr login --name myregistry
```

U kunt zich ook aanmelden met [dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/). Bijvoorbeeld, u mogelijk [toegewezen van een service-principal](container-registry-authentication.md#service-principal) naar het register voor een automation-scenario. Wanneer u de volgende opdracht uitvoert, geeft u interactief de service-principal appID (gebruikersnaam) en het wachtwoord wanneer hierom wordt gevraagd. Zie voor aanbevolen procedures voor het beheren van referenties voor clusteraanmelding de [dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/) opdrachten:

```
docker login myregistry.azurecr.io
```

Beide opdrachten retourneren `Login Succeeded` nadat deze is voltooid.

> [!TIP]
> Geef de volledig gekwalificeerde registernaam (zonder hoofdletters) altijd bij het gebruik `docker login` en wanneer het taggen van installatiekopieën voor pushen naar uw register. In de voorbeelden in dit artikel is de volledig gekwalificeerde naam is *myregistry.azurecr.io*.

## <a name="pull-the-official-nginx-image"></a>Haal de officiële Nginx-installatiekopie

Haal eerst, de openbare Nginx-installatiekopie naar uw lokale computer.

```
docker pull nginx
```

## <a name="run-the-container-locally"></a>De container lokaal uitvoeren

Uitvoeren van volgende [docker uitvoeren](https://docs.docker.com/engine/reference/run/) opdracht een lokaal exemplaar van de Nginx-container interactief gestart (`-it`) op poort 8080. De `--rm` argument geeft aan dat de container moet worden verwijderd wanneer u deze stoppen.

```
docker run -it --rm -p 8080:80 nginx
```

Blader naar [ http://localhost:8080 ](http://localhost:8080) om de standaardwebpagina bediend door Nginx in de actieve container weer te geven. U ziet een pagina zoals in het volgende:

![Nginx op lokale computer](./media/container-registry-get-started-docker-cli/nginx.png)

Omdat u de container interactief met gestart `-it`, ziet u de Nginx-server op de opdrachtregel-uitvoer na het navigeren naar het in uw browser.

Als u wilt stoppen en verwijderen van de container, drukt u op `Control` + `C`.

## <a name="create-an-alias-of-the-image"></a>Een alias van de installatiekopie maken

Gebruik [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) om te maken van een alias van de installatiekopie met de volledig gekwalificeerde pad naar het register. In dit voorbeeld wordt de naamruimte `samples` gespecificeerd om overbodige items in de hoofdmap van het register te voorkomen.

```
docker tag nginx myregistry.azurecr.io/samples/nginx
```

Zie voor meer informatie over het taggen met naamruimten het [opslagplaatsnaamruimten](container-registry-best-practices.md#repository-namespaces) sectie van [aanbevolen procedures voor Azure Container Registry](container-registry-best-practices.md).

## <a name="push-the-image-to-your-registry"></a>De installatiekopie naar uw register pushen

Nu dat u hebt de installatiekopie met de volledig gekwalificeerde pad naar uw privéregister gelabeld, kunt u deze naar het register met pushen [docker push](https://docs.docker.com/engine/reference/commandline/push/):

```
docker push myregistry.azurecr.io/samples/nginx
```

## <a name="pull-the-image-from-your-registry"></a>Haal de installatiekopie van het register

Gebruik de [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) opdracht voor het ophalen van de installatiekopie van het register:

```
docker pull myregistry.azurecr.io/samples/nginx
```

## <a name="start-the-nginx-container"></a>De Nginx-container starten

Gebruik de [docker uitvoeren](https://docs.docker.com/engine/reference/run/) opdracht om uit te voeren van de installatiekopie die u hebt opgehaald uit het register:

```
docker run -it --rm -p 8080:80 myregistry.azurecr.io/samples/nginx
```

Blader naar [ http://localhost:8080 ](http://localhost:8080) om de actieve container weer te geven.

Als u wilt stoppen en verwijderen van de container, drukt u op `Control` + `C`.

## <a name="remove-the-image-optional"></a>Verwijder de installatiekopie (optioneel)

Als u de Nginx-installatiekopie niet meer nodig hebt, kunt u het verwijderen lokaal met de [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) opdracht.

```
docker rmi myregistry.azurecr.io/samples/nginx
```

Als u wilt verwijderen van installatiekopieën van het Azure container registry, kunt u de Azure CLI-opdracht [az acr repository delete](/cli/azure/acr/repository#az-acr-repository-delete). Bijvoorbeeld de volgende opdracht verwijdert u het manifest waarnaar wordt verwezen door de `samples/nginx:latest` tag, alle unieke layer-gegevens en alle andere codes die verwijst naar het manifest.

```azurecli
az acr repository delete --name myregistry --image samples/nginx:latest
```

## <a name="next-steps"></a>Volgende stappen

Nu dat u de basisprincipes kent, bent u klaar om te beginnen met behulp van het register. Bijvoorbeeld, containerinstallatiekopieën uit het register te implementeren:

* [Azure Kubernetes Service (AKS)](../aks/tutorial-kubernetes-prepare-app.md)
* [Azure Container Instances](../container-instances/container-instances-tutorial-prepare-app.md)
* [Service Fabric](../service-fabric/service-fabric-tutorial-create-container-images.md)
