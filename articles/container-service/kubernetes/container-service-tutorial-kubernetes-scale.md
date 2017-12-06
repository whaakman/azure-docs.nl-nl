---
title: Zelfstudie voor Azure Container Service - toepassing schalen
description: Zelfstudie voor Azure Container Service - toepassing schalen
services: container-service
author: dlepow
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: a748e15abbc01f260349fba2678c03a40c4d7713
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="scale-kubernetes-pods-and-kubernetes-infrastructure"></a>Schaal Kubernetes gehele product en Kubernetes infrastructuur

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Als u de zelfstudies volgt hebt, hebt u een werkende Kubernetes cluster in Azure Container Service en u de app Azure stemmen hebt geïmplementeerd. 

In deze zelfstudie maakt deel uit vijf zeven, uitbreiden van het gehele product in de app en probeer het schil automatisch schalen. U leert ook hoe schalen het aantal knooppunten van de Azure VM-agent wijzigen van het cluster capaciteit voor het hosten van werkbelastingen. Taken zijn voltooid, zijn onder andere:

> [!div class="checklist"]
> * Handmatig schalen Kubernetes gehele product
> * Automatisch schalen gehele product met de front-end van de app configureren
> * De agent Kubernetes Azure knooppunten schalen

In volgende zelfstudies leert de stem van de Azure-toepassing wordt bijgewerkt en Operations Management Suite is geconfigureerd voor het controleren van het cluster Kubernetes.

## <a name="before-you-begin"></a>Voordat u begint

In vorige zelfstudies is een toepassing worden verpakt in een installatiekopie van een container, deze installatiekopie geüpload naar het register van Azure-Container en een Kubernetes-cluster gemaakt. De toepassing is vervolgens op het cluster Kubernetes uitgevoerd. 

Als u deze stappen nog niet hebt gedaan en u wilt volgen, terug naar de [zelfstudie 1 – installatiekopieën van de container maken](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="manually-scale-pods"></a>Handmatig schalen gehele product

Dus helemaal, de Azure-stem front-end- en het Redis-exemplaar zijn geïmplementeerd, elk met een enkele replica. Uitvoeren om te controleren, de [kubectl ophalen](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) opdracht.

```azurecli-interactive
kubectl get pods
```

Uitvoer:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Handmatig wijzigen van het nummer van het gehele product in de `azure-vote-front` implementatie met behulp van de [kubectl scale](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale) opdracht. In dit voorbeeld verhoogt het aantal op 5.

```azurecli-interactive
kubectl scale --replicas=5 deployment/azure-vote-front
```

Voer [kubectl ophalen gehele product](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) Kubernetes maken van het gehele product controleren. Na een minuut of dat worden het extra gehele product uitgevoerd:

```azurecli-interactive
kubectl get pods
```

Uitvoer:

```bash
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


```azurecli-interactive
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Overzicht van de status van de autoscaler, voer de volgende opdracht:

```azurecli-interactive
kubectl get hpa
```

Uitvoer:

```bash
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Na een paar minuten, met minimale belasting van de app Azure stem vermindert het aantal replica's schil automatisch in 3.

## <a name="scale-the-agents"></a>De agents schalen

Als u uw Kubernetes-cluster met standaardopdrachten in de vorige zelfstudie hebt gemaakt, heeft deze drie knooppunten van de agent. Als u van plan meer of minder container werkbelastingen uw cluster bent, kunt u het aantal agents handmatig aanpassen. Gebruik de [az acs schalen](/cli/azure/acs#scale) opdracht in en geef het aantal agents met de `--new-agent-count` parameter.

Het volgende voorbeeld verhoogt het aantal knooppunten tot en met 4 in het Kubernetes-cluster met de naam agent *myK8sCluster*. De opdracht duurt enkele minuten duren.

```azurecli-interactive
az acs scale --resource-group=myResourceGroup --name=myK8SCluster --new-agent-count 4
```

Uitvoer van de opdracht toont het aantal agent knooppunten in de waarde van `agentPoolProfiles:count`:

```azurecli
{
  "agentPoolProfiles": [
    {
      "count": 4,
      "dnsPrefix": "myK8SCluster-myK8SCluster-e44f25-k8s-agents",
      "fqdn": "",
      "name": "agentpools",
      "vmSize": "Standard_D2_v2"
    }
  ],
...

```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie kunt u verschillende functies die schalen in uw cluster Kubernetes gebruikt. Taken aan de orde opgenomen:

> [!div class="checklist"]
> * Handmatig schalen Kubernetes gehele product
> * Automatisch schalen gehele product met de front-end van de app configureren
> * De agent Kubernetes Azure knooppunten schalen

Ga naar de volgende zelfstudie voor meer informatie over het bijwerken van de toepassing in Kubernetes.

> [!div class="nextstepaction"]
> [Bijwerken van een toepassing in Kubernetes](./container-service-tutorial-kubernetes-app-update.md)

