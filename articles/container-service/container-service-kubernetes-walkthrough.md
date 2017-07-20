---
title: Snelstartgids - Azure Kubernetes-cluster voor Linux | Microsoft Docs
description: Leer snel een Kubernetes-cluster te maken voor Linux-containers in Azure Container Service met de Azure CLI.
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: 3be2079d205d6bfd4c796e5f6abcd7ac5fe595a2
ms.contentlocale: nl-nl
ms.lasthandoff: 07/19/2017

---

# <a name="deploy-kubernetes-cluster-for-linux-containers"></a>Kubernetes-cluster voor Linux-containers implementeren

De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. In deze handleiding vindt u informatie over het gebruik van de Azure CLI voor de implementatie van een [Kubernetes](https://kubernetes.io/docs/home/)-cluster in [Azure Container Service](container-service-intro.md). Zodra het cluster is geïmplementeerd, verbindt u het met het Kubernetes `kubectl`-opdrachtregelprogramma en implementeert u uw eerste Linux-container.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze Quickstart gebruikmaken van Azure CLI versie 2.0.4 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create). Een Azure-resourcegroep is een logische groep waarin Azure-resources worden geïmplementeerd en beheerd. 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-kubernetes-cluster"></a>Een Kubernetes-cluster maken
Maak een Kubernetes-cluster in Azure Container Service met de opdracht [az acs create](/cli/azure/acs#create). 

In het volgende voorbeeld wordt een cluster gemaakt met de naam *myK8sCluster* met een Linux-hoofdknooppunt en twee knooppunten van de Linux-agent. In dit voorbeeld worden SSH-sleutels gemaakt, als deze zich nog niet in de standaardlocaties bevinden. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`. Werk de clusternaam bij met een naam die geschikt is voor uw omgeving. 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --generate-ssh-keys 
```

Na enkele minuten is de opdracht voltooid en ziet u informatie over uw implementatie.

## <a name="install-kubectl"></a>Kubectl installeren

Gebruik [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/), de Kubernetes-opdrachtregelclient, als u vanaf uw clientcomputer verbinding wilt maken met het Kubernetes-cluster. 

Als u Azure CloudShell gebruikt, is `kubectl` al geïnstalleerd. Als u lokaal wilt installeren, kunt u de opdracht [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) gebruiken.

In het volgende voorbeeld van Azure CLI wordt `kubectl` op uw systeem geïnstalleerd. Als u de Azure CLI in Mac OS of Linux uitvoert, moet u mogelijk de opdracht met `sudo` uitvoeren.

```azurecli-interactive 
az acs kubernetes install-cli 
```

## <a name="connect-with-kubectl"></a>Verbinden met kubectl

Als u `kubectl` zo wilt configureren dat er verbinding wordt gemaakt met uw Kubernetes-cluster, voert u de opdracht [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) uit. In het volgende voorbeeld wordt de clusterconfiguratie voor uw Kubernetes-cluster gedownload.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

Als u de verbinding met uw cluster van uw computer wilt verifiëren, probeert u het volgende uit te voeren:

```azurecli-interactive
kubectl get nodes
```

`kubectl` biedt een lijst met de hoofd- en agent-knooppunten.

```azurecli-interactive
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.5.3
k8s-agent-98dc3136-1    Ready                      5m        v1.5.3
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.5.3

```


## <a name="deploy-an-nginx-container"></a>Een NGINX-container implementeren

U kunt een Docker-container uitvoeren binnen een Kubernetes-*schil*, die een of meer containers bevat. 

Met de volgende opdracht start de Docker-container NGINX in een Kubernetes-schil op een van de knooppunten. In dit geval voert de container de NGINX-webserver uit die is opgehaald uit een installatiekopie in [Docker Hub](https://hub.docker.com/_/nginx/).

```azurecli-interactive
kubectl run nginx --image nginx
```
Als u wilt zien dat de container wordt uitgevoerd, voert u het volgende uit:

```azurecli-interactive
kubectl get pods
```

## <a name="view-the-nginx-welcome-page"></a>De welkomstpagina van NGINX weergeven
Als u de NGINX-server beschikbaar wilt maken met een openbaar IP-adres, typt u de volgende opdracht:

```azurecli-interactive
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

Deze opdracht zorgt ervoor dat Kubernetes een service en een [Azure load balancer-regel](container-service-kubernetes-load-balancing.md) met een openbaar IP-adres voor de service maakt. 

Voer de volgende opdracht uit om de status van de service te bekijken.

```azurecli-interactive
kubectl get svc
```

Het IP-adres wordt in eerste instantie weergegeven als `pending`. Na een paar minuten wordt het externe IP-adres van de service ingesteld:
  
```azurecli-interactive
NAME         CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE       
kubernetes   10.0.0.1       <none>          443/TCP        21h       
nginx        10.0.111.25    52.179.3.96     80/TCP         22m
```

U kunt de gewenste webbrowser gebruiken om de standaard NGINX-welkomstpagina weer te geven op het externe IP-adres:

![Afbeelding van bladeren naar Nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


## <a name="delete-cluster"></a>Cluster verwijderen
U kunt de opdracht [az group delete](/cli/azure/group#delete) gebruiken om de resourcegroep, de containerservice en alle gerelateerde resources te verwijderen wanneer u het cluster niet meer nodig hebt.

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een Kubernetes-cluster geïmplementeerd, het verbonden met `kubectl` en een schil met een NGINX-container geïmplementeerd. Voor meer informatie over Azure Container Service gaat u naar de zelfstudie Kubernetes-cluster.

> [!div class="nextstepaction"]
> [Een ACS Kubernetes-cluster beheren](./container-service-tutorial-kubernetes-prepare-app.md)

