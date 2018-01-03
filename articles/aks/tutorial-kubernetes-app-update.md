---
title: Kubernetes op zelfstudie voor Azure - toepassing
description: AKS zelfstudie - toepassing
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 6de5173aedc836f7a2d56370ea8e54ad6e77ab5e
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="update-an-application-in-azure-container-service-aks"></a>Bijwerken van een toepassing in Azure Container Service (AKS)

Nadat een toepassing is geïmplementeerd in Kubernetes, kan deze worden bijgewerkt door het opgeven van een nieuwe container afbeeldings- of image versie. Wanneer u dit doet, de update is voorbereid zodat alleen een deel van de implementatie gelijktijdig worden bijgewerkt. Deze voorbereide update kan de toepassing tijdens de update blijven uitvoeren. Het bevat ook een terugdraaimechanisme als een implementatie-fout optreedt. 

In deze zelfstudie maakt deel uit zes acht, wordt de voorbeeld-app voor Azure stem bijgewerkt. Taken die u uitvoert, zijn onder andere:

> [!div class="checklist"]
> * De front-toepassingscode bijwerken
> * Maken van een installatiekopie van een bijgewerkte container
> * De installatiekopie van de container pushen in Azure Container register
> * De installatiekopie van het bijgewerkte container implementeren

In volgende zelfstudies is Operations Management Suite geconfigureerd voor het controleren van het cluster Kubernetes.

## <a name="before-you-begin"></a>Voordat u begint

In vorige zelfstudies is een toepassing worden verpakt in een installatiekopie van een container, de installatiekopie die is geüpload naar Azure Container register en een Kubernetes-cluster gemaakt. De toepassing is vervolgens op het cluster Kubernetes uitgevoerd. 

Een toepassing-opslagplaats is ook gekloond waaronder de broncode van de toepassing en een vooraf gemaakte Docker Compose bestand dat wordt gebruikt in deze zelfstudie. Controleer of dat u een kloon van de opslagplaats hebt gemaakt en dat u de mappen in de gekloonde directory hebt ingesteld. Binnen een map met de naam is `azure-vote` en een bestand met de naam `docker-compose.yaml`.

Als u deze stappen niet zijn voltooid en u wilt volgen, terug naar [zelfstudie 1 – installatiekopieën van de container maken][aks-tutorial-prepare-app]. 

## <a name="update-application"></a>Toepassing bijwerken

Voor deze zelfstudie is een wijziging aangebracht aan de toepassing en de bijgewerkte toepassing geïmplementeerd op het cluster Kubernetes. 

De broncode van de toepassing kan worden gevonden in de `azure-vote` directory. Open de `config_file.cfg` bestand met een code- of teksteditor. In dit voorbeeld `vi` wordt gebruikt.

```console
vi azure-vote/azure-vote/config_file.cfg
```

Wijzig de waarden voor `VOTE1VALUE` en `VOTE2VALUE`, en sla het bestand.

```console
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Sla en sluit het bestand.

## <a name="update-container-image"></a>Bijwerken van de installatiekopie van de container

Gebruik [docker compose] [ docker-compose] opnieuw maken van de front-installatiekopie en de bijgewerkte toepassing uitvoeren. De `--build` argument wordt gebruikt om te instrueren Docker Compose installatiekopie van de toepassing opnieuw te maken.

```console
docker-compose up --build -d
```

## <a name="test-application-locally"></a>Toepassing lokaal testen

Blader naar http://localhost: 8080 om te zien van de bijgewerkte toepassing.

![Afbeelding van Kubernetes-cluster in Azure](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a>Installatiekopieën van het label en pushmeldingen

Tag de `azure-vote-front` installatiekopie met de loginServer van het register van de container. 

Ophalen van de aanmeldingsnaam van server met de [az acr lijst](/cli/azure/acr#list) opdracht.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Gebruik [docker-tag] [ docker-tag] voor het taggen van de installatiekopie. Vervang `<acrLoginServer>` met uw Azure-Container register server aanmeldingsnaam of openbare register hostnaam. Ook u ziet dat de versie van de installatiekopie wordt bijgewerkt naar `redis-v2`.

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

Gebruik [docker push] [ docker-push] voor het uploaden van de installatiekopie aan het register. Vervang `<acrLoginServer>` met de naam van uw Azure-Container register-aanmelding.

```console
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a>Updatetoepassing implementeren

Maximale uptime, zodat moeten meerdere exemplaren van de toepassing schil worden uitgevoerd. Controleer de configuratie met de [kubectl ophalen schil] [ kubectl-get] opdracht.

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

Als er niet meerdere gehele product met de azure-stem-front-installatiekopie, schalen de `azure-vote-front` implementatie.


```azurecli
kubectl scale --replicas=3 deployment/azure-vote-front
```

Gebruik voor het bijwerken van de toepassing, de [kubectl set] [ kubectl-set] opdracht. Update `<acrLoginServer>` met de aanmeldingsnaam voor server of de hostnaam van het register van de container.

```azurecli
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

Voor het bewaken van de implementatie, gebruiken de [kubectl ophalen schil] [ kubectl-get] opdracht. Als de bijgewerkte toepassing wordt geïmplementeerd, worden uw gehele product is beëindigd en opnieuw gemaakt met de nieuwe installatiekopie van de container.

```azurecli
kubectl get pod
```

Uitvoer:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running   0          5m
azure-vote-front-1297194256-tpjlg   1/1       Running   0         1m
azure-vote-front-1297194256-tptnx   1/1       Running   0         5m
azure-vote-front-1297194256-zktw9   1/1       Terminating   0         1m
```

## <a name="test-updated-application"></a>Bijgewerkte toepassing testen

Het externe IP-adres van de `azure-vote-front` service.

```azurecli
kubectl get service azure-vote-front
```

Blader naar het IP-adres voor een overzicht van de bijgewerkte toepassing.

![Afbeelding van Kubernetes-cluster in Azure](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt een toepassing wordt bijgewerkt en deze update geïmplementeerd voor een cluster Kubernetes. De volgende taken zijn voltooid:

> [!div class="checklist"]
> * De front-toepassingscode bijgewerkt
> * Een installatiekopie van een bijgewerkte container gemaakt
> * De installatiekopie van de container in Azure Container register gepusht
> * De bijgewerkte toepassing is geïmplementeerd

Ga naar de volgende zelfstudie voor meer informatie over het bewaken van Kubernetes bij Operations Management Suite.

> [!div class="nextstepaction"]
> [Monitor Kubernetes met logboekanalyse][aks-tutorial-monitor]

<!-- LINKS - external -->
[docker-compose]: https://docs.docker.com/compose/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-set]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#set

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-monitor]: ./tutorial-kubernetes-monitor.md