---
title: bestand opnemen
description: bestand opnemen
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 02/07/2019
ms.author: robb
ms.custom: include file
ms.openlocfilehash: 21e2d3f75028d239175effa7a3608cc18ccfc95c
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305272"
---
**Verzameling gegevensvolume en retentie** 

| Laag | Beperken per dag | Bewaartijd van gegevens | Opmerking |
|:---|:---|:---|:---|
| Huidige Per GB-prijscategorie<br>(Zie April 2018) | Geen limiet | 30 - 730 dagen | Bewaren van gegevens langer dan 31 dagen is beschikbaar voor extra kosten in rekening gebracht. Meer informatie over prijzen van Azure Monitor. |
| Oude gratis laag<br>(Zie April 2016) | 500 MB | 7 dagen | Als uw werkruimte het 500 MB per daglimiet is bereikt, worden de opname van gegevens gestopt en wordt aan het begin van de volgende dag hervat. Een dag is gebaseerd op UTC. Gegevens die zijn verzameld door Azure Security Center wordt niet opgenomen in deze 500 MB per daglimiet en moeten worden verzameld boven deze limiet blijven.  |
| Verouderde zelfstandige Per GB-laag<br>(Zie April 2016) | Geen limiet | 30-730 dagen | Bewaren van gegevens langer dan 31 dagen is beschikbaar voor extra kosten in rekening gebracht. Meer informatie over prijzen van Azure Monitor. |
| Verouderde Per knooppunt (OMS)<br>(Zie April 2016) | Geen limiet | 30-730 dagen | Bewaren van gegevens langer dan 31 dagen is beschikbaar voor extra kosten in rekening gebracht. Meer informatie over prijzen van Azure Monitor. |
| Verouderde Standard-laag | Geen limiet | 30 dagen  | Retentie kan niet worden aangepast |
| Verouderde Premium-laag | Geen limiet | 365 dagen  | Retentie kan niet worden aangepast |

**Het aantal werkruimten per abonnement.**

| Prijscategorie    | Limiet voor werkruimte | Opmerkingen
|:---|:---|:---|
| Gratis laag  | 10 | Deze limiet kan niet worden verhoogd. |
| Alle andere lagen | Geen limiet | U bent beperkt door het aantal resources binnen een resourcegroep en het aantal resourcegroepen per abonnement. |

**Azure Portal**

| Category | Limits | Opmerkingen |
|:---|:---|:---|
| Maximum aantal records geretourneerd door een logboekquery | 10.000 | Minder resultaten met behulp van querybereik, tijdsbereik en filters in de query. |


**Gegevensverzamelaar-API**

| Category | Limits | Opmerkingen |
|:---|:---|:---|
| Maximale grootte voor een enkel bericht | 30 MB | Grotere volumes splitsen in meerdere berichten. |
| Maximumgrootte voor veldwaarden  | 32 KB | Velden die langer zijn dan 32 KB worden afgebroken. |

**Zoeken-API**

| Category | Limits | Opmerkingen |
|:---|:---|:---|
| Maximum aantal records geretourneerd voor niet-samengevoegde gegevens | 5,000 | |
| Maximum aantal records voor samengevoegde gegevens | 500,000 | Samengevoegde gegevens is een zoekopdracht met de `summarize` opdracht. |
| Maximale grootte van gegevens die worden geretourneerd | 64,000,000 bytes (~ 61 MiB)| |
| Maximale query uitgevoerd tijd | 10 minuten | Zie [time-outs](https://dev.loganalytics.io/documentation/Using-the-API/Timeouts) voor meer informatie.  |
| Maximale aantal aanvragen | 200 aanvragen per 30 seconden per AAD gebruiker of -client IP-adres | Zie [frequentielimieten](https://dev.loganalytics.io/documentation/Using-the-API/Limits) voor meer informatie. |

**Algemene werkruimte limieten**

| Category | Limits | Opmerkingen |
|:---|:---|:---|
| Maximum aantal kolommen in een tabel         | 500 | |
| Maximum aantal tekens in voor de naam van kolom | 500 | |
| Regio's op capaciteit | US - west-centraal | U kan niet op dit moment een nieuwe werkruimte maken in deze regio, omdat deze capaciteitslimiet bereikt tijdelijke is. Deze limiet is gepland om te worden verholpen door de einde van September 2019. |
| Gegevens exporteren | Momenteel niet beschikbaar | Azure-functie of logische App gebruiken om te aggregeren en exporteren van gegevens. | 

>[!NOTE]
>Afhankelijk van hoe lang u hebt gebruikt Log Analytics, wellicht u toegang tot de oudere Prijscategorieën. Meer informatie over [Log Analytics oudere Prijscategorieën](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#legacy-pricing-tiers). 