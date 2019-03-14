---
title: Maken van virtuele knooppunten met behulp van de portal in Azure Kubernetes Services (AKS)
description: Leer hoe u de Azure portal gebruiken voor het maken van een Azure Kubernetes-Services (AKS)-cluster dat gebruik maakt van virtuele knooppunten om uit te voeren schillen.
services: container-service
author: iainfoulds
ms.topic: conceptual
ms.service: container-service
ms.date: 12/03/2018
ms.author: iainfou
ms.openlocfilehash: 550918292d8cf8baca9403cdc687f336672150f7
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57790867"
---
# <a name="create-and-configure-an-azure-kubernetes-services-aks-cluster-to-use-virtual-nodes-in-the-azure-portal"></a>Maken en configureren van een cluster Azure Kubernetes-Services (AKS) voor het gebruik van virtuele-knooppunten in de Azure-portal

Als u wilt snel workloads in een Azure Kubernetes Service (AKS)-cluster implementeert, kunt u virtuele-knooppunten. Met virtuele knooppunten kunt u snel inrichten van pods hebt, en betaalt alleen per seconde voor de uitvoeringstijd. In een scenario voor vergroten/verkleinen moet u niet wachten op het Kubernetes-cluster automatisch schalen om te implementeren VM-rekenknooppunten voor het uitvoeren van de extra schillen. In dit artikel wordt beschreven hoe u maken en configureren van de virtuele-netwerkbronnen en een AKS-cluster met virtuele knooppunten die zijn ingeschakeld.

> [!IMPORTANT]
> Virtuele voor AKS-knooppunten zijn momenteel in **preview**. Previews worden voor u beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Sommige aspecten van deze functie worden mogelijk nog gewijzigd voordat de functie algemeen beschikbaar wordt.

## <a name="preview-limitations"></a>Preview-beperkingen

Hoewel deze functie nog in Preview-versie, worden de volgende regio's worden ondersteund voor implementaties:

* Australië-Oost (australiaeast)
* VS-Oost (VS-Oost)
* West-Centraal VS (westcentralus)
* West Europe (westeurope)
* VS-West (VS West)

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="create-an-aks-cluster"></a>Een AKS-cluster maken

Selecteer in de linkerbovenhoek van de Azure-portal **Een resource maken** > **Kubernetes-service**.

Op de **basisbeginselen** pagina, configureer de volgende opties:

- *PROJECTGEGEVENS*: Selecteer een Azure-abonnement, en selecteer of maak vervolgens een Azure-resourcegroep, zoals *myResourceGroup*. Voer een **Kubernetes-clusternaam** in, zoals *myAKSCluster*.
- *CLUSTERDETAILS*: Selecteer een regio, Kubernetes-versie en DNS-naamvoorvoegsel voor het AKS-cluster.
- *SCHAAL*: Selecteer een VM-grootte voor de AKS-knooppunten. De VM-grootte kan **niet** meer worden gewijzigd als een AKS-cluster eenmaal is geïmplementeerd.
    - Selecteer het aantal knooppunten dat u in het cluster wilt implementeren. In dit artikel instellen **aantal knooppunten** naar *1*. Het aantal knooppunten kan nog **wel** worden gewijzigd als het cluster is geïmplementeerd.
    - Onder **virtuele knooppunten**, selecteer *ingeschakeld*.

![AKS-cluster maken en inschakelen van de virtuele-knooppunten](media/virtual-nodes-portal/enable-virtual-nodes.png)

Standaard wordt een Azure Active Directory service-principal gemaakt. Deze service-principal wordt gebruikt voor communicatie binnen het cluster en de integratie met andere Azure-services.

Het cluster wordt ook geconfigureerd voor geavanceerde netwerken. De virtuele-knooppunten zijn geconfigureerd voor gebruik van hun eigen subnet met virtuele Azure-netwerk. Dit subnet is gemachtigd voor het verbinding maken met Azure-resources tussen het AKS-cluster. Als u gedelegeerde subnet nog geen hebt, wordt de Azure portal maakt en configureert de Azure-netwerk en subnet voor gebruik met de virtuele-knooppunten.

Selecteer **Controleren + maken**. Nadat de validatie voltooid is, selecteert u **maken**.

Het duurt enkele minuten om het AKS-cluster te maken en voor te bereiden voor gebruik.

## <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster

Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Als u een Kubernetes-cluster wilt beheren, gebruikt u [kubectl][kubectl], de Kubernetes-opdrachtregelclient. De client `kubectl` is vooraf geïnstalleerd in Azure Cloud Shell.

Selecteer om te openen in de Cloud Shell, **uitproberen** in de rechterbovenhoek van een codeblok. U kunt Cloud Shell ook openen in een afzonderlijk browsertabblad door naar [https://shell.azure.com/bash](https://shell.azure.com/bash) te gaan. Klik op **Kopiëren** om de codeblokken te kopiëren, plak deze in Cloud Shell en druk vervolgens op Enter om de code uit te voeren.

Gebruik de opdracht [az aks get-credentials][az-aks-get-credentials] om `kubectl` te configureren om verbinding te maken met het Kubernetes-cluster. In het volgende voorbeeld worden de referenties opgehaald voor de clusternaam *myAKSCluster* in de resourcegroep met de naam *myResourceGroup*:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Als u de verbinding met uw cluster wilt controleren, gebruikt u de opdracht [kubectl get][kubectl-get] om een lijst met clusterknooppunten te retourneren.

```azurecli-interactive
kubectl get nodes
```

De volgende voorbeelduitvoer ziet u de enkel VM-knooppunt dat is gemaakt en vervolgens de virtuele knooppunt voor Linux, *virtuele-knooppunt-aci-linux*:

```
$ kubectl get nodes

NAME                           STATUS    ROLES     AGE       VERSION
virtual-node-aci-linux         Ready     agent     28m       v1.11.2
aks-agentpool-14693408-0       Ready     agent     32m       v1.11.2
```

## <a name="deploy-a-sample-app"></a>Een voorbeeld-app implementeren

Maak een bestand met de naam in de Azure Cloud Shell, `virtual-node.yaml` en kopieer de volgende YAML. Voor het plannen van de container op het knooppunt, een [nodeSelector] [ node-selector] en [toleration] [ toleration] zijn gedefinieerd. Deze instellingen kunnen de schil te worden gepland op de virtuele-knooppunt en te bevestigen dat de functie is ingeschakeld.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aci-helloworld
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aci-helloworld
  template:
    metadata:
      labels:
        app: aci-helloworld
    spec:
      containers:
      - name: aci-helloworld
        image: microsoft/aci-helloworld
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/role: agent
        beta.kubernetes.io/os: linux
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Exists
      - key: azure.com/aci
        effect: NoSchedule
```

Voer de toepassing met de [kubectl toepassen] [ kubectl-apply] opdracht.

```azurecli-interactive
kubectl apply -f virtual-node.yaml
```

Gebruik de [kubectl ophalen schillen] [ kubectl-get] opdracht met de `-o wide` argument voor een lijst van schillen en het knooppunt geplande uitvoer. U ziet dat de `virtual-node-helloworld` schil is gepland op de `virtual-node-linux` knooppunt.

```
$ kubectl get pods -o wide

NAME                                     READY     STATUS    RESTARTS   AGE       IP           NODE
virtual-node-helloworld-9b55975f-bnmfl   1/1       Running   0          4m        10.241.0.4   virtual-node-aci-linux
```

De schil is een interne IP-adres toegewezen vanuit het Azure-netwerksubnet overgedragen voor gebruik met virtuele-knooppunten.

> [!NOTE]
> Als u installatiekopieën die zijn opgeslagen in Azure Container Registry gebruikt [configureren en gebruiken van een Kubernetes-geheim][acr-aks-secrets]. Een beperking van de virtuele knooppunten preview is dat u kunt geen geïntegreerde verificatie van Azure AD-service-principal. Als u een geheim niet gebruikt, schillen gepland op virtuele-knooppunten niet starten en de fout melden `HTTP response status code 400 error code "InaccessibleImage"`.

## <a name="test-the-virtual-node-pod"></a>Test de schil virtueel knooppunt

Als u wilt testen de pod uitgevoerd op de virtuele-knooppunt, blader naar de demo-toepassing met een webclient. Tijdens de schil een interne IP-adres toegewezen is, kunt u deze verbinding hebben met een andere schil op het AKS-cluster snel testen. Een test-schil maken en koppelen van een terminalsessie toe:

```azurecli-interactive
kubectl run -it --rm virtual-node-test --image=debian
```

Installeer `curl` in de schil met `apt-get`:

```azurecli-interactive
apt-get update && apt-get install -y curl
```

Nu toegang tot het adres van het gebruik van uw schil `curl`, zoals *http://10.241.0.4*. Geef uw eigen interne IP-adres wordt weergegeven in de vorige `kubectl get pods` opdracht:

```azurecli-interactive
curl -L http://10.241.0.4
```

De demo-toepassing wordt weergegeven, zoals wordt weergegeven in de volgende verkorte voorbeeld-uitvoer:

```
$ curl -L 10.241.0.4

<html>
<head>
  <title>Welcome to Azure Container Instances!</title>
</head>
[...]
```

Sluit de terminal-sessie op uw schil test met `exit`. Wanneer uw sessie is beëindigd, is de schil is het verwijderd.

## <a name="next-steps"></a>Volgende stappen

In dit artikel is een schil gepland op de virtuele-knooppunt en een persoonlijke, interne IP-adres toegewezen. U kunt in plaats daarvan een service-implementatie en het routeren van netwerkverkeer maken op uw schil via een load balancer of een controller voor binnenkomend verkeer. Zie voor meer informatie, [een eenvoudige ingangscontroller in AKS maakt][aks-basic-ingress].

Virtuele knooppunten zijn onderdeel van een oplossing voor schalen in AKS. Zie voor meer informatie over oplossingen voor vergroten/verkleinen, de volgende artikelen:

- [Gebruik het Kubernetes-schil horizontale automatisch schalen][aks-hpa]
- [Gebruik het Kubernetes-cluster automatisch schalen][aks-cluster-autoscaler]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md

<!-- LINKS - internal -->
[aks-network]: ./networking-overview.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[aks-hpa]: tutorial-kubernetes-scale.md
[aks-cluster-autoscaler]: autoscaler.md
[aks-basic-ingress]: ingress-basic.md
[acr-aks-secrets]: ../container-registry/container-registry-auth-aks.md#access-with-kubernetes-secret

