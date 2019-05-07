---
title: Maken van virtuele knooppunten met behulp van de portal in Azure Kubernetes Services (AKS)
description: Leer hoe u de Azure portal gebruiken voor het maken van een Azure Kubernetes-Services (AKS)-cluster dat gebruik maakt van virtuele knooppunten om uit te voeren schillen.
services: container-service
author: iainfoulds
ms.topic: conceptual
ms.service: container-service
ms.date: 05/06/2019
ms.author: iainfou
ms.openlocfilehash: 4376db8cdfa90b8d29ecd9b210e683848b4c94b4
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65072589"
---
# <a name="create-and-configure-an-azure-kubernetes-services-aks-cluster-to-use-virtual-nodes-in-the-azure-portal"></a>Maken en configureren van een cluster Azure Kubernetes-Services (AKS) voor het gebruik van virtuele-knooppunten in de Azure-portal

Als u wilt snel workloads in een Azure Kubernetes Service (AKS)-cluster implementeert, kunt u virtuele-knooppunten. Met virtuele knooppunten kunt u snel inrichten van pods hebt, en betaalt alleen per seconde voor de uitvoeringstijd. In een scenario voor vergroten/verkleinen moet u niet wachten op het Kubernetes-cluster automatisch schalen om te implementeren VM-rekenknooppunten voor het uitvoeren van de extra schillen. Virtuele knooppunten worden alleen ondersteund met Linux-schillen en knooppunten.

In dit artikel wordt beschreven hoe u maken en configureren van de virtuele-netwerkbronnen en een AKS-cluster met virtuele knooppunten die zijn ingeschakeld.

## <a name="before-you-begin"></a>Voordat u begint

Virtuele knooppunten Schakel netwerkcommunicatie tussen pods in ACI met en het AKS-cluster. Voor deze communicatie een subnet van een virtueel netwerk wordt gemaakt en gedelegeerde machtigingen zijn toegewezen. Virtuele knooppunten werken alleen met AKS-clusters die zijn gemaakt met behulp van *geavanceerde* netwerken. AKS-clusters worden standaard gemaakt met *basic* netwerken. Dit artikel ziet u hoe u een virtueel netwerk en subnetten maken en implementeren van een AKS-cluster maakt gebruik van geavanceerde netwerken.

Als u ACI niet eerder hebt gebruikt, moet u de service-provider registreren met uw abonnement. U kunt de status van de ACI provider registreren met behulp van controleren de [az provider list] [ az-provider-list] opdracht, zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az provider list --query "[?contains(namespace,'Microsoft.ContainerInstance')]" -o table
```

De *Microsoft.ContainerInstance* provider wilt laten rapporteren als *geregistreerde*, zoals weergegeven in de volgende voorbeelduitvoer:

```
Namespace                    RegistrationState
---------------------------  -------------------
Microsoft.ContainerInstance  Registered
```

Als de provider wordt weergegeven als *NotRegistered*, de provider met behulp van de [az provider register] [az provider register] registreren, zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerInstance
```

## <a name="regional-availability"></a>Regionale beschikbaarheid

De volgende regio's worden ondersteund voor implementaties met virtuele knooppunten:

* Australië-Oost (australiaeast)
* VS-midden (centralus)
* VS-Oost (VS-Oost)
* VS-Oost 2 (eastus2)
* Japan-Oost (japaneast)
* North Europe (northeurope)
* Zuidoost-Azië (southeastasia)
* West-Centraal VS (westcentralus)
* West Europe (westeurope)
* VS-West (VS West)
* West US 2 (westus2)

## <a name="known-limitations"></a>Bekende beperkingen
Virtuele knooppunten functionaliteit is sterk afhankelijk van de ACI-functieset. De volgende scenario's worden nog niet ondersteund met virtuele knooppunten

* Met behulp van service-principal naar pull ACR afbeeldingen. [Tijdelijke oplossing](https://github.com/virtual-kubelet/virtual-kubelet/blob/master/providers/azure/README.md#Private-registry) is het gebruik van [Kubernetes-geheimen](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/#create-a-secret-by-providing-credentials-on-the-command-line)
* [Beperkingen op het virtuele netwerk](../container-instances/container-instances-vnet.md) met inbegrip van VNet-peering, Kubernetes-netwerkbeleid en uitgaand verkeer naar internet met netwerkbeveiligingsgroepen.
* Init containers
* [Host-aliassen](https://kubernetes.io/docs/concepts/services-networking/add-entries-to-pod-etc-hosts-with-host-aliases/)
* [Argumenten](../container-instances/container-instances-exec.md#restrictions) voor exec in ACI
* [Daemonsets](concepts-clusters-workloads.md#statefulsets-and-daemonsets) distribueert schillen niet aan de virtuele-knooppunt

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="create-an-aks-cluster"></a>Een AKS-cluster maken

Selecteer in de linkerbovenhoek van de Azure-portal **Een resource maken** > **Kubernetes-service**.

Op de **basisbeginselen** pagina, configureer de volgende opties:

- *PROJECTGEGEVENS*: Selecteer een Azure-abonnement, en selecteer of maak vervolgens een Azure-resourcegroep, zoals *myResourceGroup*. Voer een **Kubernetes-clusternaam** in, zoals *myAKSCluster*.
- *CLUSTERDETAILS*: Selecteer een regio, Kubernetes-versie en DNS-naamvoorvoegsel voor het AKS-cluster.
- *PRIMAIRE KNOOPPUNTGROEP*: Selecteer een VM-grootte voor de AKS-knooppunten. De VM-grootte kan **niet** meer worden gewijzigd als een AKS-cluster eenmaal is geïmplementeerd.
     - Selecteer het aantal knooppunten dat u in het cluster wilt implementeren. In dit artikel instellen **aantal knooppunten** naar *1*. Het aantal knooppunten kan nog **wel** worden gewijzigd als het cluster is geïmplementeerd.

Klik op **Next: Schaal**.

Op de **schaal** weergeeft, schakelt *ingeschakeld* onder **virtuele knooppunten**.

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
> Als u installatiekopieën die zijn opgeslagen in Azure Container Registry gebruikt [configureren en gebruiken van een Kubernetes-geheim][acr-aks-secrets]. Een beperking van de virtuele-knooppunten is dat u kunt geen geïntegreerde Azure AD-service-principal verificatie. Als u een geheim niet gebruikt, schillen gepland op virtuele-knooppunten niet starten en de fout melden `HTTP response status code 400 error code "InaccessibleImage"`.

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
- [Bekijk het voorbeeld voor automatisch schalen voor virtuele-knooppunten][virtual-node-autoscale]
- [Meer informatie over de Virtual Kubelet open source-bibliotheek][virtual-kubelet-repo]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[aks-github]: https://github.com/azure/aks/issues]
[virtual-node-autoscale]: https://github.com/Azure-Samples/virtual-node-autoscale
[virtual-kubelet-repo]: https://github.com/virtual-kubelet/virtual-kubelet

<!-- LINKS - internal -->
[aks-network]: ./networking-overview.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[aks-hpa]: tutorial-kubernetes-scale.md
[aks-cluster-autoscaler]: cluster-autoscaler.md
[aks-basic-ingress]: ingress-basic.md
[acr-aks-secrets]: ../container-registry/container-registry-auth-aks.md#access-with-kubernetes-secret
[az-provider-list]: /cli/azure/provider#az-provider-list