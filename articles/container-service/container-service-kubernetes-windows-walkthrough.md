---
title: Azure Kubernetes-cluster voor Windows | Microsoft Docs
description: Aan de slag met een Kubernetes-cluster voor Windows-containers en dit implementeren in Azure Container Service
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 18d4994f303a11e9ce2d07bc1124aaedf570fc82
ms.openlocfilehash: 4e730b65a98af05ea00c5f8ebd9914e3367b66a7
ms.contentlocale: nl-nl
ms.lasthandoff: 05/09/2017


---

# <a name="get-started-with-kubernetes-and-windows-containers-in-container-service"></a>Aan de slag met Kubernetes- en Windows-containers in Container Service


In dit artikel ziet u hoe u een Kubernetes-cluster maakt in Azure Container Service dat Windows-knooppunten bevat om Windows-containers uit te voeren. U gaat aan de slag met opdrachten uit de `az acs` Azure CLI 2.0 om het Kubernetes-cluster in Azure Container Service te maken. Vervolgens gebruikt u het opdrachtregelprogramma van Kubernetes `kubectl` om aan de slag te gaan met Windows-containers die zijn afgeleid van Docker-installatiekopieën. 

> [!NOTE]
> De ondersteuning voor Windows-containers met Kubernetes in Azure Container Service is momenteel beschikbaar als preview. 
>



In de volgende afbeelding ziet u de architectuur van een Kubernetes-cluster in Azure Container Service met één Linux-hoofdknooppunt en twee Windows-agentknooppunten. 

![Afbeelding van Kubernetes-cluster in Azure](media/container-service-kubernetes-windows-walkthrough/kubernetes-windows.png)

* Het Linus-hoofdknooppunt dient voor de Kubernetes REST-API en is toegankelijk via SSH op poort 22 of `kubectl` op poort 443. 
* De Windows-agentknooppunten worden gegroepeerd in een Azure-beschikbaarheidsset en voeren uw containers uit. De Windows-knooppunten kunnen worden geopend via een RDP SSH-tunnel via het hoofdknooppunt. Azure Load Balancer-regels worden dynamisch toegevoegd aan het cluster, afhankelijk van de weergegeven services.



Alle virtuele machines bevinden zich in hetzelfde virtuele privénetwerk en hebben volledige toegang tot elkaar. Alle virtuele machines zijn voorzien van een kubelet, een Docker en een proxy.

Zie voor meer achtergrondinformatie de [Inleiding over Azure Container Service](container-service-intro.md) en de [Kubernetes-documentatie](https://kubernetes.io/docs/home/).

## <a name="prerequisites"></a>Vereisten
Als u een Azure Container Service-cluster wilt maken met Azure CLI 2.0, moet u:
* beschikken over een Azure-account ([krijg een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/))
* [Azure CLI 2.0](/cli/azure/install-az-cli2) hebben geïnstalleerd en erbij zijn ingelogd

U hebt ook het volgende nodig voor uw Kubernetes-cluster. U kunt deze zaken vooraf voorbereiden of de `az acs create`-opdrachtopties gebruiken om ze automatisch te genereren tijdens de implementatie van het cluster. 

* **Openbare SSH RSA-sleutel**: als u SSH RSA-sleutels (Secure Shell) wilt maken, ziet u de richtlijnen voor [macOS en Linux](../virtual-machines/linux/mac-create-ssh-keys.md) of [Windows](../virtual-machines/linux/ssh-from-windows.md). 

* **Service-principalclient-id en -geheim**: zie [Informatie over de service-principal voor een Kubernetes-cluster](container-service-kubernetes-service-principal.md) voor stappen voor het maken van een service-principal voor Azure Active Directory en extra informatie.

Het opdrachtvoorbeeld in dit artikel genereert de SSH-sleutels en service-principal automatisch.
  
## <a name="create-your-kubernetes-cluster"></a>Uw Kubernetes-cluster maken

Hier vindt u Azure CLI 2.0-opdrachten voor het maken van uw cluster. 

### <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een resourcegroep op een locatie waar de Azure Container Service [beschikbaar](https://azure.microsoft.com/regions/services/) is. De volgende opdracht maakt een resourcegroep met de naam *myKubernetesResourceGroup* op de locatie *VS West*:

```azurecli
az group create --name=myKubernetesResourceGroup --location=westus
```

### <a name="create-a-kubernetes-cluster-with-windows-agent-nodes"></a>Een Kubernetes-cluster maken met Windows-agentknooppunten

Maak een Kubernetes-cluster in de resourcegroep met behulp van de `az acs create`-opdracht met `--orchestrator-type=kubernetes` en de `--windows`-agentoptie. Zie de `az acs create` [help](/cli/azure/acs#create) voor de opdrachtsyntax.

De volgende opdracht maakt u een Container Service-cluster met de naam *myKubernetesClusterName*, met het DNS-voorvoegsel *myPrefix* voor het beheerknooppunt en de opgegeven referenties om de Windows-knooppunten te bereiken. Deze versie van de opdracht genereert automatisch de SSH-RSA-sleutels en service-principal voor het Kubernetes-cluster.


```azurecli
az acs create --orchestrator-type=kubernetes \
    --resource-group myKubernetesResourceGroup \
    --name=myKubernetesClusterName \
    --dns-prefix=myPrefix \
    --agent-count=2 \
    --generate-ssh-keys \
    --windows --admin-username myWindowsAdminName \
    --admin-password myWindowsAdminPassword
```

Na enkele minuten is de opdracht voltooid en hebt u een werkend Kubernetes-cluster.

> [!IMPORTANT]
> Als uw account geen machtigingen heeft voor het maken van de Azure AD-service-principal, genereert de opdracht een fout die vergelijkbaar is met `Insufficient privileges to complete the operation.`. Voor meer informatie raadpleegt u [Informatie over de service-principal voor een Kubernetes-cluster](container-service-kubernetes-service-principal.md). 
> 

## <a name="connect-to-the-cluster-with-kubectl"></a>Verbinding maken met het cluster met kubectl

Gebruik [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/), de Kubernetes-opdrachtregelclient, als u vanaf uw clientcomputer verbinding wilt maken met het Kubernetes-cluster. 

Als u `kubectl` nog niet hebt geïnstalleerd, kunt u dit doen met `az acs kubernetes install-cli`. (U kunt dit ook downloaden via de [Kubernetes-site](https://kubernetes.io/docs/tasks/kubectl/install/).)

**Linux of macOS**

```azurecli
sudo az acs kubernetes install-cli
```

**Windows**
```azurecli
az acs kubernetes install-cli
```

> [!TIP]
> Deze opdracht installeert standaard het binaire `kubectl`-bestand in `/usr/local/bin/kubectl` op een Linux- of macOS-systeem, of `C:\Program Files (x86)\kubectl.exe` in Windows. Gebruik de parameter `--install-location` als u een ander installatiepad wilt opgeven.
>
> Wanneer `kubectl` is geïnstalleerd, controleert u of de directory in het systeempad staat. Is dit niet het geval, voeg deze dan toe. 


Voeg daarna de volgende opdracht uit om de configuratie van het Kubernetes-hoofdcluster te downloaden naar het lokale bestand `~/.kube/config`:

```azurecli
az acs kubernetes get-credentials --resource-group=myKubernetesResourceGroup --name=myKubernetesClusterName
```

U moet nu toegang hebben tot uw cluster vanaf uw computer. Probeer het volgende uit te voeren:

```bash
kubectl get nodes
```

Controleer of u een lijst met de machines in het cluster kunt zien.

![Knooppunten die worden uitgevoerd in een Kubernetes-cluster](media/container-service-kubernetes-windows-walkthrough/kubectl-get-nodes.png)

## <a name="create-your-first-kubernetes-service"></a>Uw eerste Kubernetes-service maken

Wanneer u het cluster hebt gemaakt en u verbinding hebt gemaakt met `kubectl`, kunt u een eenvoudige Windows-app openen via een Docker-container en deze beschikbaar maken op internet. In dit eenvoudige voorbeeld wordt een JSON-bestand gebruikt om een ISS-container Microsoft Internet Information Server op te geven en deze vervolgens te maken met behulp van `kubctl apply`. 

1. Maak een lokaal bestand met de naam `iis.json` en kopieer het volgende. Dit bestand vertelt Kubernetes IIS uit te voeren op Windows Server 2016 Server Core met behulp van een openbare installatiekopie van [Docker Hub](https://hub.docker.com/r/microsoft/iis/). De container gebruikt poort 80, maar is in eerste instantie alleen toegankelijk vanuit het clusternetwerk.

  ```JSON
  {
    "apiVersion": "v1",
    "kind": "Pod",
    "metadata": {
      "name": "iis",
      "labels": {
        "name": "iis"
      }
    },
    "spec": {
      "containers": [
        {
          "name": "iis",
          "image": "microsoft/iis",
          "ports": [
            {
            "containerPort": 80
            }
          ]
        }
      ],
      "nodeSelector": {
        "beta.kubernetes.io/os": "windows"
      }
    }
  }
  ```
2. Typ het volgende om de toepassing te starten:  
  
  ```bash
  kubectl apply -f iis.json
  ```  
3. Typ het volgende om de implementatie van de container bij te houden:  
  ```bash
  kubectl get pods
  ```
  Tijdens het implementeren van de container is de status `ContainerCreating`. 

  ![IIS-container in de staat ContainerCreating](media/container-service-kubernetes-windows-walkthrough/iis-pod-creating.png)   

  Vanwege de grootte van de IIS-installatiekopie duurt het enkele minuten voor de container in de staat `Running` kan worden gebracht.

  ![IIS-container in de staat Uitvoeren](media/container-service-kubernetes-windows-walkthrough/iis-pod-running.png)

4. Als u de container voor de rest van de wereld wilt weergeven, typt u de volgende opdracht:

  ```bash
  kubectl expose pods iis --port=80 --type=LoadBalancer
  ```

  Deze opdracht zorgt ervoor dat Kubernetes een Azure Load Balancer-regel met een openbaar IP-adres maakt. De wijziging wordt in enkele minuten doorgegeven aan de load balancer. Zie voor meer informatie [Load balance containers in a Kubernetes cluster in Azure Container Service](container-service-kubernetes-load-balancing.md) (Taakverdeling instellen voor containers in een Kubernetes-cluster in Azure Container Service).

5. Voer de volgende opdracht uit om de status van de service te bekijken.

  ```bash
  kubectl get svc
  ```

  Het IP-adres wordt win eerste instantie eergegeven als `pending`:

  ![Extern IP-adres in behandeling](media/container-service-kubernetes-windows-walkthrough/iis-svc-expose.png)

  Na een paar minuten is het IP-adres ingesteld:
  
  ![Extern IP-adres voor IIS](media/container-service-kubernetes-windows-walkthrough/iis-svc-expose-public.png)


6. Als het externe IP-adres beschikbaar is, kunt u ernaar bladeren in uw browser:

  ![Afbeelding van bladeren naar IIS](media/container-service-kubernetes-windows-walkthrough/kubernetes-iis.png)  

7. Als u de IIS-schil wilt verwijderen, typt u:

  ```bash
  kubectl delete pods iis
  ```

## <a name="next-steps"></a>Volgende stappen

* Voer de opdracht `kubectl proxy` uit om de Kubernetes-UI te gebruiken. Blader vervolgens naar http://localhost:8001/ui.

* Zie de informatie op [Docker Hub](https://hub.docker.com/r/microsoft/iis/) voor de stappen voor het bouwen van een aangepaste IIS-website en het uitvoeren ervan in een Windows-container.

* Voor toegang tot de Windows-knooppunten via een RDP SSH-tunnel naar het hoofdniveau met PuTTy, raadpleegt u de [ACS-Engine-documentatie](https://github.com/Azure/acs-engine/blob/master/docs/ssh.md#create-port-80-tunnel-to-the-master). 

