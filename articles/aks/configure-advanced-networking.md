---
title: Geavanceerde netwerken geconfigureerd in Azure Kubernetes Service (AKS)
description: Informatie over het configureren van geavanceerde network in Azure Kubernetes Service (AKS), met inbegrip van een AKS-cluster implementeren in een bestaand virtueel netwerk en subnet.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/11/2018
ms.author: iainfou
ms.openlocfilehash: 87c3ab9624116e9c1c61041531fdf5d3b26117e1
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49381024"
---
# <a name="configure-advanced-networking-in-azure-kubernetes-service-aks"></a>Geavanceerde netwerken configureren in Azure Kubernetes Service (AKS)

Standaard-AKS clusters gebruik *basic* netwerkservices, deze maakt en configureert een virtueel netwerk en subnet voor gebruik met het cluster. Als u meer controle over deze netwerken opties, zoals de IP-adresbereiken, kunt u in plaats daarvan gebruiken *geavanceerde* netwerken. Met geavanceerde netwerken, kunt u ook een AKS-cluster maken in een bestaand virtueel netwerk en subnet. Deze bestaand virtueel netwerk biedt vaak verbinding met een on-premises netwerk met behulp van Azure ExpressRoute of Site-naar-Site VPN.

Dit artikel leest u hoe geavanceerde netwerken gebruiken om te maken en gebruiken van een virtueel netwerk met een AKS-cluster. Zie voor meer algemene informatie over netwerken [netwerk basisbegrippen voor Kubernetes en AKS][aks-network-concepts].

## <a name="prerequisites"></a>Vereisten

* Het virtuele netwerk voor het AKS-cluster moet uitgaande internetverbinding toestaan.
* Maak niet meer dan één AKS-cluster in hetzelfde subnet.
* AKS-clusters worden niet gebruikt `169.254.0.0/16`, `172.30.0.0/16`, of `172.31.0.0/16` voor het Kubernetes-service-adresbereik.
* De service-principal die worden gebruikt door het AKS-cluster moet minimaal beschikken over [Inzender voor netwerken](../role-based-access-control/built-in-roles.md#network-contributor) machtigingen op het subnet binnen uw virtuele netwerk. Als u wilt definiëren een [aangepaste rol](../role-based-access-control/custom-roles.md) in plaats van de ingebouwde rol van inzender voor netwerken, de volgende machtigingen zijn vereist:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`

## <a name="plan-ip-addressing-for-your-cluster"></a>IP-adressen voor uw cluster plannen

Clusters die zijn geconfigureerd met geavanceerde netwerken vereist extra planning. De grootte van het virtuele netwerk en een subnet moet geschikt zijn voor zowel het aantal schillen die u van plan bent om uit te voeren als het aantal knooppunten voor het cluster.

IP-adressen voor het gehele product en de knooppunten van het cluster worden toegewezen uit het opgegeven subnet binnen het virtuele netwerk. Elk knooppunt is geconfigureerd met een primaire IP-adres, is het IP-adres van het knooppunt en 30 extra IP-adressen vooraf is geconfigureerd door Azure CNI die zijn toegewezen aan schillen gepland naar het knooppunt. Wanneer u uw cluster uitschalen, wordt elk knooppunt op dezelfde manier geconfigureerd met IP-adressen uit het subnet.

Het IP-adres-plan voor een AKS-cluster bestaat uit een virtueel netwerk, ten minste één subnet voor de knooppunten en schillen en Kubernetes service adresbereik.

| Adresbereik / Azure resource | Limieten en grootte |
| --------- | ------------- |
| Virtueel netwerk | Het Azure-netwerk zo groot is als/8 die kan worden, maar is beperkt tot 65.536 geconfigureerde IP-adressen. |
| Subnet | Moet groot genoeg is voor de knooppunten, schillen en alle Kubernetes en Azure-resources die kunnen worden ingericht in het cluster. Als u een interne Load Balancer in Azure implementeert, kunt u voor de front-end-IP-adressen, worden toegewezen uit het subnet van het cluster, geen openbare IP-adressen. <p/>Voor het berekenen van *minimale* subnetgrootte: `(number of nodes) + (number of nodes * pods per node)` <p/>Voorbeeld voor een cluster met 50 knooppunten: `(50) + (50 * 30) = 1,550` (/ 21 of hoger) |
| Adresbereik van Kubernetes service | Dit bereik moet niet worden gebruikt door elk netwerkelement op of verbonden met dit virtuele netwerk. Adres van service CIDR moet kleiner zijn dan /12. |
| IP-adres van Kubernetes DNS-service | IP-adres binnen het Kubernetes-service-adresbereik dat wordt gebruikt door clusterservicedetectie (kube-dns). |
| Docker bridge-adres | IP-adres (in CIDR-notatie) gebruikt als de Docker bridge-IP-adres op knooppunten. De standaardwaarde van 172.17.0.1/16. |

## <a name="maximum-pods-per-node"></a>Maximale schillen per knooppunt

Het standaard maximum aantal schillen per knooppunt in een AKS-cluster varieert tussen de basisopties en geavanceerde netwerken en de methode van implementatie van het cluster.

| Implementatiemethode | Basic | Geavanceerd | Kan worden geconfigureerd tijdens de implementatie |
| -- | :--: | :--: | -- |
| Azure-CLI | 110 | 30 | Ja (maximaal 110) |
| Resource Manager-sjabloon | 110 | 30 | Ja (maximaal 110) |
| Portal | 110 | 30 | Nee |

### <a name="configure-maximum---new-clusters"></a>Maximum - nieuwe clusters configureren

Het configureren van het maximum aantal schillen per knooppunt kunt u *alleen tijdens de cluster implementatie*. Als u met de Azure CLI of met een Resource Manager-sjabloon implementeert, kunt u de maximale schillen per knooppuntwaarde zo hoog 110 instellen.

* **Azure CLI**: Geef de `--max-pods` argument bij het implementeren van een cluster met de [az aks maken] [ az-aks-create] opdracht. De maximumwaarde is 110.
* **Resource Manager-sjabloon**: Geef de `maxPods` eigenschap in de [ManagedClusterAgentPoolProfile] object wanneer u een cluster met een Resource Manager-sjabloon implementeert. De maximumwaarde is 110.
* **Azure-portal**: U kunt het maximale aantal schillen per knooppunt niet wijzigen wanneer u een cluster met de Azure portal implementeert. Geavanceerde VPN-clusters zijn beperkt tot 30 schillen per knooppunt wanneer u implementeert met behulp van de Azure portal.

### <a name="configure-maximum---existing-clusters"></a>Maximum - bestaande clusters configureren

U kunt de maximale schillen per knooppunt in een bestaand AKS-cluster niet wijzigen. Alleen wanneer u in eerste instantie het cluster implementeert, kunt u het nummer van de aanpassen.

## <a name="deployment-parameters"></a>Implementatieparameters

Wanneer u een AKS-cluster maakt, kunnen de volgende parameters worden geconfigureerd voor geavanceerde netwerken:

**Virtueel netwerk**: het virtuele netwerk waarin u wilt de Kubernetes-cluster implementeren. Als u een nieuw virtueel netwerk voor uw cluster maken wilt, selecteert u *nieuw* en volg de stappen in de *virtueel netwerk maken* sectie. Zie voor meer informatie over de limieten en quota's voor een Azure virtual network [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).

**Subnet**: het subnet binnen het virtuele netwerk waar u het cluster te implementeren. Als u een nieuw subnet in het virtuele netwerk voor uw cluster te maken wilt, selecteert u *nieuw* en volg de stappen in de *subnet maken* sectie.

**Kubernetes-service-adresbereik**: dit is de set van virtuele IP-adressen die Kubernetes wordt toegewezen aan [services] [ services] in uw cluster. U kunt elk bereik met privé-adres dat voldoet aan de volgende vereisten:

* Moet niet binnen het virtuele netwerk IP-adresbereik van het cluster
* Mag niet overlappen met andere virtuele netwerken met die van het virtuele clusternetwerk collega 's
* Mag niet overlappen met een on-premises-IP-adressen
* Moet niet binnen de bereiken `169.254.0.0/16`, `172.30.0.0/16`, of `172.31.0.0/16`

Hoewel het technisch mogelijk om op te geven van een service-adresbereik binnen hetzelfde virtuele netwerk als uw cluster, wordt doen zo niet aanbevolen. Onvoorspelbaar gedrag kan veroorzaken als er overlappende IP-adresbereiken worden gebruikt. Zie voor meer informatie de [Veelgestelde vragen over](#frequently-asked-questions) sectie van dit artikel. Zie voor meer informatie over Kubernetes-services, [Services] [ services] in het Kubernetes-documentatie.

**IP-adres van Kubernetes DNS-service**: het IP-adres van het cluster DNS-service. Dit adres moet zich binnen de *Kubernetes service-adresbereik*.

**Docker Bridge-adres**: het IP-adres en netmasker om toe te wijzen aan de brug Docker. Dit IP-adres moet niet binnen het virtuele netwerk IP-adresbereik van het cluster.

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

  Nee. Implementatie van VM's in het subnet dat wordt gebruikt door uw Kubernetes-cluster wordt niet ondersteund. VM's kunnen worden geïmplementeerd in hetzelfde virtuele netwerk, maar in een ander subnet.

* *Kan ik per pod-netwerkbeleid configureren?*

  Nee. Per pod-netwerkbeleid worden momenteel niet ondersteund.

* *Is het maximum aantal schillen kan worden geïmplementeerd naar een knooppunt kunnen worden geconfigureerd?*

  Ja, wanneer u een cluster met de Azure CLI of een Resource Manager-sjabloon implementeert. Zie [Maximum schillen per knooppunt](#maximum-pods-per-node).

  U kunt het maximale aantal schillen per knooppunt in een bestaand cluster niet wijzigen.

* *Hoe configureer ik extra eigenschappen voor het subnet dat ik heb tijdens het maken van de AKS-cluster gemaakt? Bijvoorbeeld, service-eindpunten.*

  De volledige lijst met eigenschappen voor het virtuele netwerk en subnetten die u tijdens het maken van de AKS-cluster maakt kan worden geconfigureerd in de configuratiepagina van de standaard virtueel netwerk in Azure portal.

* *Kan ik een ander subnet gebruiken in mijn cluster virtuele netwerk voor de* **Kubernetes service-adresbereik**?

  Het wordt niet aanbevolen, maar deze configuratie is mogelijk. Het adresbereik van de service is een set van virtuele IP-adressen (VIP's) die Kubernetes wordt toegewezen aan de services in uw cluster. Azure-netwerk heeft geen zichtbaarheid van het service-IP-adresbereik van het Kubernetes-cluster. Vanwege de afwezigheid van inzicht in het adresbereik van het cluster-service is het mogelijk te maken later een nieuw subnet in het cluster virtuele netwerk met het service-adresbereik overlapt. Als deze een overlapping optreedt, toewijzen Kubernetes een service een IP-adres dat wordt al gebruikt door een andere resource in het subnet of fouten in onvoorspelbaar gedrag veroorzaken. Door ervoor te zorgen dat u een adresbereik buiten het virtuele netwerk van het cluster gebruikt, kunt u dit risico overlapping te vermijden.

## <a name="next-steps"></a>Volgende stappen

### <a name="networking-in-aks"></a>Netwerken in AKS

Meer informatie over netwerken in AKS in de volgende artikelen:

- [Gebruik een statisch IP-adres aan de load balancer van Azure Kubernetes Service (AKS)](static-ip.md)
- [Een interne load balancer gebruiken met Azure Container Service (AKS)](internal-lb.md)

- [Een eenvoudige ingangscontroller met verbinding met het externe netwerk maken][aks-ingress-basic]
- [De HTTP-aanvraag routering invoegtoepassing inschakelen][aks-http-app-routing]
- [Een controller voor binnenkomend verkeer die gebruikmaakt van een privé-interne netwerken en IP-adres maken][aks-ingress-internal]
- [Maken van een controller voor binnenkomend verkeer met een dynamische openbare IP-adres en we gaan coderen voor het automatisch genereren van TLS-certificaten configureren][aks-ingress-tls]
- [Maken van een controller voor binnenkomend verkeer met een statisch openbaar IP-adres en we gaan coderen voor het automatisch genereren van TLS-certificaten configureren][aks-ingress-static-tls]

### <a name="acs-engine"></a>ACS-Engine

[Azure Container Service Engine (ACS-Engine)] [ acs-engine] is een open-source-project dat Azure Resource Manager-sjablonen die u gebruiken genereert kunt voor het implementeren van clusters op Azure Docker-functionaliteit. Kubernetes, DC/OS, Swarm-modus en Swarm-orchestrators kunnen worden geïmplementeerd met ACS-Engine.

Kubernetes-clusters die zijn gemaakt met de ACS-Engine ondersteunen zowel de [kubenet] [ kubenet] en [Azure CNI] [ cni-networking] invoegtoepassingen. Als zodanig worden basiseigenschappen en geavanceerde scenario's voor netwerken ondersteund door de ACS-Engine.

<!-- IMAGES -->
[advanced-networking-diagram-01]: ./media/networking-overview/advanced-networking-diagram-01.png
[portal-01-networking-advanced]: ./media/networking-overview/portal-01-networking-advanced.png

<!-- LINKS - External -->
[acs-engine]: https://github.com/Azure/acs-engine
[services]: https://kubernetes.io/docs/concepts/services-networking/service/
[portal]: https://portal.azure.com
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-ssh]: ssh.md
[ManagedClusterAgentPoolProfile]: /azure/templates/microsoft.containerservice/managedclusters#managedclusteragentpoolprofile-object
[aks-network-concepts]: concepts-network.md
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-internal]: ingress-internal-ip.md
