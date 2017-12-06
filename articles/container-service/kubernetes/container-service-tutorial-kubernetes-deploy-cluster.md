---
title: Zelfstudie voor Azure Container Service - Cluster implementeren
description: Zelfstudie voor Azure Container Service - Cluster implementeren
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c91eea3734820239187bcf7b497fb06d7fd5f7ef
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="deploy-a-kubernetes-cluster-in-azure-container-service"></a>Een cluster Kubernetes in Azure Container Service implementeren

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Kubernetes biedt een gedistribueerd platform voor toepassingen in containers. Met Azure Container Service is het inrichten van een cluster met productie gereed Kubernetes snel en eenvoudig. In deze zelfstudie maakt deel 3 van 7, wordt een Azure Container Service Kubernetes-cluster geïmplementeerd. Stappen voltooid omvatten:

> [!div class="checklist"]
> * Een Kubernetes ACS-cluster implementeren
> * Installatie van de CLI Kubernetes (kubectl)
> * Configuratie van kubectl

In volgende zelfstudies leert de stem van de Azure-toepassing is geïmplementeerd op het cluster, uitgebreide, bijgewerkt en Operations Management Suite is geconfigureerd voor het bewaken van het cluster Kubernetes.

## <a name="before-you-begin"></a>Voordat u begint

In vorige zelfstudies is een installatiekopie van een container gemaakt en geüpload naar een Azure Container register-exemplaar. Als u deze stappen nog niet hebt gedaan en u wilt volgen, terug naar [zelfstudie 1 – installatiekopieën van de container maken](./container-service-tutorial-kubernetes-prepare-app.md).

## <a name="create-kubernetes-cluster"></a>Een Kubernetes-cluster maken

Maak een Kubernetes-cluster in Azure Container Service met de opdracht [az acs create](/cli/azure/acs#create). 

Het volgende voorbeeld wordt een cluster met de naam `myK8sCluster` in een resourcegroep met de naam `myResourceGroup`. Deze resourcegroep is gemaakt in de [vorige zelfstudie](./container-service-tutorial-kubernetes-prepare-acr.md).

```azurecli-interactive 
az acs create --orchestrator-type kubernetes --resource-group myResourceGroup --name myK8SCluster --generate-ssh-keys 
```

In sommige gevallen, zoals met een beperkte proefversie, heeft een Azure-abonnement beperkte toegang tot Azure-resources. Als de implementatie mislukt vanwege beperkte beschikbare kernen, verminder dan het aantal standaardagenten door `--agent-count 1` toe te voegen aan de opdracht [az acs create](/cli/azure/acs#create). 

De implementatie is voltooid na een paar minuten en retourneert json geformatteerd informatie over de ACS-implementatie.

## <a name="install-the-kubectl-cli"></a>De kubectl CLI installeren

Gebruiken voor verbinding met het cluster Kubernetes vanaf de clientcomputer, [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), de opdrachtregel Kubernetes-client. 

Als u Azure CloudShell gebruikt, is kubectl al geïnstalleerd. Als u lokaal installeren wilt, gebruikt u de [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) opdracht.

Als in de Linux- of Mac OS uitgevoerd, moet u wellicht uitvoeren met sudo. In Windows, zorg ervoor dat uw shell is uitgevoerd als administrator.

```azurecli-interactive 
az acs kubernetes install-cli 
```

Op Windows, de standaardinstallatie is *c:\program files (x86)\kubectl.exe*. Mogelijk moet u dit bestand toevoegen aan het Windows-pad. 

## <a name="connect-with-kubectl"></a>Verbinden met kubectl

Als u kubectl zo wilt configureren dat de client verbinding maakt met uw Kubernetes-cluster, voert u de opdracht [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) uit.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group myResourceGroup --name myK8SCluster
```

Om te controleren of de verbinding met uw cluster, voer de [kubectl ophalen knooppunten](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) opdracht.

```azurecli-interactive
kubectl get nodes
```

Uitvoer:

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.6.2
k8s-agent-98dc3136-1    Ready                      5m        v1.6.2
k8s-agent-98dc3136-2    Ready                      5m        v1.6.2
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.6.2
```

Zelfstudie is voltooid hebt u een ACS-Kubernetes cluster gereed voor werkbelastingen. In volgende zelfstudies, is een toepassing met meerdere container geïmplementeerd op dit cluster, uitgebreid, bijgewerkt en bewaakt.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie is een Azure Container Service Kubernetes-cluster is geïmplementeerd. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * Implementatie van een cluster Kubernetes ACS
> * De Kubernetes CLI (kubectl) geïnstalleerd
> * Geconfigureerde kubectl

Ga naar de volgende zelfstudie voor meer informatie over het uitvoeren van toepassing op het cluster.

> [!div class="nextstepaction"]
> [De toepassing in Kubernetes implementeren](./container-service-tutorial-kubernetes-deploy-application.md)
