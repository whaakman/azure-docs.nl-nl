---
title: Snelstartgids - Azure Kubernetes-cluster voor Linux | Microsoft Docs
description: Snel informatie over het maken van een cluster Kubernetes voor Linux-containers in AKS met de Azure CLI.
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service, kubernetes
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017, mvc, devcenter
ms.openlocfilehash: d774f776bf4ef5434b0748e50b3064f47c834b7e
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="deploy-an-azure-container-service-aks-cluster"></a>Een Azure Container Service (AKS)-cluster implementeren

In deze snelstartgids wordt een cluster AKS geïmplementeerd met de Azure CLI. Een container voor meerdere-toepassing die bestaan uit een webfront-end en een Redis-exemplaar wordt vervolgens uitgevoerd op het cluster. Zodra de toepassing is voltooid, is deze toegankelijk via internet.

![Afbeelding van browsen naar Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

Deze snelstartgids wordt ervan uitgegaan dat een basiskennis van Kubernetes-concepten voor gedetailleerde informatie over Kubernetes Zie de [Kubernetes documentatie]( https://kubernetes.io/docs/home/).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, is deze snelstartgids vereist dat u de Azure CLI versie 2.0.20 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="enabling-aks-preview-for-your-azure-subscription"></a>AKS preview voor uw Azure-abonnement inschakelen
Terwijl AKS preview wordt, moet voor het maken van nieuwe clusters een functie-vlag op uw abonnement. U kunt deze functie voor een willekeurig aantal abonnementen die u wilt gebruiken kan aanvragen. Gebruik de `az provider register` opdracht voor het registreren van de provider AKS:

```azurecli-interactive
az provider register -n Microsoft.ContainerService
```

Na de registratie, bent u nu klaar voor het maken van een cluster Kubernetes met AKS.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create). Een Azure-resourcegroep is een logische groep waarin Azure-resources worden geïmplementeerd en beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Uitvoer:

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-aks-cluster"></a>AKS cluster maken

Het volgende voorbeeld wordt een cluster met de naam *myK8sCluster* met één knooppunt.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myK8sCluster --agent-count 1 --generate-ssh-keys
```

Na enkele minuten is de opdracht voltooid en retourneert deze informatie over het cluster in JSON-indeling.

## <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster

Als u een Kubernetes-cluster wilt beheren, gebruikt u [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), de Kubernetes-opdrachtregelclient.

Als u Azure Cloud-Shell, wordt kubectl is al geïnstalleerd. Als u lokaal installeren wilt, voert u de volgende opdracht.


```azurecli
az aks install-cli
```

Voer de volgende opdracht voor het configureren van kubectl verbinding maken met uw cluster Kubernetes. In deze stap worden referenties gedownload en wordt Kubernetes CLI geconfigureerd voor het gebruik van deze referenties.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myK8sCluster
```

Als u de verbinding met uw cluster wilt controleren, gebruikt u de opdracht [kubectl get](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) om een lijst met clusterknooppunten te retourneren.

```azurecli-interactive
kubectl get nodes
```

Uitvoer:

```
NAME                          STATUS    ROLES     AGE       VERSION
k8s-myk8scluster-36346190-0   Ready     agent     2m        v1.7.7
```

## <a name="run-the-application"></a>De toepassing uitvoeren

In een Kubernetes-manifestbestand wordt een gewenste status voor het cluster gedefinieerd, inclusief zaken zoals welke containerinstallatiekopieën moeten worden uitgevoerd. In dit voorbeeld worden met behulp van een manifest alle objecten gemaakt die nodig zijn om de Azure Vote-toepassing uit te voeren.

Maak een bestand met de naam `azure-vote.yml` en kopieert u de volgende YAML-code naar het. Als u werkt in Azure Cloud Shell, kan dit bestand worden gemaakt met behulp van vi of Nano, zoals bij een virtueel of fysiek systeem.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      containers:
      - name: azure-vote-back
        image: redis
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:redis-v1
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Gebruik de opdracht [kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) om de toepassing uit te voeren.

```azurecli-interactive
kubectl create -f azure-vote.yml
```

Uitvoer:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>De toepassing testen

Terwijl de toepassing wordt uitgevoerd, wordt er een [Kubernetes-service](https://kubernetes.io/docs/concepts/services-networking/service/) gemaakt die de front-end van de toepassing beschikbaar maakt op internet. Dit proces kan enkele minuten duren.

Gebruik de opdracht [kubectl get service](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) met het argument `--watch` om de voortgang te controleren.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Eerst wordt het *externe IP-adres* voor de service *azure-vote-front* weergegeven als *in behandeling*.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

Eenmaal de *extern IP-* adres is gewijzigd van *in behandeling* naar een *IP-adres*, gebruik `CTRL-C` het proces van de controle kubectl te stoppen.

```
azure-vote-front   LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Nu kunt u naar het externe IP-adres browsen voor een overzicht van de Azure Vote-toepassing.

![Afbeelding van browsen naar Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a>Cluster verwijderen
U kunt de opdracht [az group delete](/cli/azure/group#delete) gebruiken om de resourcegroep, de containerservice en alle gerelateerde resources te verwijderen wanneer u het cluster niet meer nodig hebt.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-the-code"></a>Code ophalen

In deze snelstartgids zijn vooraf gemaakte container installatiekopieën gebruikt om een Kubernetes-implementatie te maken. De gerelateerde toepassingscode, Dockerfile en het Kubernetes-manifestbestand zijn beschikbaar op GitHub.

[https://github.com/Azure-Samples/azure-voting-app-redis](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een Kubernetes-cluster geïmplementeerd en vervolgens een toepassing met meerdere containers naar het cluster geïmplementeerd.

Voor meer informatie over AKS en helpt bij een volledige code als implementatievoorbeeld, blijven de zelfstudie Kubernetes-cluster.

> [!div class="nextstepaction"]
> [Een cluster AKS beheren](./tutorial-kubernetes-prepare-app.md)
