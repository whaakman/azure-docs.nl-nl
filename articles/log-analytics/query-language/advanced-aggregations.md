---
title: Geavanceerde aggregaties in Azure Log Analytics-query's | Microsoft Docs
description: Beschrijft een aantal van de meer geavanceerde aggregatieopties beschikbaar voor Log Analytics-query's.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/16/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 4f2d49233a6eb92f567d4265210fcab394aa6461
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "40190127"
---
# <a name="advanced-aggregations-in-log-analytics-queries"></a>Geavanceerde aggregaties in Log Analytics-query 's

> [!NOTE]
> U moet voltooien [aggregaties in Log Analytics-query's](./aggregations.md) voordat het voltooien van deze les gaat uitvoeren.

In dit artikel worden enkele van de meer geavanceerde aggregatieopties beschikbaar voor Log Analytics-query's beschreven.

## <a name="generating-lists-and-sets"></a>Lijsten en sets genereren
U kunt `makelist` pivot gegevens door de volgorde van waarden in een bepaalde kolom. U wilt bijvoorbeeld de meest voorkomende volgorde gebeurtenissen plaatsvinden op uw virtuele machines verkennen. U kunt de gegevens door de volgorde van EventIDs op elke computer in feite draaien. 

```OQL
Event
| where TimeGenerated > ago(12h)
| order by TimeGenerated desc
| summarize makelist(EventID) by Computer
```
|Computer|list_EventID|
|---|---|
| Computer1 | [704,701,1501,1500,1085,704,704,701] |
| Computer2 | [326,105,302,301,300,102] |
| ... | ... |

`makelist` genereert een lijst in de volgorde waarin de gegevens erin is doorgegeven. Gebruiken om gebeurtenissen van oudste naar nieuwste sorteren, `asc` in de instructie order in plaats van `desc`. 

Het is ook handig voor het maken van een lijst met alleen unieke waarden. Dit heet een _ingesteld_ en kan worden gegenereerd met `makeset`:

```OQL
Event
| where TimeGenerated > ago(12h)
| order by TimeGenerated desc
| summarize makeset(EventID) by Computer
```
|Computer|list_EventID|
|---|---|
| Computer1 | [704,701,1501,1500,1085] |
| Computer2 | [326,105,302,301,300,102] |
| ... | ... |

Zoals `makelist`, `makeset` ook werkt met gegevens geordend en genereert de matrices op basis van orde van grootte de rijen die erin worden doorgegeven.

## <a name="expanding-lists"></a>Lijsten uitvouwen
De inverse werking van `makelist` of `makeset` is `mvexpand`, die een lijst met waarden voor het scheiden van rijen wordt uitgevouwen. Deze kunt uitbreiden naar een willekeurig aantal dynamische kolommen, JSON- en matrix. U kunt bijvoorbeeld controleren de *Heartbeat* tabel voor oplossingen voor het verzenden van gegevens van computers die in het afgelopen uur geen heartbeat heeft verzonden:

```OQL
Heartbeat
| where TimeGenerated > ago(1h)
| project Computer, Solutions
```
| Computer | Oplossingen | 
|--------------|----------------------|
| Computer1 | "beveiliging", "updates", 'voor wijzigingen bijhouden' |
| Computer2 | "beveiliging", "updates" |
| computer3 | 'malware', 'voor wijzigingen bijhouden' |
| ... | ... | ... |

Gebruik `mvexpand` om elke waarde in een afzonderlijke rij in plaats van een door komma's gescheiden lijst weer te geven:

Heartbeat | waar TimeGenerated > ago(1h) | Computer-project, splitsen (oplossingen, ',') | mvexpand oplossingen
```
| Computer | Solutions | 
|--------------|----------------------|
| computer1 | "security" |
| computer1 | "updates" |
| computer1 | "changeTracking" |
| computer2 | "security" |
| computer2 | "updates" |
| computer3 | "antiMalware" |
| computer3 | "changeTracking" |
| ... | ... | ... |
```

U kunt vervolgens `makelist` opnieuw aan de groep items samen en deze keer dat de lijst met computers per oplossing weergegeven:

```OQL
Heartbeat
| where TimeGenerated > ago(1h)
| project Computer, split(Solutions, ",")
| mvexpand Solutions
| summarize makelist(Computer) by tostring(Solutions) 
```
|Oplossingen | list_Computer |
|--------------|----------------------|
| "beveiliging" | ["computer1", "computer2"] |
| "updates" | ["computer1", "computer2"] |
| 'voor wijzigingen bijhouden' | ["computer1", "computer3"] |
| "antiMalware" | ["computer3"] |
| ... | ... |

## <a name="handling-missing-bins"></a>Afhandeling van ontbrekende opslaglocaties
Een handige toepassing van `mvexpand` hoeft in te vullen standaardwaarden voor ontbrekende opslaglocaties is. Stel bijvoorbeeld dat u zoekt de uptime van een bepaalde machine door het verkennen van de heartbeat. U ook wilt zien van de bron van de heartbeat die zich in de _categorie_ kolom. Normaal gesproken, gebruiken we een eenvoudige instructie als volgt samengevat:

```OQL
Heartbeat
| where TimeGenerated > ago(12h)
| summarize count() by Category, bin(TimeGenerated, 1h)
```
| Categorie | TimeGenerated | count_ |
|--------------|----------------------|--------|
| Agent toewijzen | 2017-06-06T17:00:00Z | 15 |
| Agent toewijzen | 2017-06-06T18:00:00Z | 60 |
| Agent toewijzen | 2017-06-06T20:00:00Z | 55 |
| Agent toewijzen | 2017-06-06T21:00:00Z | 57 |
| Agent toewijzen | 2017-06-06T22:00:00Z | 60 |
| ... | ... | ... |

In deze resulteert echter de bucket die zijn gekoppeld aan ' 2017-06-06T19:00:00Z ' ontbreekt omdat er geen heartbeat-gegevens voor dat uur. Gebruik de `make-series` functie een standaardwaarde toewijzen aan lege buckets. Hiermee wordt een rij voor elke categorie met twee extra matrix kolommen, één voor waarden en één voor de overeenkomende tijd buckets gegenereerd:

```OQL
Heartbeat
| make-series count() default=0 on TimeGenerated in range(ago(1d), now(), 1h) by Category 
```

| Categorie | count_ | TimeGenerated |
|---|---|---|
| Agent toewijzen | [15,60,0,55,60,57,60,...] | ["2017-06-06T17:00:00.0000000Z","2017-06-06T18:00:00.0000000Z","2017-06-06T19:00:00.0000000Z","2017-06-06T20:00:00.0000000Z","2017-06-06T21:00:00.0000000Z',...] |
| ... | ... | ... |

Het derde element van de *count_* matrix 0 is zoals verwacht en er is een overeenkomende tijdstempel van ' 2017-06-06T19:00:00.0000000Z ' in de _TimeGenerated_ matrix. Deze matrix-indeling is moeilijk te lezen is echter. Gebruik `mvexpand` uit te breiden de matrices en dezelfde indeling als die worden gegenereerd door de uitvoer produceren `summarize`:

```OQL
Heartbeat
| make-series count() default=0 on TimeGenerated in range(ago(1d), now(), 1h) by Category 
| mvexpand TimeGenerated, count_
| project Category, TimeGenerated, count_
```
| Categorie | TimeGenerated | count_ |
|--------------|----------------------|--------|
| Agent toewijzen | 2017-06-06T17:00:00Z | 15 |
| Agent toewijzen | 2017-06-06T18:00:00Z | 60 |
| Agent toewijzen | 2017-06-06T19:00:00Z | 0 |
| Agent toewijzen | 2017-06-06T20:00:00Z | 55 |
| Agent toewijzen | 2017-06-06T21:00:00Z | 57 |
| Agent toewijzen | 2017-06-06T22:00:00Z | 60 |
| ... | ... | ... |



## <a name="narrowing-results-to-a-set-of-elements-let-makeset-toscalar-in"></a>Resultaten in een set met elementen technologiegebied: `let`, `makeset`, `toscalar`, `in`
Een veelvoorkomend scenario is de namen van bepaalde specifieke entiteiten op basis van een set criteria selecteren en vervolgens een andere gegevensset tot die set met entiteiten filteren. U kunt bijvoorbeeld computers waarvan bekend is dat de ontbrekende updates gevonden en IP-adressen die deze computers te identificeren:


```OQL
let ComputersNeedingUpdate = toscalar(
    Update
    | summarize makeset(Computer)
    | project set_Computer
);
WindowsFirewall
| where Computer in (ComputersNeedingUpdate)
```

## <a name="next-steps"></a>Volgende stappen

Zie andere lessen voor het gebruik van de querytaal van Log Analytics:

- [Bewerkingen op tekenreeksen uitvoeren](string-operations.md)
- [Datum- en tijdbewerkingen](datetime-operations.md)
- [Aggregatiefuncties](aggregations.md)
- [Geavanceerde aggregaties](advanced-aggregations.md)
- [JSON en gegevensstructuren](json-data-structures.md)
- [Joins](joins.md)
- [Grafieken](charts.md)