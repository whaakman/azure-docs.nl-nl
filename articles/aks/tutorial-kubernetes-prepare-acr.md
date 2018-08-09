---
title: Kubernetes in Azure-zelfstudie - ACR voorbereiden
description: AKS-zelfstudie - ACR voorbereiden
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 4ad5dcb8dbb11f1d6e12e3c19eab5da68009df58
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39430753"
---
# <a name="tutorial-deploy-and-use-azure-container-registry"></a>Zelfstudie: Azure Container Registry implementeren en gebruiken

Azure Container Registry (ACR) is een op Azure gebaseerd, persoonlijk register voor installatiekopieën van Docker-container. Deze zelfstudie, deel twee van zeven, beschrijft het implementeren van een Azure Container Registry-exemplaar en het pushen een containerinstallatiekopie naar dat exemplaar. Dit zijn de uitgevoerde stappen:

> [!div class="checklist"]
> * Een exemplaar van Azure Container Registry (ACR) implementeren
> * Een containerinstallatiekopie voor ACR taggen
> * De installatiekopie uploaden naar ACR

In volgende zelfstudies wordt dit ACR-exemplaar geïntegreerd met een Kubernetes-cluster in AKS.

## <a name="before-you-begin"></a>Voordat u begint

In de [vorige zelfstudie][aks-tutorial-prepare-app] hebt u een containerinstallatiekopie voor een eenvoudige Azure Voting-toepassing gemaakt. Als u niet de installatiekopie voor de Azure Voting-toepassing hebt gemaakt, ga dan terug naar [Zelfstudie 1: Containerinstallatiekopieën maken][aks-tutorial-prepare-app].

Voor deze zelfstudie moet u de versie Azure CLI 2.0.27 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

## <a name="deploy-azure-container-registry"></a>Azure Container Registry implementeren

Wanneer u een Azure Container Registry implementeert, hebt u eerst een resourcegroep nodig. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

Een resourcegroep maken met de opdracht [az group create][az-group-create]. In dit voorbeeld wordt er een resourcegroep met de naam `myResourceGroup` gemaakt in de regio `eastus`.

```azurecli
az group create --name myResourceGroup --location eastus
```

Maak een Azure Container Registry met de opdracht [az acr create] [ az-acr-create]. De registernaam moet uniek zijn binnen Azure en mag 5 tot 50 alfanumerieke tekens bevatten.

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

In de rest van deze zelfstudie gebruiken we `<acrName>` als tijdelijke aanduiding voor de naam van het containerregister.

## <a name="container-registry-login"></a>Aanmelden bij het containerregister

Gebruik de opdracht [az acr login][az-acr-login] om u aan te melden bij het ACR-exemplaar. Geef de unieke naam op van het containerregister toen dat werd gemaakt.

```azurecli
az acr login --name <acrName>
```

De opdracht retourneert een bericht dat de aanmelding is gelukt.

## <a name="tag-container-images"></a>Containerinstallatiekopieën taggen

Als u een lijst met huidige installatiekopieën wilt weergeven, gebruikt u de opdracht [docker images][docker-images].

```console
docker images
```

Uitvoer:

```
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front             latest              4675398c9172        13 minutes ago      694MB
redis                        latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask               788ca94b2313        9 months ago        694MB
```

Elke containerinstallatiekopie moet worden getagd met de loginServer-naam van het register. Deze tag wordt gebruikt voor routering bij het pushen van containerinstallatiekopieën naar een installatiekopieregister.

Voer de opdracht [az acr list ][az-acr-list] uit om de loginServer-naam te verkrijgen.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Tag nu de `azure-vote-front`-installatiekopie met de loginServer van het containerregister. Voeg bovendien `:v1` toe aan het eind van de naam van de installatiekopie. Deze tag geeft de versie van de installatiekopie aan.

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Voer na het taggen de opdracht [docker images] [ docker-images] uit om de bewerking te controleren.

```console
docker images
```

Uitvoer:

```
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest              eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry082.azurecr.io/azure-vote-front   v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask               788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-to-registry"></a>Installatiekopieën naar het register pushen

Push de `azure-vote-front`-installatiekopie naar het register.

In het volgende voorbeeld vervangt u de naam van de ACR-loginServer door de naam van de loginServer uit uw omgeving.

```console
docker push <acrLoginServer>/azure-vote-front:v1
```

Dit duurt enkele minuten duren.

## <a name="list-images-in-registry"></a>Installatiekopieën vermelden in het register

Gebruik de opdracht [az acr repository list][az-acr-repository-list] om een lijst met installatiekopieën te retourneren die naar het Azure Container Registry zijn gepusht. Werk de opdracht bij met de naam van het ACR-exemplaar.

```azurecli
az acr repository list --name <acrName> --output table
```

Uitvoer:

```azurecli
Result
----------------
azure-vote-front
```

Gebruik vervolgens de opdracht [az acr repository show-tags][az-acr-repository-show-tags] om de tags voor een specifieke installatiekopie weer te geven.

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

Uitvoer:

```azurecli
Result
--------
v1
```

Bij het voltooien van de zelfstudie is de containerinstallatiekopie opgeslagen in een persoonlijk Azure Container Registry-exemplaar. Deze installatiekopie wordt vanuit ACR geïmplementeerd naar een Kubernetes-cluster in volgende zelfstudies.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie is een Azure Container Registry voorbereid voor gebruik in een AKS-cluster. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * Er is een Azure Container Registry-exemplaar geïmplementeerd
> * Er is een containerinstallatiekopie voor ACR getagd
> * De installatiekopie is geüpload naar ACR

Ga naar de volgende zelfstudie voor informatie over het implementeren van een Kubernetes-cluster in Azure.

> [!div class="nextstepaction"]
> [Kubernetes-cluster implementeren][aks-tutorial-deploy-cluster]

<!-- LINKS - external -->
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#create
[az-acr-list]: /cli/azure/acr#list
[az-acr-login]: https://docs.microsoft.com/cli/azure/acr#az-acr-login
[az-acr-list]: https://docs.microsoft.com/cli/azure/acr#az-acr-list
[az-acr-repository-list]: /cli/azure/acr/repository#list
[az-acr-repository-show-tags]: /cli/azure/acr/repository#show-tags
[az-group-create]: /cli/azure/group#az-group-create
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-tutorial-deploy-cluster]: ./tutorial-kubernetes-deploy-cluster.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
