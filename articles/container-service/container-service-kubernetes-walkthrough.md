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
ms.date: 11/15/2016
ms.author: anhowe
translationtype: Human Translation
ms.sourcegitcommit: 7ed8fb75f057d5a7cfde5436e72e8fec52d07156
ms.openlocfilehash: f3b2fc301bf7083f192c0ec872c4e032472eef97


---

# <a name="microsoft-azure-container-service-engine---kubernetes-walkthrough"></a>Microsoft Azure Container Service-engine - Kennismaking met Kubernetes

## <a name="prerequisites"></a>Vereisten
In dit scenario wordt ervan uitgegaan dat u het [opdrachtregelprogramma 'azure-cli'](https://github.com/azure/azure-cli#installation) hebt geïnstalleerd en een [openbare SSH-sleutel](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) hebt gemaakt op `~/.ssh/id_rsa.pub`.

## <a name="overview"></a>Overzicht

Met de onderstaande instructies maakt u een Kubernetes-cluster met één hoofdkooppunt en twee werkknooppunten.
De master dient de Kubernetes REST-API.  De werkknooppunten worden gegroepeerd in een Azure-beschikbaarheidset en voeren uw containers uit. Alle virtuele machines bevinden zich in hetzelfde persoonlijke VNET en hebben volledige toegang tot elkaar.

> [!NOTE]
> Ondersteuning voor Kubernetes in Azure Container Service is momenteel in de preview-fase.
>

De volgende afbeelding geeft de architectuur van een clustercontainerservice weer met één master en twee agents:

![Afbeelding van Kubernetes-cluster op Azure](media/container-service-kubernetes-walkthrough/kubernetes.png)

## <a name="creating-your-kubernetes-cluster"></a>Uw Kubernetes-cluster maken

### <a name="create-a-resource-group"></a>Een resourcegroep maken
Als u het cluster wilt maken, moet u eerst een resourcegroep maken op een specifieke locatie. U doet dit met:
```console
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Een cluster maken
Als u een resourcegroep hebt, kunt u een cluster maken in de groep met:
```console
DNS_PREFIX=some-unique-value
SERVICE_NAME=any-acs-service-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$SERVICE_NAME --dns-prefix=$DNS_PREFIX
```

> [!NOTE]
> azure-cli uploadt `~/.ssh/id_rsa.pub` naar de virtuele Linux-machines.
>

Als deze opdracht is voltooid, hebt u als het goed is een werkend Kubernetes-cluster.

### <a name="configure-kubectl"></a>Kubectl configureren
`kubectl` is de Kubernetes-opdrachtregelclient.  Als u deze nog niet hebt geïnstalleerd, kunt u dit doen met:

```console
az acs kubernetes install-cli
```

Als `kubectl` is geïnstalleerd, downloadt u met de onderstaande opdracht de hoofdclusterconfiguratie naar het bestand ~/.kube/config
```console
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$SERVICE_NAME
```

U moet nu toegang hebben tot uw cluster vanaf uw computer. Probeer het volgende uit te voeren:
```console
kubectl get nodes
```

Controleer of u de machines in het cluster kunt zien.

## <a name="create-your-first-kubernetes-service"></a>Uw eerste Kubernetes-service maken

Als u deze handleiding hebt voltooid, weet u:
 * hoe u een Docker-toepassing implementeert en voor iedereen zichtbaar maakt,
 * hoe u `kubectl exec`-opdrachten kunt uitvoeren in een container, 
 * en hoe u toegang krijgt tot het Kubernetes-dashboard.

### <a name="start-a-simple-container"></a>Een eenvoudige container starten
U kunt een eenvoudige container uitvoeren (in dit geval de `nginx`-webserver) door het volgende uit te voeren:

```console
kubectl run nginx --image nginx
```

Met deze opdracht start de Docker-container nginx in een schil op een van de knooppunten.

U kunt het volgende uitvoeren
```console
kubectl get pods
```

om de werkende container te zien.

### <a name="expose-the-service-to-the-world"></a>De service voor iedereen zichtbaar maken
Ga als volgt te werk om de service voor iedereen zichtbaar maken.  U maakt een Kubernetes `Service` van het type `LoadBalancer`:

```console
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

Hierdoor maakt Kubernetes een Azure Load Balancer met een openbaar IP-adres. De wijziging wordt in ongeveer 2-3 minuten doorgegeven aan de load balancer.

Typ het volgende om de service te wijzigen van 'in behandeling' in een extern IP-adres:
```console
watch 'kubectl get svc'
```

  ![Afbeelding van de overgang van in behandeling naar extern IP-adres](media/container-service-kubernetes-walkthrough/kubernetes-nginx3.png)

Als u het externe IP-adres ziet, kunt u ernaar bladeren in uw browser:

  ![Afbeelding van bladeren naar nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


### <a name="browse-the-kubernetes-ui"></a>Naar de Kubernetes-gebruikersinterface bladeren
Als u de Kubernetes-webinterface wilt bekijken, kunt u het volgende gebruiken:

```console
kubectl proxy
```
Hiermee wordt een eenvoudige geverifieerde proxy uitgevoerd op localhost, die u kunt gebruiken om de [kubernetes-ui](http://localhost:8001/ui) weer te geven

![Afbeelding van Kubernetes-dashboard](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Externe sessies in uw containers
Met Kubernetes kunt u opdrachten uitvoeren in een externe Docker-container die in het cluster wordt uitgevoerd.

```console
# Get the name of your nginx pod
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

![Afbeelding van curl naar podIP](media/container-service-kubernetes-walkthrough/kubernetes-remote.png)


## <a name="details"></a>Details
### <a name="installing-the-kubectl-configuration-file"></a>Het configuratiebestand kubectl installeren
Tijdens het uitvoeren van `az acs kubernetes get-credentials` is het configuratiebestand kube voor externe toegang opgeslagen in de basismap ~/.kube/config.

Als u het ooit rechtstreeks moet downloaden, kunt u `ssh` gebruiken in Linux of OS X, of `Putty` in Windows:

#### <a name="windows"></a>Windows
Ga als volgt te werk om pscp vanuit [putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) te gebruiken.  Zorg ervoor dat uw certificaat toegankelijk is via [pageant](https://github.com/Azure/acs-engine/blob/master/docs/ssh.md#key-management-and-agent-forwarding-with-windows-pageant):
  ```
  # MASTERFQDN is obtained in step1
  pscp azureuser@MASTERFQDN:.kube/config .
  SET KUBECONFIG=%CD%\config
  kubectl get nodes
  ```

#### <a name="os-x-or-linux"></a>OS X of Linux:
  ```
  # MASTERFQDN is obtained in step1
  scp azureuser@MASTERFQDN:.kube/config .
  export KUBECONFIG=`pwd`/config
  kubectl get nodes
  ```
## <a name="learning-more"></a>Meer informatie

### <a name="azure-container-service"></a>Azure Container Service

1. [Documentatie over Azure Container Service](https://azure.microsoft.com/en-us/documentation/services/container-service/)
2. [Open Source-engine van Azure Container Service ](https://github.com/azure/acs-engine)

### <a name="kubernetes-community-documentation"></a>Documentatie van de Kubernetes-gemeenschap

1. [Kubernetes Bootcamp](https://katacoda.com/embed/kubernetes-bootcamp/1/): hier leest u hoe u toepassingen in containers kunt implementeren, schalen en bijwerken, en hoe u fouten kunt opsporen.
2. [Gebruikershandleiding voor Kubernetes](http://kubernetes.io/docs/user-guide/): bevat informatie over het uitvoeren van programma's in een bestaand Kubernetes-cluster.
3. [Voorbeelden van Kubernetes](https://github.com/kubernetes/kubernetes/tree/master/examples): biedt een aantal voorbeelden voor het uitvoeren van echte toepassingen met Kubernetes.



<!--HONumber=Jan17_HO4-->


