---
title: Kubernetes op zelfstudie voor Azure - toepassing schalen
description: AKS zelfstudie - toepassing schalen
services: container-service
author: dlepow
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 9eba0de054b06233f2de7fb375010b4b40c6937f
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="scale-application-in-azure-container-service-aks"></a>Toepassing schalen in Azure Container Service (AKS)

Als u de zelfstudies volgt hebt, hebt u een werkende Kubernetes cluster in AKS en u de app Azure stemmen hebt geïmplementeerd.

In deze zelfstudie maakt deel uit vijf acht, het gehele product in de app uitbreiden en probeer het schil automatisch schalen. U leert ook hoe schalen het aantal knooppunten van de virtuele machine van Azure te wijzigen van het cluster capaciteit voor het hosten van werkbelastingen. Taken zijn voltooid, zijn onder andere:

> [!div class="checklist"]
> * De knooppunten Kubernetes Azure schalen
> * Handmatig schalen Kubernetes gehele product
> * Automatisch schalen gehele product met de front-end van de app configureren

In volgende zelfstudies leert de stem van de Azure-toepassing wordt bijgewerkt en Operations Management Suite is geconfigureerd voor het controleren van het cluster Kubernetes.

## <a name="before-you-begin"></a>Voordat u begint

In vorige zelfstudies is een toepassing worden verpakt in een installatiekopie van een container, deze installatiekopie geüpload naar het register van Azure-Container en een Kubernetes-cluster gemaakt. De toepassing is vervolgens op het cluster Kubernetes uitgevoerd.

Als u deze stappen nog niet hebt gedaan en u wilt volgen, terug naar de [zelfstudie 1 – installatiekopieën van de container maken](./tutorial-kubernetes-prepare-app.md).

## <a name="scale-aks-nodes"></a>Schaal AKS knooppunten

Als u uw Kubernetes-cluster met de opdrachten in de vorige zelfstudie hebt gemaakt, heeft één knooppunt. Als u van plan meer of minder container werkbelastingen uw cluster bent, kunt u het aantal knooppunten handmatig aanpassen.

Het volgende voorbeeld verhoogt het aantal knooppunten tot drie in het Kubernetes-cluster met de naam *myK8sCluster*. De opdracht duurt enkele minuten duren.

```azurecli
az aks scale --resource-group=myResourceGroup --name=myK8SCluster --node-count 3
```

De uitvoer is vergelijkbaar met:

```
"agentPoolProfiles": [
  {
    "count": 3,
    "dnsPrefix": null,
    "fqdn": null,
    "name": "myK8sCluster",
    "osDiskSizeGb": null,
    "osType": "Linux",
    "ports": null,
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_D2_v2",
    "vnetSubnetId": null
  }
```

## <a name="manually-scale-pods"></a>Handmatig schalen gehele product

Dus helemaal, de Azure-stem front-end- en het Redis-exemplaar zijn geïmplementeerd, elk met een enkele replica. Uitvoeren om te controleren, de [kubectl ophalen](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) opdracht.

```azurecli
kubectl get pods
```

Uitvoer:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Handmatig wijzigen van het nummer van het gehele product in de `azure-vote-front` implementatie met behulp van de [kubectl scale](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale) opdracht. In dit voorbeeld verhoogt het aantal op 5.

```azurecli
kubectl scale --replicas=5 deployment/azure-vote-front
```

Voer [kubectl ophalen gehele product](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) Kubernetes maken van het gehele product controleren. Na een minuut of dat worden het extra gehele product uitgevoerd:

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

## <a name="autoscale-pods"></a>Gehele product automatisch schalen

Biedt ondersteuning voor Kubernetes [horizontale schil automatisch schalen](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) aanpassen zodat het nummer van het gehele product in een implementatie, afhankelijk van de CPU-gebruik of andere metrische gegevens selecteren.

Voor het gebruik van de autoscaler hebt uw gehele product CPU-aanvragen en limieten die zijn gedefinieerd. In de `azure-vote-front` voor implementatie, de front-container aanvragen 0,25 CPU, met een limiet van 0,5 CPU. De instellingen eruitzien als:

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

Het volgende voorbeeld wordt de [kubectl automatisch schalen](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#autoscale) opdracht die moet worden automatisch schalen het aantal gehele product in de `azure-vote-front` implementatie. Als het CPU-gebruik hoger is dan 50%, verhoogt de autoscaler hier het gehele product tot maximaal 10.


```azurecli
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Overzicht van de status van de autoscaler, voer de volgende opdracht:

```azurecli
kubectl get hpa
```

Uitvoer:

```
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Na een paar minuten, met minimale belasting van de app Azure stem vermindert het aantal replica's schil automatisch in 3.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie kunt u verschillende functies die schalen in uw cluster Kubernetes gebruikt. Taken aan de orde opgenomen:

> [!div class="checklist"]
> * Handmatig schalen Kubernetes gehele product
> * Automatisch schalen gehele product met de front-end van de app configureren
> * De knooppunten Kubernetes Azure schalen

Ga naar de volgende zelfstudie voor meer informatie over het bijwerken van de toepassing in Kubernetes.

> [!div class="nextstepaction"]
> [Bijwerken van een toepassing in Kubernetes](./tutorial-kubernetes-app-update.md)