---
title: Kubernetes op Azure zelfstudie - Cluster implementeren
description: AKS zelfstudie - Cluster implementeren
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2c2318d9a5f72800f9cfbd430dca448fd1e5746f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="deploy-an-azure-container-service-aks-cluster"></a>Een Azure Container Service (AKS)-cluster implementeren

Kubernetes biedt een gedistribueerd platform voor toepassingen in containers. Met AKS is het inrichten van een cluster met productie gereed Kubernetes snel en eenvoudig. In deze zelfstudie maakt deel uit drie acht, is in AKS een Kubernetes-cluster geïmplementeerd. Stappen voltooid omvatten:

> [!div class="checklist"]
> * Een Kubernetes AKS-cluster implementeren
> * Installatie van de CLI Kubernetes (kubectl)
> * Configuratie van kubectl

In volgende zelfstudies leert de stem van de Azure-toepassing is geïmplementeerd op het cluster, uitgebreide, bijgewerkt en Operations Management Suite is geconfigureerd voor het bewaken van het cluster Kubernetes.

## <a name="before-you-begin"></a>Voordat u begint

In vorige zelfstudies is een installatiekopie van een container gemaakt en geüpload naar een Azure Container register-exemplaar. Als u deze stappen nog niet hebt gedaan en u wilt volgen, terug naar [zelfstudie 1 – installatiekopieën van de container maken][aks-tutorial-prepare-app].

## <a name="enabling-aks-preview-for-your-azure-subscription"></a>AKS preview voor uw Azure-abonnement inschakelen
Terwijl AKS preview wordt, moet voor het maken van nieuwe clusters een functie-vlag op uw abonnement. U kunt deze functie voor een willekeurig aantal abonnementen die u wilt gebruiken kan aanvragen. Gebruik de `az provider register` opdracht voor het registreren van de provider AKS:

```azurecli-interactive
az provider register -n Microsoft.ContainerService
```

Na de registratie, bent u nu klaar voor het maken van een cluster Kubernetes met AKS.

## <a name="create-kubernetes-cluster"></a>Een Kubernetes-cluster maken

Het volgende voorbeeld wordt een cluster met de naam `myK8sCluster` in een resourcegroep met de naam `myResourceGroup`. Deze resourcegroep is gemaakt in de [vorige zelfstudie][aks-tutorial-prepare-acr].

```azurecli
az aks create --resource-group myResourceGroup --name myK8sCluster --node-count 1 --generate-ssh-keys
```

De implementatie is voltooid na een paar minuten en json retourneert informatie over de implementatie AKS geformatteerd.

## <a name="install-the-kubectl-cli"></a>De kubectl CLI installeren

Gebruiken voor verbinding met het cluster Kubernetes vanaf de clientcomputer, [kubectl][kubectl], de opdrachtregel Kubernetes-client.

Als u Azure CloudShell gebruikt, is kubectl al geïnstalleerd. Als u lokaal installeren wilt, voert u de volgende opdracht:

```azurecli
az aks install-cli
```

## <a name="connect-with-kubectl"></a>Verbinden met kubectl

Voer de volgende opdracht voor het configureren van kubectl verbinding maken met uw cluster Kubernetes:

```azurecli
az aks get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

Om te controleren of de verbinding met uw cluster, voer de [kubectl ophalen knooppunten] [ kubectl-get] opdracht.

```azurecli
kubectl get nodes
```

Uitvoer:

```
NAME                          STATUS    AGE       VERSION
k8s-myk8scluster-36346190-0   Ready     49m       v1.7.7
```

Zelfstudie is voltooid hebt u een cluster AKS gereed voor werkbelastingen. In volgende zelfstudies, is een toepassing met meerdere container geïmplementeerd op dit cluster, uitgebreid, bijgewerkt en bewaakt.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie is een cluster Kubernetes geïmplementeerd in AKS. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * Implementatie van een cluster Kubernetes AKS
> * De Kubernetes CLI (kubectl) geïnstalleerd
> * Geconfigureerde kubectl

Ga naar de volgende zelfstudie voor meer informatie over het uitvoeren van toepassing op het cluster.

> [!div class="nextstepaction"]
> [De toepassing in Kubernetes implementeren][aks-tutorial-deploy-app]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-acr]: ./tutorial-kubernetes-prepare-acr.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md