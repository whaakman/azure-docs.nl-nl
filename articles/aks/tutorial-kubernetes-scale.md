---
title: 'Zelfstudie voor Kubernetes op Azure: Toepassing schalen'
description: 'Zelfstudie voor AKS: Toepassing schalen'
services: container-service
author: dlepow
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 6af564a1dc33b6382e70aaad107122034377a95e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-scale-application-in-azure-kubernetes-service-aks"></a>Zelfstudie: Een toepassing schalen in AKS (Azure Kubernetes Service)

Als u de zelfstudies volgt, hebt u een werkend Kubernetes-cluster in AKS en hebt u de Azure Voting-app geïmplementeerd.

In deze zelfstudie, deel vijf van de acht, schaalt u de schillen in de app en probeert u automatisch schalen van schillen uit. U leert ook hoe u het aantal Azure VM-knooppunten schaalt om de capaciteit van het cluster voor het hosten van werkbelastingen te wijzigen. Dit zijn de uitgevoerde taken:

> [!div class="checklist"]
> * De Kubernetes Azure-knooppunten schalen
> * Kubernetes-schillen handmatig schalen
> * Schillen die de front-end van de app uitvoeren, automatisch schalen

In de volgende zelfstudies wordt de Azure Vote-toepassing bijgewerkt en wordt Log Analytics geconfigureerd om het Kubernetes-cluster te controleren.

## <a name="before-you-begin"></a>Voordat u begint

In de vorige zelfstudies is een toepassing verpakt in een containerinstallatiekopie, is deze installatiekopie geüpload naar Azure Container Registry en is een Kubernetes-cluster gemaakt. De toepassing is vervolgens in het Kubernetes-cluster uitgevoerd.

Als u deze stappen niet hebt uitgevoerd en deze zelfstudie wilt volgen, gaat u terug naar [Zelfstudie 1: Containerinstallatiekopieën maken][aks-tutorial-prepare-app].

## <a name="scale-aks-nodes"></a>AKS-knooppunten schalen

Als u uw Kubernetes-cluster in de vorige zelfstudie hebt gemaakt met de opdrachten uit de vorige zelfstudie, heeft het cluster één knooppunt. U kunt het aantal knooppunten handmatig aanpassen als u meer of minder containerwerkbelastingen in uw cluster plant.

In het volgende voorbeeld wordt het aantal knooppunten in het Kubernetes-cluster *myAKSCluster* verhoogd tot drie. Het uitvoeren van deze opdracht duurt enkele minuten.

```azurecli
az aks scale --resource-group=myResourceGroup --name=myAKSCluster --node-count 3
```

De uitvoer is vergelijkbaar met:

```
"agentPoolProfiles": [
  {
    "count": 3,
    "dnsPrefix": null,
    "fqdn": null,
    "name": "myAKSCluster",
    "osDiskSizeGb": null,
    "osType": "Linux",
    "ports": null,
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_D2_v2",
    "vnetSubnetId": null
  }
```

## <a name="manually-scale-pods"></a>Schillen handmatig schalen

Tot nu toe zijn de Azure Vote-front-end en het Redis-exemplaar geïmplementeerd, elk met één replica. U kunt dit controleren door de opdracht [kubectl get][kubectl-get] uit te voeren.

```azurecli
kubectl get pods
```

Uitvoer:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Wijzig het aantal schillen in de `azure-vote-front`-implementatie handmatig met de opdracht [kubectl scale][kubectl-scale]. In dit voorbeeld wordt het aantal verhoogd naar 5.

```azurecli
kubectl scale --replicas=5 deployment/azure-vote-front
```

Voer [kubectl get pods][kubectl-get] uit om te controleren of Kubernetes de schillen heeft gemaakt. Na ongeveer een minuut worden de extra schillen uitgevoerd:

```azurecli
kubectl get pods
```

Uitvoer:

```
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Schillen automatisch schalen

Kubernetes biedt ondersteuning voor het [automatisch horizontaal schalen van schillen][kubernetes-hpa] om zo het aantal schillen in een implementatie aan te passen op basis van het CPU-gebruik of andere geselecteerde metrische gegevens.

Als u automatisch schalen wilt gebruiken, moeten CPU-aanvragen en -limieten voor uw schillen zijn gedefinieerd. In de `azure-vote-front`-implementatie vraagt de front-end container om 0,25 CPU, met een limiet van 0,5 CPU. De instellingen zien er als volgt uit:

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

In het volgende voorbeeld wordt de opdracht [kubectl autoscale][kubectl-autoscale] gebruikt om het aantal schillen in de `azure-vote-front`-implementatie automatisch te schalen. Als het CPU-gebruik in dit geval hoger is dan 50%, wordt het aantal schillen automatisch verhoogd naar maximaal 10.


```azurecli
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Voer de volgende opdracht uit om de status van het automatisch schalen te bekijken:

```azurecli
kubectl get hpa
```

Uitvoer:

```
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Bij een minimale belasting van de Azure Vote-app neemt het aantal schilreplica's na enkele minuten automatisch af tot 3.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u verschillende schaalfuncties in uw Kubernetes-cluster gebruikt. Behandelde taken zijn:

> [!div class="checklist"]
> * Kubernetes-schillen handmatig schalen
> * Schillen die de front-end van de app uitvoeren, automatisch schalen
> * De Kubernetes Azure-knooppunten schalen

Ga naar de volgende zelfstudie om te leren hoe u de toepassing bijwerkt in Kubernetes.

> [!div class="nextstepaction"]
> [Een toepassing bijwerken in Kubernetes][aks-tutorial-update-app]

<!-- LINKS - external -->
[kubectl-autoscale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-scale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale
[kubernetes-hpa]: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-update-app]: ./tutorial-kubernetes-app-update.md
