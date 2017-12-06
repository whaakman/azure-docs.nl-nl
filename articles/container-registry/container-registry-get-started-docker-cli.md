---
title: Push-Docker-installatiekopie naar persoonlijke Azure-register
description: "Docker-installatiekopieën pushen naar en ophalen van een privécontainerregister in Azure met de Docker-CLI"
services: container-registry
author: stevelas
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 11/29/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 21d1abfbb49eaeae654a600d35ab350b96a12fd3
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="push-your-first-image-to-a-private-docker-container-registry-using-the-docker-cli"></a>Uw eerste installatiekopie naar een Docker-containerregister pushen met de Docker-CLI

Een Azure-containerregister slaat persoonlijke [Docker](http://hub.docker.com)-containerinstallatiekopieën op en beheert ze, vergelijkbaar met de manier waarop [Docker Hub](https://hub.docker.com/) openbare Docker-installatiekopieën opslaat. U kunt de [Docker-opdrachtregelinterface](https://docs.docker.com/engine/reference/commandline/cli/) (Docker CLI) voor [aanmelding](https://docs.docker.com/engine/reference/commandline/login/), [push](https://docs.docker.com/engine/reference/commandline/push/), [pull](https://docs.docker.com/engine/reference/commandline/pull/), en andere bewerkingen op de container het register.

In de volgende stappen maakt u een officieel downloaden [Nginx-afbeelding](https://store.docker.com/images/nginx) uit het register met openbare Docker-Hub, het label voor de registersleutel van uw persoonlijke Azure-container, dit doorgeven aan het register en voeg alles uit het register.

## <a name="prerequisites"></a>Vereisten

* **Azure-containerregister**: maak een containerregister in uw Azure-abonnement. Gebruik bijvoorbeeld [Azure Portal](container-registry-get-started-portal.md) of de [Azure-CLI 2.0](container-registry-get-started-azure-cli.md).
* **Docker CLI** - wilt instellen op uw lokale computer als een Docker-host en toegang tot de Docker CLI-opdrachten, installeert u [Docker](https://docs.docker.com/engine/installation/).

## <a name="log-in-to-a-registry"></a>Aanmelden bij een register

Er zijn [op verschillende manieren om te verifiëren](container-registry-authentication.md) toe aan het register privé-container. De aanbevolen methode wanneer u werkt in een opdrachtregel met de Azure CLI-opdracht is [az acr aanmelding](/cli/azure/acr?view=azure-cli-latest#az_acr_login). Bijvoorbeeld, zich aanmelden bij een register met de naam *myregistry*:

```azurecli
az acr login --name myregistry
```

U kunt zich ook aanmelden met [docker aanmelding](https://docs.docker.com/engine/reference/commandline/login/). In het volgende voorbeeld worden de id en het wachtwoord van een [service-principal](../active-directory/active-directory-application-objects.md) van Azure Active Directory doorgegeven. Bijvoorbeeld, u wellicht [toegewezen van een service-principal](container-registry-authentication.md#service-principal) toe aan het register voor een automatiseringsscenario.

```Bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Beide opdrachten retourneren `Login Succeeded` eenmaal is voltooid. Als u `docker login`, mogelijk ziet u ook een beveiligingswaarschuwing aanbevelen van het gebruik van de `--password-stdin` parameter. Hoewel het gebruik ervan buiten het bereik van dit artikel is, wordt aangeraden na deze aanbevolen procedure. Zie voor meer informatie de [docker aanmelding](https://docs.docker.com/engine/reference/commandline/login/) -opdrachten.

> [!TIP]
> (Alleen kleine letters) op de registernaam van de volledig gekwalificeerde altijd opgeven als u werkt met `docker login` en wanneer het labelen van installatiekopieën voor pushen toe aan het register. In de voorbeelden in dit artikel, de volledig gekwalificeerde naam is *myregistry.azurecr.io*.

## <a name="pull-the-official-nginx-image"></a>De installatiekopie van het officiële Nginx ophalen

Trek eerst de installatiekopie van het openbare Nginx op uw lokale computer.

```Bash
docker pull nginx
```

## <a name="run-the-container-locally"></a>De container lokaal uitvoeren

Uitvoeren van volgende [docker uitvoeren](https://docs.docker.com/engine/reference/run/) opdracht interactief starten van een lokaal exemplaar van de Nginx-container (`-it`) op poort 8080. De `--rm` argument geeft aan dat de container moet worden verwijderd wanneer u het stopt.

```Bash
docker run -it --rm -p 8080:80 nginx
```

Blader naar [http://localhost: 8080](http://localhost:8080) om de standaardwebpagina afgehandeld door Nginx in de actieve container weer te geven. Hier ziet u een pagina zoals in het volgende:

![Nginx op lokale computer](./media/container-registry-get-started-docker-cli/nginx.png)

Omdat u de container interactief met gestart `-it`, ziet u het Nginx-server op de opdrachtregel-uitvoer na het navigeren naar het in uw browser.

Als u wilt stoppen en verwijderen van de container, drukt u op `Control` + `C`.

## <a name="create-an-alias-of-the-image"></a>Een alias van de afbeelding maken

Gebruik [docker-tag](https://docs.docker.com/engine/reference/commandline/tag/) een alias van de installatiekopie te maken met de volledig gekwalificeerde pad naar het register. In dit voorbeeld wordt de naamruimte `samples` gespecificeerd om overbodige items in de hoofdmap van het register te voorkomen.

```Bash
docker tag nginx myregistry.azurecr.io/samples/nginx
```

Zie voor meer informatie over labels met naamruimten het [opslagplaats naamruimten](container-registry-best-practices.md#repository-namespaces) sectie van [aanbevolen procedures voor het Azure Container register](container-registry-best-practices.md).

## <a name="push-the-image-to-your-registry"></a>De installatiekopie aan het register push

Nu dat u hebt de installatiekopie met de volledig gekwalificeerde pad naar uw persoonlijke register gecodeerd, kunt u dit doorgeven aan het register met [docker push](https://docs.docker.com/engine/reference/commandline/push/):

```Bash
docker push myregistry.azurecr.io/samples/nginx
```

## <a name="pull-the-image-from-your-registry"></a>Ophalen van de installatiekopie uit het register

Gebruik de [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) opdracht voor het ophalen van de installatiekopie uit het register:

```Bash
docker pull myregistry.azurecr.io/samples/nginx
```

## <a name="start-the-nginx-container"></a>Start de Nginx-container

Gebruik de [docker uitvoeren](https://docs.docker.com/engine/reference/run/) uit te voeren van de installatiekopie die u hebt opgehaald uit het register:

```Bash
docker run -it --rm -p 8080:80 myregistry.azurecr.io/samples/nginx
```

Ga naar [http://localhost:8080](http://localhost:8080) om de container die wordt uitgevoerd, weer te geven.

Als u wilt stoppen en verwijderen van de container, drukt u op `Control` + `C`.

## <a name="remove-the-image-optional"></a>Verwijder de installatiekopie (optioneel)

Als u het Nginx-installatiekopie niet meer nodig hebt, kunt u het verwijderen lokaal met de [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) opdracht.

```Bash
docker rmi myregistry.azurecr.io/samples/nginx
```

Als u wilt verwijderen installatiekopieën uit het register van uw Azure-container, kunt u de Azure CLI-opdracht [az acr opslagplaats verwijderen](/cli/azure/acr/repository#az_acr_repository_delete). De volgende opdracht wordt bijvoorbeeld het manifest waarnaar wordt verwezen door een label, geen gegevens van de bijbehorende laag en alle andere codes die verwijzen naar het manifest verwijderd.

```azurecli
az acr repository delete --name myregistry --repository samples/nginx --tag latest --manifest
```

## <a name="next-steps"></a>Volgende stappen

Nu u weet de basisbeginselen, bent u klaar om te beginnen met het register. Bijvoorbeeld implementeren container installatiekopieën van het register in op een [Azure Container Service (AKS)](../aks/tutorial-kubernetes-prepare-app.md) cluster.
