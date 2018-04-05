---
title: 'Zelfstudie voor Kubernetes op Azure: toepassing bijwerken'
description: 'Zelfstudie voor AKS: toepassing bijwerken'
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/24/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 97a7e0b8e33042739ccea9a086642d9019c15e5b
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="tutorial-update-an-application-in-azure-container-service-aks"></a>Zelfstudie: een toepassing bijwerken in Azure Container Service (AKS)

Nadat een toepassing is geïmplementeerd in Kubernetes, kunt u deze bijwerken door een nieuwe containerinstallatiekopie of versie van de installatiekopie op te geven. Wanneer u dit doet, wordt de update gefaseerd, zodat telkens maar een deel van de implementatie wordt bijgewerkt. Dankzij deze gefaseerde update kan de toepassing tijdens de update worden uitgevoerd. Het biedt ook een terugdraaimechanisme als er een implementatiefout optreedt. 

In deze zelfstudie, deel zes van acht, wordt de voorbeeldapp Azure Vote bijgewerkt. Taken die u uitvoert, zijn onder andere:

> [!div class="checklist"]
> * De code van de front-endtoepassing bijwerken
> * Een bijgewerkte containerinstallatiekopie maken
> * De containerinstallatiekopie naar Azure Container Registry pushen
> * De bijgewerkte containerinstallatiekopie implementeren

In de volgende zelfstudies wordt Log Analytics geconfigureerd om het Kubernetes-cluster te controleren.

## <a name="before-you-begin"></a>Voordat u begint

In de vorige zelfstudies is een toepassing verpakt in een containerinstallatiekopie, de installatiekopie geüpload naar Azure Container Registry en een Kubernetes-cluster gemaakt. De toepassing is vervolgens in het Kubernetes-cluster uitgevoerd. 

Er is ook een toepassingsopslagplaats gekloond die de broncode van de toepassing bevat en een vooraf gemaakt Docker Compose-bestand dat wordt gebruikt in deze zelfstudie. Controleer of u een kloon van de opslagplaats hebt gemaakt en of u mappen in de gekloonde map hebt gewijzigd. In deze map bevindt zich een map genaamd `azure-vote` en een bestand genaamd `docker-compose.yaml`.

Als u deze stappen niet hebt voltooid en deze zelfstudie wilt volgen, gaat u terug naar [Zelfstudie 1: Containerinstallatiekopieën maken][aks-tutorial-prepare-app]. 

## <a name="update-application"></a>Toepassing bijwerken

Voor deze zelfstudie wordt een wijziging aangebracht in de toepassing en wordt de bijgewerkte toepassing geïmplementeerd in het Kubernetes-cluster. 

De broncode van de toepassing vindt u in de map `azure-vote`. Open het bestand `config_file.cfg` met een code- of teksteditor. In dit voorbeeld wordt `vi` gebruikt.

```console
vi azure-vote/azure-vote/config_file.cfg
```

Wijzig de waarden voor `VOTE1VALUE` en `VOTE2VALUE` en sla het bestand op.

```console
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Sla het bestand op en sluit het.

## <a name="update-container-image"></a>Containerinstallatiekopie bijwerken

Gebruik [docker-compose][docker-compose] om de front-endinstallatiekopie opnieuw te maken en voer de bijgewerkte toepassing uit. Het argument `--build` wordt gebruikt om Docker Compose te instrueren de installatiekopie van de toepassing opnieuw te maken.

```console
docker-compose up --build -d
```

## <a name="test-application-locally"></a>Toepassing lokaal testen

Blader naar http://localhost:8080 om de bijgewerkte toepassing te zien.

![Afbeelding van Kubernetes-cluster in Azure](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a>Installatiekopieën taggen en pushen

Tag de `azure-vote-front`-installatiekopie met de aanmeldingserver van het containerregister. 

Haal de naam van de aanmeldingsserver op met de opdracht [az acr list](/cli/azure/acr#az_acr_list).

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Gebruik [docker tag][docker-tag] om de installatiekopie te taggen. Vervang `<acrLoginServer>` door de naam van de aanmeldingsserver van Azure Container Registry of de hostnaam van een openbaar register. Merk op dat de versie van de installatiekopie is bijgewerkt naar `v2`.

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v2
```

Gebruik [docker push][docker-push] om de installatiekopie naar uw register te uploaden. Vervang `<acrLoginServer>` door de naam van de aanmeldingsserver van Azure Container Registry. Als u problemen ondervindt met pushen naar het ACR-register, controleert u of u de opdracht [az acr login][az-acr-login] hebt uitgevoerd.

```console
docker push <acrLoginServer>/azure-vote-front:v2
```

## <a name="deploy-update-application"></a>De bijwerkte toepassing implementeren

Voor een maximale uptime moeten meerdere exemplaren van de toepassingsschil worden uitgevoerd. Controleer deze configuratie met de opdracht [kubectl get pod][kubectl-get].

```
kubectl get pod
```

Uitvoer:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

Als er niet meerdere schillen zijn waarin de installatiekopie azure-vote-front wordt uitgevoerd, schaalt u de `azure-vote-front`-implementatie.


```azurecli
kubectl scale --replicas=3 deployment/azure-vote-front
```

Gebruik de opdracht [kubectl set][kubectl-set] om de toepassing bij te werken. Werk `<acrLoginServer>` bij met de aanmeldingsserver of hostnaam van uw containerregister.

```azurecli
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:v2
```

Voor het controleren van de implementatie gebruikt u de opdracht [kubectl get pod][kubectl-get]. Terwijl de bijgewerkte toepassing wordt geïmplementeerd, worden uw schillen beëindigd en opnieuw gemaakt met de nieuwe containerinstallatiekopie.

```azurecli
kubectl get pod
```

Uitvoer:

```
NAME                               READY     STATUS        RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running       0          5m
azure-vote-front-1297194256-tpjlg  1/1       Running       0          1m
azure-vote-front-1297194256-tptnx  1/1       Running       0          5m
azure-vote-front-1297194256-zktw9  1/1       Terminating   0          1m
```

## <a name="test-updated-application"></a>Bijgewerkte toepassing testen

Haal het externe IP-adres van de service `azure-vote-front` op.

```azurecli
kubectl get service azure-vote-front
```

Blader naar het IP-adres om de bijgewerkte toepassing te zien.

![Afbeelding van Kubernetes-cluster in Azure](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een toepassing bijgewerkt en deze update geïmplementeerd in een Kubernetes-cluster. De volgende taken zijn uitgevoerd:

> [!div class="checklist"]
> * De code van de front-endtoepassing is bijgewerkt
> * Een bijgewerkte containerinstallatiekopie is gemaakt
> * De containerinstallatiekopie is gepusht naar Azure Container Registry
> * De bijgewerkte toepassing is geïmplementeerd

Ga naar de volgende zelfstudie om te leren hoe u Kubernetes controleert met Log Analytics.

> [!div class="nextstepaction"]
> [Kubernetes bewaken met Log Analytics][aks-tutorial-monitor]

<!-- LINKS - external -->
[docker-compose]: https://docs.docker.com/compose/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-set]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#set

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-monitor]: ./tutorial-kubernetes-monitor.md
[az-acr-login]: https://docs.microsoft.com/cli/azure/acr#az_acr_login