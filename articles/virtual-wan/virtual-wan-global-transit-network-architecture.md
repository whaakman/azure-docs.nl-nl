---
title: Azure virtuele WAN globale doorvoer netwerkarchitectuur | Microsoft Docs
description: Meer informatie over de algemene doorvoer netwerkarchitectuur voor virtuele WAN
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: article
ms.date: 05/20/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand global transit network architecture as it relates to Virtual WAN.
ms.openlocfilehash: 114d11f98c6181a03f5ce52527b5e2efea468c42
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65965977"
---
# <a name="global-transit-network-architecture-and-virtual-wan"></a>Globale doorvoer netwerkarchitectuur en virtuele WAN

Netwerkarchitectuur van de algemene doorvoer wordt toegepast door ondernemingen samenvoegen, verbinding maken en beheren van de cloud gerichte moderne onderneming IT-footprint. In een moderne cloud-gebaseerde enterprise hoeft het netwerkverkeer niet te worden backhauled op het hoofdkantoor. Netwerkarchitectuur van de algemene doorvoer is gebaseerd op vertrouwde netwerken concepten en nieuwe concepten die uniek zijn voor de cloud en cloud-gebaseerde architecturen.

![architectuur](./media/virtual-wan-global-transit-network-architecture/architecture2.png)

**Afbeelding 1: Globale tussenliggende netwerk met virtuele WAN**

Moderne ondernemingen vereist alomtegenwoordige connectiviteit tussen hyper-gedistribueerde toepassingen, gegevens en gebruikers in de cloud en on-premises. Azure virtuele WAN kunt een globale doorvoer netwerkarchitectuur doordat alomtegenwoordige, any-to-any connectiviteit tussen vnet's, sites, toepassingen en gebruikers wereldwijd gedistribueerde gegevenssets. Azure virtuele WAN is een beheerd door Microsoft service. Alle netwerkonderdelen die deze service bestaat uit worden gehost en beheerd door Microsoft. Zie voor meer informatie over virtuele WAN, de [virtuele WAN-overzicht](virtual-wan-about.md) artikel.

In de Azure virtuele WAN-architectuur fungeren Azure-regio's als hubs waarop u kunt verbinding maken met uw vertakkingen. Zodra de filialen zijn verbonden, kunt u gebruikmaken van de Azure-backbone voor het maken van branch-naar-VNet en, optioneel, vertakking-vertakking-connectiviteit.

U kunt een virtuele WAN maken door het maken van een enkele virtuele WAN-hub in de regio die het grootste aantal knooppunten (vertakkingen, VNets, gebruikers), en vervolgens te verbinden met de knooppunten die zich in andere regio's naar de hub. U kunt ook als knooppunten zijn geografisch wordt gedistribueerd, kunt u ook regionale hubs maken en de hubs interconnect. De hubs zijn allemaal deel uit van de dezelfde virtuele WAN, maar ze kunnen worden gekoppeld aan verschillende regionale beleidsregels.

## <a name="hub"></a>Hub en spoke-doorvoer

De netwerkarchitectuur van de algemene doorvoer is gebaseerd op een klassieke connectiviteit van de hub en spoke-model waarbij het netwerk in de cloud gehoste 'hub' zorgt voor transitieve-connectiviteit tussen de eindpunten die kan worden verdeeld over verschillende typen 'knooppunten'.
  
In dit model, kan een knooppunt zijn:

* Virtueel netwerk (vnet's)
* Fysieke filiaalsite
* Externe gebruiker
* Internet

![hub en spoke globale doorvoer-diagram](./media/virtual-wan-global-transit-network-architecture/architecture.png)

**Afbeelding 2: Hub-and-spoke**

Afbeelding 2 toont de logische weergave van het wereldwijde netwerk waar geografisch verspreide gebruikers, fysieke locaties en vnet's onderling via een VPN-hub die wordt gehost in de cloud verbonden zijn. Deze architectuur zorgt logische één hop doorvoer connectiviteit tussen de netwerken eindpunten. De knooppunten zijn verbonden met de hub door verschillende Azure netwerkservices zoals ExpressRoute of site-naar-site-VPN voor fysieke vertakkingen, VNet-peering voor vnet's en punt-naar-site VPN voor externe gebruikers.

## <a name="crossregion"></a>Regio-overschrijdende verbindingen

Voor een bedrijf volgt een cloudfootprint doorgaans de fysieke footprint. In de meeste bedrijven toegang tot de cloud vanuit een regio het dichtst bij hun fysieke locatie en gebruikers. Een van de sleutel principals van het wereldwijde netwerkarchitectuur is om regio-overschrijdende verbindingen tussen netwerkentiteiten- en eindpunten. Een cloudfootprint kan meerdere regio's omvatten. Dit betekent dat verkeer van een vertakking die is verbonden met de cloud in één regio bereiken kan, een andere vertakking of een VNet in een andere regio met behulp van de hub-naar-hub-connectiviteit die zich momenteel in preview.

## <a name="any"></a>Any-to-any connectiviteit

Architectuur van de algemene doorvoer netwerk *any-to-any connectiviteit* via een hub in het centrale netwerk. Deze architectuur wordt voorkomen dat of vermindert de noodzaak voor volledige mesh of gedeeltelijke mesh-connectiviteitsmodellen, hoe complexer het bouwen en onderhouden. Routering besturingselement in de hub en spoke-versus mesh-netwerken is bovendien gemakkelijker te configureren en onderhouden.

Any-to-any connectiviteit, kan ondernemingen met gebruikers wereldwijd gedistribueerde, vertakkingen, datacenters, vnet's en toepassingen te verbinden met elkaar via de tussenliggende hub in de context van een algemene architectuur. De tussenliggende hub fungeert als de algemene doorvoer-systeem.

![paden voor gegevensoverdracht](./media/virtual-wan-global-transit-network-architecture/trafficpath.png)

**Afbeelding 3: Virtuele WAN-verkeer-paden**

Azure virtuele WAN ondersteunt de volgende paden van de algemene doorvoer-connectiviteit. De letters tussen haakjes worden toegewezen aan de afbeelding 3.

* Branch-naar-VNet (a)  
* Vertakking vertakking (b)
* Externe gebruiker-naar-VNet (c)
* Externe gebruiker voor filialen (d)
* VNet-naar-VNet met behulp van VNet-peering (e)
* ExpressRoute wereldwijd bereik 

### <a name="branchvnet"></a>Branch-naar-VNet

Branch-naar-VNet is het primaire pad dat wordt ondersteund door Azure virtuele WAN. Dit pad kunt u verbinding maken met vertakkingen Azure IAAS-workloads voor enterprise die zijn geïmplementeerd in Azure VNets. Vertakkingen kunnen worden verbonden met het virtuele WAN via ExpressRoute of site-naar-site VPN. Het verkeer passages met vnet's die zijn verbonden met de virtuele WAN-hubs via VNet-verbindingen.

### <a name="branchbranch"></a>Branch-naar-vertakking

Vertakkingen kunnen worden verbonden met een Azure virtuele WAN hub met behulp van ExpressRoute-circuits en/of site-naar-site VPN-verbindingen. U kunt de branches verbinden met de virtuele WAN-hub die zich in de regio het dichtst bij de vertakking.

Deze optie kunt bedrijven gebruikmaken van de Azure-backbone om vertakkingen verbinding te maken. Hoewel deze mogelijkheid beschikbaar is, moet u de voordelen van filialen met elkaar verbinden via Azure virtuele WAN versus met behulp van een persoonlijke WAN afwegen.

### <a name="usertovnet"></a>Externe gebruiker-naar-VNet

U kunt direct, veilige externe toegang tot Azure met behulp van punt-naar-site-verbindingen vanaf een externe gebruiker-client met een virtueel WAN inschakelen. Externe gebruikers Enterprise hoeft niet langer te hairpin naar de cloud met behulp van een zakelijke VPN.

### <a name="usertobranch"></a>Externe gebruiker voor filialen

Het pad van de externe gebruiker-naar-vertakking kan externe gebruikers die gebruik van een punt-naar-site-verbinding met de toegang van Azure on-premises werkbelastingen en toepassingen maken door de doorvoer via de cloud. Dit pad biedt externe gebruikers de flexibiliteit voor de toegang van workloads die geïmplementeerd in Azure en on-premises zijn. Ondernemingen die kunnen centrale cloud-gebaseerde veilige externe toegang-service in Azure virtuele WAN inschakelen.

### <a name="vnetvnet"></a>VNet-naar-VNet-overdracht met behulp van VNet-peering

Voor vnet's ter ondersteuning van toepassingen met meerdere lagen die worden geïmplementeerd op meerdere VNets met elkaar verbinden, gebruikt u VNet-peering. Een scenario voor het tussenliggende VNet-naar-VNet via Azure virtuele WAN wordt momenteel niet ondersteund, maar is op de Azure-roadmap. VNets verbinden via VNet-Peering is de aanbevolen oplossing voor vnet's die moeten worden verbonden met elkaar. [Gateway-doorvoer](../virtual-network/virtual-network-peering-overview.md#gateways-and-on-premises-connectivity) (in de context van de VNet-peering) is niet vereist voor virtuele WAN omdat virtuele WAN automatisch gateway-doorvoer kunt.

### <a name="globalreach"></a>ExpressRoute wereldwijd bereik

ExpressRoute is een privé- en flexibele manier om uw on-premises netwerken verbinden met de Microsoft-Cloud. Globaal bereik ExpressRoute is een functie van de invoegtoepassing voor ExpressRoute. U kunt ExpressRoute-circuits samen om te maken van een particulier netwerk tussen uw on-premises netwerken koppelen met globaal bereik. Branches die zijn verbonden met Azure virtuele WAN met behulp van ExpressRoute moeten de ExpressRoute globale bereiken om te communiceren met elkaar.

In dit model, kunt elke vertakking die is verbonden met het virtuele WAN-hub met behulp van ExpressRoute verbinden met VNets met behulp van de vertakking-naar-VNet-pad. Branch-vertakking-verkeer wordt niet de hub doorvoer omdat een meer optimale pad via WAN Azure ExpressRoute globaal bereik worden ingeschakeld.

## <a name="security"></a>Beveiliging en beleid beheren

De hub in het virtuele netwerk koppelt en ziet mogelijk alle transitverkeer. Het kan de plaats voor het centrale netwerk functies van de host en -services zoals dergelijke een cloud routering, netwerkbeleid en -beveiliging en toegangsbeheer voor Internet zijn.

## <a name="next-steps"></a>Volgende stappen

Maak een verbinding met virtuele WAN.

* [Site-naar-site-verbindingen met behulp van virtuele WAN](virtual-wan-site-to-site-portal.md)
* [Punt-naar-site-verbindingen met behulp van virtuele WAN](virtual-wan-point-to-site-portal.md)
* [ExpressRoute-verbindingen met virtuele WAN](virtual-wan-expressroute-portal.md)
