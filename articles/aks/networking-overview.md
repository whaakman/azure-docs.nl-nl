---
title: Configuratie van het netwerk in Azure Kubernetes Service (AKS)
description: Meer informatie over de basisfuncties en geavanceerde netwerkconfiguratie in Azure Kubernetes Service (AKS).
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 08/08/2018
ms.author: marsma
ms.openlocfilehash: 1d7855ff840fc1dd68effb19c43c3a691bd15d62
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39714669"
---
# <a name="network-configuration-in-azure-kubernetes-service-aks"></a>Configuratie van het netwerk in Azure Kubernetes Service (AKS)

Wanneer u een Azure Kubernetes Service (AKS)-cluster maakt, kunt u selecteren uit twee netwerkopties: **Basic** of **Geavanceerd**.

## <a name="basic-networking"></a>Netwerkgebruik

De **Basic** optie netwerken is de standaardconfiguratie voor het AKS-cluster maken. De netwerkconfiguratie van het cluster en de schillen volledig wordt beheerd door Azure, en is geschikt voor implementaties die niet te worden aangepast VNet-configuratie hoeven. U hebt controle over de configuratie van het netwerk, zoals subnetten of het IP-adresbereiken die zijn toegewezen aan het cluster wanneer u netwerkgebruik selecteert.

Knooppunten in een AKS-cluster is geconfigureerd voor gebruik van eenvoudige netwerken de [kubenet] [ kubenet] Kubernetes-invoegtoepassing.

## <a name="advanced-networking"></a>Geavanceerde netwerken

**Geavanceerde** netwerken plaatst uw schillen in een Azure-netwerk (VNet) dat u configureert, zodat ze automatisch verbinding met de VNet-resources en integratie met de uitgebreide set mogelijkheden voor deze aanbieding VNets. Geavanceerde netwerken is beschikbaar bij het implementeren van AKS-clusters met de [Azure-portal][portal], Azure CLI, of met een Resource Manager-sjabloon.

Knooppunten in een AKS-cluster is geconfigureerd voor gebruik van geavanceerde netwerken de [Azure Container netwerken Interface (CNI)] [ cni-networking] Kubernetes-invoegtoepassing.

![Diagram van twee knooppunten met bruggen, elk met een enkel Azure-VNet verbinding maken][advanced-networking-diagram-01]

## <a name="advanced-networking-features"></a>Geavanceerde functies voor netwerkbeheer

Geavanceerde netwerken biedt de volgende voordelen:

* Uw AKS-cluster implementeren in een bestaand VNet of maak een nieuw VNet en subnet voor uw cluster.
* Elke pod in het cluster een IP-adres in het VNet wordt toegewezen, en kan rechtstreeks communiceren met andere schillen in het cluster en andere knooppunten in het VNet.
* Een schil kunt verbinding maken met andere services in een gekoppeld VNet, en on-premises netwerken via ExpressRoute en site-naar-site (S2S) VPN-verbindingen. Schillen zijn ook bereikbaar is vanaf on-premises.
* Een Kubernetes-service extern of intern beschikbaar via de Azure Load Balancer. Ook een functie van basisnetwerken.
* Schillen in een subnet waarvoor service-eindpunten ingeschakeld kunnen veilig verbinding maken met Azure-services, bijvoorbeeld Azure Storage en SQL-database.
* Gebruik de gebruiker gedefinieerde routes (UDR) om verkeer te routeren van schillen naar een virtueel netwerkapparaat.
* Schillen hebben toegang tot bronnen op het openbare Internet. Ook een functie van basisnetwerken.

## <a name="advanced-networking-prerequisites"></a>Geavanceerde netwerken vereisten

* Het VNet voor het AKS-cluster moet uitgaande internetverbinding toestaan.
* Maak niet meer dan één AKS-cluster in hetzelfde subnet.
* AKS-clusters worden niet gebruikt `169.254.0.0/16`, `172.30.0.0/16`, of `172.31.0.0/16` voor het Kubernetes-service-adresbereik.
* De service-principal die worden gebruikt door het AKS-cluster moet minimaal beschikken over [Inzender voor netwerken](../role-based-access-control/built-in-roles.md#network-contributor) machtigingen op het subnet binnen uw VNet. Als u wilt definiëren een [aangepaste rol](../role-based-access-control/custom-roles.md) in plaats van de ingebouwde rol van inzender voor netwerken, de volgende machtigingen zijn vereist:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`

## <a name="plan-ip-addressing-for-your-cluster"></a>IP-adressen voor uw cluster plannen

Clusters die zijn geconfigureerd met geavanceerde netwerken vereist extra planning. De grootte van uw VNet en een subnet moet geschikt zijn voor zowel het aantal schillen die u van plan bent om uit te voeren als het aantal knooppunten voor het cluster.

IP-adressen voor het gehele product en de knooppunten van het cluster worden toegewezen uit het opgegeven subnet binnen het VNet. Elk knooppunt is geconfigureerd met een primaire IP-adres, is het IP-adres van het knooppunt en 30 extra IP-adressen vooraf is geconfigureerd door Azure CNI die zijn toegewezen aan schillen gepland naar het knooppunt. Wanneer u uw cluster uitschalen, wordt elk knooppunt op dezelfde manier geconfigureerd met IP-adressen uit het subnet.

Het abonnement van de IP-adres voor een AKS-cluster bestaat uit een VNet, ten minste één subnet voor de knooppunten en schillen, en een Kubernetes-service-adresbereik.

| Adresbereik / Azure resource | Limieten en grootte |
| --------- | ------------- |
| Virtueel netwerk | Azure VNet kan even groot zijn als de/8 die maar mogelijk alleen 16.000 geconfigureerd IP-adressen. |
| Subnet | Moet groot genoeg is voor de knooppunten, schillen en alle Kubernetes en Azure-resources die kunnen worden ingericht in het cluster. Als u een interne Load Balancer in Azure implementeert, kunt u voor de front-end-IP-adressen, worden toegewezen uit het subnet van het cluster, geen openbare IP-adressen. <p/>Voor het berekenen van *minimale* subnetgrootte: `(number of nodes) + (number of nodes * pods per node)` <p/>Voorbeeld voor een cluster met 50 knooppunten: `(50) + (50 * 30) = 1,550` (/ 21 of hoger) |
| Adresbereik van Kubernetes service | Dit bereik moet niet worden gebruikt door een willekeurig netwerkelement op of verbonden met dit VNet. Adres van service CIDR moet kleiner zijn dan /12. |
| IP-adres van Kubernetes DNS-service | IP-adres binnen het Kubernetes-service-adresbereik dat wordt gebruikt door clusterservicedetectie (kube-dns). |
| Docker bridge-adres | IP-adres (in CIDR-notatie) gebruikt als de Docker bridge-IP-adres op knooppunten. De standaardwaarde van 172.17.0.1/16. |

Elk VNet dat is ingericht voor gebruik met de invoegtoepassing Azure CNI beperkt tot is **16.000 geconfigureerde IP-adressen**.

## <a name="maximum-pods-per-node"></a>Maximale schillen per knooppunt

Het standaard maximum aantal schillen per knooppunt in een AKS-cluster varieert tussen basiseigenschappen en geavanceerde netwerken en de methode van implementatie van het cluster.

### <a name="default-maximum"></a>Standaard maximale

* Netwerkgebruik: **110 schillen per knooppunt**
* Geavanceerde netwerken **30 schillen per knooppunt**

### <a name="configure-maximum"></a>Maximale configureren

Afhankelijk van de methode van implementatie, is het mogelijk om te wijzigen van het maximum aantal schillen per knooppunt in een AKS-cluster.

* **Azure CLI**: Geef de `--max-pods` argument bij het implementeren van een cluster met de [az aks maken] [ az-aks-create] opdracht.
* **Resource Manager-sjabloon**: Geef de `maxPods` eigenschap in de [ManagedClusterAgentPoolProfile] object wanneer u een cluster met een Resource Manager-sjabloon implementeert.
* **Azure-portal**: U kunt het maximale aantal schillen per knooppunt niet wijzigen wanneer u een cluster met de Azure portal implementeert. Geavanceerde VPN-clusters zijn beperkt tot 30 schillen per knooppunt bij de implementatie in Azure portal.

## <a name="deployment-parameters"></a>Implementatieparameters

Wanneer u een AKS-cluster maakt, kunnen de volgende parameters worden geconfigureerd voor geavanceerde netwerken:

**Virtueel netwerk**: het VNet waarin u wilt de Kubernetes-cluster implementeren. Als u maken van een nieuw VNet voor uw cluster wilt, selecteert u *nieuw* en volg de stappen in de *virtueel netwerk maken* sectie. Het VNet is beperkt tot 16.000 geconfigureerde IP-adressen.

**Subnet**: het subnet binnen het VNet waar u het cluster te implementeren. Als u een nieuw subnet in het VNet voor uw cluster te maken wilt, selecteert u *nieuw* en volg de stappen in de *subnet maken* sectie.

**Kubernetes-service-adresbereik**: dit is de set van virtuele IP-adressen die Kubernetes wordt toegewezen aan [services] [ services] in uw cluster. U kunt elk bereik met privé-adres dat voldoet aan de volgende vereisten:

* Moet niet binnen het VNet-IP-adresbereik van het cluster
* Mag niet overlappen met een andere vnet's die van het cluster VNet collega 's
* Mag niet overlappen met een on-premises-IP-adressen
* Moet niet binnen de bereiken `169.254.0.0/16`, `172.30.0.0/16`, of `172.31.0.0/16`

Hoewel het technisch mogelijk om op te geven van een service-adresbereik binnen hetzelfde VNet als uw cluster, wordt doen zo niet aanbevolen. Onvoorspelbaar gedrag kan veroorzaken als er overlappende IP-adresbereiken worden gebruikt. Zie voor meer informatie de [Veelgestelde vragen over](#frequently-asked-questions) sectie van dit artikel. Zie voor meer informatie over Kubernetes-services, [Services] [ services] in het Kubernetes-documentatie.

**IP-adres van Kubernetes DNS-service**: het IP-adres van het cluster DNS-service. Dit adres moet zich binnen de *Kubernetes service-adresbereik*.

**Docker Bridge-adres**: het IP-adres en netmasker om toe te wijzen aan de brug Docker. Dit IP-adres moet niet binnen het VNet-IP-adresbereik van het cluster.

## <a name="configure-networking---cli"></a>Configureren van netwerken - CLI

Wanneer u een AKS-cluster met de Azure CLI maakt, kunt u ook geavanceerde netwerken configureren. Gebruik de volgende opdrachten om te maken van een nieuw AKS-cluster met geavanceerde functies voor netwerkbeheer ingeschakeld.

Haal eerst de resource-ID van het subnet voor het bestaande subnet waarin het AKS-cluster wordt gekoppeld:

```console
$ az network vnet subnet list --resource-group myVnet --vnet-name myVnet --query [].id --output tsv

/subscriptions/d5b9d4b7-6fc1-46c5-bafe-38effaed19b2/resourceGroups/myVnet/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/default
```

Gebruik de [az aks maken] [ az-aks-create] opdracht met de `--network-plugin azure` argument voor een cluster maken met geavanceerde netwerken. Update de `--vnet-subnet-id` waarde met de subnet-ID die worden verzameld in de vorige stap:

```azurecli
az aks create --resource-group myAKSCluster --name myAKSCluster --network-plugin azure --vnet-subnet-id <subnet-id> --docker-bridge-address 172.17.0.1/16 --dns-service-ip 10.2.0.10 --service-cidr 10.2.0.0/24
```

## <a name="configure-networking---portal"></a>Configureren van netwerken - portal

De volgende schermafbeelding van de Azure-portal ziet u een voorbeeld van het configureren van deze instellingen tijdens het maken van de AKS-cluster:

![Geavanceerde configuratie van netwerken in Azure portal][portal-01-networking-advanced]

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

De volgende vragen en antwoorden van toepassing op de **Geavanceerd** netwerkconfiguratie.

* *Kan ik virtuele machines implementeren in het clustersubnet van mijn?*

  Nee. Implementatie van VM's in het subnet dat wordt gebruikt door uw Kubernetes-cluster wordt niet ondersteund. VM's kunnen worden geïmplementeerd in hetzelfde VNet, maar in een ander subnet.

* *Kan ik per pod-netwerkbeleid configureren?*

  Nee. Per pod-netwerkbeleid worden momenteel niet ondersteund.

* *Is het maximum aantal schillen kan worden geïmplementeerd naar een knooppunt kunnen worden geconfigureerd?*

  Ja, wanneer u een cluster met de Azure CLI of een Resource Manager-sjabloon implementeert. Zie [Maximum schillen per knooppunt](#maximum-pods-per-node).

* *Hoe configureer ik extra eigenschappen voor het subnet dat ik heb tijdens het maken van de AKS-cluster gemaakt? Bijvoorbeeld, service-eindpunten.*

  De volledige lijst met eigenschappen voor de VNet en subnetten die u tijdens het maken van de AKS-cluster maakt kan worden geconfigureerd in de standaard pagina met de VNet-configuratie in Azure portal.

* *Kan ik een ander subnet binnen mijn cluster VNet gebruiken voor de* **Kubernetes service-adresbereik**?

  Het wordt niet aanbevolen, maar deze configuratie is mogelijk. Het adresbereik van de service is een set van virtuele IP-adressen (VIP's) die Kubernetes wordt toegewezen aan de services in uw cluster. Azure-netwerk heeft geen zichtbaarheid van het service-IP-adresbereik van het Kubernetes-cluster. Vanwege de afwezigheid van inzicht in het adresbereik van het cluster-service is het mogelijk te maken later een nieuw subnet in het VNet dat met het adresbereik van de service overlapt-cluster. Als deze een overlapping optreedt, toewijzen Kubernetes een service een IP-adres dat wordt al gebruikt door een andere resource in het subnet of fouten in onvoorspelbaar gedrag veroorzaken. Door ervoor te zorgen dat u een adresbereik buiten de VNet van het cluster gebruikt, kunt u dit risico overlapping te vermijden.

## <a name="next-steps"></a>Volgende stappen

### <a name="networking-in-aks"></a>Netwerken in AKS

Meer informatie over netwerken in AKS in de volgende artikelen:

[Gebruik een statisch IP-adres aan de load balancer van Azure Kubernetes Service (AKS)](static-ip.md)

[HTTPS inkomend verkeer op Azure Container Service (AKS)](ingress.md)

[Een interne load balancer gebruiken met Azure Container Service (AKS)](internal-lb.md)

### <a name="acs-engine"></a>ACS-Engine

[Azure Container Service Engine (ACS-Engine)] [ acs-engine] is een open-source-project dat Azure Resource Manager-sjablonen die u gebruiken genereert kunt voor het implementeren van clusters op Azure Docker-functionaliteit. Kubernetes, DC/OS, Swarm-modus en Swarm-orchestrators kunnen worden geïmplementeerd met ACS-Engine.

Kubernetes-clusters die zijn gemaakt met de ACS-Engine ondersteunen zowel de [kubenet] [ kubenet] en [Azure CNI] [ cni-networking] invoegtoepassingen. Als zodanig worden basiseigenschappen en geavanceerde scenario's voor netwerken ondersteund door de ACS-Engine.

<!-- IMAGES -->
[advanced-networking-diagram-01]: ./media/networking-overview/advanced-networking-diagram-01.png
[portal-01-networking-advanced]: ./media/networking-overview/portal-01-networking-advanced.png

<!-- LINKS - External -->
[acs-engine]: https://github.com/Azure/acs-engine
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[services]: https://kubernetes.io/docs/concepts/services-networking/service/
[portal]: https://portal.azure.com

<!-- LINKS - Internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-ssh]: aks-ssh.md
[ManagedClusterAgentPoolProfile]: /azure/templates/microsoft.containerservice/managedclusters#managedclusteragentpoolprofile-object
