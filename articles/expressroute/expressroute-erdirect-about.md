---
title: Over ExpressRoute Direct - Azure | Microsoft Docs
description: Deze pagina biedt een overzicht van ExpressRoute Direct (Preview)
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: 2f984eb8cb09e5d65c4a366b827f695c739003f3
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53163818"
---
# <a name="about-expressroute-direct-preview"></a>Over ExpressRoute Direct (Preview)

ExpressRoute Direct biedt de mogelijkheid om rechtstreeks in het wereldwijde netwerk van Microsoft op peeringlocaties strategisch verdeeld over de hele wereld verbinding te maken. ExpressRoute Direct biedt twee 100-Gbps-connectiviteit, die ondersteuning biedt voor actief/actief-verbindingen op schaal.

Belangrijke functies van ExpressRoute Direct bevatten, maar niet beperkt tot:

* Massale gegevensopname in services als Storage en Cosmos DB
* Fysieke isolatie voor branches die worden geregeld en vereisen toegewezen en geïsoleerd verbinding, zoals: Bankieren-, overheids- en detailhandel
* Gedetailleerde controle van circuitdistributie op basis van bedrijfsonderdelen

> [!IMPORTANT]
> ExpressRoute Direct is momenteel in Preview.
>
> Deze openbare preview-versie wordt aangeboden zonder serviceovereenkomst en moet niet worden gebruikt voor productieworkloads. Bepaalde functies worden mogelijk niet ondersteund, zijn mogelijk beperkt of zijn mogelijk niet beschikbaar in alle Azure-locaties. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="enroll-in-the-preview"></a>Registreren voor de preview-versie

Voordat u ExpressRoute Direct gebruikt, moet u eerst uw abonnement in de Preview-versie worden geregistreerd. Als u wilt inschrijven, stuur een E-mail naar <ExpressRouteDirect@microsoft.com> door uw abonnements-ID, met inbegrip van de volgende gegevens:

* Scenario's die u nodig hebt om uit te voeren met **ExpressRoute Direct**
* Voorkeuren voor locatie - Zie [Partners en peeringlocaties](expressroute-locations-providers.md) voor een volledige lijst van alle locaties
* Tijdlijn voor implementatie
* Andere vragen

## <a name="expressroute-using-a-service-provider-and-expressroute-direct"></a>ExpressRoute met behulp van een serviceprovider en ExpressRoute Direct

| **ExpressRoute met behulp van een serviceprovider** | **ExpressRoute Direct** | 
| --- | --- |
| Maakt gebruik van serviceproviders om in te schakelen voor snelle onboarding en connectiviteit in bestaande infrastructuur | 100 Gbps-infrastructuur en het volledige beheer van alle lagen
| Kan worden geïntegreerd met honderden van met inbegrip van Ethernet- en MPLS-providers | Direct/toegewezen capaciteit voor gereguleerde industrieën en enorme gegevensopname |
| Circuits SKU's van 50 Mbps naar 10 Gbps | Klanten kan een combinatie van de volgende circuit SKU's selecteren: 5 Gbps, 10 Gbps, 40 Gbps, 100 Gbps - beperkt tot een totaal van 200 Gbps
| Geoptimaliseerd voor één tenant | Geoptimaliseerd voor één tenant/Cloud serviceproviders / meerdere bedrijfseenheden

## <a name="expressroute-direct-circuits"></a>ExpressRoute Direct-circuits

Met Microsoft Azure ExpressRoute kunt u uw on-premises netwerken in de Microsoft Cloud uitbreiden via een persoonlijke verbinding die wordt gefaciliteerd door een connectiviteitsprovider. Met ExpressRoute kunt u verbindingen tot stand brengen met Microsoft Cloud-services, zoals Microsoft Azure, Office 365 en Dynamics 365.  

Elke locatie toegang heeft tot het wereldwijde netwerk van Microsoft en hebben toegang tot andere regio's in een geopolitieke zone standaard en hebben toegang tot alle globale regio's met een premium-circuit.  

De functionaliteit in de meeste gevallen is gelijk aan-circuits die gebruikmaken van een ExpressRoute-provider om te werken. Ter ondersteuning van meer granulariteit en nieuwe functionaliteit die met behulp van ExpressRoute Direct, zijn er bepaalde kernfuncties die aanwezig zijn op directe ExpressRoute-Circuits.

## <a name="circuit-skus"></a>Circuit-SKU 's

ExpressRoute Direct biedt ondersteuning voor scenario's voor het opnemen van enorme gegevens in Azure storage en andere services big data. ExpressRoute circuits op ExpressRoute Direct nu ook ondersteuning voor **40 Gbps** en **100 Gbps** circuit SKU's.

## <a name="vlan-tagging"></a>VLAN-Tagging

ExpressRoute Direct biedt ondersteuning voor zowel QinQ en Dot1Q VLAN-tagging.

* **QinQ VLAN-Tagging** kunnen voor geïsoleerde Routeringsdomeinen op een per ExpressRoute-circuit gefactureerd. Azure dynamisch een S-Tag bij het maken van circuit kan worden toegewezen en kan niet worden gewijzigd. Elke peering van het circuit (privé- en Microsoft), wordt een unieke C-code gebruiken als het VLAN. De C-Tag is niet uniek zijn in circuits op de poorten die ExpressRoute rechtstreeks vereist.

* **Dot1Q VLAN-Tagging** kunt voor een enkel VLAN gelabeld op een per ExpressRoute directe poort paar gefactureerd. Een C-code die wordt gebruikt op een peering moet uniek zijn in alle circuits en op het paar van de poort ExpressRoute Direct-peerings.

## <a name="workflow"></a>Werkstroom

![werkstroom](./media/expressroute-erdirect-about/workflow1.png)

## <a name="sla"></a>SLA

ExpressRoute Direct biedt de dezelfde SLA op ondernemingsniveau met redundante verbindingen actief/actief in het wereldwijde netwerk van Microsoft. ExpressRoute-infrastructuur is overbodig en connectiviteit in het wereldwijde netwerk van Microsoft is overbodig en diverse en kan worden geschaald dienovereenkomstig aan de klantvereisten. Tijdens de preview, zal er geen SLA en gelden alleen voor niet-productiewerkbelastingen.

## <a name="next-steps"></a>Volgende stappen

[ExpressRoute Direct configureren](expressroute-howto-erdirect.md)
