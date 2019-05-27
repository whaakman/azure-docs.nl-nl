---
title: API-server gemachtigd IP-adresbereiken in Azure Kubernetes Service (AKS)
description: Informatie over hoe op beveiligde uw cluster met behulp van een IP-adresbereiken voor toegang tot de API-server in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: iainfou
ms.openlocfilehash: 1b983c534ab92218759175655bbf396788e4c39d
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956490"
---
# <a name="preview---secure-access-to-the-api-server-using-authorized-ip-address-ranges-in-azure-kubernetes-service-aks"></a>Preview - beveiligde toegang tot de API-server met gemachtigd IP-adresbereiken in Azure Kubernetes Service (AKS)

In Kubernetes ontvangt de API-server aanvragen voor het uitvoeren van acties in het cluster, zoals resources maken of schalen van het aantal knooppunten. De API-server is de centrale manier om te communiceren met en een cluster beheren. Voor het clusterbeveiliging te verbeteren en aanvallen te minimaliseren, moet de API-server alleen toegankelijk vanuit een beperkt aantal IP-adresbereiken.

Dit artikel ziet u hoe u aanvragen beperken tot besturingselement vlak met API server gemachtigd IP-adresbereiken. Deze functie is momenteel beschikbaar als preview-product.

> [!IMPORTANT]
> AKS-preview-functies zijn selfservice en aanmelden. Previews worden opgegeven voor het verzamelen van fouten en feedback van onze community. Ze worden echter niet ondersteund door Azure technische ondersteuning. Als u een cluster maken of deze functies aan bestaande clusters toevoegen, is dat cluster wordt niet ondersteund totdat de functie niet langer in preview is en is geslaagd voor algemene beschikbaarheid (GA).
>
> Als u problemen met de preview-functies ondervindt, [opent u een probleem op de AKS-GitHub-opslagplaats] [ aks-github] met de naam van de preview-functie in de titel van fout.

## <a name="before-you-begin"></a>Voordat u begint

API-server toegestane IP-adresbereiken werkt alleen voor nieuwe AKS-clusters die u maakt. Dit artikel laat u het maken van een AKS-cluster met behulp van de Azure CLI.

U moet de Azure CLI versie 2.0.61 of later geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u de Azure CLI wilt installeren of upgraden, raadpleegt u  [Azure CLI installeren][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Aks-preview CLI-extensie installeren

De CLI-opdrachten voor het configureren van IP-bereiken van API-server die gemachtigd zijn beschikbaar in de *aks-preview* CLI-extensie. Installeer de *aks-preview* Azure CLI-extensie met de [az-extensie toevoegen] [ az-extension-add] opdracht, zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az extension add --name aks-preview
```

> [!NOTE]
> Als u eerder hebt geïnstalleerd het *aks-preview* extensie, installatie van de beschikbare updates met behulp van de `az extension update --name aks-preview` opdracht.

### <a name="register-feature-flag-for-your-subscription"></a>Functievlag voor uw abonnement registreren

Voor het gebruik van API-server gemachtigd IP-bereiken, schakelt u eerst een functievlag voor uw abonnement. Om u te registreren de *APIServerSecurityPreview* vlag functie, gebruikt u de [az functie registreren] [ az-feature-register] opdracht zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az feature register --name APIServerSecurityPreview --namespace Microsoft.ContainerService
```

Het duurt enkele minuten duren voordat de status om weer te geven *geregistreerde*. U kunt controleren op de registratie van status met behulp van de [az Functielijst] [ az-feature-list] opdracht:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/APIServerSecurityPreview')].{Name:name,State:properties.state}"
```

Wanneer u klaar bent, vernieuwt u de registratie van de *Microsoft.ContainerService* resourceprovider met behulp van de [az provider register] [ az-provider-register] opdracht:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations"></a>Beperkingen

De volgende beperkingen zijn van toepassing wanneer u API server gemachtigd IP-adresbereiken configureren:

* Je niet momenteel Azure Dev opslagruimten gebruiken als de communicatie met de API-server is ook geblokkeerd.

## <a name="overview-of-api-server-authorized-ip-ranges"></a>Overzicht van API-server gemachtigd IP-bereiken

De Kubernetes API-server is hoe de onderliggende Kubernetes APIs worden weergegeven. Dit onderdeel biedt de interactie van beheerhulpprogramma's, zoals `kubectl` of het Kubernetes-dashboard. AKS biedt een cluster met één tenant-model, met een eigen API-server. Standaard, openbaar IP-adres is toegewezen door de API-server en u moet de toegang met behulp van op rollen gebaseerd toegangsbeheer (RBAC).

Beveiligde toegang tot het anders openbaar toegankelijke AKS besturingselement vlak / -API-server die u kunt inschakelen en gebruiken gemachtigd IP-bereiken. Deze gemachtigde IP-adresbereiken staan alleen de gedefinieerde IP-adresbereiken om te communiceren met de API-server. Een aanvraag aan de API-server vanaf een IP-adres die geen deel uitmaakt van deze toegestane IP-adresbereiken is geblokkeerd. U moet nog steeds gebruikmaken van RBAC vervolgens autoriseren van gebruikers en de acties die zij aanvragen.

De toegestane IP-bereik-functionaliteit wilt gebruiken, wordt een openbaar IP-adres op de knooppuntgroep blootgesteld door het implementeren van een eenvoudige NGINX-service. De API-server communiceert met de knooppuntgroep via deze geautoriseerde openbare IP-adres. Vervolgens definieert u extra IP-adresbereiken dat toegang heeft tot de API-server.

Zie voor meer informatie over de API-server en andere clusteronderdelen [Kubernetes-concepten voor AKS core][concepts-clusters-workloads].

## <a name="create-an-aks-cluster"></a>Een AKS-cluster maken

API-server gemachtigd IP-adresbereiken werken alleen voor nieuwe AKS-clusters. U kunt toegestane IP-adresbereiken niet inschakelen als onderdeel van het cluster opnieuw maken. Als u probeert om in te schakelen als onderdeel van het cluster toegestane IP-adresbereiken proces maken, de clusterknooppunten zijn geen toegang krijgt tot de API-server tijdens de implementatie als de uitgaande IP-adres op dat moment is niet gedefinieerd.

Maak eerst een cluster met de [az aks maken] [ az-aks-create] opdracht. Het volgende voorbeeld wordt een cluster met één knooppunt met de naam *myAKSCluster* in de resourcegroep met de naam *myResourceGroup*.

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location eastus

# Create an AKS cluster
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --generate-ssh-keys

# Get credentials to access the cluster
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="create-outbound-gateway-for-firewall-rules"></a>Uitgaande-gateway voor firewall-regels maken

Om ervoor te zorgen dat de knooppunten in een cluster op betrouwbare wijze met de API-server communiceren kunnen wanneer u toegestane IP-adresbereiken in de volgende sectie inschakelt, maakt u een firewall van Azure voor gebruik als de uitgaande gateway. Het IP-adres van de firewall van Azure wordt vervolgens toegevoegd aan de lijst met toegestane API-server IP-adressen in de volgende sectie.

> [!WARNING]
> Het gebruik van Azure-Firewall kan dit tot aanzienlijke kosten voor een maandelijkse factureringscyclus. De vereiste voor het gebruik van Azure-Firewall moet alleen nodig in deze eerste preview-periode. Zie voor meer informatie en kosten plannen, [prijzen voor Azure-Firewall][azure-firewall-costs].

Haal eerst de *MC_* Resourcegroepnaam voor het AKS-cluster en het virtuele netwerk. Vervolgens maakt u een subnet met de [az network vnet subnet maken] [ az-network-vnet-subnet-create] opdracht. Het volgende voorbeeld wordt een subnet met de naam *AzureFirewallSubnet* met de CIDR-bereik van *10.200.0.0/16*:

```azurecli-interactive
# Get the name of the MC_ cluster resource group
MC_RESOURCE_GROUP=$(az aks show \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --query nodeResourceGroup -o tsv)

# Get the name of the virtual network used by the cluster
VNET_NAME=$(az network vnet list \
    --resource-group $MC_RESOURCE_GROUP \
    --query [0].name -o tsv)

# Create a subnet in the virtual network for Azure Firewall
az network vnet subnet create \
    --resource-group $MC_RESOURCE_GROUP \
    --vnet-name $VNET_NAME \
    --name AzureFirewallSubnet \
    --address-prefixes 10.200.0.0/16
```

Voor het maken van een Azure-Firewall, installeert de *azure-firewall* CLI-extensie met de [az-extensie toevoegen] [ az-extension-add] opdracht. Vervolgens maakt u een firewall met de [az netwerkfirewall maken] [ az-network-firewall-create] opdracht. Het volgende voorbeeld wordt een firewall van Azure met de naam *myAzureFirewall*:

```azurecli-interactive
# Install the CLI extension for Azure Firewall
az extension add --name azure-firewall

# Create an Azure firewall
az network firewall create \
    --resource-group $MC_RESOURCE_GROUP\
    --name myAzureFirewall
```

Een firewall van Azure is een openbaar IP-adres die uitgaand verkeer via toegewezen. Maak een openbaar adres met de [az network public-ip maken] [ az-network-public-ip-create] opdracht en vervolgens een IP-configuratie maken over het gebruik van de firewall de [az netwerk firewall voor ip-config maken] [ az-network-firewall-ip-config-create] die van toepassing is het openbare IP-adres:

```azurecli-interactive
# Create a public IP address for the firewall
FIREWALL_PUBLIC_IP=$(az network public-ip create \
    --resource-group $MC_RESOURCE_GROUP \
    --name myAzureFirewallPublicIP \
    --sku Standard \
    --query publicIp.ipAddress -o tsv)

# Associated the firewall with virtual network
az network firewall ip-config create \
    --resource-group $MC_RESOURCE_GROUP \
    --name myAzureFirewallIPConfig \
    --vnet-name $VNET_NAME \
    --firewall-name myAzureFirewall \
    --public-ip-address myAzureFirewallPublicIP
```

Maak nu de Azure-netwerk firewallregel *toestaan* alle *TCP* verkeer met behulp van de [az netwerk netwerk-firewallregel maken] [ az-network-firewall-network-rule-create] de opdracht. Het volgende voorbeeld wordt een regel met de naam *AllowTCPOutbound* voor verkeer met een bron- of -adres:

```azurecli-interactive
az network firewall network-rule create \
    --resource-group $MC_RESOURCE_GROUP \
    --firewall-name myAzureFirewall \
    --name AllowTCPOutbound \
    --collection-name myAzureFirewallCollection \
    --priority 200 \
    --action Allow \
    --protocols TCP \
    --source-addresses '*' \
    --destination-addresses '*' \
    --destination-ports '*'
```

Als u wilt de firewall van Azure aan de netwerkroute koppelt, verkrijgen, de bestaande informatie over een route-tabel, het interne IP-adres van de Azure-firewall en het IP-adres van de API-server. Deze IP-adressen zijn opgegeven in de volgende sectie om te bepalen hoe verkeer voor communicatie binnen het cluster moet worden gerouteerd.

```azurecli-interactive
# Get the AKS cluster route table
ROUTE_TABLE=$(az network route-table list \
    --resource-group $MC_RESOURCE_GROUP \
    --query "[?contains(name,'agentpool')].name" -o tsv)

# Get internal IP address of the firewall
FIREWALL_INTERNAL_IP=$(az network firewall show \
    --resource-group $MC_RESOURCE_GROUP \
    --name myAzureFirewall \
    --query ipConfigurations[0].privateIpAddress -o tsv)

# Get the IP address of API server endpoint
K8S_ENDPOINT_IP=$(kubectl get endpoints -o=jsonpath='{.items[?(@.metadata.name == "kubernetes")].subsets[].addresses[].ip}')
```

Maak ten slotte een route in de bestaande AKS netwerk route tabel met de [az network route-table route maken] [ az-network-route-table-route-create] opdracht waarmee verkeer op het apparaat van de firewall van Azure gebruiken voor API-server communicatie.

```azurecli-interactive
az network route-table route create \
    --resource-group $MC_RESOURCE_GROUP \
    --route-table-name $ROUTE_TABLE \
    --name AzureFirewallAPIServer \
    --address-prefix $K8S_ENDPOINT_IP/32 \
    --next-hop-ip-address $FIREWALL_INTERNAL_IP \
    --next-hop-type VirtualAppliance

echo "Public IP address for the Azure Firewall instance that should be added to the list of API server authorized addresses is:" $FIREWALL_PUBLIC_IP
```

Noteer het openbare IP-adres van uw Azure-Firewall-apparaat. Dit adres wordt toegevoegd aan de lijst met API-server gemachtigd IP-adresbereiken in de volgende sectie.

## <a name="enable-authorized-ip-ranges"></a>Toegestane IP-adresbereiken inschakelen

Om in te schakelen API server gemachtigd IP-bereiken, moet u een lijst met toegestane IP-adresbereiken opgeven. Wanneer u een CIDR-bereik opgeeft, kunt u beginnen met het eerste IP-adres binnen het bereik. Bijvoorbeeld, *137.117.106.90/29* is een geldig bereik, maar zorg ervoor dat u het eerste IP-adres binnen het bereik opgeven zoals *137.117.106.88/29*.

Gebruik [az aks bijwerken] [ az-aks-update] opdracht en geeft u de *--api-server-geautoriseerd-ip-adresbereiken* om toe te staan. Deze IP-adresbereiken zijn meestal-adresbereiken die worden gebruikt door uw on-premises netwerken. Toevoegen van het openbare IP-adres van de firewall van uw eigen Azure, zoals in de vorige stap hebt verkregen *20.42.25.196/32*.

Het volgende voorbeeld wordt een API-server gemachtigd IP-adresbereiken voor het cluster met de naam *myAKSCluster* in de resourcegroep met de naam *myResourceGroup*. De IP-adresbereiken te machtigen zijn *20.42.25.196/32* (de firewall van Azure openbaar IP-adres), klikt u vervolgens *172.0.0.10/16* en *168.10.0.10/18*:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges 20.42.25.196/32,172.0.0.10/16,168.10.0.10/18
```

## <a name="update-or-disable-authorized-ip-ranges"></a>Update- of uitschakelen van toegestane IP-adresbereiken

Als u wilt bijwerken of toegestane IP-adresbereiken uitschakelen, gebruikt u opnieuw [az aks bijwerken] [ az-aks-update] opdracht. Geef de bijgewerkte CIDR-bereik dat u wilt toestaan of geef een leeg bereik-API-server uitschakelen gemachtigd IP-adresbereiken, zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges ""
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u API server gemachtigd IP-adresbereiken ingeschakeld. Deze aanpak is een onderdeel van hoe u een AKS-cluster veilig kunt uitvoeren.

Zie voor meer informatie, [veiligheidsconcepten voor toepassingen en -clusters in AKS] [ concepts-security] en [aanbevolen procedures voor beveiliging van clusters en upgrades in AKS] [ operator-best-practices-cluster-security].

<!-- LINKS - external -->
[aks-github]: https://github.com/azure/aks/issues]
[azure-firewall-costs]: https://azure.microsoft.com/pricing/details/azure-firewall/

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-update]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-update
[concepts-clusters-workloads]: concepts-clusters-workloads.md
[concepts-security]: concepts-security.md
[operator-best-practices-cluster-security]: operator-best-practices-cluster-security.md
[create-aks-sp]: kubernetes-service-principal.md#manually-create-a-service-principal
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-network-vnet-subnet-create]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-create
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-network-firewall-create]: /cli/azure/ext/azure-firewall/network/firewall#ext-azure-firewall-az-network-firewall-create
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-firewall-ip-config-create]: /cli/azure/ext/azure-firewall/network/firewall/ip-config#ext-azure-firewall-az-network-firewall-ip-config-create
[az-network-firewall-network-rule-create]: /cli/azure/ext/azure-firewall/network/firewall/network-rule#ext-azure-firewall-az-network-firewall-network-rule-create
[az-network-route-table-route-create]: /cli/azure/network/route-table/route#az-network-route-table-route-create
