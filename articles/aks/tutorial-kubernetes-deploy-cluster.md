---
title: 'Zelfstudie voor Kubernetes in Azure: Cluster implementeren'
description: 'Zelfstudie voor AKS: Cluster implementeren'
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/24/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 99b65ba5e5f0be40e464fd6565db40fda4170a0f
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="tutorial-deploy-an-azure-container-service-aks-cluster"></a>Zelfstudie: een AKS-cluster (Azure Container Service) implementeren

Kubernetes biedt een gedistribueerd platform voor toepassingen in containers. Met AKS kunt u eenvoudig en snel een Kubernetes-cluster inrichten dat gereed is voor productie. In deze zelfstudie, deel drie van de acht, wordt een Kubernetes-cluster geïmplementeerd in AKS. Dit zijn de uitgevoerde stappen:

> [!div class="checklist"]
> * Een Kubernetes AKS-cluster implementeren
> * Installatie van de Kubernetes-CLI (kubectl)
> * Configuratie van kubectl

In de volgende zelfstudies wordt de Azure Vote-toepassing geïmplementeerd in het cluster, geschaald en bijgewerkt. Ook wordt Log Analytics geconfigureerd om het Kubernetes-cluster te controleren.

## <a name="before-you-begin"></a>Voordat u begint

In de vorige zelfstudies is een containerinstallatiekopie gemaakt en geüpload naar een Azure Container Registry-exemplaar. Als u deze stappen niet hebt uitgevoerd en deze zelfstudie wilt volgen, gaat u terug naar [Zelfstudie 1: Containerinstallatiekopieën maken][aks-tutorial-prepare-app].

## <a name="enable-aks-preview"></a>AKS-preview inschakelen

Zolang AKS in preview is, moet voor het maken van nieuwe clusters een functievlag worden toegevoegd aan uw abonnement. U kunt deze functie aanvragen voor een willekeurig aantal abonnementen dat u wilt gebruiken. Gebruik de opdracht `az provider register` om de AKS-provider te registreren:

```azurecli
az provider register -n Microsoft.ContainerService
```

Als dat is gebeurd, kunt u een Kubernetes-cluster gaan maken met AKS.

## <a name="create-kubernetes-cluster"></a>Een Kubernetes-cluster maken

In het volgende voorbeeld wordt een cluster genaamd `myAKSCluster` gemaakt in de resourcegroep genaamd `myResourceGroup`. Deze resourcegroep is gemaakt in de [vorige zelfstudie][aks-tutorial-prepare-acr].

```azurecli
az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 1 --generate-ssh-keys
```

Na enkele minuten is de implementatie voltooid en retourneert json opgemaakte informatie over de AKS-implementatie.

## <a name="install-the-kubectl-cli"></a>De CLI kubectl installeren

Gebruik [kubectl][kubectl], de Kubernetes-opdrachtregelclient, als u vanaf uw clientcomputer verbinding wilt maken met het Kubernetes-cluster.

Als u Azure CloudShell gebruikt, is kubectl al geïnstalleerd. Als u de client lokaal wilt installeren, voert u de volgende opdracht uit:

```azurecli
az aks install-cli
```

## <a name="connect-with-kubectl"></a>Verbinden met kubectl

Als u kubectl zo wilt configureren dat de client verbinding maakt met uw Kubernetes-cluster, voert u de volgende opdracht uit:

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Als u de verbinding met uw cluster wilt controleren, voert u de opdracht [kubectl get nodes][kubectl-get] uit.

```azurecli
kubectl get nodes
```

Uitvoer:

```
NAME                          STATUS    AGE       VERSION
k8s-myAKSCluster-36346190-0   Ready     49m       v1.7.9
```

Wanneer de zelfstudie is voltooid, hebt u een AKS Kubernetes-cluster dat gereed is voor werkbelastingen. In de volgende zelfstudies wordt een toepassing met meerdere containers geïmplementeerd in dit cluster, geschaald, bijgewerkt en gecontroleerd.

## <a name="configure-acr-authentication"></a>ACR-verificatie configureren

Verificatie moet worden geconfigureerd tussen het AKS-cluster en het ACR-register. Dit omvat het verlenen van de juiste rechten aan de AKS-identiteit voor het ophalen van installatiekopieën uit het ACR-register.

Eerst moet de id van de service-principal die is geconfigureerd voor AKS worden opgehaald. Werk de naam van de resourcegroep en de naam van het AKS-cluster zó bij, dat deze overeenkomen met uw omgeving.

```azurecli
CLIENT_ID=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query "servicePrincipalProfile.clientId" --output tsv)
```

Haal de resource-id van het ACR-register op. Werk de naam van het register bij met die van uw ACR-register, en werk de naam van de resourcegroep bij met de resourcegroep waar het ACR-register zich bevindt.

```azurecli
ACR_ID=$(az acr show --name <acrName> --resource-group myResourceGroup --query "id" --output tsv)
```

Maak de roltoewijzing, waarmee de juiste toegangsrechten worden verleend.

```azurecli
az role assignment create --assignee $CLIENT_ID --role Reader --scope $ACR_ID
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie is een cluster Kubernetes geïmplementeerd in AKS. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * Een Kubernetes AKS-cluster is geïmplementeerd
> * De Kubernetes-CLI (kubectl) is geïnstalleerd
> * Kubectl is geconfigureerd

Ga naar de volgende zelfstudie om te leren hoe u de toepassing uitvoert in het cluster.

> [!div class="nextstepaction"]
> [Toepassing implementeren in Kubernetes][aks-tutorial-deploy-app]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-acr]: ./tutorial-kubernetes-prepare-acr.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
