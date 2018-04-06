---
title: Veelgestelde vragen over prijzen voor Azure-netwerk Prestatiemeter | Microsoft Docs
description: Veelgestelde vragen - Prestatiemeter Azure-netwerk
services: monitoring-and-diagnostics
documentationcenter: na
author: agummadi
manager: cherylmc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2018
ms.author: ajaycode
ms.openlocfilehash: 84a3834416c7d93ecb4f51fea9c7baed8af85db0
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="pricing-changes-for-azure-network-performance-monitor"></a>Wijzigingen in de prijzen voor Azure-netwerk Prestatiemeter

We hebben geluisterd naar u en onlangs hebt toegevoegd een [prijzen van nieuwe ervaring](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/), bewaking voor verschillende services in Azure.

Dit document worden de prijscategorie wijzigingen die betrekking hebben op Azure vastgelegd [netwerk Prestatiemeter](https://docs.microsoft.com/azure/networking/network-monitoring-overview) (NPM), in een gemakkelijk te lezen van de vraag en antwoord-indeling.

Prestatiemeter netwerk bestaat uit drie onderdelen:
* [Prestatiemeter](https://docs.microsoft.com/azure/networking/network-monitoring-overview#performance-monitor)
* [Monitor voor eindpunt](https://docs.microsoft.com/azure/networking/network-monitoring-overview#service-endpoint-monitor) en
* [ExpressRoute Monitor](https://docs.microsoft.com/azure/networking/network-monitoring-overview#expressroute-monitor)

De onderstaande sectie wordt uitgelegd van de prijscategorie wijzigingen voor Prestatiemeter en ExpressRoute-Monitor de Monitor voor Service-eindpunt.

## <a name="performance-monitor-pm"></a>Prestatiemeter (PM)

**Hoe was gebruik van de Prestatiemeter in rekening gebracht in het oude schema?**

De facturering voor NPM is gebaseerd op het gebruik/verbruik uit twee onderdelen:
* Knooppunten: Alle synthetische transacties zijn afkomstig en eindigen op de knooppunten. Knooppunten zijn ook agents of MMA (Microsoft Management Agents) genoemd.
* Gegevens: De resultaten van de verschillende netwerktests in Log Analytics-bibliotheek worden opgeslagen.

Onder het oude model, is de factuur berekend op basis van het aantal knooppunten en de hoeveelheid gegevens die zijn gegenereerd. 

**Wat is het gebruik van de Prestatiemeter in rekening gebracht onder het nieuwe model?**

De functie van de Prestatiemeter in NPM wordt nu in rekening gebracht op een combinatie van: 

* subnet koppelingen bewaakt en
* Gegevensvolume

**Wat is er een koppeling subnet?**

Prestatiemeter bewaakt connectiviteit tussen twee of meer locaties op het netwerk.  De verbinding tussen een groep van knooppunten/agents op één subnet en een groep van knooppunten op een ander subnet is een subnet-koppeling genoemd.

**Ik heb twee subnetten (subnet A en B) en verschillende agents hebben in elk subnet.  Prestatiemeter bewaakt de verbinding tussen alle agents op subnet A en alle agents op subnet B.  Ik gefactureerd op basis van het aantal verbindingen tussen subnet?**

Nee. Alle verbindingen van een subnet naar subnet B zijn gegroepeerd in één subnet koppeling voor facturatie, en u wordt gefactureerd voor een enkele verbinding.  Prestatiemeter blijven bewaken connectiviteit tussen verschillende agents op elk subnet.

**Wat zijn de kosten voor het bewaken van een subnet koppeling?**

Raadpleeg het gedeelte [Ping Mesh](https://azure.microsoft.com/pricing/details/network-watcher/) voor de kosten van de bewaking van een koppeling met één subnet voor de hele maand.

**Wat zijn de kosten voor de gegevens die worden gegenereerd door Prestatiemeter?**

De kosten voor opname (gegevens uploaden met Log Analytics, verwerken en indexeren) is beschikbaar op de [pagina met prijzen](https://azure.microsoft.com/pricing/details/log-analytics/) voor logboekanalyse.  (Sectie: gegevensopname).

De kosten voor het bewaren van gegevens (dat wil zeggen, gegevens bewaard op de optie van de klant, na de eerste maand) is ook beschikbaar op de [pagina met prijzen](https://azure.microsoft.com/pricing/details/log-analytics/).  (Sectie: bewaren van gegevens).


## <a name="expressroute-monitor-erm"></a>ExpressRoute-Monitor (ERM)

**Wat zijn de kosten voor informatie over het gebruik van ExpressRoute-Monitor?**

Kosten voor ExpressRoute-Monitor wordt gefactureerd op basis van de hoeveelheid gegevens die zijn gegenereerd tijdens de bewaking.   Raadpleeg de vraag 'Wat zijn de kosten voor gegevens die zijn gegenereerd door Prestatiemeter?' voor meer informatie.

**Ik ERM gebruiken voor het bewaken van meerdere ExpressRoute-circuits. Ben ik in rekening gebracht op basis van het aantal circuits worden bewaakt?**

Er zijn niet in rekening gebracht op basis van beide het aantal circuits of het type van de peering (bijvoorbeeld persoonlijke peering, Microsoft-peering).  Er worden in rekening gebracht op het volume aan gegevens, zoals hierboven is uitgelegd.

**Wat is de hoeveelheid gegevens die zijn gegenereerd, bij de bewaking van één circuit?**

De hoeveelheid gegevens die worden gegenereerd per maand, wanneer een persoonlijke peering verbinding bewaking als volgt is:

|Percentiel      |Gegevens per maand (MB)|
| :---:          |           ---:|
|50<sup>th</sup> |            192|
|60<sup>th</sup> |            256|
|70<sup>th</sup> |            360|
|80<sup>th</sup> |            498|
|90<sup>th</sup> |            870|
|95<sup>th</sup> |           1560|


Klanten op het 50e percentiel betalen per de bovenstaande tabel 192 MB aan gegevens. USD $2.30/GB voor de eerste maand, de kosten voor het bewaken van een circuit is USD 0,43 (= 192 * 2.30 / 1024) voor de eerste maand.

**Wat zijn enkele van de redenen voor de verschillen in de hoeveelheid gegevens?**

Het volume van gegenereerde gegevens bewaking is afhankelijk van verschillende factoren, zoals de:
* aantal agents - de nauwkeurigheid van de fout isolatie toeneemt met een toename van het aantal agents
* aantal hops op het netwerk
* aantal paden tussen de bron en bestemming

Klanten op de hogere percentielen (in de bovenstaande tabel), bewaken meestal hun circuits vanuit verschillende vantage die op hun on-premises netwerk.  Meerdere agents worden ook geplaatst dieper in het netwerk, verder af van de serviceprovider edge-router. De agents worden vaak op verschillende sites van de gebruiker, filialen en rekken in datacenters geplaatst.

## <a name="service-endpoint-monitor-sepm"></a>Monitor voor service-eindpunt (SEPM)

**Wat zijn de kosten voor het gebruik van de Monitor voor Service-eindpunt?**

Kosten voor het gebruik van de Monitor voor Service-eindpunt worden berekend op basis van:
* aantal verbindingen
* volume van gegevens

**Wat is er een verbinding?**

Een verbinding is een test van bereikbaarheid naar één eindpunt (service-URL of de netwerkverbinding) vanaf één agent voor de hele maand. Bewaking van een verbinding aan bing.com van drie agents vormt bijvoorbeeld drie verbindingen.

**Wat zijn de kosten voor Service-eindpunt Monitor?**

- Raadpleeg de [verbinding bewaking](https://azure.microsoft.com/pricing/details/network-watcher/) sectie voor de kosten van de bewaking van een eindpunt voor de hele maand.
- De kosten voor gegevens is beschikbaar op de [pagina met prijzen](https://azure.microsoft.com/pricing/details/log-analytics/) voor logboekanalyse.  (Sectie: gegevensopname).