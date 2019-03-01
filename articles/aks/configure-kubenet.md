---
title: Configureren van kubenet netwerken in Azure Kubernetes Service (AKS)
description: Informatie over het configureren van kubenet (standaard) netwerk in Azure Kubernetes Service (AKS) voor het implementeren van een AKS-cluster in een bestaand virtueel netwerk en subnet.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 01/31/2019
ms.author: iainfou
ms.reviewer: nieberts, jomore
ms.openlocfilehash: b80177d17e0dc5a4e54396907ecee61890ec523f
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57011344"
---
# <a name="use-kubenet-networking-with-your-own-ip-address-ranges-in-azure-kubernetes-service-aks"></a>Gebruik van kubenet netwerken met uw eigen IP-adresbereiken in Azure Kubernetes Service (AKS)

Standaard-AKS clusters gebruik [kubenet][kubenet], en een Azure-netwerk en subnet worden voor u gemaakt. Met *kubenet*, knooppunten een IP-adres ophalen uit het subnet van de Azure-netwerk. Schillen ontvangen een IP-adres van een logisch verschillende adresruimte op het subnet van de Azure-netwerk van de knooppunten. Netwerkadresomzetting (NAT) wordt vervolgens geconfigureerd zodat de schillen bronnen op het Azure-netwerk kunnen bereiken. De bron-IP-adres van het verkeer is dat NAT wilt aan de primaire IP-adres van het knooppunt oplossen. Deze aanpak vermindert aanzienlijk het aantal IP-adressen die u reserveren in de adresruimte van uw netwerk voor pods wilt te gebruiken.

Met [Azure Container netwerken Interface (CNI)][cni-networking], elke pod krijgt u een IP-adres van het subnet en kunnen rechtstreeks worden geopend. Deze IP-adressen moeten uniek zijn in de adresruimte van uw netwerk, en moeten vooraf worden gepland. Elk knooppunt heeft een configuratieparameter voor het maximum aantal schillen worden ondersteund. Het equivalente aantal IP-adressen per knooppunt worden vervolgens een gereserveerd voor dat knooppunt. Deze aanpak vereist meer planning en leidt vaak tot uitputting van IP-adres of de noodzaak voor het opnieuw opbouwen van clusters in een groter subnet aan de vereisten van uw toepassing.

In dit artikel leest u hoe u *kubenet* netwerken maken en een subnet van een virtueel netwerk gebruiken voor een AKS-cluster. Zie voor meer informatie over opties voor netwerken en overwegingen met betrekking tot [netwerk basisbegrippen voor Kubernetes en AKS][aks-network-concepts].

## <a name="before-you-begin"></a>Voordat u begint

U moet de Azure CLI versie 2.0.56 of later geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u de Azure CLI wilt installeren of upgraden, raadpleegt u  [Azure CLI installeren][install-azure-cli].

## <a name="overview-of-kubenet-networking-with-your-own-subnet"></a>Overzicht van netwerken met uw eigen subnet kubenet

In veel omgevingen, kunt u virtuele netwerken en subnetten met toegewezen IP-adresbereiken hebt gedefinieerd. Deze virtuele-netwerkbronnen worden gebruikt voor de ondersteuning van meerdere services en toepassingen. Voor verbinding met het netwerk, de AKS-clusters kunt *kubenet* (netwerkgebruik) of Azure CNI (*geavanceerde netwerken*).

Met *kubenet*, alleen de knooppunten een IP-adres in het subnet van het virtuele netwerk ontvangen. Schillen communiceren niet rechtstreeks met elkaar. In plaats daarvan wordt doorsturen van de gebruiker gedefinieerde routering (UDR) en IP-gebruikt voor connectiviteit tussen schillen over knooppunten. U kunt ook implementeren schillen achter een service die een toegewezen IP-adres ontvangt en verdeelt de belasting van verkeer voor de toepassing. Het volgende diagram toont hoe de AKS-knooppunten een IP-adres ontvangen in het subnet van het virtuele netwerk, maar niet het gehele product:

![Netwerk-model Kubenet met een AKS-cluster](media/use-kubenet/kubenet-overview.png)

Azure ondersteunt maximaal 400 routes in een UDR, zodat u kunt geen een AKS-cluster die groter zijn dan 400 knooppunten. AKS functies zoals [virtuele knooppunten] [ virtual-nodes] of beleid voor netwerken worden niet ondersteund met *kubenet*.

Met *Azure CNI*, elke pod ontvangt u een IP-adres in het IP-subnet, en kan rechtstreeks communiceren met andere schillen en -services. Uw clusters is even groot zijn als het IP-adresbereik dat u opgeeft. Echter, het IP-adresbereik moet vooraf worden gepland en alle van de IP-adressen worden gebruikt door de AKS-knooppunten op basis van het maximum aantal schillen die kunnen worden ondersteund. Geavanceerde netwerkfuncties en scenario's zoals [virtuele knooppunten] [ virtual-nodes] of beleid voor netwerken worden ondersteund met *Azure CNI*.

### <a name="ip-address-availability-and-exhaustion"></a>IP-adres beschikbaarheid en bronuitputting

Met *Azure CNI*, een veel voorkomend probleem is met het toegewezen IP-adresbereik is te klein om vervolgens extra knooppunten toevoegen wanneer u een cluster upgraden of schalen. De netwerkteam kan ook niet mogelijk om uit te geven van een groot genoeg zijn IP-adresbereik ter ondersteuning van uw verwachte behoeften.

Als een inbreuk, kunt u een AKS-cluster dat gebruik maakt *kubenet* en maak verbinding met het subnet van een bestaande virtuele netwerk. Deze aanpak kunt de knooppunten die zijn ontvangen van de gedefinieerde IP-adressen, zonder de noodzaak om te reserveren van een groot aantal IP-adressen van voorgrond voor alle van de mogelijke schillen die kunnen worden uitgevoerd in het cluster.

Met *kubenet*, u kunt een veel kleinere IP-adresbereik gebruiken en kunnen ter ondersteuning van grote clusters en toepassingsbehoeften. Bijvoorbeeld, zelfs met een */27* IP-adresbereik, kan u een cluster met 20-25 knooppunten met voldoende ruimte heeft voor schalen of een upgrade uitvoeren. Deze clustergrootte wordt ondersteuning geboden voor maximaal *2,200 2.750* schillen (met een standaard maximaal 110 schillen per knooppunt).

De volgende eenvoudige berekeningen Vergelijk het verschil in netwerk modellen:

- **kubenet** -een eenvoudige */24* IP-adresbereik kan maximaal ondersteunen *251* knooppunten in het cluster (elk subnet van het virtuele netwerk van Azure reserveert de eerste drie IP-adressen voor management-bewerkingen)
  - Dit aantal knooppunten tot kan ondersteunen *27,610* schillen (met standaard maximaal 110 schillen per knooppunt met *kubenet*)
- **Azure CNI** -die dezelfde basic */24* subnetbereik kan alleen ondersteuning voor maximaal *8* knooppunten in het cluster
  - Dit aantal knooppunten kan alleen ondersteuning voor maximaal *240* schillen (met een standaardmaximum van 30 schillen per knooppunt met *Azure CNI*)

> [!NOTE]
> Deze maximumwaarden geen upgrade van het rekening of schaalbewerkingen. In de praktijk moet uitvoeren u het maximum aantal knooppunten die ondersteuning biedt voor het subnet IP-adresbereik niet. U moet enkele IP-adressen beschikbaar voor gebruik tijdens de schaal van de upgrade operations laten.

### <a name="virtual-network-peering-and-expressroute-connections"></a>Peering op virtueel netwerk- en ExpressRoute-verbindingen

Voor on-premises-connectiviteit, beide *kubenet* en *Azure CNI* netwerk benaderingen kunt [peering op virtueel netwerk] [ vnet-peering]of [ExpressRoute-verbindingen][express-route]. Plan de IP-adresbereiken zorgvuldig om te voorkomen dat overlapping en onjuiste verkeersroutering. Bijvoorbeeld: veel on-premises netwerken gebruiken een *10.0.0.0/8* adresbereik die wordt geadverteerd via de ExpressRoute-verbinding. Het is raadzaam om te maken van uw AKS-clusters in Azure-netwerksubnetten buiten dit adresbereik zoals *172.26.0.0/16*.

### <a name="choose-a-network-model-to-use"></a>Kies een netwerk-model te gebruiken

De keuze van welke netwerk-invoegtoepassing te gebruiken voor uw AKS-cluster meestal een balans tussen flexibiliteit en geavanceerde configuratie nodig is. De volgende overwegingen helpen overzicht wanneer elk model netwerk mogelijk het meest geschikt is.

Gebruik *kubenet* wanneer:

- U kunt IP-adresruimte hebt beperkt.
- De meeste van de schil-communicatie is binnen het cluster.
- U hoeft geen geavanceerde functies zoals virtuele knooppunten of netwerkbeleid.

Gebruik *Azure CNI* wanneer:

- U hebt een beschikbaar IP-adresruimte.
- De meeste van de schil-communicatie is met bronnen buiten het cluster.
- U wilt niet dat de udr's beheren.
- Moet u geavanceerde functies zoals virtuele knooppunten of netwerkbeleid.

> [!NOTE]
> Kuberouter maakt het mogelijk om in te schakelen netwerkbeleid bij het gebruik van kubenet en kan worden geïnstalleerd als een daemonset in een AKS-cluster. Houd er rekening mee kube-router is nog in de bètafase en biedt geen ondersteuning wordt aangeboden door Microsoft voor het project.

## <a name="create-a-virtual-network-and-subnet"></a>Een virtueel netwerk en een subnet maken

Aan de slag met het gebruik van *kubenet* en het subnet van uw eigen virtuele netwerk maakt eerst een resource-groep met de [az-groep maken] [ az-group-create] opdracht. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Als u geen een bestaand virtueel netwerk en subnet te gebruiken, maakt u deze netwerkbronnen met behulp van de [az network vnet maken] [ az-network-vnet-create] opdracht. In het volgende voorbeeld wordt het virtuele netwerk met de naam *myVnet* met het adresvoorvoegsel van *10.0.0.0/8*. Een subnet wordt gemaakt met de naam *myAKSSubnet* met het adresvoorvoegsel *10.240.0.0/16*.

```azurecli-interactive
az network vnet create \
    --resource-group myResourceGroup \
    --name myAKSVnet \
    --address-prefixes 10.0.0.0/8 \
    --subnet-name myAKSSubnet \
    --subnet-prefix 10.240.0.0/16
```

## <a name="create-a-service-principal-and-assign-permissions"></a>Een service-principal maken en toewijzen van machtigingen

Een AKS-cluster heeft een service-principal van Azure Active Directory nodig om met andere Azure-resources te kunnen communiceren. De service-principal moet machtigingen hebben voor het beheren van het virtuele netwerk en subnet die gebruikmaken van de AKS-knooppunten. U kunt een service-principal maken met de [az ad sp create-for-rbac] [ az-ad-sp-create-for-rbac] opdracht:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

De volgende voorbeelduitvoer ziet u de toepassings-ID en het wachtwoord voor uw service-principal. Deze waarden worden gebruikt in de extra stappen uitvoeren om een rol toewijzen aan de service-principal en vervolgens de AKS-cluster maken:

```console
$ az ad sp create-for-rbac --skip-assignment

{
  "appId": "476b3636-5eda-4c0e-9751-849e70b5cfad",
  "displayName": "azure-cli-2019-01-09-22-29-24",
  "name": "http://azure-cli-2019-01-09-22-29-24",
  "password": "a1024cd7-af7b-469f-8fd7-b293ecbb174e",
  "tenant": "72f998bf-85f1-41cf-92ab-2e7cd014db46"
}
```

Als u wilt toewijzen op de juiste overdrachten in de resterende stappen, gebruikt u de [az network vnet show] [ az-network-vnet-show] en [az network vnet subnet show] [ az-network-vnet-subnet-show] opdrachten voor het ophalen van de vereiste resource-id's. Deze resource-id worden opgeslagen als variabelen en waarnaar wordt verwezen in de resterende stappen uit:

```azurecli-interactive
VNET_ID=$(az network vnet show --resource-group myResourceGroup --name myAKSVnet --query id -o tsv)
SUBNET_ID=$(az network vnet subnet show --resource-group myResourceGroup --vnet-name myAKSVnet --name myAKSSubnet --query id -o tsv)
```

Nu toewijzen aan de service-principal voor uw AKS-cluster *Inzender* machtigingen voor het virtuele netwerk met de [az roltoewijzing maken] [ az-role-assignment-create] opdracht. Geef uw eigen  *\<appId >* zoals wordt weergegeven in de uitvoer van de vorige opdracht om de serviceprincipal te maken:

```azurecli-interactive
az role assignment create --assignee <appId> --scope $VNET_ID --role Contributor
```

## <a name="create-an-aks-cluster-in-the-virtual-network"></a>Een AKS-cluster maken in het virtuele netwerk

U hebt nu een virtueel netwerk en subnet, gemaakt en die zijn gemaakt en toegewezen machtigingen voor een service-principal die netwerkbronnen gebruiken. Nu een AKS-cluster maken in uw virtuele netwerk en subnet met behulp van de [az aks maken] [ az-aks-create] opdracht. Definieer uw eigen service-principal  *\<appId >* en  *\<wachtwoord >*, zoals wordt weergegeven in de uitvoer van de vorige opdracht om de serviceprincipal te maken.

De volgende IP-adresbereiken zijn ook worden gedefinieerd als onderdeel van het cluster proces maken:

* De *--service cidr* wordt gebruikt voor het toewijzen van interne services in de AKS-cluster een IP-adres. Deze IP-adresbereik moet een adresruimte die zich niet in gebruik ergens anders in uw netwerkomgeving. Dit omvat alle on-premises netwerkbereiken als u verbinding maken of van plan bent, uw Azure-netwerken met behulp van Express Route of een Site-naar-Site VPN-verbindingen.

* De *--dns-service-IP-* -adres moet de *.10* -adres van uw service IP-adresbereik.

* De *--pod-cidr* moet een grote adresruimte die zich niet in gebruik ergens anders in uw netwerkomgeving. Dit omvat alle on-premises netwerkbereiken als u verbinding maken of van plan bent om uw virtuele netwerken van Azure Express Route of Site-naar-Site VPN-verbinding verbinding te maken.
    * Dit adresbereik moet groot genoeg is voor het aantal knooppunten die u verwacht te schalen tot. U kunt dit adresbereik niet wijzigen nadat het cluster is geïmplementeerd als u meer adressen voor extra knooppunten nodig hebt.
    * De schil IP-adresbereik wordt gebruikt om toe te wijzen een */24* -adresruimte aan elk knooppunt in het cluster. In het volgende voorbeeld wordt de *--pod-cidr* van *192.168.0.0/16* het eerste knooppunt toegewezen *192.168.0.0/24*, het tweede knooppunt *192.168.1.0/24*, en het derde knooppunt *192.168.2.0/24*.
    * Als het cluster schalen of upgrades worden uitgevoerd blijft het Azure-platform een pod IP-adresbereik toewijzen aan elke nieuwe knooppunt.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 3 \
    --network-plugin kubenet \
    --service-cidr 10.0.0.0/16 \
    --dns-service-ip 10.0.0.10 \
    --pod-cidr 192.168.0.0/16 \
    --docker-bridge-address 172.17.0.1/16 \
    --vnet-subnet-id $SUBNET_ID \
    --service-principal <appId> \
    --client-secret <password>
```

## <a name="associate-network-resources-with-the-node-subnet"></a>Netwerkbronnen koppelen aan het subnet van het knooppunt

Wanneer u een AKS-cluster maakt, wordt een groep en route-tabel voor de beveiliging voor netwerk worden gemaakt. Deze netwerkresources worden beheerd door de controlelaag AKS en bijgewerkt bij het maken en beschikbaar maken van services. Koppel de netwerk-beveiliging groep en route-tabel met het subnet van uw virtuele netwerk als volgt:

```azurecli-interactive
# Get the MC_ resource group for the AKS cluster resources
MC_RESOURCE_GROUP=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv)

# Get the route table for the cluster
ROUTE_TABLE=$(az network route-table list -g ${MC_RESOURCE_GROUP} --query "[].id | [0]" -o tsv)

# Get the network security group
NODE_NSG=$(az network nsg list -g ${MC_RESOURCE_GROUP} --query "[].id | [0]" -o tsv)

# Update the subnet to associate the route table and network security group
az network vnet subnet update \
    --route-table $ROUTE_TABLE \
    --network-security-group $NODE_NSG \
    --ids $SUBNET_ID
```

## <a name="next-steps"></a>Volgende stappen

Met een AKS-cluster geïmplementeerd in het subnet van uw bestaande virtuele netwerk, kunt u nu het cluster als normale gebruiken. Aan de slag met [het bouwen van apps met behulp van Azure Dev spaties] [ dev-spaces] of [met Draft][use-draft], of [apps implementeren met behulp van Helm] [use-helm].

<!-- LINKS - External -->
[dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[aks-network-concepts]: concepts-network.md
[az-group-create]: /cli/azure/group#az-group-create
[az-network-vnet-create]: /cli/azure/network/vnet#az-network-vnet-create
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-network-vnet-show]: /cli/azure/network/vnet#az-network-vnet-show
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[use-helm]: kubernetes-helm.md
[use-draft]: kubernetes-draft.md
[virtual-nodes]: virtual-nodes-cli.md
[vnet-peering]: ../virtual-network/virtual-network-peering-overview.md
[express-route]: ../expressroute/expressroute-introduction.md
