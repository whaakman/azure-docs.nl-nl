---
title: Maken van virtuele knooppunten met de Azure CLI in Azure Kubernetes Services (AKS)
description: Leer hoe u de Azure CLI gebruiken voor het maken van een Azure Kubernetes-Services (AKS)-cluster dat gebruik maakt van virtuele knooppunten om uit te voeren schillen.
services: container-service
author: iainfoulds
ms.service: container-service
ms.date: 12/03/2018
ms.author: iainfou
ms.openlocfilehash: ed3697cbfc605fbdecad2c1cc4b32c3ddd1e3813
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56819038"
---
# <a name="create-and-configure-an-azure-kubernetes-services-aks-cluster-to-use-virtual-nodes-using-the-azure-cli"></a>Maken en configureren van een cluster Azure Kubernetes-Services (AKS) voor het gebruik van virtuele knooppunten met de Azure CLI

Als u wilt snel schalen werkbelastingen van toepassingen in een cluster Azure Kubernetes Service (AKS), kunt u virtuele-knooppunten. Met virtuele knooppunten kunt u snelle inrichting van schillen hebt, en betaalt alleen per seconde voor de uitvoeringstijd. U hoeft niet te wachten op Kubernetes-cluster automatisch schalen om te implementeren VM-rekenknooppunten voor het uitvoeren van de extra schillen. In dit artikel wordt beschreven hoe u maken en configureren van de virtuele-netwerkbronnen en AKS-cluster en vervolgens virtuele knooppunten inschakelen.

> [!IMPORTANT]
> Virtuele voor AKS-knooppunten zijn momenteel in **preview**. Previews worden voor u beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Sommige aspecten van deze functie worden mogelijk nog gewijzigd voordat de functie algemeen beschikbaar wordt.

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

Als de provider wordt weergegeven als *NotRegistered*, Registreer de provider met behulp van de [az provider register] [ az-provider-register] zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerInstance
```

## <a name="preview-limitations"></a>Beperkingen voor Preview

Hoewel deze functie nog in Preview-versie, worden de volgende regio's worden ondersteund voor implementaties:

* Australië-Oost (australiaeast)
* VS-Oost (VS-Oost)
* West-Centraal VS (westcentralus)
* West Europe (westeurope)
* VS-West (VS West)

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell starten

Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account.

Selecteer om te openen in de Cloud Shell, **uitproberen** in de rechterbovenhoek van een codeblok. U kunt Cloud Shell ook openen in een afzonderlijk browsertabblad door naar [https://shell.azure.com/bash](https://shell.azure.com/bash) te gaan. Klik op **Kopiëren** om de codeblokken te kopiëren, plak deze in Cloud Shell en druk vervolgens op Enter om de code uit te voeren.

Als u liever om te installeren en de CLI lokaal gebruikt, in dit artikel gebruikmaken van Azure CLI versie 2.0.49 of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een Azure-resourcegroep is een logische groep waarin Azure-resources worden geïmplementeerd en beheerd. Een resourcegroep maken met de opdracht [az group create][az-group-create]. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *westus*.

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Maak een virtueel netwerk met de [az network vnet maken] [ az-network-vnet-create] opdracht. Het volgende voorbeeld wordt een virtuele-netwerknaam *myVnet* met een adresvoorvoegsel van *10.0.0.0/8*, en een subnet met de naam *myAKSSubnet*. Het adresvoorvoegsel van dit subnet standaard ingesteld op *10.240.0.0/16*:

```azurecli-interactive
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefixes 10.0.0.0/8 \
    --subnet-name myAKSSubnet \
    --subnet-prefix 10.240.0.0/16
```

Maak nu een aanvullende subnet op voor de virtuele-knooppunten met behulp van de [az network vnet subnet maken] [ az-network-vnet-subnet-create] opdracht. Het volgende voorbeeld wordt een subnet met de naam *myVirtualNodeSubnet* met het adresvoorvoegsel van *10.241.0.0/16*.

```azurecli-interactive
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name myVirtualNodeSubnet \
    --address-prefix 10.241.0.0/16
```

## <a name="create-a-service-principal"></a>Een service-principal maken

Een AKS-cluster heeft een service-principal van Azure Active Directory nodig om met andere Azure-resources te kunnen communiceren. Deze service-principal kan automatisch worden gemaakt door de Azure CLI of via de portal, of u kunt er een vooraf maken en hieraan extra machtigingen toewijzen.

Een service principal maken met behulp de [az ad sp create-for-rbac] [ az-ad-sp-create-for-rbac] opdracht. De parameter `--skip-assignment` zorgt ervoor dat eventuele extra machtigingen beperkt kunnen worden toegewezen.

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

De uitvoer lijkt op die in het volgende voorbeeld:

```
{
  "appId": "bef76eb3-d743-4a97-9534-03e9388811fc",
  "displayName": "azure-cli-2018-11-21-18-42-00",
  "name": "http://azure-cli-2018-11-21-18-42-00",
  "password": "1d257915-8714-4ce7-a7fb-0e5a5411df7f",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

Noteer de *appId* en *wachtwoord*. Deze waarden worden gebruikt in de volgende stappen.

## <a name="assign-permissions-to-the-virtual-network"></a>Machtigingen toewijzen aan het virtuele netwerk

Als u wilt dat uw cluster te gebruiken en beheren van het virtuele netwerk, moet u de AKS-service-principal de juiste rechten voor het gebruik van de netwerkbronnen verlenen.

Haal eerst het virtuele netwerk resource ID met [az network vnet show][az-network-vnet-show]:

```azurecli-interactive
az network vnet show --resource-group myResourceGroup --name myVnet --query id -o tsv
```

De juiste om toegang te verlenen voor het AKS-cluster te gebruiken van het virtuele netwerk, maken van een rol toewijzing met de [az roltoewijzing maken] [ az-role-assignment-create] opdracht. Vervang `<appId`> en `<vnetId>` door de waarden die zijn verzameld in de vorige twee stappen.

```azurecli-interactive
az role assignment create --assignee <appId> --scope <vnetId> --role Contributor
```

## <a name="create-an-aks-cluster"></a>Een AKS-cluster maken

U implementeert een AKS-cluster in de AKS-subnet in een vorige stap hebt gemaakt. Haal de ID van het gebruik van dit subnet [az network vnet subnet show][az-network-vnet-subnet-show]:

```azurecli-interactive
az network vnet subnet show --resource-group myResourceGroup --vnet-name myVnet --name myAKSSubnet --query id -o tsv
```

Gebruik de opdracht [az aks create][az-aks-create] om een AKS-cluster te maken. In het volgende voorbeeld wordt een cluster met de naam *myAKSCluster* gemaakt met één knooppunt. Vervang `<subnetId>` met de ID in de vorige stap hebt verkregen en vervolgens `<appId>` en `<password>` met de 

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --network-plugin azure \
    --service-cidr 10.0.0.0/16 \
    --dns-service-ip 10.0.0.10 \
    --docker-bridge-address 172.17.0.1/16 \
    --vnet-subnet-id <subnetId> \
    --service-principal <appId> \
    --client-secret <password>
```

Na enkele minuten is de opdracht voltooid en retourneert deze informatie over het cluster in JSON-indeling.

## <a name="enable-virtual-nodes"></a>Virtuele knooppunten inschakelen

De connector virtuele knooppunten gebruikt om aanvullende functionaliteit, een Azure CLI-extensie. Voordat u de connector virtuele knooppunten inschakelen kunt, installeer eerst de extensie met de [az-extensie toevoegen] [ az-extension-add] opdracht:

```azurecli-interactive
az extension add --source https://aksvnodeextension.blob.core.windows.net/aks-virtual-node/aks_virtual_node-0.2.0-py2.py3-none-any.whl
```

Als u wilt inschakelen virtuele knooppunten, nu gebruiken de [az aks inschakelen--invoegtoepassingen] [ az-aks-enable-addons] opdracht. Het volgende voorbeeld wordt het subnet met de naam *myVirtualNodeSubnet* in een vorige stap hebt gemaakt:

```azurecli-interactive
az aks enable-addons \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --addons virtual-node \
    --subnet-name myVirtualNodeSubnet
```

## <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster

Gebruik de opdracht [az aks get-credentials][az-aks-get-credentials] om `kubectl` te configureren dat er verbinding wordt gemaakt met het Kubernetes-cluster. In deze stap worden referenties gedownload en wordt Kubernetes CLI geconfigureerd voor het gebruik van deze referenties.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Als u de verbinding met uw cluster wilt controleren, gebruikt u de opdracht [kubectl get][kubectl-get] om een lijst met clusterknooppunten te retourneren.

```console
kubectl get nodes
```

De volgende voorbeelduitvoer ziet u de enkel VM-knooppunt dat is gemaakt en vervolgens de virtuele knooppunt voor Linux, *virtuele-knooppunt-aci-linux*:

```
$ kubectl get nodes

NAME                          STATUS    ROLES     AGE       VERSION
virtual-node-aci-linux        Ready     agent     28m       v1.11.2
aks-agentpool-14693408-0      Ready     agent     32m       v1.11.2
```

## <a name="deploy-a-sample-app"></a>Een voorbeeld-app implementeren

Maak een bestand met de naam `virtual-node.yaml` en kopieer de volgende YAML. Voor het plannen van de container op het knooppunt, een [nodeSelector] [ node-selector] en [toleration] [ toleration] zijn gedefinieerd.

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

```console
kubectl apply -f virtual-node.yaml
```

Gebruik de [kubectl ophalen schillen] [ kubectl-get] opdracht met de `-o wide` argument voor een lijst van schillen en het knooppunt geplande uitvoer. U ziet dat de `aci-helloworld` schil is gepland op de `virtual-node-aci-linux` knooppunt.

```
$ kubectl get pods -o wide

NAME                            READY     STATUS    RESTARTS   AGE       IP           NODE
aci-helloworld-9b55975f-bnmfl   1/1       Running   0          4m        10.241.0.4   virtual-node-aci-linux
```

De schil is een interne IP-adres toegewezen vanuit het Azure-netwerksubnet overgedragen voor gebruik met virtuele-knooppunten.

> [!NOTE]
> Als u installatiekopieën die zijn opgeslagen in Azure Container Registry gebruikt [configureren en gebruiken van een Kubernetes-geheim][acr-aks-secrets]. Een beperking van de virtuele knooppunten preview is dat u kunt geen geïntegreerde verificatie van Azure AD-service-principal. Als u een geheim niet gebruikt, schillen gepland op virtuele-knooppunten niet starten en de fout melden `HTTP response status code 400 error code "InaccessibleImage"`.

## <a name="test-the-virtual-node-pod"></a>Test de schil virtueel knooppunt

Als u wilt testen de pod uitgevoerd op de virtuele-knooppunt, blader naar de demo-toepassing met een webclient. Tijdens de schil een interne IP-adres toegewezen is, kunt u deze verbinding hebben met een andere schil op het AKS-cluster snel testen. Een test-schil maken en koppelen van een terminalsessie toe:

```console
kubectl run -it --rm virtual-node-test --image=debian
```

Installeer `curl` in de schil met `apt-get`:

```console
apt-get update && apt-get install -y curl
```

Nu toegang tot het adres van het gebruik van uw schil `curl`, zoals *http://10.241.0.4*. Geef uw eigen interne IP-adres wordt weergegeven in de vorige `kubectl get pods` opdracht:

```console
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

## <a name="remove-virtual-nodes"></a>Virtuele knooppunten verwijderen

Als u niet meer virtuele-knooppunten gebruiken wilt, kunt u deze uitschakelen met behulp van de [az aks disable--invoegtoepassingen] [ az aks disable-addons] opdracht. Het volgende voorbeeld wordt de virtuele Linux-knooppunten:

```azurecli-interactive
az aks disable-addons --resource-group myResourceGroup --name myAKSCluster --addons virtual-node
```

Nu de virtuele-netwerkbronnen en de resourcegroep verwijderen:

```azurecli-interactive
# Change the name of your resource group and network resources as needed
RES_GROUP=myResourceGroup

# Get network profile ID
NETWORK_PROFILE_ID=$(az network profile list --resource-group $RES_GROUP --query [0].id --output tsv)

# Delete the network profile
az network profile delete --id $NETWORK_PROFILE_ID -y

# Get the service association link (SAL) ID
SAL_ID=$(az network vnet subnet show --resource-group $RES_GROUP --vnet-name myVnet --name myVirtualNodeSubnet --query id --output tsv)/providers/Microsoft.ContainerInstance/serviceAssociationLinks/default

# Delete the default SAL ID for the subnet
az resource delete --ids $SAL_ID --api-version 2018-07-01

# Delete the subnet delegation to Azure Container Instances
az network vnet subnet update --resource-group $RES_GROUP --vnet-name myVnet --name myVirtualNodeSubnet --remove delegations 0
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel is een schil gepland op de virtuele-knooppunt en een persoonlijke, interne IP-adres toegewezen. U kunt in plaats daarvan een service-implementatie en het routeren van netwerkverkeer maken op uw schil via een load balancer of een controller voor binnenkomend verkeer. Zie voor meer informatie, [een eenvoudige ingangscontroller in AKS maakt][aks-basic-ingress].

Virtuele knooppunten zijn vaak een onderdeel van een oplossing voor schalen in AKS. Zie voor meer informatie over oplossingen voor vergroten/verkleinen, de volgende artikelen:

- [Gebruik het Kubernetes-schil horizontale automatisch schalen][aks-hpa]
- [Gebruik het Kubernetes-cluster automatisch schalen][aks-cluster-autoscaler]

<!-- LINKS - external -->
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/

<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[az-group-create]: /cli/azure/group#az-group-create
[az-network-vnet-create]: /cli/azure/network/vnet#az-network-vnet-create
[az-network-vnet-subnet-create]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-create
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-network-vnet-show]: /cli/azure/network/vnet#az-network-vnet-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-show
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-enable-addons]: /cli/azure/aks#az-aks-enable-addons
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az aks disable-addons]: /cli/azure/aks#az-aks-disable-addons
[aks-hpa]: tutorial-kubernetes-scale.md
[aks-cluster-autoscaler]: autoscaler.md
[aks-basic-ingress]: ingress-basic.md
[az-provider-list]: /cli/azure/provider#az-provider-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[acr-aks-secrets]: ../container-registry/container-registry-auth-aks.md#access-with-kubernetes-secret
