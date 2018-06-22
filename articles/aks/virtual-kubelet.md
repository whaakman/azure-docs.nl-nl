---
title: Virtuele kubelet in een cluster Azure Kubernetes Service (AKS) uitgevoerd
description: Gebruik virtuele kubelet Kubernetes containers uitvoeren op Azure Containerexemplaren.
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/12/2018
ms.author: nepeters
ms.openlocfilehash: 0d046970b40f5253b07005ab578035c7c179e7df
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36304442"
---
# <a name="virtual-kubelet-with-aks"></a>Virtuele Kubelet met AKS

Azure Container exemplaren (ACI) bieden een gehoste omgeving voor het uitvoeren van containers in Azure. Wanneer u ACI gebruikt, is niet nodig voor het beheren van de onderliggende beheerinfrastructuur, Azure-beheer deze voor u verwerkt. Wanneer u containers in ACI uitvoert, wordt u in rekening gebracht door de tweede voor elke container uitgevoerd.

Wanneer u de virtuele Kubelet-provider voor exemplaren van Azure-Container, kunnen Linux- en Windows-containers worden gepland op een exemplaar van de container als het een standaard Kubernetes-knooppunt. Deze configuratie kunt u profiteren van de mogelijkheden van Kubernetes en het beheer waarde en de kosten van het voordeel van containerexemplaren.

> [!NOTE]
> Virtuele Kubelet is een experimentele open source-project en als zodanig moet worden gebruikt. Als u wilt bijdragen, problemen met de bestanden en lees meer over virtuele kubelet, Zie de [virtuele Kubelet GitHub project][vk-github].

Dit documentgegevens voor het configureren van de virtuele Kubelet voor containerexemplaren voor een AKS.

## <a name="prerequisite"></a>Vereiste

Dit document wordt ervan uitgegaan dat u een AKS-cluster hebt. Als u een cluster AKS nodig hebt, raadpleegt u de [Azure Kubernetes Service (AKS) Quick Start][aks-quick-start].

U moet ook de Azure CLI versie **2.0.33** of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

[Helm](https://docs.helm.sh/using_helm/#installing-helm) is ook vereist voor de virtuele Kubelet installeren.

## <a name="installation"></a>Installatie

Gebruik de [az aks install-connector] [ aks-install-connector] opdracht voor het installeren van de virtuele Kubelet. Het volgende voorbeeld wordt de Linux- en Windows connector geïmplementeerd.

```azurecli-interactive
az aks install-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet --os-type Both
```

Deze argumenten zijn beschikbaar voor de `aks install-connector` opdracht.

| Argument: | Beschrijving | Vereist |
|---|---|:---:|
| `--connector-name` | Naam van de Connector ACI.| Ja |
| `--name` `-n` | De naam van het beheerde cluster. | Ja |
| `--resource-group` `-g` | Naam van de resourcegroep. | Ja |
| `--os-type` | Container instanties van het besturingssysteemtype. Toegestane waarden: beide, Linux, Windows. Standaard: Linux. | Nee |
| `--aci-resource-group` | De resourcegroep waarin u de container ACI groepen maken. | Nee |
| `--location` `-l` | De locatie voor het maken van de container ACI groepen. | Nee |
| `--service-principal` | Service-principal is gebruikt voor verificatie bij de Azure-API's. | Nee |
| `--client-secret` | Het geheim die is gekoppeld aan de service-principal. | Nee |
| `--chart-url` | De URL van een grafiek Helm die ACI Connector installeert. | Nee |
| `--image-tag` | De code van de installatiekopie van de installatiekopie van het virtuele kubelet-container. | Nee |

## <a name="validate-virtual-kubelet"></a>Virtuele Kubelet valideren

Retourneert een lijst van Kubernetes knooppunten gebruikt om te valideren dat virtuele Kubelet is geïnstalleerd, de [kubectl ophalen knooppunten] [ kubectl-get] opdracht.

```console
$ kubectl get nodes

NAME                                    STATUS    ROLES     AGE       VERSION
aks-nodepool1-23443254-0                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-1                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-2                Ready     agent     16d       v1.9.6
virtual-kubelet-virtual-kubelet-linux   Ready     agent     4m        v1.8.3
virtual-kubelet-virtual-kubelet-win     Ready     agent     4m        v1.8.3
```

## <a name="run-linux-container"></a>Linux-container uitvoeren

Maak een bestand met de naam `virtual-kubelet-linux.yaml` en kopieer de volgende YAML. Vervang de `kubernetes.io/hostname` waarde met de naam van de virtuele Linux-Kubelet-knooppunt. Let op dat een [nodeSelector] [ node-selector] en [toleration] [ toleration] worden gebruikt voor het plannen van de container op het knooppunt.

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

```azurecli-interactive
kubectl create -f virtual-kubelet-linux.yaml
```

Gebruik de [kubectl ophalen gehele product] [ kubectl-get] opdracht met de `-o wide` argument voor een lijst met gehele product met het geplande knooppunt uitvoer. U ziet dat de `aci-helloworld` schil is gepland op de `virtual-kubelet-virtual-kubelet-linux` knooppunt.

```console
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
aci-helloworld-2559879000-8vmjw     1/1       Running   0          39s       52.179.3.180   virtual-kubelet-virtual-kubelet-linux
```

## <a name="run-windows-container"></a>Uitvoeren van Windows-container

Maak een bestand met de naam `virtual-kubelet-windows.yaml` en kopieer de volgende YAML. Vervang de `kubernetes.io/hostname` waarde met de naam van de virtuele Windows-Kubelet-knooppunt. Let op dat een [nodeSelector] [ node-selector] en [toleration] [ toleration] worden gebruikt voor het plannen van de container op het knooppunt.

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

```azurecli-interactive
kubectl create -f virtual-kubelet-windows.yaml
```

Gebruik de [kubectl ophalen gehele product] [ kubectl-get] opdracht met de `-o wide` argument voor een lijst met gehele product met het geplande knooppunt uitvoer. U ziet dat de `nanoserver-iis` schil is gepland op de `virtual-kubelet-virtual-kubelet-win` knooppunt.

```console
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
nanoserver-iis-868bc8d489-tq4st     1/1       Running   8         21m       138.91.121.91   virtual-kubelet-virtual-kubelet-win
```

## <a name="remove-virtual-kubelet"></a>Virtuele Kubelet verwijderen

Gebruik de [az aks remove-connector] [ aks-remove-connector] opdracht virtuele Kubelet verwijderen. Vervang de argumentwaarden met de naam van de connector, AKS cluster en de AKS clusterbrongroep.

```azurecli-interactive
az aks remove-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over virtuele Kubelet op de [virtuele Kubelet Github projet][vk-github].

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-remove-connector]: /cli/azure/aks#az-aks-remove-connector
[az-container-list]: /cli/azure/aks#az_aks_list
[aks-install-connector]: /cli/azure/aks#az-aks-install-connector

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create
[kubectl-get]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#get
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[vk-github]: https://github.com/virtual-kubelet/virtual-kubelet
