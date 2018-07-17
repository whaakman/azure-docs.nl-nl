---
title: Configuratie van het netwerk in Azure Kubernetes Service (AKS)
description: Meer informatie over de basisfuncties en geavanceerde netwerkconfiguratie in Azure Kubernetes Service (AKS).
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/15/2018
ms.author: marsma
ms.openlocfilehash: da78d388c8e9fc9684942342f48902c2a248e3b1
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072296"
---
# <a name="network-configuration-in-azure-kubernetes-service-aks"></a>Configuratie van het netwerk in Azure Kubernetes Service (AKS)

Wanneer u een Azure Kubernetes Service (AKS)-cluster maakt, kunt u selecteren uit twee netwerkopties: **Basic** of **Geavanceerd**.

## <a name="basic-networking"></a>Netwerkgebruik

De **Basic** optie netwerken is de standaardconfiguratie voor het AKS-cluster maken. De netwerkconfiguratie van het cluster en de schillen volledig worden beheerd door Azure, en is geschikt voor implementaties die niet te worden aangepast VNet-configuratie hoeven. U hebt controle over de configuratie van het netwerk, zoals subnetten of het IP-adresbereiken die zijn toegewezen aan het cluster wanneer u netwerkgebruik selecteert.

Knooppunten in een AKS-cluster is geconfigureerd voor gebruik van eenvoudige netwerken de [kubenet] [ kubenet] Kubernetes-invoegtoepassing.

## <a name="advanced-networking"></a>Geavanceerde netwerken

**Geavanceerde** netwerken plaatst uw schillen in een Azure-netwerk (VNet) dat u configureert, zodat ze automatisch verbinding met de VNet-resources en integratie met de uitgebreide set mogelijkheden voor deze aanbieding VNets.
Geavanceerde netwerken is beschikbaar bij het implementeren van AKS-clusters met de [Azure-portal][portal], Azure CLI, of met een Resource Manager-sjabloon.

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

> [!IMPORTANT]
> Elk knooppunt in een AKS-cluster is geconfigureerd voor geavanceerde netwerken kan maximaal hosten **30 schillen** wanneer geconfigureerd met behulp van de Azure-portal.  U kunt de maximale waarde wijzigen alleen door het wijzigen van de eigenschap maxPods bij het implementeren van een cluster met een Resource Manager-sjabloon. Elk VNet dat is ingericht voor gebruik met de invoegtoepassing Azure CNI beperkt tot is **4096 geconfigureerde IP-adressen**.

## <a name="advanced-networking-prerequisites"></a>Geavanceerde netwerken vereisten

* Het VNet voor het AKS-cluster moet uitgaande internetverbinding toestaan.
* Maak niet meer dan één AKS-cluster in hetzelfde subnet.
* Geavanceerde netwerken voor AKS biedt geen ondersteuning voor vnet's die gebruikmaken van Azure privé-DNS-Zones.
* AKS-clusters worden niet gebruikt `169.254.0.0/16`, `172.30.0.0/16`, of `172.31.0.0/16` voor het Kubernetes-service-adresbereik.
* De service-principal gebruikt voor het AKS-cluster moet hebben `Contributor` machtigingen voor de resourcegroep met de bestaande VNet.

## <a name="plan-ip-addressing-for-your-cluster"></a>IP-adressen voor uw cluster plannen

Clusters die zijn geconfigureerd met geavanceerde netwerken vereist extra planning. De grootte van uw VNet en een subnet moet geschikt zijn voor zowel het aantal schillen die u van plan bent om uit te voeren als het aantal knooppunten voor het cluster.

IP-adressen voor het gehele product en de knooppunten van het cluster worden toegewezen uit het opgegeven subnet binnen het VNet. Elk knooppunt is geconfigureerd met een primaire IP-adres, is het IP-adres van het knooppunt en 30 extra IP-adressen vooraf is geconfigureerd door Azure CNI die zijn toegewezen aan schillen gepland naar het knooppunt. Wanneer u uw cluster uitschalen, wordt elk knooppunt op dezelfde manier geconfigureerd met IP-adressen uit het subnet.

Het abonnement van de IP-adres voor een AKS-cluster bestaat uit een VNet, ten minste één subnet voor de knooppunten en schillen, en een Kubernetes-service-adresbereik.

| Adresbereik / Azure resource | Limieten en grootte |
| --------- | ------------- |
| Virtueel netwerk | Azure VNet kan even groot zijn als de/8 die maar mogelijk alleen 4096 geconfigureerd IP-adressen. |
| Subnet | Moet groot genoeg is voor de knooppunten en schillen zijn. Voor het berekenen van de subnetgrootte van uw minimale: (aantal knooppunten) + (aantal knooppunten * schillen per knooppunt). Voor een cluster met 50 knooppunten: (50) + (50 * 30) = 1.550, uw subnet moet een /21 of groter. |
| Adresbereik van Kubernetes service | Dit bereik moet niet worden gebruikt door een willekeurig netwerkelement op of verbonden met dit VNet. Adres van service CIDR moet kleiner zijn dan /12. |
| IP-adres van Kubernetes DNS-service | IP-adres binnen het Kubernetes-service-adresbereik dat wordt gebruikt door clusterservicedetectie (kube-dns). |
| Docker bridge-adres | IP-adres (in CIDR-notatie) gebruikt als de Docker bridge-IP-adres op knooppunten. De standaardwaarde van 172.17.0.1/16. |

Zoals gezegd, elk VNet dat is ingericht voor gebruik met de invoegtoepassing Azure CNI beperkt tot is **4096 geconfigureerde IP-adressen**. Elk knooppunt in een cluster is geconfigureerd voor geavanceerde netwerken kan maximaal hosten **30 schillen**.

## <a name="deployment-parameters"></a>Implementatieparameters

Wanneer een AKS-cluster maakt, de volgende parameters kunnen worden geconfigureerd voor geavanceerde netwerken:

**Virtueel netwerk**: het VNet waarin u wilt de Kubernetes-cluster implementeren. Als u maken van een nieuw VNet voor uw cluster wilt, selecteert u *nieuw* en volg de stappen in de *virtueel netwerk maken* sectie.

**Subnet**: het subnet binnen het VNet waar u het cluster te implementeren. Als u een nieuw subnet in het VNet voor uw cluster te maken wilt, selecteert u *nieuw* en volg de stappen in de *subnet maken* sectie.

**Kubernetes-service-adresbereik**: de *Kubernetes service-adresbereik* is het IP-adresbereik van waaruit de adressen die zijn toegewezen aan Kubernetes-services in uw cluster (Zie voor meer informatie over Kubernetes-services, [ Services] [ services] in het Kubernetes-documentatie).

De Kubernetes-service IP-adresbereik:

* Moet niet binnen het VNet-IP-adresbereik van het cluster
* Mag niet overlappen met een andere vnet's die van het cluster VNet collega 's
* Mag niet overlappen met een on-premises-IP-adressen

Onvoorspelbaar gedrag kan veroorzaken als er overlappende IP-adresbereiken worden gebruikt. Bijvoorbeeld, als een schil probeert te krijgen tot een IP-adres buiten het cluster, en er ook IP moet een IP-adres voor de service gebeurt, ziet u mogelijk onvoorspelbaar gedrag en fouten.

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

  Standaard kan elk knooppunt een maximum van 30 schillen hosten. U kunt de maximale waarde wijzigen door alleen aan te passen de `maxPods` eigenschap bij het implementeren van een cluster met een Resource Manager-sjabloon.

* *Hoe configureer ik extra eigenschappen voor het subnet dat ik heb tijdens het maken van de AKS-cluster gemaakt? Bijvoorbeeld, service-eindpunten.*

  De volledige lijst met eigenschappen voor de VNet en subnetten die u tijdens het maken van de AKS-cluster maakt kan worden geconfigureerd in de standaard pagina met de VNet-configuratie in Azure portal.

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
