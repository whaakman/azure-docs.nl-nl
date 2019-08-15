---
title: Virtuele-Kubelet uitvoeren in een Azure Kubernetes service-cluster (AKS)
description: Meer informatie over het gebruik van Virtual Kubelet met Azure Kubernetes service (AKS) voor het uitvoeren van Linux-en Windows-containers op Azure Container Instances.
services: container-service
author: mlearned
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: mlearned
ms.openlocfilehash: f18992be353d2d6cc739412d98ccd97d5e78d4c7
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "67613862"
---
# <a name="use-virtual-kubelet-with-azure-kubernetes-service-aks"></a>Virtuele-Kubelet gebruiken met Azure Kubernetes service (AKS)

Azure Container Instances (ACI) bieden een gehoste omgeving voor het uitvoeren van containers in Azure. Wanneer u ACI gebruikt, is het niet nodig om de onderliggende Compute-infra structuur te beheren, maar dit beheer wordt door Azure afgehandeld. Bij het uitvoeren van containers in ACI worden kosten in rekening gebracht op basis van de seconde voor elke container die wordt uitgevoerd.

Wanneer u de virtuele-Kubelet provider voor Azure Container Instances gebruikt, kunnen Linux-en Windows-containers worden gepland op een container exemplaar alsof het een standaard Kubernetes-knoop punt is. Met deze configuratie kunt u profiteren van de mogelijkheden van Kubernetes en de beheer waarde en kosten voordelen van container exemplaren.

> [!NOTE]
> AKS heeft nu ingebouwde ondersteuning voor het plannen van containers op ACI, *virtuele knoop punten*genoemd. Deze virtuele knoop punten ondersteunen momenteel Linux-container exemplaren. Als u Windows-container exemplaren wilt plannen, kunt u virtuele-Kubelet blijven gebruiken. Anders moet u virtuele knoop punten gebruiken in plaats van de hand matige instructies voor de virtuele-Kubelet die in dit artikel worden vermeld. U kunt aan de slag met virtuele knoop punten met behulp van [Azure cli][virtual-nodes-cli] of [Azure Portal][virtual-nodes-portal].
>
> De virtuele-Kubelet is een experimenteel open-source project dat als zodanig moet worden gebruikt. Voor het bijdragen, het oplossen van problemen en het lezen van de virtuele-kubelet raadpleegt u het [github-project van Virtual kubelet][vk-github].

## <a name="before-you-begin"></a>Voordat u begint

In dit document wordt ervan uitgegaan dat u een AKS-cluster hebt. Als u een AKS-cluster nodig hebt, raadpleegt u de [Snelstartgids voor Azure Kubernetes service (AKS)][aks-quick-start].

U hebt ook de Azure CLI-versie **2.0.65** of hoger nodig. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

Als u de virtuele-Kubelet wilt installeren, installeert en configureert u [helm][aks-helm] in uw AKS-cluster. Zorg ervoor dat uw Tiller is [geconfigureerd voor gebruik met KUBERNETES RBAC](#for-rbac-enabled-clusters), indien nodig.

### <a name="register-container-instances-feature-provider"></a>Container Instances feature provider registreren

Als u de Azure container instance-service (ACI) niet eerder hebt gebruikt, registreert u de service provider bij uw abonnement. U kunt de status van de ACI-provider registratie controleren met de opdracht [AZ provider List][az-provider-list] , zoals wordt weer gegeven in het volgende voor beeld:

```azurecli-interactive
az provider list --query "[?contains(namespace,'Microsoft.ContainerInstance')]" -o table
```

De provider van *micro soft. ContainerInstance* meldt zich aan als *geregistreerd*, zoals wordt weer gegeven in de volgende voorbeeld uitvoer:

```console
Namespace                    RegistrationState
---------------------------  -------------------
Microsoft.ContainerInstance  Registered
```

Als de provider wordt weer gegeven als *NotRegistered*, registreert u de provider met behulp van de [AZ provider REGI ster][az-provider-register] , zoals wordt weer gegeven in het volgende voor beeld:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerInstance
```

### <a name="for-rbac-enabled-clusters"></a>Voor RBAC-clusters

Als voor uw AKS-cluster RBAC is ingeschakeld, moet u een service account en een functie binding maken voor gebruik met Tiller. Zie voor meer informatie [helm toegangs beheer op basis van rollen][helm-rbac]. Als u een service account en een verbindingsrol wilt maken, maakt u een bestand met de naam *RBAC-Virtual-kubelet. yaml* en plakt u de volgende definitie:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Pas het service account en de binding met [kubectl toe][kubectl-apply] en geef het bestand *RBAC-virtueel-kubelet. yaml* op, zoals wordt weer gegeven in het volgende voor beeld:

```console
$ kubectl apply -f rbac-virtual-kubelet.yaml

clusterrolebinding.rbac.authorization.k8s.io/tiller created
```

Helm configureren voor het gebruik van het Tiller-service account:

```console
helm init --service-account tiller
```

U kunt nu door gaan met het installeren van de virtuele-Kubelet in uw AKS-cluster.

## <a name="installation"></a>Installatie

Gebruik de opdracht [AZ AKS install-connector][aks-install-connector] om virtuele Kubelet te installeren. In het volgende voor beeld worden zowel de Linux-als de Windows-connector geïmplementeerd.

```azurecli-interactive
az aks install-connector \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --connector-name virtual-kubelet \
    --os-type Both
```

Deze argumenten zijn beschikbaar voor de opdracht [AZ AKS install-connector][aks-install-connector] .

| Instelt | Description | Vereist |
|---|---|:---:|
| `--connector-name` | De naam van de ACI-connector.| Ja |
| `--name` `-n` | De naam van het beheerde cluster. | Ja |
| `--resource-group` `-g` | De naam van de resource groep. | Ja |
| `--os-type` | Type besturings systeem container exemplaren. Toegestane waarden: Zowel Linux, Windows. Standaard: Linux. | Nee |
| `--aci-resource-group` | De resource groep waarin de ACI-container groepen moeten worden gemaakt. | Nee |
| `--location` `-l` | De locatie voor het maken van de ACI-container groepen. | Nee |
| `--service-principal` | De service-principal die wordt gebruikt voor verificatie bij Azure-Api's. | Nee |
| `--client-secret` | Geheim dat is gekoppeld aan de Service-Principal. | Nee |
| `--chart-url` | URL van een helm-diagram dat ACI-connector installeert. | Nee |
| `--image-tag` | De afbeeldings code van de container installatie kopie van de virtuele-kubelet. | Nee |

## <a name="validate-virtual-kubelet"></a>Virtuele-Kubelet valideren

Als u wilt controleren of de virtuele-Kubelet is geïnstalleerd, retourneert u een lijst met Kubernetes-knoop punten met behulp van de [kubectl opdracht knoop punten ophalen][kubectl-get] :

```console
$ kubectl get nodes

NAME                                             STATUS   ROLES   AGE   VERSION
aks-nodepool1-56577038-0                         Ready    agent   11m   v1.12.8
virtual-kubelet-virtual-kubelet-linux-eastus     Ready    agent   39s   v1.13.1-vk-v0.9.0-1-g7b92d1ee-dev
virtual-kubelet-virtual-kubelet-windows-eastus   Ready    agent   37s   v1.13.1-vk-v0.9.0-1-g7b92d1ee-dev
```

## <a name="run-linux-container"></a>Linux-container uitvoeren

Maak een bestand met `virtual-kubelet-linux.yaml` de naam en kopieer de volgende YAML. Houd er rekening mee dat er een [nodeSelector][node-selector] en- [tolerantie][toleration] wordt gebruikt om de container op het knoop punt te plannen.

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
        beta.kubernetes.io/os: linux
        kubernetes.io/role: agent
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Equal
        value: azure
        effect: NoSchedule
```

Voer de toepassing uit met de opdracht [kubectl Create][kubectl-create] .

```console
kubectl create -f virtual-kubelet-linux.yaml
```

Gebruik de opdracht [kubectl Get peul][kubectl-get] with het `-o wide` argument voor het uitvoeren van een lijst met peulen met het geplande knoop punt. U ziet dat `aci-helloworld` de Pod is gepland op het `virtual-kubelet-virtual-kubelet-linux` knoop punt.

```console
$ kubectl get pods -o wide

NAME                              READY   STATUS    RESTARTS   AGE     IP               NODE
aci-helloworld-7b9ffbf946-rx87g   1/1     Running   0          22s     52.224.147.210   virtual-kubelet-virtual-kubelet-linux-eastus
```

## <a name="run-windows-container"></a>Windows-container uitvoeren

Maak een bestand met `virtual-kubelet-windows.yaml` de naam en kopieer de volgende YAML. Houd er rekening mee dat er een [nodeSelector][node-selector] en- [tolerantie][toleration] wordt gebruikt om de container op het knoop punt te plannen.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nanoserver-iis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nanoserver-iis
  template:
    metadata:
      labels:
        app: nanoserver-iis
    spec:
      containers:
      - name: nanoserver-iis
        image: microsoft/iis:nanoserver
        ports:
        - containerPort: 80
      nodeSelector:
        beta.kubernetes.io/os: windows
        kubernetes.io/role: agent
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Equal
        value: azure
        effect: NoSchedule
```

Voer de toepassing uit met de opdracht [kubectl Create][kubectl-create] .

```console
kubectl create -f virtual-kubelet-windows.yaml
```

Gebruik de opdracht [kubectl Get peul][kubectl-get] with het `-o wide` argument voor het uitvoeren van een lijst met peulen met het geplande knoop punt. U ziet dat `nanoserver-iis` de Pod is gepland op het `virtual-kubelet-virtual-kubelet-windows` knoop punt.

```console
$ kubectl get pods -o wide

NAME                              READY   STATUS    RESTARTS   AGE     IP               NODE
nanoserver-iis-5d999b87d7-6h8s9   1/1     Running   0          47s     52.224.143.39    virtual-kubelet-virtual-kubelet-windows-eastus
```

## <a name="remove-virtual-kubelet"></a>Virtuele-Kubelet verwijderen

Gebruik de opdracht [AZ AKS Remove-connector][aks-remove-connector] om de virtuele-Kubelet te verwijderen. Vervang de argument waarden door de naam van de connector, het AKS-cluster en de cluster resource groep AKS.

```azurecli-interactive
az aks remove-connector \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --connector-name virtual-kubelet \
    --os-type Both
```

> [!NOTE]
> Als er fouten optreden bij het verwijderen van beide OS-connectors of als u alleen de Windows-of Linux OS-connector wilt verwijderen, kunt u het type besturings systeem hand matig opgeven. Voeg de `--os-type` para meter toe aan `az aks remove-connector` de vorige opdracht en `Windows` Geef `Linux`of op.

## <a name="next-steps"></a>Volgende stappen

Zie de [bekende quirks en tijdelijke oplossingen][vk-troubleshooting]voor mogelijke problemen met de virtuele-Kubelet. Als u problemen met de virtuele-Kubelet wilt melden, [opent u een github-probleem][vk-issues].

Meer informatie over Virtual Kubelet vindt u in het [github-project van Virtual Kubelet][vk-github].

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-remove-connector]: /cli/azure/aks#az-aks-remove-connector
[az-container-list]: /cli/azure/aks#az-aks-list
[aks-install-connector]: /cli/azure/aks#az-aks-install-connector
[virtual-nodes-cli]: virtual-nodes-cli.md
[virtual-nodes-portal]: virtual-nodes-portal.md
[aks-helm]: kubernetes-helm.md
[az-provider-list]: /cli/azure/provider#az-provider-list
[az-provider-register]: /cli/azure/provider#az-provider-register

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[vk-github]: https://github.com/virtual-kubelet/virtual-kubelet
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[vk-troubleshooting]: https://github.com/virtual-kubelet/virtual-kubelet#known-quirks-and-workarounds
[vk-issues]: https://github.com/virtual-kubelet/virtual-kubelet/issues
