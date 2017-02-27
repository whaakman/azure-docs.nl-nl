---
title: Snelstartgids voor Kubernetes-clusters in Azure | Microsoft Docs
description: Aan de slag met een Kubernetes-cluster en dit implementeren in Azure Container Service
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
ms.date: 02/21/2017
ms.author: anhowe
translationtype: Human Translation
ms.sourcegitcommit: 2a381431acb6436ddd8e13c69b05423a33cd4fa6
ms.openlocfilehash: 1742a6d4d99b81509564696e6faaf9e6fbf8f604


---

# <a name="azure-container-service---kubernetes-walkthrough"></a>Azure Container Service - Kennismaking met Kubernetes


Met de instructies in dit artikel kunt u de Azure CLI 2.0-opdrachten gebruiken om een Kubernetes-cluster te maken. Vervolgens gebruikt u het opdrachtregelprogramma `kubectl` om aan de slag te gaan met containers in het cluster.

Op de volgende afbeelding ziet u de architectuur van een Container Service-cluster met één master en twee agents. De master dient de Kubernetes REST-API. De agentknooppunten worden gegroepeerd in een Azure-beschikbaarheidsset en voeren uw containers uit. Alle virtuele machines bevinden zich in hetzelfde virtuele privénetwerk en hebben volledige toegang tot elkaar.

![Afbeelding van Kubernetes-cluster in Azure](media/container-service-kubernetes-walkthrough/kubernetes.png)

## <a name="prerequisites"></a>Vereisten
In dit scenario wordt ervan uitgegaan dat u [Azure CLI-versie 2.0](/cli/azure/install-az-cli2) hebt geïnstalleerd en ingesteld. Ook moet u een openbare SSH-RSA-sleutel hebben op `~/.ssh/id_rsa.pub`. Als u er geen hebt, raadpleegt u de stappen voor [OS X en Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) of [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md).






## <a name="create-your-kubernetes-cluster"></a>Uw Kubernetes-cluster maken

Hier vindt u korte shellopdrachten voor de Azure CLI 2.0 waarmee u een cluster kunt maken. Zie [Azure CLI 2.0 gebruiken voor het maken van een Azure Container Service-cluster](container-service-create-acs-cluster-cli.md) voor meer informatie.

### <a name="create-a-resource-group"></a>Een resourcegroep maken
Als u het cluster wilt maken, moet u eerst een resourcegroep maken op een specifieke locatie. De uitvoering van de opdrachten is vergelijkbaar met het volgende:

```console
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Een cluster maken
Als u een resourcegroep hebt, kunt u een cluster maken in de groep:

```console
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$CLUSTER_NAME --dns-prefix=$DNS_PREFIX
```

> [!NOTE]
> Tijdens de implementatie uploadt de CLI `~/.ssh/id_rsa.pub` naar de Linux-VM's.
>

Als deze opdracht is voltooid, hebt u als het goed is een werkend Kubernetes-cluster.

### <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster

Hieronder vindt u Azure CLI-opdrachten waarmee u verbinding kunt maken met het Kubernetes-cluster vanaf uw clientcomputer met behulp van `kubectl`, de Kubernetes-opdrachtregelclient. Zie [Verbinding maken met een Azure Container Service-cluster](container-service-connect.md) voor meer informatie.

Als u `kubectl` nog niet hebt geïnstalleerd, kunt u dit doen met:

```console
az acs kubernetes install-cli
```

Als `kubectl` is geïnstalleerd, downloadt u met de volgende opdracht de hoofdclusterconfiguratie naar het bestand ~/.kube/config:

```console
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$SERVICE_NAME
```

U moet nu toegang hebben tot uw cluster vanaf uw computer. Probeer het volgende uit te voeren:
```console
kubectl get nodes
```

Controleer of u een lijst met de machines in het cluster kunt zien.

## <a name="create-your-first-kubernetes-service"></a>Uw eerste Kubernetes-service maken

Als u deze handleiding hebt voltooid, weet u:
 * hoe u een Docker-toepassing implementeert en voor iedereen zichtbaar maakt,
 * hoe u `kubectl exec`-opdrachten kunt uitvoeren in een container, 
 * en hoe u toegang krijgt tot het Kubernetes-dashboard.

### <a name="start-a-simple-container"></a>Een eenvoudige container starten
U kunt een eenvoudige container uitvoeren (in dit geval de Nginx-webserver) door het volgende uit te voeren:

```console
kubectl run nginx --image nginx
```

Met deze opdracht start de Docker-container Nginx in een schil op een van de knooppunten.

Om de werkende container te zien, voert u het volgende uit:

```console
kubectl get pods
```

### <a name="expose-the-service-to-the-world"></a>De service voor iedereen zichtbaar maken
Als u de service voor iedereen zichtbaar wilt maken, maakt u een Kubernetes `Service` van het type `LoadBalancer`:

```console
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

Hierdoor maakt Kubernetes een Azure Load Balancer-regel met een openbaar IP-adres. De wijziging wordt in enkele minuten doorgegeven aan de load balancer. Zie voor meer informatie [Load balance containers in a Kubernetes cluster in Azure Container Service](container-service-kubernetes-load-balancing.md) (Taakverdeling instellen voor containers in een Kubernetes-cluster in Azure Container Service).

Voer de volgende opdracht uit om de service te wijzigen van `pending` in een extern IP-adres:

```console
watch 'kubectl get svc'
```

  ![Afbeelding van de overgang van 'in behandeling' naar extern IP-adres](media/container-service-kubernetes-walkthrough/kubernetes-nginx3.png)

Als u het externe IP-adres ziet, kunt u ernaar bladeren in uw browser:

  ![Afbeelding van bladeren naar Nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


### <a name="browse-the-kubernetes-ui"></a>Naar de Kubernetes-gebruikersinterface bladeren
Als u de Kubernetes-webinterface wilt bekijken, kunt u het volgende gebruiken:

```console
kubectl proxy
```
Hiermee wordt een eenvoudige geverifieerde proxy uitgevoerd op localhost, die u kunt gebruiken om de [Kubernetes-webinterface](http://localhost:8001/ui) weer te geven. Zie voor meer informatie [Using the Kubernetes web UI with Azure Container Service](container-service-kubernetes-ui.md) (De Kubernetes-webinterface gebruiken met Azure Container Service).

![Afbeelding van Kubernetes-dashboard](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Externe sessies in uw containers
Met Kubernetes kunt u opdrachten uitvoeren in een externe Docker-container die in het cluster wordt uitgevoerd.

```console
# Get the name of your nginx pods
kubectl get pods
```

Met de naam van uw schil kunt u een externe opdracht uitvoeren op uw schil.  Bijvoorbeeld:

```console
kubectl exec nginx-701339712-retbj date
```

U kunt ook een volledig interactieve sessie openen met de `-it`-vlaggen:

```console
kubectl exec nginx-701339712-retbj -it bash
```

![Externe sessie binnen een container](media/container-service-kubernetes-walkthrough/kubernetes-remote.png)



## <a name="next-steps"></a>Volgende stappen

Om meer te doen met uw Kubernetes-cluster raadpleegt u de volgende resources:

* [Kubernetes Bootcamp](https://katacoda.com/embed/kubernetes-bootcamp/1/): hier leest u hoe u toepassingen in containers kunt implementeren, schalen en bijwerken, en hoe u fouten kunt opsporen.
* [Gebruikershandleiding voor Kubernetes](http://kubernetes.io/docs/user-guide/): bevat informatie over het uitvoeren van programma's in een bestaand Kubernetes-cluster.
* [Voorbeelden van Kubernetes](https://github.com/kubernetes/kubernetes/tree/master/examples): biedt een aantal voorbeelden voor het uitvoeren van echte toepassingen met Kubernetes.



<!--HONumber=Feb17_HO4-->


