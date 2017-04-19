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
ms.date: 04/05/2017
ms.author: anhowe
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 5c529ae41b42d276d37e6103305e33ed04694e18
ms.lasthandoff: 04/07/2017

---

# <a name="get-started-with-a-kubernetes-cluster-in-container-service"></a>Aan de slag met een Kubernetes-cluster in Container Service


In dit scenario wordt beschreven hoe u met opdrachten uit de Azure CLI 2.0 een Kubernetes-cluster in Azure-Container Service maakt. Vervolgens kunt u het opdrachtregelprogramma `kubectl` gebruiken om aan de slag te gaan met containers in het cluster.

In de volgende afbeelding ziet u de architectuur van een Container Service-cluster met één master en twee agents. De master dient de Kubernetes REST-API. De agentknooppunten worden gegroepeerd in een Azure-beschikbaarheidsset en voeren uw containers uit. Alle virtuele machines bevinden zich in hetzelfde virtuele privénetwerk en hebben volledige toegang tot elkaar.

![Afbeelding van Kubernetes-cluster in Azure](media/container-service-kubernetes-walkthrough/kubernetes.png)

## <a name="prerequisites"></a>Vereisten
In dit scenario wordt ervan uitgegaan dat u de [Azure CLI 2.0](/cli/azure/install-az-cli2) hebt geïnstalleerd en geconfigureerd. 

Bij de opdrachtvoorbeelden wordt ervan uitgegaan dat u de Azure CLI uitvoert in een Bash-shell, wat gebruikelijk is in Linux en Mac OS. Als u de Azure CLI uitvoert op een Windows-client, kan de script- en bestandssyntaxis anders zijn, afhankelijk van de opdrachtshell. 

## <a name="create-your-kubernetes-cluster"></a>Uw Kubernetes-cluster maken

Hier vindt u korte shellopdrachten voor de Azure CLI 2.0 waarmee u het cluster maakt. 

### <a name="create-a-resource-group"></a>Een resourcegroep maken
Als u het cluster wilt maken, moet u eerst een resourcegroep maken op een specifieke locatie. De uitvoering van de opdrachten is vergelijkbaar met het volgende:

```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Een cluster maken
Als u een resourcegroep hebt, kunt u een cluster maken in die groep. In het volgende voorbeeld wordt de optie `--generate-ssh-keys` gebruikt. Hiermee worden de benodigde openbare en persoonlijke SSH-sleutelbestanden voor de implementatie gegenereerd als deze nog niet bestaan in de standaard `~/.ssh/`-directory. 

Met deze opdracht wordt ook automatisch de [Azure Active Directory-service-principal](container-service-kubernetes-service-principal.md) gegenereerd die nodig is voor een Kubernetes-cluster in Azure.

```azurecli
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$CLUSTER_NAME --dns-prefix=$DNS_PREFIX --generate-ssh-keys
```


Na enkele minuten is de opdracht voltooid en hebt u een werkend Kubernetes-cluster.

### <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster

Gebruik [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/), de Kubernetes-opdrachtregelclient, als u vanaf uw clientcomputer verbinding wilt maken met het Kubernetes-cluster. 

Als u `kubectl` nog niet hebt geïnstalleerd, kunt u dit doen met:

```azurecli
sudo az acs kubernetes install-cli
```
> [!TIP]
> Deze opdracht installeert standaard het binaire `kubectl`-bestand in `/usr/local/bin/kubectl` op een Linux- of Mac OS-systeem, of in `C:\Program Files (x86)\kubectl.exe` in Windows. Gebruik de parameter `--install-location` als u een ander installatiepad wilt opgeven.
>

Wanneer `kubectl` is geïnstalleerd, controleert u of de directory in het systeempad staat. Is dit niet het geval, voeg deze dan toe. 


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

### <a name="start-a-simple-container"></a>Een eenvoudige container starten
U kunt een eenvoudige container uitvoeren (in dit geval de Nginx-webserver) door het volgende uit te voeren:

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
Deze opdracht voert een eenvoudige geverifieerde proxy uit op localhost, die u kunt gebruiken om de Kubernetes-webinterface weer te geven, die wordt uitgevoerd op [http://localhost:8001/ui](http://localhost:8001/ui). Zie voor meer informatie [Using the Kubernetes web UI with Azure Container Service](container-service-kubernetes-ui.md) (De Kubernetes-webinterface gebruiken met Azure Container Service).

![Afbeelding van Kubernetes-dashboard](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Externe sessies in uw containers
Met Kubernetes kunt u opdrachten uitvoeren in een externe Docker-container die in het cluster wordt uitgevoerd.

```bash
# Get the name of your nginx pods
kubectl get pods
```

Met de naam van uw schil kunt u een externe opdracht uitvoeren op uw schil.  Bijvoorbeeld:

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

