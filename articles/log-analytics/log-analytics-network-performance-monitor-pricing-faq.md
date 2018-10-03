---
title: Veelgestelde vragen over prijzen voor Azure Network Performance Monitor | Microsoft Docs
description: Veelgestelde vragen - Azure Network Performance Monitor
services: monitoring-and-diagnostics
documentationcenter: na
author: agummadi
manager: cherylmc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2018
ms.author: ajaycode
ms.component: ''
ms.openlocfilehash: 9e9fdfce9dbb165227f88e9f72bc219dce8f3307
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48043377"
---
# <a name="pricing-changes-for-azure-network-performance-monitor"></a>Prijswijzigingen voor Azure Network Performance Monitor

We hebben uw feedback geluisterd en onlangs geïntroduceerd een [nieuwe prijsmodel ervaring](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) voor verschillende bewakingsservices in Azure. In dit artikel worden de prijzen wijzigingen met betrekking tot Azure vastgelegd [Network Performance Monitor](https://docs.microsoft.com/azure/networking/network-monitoring-overview) (NPM) in een gemakkelijk leesbare vraag en antwoord-indeling.

Network Performance Monitor bestaat uit drie onderdelen:
* [Prestatiemeter](https://docs.microsoft.com/azure/networking/network-monitoring-overview#performance-monitor)
* [Service-Eindpuntbewaking](https://docs.microsoft.com/azure/networking/network-monitoring-overview#service-endpoint-monitor)
* [ExpressRoute-bewaking](https://docs.microsoft.com/azure/networking/network-monitoring-overview#expressroute-monitor)

De volgende secties worden de prijswijzigingen voor de NPM-onderdelen.

## <a name="performance-monitor"></a>Prestatiemeter

**Hoe is het gebruik van de Prestatiemeter in rekening gebracht in het oude model?**

De facturering voor NPM is gebaseerd op het gebruik en het verbruik van twee onderdelen:
* **Knooppunten**: alle synthetische transacties afkomstig zijn en eindigen op de knooppunten. Knooppunten worden ook aangeduid als agents of Microsoft Beheeragents.
* **Gegevens**: de resultaten van de verschillende netwerktests zijn opgeslagen in de Azure Log Analytics-opslagplaats.

De factuur is berekend op basis van het aantal knooppunten en het volume van gegenereerde gegevens onder het oude model. 

**Hoe wordt het gebruik van de Prestatiemeter in rekening gebracht onder het nieuwe model?**

De Prestatiemeter-functie in NPM is nu in rekening gebracht op basis van een combinatie van: 

* Bewaakt subnetkoppelingen
* Gegevensvolume

**Wat is een subnet koppelen?**

Performance Monitor controleert de connectiviteit tussen twee of meer locaties op het netwerk. De verbinding tussen een groep knooppunten of agents op één subnet en een groep van knooppunten op een ander subnet, wordt een subnet-koppeling genoemd.

**Ik heb twee subnetten (A en B), en ik heb verschillende agents op elk subnet. Performance Monitor bewaakt de connectiviteit van alle agents in een subnet met alle agents op subnet B. Moet ik betalen op basis van het aantal verbindingen tussen subnet?**

Nee. Voor factureringsdoeleinden, zijn alle verbindingen van een subnet naar subnet B gegroepeerd tot één subnet-koppeling. U wordt gefactureerd voor een enkele verbinding. Prestatiemeter blijft Controleer de connectiviteit tussen verschillende agents op elk subnet.

**Wat zijn de kosten voor het bewaken van een subnet koppelen?**

Zie voor de kosten van de bewaking van een koppeling met één subnet voor de hele maand, de [Ping Mesh](https://azure.microsoft.com/pricing/details/network-watcher/) sectie.

**Wat zijn de kosten voor gegevens die in Prestatiemeter gegenereerd?**

De kosten voor gegevensopname (het uploaden van gegevens naar Log Analytics, verwerking en indexeren) is beschikbaar op de [pagina met prijzen](https://azure.microsoft.com/pricing/details/log-analytics/) voor Log Analytics, in de sectie opname van gegevens. De kosten voor het bewaren van gegevens (dat wil zeggen gegevens bewaard in de optie van de klant, na de eerste maand) is ook beschikbaar op de [pagina met prijzen](https://azure.microsoft.com/pricing/details/log-analytics/), in de sectie voor het bewaren van gegevens.


## <a name="expressroute-monitor"></a>ExpressRoute-bewaking

**Wat zijn de kosten voor het gebruik van ExpressRoute Monitor?**

Kosten voor ExpressRoute-bewaking worden in rekening gebracht op basis van de hoeveelheid gegevens die zijn gegenereerd tijdens de bewaking. Zie voor meer informatie "Wat zijn de kosten voor gegevens die in Prestatiemeter gegenereerd?"

**Ik gebruik van ExpressRoute-Monitor voor het bewaken van meerdere ExpressRoute-circuits. Moet ik dan betalen op basis van het aantal circuits die worden bewaakt?**

Er zijn niet in rekening gebracht op basis van een van beide het nummer van circuits of het type van de peering (bijvoorbeeld persoonlijke peering, Microsoft-peering). U betaalt op basis van het volume van gegevens, zoals eerder uitgelegd.

**Wat is de hoeveelheid gegevens die worden gegenereerd wanneer ExpressRoute een één-circuit bewaakt?**

De hoeveelheid gegevens die worden gegenereerd per maand, ExpressRoute gecontroleerd wanneer een privé-peering verbinding, is als volgt:

|Percentiel      |Gegevens/maand (MB)|
| :---:          |           ---:|
|50<sup>th</sup> |            192|
|60<sup>th</sup> |            256|
|70<sup>th</sup> |            360|
|80<sup>th</sup> |            498|
|90<sup>th</sup> |            870|
|95<sup>th</sup> |           1560|


Klanten in het 50e percentiel betalen op basis van deze tabel 192 MB aan gegevens. USD $2.30/GB voor de eerste maand, de kosten in rekening gebracht voor het bewaken van een circuit is USD $0,43 (= 192 * 2.30 / 1024).

**Wat zijn enkele redenen voor variaties in het volume van gegevens?**

Het volume van de bewaking van de gegevens die zijn gegenereerd is afhankelijk van diverse factoren, zoals de:
* Het aantal agents. De nauwkeurigheid van foutisolatie verhoogd met een toename van het aantal agents.
* Het aantal hops in het netwerk.
* Het aantal paden tussen de bron en bestemming.

Klanten op de hogere percentielen (in de voorgaande tabel) meestal de circuits van verschillende vantage punten op hun on-premises netwerk te controleren. Meerdere agents worden ook dieper geplaatst in het netwerk, verder de serviceprovider edge router. De agents worden vaak geplaatst op verschillende sites van de gebruiker, vertakkingen en rekken in datacenters.

## <a name="service-endpoint-monitor"></a>Service-Eindpuntbewaking

**Wat zijn de kosten voor het gebruik van Service-Eindpuntbewaking?**

Kosten voor het gebruik van Service-Eindpuntbewaking worden berekend op basis van:
* Aantal verbindingen
* Volume van gegevens

**Wat is een verbinding?**

Een verbinding is een test van bereikbaarheid naar een eindpunt (URL of network service) van een afzonderlijke agent voor de hele maand. Bewaking van een verbinding op bing.com met drie agenten vormt bijvoorbeeld drie verbindingen.

**Wat zijn de kosten voor Service-Eindpuntbewaking?**

Raadpleeg de [Verbindingscontrole](https://azure.microsoft.com/pricing/details/network-watcher/) sectie voor de kosten van de bewaking van een eindpunt voor de hele maand. De kosten voor gegevens is beschikbaar op de [pagina met prijzen](https://azure.microsoft.com/pricing/details/log-analytics/) voor Log Analytics, in de sectie opname van gegevens.

## <a name="references"></a>Verwijzingen

[Meld u Analytics Veelgestelde vragen over tarieven](https://azure.microsoft.com/pricing/details/log-analytics/): veelgestelde vragen over de de sectie bevat informatie over de gratis laag, per knooppunt en andere informatie over prijsinformatie.

