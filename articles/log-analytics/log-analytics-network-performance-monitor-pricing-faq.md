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
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2018
ms.author: ajaycode
ms.component: na
ms.openlocfilehash: 96eb26d6a4faf8c6907d23ebf21f2446722c913b
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127090"
---
# <a name="pricing-changes-for-azure-network-performance-monitor"></a>Prijzen van wijzigingen voor de Prestatiemeter voor Azure-netwerk

We hebben geluisterd naar uw feedback en onlangs geïntroduceerd een [prijzen van nieuwe ervaring](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) bewaking voor verschillende services in Azure. In dit artikel worden de prijscategorie wijzigingen die betrekking hebben op Azure vastgelegd [netwerk Prestatiemeter](https://docs.microsoft.com/azure/networking/network-monitoring-overview) (NPM) in een gemakkelijk leesbare vraag en antwoord-indeling.

Prestatiemeter netwerk bestaat uit drie onderdelen:
* [Prestatiemeter](https://docs.microsoft.com/azure/networking/network-monitoring-overview#performance-monitor)
* [Monitor voor service-eindpunt](https://docs.microsoft.com/azure/networking/network-monitoring-overview#service-endpoint-monitor)
* [ExpressRoute-Monitor](https://docs.microsoft.com/azure/networking/network-monitoring-overview#expressroute-monitor)

De volgende secties worden de prijscategorie wijzigingen voor de NPM-onderdelen.

## <a name="performance-monitor"></a>Prestatiemeter

**Hoe was gebruik van de Prestatiemeter in rekening gebracht in het oude model?**

De facturering voor NPM is gebaseerd op het gebruik en het verbruik van twee onderdelen:
* **Knooppunten**: alle synthetische transacties afkomstig zijn en eindigen op de knooppunten. Knooppunten worden ook aangeduid als agents of Microsoft Beheeragents.
* **Gegevens**: de resultaten van de verschillende netwerktests worden opgeslagen in de Azure-logboekanalyse-opslagplaats.

Onder het oude model, is de factuur berekend op basis van het aantal knooppunten en de hoeveelheid gegevens die zijn gegenereerd. 

**Hoe wordt informatie over het gebruik van de Prestatiemeter in rekening gebracht onder het nieuwe model?**

De functie van de Prestatiemeter in NPM wordt nu in rekening gebracht op basis van een combinatie van: 

* Subnet koppelingen bewaakt
* Gegevensvolume

**Wat is er een koppeling subnet?**

Prestatiemeter bewaakt connectiviteit tussen twee of meer locaties op het netwerk. De verbinding tussen een groep knooppunten of agents op één subnet en een groep van knooppunten op een ander subnet wordt een subnet-koppeling genoemd.

**Ik heb twee subnetten (A en B) en er verschillende agents in elk subnet. Prestatiemeter bewaakt de verbinding tussen alle agents op subnet A en alle agents op subnet B. Ik gefactureerd op basis van het aantal verbindingen tussen subnet?**

Nee. Voor facturatie, worden alle verbindingen van subnet A naar B subnet gegroepeerd in één subnet-koppeling. U wordt gefactureerd voor een enkele verbinding. Prestatiemeter blijft connectiviteit tussen verschillende agents op elk subnet bewaken.

**Wat zijn de kosten voor het bewaken van een subnet koppeling?**

Zie voor de kosten van de bewaking van een koppeling met één subnet voor de hele maand, de [Ping Mesh](https://azure.microsoft.com/pricing/details/network-watcher/) sectie.

**Wat zijn de kosten voor gegevens die in Prestatiemeter gegenereerd?**

De kosten voor opname (gegevens uploaden met Log Analytics, verwerken en indexeren) beschikbaar is op de [pagina met prijzen](https://azure.microsoft.com/pricing/details/log-analytics/) voor logboekanalyse in de sectie gegevensopname. De kosten voor het bewaren van gegevens (dat wil zeggen, gegevens bewaard op de optie van de klant, na de eerste maand) is ook beschikbaar op de [pagina met prijzen](https://azure.microsoft.com/pricing/details/log-analytics/), in de sectie bewaren van gegevens.


## <a name="expressroute-monitor"></a>ExpressRoute-bewaking

**Wat zijn de kosten voor informatie over het gebruik van ExpressRoute-Monitor?**

Kosten voor ExpressRoute-Monitor wordt gefactureerd op basis van de hoeveelheid gegevens die zijn gegenereerd tijdens de bewaking. Zie voor meer informatie 'Wat zijn de kosten voor gegevens die in Prestatiemeter gegenereerd?'

**Ik gebruik ExpressRoute-Monitor voor het bewaken van meerdere ExpressRoute-circuits. Ben ik in rekening gebracht op basis van het aantal circuits worden bewaakt?**

Er zijn niet in rekening gebracht op basis van beide het aantal circuits of het type van de peering (bijvoorbeeld persoonlijke peering, Microsoft-peering). Er worden in rekening gebracht op basis van het volume aan gegevens, zoals eerder beschreven.

**Wat is de hoeveelheid gegevens die worden gegenereerd wanneer ExpressRoute één circuit bewaakt?**

De hoeveelheid gegevens die worden gegenereerd per maand, ExpressRoute wanneer een persoonlijke peering verbinding bewaakt, is als volgt:

|Percentiel      |Gegevens per maand (MB)|
| :---:          |           ---:|
|50<sup>th</sup> |            192|
|60<sup>th</sup> |            256|
|70<sup>th</sup> |            360|
|80<sup>th</sup> |            498|
|90<sup>th</sup> |            870|
|95<sup>th</sup> |           1560|


Volgens deze tabel Klanten op het 50e percentiel betaalt voor 192 MB aan gegevens. USD $2.30/GB voor de eerste maand, de kosten voor het bewaken van een circuit is USD $0,43 (= 192 * 2.30 / 1024).

**Wat zijn enkele redenen voor variaties in de hoeveelheid gegevens?**

Het volume van gegenereerde gegevens bewaking is afhankelijk van verschillende factoren, zoals de:
* Aantal agents. De nauwkeurigheid van de fout isolatie verhoogt met een toename van het aantal agents.
* Het aantal hops op het netwerk.
* Het aantal paden tussen de bron en bestemming.

Klanten op de hogere percentielen (in de voorgaande tabel) meestal hun circuits vanuit verschillende vantage die op hun on-premises netwerk te bewaken. Meerdere agents worden ook geplaatst dieper in het netwerk, verder af van de serviceprovider edge-router. De agents worden vaak op verschillende sites van de gebruiker, filialen en rekken in datacenters geplaatst.

## <a name="service-endpoint-monitor"></a>Monitor voor service-eindpunt

**Wat zijn de kosten voor het gebruik van de Monitor voor Service-eindpunt?**

Kosten voor het gebruik van de Monitor voor Service-eindpunt worden berekend op basis van:
* Aantal verbindingen
* Volume van gegevens

**Wat is er een verbinding?**

Een verbinding is een test van bereikbaarheid naar één eindpunt (service-URL of de netwerkverbinding) vanaf één agent voor de hele maand. Bewaking van een verbinding aan bing.com van drie agents vormt bijvoorbeeld drie verbindingen.

**Wat zijn de kosten voor Service-eindpunt Monitor?**

Raadpleeg de [verbinding bewaking](https://azure.microsoft.com/pricing/details/network-watcher/) sectie voor de kosten van de bewaking van een eindpunt voor de hele maand. De kosten voor gegevens is beschikbaar op de [pagina met prijzen](https://azure.microsoft.com/pricing/details/log-analytics/) voor logboekanalyse in de sectie gegevensopname.

## <a name="references"></a>Verwijzingen

[Meld u Analytics prijzen Veelgestelde vragen over](https://azure.microsoft.com/pricing/details/log-analytics/): veelgestelde vragen over de de sectie bevat informatie over de gratis laag, per knooppunt prijzen en andere prijsinformatie.

