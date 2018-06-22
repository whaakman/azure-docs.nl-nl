---
title: Netwerkconfiguratie in Azure Kubernetes Service (AKS)
description: Meer informatie over de basiseigenschappen en geavanceerde netwerkconfiguratie in Azure Kubernetes Service (AKS).
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/15/2018
ms.author: marsma
ms.openlocfilehash: 207accc30e10c4e2bed5b713fc59e2f9ad86a876
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309846"
---
# <a name="network-configuration-in-azure-kubernetes-service-aks"></a>Netwerkconfiguratie in Azure Kubernetes Service (AKS)

Wanneer u een Azure Kubernetes Service (AKS)-cluster maakt, kunt u selecteren uit twee netwerkopties: **Basic** of **Geavanceerd**.

## <a name="basic-networking"></a>Netwerkgebruik

De **Basic** networking optie is de standaardconfiguratie voor het maken van het cluster AKS. De netwerkconfiguratie van het cluster en het gehele product volledig door Azure worden beheerd en geschikt is voor implementaties die niet te worden aangepast VNet-configuratie hoeven. U hoeft geen controle over de netwerkconfiguratie, zoals subnetten of het IP-adresbereiken die zijn toegewezen aan het cluster wanneer u basisnetwerken selecteert.

Knooppunten in een cluster AKS is geconfigureerd voor eenvoudige netwerken gebruik de [kubenet] [ kubenet] Kubernetes-invoegtoepassing.

## <a name="advanced-networking"></a>Geavanceerde netwerken

**Geavanceerde** netwerken plaatst het gehele product in een Azure-netwerk (VNet) die u configureert, meeleveren automatische verbinding met de VNet-resources en integratie met de uitgebreide set mogelijkheden die aanbieding VNets.
Geavanceerde netwerken is beschikbaar wanneer u clusters AKS implementeren met de [Azure-portal][portal], Azure CLI of met Resource Manager-sjabloon.

Knooppunten in een cluster AKS is geconfigureerd voor gebruik van geavanceerde netwerken de [Azure Container netwerken Interface (CNI)] [ cni-networking] Kubernetes-invoegtoepassing.

![Diagram van twee knooppunten met bruggen verbinding maken met elk op één Azure-VNet][advanced-networking-diagram-01]

## <a name="advanced-networking-features"></a>Geavanceerde functies voor netwerkbeheer

Geavanceerde netwerkvoorzieningen bieden de volgende voordelen:

* Uw cluster AKS in een bestaand VNet implementeren of maak een nieuw VNet en een subnet voor uw cluster.
* Elke schil in het cluster een IP-adres in het VNet is toegewezen en rechtstreeks kan communiceren met andere gehele product in het cluster en de andere knooppunten in het VNet.
* Een schil kan verbinding maken met andere services in een peered VNet en on-premises netwerken via ExpressRoute en site-naar-site (S2S) VPN-verbindingen. Er zijn ook gehele product bereikbaar van on-premises.
* Tonen een service Kubernetes intern of extern via de Azure Load Balancer. Ook een functie van basisnetwerken.
* Gehele product in een subnet die ingeschakeld voor service-eindpunten zijn kunnen veilig verbinding maken met Azure-services, bijvoorbeeld Azure Storage en SQL-database.
* Gebruik de gebruiker gedefinieerde routes (UDR) om verkeer te leiden van gehele product aan een virtueel netwerkapparaat.
* Gehele product hebben toegang tot bronnen op het openbare Internet. Ook een functie van basisnetwerken.

> [!IMPORTANT]
> Elk knooppunt in een cluster AKS is geconfigureerd voor geavanceerde netwerken kan maximaal hosten **30 gehele product** wanneer geconfigureerd met behulp van de Azure-portal.  Alleen door het wijzigen van de eigenschap maxPods bij het implementeren van een cluster met Resource Manager-sjabloon kunt u de maximale waarde. Elke VNet ingericht voor gebruik met de Azure-CNI-invoegtoepassing beperkt tot is **4096 geconfigureerde IP-adressen**.

## <a name="advanced-networking-prerequisites"></a>Geavanceerde netwerken vereisten

* Het VNet voor het cluster AKS moet uitgaande internetverbinding toestaan.
* Maak niet meer dan één AKS cluster in hetzelfde subnet.
* Geavanceerd netwerkgebruik AKS biedt geen ondersteuning voor VNets die gebruikmaken van Azure persoonlijke DNS-Zones.
* AKS clusters worden niet gebruikt `169.254.0.0/16`, `172.30.0.0/16`, of `172.31.0.0/16` voor de Kubernetes service-adresbereik.
* De service-principal die is gebruikt voor het cluster AKS moet hebben `Contributor` machtigingen voor de resourcegroep met de bestaande VNet.

## <a name="plan-ip-addressing-for-your-cluster"></a>IP-adressen voor uw cluster plannen

Clusters die zijn geconfigureerd met geavanceerde netwerken vereisen aanvullende planning. De grootte van uw VNet en een subnet moet geschikt zijn voor zowel het nummer van het gehele product die u van plan bent om uit te voeren als het aantal knooppunten voor het cluster.

IP-adressen voor het gehele product en de knooppunten van het cluster toegewezen uit het opgegeven subnet binnen het VNet. Elk knooppunt is geconfigureerd met een primaire IP-adres is het IP-adres van het knooppunt en 30 extra IP-adressen vooraf geconfigureerd door Azure CNI die zijn toegewezen aan het gehele product gepland naar het knooppunt. Wanneer u uw cluster hebt uitgebreid, wordt elk knooppunt op dezelfde manier geconfigureerd met IP-adressen van het subnet.

Het plan IP-adres voor een cluster AKS bestaat uit een VNet, ten minste één subnet voor de knooppunten en het gehele product, en een Kubernetes service-adresbereik.

| -Adresbereik / Azure resource | Limieten en schaling |
| --------- | ------------- |
| Virtueel netwerk | Azure VNet kan even groot zijn als de/8 die maar mogelijk alleen 4096 geconfigureerd IP-adressen. |
| Subnet | Moet groot genoeg voor de knooppunten en het gehele product. Voor het berekenen van de subnetgrootte van uw minimale: (aantal knooppunten) + (aantal knooppunten * gehele product per knooppunt). Voor een cluster 50 knooppunten: (50) + (50 * 30) = 1.550, uw subnet moet een /21 of groter. |
| Kubernetes service-adresbereik | Dit bereik moet niet worden gebruikt door elk element van het netwerk op of verbonden met dit VNet. CIDR-adres van de service moet kleiner zijn dan /12. |
| IP-adres van Kubernetes DNS-service | IP-adres binnen de Kubernetes-adresbereik dat wordt gebruikt door clusterservicedetectie (kube-dns)-service. |
| Adres van docker-brug | IP-adres (in CIDR-notatie) gebruikt als de brug Docker-IP-adres op knooppunten. De standaardwaarde van 172.17.0.1/16. |

Zoals gezegd, elke VNet ingericht voor gebruik met de Azure-CNI-invoegtoepassing beperkt tot is **4096 geconfigureerde IP-adressen**. Elk knooppunt in een cluster is geconfigureerd voor geavanceerde netwerken kan maximaal hosten **30 gehele product**.

## <a name="deployment-parameters"></a>Implementatieparameters

Wanneer een AKS-cluster maakt, de volgende parameters kunnen worden geconfigureerd voor geavanceerde netwerken:

**Virtueel netwerk**: de VNet waarin u wilt implementeren van het cluster Kubernetes. Als u maken van een nieuw VNet voor uw cluster wilt, selecteert u *nieuw* en volg de stappen in de *virtueel netwerk maken* sectie.

**Subnet**: het subnet binnen het VNet waar u wilt implementeren van het cluster. Als u maken van een nieuw subnet in het VNet voor uw cluster wilt, selecteert u *nieuw* en volg de stappen in de *subnet maken* sectie.

**Kubernetes service adresbereik**: de *Kubernetes service adresbereik* is het IP-adresbereik van waaruit de adressen die zijn toegewezen aan Kubernetes services in uw cluster (Zie voor meer informatie over services Kubernetes, [ Services] [ services] in de documentatie van Kubernetes).

Het Kubernetes service IP-adresbereik:

* Moet niet binnen het VNet-IP-adresbereik van het cluster
* Mogen elkaar niet overlappen met een andere vnet's waarmee het cluster VNet samenwerkt
* Mogen elkaar niet overlappen met een lokale IP-adressen

Onvoorspelbaar gedrag kan veroorzaken als er overlappende IP-adresbereiken worden gebruikt. Bijvoorbeeld, als een schil probeert te krijgen tot een IP-adres buiten het cluster, en als dat gebeurt ook IP moet een service IP-adres, ziet u mogelijk onvoorspelbaar gedrag en fouten.

**Kubernetes DNS-service IP-adres**: het IP-adres voor de DNS-service van het cluster. Dit adres moet binnen het *Kubernetes service adresbereik*.

**Docker Bridge adres**: het IP-adres en IP-masker toewijzen aan de Docker-brug. Dit IP-adres moet niet binnen het VNet-IP-adresbereik van het cluster.

## <a name="configure-networking---cli"></a>Networking - CLI configureren

Wanneer u een cluster AKS met de Azure CLI maakt, kunt u ook geavanceerde netwerken configureren. Gebruik de volgende opdrachten voor het maken van een nieuw cluster met AKS met geavanceerde netwerkfuncties ingeschakeld.

Haal eerst de subnet-ID voor de bestaande subnet waarin het cluster AKS wordt gekoppeld:

```console
$ az network vnet subnet list --resource-group myVnet --vnet-name myVnet --query [].id --output tsv

/subscriptions/d5b9d4b7-6fc1-46c5-bafe-38effaed19b2/resourceGroups/myVnet/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/default
```

Gebruik de [az aks maken] [ az-aks-create] opdracht met de `--network-plugin azure` argument voor het maken van een cluster met geavanceerde netwerken. Update de `--vnet-subnet-id` waarde met de subnet-ID in de vorige stap verzameld:

```azurecli
az aks create --resource-group myAKSCluster --name myAKSCluster --network-plugin azure --vnet-subnet-id <subnet-id> --docker-bridge-address 172.17.0.1/16 --dns-service-ip 10.2.0.10 --service-cidr 10.2.0.0/24
```

## <a name="configure-networking---portal"></a>Configureren van netwerken - portal

De volgende schermafbeelding van de Azure-portal toont een voorbeeld van het configureren van deze instellingen tijdens het maken van de cluster AKS:

![Geavanceerde netwerkconfiguratie in de Azure portal][portal-01-networking-advanced]

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

De volgende vragen en antwoorden van toepassing op de **Geavanceerd** netwerkconfiguratie.

* *Kan ik geavanceerde netwerken configureren met de Azure CLI*

  Nee. Geavanceerde netwerken wordt momenteel beschikbaar is, alleen wanneer het implementeren van AKS clusters in de Azure portal of met Resource Manager-sjabloon.

* *Kan ik virtuele machines implementeren in mijn subnet van het cluster?*

  Nee. Implementatie van virtuele machines in het subnet dat wordt gebruikt door uw cluster Kubernetes wordt niet ondersteund. Virtuele machines kunnen worden geïmplementeerd in hetzelfde VNet, maar in een ander subnet.

* *Kan ik per schil netwerkbeleid configureren?*

  Nee. Per schil netwerkbeleid worden momenteel niet ondersteund.

* *Is het maximum aantal gehele product worden geïmplementeerd op een knooppunt kunnen worden geconfigureerd?*

  Elk knooppunt kan maximaal 30 gehele product host standaard. U kunt de maximale waarde wijzigen door alleen aan te passen de `maxPods` eigenschap bij het implementeren van een cluster met Resource Manager-sjabloon.

* *Hoe kan ik extra eigenschappen voor het subnet dat ik heb tijdens het maken van de cluster AKS gemaakt configureren? Bijvoorbeeld, service-eindpunten.*

  De volledige lijst met eigenschappen voor het VNet en subnetten die u tijdens het maken van AKS cluster maakt kan worden geconfigureerd in de standaard pagina VNet-configuratie in de Azure portal.

## <a name="next-steps"></a>Volgende stappen

### <a name="networking-in-aks"></a>Netwerken in AKS

Meer informatie over netwerken in AKS in de volgende artikelen:

[Gebruik een statisch IP-adres met de Azure Kubernetes Service (AKS) load balancer](static-ip.md)

[HTTPS, inkomend op Azure Container Service (AKS)](ingress.md)

[Een interne load balancer gebruiken met Azure Container Service (AKS)](internal-lb.md)

### <a name="acs-engine"></a>ACS-Engine

[Azure Container Service Engine (ACS-Engine)] [ acs-engine] is een open source-project dat Azure Resource Manager-sjablonen die u gebruiken genereert kunt voor het implementeren van clusters Docker-functionaliteit in Azure. Kubernetes, DC/OS Swarm-modus en Swarm orchestrators kunnen worden geïmplementeerd met de ACS-Engine.

Kubernetes clusters die zijn gemaakt met de ACS-Engine ondersteuning voor zowel de [kubenet] [ kubenet] en [Azure CNI] [ cni-networking] invoegtoepassingen gebruikt. Als zodanig worden basiseigenschappen en geavanceerde scenario's voor netwerken ondersteund door de ACS-Engine.

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
