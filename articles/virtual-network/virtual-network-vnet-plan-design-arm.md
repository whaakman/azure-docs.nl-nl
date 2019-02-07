---
title: Virtuele Azure-netwerken plannen | Microsoft Docs
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
ms.openlocfilehash: ef293b39d0e82cdd26e0c41af5d63d0459064017
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55820790"
---
# <a name="plan-virtual-networks"></a>Virtuele netwerken plannen

Het maken van een virtueel netwerk om te experimenteren met is simpel, maar waarschijnlijk hebt u meerdere virtuele netwerken na verloop van tijd voor de ondersteuning van de productie-behoeften van uw organisatie gaat implementeren. Met een planning, kunt u zich het implementeren van virtuele netwerken en verbinding maken met de resources die u effectiever nodig. De informatie in dit artikel is vooral handig als u al bekend met virtuele netwerken bent en hebt ervaring hebt met hen. Als u niet bekend met virtuele netwerken bent, dat u leest, het raadzaam [overzicht van Virtual network](virtual-networks-overview.md).

## <a name="naming"></a>Naamgeving

Alle Azure-resources hebben een naam. De naam moet uniek zijn binnen een bereik, die voor elk resourcetype verschillen kan. Bijvoorbeeld, de naam van een virtueel netwerk moet uniek zijn binnen een [resourcegroep](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), maar kunnen worden gedupliceerd. binnen een [abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) of Azure [regio](https://azure.microsoft.com/regions/#services). Het definiëren van een naamgevingsconventie die u consequent gebruiken kunt bij de naamgeving van resources is handig bij het beheren van verschillende netwerkbronnen na verloop van tijd. Zie voor suggesties, [naamconventies](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#networking).

## <a name="regions"></a>Regio's

Alle Azure-resources worden gemaakt in een Azure-regio en een abonnement. Een resource kan alleen worden gemaakt in een virtueel netwerk dat zich in dezelfde regio en hetzelfde abonnement als de resource. U kunt echter virtuele netwerken koppelen die zich in verschillende abonnementen en regio's. Zie voor meer informatie, [connectiviteit](#connectivity). Wanneer u bepaalt welke regio('s) om resources in te implementeren, moet u overwegen waar gebruikers van de resources zich fysiek bevinden:

- Gebruikers van resources het meestal wel verstandig de laagste netwerklatentie tot hun bronnen. Om te bepalen de relatieve latentie tussen een opgegeven locatie en Azure-regio's, Zie [weergeven van relatieve latentie](../network-watcher/view-relative-latencies.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Hebt u gegevensvereisten gegevenslocatie, soevereiniteit, naleving en tolerantie? Als dit het geval is, is het essentieel dat u de regio die met de vereisten overeenstemt te kiezen. Zie voor meer informatie, [Azure-geografieën](https://azure.microsoft.com/global-infrastructure/geographies/).
- U moet tolerantie in meerdere Azure-Beschikbaarheidszones binnen dezelfde Azure-regio voor de resources die u hebt geïmplementeerd? U kunt resources, zoals virtuele machines (VM) implementeren voor verschillende beschikbaarheidszones binnen hetzelfde virtuele netwerk. Niet alle Azure-regio's ondersteuning echter voor beschikbaarheidszones. Zie voor meer informatie over beschikbaarheidszones en de regio's die ondersteuning bieden voor deze [beschikbaarheidszones](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="subscriptions"></a>Abonnementen

U kunt zo veel virtuele netwerken als vereiste binnen elk abonnement maximaal implementeren de [limiet](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits). Sommige organisaties hebben bijvoorbeeld verschillende abonnementen voor verschillende afdelingen. Zie voor meer informatie en overwegingen over abonnementen, [abonnementsgovernance](/azure/architecture/cloud-adoption-guide/subscription-governance#define-your-hierarchy).

## <a name="segmentation"></a>Segmentatie

U kunt meerdere virtuele netwerken per abonnement of per regio maken. U kunt meerdere subnetten in elk virtueel netwerk maken. De overwegingen die volgen, kunt u bepalen hoeveel virtuele netwerken en subnetten die u nodig:

### <a name="virtual-networks"></a>Virtuele netwerken

Een virtueel netwerk is een gedeelte van het virtuele, geïsoleerd van de openbare Azure-netwerk. Elk virtueel netwerk is toegewezen aan uw abonnement. Dingen om rekening moet houden bij besluit te maken van een virtueel netwerk of meerdere virtuele netwerken in een abonnement:

- Uw organisatie beveiligingsvereisten bestaan voor het verkeer isoleren in afzonderlijke virtuele netwerken? U kunt verbinding maken met virtuele netwerken of niet. Als u verbinding maken met virtuele netwerken, kunt u een virtueel netwerkapparaat, zoals een firewall, voor het beheren van de verkeersstroom tussen de virtuele netwerken kunt implementeren. Zie voor meer informatie, [security](#security) en [connectiviteit](#connectivity).
- Nog een organisatorische vereisten bestaan voor het isoleren van virtuele netwerken in afzonderlijke [abonnementen](#subscriptions) of [regio's](#regions)?
- Een [netwerkinterface](virtual-network-network-interface.md) kan een virtuele machine om te communiceren met andere resources. Elke netwerkinterface heeft een of meer privé IP-adressen worden toegewezen. Het aantal netwerkinterfaces en [privé IP-adressen](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) wilt u dat in een virtueel netwerk? Er zijn [limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) aan het aantal netwerkinterfaces en privé IP-adressen die u kunt binnen een virtueel netwerk.
- Wilt u verbinding maken met het virtuele netwerk naar een ander virtueel netwerk of on-premises netwerk? U kunt sommige virtuele netwerken met elkaar of on-premises netwerken, maar niet voor andere verbinden. Zie voor meer informatie, [connectiviteit](#connectivity). Elk virtueel netwerk dat u verbinding met een ander virtueel netwerk maken, of on-premises netwerk, moet een unieke adresruimte hebben. Elk virtueel netwerk heeft een of meer openbaar of privé adresbereiken toegewezen aan de adresruimte. Een adresbereik is opgegeven in (classless internet domein routering CIDR)-indeling, zoals 10.0.0.0/16. Meer informatie over [-adresbereiken](manage-virtual-network.md#add-or-remove-an-address-range) voor virtuele netwerken.
- Hebt u een organisatie-beheer-vereisten voor resources in verschillende virtuele netwerken? Als dus u resources in afzonderlijke virtueel netwerk scheidt mogelijk voor het vereenvoudigen van [machtigingstoewijzing](#permissions) naar personen in uw organisatie of andere beleidsregels toewijzen aan verschillende virtuele netwerken.
- Wanneer u een Azure-serviceresources in een virtueel netwerk implementeert, maken ze hun eigen virtuele netwerk. Om te bepalen of een eigen virtuele netwerk wordt gemaakt door een Azure-service, raadpleegt u de gegevens voor elke [Azure-service die kan worden geïmplementeerd in een virtueel netwerk](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network).

### <a name="subnets"></a>Subnetten

Een virtueel netwerk kan worden gesegmenteerd in een of meer subnetten tot de [limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits). Dingen om rekening moet houden bij besluit te maken van één subnet of meerdere virtuele netwerken in een abonnement:

- Elk subnet moet een unieke adresbereik in CIDR-indeling in de adresruimte van het virtuele netwerk hebben. Het adresbereik mag niet overlappen met andere subnetten in het virtuele netwerk.
- Als u van plan bent voor het implementeren van sommige Azure-serviceresources in een virtueel netwerk, kunnen ze nodig hebt, of hun eigen subnet maken, dus er moet voldoende niet-toegewezen ruimte voor deze om dit te doen. Om te bepalen of een Azure-service maakt u een eigen subnet, raadpleegt u de gegevens voor elke [Azure-service die kan worden geïmplementeerd in een virtueel netwerk](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). Als u een virtueel netwerk verbinden met een on-premises netwerk met behulp van een Azure VPN-Gateway, moet het virtuele netwerk een speciaal subnet voor de gateway hebben. Meer informatie over [gatewaysubnetten](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub).
- Azure routeert het netwerkverkeer tussen alle subnetten in een virtueel netwerk, standaard. U kunt van Azure-standaardinstelling routering om te voorkomen dat Azure routering tussen subnetten of het routeren van verkeer tussen subnetten via een virtueel netwerkapparaat, bijvoorbeeld negeren. Als u vereist dat verkeer tussen resources in dezelfde stroom virtueel netwerk via een virtueel netwerkapparaat (NVA), moet u de resources implementeren op verschillende subnetten. Meer informatie in [security](#security).
- U kunt toegang tot Azure-resources, zoals een Azure storage-account of een Azure SQL-database tot specifieke subnetten met een service-eindpunt voor virtueel netwerk beperken. U kunt bovendien toegang weigeren tot de resources van het internet. U kunt meerdere subnetten maken en inschakelen van een service-eindpunt voor sommige subnetten, maar niet voor andere. Meer informatie over [service-eindpunten](virtual-network-service-endpoints-overview.md), en de Azure-resources u deze kunt inschakelen.
- U kunt nul of één netwerkbeveiligingsgroep aan elk subnet in een virtueel netwerk koppelen. U kunt hetzelfde, of een andere koppelen, beveiligingsgroep voor elk subnet-netwerk. Elke netwerkbeveiligingsgroep bevat regels die verkeer naar en van bronnen en bestemmingen toestaan of weigeren. Meer informatie over [netwerkbeveiligingsgroepen](#traffic-filtering).

## <a name="security"></a>Beveiliging

U kunt het netwerkverkeer naar en van resources in een virtueel netwerk met behulp van netwerkbeveiligingsgroepen en virtuele netwerkapparaten filteren. U kunt bepalen hoe Azure routeert verkeer van subnetten. U kunt ook beperken wie er binnen uw organisatie met resources in virtuele netwerken werken kunt.

### <a name="traffic-filtering"></a>Verkeer filteren

- U kunt het netwerkverkeer tussen resources in een virtueel netwerk met behulp van een netwerkbeveiligingsgroep, een NVA die filters netwerkverkeer en/of filteren. Voor het implementeren van een NVA, zoals een firewall om netwerkverkeer te filteren, Zie de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?subcategories=appliances&page=1). Wanneer u een NVA gebruikt, maakt u ook aangepaste routes om verkeer van subnetten naar de NVA te routeren. Meer informatie over [routering van verkeer in](#traffic-routing).
- Een netwerkbeveiligingsgroep bevat verschillende standaardbeveiligingsregels die verkeer naar of van resources toestaan of weigeren. Een netwerkbeveiligingsgroep kan worden gekoppeld aan een netwerkinterface en/of het subnet dat de netwerkinterface zich bevindt. Ter vereenvoudiging van beheer van de beveiligingsregels voor verbindingen, verdient het aanbeveling dat u een netwerkbeveiligingsgroep aan afzonderlijke subnetten koppelen, in plaats van afzonderlijke netwerkinterfaces binnen het subnet, indien mogelijk.
- Als verschillende virtuele machines binnen een subnet verschillende beveiligingsregels worden toegepast moeten, kunt u de netwerkinterface op de virtuele machine aan een of meer beveiligingsgroepen voor toepassing koppelen. Een beveiligingsregel kunt u een toepassingsbeveiligingsgroep opgeven in de bron of bestemming. Deze regel vervolgens geldt alleen voor de netwerkinterfaces die lid van de toepassingsbeveiligingsgroep zijn. Meer informatie over [netwerkbeveiligingsgroepen](security-overview.md) en [toepassingsbeveiligingsgroepen](security-overview.md#application-security-groups).
- Azure maakt verschillende standaard beveiligingsregels binnen elke netwerkbeveiligingsgroep. Een standaardregel kunt alle verkeer toe tussen alle resources in een virtueel netwerk. Dit gedrag negeren, gebruikt u netwerkbeveiliging groepen, aangepaste routering om verkeer te routeren naar een NVA, of beide. Het wordt aanbevolen dat u vertrouwd met alle Azure raakt [standaard beveiligingsregels](security-overview.md#default-security-rules) en te begrijpen hoe regels voor netwerkbeveiligingsgroepen worden toegepast op een resource.

Vindt u voorbeeld ontwerpen voor het implementeren van een DMZ tussen Azure en het internet via een [NVA](/azure/architecture/reference-architectures/dmz/secure-vnet-dmz?toc=%2Fazure%2Fvirtual-network%2Ftoc.json) of [netwerkbeveiligingsgroepen](virtual-networks-dmz-nsg.md).

### <a name="traffic-routing"></a>Routering van verkeer

Azure maakt verschillende standaardroutes voor uitgaand verkeer van een subnet. U kunt van Azure-standaardinstelling routering door een routetabel maken en koppelen aan een subnet negeren. Veelvoorkomende redenen voor het onderdrukken van Azure standaard routering zijn:
- Omdat u wilt dat verkeer tussen subnetten via een NVA stromen. Voor meer informatie over hoe u [configureren routetabellen om af te dwingen dat verkeer via een NVA](tutorial-create-route-table-portal.md).
- Omdat u afdwingen dat alle internetverkeer via een NVA, of on-premises, via een Azure VPN-gateway wilt. Afdwingen van internet verkeer on-premises voor controle en logboekregistratie wordt vaak aangeduid als geforceerde tunneling. Meer informatie over het configureren van [geforceerde tunneling](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2Fazure%2Fvirtual-network%2Ftoc.json).

Als u nodig hebt voor het implementeren van aangepaste routering, wordt het aanbevolen dat u bekend zijn met [routering in Azure](virtual-networks-udr-overview.md).

## <a name="connectivity"></a>Connectiviteit

U kunt verbinding maken met een virtueel netwerk naar andere virtuele netwerken met behulp van peering in virtuele netwerken of naar uw on-premises netwerk met behulp van een Azure VPN-gateway.

### <a name="peering"></a>Peering

Bij het gebruik van [peering op virtueel netwerk](virtual-network-peering-overview.md), de virtuele netwerken kunnen zich in de dezelfde of verschillende Azure-regio's wordt ondersteund. De virtuele netwerken kunnen zich in hetzelfde of een ander Azure-abonnementen (zelfs abonnementen die behoren tot verschillende Azure Active Directory-tenants). Voordat u een peering, wordt aanbevolen dat u vertrouwd met alle van de peering raakt [vereisten en beperkingen](virtual-network-manage-peering.md#requirements-and-constraints). Bandbreedte tussen resources in virtuele netwerken aan elkaar gekoppeld in dezelfde regio is hetzelfde als wanneer de resources zich in hetzelfde virtuele netwerk.

### <a name="vpn-gateway"></a>VPN-gateway

Kunt u een Azure [VPN-Gateway](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) aan een virtueel netwerk verbinden met uw on-premises netwerk met een [site-naar-site VPN](../vpn-gateway/vpn-gateway-tutorial-vpnconnection-powershell.md?toc=%2fazure%2fvirtual-network%2ftoc.json), of met behulp van een speciale verbinding met Azure [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

U kunt combineren peering en een VPN-gateway maken [hub en spoke-netwerken](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json), waarbij virtuele spoke-netwerken verbinding met een virtuele hub-netwerk maken en de hub die verbinding met een on-premises netwerk, bijvoorbeeld maakt.

### <a name="name-resolution"></a>Naamomzetting

De namen van resources in een gekoppeld virtueel netwerk met behulp van Azure kunnen niet worden omgezet door resources in een virtueel netwerk [ingebouwde DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md). Omzetten van namen in een gekoppeld virtueel netwerk, [uw eigen DNS-server implementeren](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server), of gebruik van Azure DNS [persoonlijke domeinen](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Het omzetten van namen tussen resources in een virtueel netwerk- en on-premises netwerken moet u uw eigen DNS-server implementeren.

## <a name="permissions"></a>Machtigingen

Maakt gebruik van Azure [op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (RBAC) met bronnen. Machtigingen zijn toegewezen aan een [bereik](../role-based-access-control/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#scope) in de volgende hiërarchie: Abonnement, beheergroep, resourcegroep en afzonderlijke resource. Zie voor meer informatie over de hiërarchie, [ordenen van uw resources](../azure-resource-manager/management-groups-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Als u wilt werken met Azure virtual networks en al hun verwante mogelijkheden zoals peering, netwerkbeveiligingsgroepen, service-eindpunten en routetabellen, u leden van uw organisatie kunt toewijzen aan de ingebouwde [eigenaar](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#owner), [Inzender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#contributor), of [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rollen, en vervolgens wijst u de rol aan het juiste bereik. Als u toewijzen van specifieke machtigingen voor een subset van mogelijkheden voor virtueel netwerk wilt, maakt u een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en toewijzen van de specifieke machtigingen die vereist zijn voor [virtuele netwerken](manage-virtual-network.md#permissions), [ subnetten en service-eindpunten](virtual-network-manage-subnet.md#permissions), [netwerkinterfaces](virtual-network-network-interface.md#permissions), [peering](virtual-network-manage-peering.md#permissions), [netwerk- en beveiligingsgroepen](manage-network-security-group.md#permissions), of [routetabellen](manage-route-table.md#permissions) aan de rol.

## <a name="policy"></a>Beleid

Azure Policy kunt u maken, toewijzen en beheren van beleidsdefinities. Beleidsdefinities afdwingen verschillende regels voor uw resources, zodat de resources voldoen aan de standaarden van de organisatie en de service level agreements blijven. Azure Policy wordt een evaluatie van uw resources, zoeken naar resources die niet compatibel zijn met de beleidsdefinities die u hebt uitgevoerd. U kunt bijvoorbeeld definiëren en een beleid toegepast dat Hiermee worden gemaakt van virtuele netwerken in een specifieke resourcegroep of een regio kunnen. Een ander beleid kunt vereisen dat elk subnet een netwerkbeveiligingsgroep zijn gekoppeld heeft. De beleidsregels worden vervolgens geëvalueerd bij het maken en bijwerken van resources.

Beleid wordt toegepast op de volgende hiërarchie: Abonnement, beheergroep en resourcegroep. Meer informatie over [Azure policy](../azure-policy/azure-policy-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of implementeren van een virtueel netwerk [beleidssjabloon](policy-samples.md) voorbeelden.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over alle taken, instellingen en opties voor een [virtueel netwerk](manage-virtual-network.md), [subnet en service-eindpunt](virtual-network-manage-subnet.md), [netwerkinterface](virtual-network-network-interface.md), [peering](virtual-network-manage-peering.md), [netwerk- en beveiligingsgroep](manage-network-security-group.md), of [routetabel](manage-route-table.md).
