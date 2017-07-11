---
title: Snelstartgids - Azure Kubernetes-cluster voor Linux | Microsoft Docs
description: Leer snel een Kubernetes-cluster te maken voor Linux-containers in Azure Container Service met de Azure CLI.
services: container-service
documentationcenter: 
author: anhowe
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
ms.date: 05/31/2017
ms.author: anhowe
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 25043f6bf5e5ab3def8563bd2c096b79706bfec1
ms.contentlocale: nl-nl
ms.lasthandoff: 06/20/2017

---

<a id="deploy-kubernetes-cluster-for-linux-containers" class="xliff"></a>

# Kubernetes-cluster voor Linux-containers implementeren

De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. In deze handleiding vindt u informatie over het gebruik van de Azure CLI voor de implementatie van een [Kubernetes](https://kubernetes.io/docs/home/)-cluster in [Azure Container Service](container-service-intro.md). Zodra het cluster is geïmplementeerd, verbindt u het met het Kubernetes `kubectl`-opdrachtregelprogramma en implementeert u uw eerste Linux-container.

Voor deze zelfstudie is versie 2.0.4 of hoger van de Azure CLI vereist. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

<a id="log-in-to-azure" class="xliff"></a>

## Meld u aan bij Azure. 

Meld u aan bij uw Azure-abonnement met de opdracht [az login](/cli/azure/#login) en volg de instructies op het scherm.

```azurecli-interactive
az login
```

<a id="create-a-resource-group" class="xliff"></a>

## Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create). Een Azure-resourcegroep is een logische groep waarin Azure-resources worden geïmplementeerd en beheerd. 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

<a id="create-kubernetes-cluster" class="xliff"></a>

## Een Kubernetes-cluster maken
Maak een Kubernetes-cluster in Azure Container Service met de opdracht [az acs create](/cli/azure/acs#create). 

In het volgende voorbeeld wordt een cluster gemaakt met de naam *myK8sCluster* met een Linux-hoofdknooppunt en twee knooppunten van de Linux-agent. In dit voorbeeld worden SSH-sleutels gemaakt, als deze zich nog niet in de standaardlocaties bevinden. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`. Werk de clusternaam bij met een naam die geschikt is voor uw omgeving. 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --generate-ssh-keys 
```

Na enkele minuten is de opdracht voltooid en ziet u informatie over uw implementatie.

<a id="install-kubectl" class="xliff"></a>

## Kubectl installeren

Gebruik [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/), de Kubernetes-opdrachtregelclient, als u vanaf uw clientcomputer verbinding wilt maken met het Kubernetes-cluster. 

Als u Azure CloudShell gebruikt, is `kubectl` al geïnstalleerd. Als u lokaal wilt installeren, kunt u de opdracht [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) gebruiken.

In het volgende voorbeeld van Azure CLI wordt `kubectl` op uw systeem geïnstalleerd. Als u de Azure CLI in Mac OS of Linux uitvoert, moet u mogelijk de opdracht met `sudo` uitvoeren.

```azurecli-interactive 
az acs kubernetes install-cli 
```

<a id="connect-with-kubectl" class="xliff"></a>

## Verbinden met kubectl

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


<a id="deploy-an-nginx-container" class="xliff"></a>

## Een NGINX-container implementeren

U kunt een Docker-container uitvoeren binnen een Kubernetes-*schil*, die een of meer containers bevat. 

Met de volgende opdracht start de Docker-container NGINX in een Kubernetes-schil op een van de knooppunten. In dit geval voert de container de NGINX-webserver uit die is opgehaald uit een installatiekopie in [Docker Hub](https://hub.docker.com/_/nginx/).

```azurecli-interactive
kubectl run nginx --image nginx
```
Als u wilt zien dat de container wordt uitgevoerd, voert u het volgende uit:

```azurecli-interactive
kubectl get pods
```

<a id="view-the-nginx-welcome-page" class="xliff"></a>

## De welkomstpagina van NGINX weergeven
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


<a id="delete-cluster" class="xliff"></a>

## Cluster verwijderen
U kunt de opdracht [az group delete](/cli/azure/group#delete) gebruiken om de resourcegroep, de containerservice en alle gerelateerde resources te verwijderen wanneer u het cluster niet meer nodig hebt.

```azurecli-interactive 
az group delete --name myResourceGroup
```


<a id="next-steps" class="xliff"></a>

## Volgende stappen

In deze snelstartgids hebt u een Kubernetes-cluster geïmplementeerd, het verbonden met `kubectl` en een schil met een NGINX-container geïmplementeerd. Voor meer informatie over Azure Container Service gaat u naar de zelfstudie Kubernetes-cluster.

> [!div class="nextstepaction"]
> [Een ACS Kubernetes-cluster beheren](./container-service-tutorial-kubernetes-prepare-app.md)

