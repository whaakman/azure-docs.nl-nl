---
title: Virtuele netwerken van Azure plannen | Microsoft Docs
description: Informatie over het plannen voor virtuele netwerken op basis van uw isolatie, connectiviteit en Locatievereisten.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: 3a4a9aea-7608-4d2e-bb3c-40de2e537200
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/16/2018
ms.author: jdial
ms.openlocfilehash: fd290420c2c755e07f6949750e3a88bcb64682f3
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34656904"
---
# <a name="plan-virtual-networks"></a>Virtuele netwerken plannen

Maken van een virtueel netwerk als u wilt experimenteren met eenvoudig genoeg is, maar waarschijnlijk hebt u meerdere virtuele netwerken na verloop van tijd voor de ondersteuning van de productie-behoeften van uw organisatie gaat implementeren. Met sommige planning kunt zich u implementeren van virtuele netwerken en verbinding maken met de gewenste effectiever resources. De informatie in dit artikel is vooral handig als u al bekend met virtuele netwerken bent en ervaring hebt met hen hebt. Als u niet bekend met virtuele netwerken bent, kunt u lezen [Virtual network-overzicht](virtual-networks-overview.md).

## <a name="naming"></a>Naamgeving

Alle Azure-resources hebben een naam. De naam moet uniek zijn binnen een scope die voor elk resourcetype verschillen kan. Bijvoorbeeld, de naam van een virtueel netwerk moet uniek zijn binnen een [resourcegroep](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), maar kan worden gedupliceerd binnen een [abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) of Azure [regio](https://azure.microsoft.com/regions/#services). Het definiëren van een naamgevingsconventie die u consequent gebruiken kunt bij het benoemen van resources is handig bij het beheren van verschillende netwerkbronnen gedurende een bepaalde periode. Zie voor suggesties [naamconventies](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#networking).

## <a name="regions"></a>Regio's

Alle Azure-resources worden in een Azure-regio en het abonnement gemaakt. Een resource kan alleen worden gemaakt in een virtueel netwerk die in dezelfde regio en hetzelfde abonnement als de resource voorkomt. U kunt wel verbinding maken met virtuele netwerken die bestaan in verschillende abonnementen en regio's. Zie voor meer informatie [connectiviteit](#connectivity). Wanneer u beslist welke regio('s) voor het implementeren van resources in, kunt u overwegen waar gebruikers van de bronnen zich fysiek bevinden:

- Consumenten van bronnen meestal wel verstandig de laagste netwerklatentie tot hun bronnen. Om te bepalen relatieve latenties tussen een opgegeven locatie en de Azure-regio's, Zie [relatieve latenties weergeven](../network-watcher/view-relative-latencies.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Hebt u vereisten voor de hand van vestigingsplaats, onafhankelijkheid, compatibiliteit of tolerantie van gegevens? Als dit het geval is, is het essentieel dat u de regio die wordt uitgelijnd met de vereisten te kiezen. Zie voor meer informatie [Azure locaties](https://azure.microsoft.com/global-infrastructure/geographies/).
- Wilt u tolerantie via Azure beschikbaarheid Zones dat binnen dezelfde Azure-regio voor de bronnen die u implementeren? U kunt resources, zoals virtuele machines (VM) implementeren op verschillende beschikbaarheid zones binnen hetzelfde virtuele netwerk. Niet alle Azure-regio's ondersteunen echter beschikbaarheid zones. Zie voor meer informatie over de zones van de beschikbaarheid en de regio's die deze ondersteunen, [beschikbaarheid zones](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="subscriptions"></a>Abonnementen

U kunt zoveel virtuele netwerken, zoals vereist in elk abonnement maximaal implementeren de [limiet](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits). Sommige organisaties hebben bijvoorbeeld verschillende abonnementen voor verschillende afdelingen. Zie voor meer informatie en overwegingen omtrent het abonnementen [abonnement governance](/azure/architecture/cloud-adoption-guide/subscription-governance#define-your-hierarchy).

## <a name="segmentation"></a>Segmentatie

U kunt meerdere virtuele netwerken per abonnement en per regio maken. U kunt meerdere subnetten in elk virtueel netwerk maken. De overwegingen die volgen, kunt u bepalen hoeveel virtuele netwerken en subnetten die u nodig:

### <a name="virtual-networks"></a>Virtuele netwerken

Een virtueel netwerk is een virtuele, geïsoleerd gedeelte van het openbare Azure-netwerk. Elk virtueel netwerk is toegewezen aan uw abonnement. Zaken die u moet letten wanneer bepalen of een virtueel netwerk of meerdere virtuele netwerken maken in een abonnement:

- Uw organisatie beveiligingsvereisten bestaan voor het verkeer isoleren in afzonderlijke virtuele netwerken? U kunt verbinding maken met virtuele netwerken of niet. Als u verbinding maakt met virtuele netwerken, kunt u een virtueel netwerkapparaat, zoals een firewall zien en beheren van de stroom van verkeer tussen virtuele netwerken kunt implementeren. Zie voor meer informatie [beveiliging](#security) en [connectiviteit](#connectivity).
- Alle organisatorische vereisten bestaan voor het isoleren van virtuele netwerken in afzonderlijke [abonnementen](#subscriptions) of [regio's](#regions)?
- Een [netwerkinterface](virtual-network-network-interface.md) kunt u een virtuele machine om te communiceren met andere bronnen. Elke netwerkinterface heeft een of meer persoonlijke IP-adressen zijn toegewezen. Het aantal netwerkinterfaces en [privé IP-adressen](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) wilt u dat in een virtueel netwerk? Er zijn [limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) aan het aantal netwerkinterfaces en privé IP-adressen die u kunt binnen een virtueel netwerk.
- Wilt u toch verbinding maken met het virtuele netwerk naar een ander virtueel netwerk of on-premises netwerk? U kunt verbinding maken met een aantal virtuele netwerken elkaar of on-premises netwerken, maar andere niet. Zie voor meer informatie [connectiviteit](#connectivity). Elke virtuele netwerk dat u verbinding met een ander virtueel netwerk maakt of on-premises netwerk moet een unieke adresruimte hebben. Elk virtueel netwerk heeft een of meer openbare of particuliere adresbereiken toegewezen aan de adresruimte. Een adresbereik is opgegeven in een internet domain routing (CIDR)-indeling, zoals 10.0.0.0/16. Meer informatie over [-adresbereiken](manage-virtual-network.md#add-or-remove-an-address-range) voor virtuele netwerken.
- Hebt u ook vereisten voor het beheer van de organisatie voor bronnen in verschillende virtuele netwerken? Als u dus resources in afzonderlijke virtuele netwerk verdelen mogelijk te vereenvoudigen [machtingstoewijzing](#permissions) aan personen in uw organisatie of toewijzen van verschillende [beleid](#policies) aan verschillende virtuele netwerken.
- Wanneer u Azure-serviceresources in een virtueel netwerk implementeren, maken ze hun eigen virtuele netwerk. Om te bepalen of een Azure-service een eigen virtuele netwerk maakt, raadpleegt u de gegevens voor elk [Azure-service die kan worden geïmplementeerd in een virtueel netwerk](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network).

### <a name="subnets"></a>Subnetten

Een virtueel netwerk kan worden gesegmenteerd in een of meer subnetten tot de [limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits). Zaken die u moet letten wanneer bepalen of een subnet of meerdere virtuele netwerken maken in een abonnement:

- Elk subnet moet een unieke adresbereik in CIDR-indeling in de adresruimte van het virtuele netwerk hebben. Het adresbereik overlappen niet met andere subnetten in het virtuele netwerk.
- Als u van plan bent sommige resources Azure-service implementeren in een virtueel netwerk, kunnen ze vereisen, of hun eigen subnet maken, zodat er moet voldoende niet-toegewezen ruimte voor te doen. Om te bepalen of een eigen subnet wordt gemaakt door een Azure-service, raadpleegt u de gegevens voor elk [Azure-service die kan worden geïmplementeerd in een virtueel netwerk](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). Als u een virtueel netwerk met een on-premises netwerk via een Azure VPN-Gateway verbinden, moet het virtuele netwerk een speciale subnet voor de gateway hebben. Meer informatie over [gatewaysubnetten](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub).
- Azure routes netwerkverkeer tussen alle subnetten in een virtueel netwerk, standaard. U kunt Azure standaard routering om te voorkomen dat Azure routering tussen subnetten of voor het routeren van verkeer tussen subnetten door middel van een virtueel netwerkapparaat, bijvoorbeeld overschrijven. Als u dat verkeer tussen resources in dezelfde stroom virtueel netwerk via een netwerk (NVA) voor virtueel apparaat vereist, moet u de resources implementeren met verschillende subnetten. Meer informatie [beveiliging](#security).
- U kunt toegang tot Azure-resources zoals een Azure storage-account of een Azure SQL-database met specifieke subnetten met een virtueel netwerk service-eindpunt beperken. Bovendien kunt u toegang tot de resources ontzeggen vanaf internet. U kunt meerdere subnetten maken en inschakelen van een service-eindpunt voor sommige subnetten, maar andere niet. Meer informatie over [service-eindpunten](virtual-network-service-endpoints-overview.md), en de Azure-resources u kunt deze inschakelen voor.
- U kunt nul of één netwerkbeveiligingsgroep voor elk subnet in een virtueel netwerk koppelen. U kunt dezelfde of een andere koppelen, beveiligingsgroep voor elk subnet-netwerk. Elke netwerkbeveiligingsgroep bevat regels die verkeer naar en van bronnen en bestemmingen toestaan of weigeren. Meer informatie over [netwerkbeveiligingsgroepen](#traffic-filtering).

## <a name="security"></a>Beveiliging

U kunt netwerkverkeer naar en van resources in een virtueel netwerk met behulp van netwerkbeveiligingsgroepen en virtuele netwerkapparaten filteren. U kunt bepalen hoe Azure routeert verkeer van subnetten. U kunt ook beperken die in uw organisatie met resources in virtuele netwerken werken kunt.

### <a name="traffic-filtering"></a>Verkeer filteren

- U kunt netwerkverkeer tussen resources in een virtueel netwerk met een netwerkbeveiligingsgroep een NVA filteren van netwerkverkeer en/of filteren. Voor het implementeren van een NVA, zoals een firewall, om netwerkverkeer te filteren, Zie de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?subcategories=appliances&page=1). Bij gebruik van een NVA maken u ook aangepaste routes voor het routeren van verkeer van subnetten naar de NVA. Meer informatie over [verkeersroutering](#traffic-routing).
- Een netwerkbeveiligingsgroep bevat verschillende beveiliging standaardregels die verkeer naar of van resources toestaan of weigeren. Een netwerkbeveiligingsgroep kan worden gekoppeld aan een netwerkinterface of het subnet met de netwerkinterface. Om beheer te vereenvoudigen van beveiligingsregels, verdient het aanbeveling dat u een netwerkbeveiligingsgroep aan afzonderlijke subnetten koppelen, in plaats van afzonderlijke netwerkinterfaces binnen het subnet, indien mogelijk.
- Als andere virtuele machines binnen een subnet verschillende beveiligingsregels worden toegepast moeten, kunt u de netwerkinterface in de virtuele machine op een of meer beveiligingsgroepen voor toepassing koppelen. Een beveiligingsregel kunt u de beveiligingsgroep van een toepassing opgeven in de bron of bestemming. Deze regel vervolgens geldt alleen voor de netwerkinterfaces die lid van de beveiligingsgroep van de toepassing zijn. Meer informatie over [netwerkbeveiligingsgroepen](security-overview.md) en [toepassing beveiligingsgroepen](security-overview.md#application-security-groups).
- Azure maakt verschillende standaard beveiligingsregels binnen elke netwerkbeveiligingsgroep. Een standaardregel kunt alle verkeer tussen alle resources in een virtueel netwerk. Netwerkbeveiliging gebruik om dit gedrag negeren groepen, aangepaste routering om verkeer te leiden naar een NVA of beide. Het raadzaam dat u vertrouwd raken met alle Azure [standaard beveiligingsregels](security-overview.md#default-security-rules) en u begrijpt hoe netwerkbeveiligingsgroepen worden toegepast op een resource.

U kunt bekijken voorbeeld ontwerpen voor het implementeren van een DMZ tussen Azure en het internet via een [NVA](/architecture/reference-architectures/dmz/secure-vnet-dmz?toc=%2Fazure%2Fvirtual-network%2Ftoc.json) of [netwerkbeveiligingsgroepen](virtual-networks-dmz-nsg.md).

### <a name="traffic-routing"></a>voor verkeersroutering

Azure maakt verschillende standaardroutes voor uitgaand verkeer van een subnet. U kunt Azure standaard routering door een routetabel maken en koppelen aan een subnet overschrijven. Veelvoorkomende redenen voor het onderdrukken van Azure standaard routering zijn:
- Omdat u wilt dat verkeer tussen subnetten door middel van een NVA stromen. Voor meer informatie over het [configureren routetabellen om af te dwingen verkeer via een NVA](tutorial-create-route-table-portal.md).
- Omdat u afdwingen van alle internetverkeer door middel van een NVA of on-premises via een Azure VPN-gateway wilt. Forceren internet verkeer on-premises voor controle en logboekregistratie vaak aangeduid als geforceerde tunneling. Meer informatie over het configureren van [geforceerde tunneling](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2Fazure%2Fvirtual-network%2Ftoc.json).

Als u implementeren met aangepaste routering wilt, kunt u vertrouwd raken met [routering in Azure](virtual-networks-udr-overview.md).

## <a name="connectivity"></a>Connectiviteit

U kunt een virtueel netwerk verbinden op andere virtuele netwerken met behulp van virtueel netwerk peering, of op uw on-premises netwerk via een Azure VPN-gateway.

### <a name="peering"></a>Peering

Wanneer u [virtueel netwerk peering](virtual-network-peering-overview.md), de virtuele netwerken kunnen worden ondersteund in de dezelfde of verschillende Azure-regio's. De virtuele netwerken kunnen zich in dezelfde of verschillende Azure-abonnementen, als beide abonnementen zijn toegewezen aan dezelfde Azure Active Directory-tenant. Voordat u maakt een peering, wordt aanbevolen dat u vertrouwd raken met alle van de peering [vereisten en beperkingen](virtual-network-manage-peering.md#requirements-and-constraints). Bandbreedte tussen resources in virtuele netwerken brengen in dezelfde regio is hetzelfde als wanneer de bronnen in hetzelfde virtuele netwerk zijn.

### <a name="vpn-gateway"></a>VPN-gateway

U kunt een Azure [VPN-Gateway](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) aan een virtueel netwerk verbinden met uw lokale netwerk met een [site-naar-site VPN](../vpn-gateway/vpn-gateway-tutorial-vpnconnection-powershell.md?toc=%2fazure%2fvirtual-network%2ftoc.json), of door een specifieke verbinding met Azure [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

U kunt combineren peering en een VPN-gateway maken [hub en spoke-netwerken](/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json), waarbij de virtuele netwerken spoke verbinding met een virtueel netwerk hub maken en de hub maakt verbinding met een on-premises netwerk, bijvoorbeeld.

### <a name="name-resolution"></a>Naamomzetting

De namen van bronnen in een peered virtueel netwerk met behulp van Azure kunnen niet worden omgezet in resources in een virtueel netwerk [ingebouwde DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md). Omzetten van namen in een virtueel netwerk peered, [uw eigen DNS-server implementeren](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server), of gebruik van Azure DNS [persoonlijke domeinen](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Het omzetten van namen tussen resources in een virtueel netwerk en on-premises netwerken moet u uw eigen DNS-server implementeren.

## <a name="permissions"></a>Machtigingen

Maakt gebruik van Azure [toegangsbeheer op basis van rollen](../role-based-access-control/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (RBAC) tot bronnen. Machtigingen zijn toegewezen aan een [bereik](../role-based-access-control/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#scope) in de volgende hiërarchie: abonnement, beheergroep, resourcegroep en afzonderlijke resource. Zie voor meer informatie over de hiërarchie, [ordenen van uw resources](../azure-resource-manager/management-groups-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Met virtuele Azure-netwerken en alle bijbehorende gerelateerde mogelijkheden, zoals peering, netwerkbeveiligingsgroepen, service-eindpunten en routetabellen wilt werken, kunt u leden van uw organisatie toewijzen aan de ingebouwde [eigenaar](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#owner), [Inzender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#contributor), of [Network contributor](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rollen, en vervolgens wijst u de rol aan het juiste bereik. Als u toewijzen specifieke machtigingen voor een subset van virtueel netwerkmogelijkheden wilt, maakt u een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en toewijzen van de specifieke machtigingen vereist voor [virtuele netwerken](manage-virtual-network.md#permissions), [ subnetten en service-eindpunten](virtual-network-manage-subnet.md#permissions), [netwerkinterfaces](virtual-network-network-interface.md#permissions), [peering](virtual-network-manage-peering.md#permissions), [netwerk- en beveiligingsgroepen](manage-network-security-group.md#permissions), of [routetabellen](manage-route-table.md#permissions) aan de rol.

## <a name="policy"></a>Beleid

Azure-beleid kunt u maakt, toewijst en beleidsdefinities beheren. Beleidsdefinities van het afdwingen verschillende regels via uw resources, zodat de resources voldoen aan uw organisatie-standaarden en de serviceovereenkomsten blijven. Azure-beleid wordt een evaluatie van uw resources, het zoeken naar resources die niet compatibel met de beleidsdefinities die u hebt uitgevoerd. U kunt bijvoorbeeld definiëren en toepassen van een beleid waarmee het maken van virtuele netwerken in een specifieke resourcegroep of een regio. Een ander beleid kan vereisen dat elk subnet een netwerkbeveiligingsgroep gekoppeld heeft. Het beleid wordt vervolgens geëvalueerd bij het maken en bijwerken van resources.

Beleidsregels worden toegepast op de volgende hiërarchie: abonnement, beheergroep en resourcegroep. Meer informatie over [Azure beleid](../azure-policy/azure-policy-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of implementeren van een virtueel netwerk [beleidssjabloon](policy-samples.md) voorbeelden.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over alle taken, instellingen en opties voor een [virtueel netwerk](manage-virtual-network.md), [subnet en service-eindpunt](virtual-network-manage-subnet.md), [netwerkinterface](virtual-network-network-interface.md), [peering](virtual-network-manage-peering.md), [netwerk- en beveiligingsgroep](manage-network-security-group.md), of [routetabel](manage-route-table.md).
