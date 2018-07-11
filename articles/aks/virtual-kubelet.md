---
title: Virtual Kubelet uitvoeren in een cluster Azure Kubernetes Service (AKS)
description: Informatie over het gebruik van de Virtual Kubelet met Azure Kubernetes Service (AKS) uit te voeren van Linux en Windows-containers op Azure Container Instances.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/12/2018
ms.author: iainfou
ms.openlocfilehash: 0466f416568b2a1a82e264a8508697fc9de87287
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952475"
---
# <a name="use-virtual-kubelet-with-azure-kubernetes-service-aks"></a>Virtual Kubelet gebruiken met Azure Kubernetes Service (AKS)

Azure Container Instances (ACI) bieden een omgeving voor het uitvoeren van containers in Azure. Wanneer u ACI, er is niet nodig voor het beheren van de onderliggende infrastructuur van compute, Azure regelt dit beheer voor u. Bij het uitvoeren van containers in ACI, worden in rekening gebracht per seconde voor elke container die wordt uitgevoerd.

Wanneer u de Virtual Kubelet-provider voor Azure Container Instances, kunnen zowel Windows als Linux-containers worden gepland op een containerinstantie alsof het een standaard Kubernetes-knooppunt. Deze configuratie kunt u profiteren van de mogelijkheden van Kubernetes en het beheer van waarde en de kosten voordeel van containerinstanties.

> [!NOTE]
> Virtual Kubelet is een experimenteel open-source-project en als zodanig moet worden gebruikt. Als u wilt bijdragen, problemen met bestanden, en lees meer over de virtual kubelet, Zie de [Virtual Kubelet GitHub-project][vk-github].

Deze document informatie over het configureren van de Virtual Kubelet voor container instances voor een AKS.

## <a name="prerequisite"></a>Vereiste

Dit document wordt ervan uitgegaan dat u een AKS-cluster hebt. Als u een cluster AKS nodig hebt, raadpleegt u de [Azure Kubernetes Service (AKS)-snelstartgids][aks-quick-start].

U moet ook de Azure CLI-versie **2.0.33** of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

Voor het installeren van de Virtual Kubelet [Helm](https://docs.helm.sh/using_helm/#installing-helm) is ook vereist.

### <a name="for-rbac-enabled-clusters"></a>Voor clusters met RBAC-ingeschakeld

Als uw AKS-cluster RBAC is ingeschakeld is, moet u een service-account en een binding van de rol voor gebruik met Tiller maken. Zie voor meer informatie, [Helm rollen gebaseerd toegangsbeheer][helm-rbac].

Een *ClusterRoleBinding* moeten ook worden gemaakt voor de Virtual Kubelet. Maak een bestand met de naam voor het maken van een binding *rbac-virtualkubelet.yaml* en plak de definitie van de volgende:

```yaml
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: virtual-kubelet
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: default
  namespace: default
```

Toepassen van de binding met [kubectl toepassen] [ kubectl-apply] en geef uw *rbac-virtualkubelet.yaml* bestand, zoals weergegeven in het volgende voorbeeld:

```
$ kubectl apply -f rbac-virtual-kubelet.yaml

clusterrolebinding.rbac.authorization.k8s.io/virtual-kubelet created
```

U kunt nu doorgaan naar de Virtual Kubelet installeren in uw AKS-cluster.

## <a name="installation"></a>Installatie

Gebruik de [az aks install-connector] [ aks-install-connector] opdracht voor het installeren van de Virtual Kubelet. De Linux- en Windows-connector heeft geïmplementeerd in het volgende voorbeeld.

```azurecli-interactive
az aks install-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet --os-type Both
```

Deze argumenten zijn beschikbaar voor de `aks install-connector` opdracht.

| Argument: | Beschrijving | Vereist |
|---|---|:---:|
| `--connector-name` | Naam van de ACI-Connector.| Ja |
| `--name` `-n` | Naam van de beheerde cluster. | Ja |
| `--resource-group` `-g` | De naam van de resourcegroep. | Ja |
| `--os-type` | Type in de container instances-besturingssysteem. Toegestane waarden: beide, Linux, Windows. Standaard: Linux. | Nee |
| `--aci-resource-group` | De resourcegroep waarin u wilt maken van de groepen ACI-container. | Nee |
| `--location` `-l` | De locatie voor het maken van de groepen ACI-container. | Nee |
| `--service-principal` | Service-principal gebruikt voor verificatie bij Azure-API's. | Nee |
| `--client-secret` | Het geheim die is gekoppeld aan de service-principal. | Nee |
| `--chart-url` | URL van een Helm-grafiek die ACI-Connector installeert. | Nee |
| `--image-tag` | De tag de installatiekopie van de installatiekopie van de virtual kubelet-container. | Nee |

## <a name="validate-virtual-kubelet"></a>Valideren van de Virtual Kubelet

Retourneert een lijst met behulp van Kubernetes-knooppunten om te valideren dat de Virtual Kubelet is geïnstalleerd, de [kubectl ophalen knooppunten] [ kubectl-get] opdracht.

```
$ kubectl get nodes

NAME                                    STATUS    ROLES     AGE       VERSION
aks-nodepool1-23443254-0                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-1                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-2                Ready     agent     16d       v1.9.6
virtual-kubelet-virtual-kubelet-linux   Ready     agent     4m        v1.8.3
virtual-kubelet-virtual-kubelet-win     Ready     agent     4m        v1.8.3
```

## <a name="run-linux-container"></a>Linux-container uitvoeren

Maak een bestand met de naam `virtual-kubelet-linux.yaml` en kopieer de volgende YAML. Vervang de `kubernetes.io/hostname` waarde met de naam van de virtuele Linux-Kubelet-knooppunt. Houd er rekening mee houden dat een [nodeSelector] [ node-selector] en [toleration] [ toleration] worden gebruikt voor het plannen van de container op het knooppunt.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: aci-helloworld
spec:
  replicas: 1
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
        kubernetes.io/hostname: virtual-kubelet-virtual-kubelet-linux
      tolerations:
      - key: azure.com/aci
        effect: NoSchedule
```

Voer de toepassing met de [kubectl maken] [ kubectl-create] opdracht.

```console
kubectl create -f virtual-kubelet-linux.yaml
```

Gebruik de [kubectl ophalen schillen] [ kubectl-get] opdracht met de `-o wide` argument voor een lijst met schillen met het knooppunt geplande uitvoer. U ziet dat de `aci-helloworld` schil is gepland op de `virtual-kubelet-virtual-kubelet-linux` knooppunt.

```
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
aci-helloworld-2559879000-8vmjw     1/1       Running   0          39s       52.179.3.180   virtual-kubelet-virtual-kubelet-linux
```

## <a name="run-windows-container"></a>Windows-container uitvoeren

Maak een bestand met de naam `virtual-kubelet-windows.yaml` en kopieer de volgende YAML. Vervang de `kubernetes.io/hostname` waarde met de naam van de Virtual Kubelet van Windows-knooppunt. Houd er rekening mee houden dat een [nodeSelector] [ node-selector] en [toleration] [ toleration] worden gebruikt voor het plannen van de container op het knooppunt.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: nanoserver-iis
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: nanoserver-iis
    spec:
      containers:
      - name: nanoserver-iis
        image: nanoserver/iis
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/hostname: virtual-kubelet-virtual-kubelet-win
      tolerations:
      - key: azure.com/aci
        effect: NoSchedule
```

Voer de toepassing met de [kubectl maken] [ kubectl-create] opdracht.

```console
kubectl create -f virtual-kubelet-windows.yaml
```

Gebruik de [kubectl ophalen schillen] [ kubectl-get] opdracht met de `-o wide` argument voor een lijst met schillen met het knooppunt geplande uitvoer. U ziet dat de `nanoserver-iis` schil is gepland op de `virtual-kubelet-virtual-kubelet-win` knooppunt.

```
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
nanoserver-iis-868bc8d489-tq4st     1/1       Running   8         21m       138.91.121.91   virtual-kubelet-virtual-kubelet-win
```

## <a name="remove-virtual-kubelet"></a>Verwijder de Virtual Kubelet

Gebruik de [az aks remove-connector] [ aks-remove-connector] opdracht om te verwijderen van de Virtual Kubelet. Vervang de waarden van het argument met de naam van de connector, AKS-cluster en de resourcegroep van de AKS-cluster.

```azurecli-interactive
az aks remove-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de Virtual Kubelet op de [Virtual Kubelet Github projet][vk-github].

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-remove-connector]: /cli/azure/aks#az-aks-remove-connector
[az-container-list]: /cli/azure/aks#az_aks_list
[aks-install-connector]: /cli/azure/aks#az-aks-install-connector

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#get
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[vk-github]: https://github.com/virtual-kubelet/virtual-kubelet
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
