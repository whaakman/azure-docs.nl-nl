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
ms.date: 05/08/2017
ms.author: anhowe
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 2ec155129374c03ba7e0ecaa5d2bf29a1d3111aa
ms.contentlocale: nl-nl
ms.lasthandoff: 05/10/2017

---

# <a name="get-started-with-a-kubernetes-cluster-in-container-service"></a>Aan de slag met een Kubernetes-cluster in Container Service


In dit scenario wordt beschreven hoe u met opdrachten uit de Azure CLI 2.0 een Kubernetes-cluster in Azure-Container Service maakt. Vervolgens kunt u het opdrachtregelprogramma `kubectl` gebruiken om aan de slag te gaan met containers in het cluster.

In de volgende afbeelding ziet u de architectuur van een Container Service-cluster met één Linux-master en twee Linux-agents. De master dient de Kubernetes REST-API. De agentknooppunten worden gegroepeerd in een Azure-beschikbaarheidsset en voeren uw containers uit. Alle virtuele machines bevinden zich in hetzelfde virtuele privénetwerk en hebben volledige toegang tot elkaar.

![Afbeelding van Kubernetes-cluster in Azure](media/container-service-kubernetes-walkthrough/kubernetes.png)

Zie voor meer achtergrondinformatie de [Inleiding over Azure Container Service](container-service-intro.md) en de [Kubernetes-documentatie](https://kubernetes.io/docs/home/).

## <a name="prerequisites"></a>Vereisten
Als u een Azure Container Service-cluster wilt maken met Azure CLI 2.0, moet u:
* beschikken over een Azure-account ([krijg een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/))
* [Azure CLI 2.0](/cli/azure/install-az-cli2) hebben geïnstalleerd en ingesteld

Bovendien hebt u de volgende zaken nodig (of u gebruikt de Azure CLI om deze automatisch te genereren tijdens de implementatie van het cluster):

* **Openbare SSH RSA-sleutel**: als u SSH RSA-sleutels (Secure Shell) vooraf wilt maken, raadpleegt u de richtlijnen voor [macOS en Linux](../virtual-machines/linux/mac-create-ssh-keys.md) of [Windows](../virtual-machines/linux/ssh-from-windows.md). 

* **Service-principalclient-id en -geheim**: zie [Informatie over de service-principal voor een Kubernetes-cluster](container-service-kubernetes-service-principal.md) voor stappen voor het maken van een service-principal voor Azure Active Directory en extra informatie.

 Het opdrachtvoorbeeld in dit artikel genereert de SSH-sleutels en service-principal automatisch.

## <a name="create-your-kubernetes-cluster"></a>Uw Kubernetes-cluster maken

Hier vindt u korte Bash-shellopdrachten voor de Azure CLI 2.0 waarmee u het cluster maakt. 

### <a name="create-a-resource-group"></a>Een resourcegroep maken
Als u een cluster wilt maken, maakt u eerst een resourcegroep op een locatie waar de Azure Container Service [beschikbaar](https://azure.microsoft.com/regions/services/) is. De uitvoering van de opdrachten is vergelijkbaar met het volgende:

```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Een cluster maken
Maak een Kubernetes-cluster in de resourcegroep met behulp van de `az acs create`-opdracht met `--orchestrator-type=kubernetes`. Zie de `az acs create` [Help](/cli/azure/acs#create) voor de opdrachtsyntaxis.

Deze versie van de opdracht genereert automatisch de SSH-RSA-sleutels en service-principal voor het Kubernetes-cluster.



```azurecli
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$CLUSTER_NAME --dns-prefix=$DNS_PREFIX --generate-ssh-keys
```

Na enkele minuten is de opdracht voltooid en hebt u een werkend Kubernetes-cluster.

> [!IMPORTANT]
> Als uw account geen machtigingen heeft voor het maken van de Azure AD-service-principal, genereert de opdracht een fout die vergelijkbaar is met `Insufficient privileges to complete the operation.`. Voor meer informatie raadpleegt u [Informatie over de service-principal voor een Kubernetes-cluster](container-service-kubernetes-service-principal.md).
> 



### <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster

Gebruik [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/), de Kubernetes-opdrachtregelclient, als u vanaf uw clientcomputer verbinding wilt maken met het Kubernetes-cluster. 

Als u `kubectl` nog niet hebt geïnstalleerd, kunt u dit doen met `az acs kubernetes install-cli`. (U kunt dit ook downloaden via de [Kubernetes-site](https://kubernetes.io/docs/tasks/kubectl/install/).)

```azurecli
sudo az acs kubernetes install-cli
```

> [!TIP]
> Deze opdracht installeert standaard het binaire `kubectl`-bestand in `/usr/local/bin/kubectl` op een Linux- of Mac OS-systeem, of in `C:\Program Files (x86)\kubectl.exe` in Windows. Gebruik de parameter `--install-location` als u een ander installatiepad wilt opgeven.
>
> Wanneer `kubectl` is geïnstalleerd, controleert u of de directory in het systeempad staat. Is dit niet het geval, voeg deze dan toe. 
>


Voeg daarna de volgende opdracht uit om de configuratie van het Kubernetes-hoofdcluster te downloaden naar het bestand `~/.kube/config`:

```azurecli
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

Zie [Verbinding maken met een Azure Container Service-cluster](container-service-connect.md) voor meer opties bij de installatie en configuratie van `kubectl`.

U moet nu toegang hebben tot uw cluster vanaf uw computer. Probeer het volgende uit te voeren:

```bash
kubectl get nodes
```

Controleer of u een lijst met de machines in het cluster kunt zien.

## <a name="create-your-first-kubernetes-service"></a>Uw eerste Kubernetes-service maken

Als u deze handleiding hebt voltooid, weet u:
* hoe u een Docker-toepassing implementeert en voor iedereen zichtbaar maakt,
* hoe u `kubectl exec`-opdrachten kunt uitvoeren in een container, 
* en hoe u toegang krijgt tot het Kubernetes-dashboard.

### <a name="start-a-container"></a>Een container openen
U kunt een container uitvoeren (in dit geval de Nginx-webserver) door het volgende uit te voeren:

```bash
kubectl run nginx --image nginx
```

Met deze opdracht start de Docker-container Nginx in een schil op een van de knooppunten.

Om de werkende container te zien, voert u het volgende uit:

```bash
kubectl get pods
```

### <a name="expose-the-service-to-the-world"></a>De service voor iedereen zichtbaar maken
Als u de service voor iedereen zichtbaar wilt maken, maakt u een Kubernetes `Service` van het type `LoadBalancer`:

```bash
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

Deze opdracht zorgt ervoor dat Kubernetes een Azure Load Balancer-regel met een openbaar IP-adres maakt. De wijziging wordt in enkele minuten doorgegeven aan de load balancer. Zie voor meer informatie [Load balance containers in a Kubernetes cluster in Azure Container Service](container-service-kubernetes-load-balancing.md) (Taakverdeling instellen voor containers in een Kubernetes-cluster in Azure Container Service).

Voer de volgende opdracht uit om de service te wijzigen van `pending` in een extern IP-adres:

```bash
watch 'kubectl get svc'
```

  ![Afbeelding van de overgang van 'in behandeling' naar extern IP-adres](media/container-service-kubernetes-walkthrough/kubernetes-nginx3.png)

Als u het externe IP-adres ziet, kunt u ernaar bladeren in uw browser:

  ![Afbeelding van bladeren naar Nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


### <a name="browse-the-kubernetes-ui"></a>Naar de Kubernetes-gebruikersinterface bladeren
Als u de Kubernetes-webinterface wilt bekijken, kunt u het volgende gebruiken:

```bash
kubectl proxy
```
Met deze opdracht voert u een geverifieerde proxy uit op localhost, die u kunt gebruiken om de Kubernetes-webinterface weer te geven die wordt uitgevoerd op [http://localhost:8001/ui](http://localhost:8001/ui). Zie voor meer informatie [Using the Kubernetes web UI with Azure Container Service](container-service-kubernetes-ui.md) (De Kubernetes-webinterface gebruiken met Azure Container Service).

![Afbeelding van Kubernetes-dashboard](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Externe sessies in uw containers
Met Kubernetes kunt u opdrachten uitvoeren in een externe Docker-container die in het cluster wordt uitgevoerd.

```bash
# Get the name of your nginx pods
kubectl get pods
```

Met de naam van uw schil kunt u een externe opdracht uitvoeren op uw schil. Bijvoorbeeld:

```bash
kubectl exec <pod name> date
```

U kunt ook een volledig interactieve sessie openen met de `-it`-vlaggen:

```bash
kubectl exec <pod name> -it bash
```

![Externe sessie binnen een container](media/container-service-kubernetes-walkthrough/kubernetes-remote.png)



## <a name="next-steps"></a>Volgende stappen

Om meer te doen met uw Kubernetes-cluster raadpleegt u de volgende resources:

* [Kubernetes Bootcamp](https://katacoda.com/embed/kubernetes-bootcamp/1/): hier leest u hoe u toepassingen in containers kunt implementeren, schalen en bijwerken, en hoe u fouten kunt opsporen.
* [Gebruikershandleiding voor Kubernetes](http://kubernetes.io/docs/user-guide/): bevat informatie over het uitvoeren van programma's in een bestaand Kubernetes-cluster.
* [Voorbeelden van Kubernetes](https://github.com/kubernetes/kubernetes/tree/master/examples): biedt een aantal voorbeelden voor het uitvoeren van echte toepassingen met Kubernetes.

