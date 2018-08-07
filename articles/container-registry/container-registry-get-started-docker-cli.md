---
title: Docker-installatiekopie pushen naar Azure privéregister
description: Docker-installatiekopieën pushen naar en ophalen van een privécontainerregister in Azure met de Docker-CLI
services: container-registry
author: stevelas
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 11/29/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 24cccd4745d611196046168f0125e7ef2a184e15
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576488"
---
# <a name="push-your-first-image-to-a-private-docker-container-registry-using-the-docker-cli"></a>Uw eerste installatiekopie naar een Docker-containerregister pushen met de Docker-CLI

Een Azure-containerregister slaat persoonlijke [Docker](http://hub.docker.com)-containerinstallatiekopieën op en beheert ze, vergelijkbaar met de manier waarop [Docker Hub](https://hub.docker.com/) openbare Docker-installatiekopieën opslaat. U kunt de [Docker-opdrachtregelinterface](https://docs.docker.com/engine/reference/commandline/cli/) (Docker CLI) voor [aanmelding](https://docs.docker.com/engine/reference/commandline/login/), [push](https://docs.docker.com/engine/reference/commandline/push/), [pull](https://docs.docker.com/engine/reference/commandline/pull/), en andere bewerkingen voor uw container het register.

In de volgende stappen maakt u een officiële downloaden [Nginx-installatiekopie](https://store.docker.com/images/nginx) uit de openbare Docker Hub-register, tag deze voor uw persoonlijke Azure container registry, naar uw register pushen en deze vervolgens te halen uit het register.

## <a name="prerequisites"></a>Vereisten

* **Azure-containerregister**: maak een containerregister in uw Azure-abonnement. Gebruik bijvoorbeeld de [Azure-portal](container-registry-get-started-portal.md) of de [Azure CLI](container-registry-get-started-azure-cli.md).
* **Docker CLI** : als u wilt instellen van uw lokale computer als een Docker-host en toegang tot de Docker CLI-opdrachten, installeert u [Docker](https://docs.docker.com/engine/installation/).

## <a name="log-in-to-a-registry"></a>Aanmelden bij een register

Er zijn [verschillende manieren om te verifiëren](container-registry-authentication.md) naar uw persoonlijke container registry. De aanbevolen methode als u werkt in een opdrachtregel doen is met de Azure CLI-opdracht [az acr login](/cli/azure/acr?view=azure-cli-latest#az-acr-login). Bijvoorbeeld, zich aanmelden bij een register met de naam *myregistry*:

```azurecli
az acr login --name myregistry
```

U kunt zich ook aanmelden met [dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/). In het volgende voorbeeld worden de id en het wachtwoord van een [service-principal](../active-directory/develop/app-objects-and-service-principals.md) van Azure Active Directory doorgegeven. Bijvoorbeeld, u mogelijk [toegewezen van een service-principal](container-registry-authentication.md#service-principal) naar het register voor een automation-scenario.

```Bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Beide opdrachten retourneren `Login Succeeded` nadat deze is voltooid. Als u `docker login`, ziet u mogelijk ook een beveiligingswaarschuwing geadviseerd het gebruik van de `--password-stdin` parameter. Hoewel buiten het bestek van dit artikel, wordt u deze best practice aangeraden. Zie voor meer informatie de [dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/) opdrachten.

> [!TIP]
> Geef de volledig gekwalificeerde registernaam (zonder hoofdletters) altijd bij het gebruik `docker login` en wanneer het taggen van installatiekopieën voor pushen naar uw register. In de voorbeelden in dit artikel is de volledig gekwalificeerde naam is *myregistry.azurecr.io*.

## <a name="pull-the-official-nginx-image"></a>Haal de officiële Nginx-installatiekopie

Haal eerst, de openbare Nginx-installatiekopie naar uw lokale computer.

```Bash
docker pull nginx
```

## <a name="run-the-container-locally"></a>De container lokaal uitvoeren

Uitvoeren van volgende [docker uitvoeren](https://docs.docker.com/engine/reference/run/) opdracht een lokaal exemplaar van de Nginx-container interactief gestart (`-it`) op poort 8080. De `--rm` argument geeft aan dat de container moet worden verwijderd wanneer u deze stoppen.

```Bash
docker run -it --rm -p 8080:80 nginx
```

Blader naar [ http://localhost:8080 ](http://localhost:8080) om de standaardwebpagina bediend door Nginx in de actieve container weer te geven. U ziet een pagina zoals in het volgende:

![Nginx op lokale computer](./media/container-registry-get-started-docker-cli/nginx.png)

Omdat u de container interactief met gestart `-it`, ziet u de Nginx-server op de opdrachtregel-uitvoer na het navigeren naar het in uw browser.

Als u wilt stoppen en verwijderen van de container, drukt u op `Control` + `C`.

## <a name="create-an-alias-of-the-image"></a>Een alias van de installatiekopie maken

Gebruik [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) om te maken van een alias van de installatiekopie met de volledig gekwalificeerde pad naar het register. In dit voorbeeld wordt de naamruimte `samples` gespecificeerd om overbodige items in de hoofdmap van het register te voorkomen.

```Bash
docker tag nginx myregistry.azurecr.io/samples/nginx
```

Zie voor meer informatie over het taggen met naamruimten het [opslagplaatsnaamruimten](container-registry-best-practices.md#repository-namespaces) sectie van [aanbevolen procedures voor Azure Container Registry](container-registry-best-practices.md).

## <a name="push-the-image-to-your-registry"></a>De installatiekopie naar uw register pushen

Nu dat u hebt de installatiekopie met de volledig gekwalificeerde pad naar uw privéregister gelabeld, kunt u deze naar het register met pushen [docker push](https://docs.docker.com/engine/reference/commandline/push/):

```Bash
docker push myregistry.azurecr.io/samples/nginx
```

## <a name="pull-the-image-from-your-registry"></a>Haal de installatiekopie van het register

Gebruik de [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) opdracht voor het ophalen van de installatiekopie van het register:

```Bash
docker pull myregistry.azurecr.io/samples/nginx
```

## <a name="start-the-nginx-container"></a>De Nginx-container starten

Gebruik de [docker uitvoeren](https://docs.docker.com/engine/reference/run/) opdracht om uit te voeren van de installatiekopie die u hebt opgehaald uit het register:

```Bash
docker run -it --rm -p 8080:80 myregistry.azurecr.io/samples/nginx
```

Blader naar [ http://localhost:8080 ](http://localhost:8080) om de actieve container weer te geven.

Als u wilt stoppen en verwijderen van de container, drukt u op `Control` + `C`.

## <a name="remove-the-image-optional"></a>Verwijder de installatiekopie (optioneel)

Als u de Nginx-installatiekopie niet meer nodig hebt, kunt u het verwijderen lokaal met de [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) opdracht.

```Bash
docker rmi myregistry.azurecr.io/samples/nginx
```

Als u wilt verwijderen van installatiekopieën van het Azure container registry, kunt u de Azure CLI-opdracht [az acr repository delete](/cli/azure/acr/repository#az-acr-repository-delete). De volgende opdracht verwijdert bijvoorbeeld het manifest waarnaar wordt verwezen door een label, alle gegevens van de bijbehorende laag en alle andere codes die verwijst naar het manifest.

```azurecli
az acr repository delete --name myregistry --repository samples/nginx --tag latest --manifest
```

## <a name="next-steps"></a>Volgende stappen

Nu dat u de basisprincipes kent, bent u klaar om te beginnen met behulp van het register. Containerinstallatiekopieën uit het register te implementeren:

* [Azure Kubernetes Service (AKS)](../aks/tutorial-kubernetes-prepare-app.md)
* [Azure Container Instances](../container-instances/container-instances-tutorial-prepare-app.md)
* [Service Fabric](../service-fabric/service-fabric-tutorial-create-container-images.md)
