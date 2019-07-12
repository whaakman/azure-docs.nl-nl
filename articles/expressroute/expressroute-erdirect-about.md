---
title: Over ExpressRoute Direct - Azure | Microsoft Docs
description: Deze pagina biedt een overzicht van ExpressRoute Direct
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: jaredro
ms.custom: seodec18
ms.openlocfilehash: e598cc03a1b7b4999719152540866c7168130e03
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807481"
---
# <a name="about-expressroute-direct"></a>Over ExpressRoute Direct

ExpressRoute Direct biedt de mogelijkheid om rechtstreeks in het wereldwijde netwerk van Microsoft op peeringlocaties strategisch verdeeld over de hele wereld verbinding te maken. ExpressRoute Direct biedt twee 100 Gbps of 10 Gbps-connectiviteit, die ondersteuning biedt voor actief/actief-verbindingen op schaal.

Belangrijke functies van ExpressRoute Direct bevatten, maar niet beperkt tot:

* Massale gegevensopname in services als Storage en Cosmos DB
* Fysieke isolatie voor branches die worden geregeld en vereisen toegewezen en geïsoleerd verbinding, zoals: het bankwezen, de overheid en de detailhandel
* Gedetailleerde controle van circuitdistributie op basis van bedrijfsonderdelen

## <a name="onboard-to-expressroute-direct"></a>Onboarding van ExpressRoute Direct

Voordat u ExpressRoute Direct gebruikt, moet u eerst uw abonnement registreren. Als u wilt inschrijven, stuur een E-mail naar <ExpressRouteDirect@microsoft.com> door uw abonnements-ID, met inbegrip van de volgende gegevens:

* Scenario's die u nodig hebt om uit te voeren met **ExpressRoute Direct**
* Voorkeuren voor locatie - Zie [Partners en peeringlocaties](expressroute-locations-providers.md) voor een volledige lijst van alle locaties
* Tijdlijn voor implementatie
* Andere vragen

## <a name="expressroute-using-a-service-provider-and-expressroute-direct"></a>ExpressRoute met behulp van een serviceprovider en ExpressRoute Direct

| **ExpressRoute met behulp van een serviceprovider** | **ExpressRoute Direct** | 
| --- | --- |
| Maakt gebruik van serviceproviders om in te schakelen voor snelle onboarding en connectiviteit in bestaande infrastructuur | 100 en Gbps/10 Gbps-infrastructuur het volledige beheer van alle lagen
| Kan worden geïntegreerd met honderden van met inbegrip van Ethernet- en MPLS-providers | Direct/toegewezen capaciteit voor gereguleerde industrieën en enorme gegevensopname |
| Circuits SKU's van 50 Mbps naar 10 Gbps | De klant kunt Direct een combinatie van de volgende SKU's op 100 Gbps ExpressRoute-circuit selecteren: <ul><li>5 Gbps</li><li>10 Gbps</li><li>40 Gbps</li><li>100 Gbps</li></ul> De klant kunt Direct een combinatie van de volgende SKU's van 10 Gbps ExpressRoute-circuit selecteren:<ul><li>1 Gbps</li><li>2 Gbps</li><li>5 Gbps</li><li>10 Gbps</li></ul>
| Geoptimaliseerd voor één tenant | Geoptimaliseerd voor één tenant/Cloud serviceproviders / meerdere bedrijfseenheden

## <a name="expressroute-direct-circuits"></a>ExpressRoute Direct-circuits

Met Microsoft Azure ExpressRoute kunt u uw on-premises netwerken in de Microsoft Cloud uitbreiden via een persoonlijke verbinding die wordt gefaciliteerd door een connectiviteitsprovider. Met ExpressRoute kunt u verbindingen tot stand brengen met Microsoft Cloud-services, zoals Microsoft Azure, Office 365 en Dynamics 365.  

Elke locatie toegang heeft tot het wereldwijde netwerk van Microsoft en hebben toegang tot andere regio's in een geopolitieke zone standaard en hebben toegang tot alle globale regio's met een premium-circuit.  

De functionaliteit in de meeste gevallen is gelijk aan-circuits die gebruikmaken van een ExpressRoute-provider om te werken. Ter ondersteuning van meer granulariteit en nieuwe functionaliteit die met behulp van ExpressRoute Direct, zijn er bepaalde kernfuncties die aanwezig zijn op directe ExpressRoute-Circuits.

## <a name="circuit-skus"></a>Circuit-SKU 's

ExpressRoute Direct biedt ondersteuning voor scenario's voor het opnemen van enorme gegevens in Azure storage en andere services big data. ExpressRoute-circuits op 100 Gbps ExpressRoute Direct nu ook ondersteuning voor **40 Gbps** en **100 Gbps** circuit SKU's. De fysieke poortparen zijn **100 of 10 Gbps** alleen en kan meerdere virtuele circuits. Circuit grootten:

| **100 Gbps ExpressRoute Direct** | **10 Gbps ExpressRoute Direct** | 
| --- | --- |
| **Bandbreedte geabonneerd**: 200 Gbps | **Bandbreedte geabonneerd**: 20 Gbps |
| <ul><li>5 Gbps</li><li>10 Gbps</li><li>40 Gbps</li><li>100 Gbps</li></ul> | <ul><li>1 Gbps</li><li>2 Gbps</li><li>5 Gbps</li><li>10 Gbps</li></ul>

## <a name="technical-requirements"></a>Technische vereisten

* Microsoft Enterprise Edge Router (MSEE) Interfaces:
    * Dubbele 10 of 100 Gigabit Ethernet-poorten alleen in combinatie van router
    * Één modus LR Fiber-connectiviteit
    * IPv4 en IPv6
    * IP-MTU 1500 bytes

* Switch/Router Layer 2/laag 3-connectiviteit:
    * 1 802.1Q (Dot1Q) tag of twee Tag 802.1Q (QinQ) moet ondersteunen inkapseling taggen
    * Ethertype 0x8100 =
    * Moet toevoegen de buitenste VLAN-label (STAG) op basis van de VLAN-ID die is opgegeven door de Microsoft - *alleen van toepassing op QinQ*
    * Moet ondersteuning bieden voor meerdere BGP-sessies (VLAN's) per poort en het apparaat
    * IPv4 en IPv6-connectiviteit. *IPv6 wordt geen aanvullende onderliggende interface worden gemaakt. IPv6-adres wordt toegevoegd aan bestaande onderliggende interface*. 
    * Optioneel: [In twee richtingen doorsturen van detectie (BFD)](https://docs.microsoft.com/azure/expressroute/expressroute-bfd) ondersteunen, dat standaard op alle privé-Peerings op ExpressRoute-circuits is geconfigureerd

## <a name="vlan-tagging"></a>VLAN-Tagging

ExpressRoute Direct biedt ondersteuning voor zowel QinQ en Dot1Q VLAN-tagging.

* **QinQ VLAN-Tagging** kunnen voor geïsoleerde Routeringsdomeinen op een per ExpressRoute-circuit gefactureerd. Azure dynamisch een S-Tag bij het maken van circuit kan worden toegewezen en kan niet worden gewijzigd. Elke peering van het circuit (privé- en Microsoft), wordt een unieke C-code gebruiken als het VLAN. De C-Tag is niet uniek zijn in circuits op de poorten die ExpressRoute rechtstreeks vereist.

* **Dot1Q VLAN-Tagging** kunt voor een enkel VLAN gelabeld op een per ExpressRoute directe poort paar gefactureerd. Een C-code die wordt gebruikt op een peering moet uniek zijn in alle circuits en op het paar van de poort ExpressRoute Direct-peerings.

## <a name="workflow"></a>Werkstroom

[![Werkstroom](./media/expressroute-erdirect-about/workflow1.png)](./media/expressroute-erdirect-about/workflow1.png#lightbox)

## <a name="sla"></a>SLA

ExpressRoute Direct biedt de dezelfde SLA op ondernemingsniveau met redundante verbindingen actief/actief in het wereldwijde netwerk van Microsoft. ExpressRoute-infrastructuur is overbodig en connectiviteit in het wereldwijde netwerk van Microsoft is overbodig en diverse en kan worden geschaald dienovereenkomstig aan de klantvereisten. 

## <a name="next-steps"></a>Volgende stappen

[ExpressRoute Direct configureren](expressroute-howto-erdirect.md)
