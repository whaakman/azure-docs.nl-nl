---
title: 'Zelfstudie Azure Container Service: Toepassing bijwerken'
description: 'Zelfstudie Azure Container Service: Toepassing bijwerken'
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 34b08111863df99dc05a7b269464ce65a916a171
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="update-an-application-in-kubernetes"></a>Een toepassing bijwerken in Kubernetes

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Nadat een toepassing is geïmplementeerd in Kubernetes, kunt u deze bijwerken door een nieuwe containerinstallatiekopie of versie van de installatiekopie op te geven. Wanneer u dit doet, wordt de update gefaseerd, zodat telkens maar een deel van de implementatie wordt bijgewerkt. Deze gefaseerde update maakt het mogelijk dat de toepassing kan worden uitgevoerd tijdens de update. Het biedt ook een terugdraaimechanisme als er een implementatiefout optreedt. 

In deze zelfstudie, deel zes van zeven, wordt de voorbeeldapp Azure Vote bijgewerkt. Taken die u uitvoert, zijn onder andere:

> [!div class="checklist"]
> * De code van de frond-end toepassing bijwerken
> * Een bijgewerkte containerinstallatiekopie maken
> * De containerinstallatiekopie naar Azure Container Registry pushen
> * De bijgewerkte containerinstallatiekopie implementeren

In de volgende zelfstudies wordt Operations Management Suite geconfigureerd voor het controleren van het Kubernetes-cluster.

## <a name="before-you-begin"></a>Voordat u begint

In de vorige zelfstudies is een toepassing verpakt in een containerinstallatiekopie, de installatiekopie geüpload naar Azure Container Registry en een Kubernetes-cluster gemaakt. De toepassing is vervolgens in het Kubernetes-cluster uitgevoerd. 

Er is ook een toepassingsopslagplaats gekloond die de broncode van de toepassing bevat en een vooraf gemaakt Docker Compose-bestand dat wordt gebruikt in deze zelfstudie. Controleer of u een kloon van de opslagplaats hebt gemaakt en of u naar de gekloonde map bent gegaan. In deze map bevindt zich een map genaamd `azure-vote` en een bestand genaamd `docker-compose.yml`.

Als u deze stappen niet hebt voltooid en deze zelfstudie wilt volgen, gaat u terug naar [Zelfstudie 1: Containerinstallatiekopieën maken](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="update-application"></a>Toepassing bijwerken

Voor deze zelfstudie wordt een wijziging aangebracht in de toepassing en wordt de bijgewerkte toepassing geïmplementeerd in het Kubernetes-cluster. 

De broncode van de toepassing vindt u in de map `azure-vote`. Open het bestand `config_file.cfg` met een code- of teksteditor. In dit voorbeeld wordt `vi` gebruikt.

```bash
vi azure-vote/azure-vote/config_file.cfg
```

Wijzig de waarden voor `VOTE1VALUE` en `VOTE2VALUE` en sla het bestand op.

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Sla het bestand op en sluit het.

## <a name="update-container-image"></a>Containerinstallatiekopie bijwerken

Gebruik [docker-compose](https://docs.docker.com/compose/) om de front-end installatiekopie opnieuw te maken en voer de bijgewerkte toepassing uit. Het argument `--build` wordt gebruikt om Docker Compose te instrueren de installatiekopie van de toepassing opnieuw te maken.

```bash
docker-compose up --build -d
```

## <a name="test-application-locally"></a>Toepassing lokaal testen

Blader naar http://localhost:8080 om de bijgewerkte toepassing te zien.

![Afbeelding van Kubernetes-cluster in Azure](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a>Installatiekopieën taggen en pushen

Tag de `azure-vote-front`-installatiekopie met de aanmeldingsserver van het containerregister. 

Haal de naam van de aanmeldingsserver op met de opdracht [az acr list](/cli/azure/acr#az_acr_list).

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Gebruik [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) om de installatiekopie te taggen. Vervang `<acrLoginServer>` door de naam van de aanmeldingsserver van uw Azure Container Registry of de hostnaam van een openbaar register. Merk op dat de versie van de installatiekopie is bijgewerkt naar `redis-v2`.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

Gebruik [docker push](https://docs.docker.com/engine/reference/commandline/push/) om de installatiekopie naar uw register te uploaden. Vervang `<acrLoginServer>` door de naam van de aanmeldingsserver van uw Azure Container Registry.

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a>De bijwerkte toepassing implementeren

Voor een maximale uptime moeten meerdere exemplaren van de toepassingsschil worden uitgevoerd. Controleer deze configuratie met de opdracht [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get).

```bash
kubectl get pod
```

Uitvoer:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

Als er niet meerdere schillen zijn waarin de installatiekopie azure-vote-front wordt uitgevoerd, schaalt u de `azure-vote-front`-implementatie.


```azurecli-interactive
kubectl scale --replicas=3 deployment/azure-vote-front
```

Gebruik de opdracht [kubectl set](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) om de toepassing bij te werken. Werk `<acrLoginServer>` bij met de aanmeldingserver of hostnaam van uw containerregister.

```azurecli-interactive
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

Voor het controleren van de implementatie gebruikt u de opdracht [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get). Terwijl de bijgewerkte toepassing wordt geïmplementeerd, worden uw schillen beëindigd en opnieuw gemaakt met de nieuwe containerinstallatiekopie.

```azurecli-interactive
kubectl get pod
```

Uitvoer:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running   0          5m
azure-vote-front-1297194256-tpjlg   1/1       Running   0         1m
azure-vote-front-1297194256-tptnx   1/1       Running   0         5m
azure-vote-front-1297194256-zktw9   1/1       Terminating   0         1m
```

## <a name="test-updated-application"></a>Bijgewerkte toepassing testen

Haal het externe IP-adres van de service `azure-vote-front` op.

```azurecli-interactive
kubectl get service azure-vote-front
```

Blader naar het IP-adres om de bijgewerkte toepassing te zien.

![Afbeelding van Kubernetes-cluster in Azure](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een toepassing bijgewerkt en deze update geïmplementeerd in een Kubernetes-cluster. De volgende taken zijn uitgevoerd:

> [!div class="checklist"]
> * De code van de front-end toepassing is bijgewerkt
> * Een bijgewerkte containerinstallatiekopie is gemaakt
> * De containerinstallatiekopie is gepusht naar Azure Container Registry
> * De bijgewerkte toepassing is geïmplementeerd

Ga naar de volgende zelfstudie om te leren hoe u Kubernetes controleert met Operations Management Suite.

> [!div class="nextstepaction"]
> [Kubernetes bewaken met OMS](./container-service-tutorial-kubernetes-monitor.md)