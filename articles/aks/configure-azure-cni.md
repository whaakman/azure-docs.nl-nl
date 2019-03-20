---
title: Configureren van Azure CNI netwerken in Azure Kubernetes Service (AKS)
description: Informatie over het configureren van Azure CNI (Geavanceerd)-netwerken in Azure Kubernetes Service (AKS), met inbegrip van een AKS-cluster implementeren in een bestaand virtueel netwerk en subnet.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/11/2018
ms.author: iainfou
ms.openlocfilehash: 4bd934c710d6300e95c60742d5873f5b71bdae59
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58002186"
---
# <a name="configure-azure-cni-networking-in-azure-kubernetes-service-aks"></a>Configureren van Azure CNI netwerken in Azure Kubernetes Service (AKS)

Standaard-AKS clusters gebruik [kubenet][kubenet], en een virtueel netwerk en subnet worden voor u gemaakt. Met *kubenet*, knooppunten een IP-adres ophalen uit het subnet van een virtueel netwerk. Netwerkadresomzetting (NAT) wordt dan geconfigureerd op de knooppunten en schillen ontvangen een IP-adres 'verborgen' achter de IP-adres. Deze aanpak vermindert het aantal IP-adressen die u reserveren in de adresruimte van uw netwerk voor pods wilt te gebruiken.

Met [Azure Container netwerken Interface (CNI)][cni-networking], elke pod krijgt u een IP-adres van het subnet en kunnen rechtstreeks worden geopend. Deze IP-adressen moeten uniek zijn in de adresruimte van uw netwerk, en moeten vooraf worden gepland. Elk knooppunt heeft een configuratieparameter voor het maximum aantal schillen worden ondersteund. Het equivalente aantal IP-adressen per knooppunt worden vervolgens een gereserveerd voor dat knooppunt. Deze aanpak vereist meer planning en leidt vaak tot uitputting van IP-adres of de noodzaak voor het opnieuw opbouwen van clusters in een groter subnet aan de vereisten van uw toepassing.

In dit artikel leest u hoe u *Azure CNI* netwerken maken en een subnet van een virtueel netwerk gebruiken voor een AKS-cluster. Zie voor meer informatie over opties voor netwerken en overwegingen met betrekking tot [netwerk basisbegrippen voor Kubernetes en AKS][aks-network-concepts].

## <a name="prerequisites"></a>Vereisten

* Het virtuele netwerk voor het AKS-cluster moet uitgaande internetverbinding toestaan.
* Niet meer dan één AKS-cluster maken in hetzelfde subnet.
* AKS-clusters worden niet gebruikt `169.254.0.0/16`, `172.30.0.0/16`, of `172.31.0.0/16` voor het Kubernetes-service-adresbereik.
* De service-principal die worden gebruikt door het AKS-cluster moet minimaal beschikken over [Inzender voor netwerken](../role-based-access-control/built-in-roles.md#network-contributor) machtigingen op het subnet binnen uw virtuele netwerk. Als u wilt definiëren een [aangepaste rol](../role-based-access-control/custom-roles.md) in plaats van de ingebouwde rol van inzender voor netwerken, de volgende machtigingen zijn vereist:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`

## <a name="plan-ip-addressing-for-your-cluster"></a>IP-adressen voor uw cluster plannen

Clusters die zijn geconfigureerd met het netwerk van Azure CNI vereist extra planning. De grootte van het virtuele netwerk en een subnet moet geschikt zijn voor het aantal schillen die u van plan bent om uit te voeren en het aantal knooppunten voor het cluster.

IP-adressen voor het gehele product en de knooppunten van het cluster worden toegewezen uit het opgegeven subnet binnen het virtuele netwerk. Elk knooppunt is geconfigureerd met een primaire IP-adres. 30 extra IP-adressen worden standaard, vooraf geconfigureerd door Azure CNI die zijn toegewezen aan schillen gepland op het knooppunt. Wanneer u uw cluster uitschalen, wordt elk knooppunt op dezelfde manier geconfigureerd met IP-adressen uit het subnet. U kunt ook zien de [maximale schillen per knooppunt](#maximum-pods-per-node).

> [!IMPORTANT]
> Het aantal IP-adressen die zijn vereist, moet overwegingen voor bijwerken en schalen herverdelen bevatten. Als u het IP-adresbereik voor de ondersteuning van alleen een vast aantal knooppunten hebt ingesteld, kan u een upgrade uitvoert of het cluster wordt geschaald.
>
> - Wanneer u **upgrade** uw AKS-cluster, een nieuw knooppunt wordt geïmplementeerd in het cluster. Services en workloads beginnen om uit te voeren op het nieuwe knooppunt en een oudere knooppunt uit het cluster wordt verwijderd. Dit proces voor rolling upgrade vereist ten minste één extra blokkeren van IP-adressen moet beschikbaar zijn. Het aantal knooppunten wordt vervolgens `n + 1`.
>
> - Wanneer u **schaal** een AKS-cluster, een nieuw knooppunt wordt geïmplementeerd in het cluster. Services en werkbelastingen kunt u beginnen om uit te voeren op het nieuwe knooppunt. Uw IP-adresbereik moet rekening overwegingen met betrekking tot hoe kunt u het aantal knooppunten en schillen die kan worden ondersteund door het cluster kan worden uitgebreid. Een extra knooppunt voor upgrade bewerkingen moet ook worden opgenomen. Het aantal knooppunten wordt vervolgens `n + number-of-additional-scaled-nodes-you-anticipate + 1`.

Als u verwacht de knooppunten die het maximum aantal schillen, uitvoeren dat en regelmatig vernietigen en schillen implementeren, moet u ook rekening houden in enkele extra IP-adressen per knooppunt. Deze extra IP-adressen rekening met die het duurt een paar seconden voor een service moet worden verwijderd en het IP-adres die zijn uitgebracht voor een nieuwe service kunnen worden geïmplementeerd en het adres hebt aanschaft.

Het IP-adres-plan voor een AKS-cluster bestaat uit een virtueel netwerk, ten minste één subnet voor de knooppunten en schillen en Kubernetes service adresbereik.

| Adresbereik / Azure resource | Limieten en grootte |
| --------- | ------------- |
| Virtueel netwerk | Het Azure-netwerk zo groot is als/8 die kan worden, maar is beperkt tot 65.536 geconfigureerde IP-adressen. |
| Subnet | Moet groot genoeg is voor de knooppunten, schillen en alle Kubernetes en Azure-resources die kunnen worden ingericht in het cluster. Als u een interne Load Balancer in Azure implementeert, kunt u voor de front-end-IP-adressen, worden toegewezen uit het subnet van het cluster, geen openbare IP-adressen. De grootte van het gatewaysubnet moet ook rekening upgrade accountbewerkingen of toekomstige behoeften voor vergroten/verkleinen.<p />Voor het berekenen van de *minimale* subnetgrootte, met inbegrip van een extra knooppunt voor upgrade bewerkingen: `(number of nodes + 1) + ((number of nodes + 1) * maximum pods per node that you configure)`<p/>Voorbeeld voor een cluster met 50 knooppunten: `(51) + (51  * 30 (default)) = 1,581` (/ 21 of hoger)<p/>Voorbeeld voor een cluster met 50 knooppunten die ook inrichten een aanvullende 10 knooppunten kan worden uitgebreid: `(61) + (61 * 30 (default)) = 1,891` (/ 21 of hoger)<p>Als u geen een maximum aantal schillen per knooppunt opgeeft wanneer u uw cluster maakt, het maximum aantal schillen per knooppunt is ingesteld op *30*. Het minimale aantal vereiste IP-adressen is gebaseerd op die waarde. Als u de minimale vereisten van IP-adres op een andere maximale waarde berekenen, Zie [het configureren van het maximum aantal schillen per knooppunt](#configure-maximum---new-clusters) kunt u deze waarde instellen wanneer u uw cluster implementeert. |
| Adresbereik van Kubernetes Service | Dit bereik moet niet worden gebruikt door elk netwerkelement op of verbonden met dit virtuele netwerk. Adres van service CIDR moet kleiner zijn dan /12. |
| IP-adres van Kubernetes DNS-service | IP-adres binnen het Kubernetes-service-adresbereik dat wordt gebruikt door clusterservicedetectie (kube-dns). Gebruik niet het eerste IP-adres in het adresbereik, bijvoorbeeld.1. Het eerste adres in het subnetbereik van uw wordt gebruikt voor de *kubernetes.default.svc.cluster.local* adres. |
| Docker bridge-adres | IP-adres (in CIDR-notatie) gebruikt als de Docker bridge-IP-adres op knooppunten. De standaardwaarde van 172.17.0.1/16. |

## <a name="maximum-pods-per-node"></a>Maximale schillen per knooppunt

Het maximum aantal schillen per knooppunt in een AKS-cluster is 110. De *standaard* maximum aantal schillen per knooppunt varieert tussen *kubenet* en *Azure CNI* netwerken en de methode van implementatie van het cluster.

| Implementatiemethode | Kubenet standaard | Azure CNI standaard | Kan worden geconfigureerd tijdens de implementatie |
| -- | :--: | :--: | -- |
| Azure-CLI | 110 | 30 | Ja (maximaal 110) |
| Resource Manager-sjabloon | 110 | 30 | Ja (maximaal 110) |
| Portal | 110 | 30 | Nee |

### <a name="configure-maximum---new-clusters"></a>Maximum - nieuwe clusters configureren

Het configureren van het maximum aantal schillen per knooppunt kunt u *alleen tijdens de cluster implementatie*. Als u met de Azure CLI of met een Resource Manager-sjabloon implementeert, kunt u de maximale schillen per knooppuntwaarde zo hoog 110 instellen.

* **Azure CLI**: Geef de `--max-pods` argument bij het implementeren van een cluster met de [az aks maken] [ az-aks-create] opdracht. De maximumwaarde is 110.
* **Resource Manager-sjabloon**: Geef de `maxPods` eigenschap in de [ManagedClusterAgentPoolProfile] object wanneer u een cluster met een Resource Manager-sjabloon implementeert. De maximumwaarde is 110.
* **Azure-portal**: U kunt het maximale aantal schillen per knooppunt niet wijzigen wanneer u een cluster met de Azure portal implementeert. Azure CNI netwerken clusters zijn beperkt tot 30 schillen per knooppunt wanneer u implementeert met behulp van de Azure portal.

### <a name="configure-maximum---existing-clusters"></a>Maximum - bestaande clusters configureren

U kunt de maximale schillen per knooppunt in een bestaand AKS-cluster niet wijzigen. Alleen wanneer u in eerste instantie het cluster implementeert, kunt u het nummer van de aanpassen.

## <a name="deployment-parameters"></a>Implementatieparameters

Wanneer u een AKS-cluster maakt, kunnen de volgende parameters worden geconfigureerd voor Azure CNI netwerken:

**Virtueel netwerk**: Het virtuele netwerk waarin u wilt de Kubernetes-cluster implementeren. Als u een nieuw virtueel netwerk voor uw cluster maken wilt, selecteert u *nieuw* en volg de stappen in de *virtueel netwerk maken* sectie. Zie voor meer informatie over de limieten en quota's voor een Azure virtual network [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).

**Subnet**: Het subnet binnen het virtuele netwerk waar u het cluster te implementeren. Als u een nieuw subnet in het virtuele netwerk voor uw cluster te maken wilt, selecteert u *nieuw* en volg de stappen in de *subnet maken* sectie. Voor hybride verbindingen, mag niet het adresbereik overlappen met andere virtuele netwerken in uw omgeving.

**Kubernetes-service-adresbereik**: Dit is de set van virtuele IP-adressen die Kubernetes wordt toegewezen aan interne [services] [ services] in uw cluster. U kunt elk bereik met privé-adres dat voldoet aan de volgende vereisten:

* Moet niet binnen het virtuele netwerk IP-adresbereik van het cluster
* Mag niet overlappen met andere virtuele netwerken met die van het virtuele clusternetwerk collega 's
* Mag niet overlappen met een on-premises-IP-adressen
* Moet niet binnen de bereiken `169.254.0.0/16`, `172.30.0.0/16`, of `172.31.0.0/16`

Hoewel het technisch mogelijk om op te geven van een service-adresbereik binnen hetzelfde virtuele netwerk als uw cluster, wordt doen zo niet aanbevolen. Onvoorspelbaar gedrag kan veroorzaken als er overlappende IP-adresbereiken worden gebruikt. Zie voor meer informatie de [Veelgestelde vragen over](#frequently-asked-questions) sectie van dit artikel. Zie voor meer informatie over Kubernetes-services, [Services] [ services] in het Kubernetes-documentatie.

**IP-adres van Kubernetes DNS-service**:  Het IP-adres van het cluster DNS-service. Dit adres moet zich binnen de *Kubernetes service-adresbereik*. Gebruik niet het eerste IP-adres in het adresbereik, bijvoorbeeld.1. Het eerste adres in het subnetbereik van uw wordt gebruikt voor de *kubernetes.default.svc.cluster.local* adres.

**Docker Bridge-adres**: De IP-adres en het IP-masker toewijzen aan de Docker bridge. De Docker Bridge kunt communiceren met de onderliggende beheerplatform AKS-knooppunten. Dit IP-adres mag niet binnen het virtuele netwerk IP-adresbereik van het cluster, en mag niet overlappen met andere adresbereiken in gebruik is op uw netwerk.

## <a name="configure-networking---cli"></a>Configureren van netwerken - CLI

Wanneer u een AKS-cluster met de Azure CLI maakt, kunt u ook Azure CNI netwerken configureren. Gebruik de volgende opdrachten in een nieuw AKS-cluster maken met Azure CNI netwerken ingeschakeld.

Haal eerst de resource-ID van het subnet voor het bestaande subnet waarin het AKS-cluster wordt gekoppeld:

```console
$ az network vnet subnet list \
    --resource-group myVnet \
    --vnet-name myVnet \
    --query [].id --output tsv

/subscriptions/<guid>/resourceGroups/myVnet/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/default
```

Gebruik de [az aks maken] [ az-aks-create] opdracht met de `--network-plugin azure` argument voor een cluster maken met geavanceerde netwerken. Update de `--vnet-subnet-id` waarde met de subnet-ID die worden verzameld in de vorige stap:

```azurecli
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --network-plugin azure \
    --vnet-subnet-id <subnet-id> \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24
```

## <a name="configure-networking---portal"></a>Configureren van netwerken - portal

De volgende schermafbeelding van de Azure-portal ziet u een voorbeeld van het configureren van deze instellingen tijdens het maken van de AKS-cluster:

![Geavanceerde configuratie van netwerken in Azure portal][portal-01-networking-advanced]

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

De volgende vragen en antwoorden van toepassing op de **Azure CNI** netwerkconfiguratie.

* *Kan ik virtuele machines implementeren in het clustersubnet van mijn?*

  Nee. Implementatie van VM's in het subnet dat wordt gebruikt door uw Kubernetes-cluster wordt niet ondersteund. VM's kunnen worden geïmplementeerd in hetzelfde virtuele netwerk, maar in een ander subnet.

* *Kan ik per pod-netwerkbeleid configureren?*

  Kubernetes-netwerkbeleid is momenteel beschikbaar als preview-functie in AKS. Als u wilt beginnen, Zie [beveiliging van verkeer tussen schillen met behulp van beleid voor netwerken in AKS][network-policy].

* *Is het maximum aantal schillen kan worden geïmplementeerd naar een knooppunt kunnen worden geconfigureerd?*

  Ja, wanneer u een cluster met de Azure CLI of een Resource Manager-sjabloon implementeert. Zie [Maximum schillen per knooppunt](#maximum-pods-per-node).

  U kunt het maximale aantal schillen per knooppunt in een bestaand cluster niet wijzigen.

* *Hoe configureer ik extra eigenschappen voor het subnet dat ik heb tijdens het maken van de AKS-cluster gemaakt? Bijvoorbeeld, service-eindpunten.*

  De volledige lijst met eigenschappen voor het virtuele netwerk en subnetten die u tijdens het maken van de AKS-cluster maakt kan worden geconfigureerd in de configuratiepagina van de standaard virtueel netwerk in Azure portal.

* *Kan ik een ander subnet gebruiken in mijn cluster virtuele netwerk voor de* **Kubernetes service-adresbereik**?

  Het wordt niet aanbevolen, maar deze configuratie is mogelijk. Het adresbereik van de service is een set van virtuele IP-adressen (VIP's) die Kubernetes wordt toegewezen aan interne services in uw cluster. Azure-netwerk heeft geen zichtbaarheid van het service-IP-adresbereik van het Kubernetes-cluster. Vanwege de afwezigheid van inzicht in het adresbereik van het cluster-service is het mogelijk te maken later een nieuw subnet in het cluster virtuele netwerk met het service-adresbereik overlapt. Als deze een overlapping optreedt, toewijzen Kubernetes een service een IP-adres dat wordt al gebruikt door een andere resource in het subnet of fouten in onvoorspelbaar gedrag veroorzaken. Door ervoor te zorgen dat u een adresbereik buiten het virtuele netwerk van het cluster gebruikt, kunt u dit risico overlapping te vermijden.

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

### <a name="aks-engine"></a>AKS Engine

[Azure Kubernetes Service Engine (Engine AKS)] [ aks-engine] is een open-source-project die Azure Resource Manager-sjablonen die u gebruiken genereert kunt voor het implementeren van Kubernetes-clusters op Azure.

Kubernetes-clusters die zijn gemaakt met AKS Engine ondersteunen zowel de [kubenet] [ kubenet] en [Azure CNI] [ cni-networking] invoegtoepassingen. Als zodanig worden beide scenario's voor netwerken worden ondersteund door AKS-Engine.

<!-- IMAGES -->
[advanced-networking-diagram-01]: ./media/networking-overview/advanced-networking-diagram-01.png
[portal-01-networking-advanced]: ./media/networking-overview/portal-01-networking-advanced.png

<!-- LINKS - External -->
[aks-engine]: https://github.com/Azure/aks-engine
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
[network-policy]: use-network-policies.md
