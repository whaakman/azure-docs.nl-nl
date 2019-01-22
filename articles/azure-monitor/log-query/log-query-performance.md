---
title: Efficiënte logboeken-query's schrijven in Azure Monitor | Microsoft Docs
description: Verwijzingen naar resources voor het leren over het schrijven van query's in Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/17/2019
ms.author: bwren
ms.openlocfilehash: 25d6b582ed4d4e24df3841f4191471296e25abd8
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54436329"
---
# <a name="writing-efficient-log-queries-in-azure-monitor"></a>Efficiënte logboeken-query's schrijven in Azure Monitor
In dit artikel bevat aanbevelingen voor het schrijven van efficiënte logboeken-query's in Azure Monitor. Met behulp van deze strategieën, kunt u ervoor dat uw query's wordt uitgevoerd, snel en met minimale overheard.

## <a name="scope-your-query"></a>Het bereik van uw query
Met uw query verwerken van meer gegevens dan u nodig hebt, kan leiden tot een langlopende query en vaak leiden tot te veel gegevens in de resultaten voor het analyseren van effectief. In uitzonderlijke gevallen kan de query ook de time-out en mislukken.

### <a name="specify-your-data-source"></a>Geef uw gegevensbron
De eerste stap bij het schrijven van een doeltreffende query is het bereik met de vereiste gegevensbronnen te beperken. Het opgeven van een tabel is altijd voorkeur via met een breed tekstzoekopdrachten, zoals `search *`. Om een specifieke tabel, start u uw query met de naam van de tabel zoals in het volgende:

``` Kusto
requests | ...
```

U kunt [zoeken](/azure/kusto/query/searchoperator) om te zoeken naar een waarde in meerdere kolommen in specifieke tabellen met behulp van een query als volgt:

``` Kusto
search in (exceptions) "The server was not found"

search in (exceptions, customEvents) "timeout"
```

Gebruik [union](/azure/kusto/query/unionoperator) query uitvoeren op meerdere tabellen als volgt:

``` Kusto
union requests, traces | ...
```

### <a name="specify-a-time-range"></a>Een tijdsperiode opgeven
U moet ook beperken dat uw query en het tijdsbereik van de gegevens die u nodig hebt. Standaard bevat de query gegevens die zijn verzameld in de afgelopen 24 uur. Kunt u deze optie in de [tijd bereik selector](get-started-portal.md#select-a-time-range) of deze expliciet toevoegen aan uw query. Het is raadzaam het tijdfilter direct na de naam van de tabel toevoegen zodat de rest van de query alleen gegevens binnen dat bereik verwerkt:

``` Kusto
requests | where timestamp > ago(1h)

requests | where timestamp between (ago(1h) .. ago(30m))
```
   
### <a name="get-only-the-latest-records"></a>Alleen de meest recente records ophalen

Om terug te keren alleen de meest recente records, gebruikt u de *boven* operator zoals in de volgende query de laatste 10 records retourneert die zijn vastgelegd in de *traceringen* tabel:

``` Kusto
traces | top 10 by timestamp
```

   
### <a name="filter-records"></a>Records filteren
Als u wilt controleren alleen logboeken die overeenkomen met een bepaalde voorwaarde, gebruikt u de *waar* operator zoals in de volgende query alleen de records waarin retourneert de _severityLevel_ waarde hoger is dan 0:

``` Kusto
traces | where severityLevel > 0
```



## <a name="string-comparisons"></a>Tekenreeksvergelijkingen
Wanneer [evalueren tekenreeksen](/azure/kusto/query/datatypes-string-operators), moet u meestal gebruiken `has` in plaats van `contains` bij het zoeken naar volledige tokens. `has` is efficiënter omdat deze geen opgezocht voor subtekenreeksen.

## <a name="returned-columns"></a>Geretourneerde kolommen

Gebruik [project](/azure/kusto/query/projectoperator) om te beperken van de set kolommen wordt verwerkt met alleen die u nodig hebt:

``` Kusto
traces 
| project timestamp, severityLevel, client_City 
| ...
```

U kunt gebruiken [uitbreiden](/azure/kusto/query/extendoperator) berekent waarden en uw eigen kolommen maken, deze wordt doorgaans efficiënter om te filteren op een tabelkolom.

Bijvoorbeeld, de eerste query hieronder die kunt u filteren op _bewerking\_naam_ zou zijn efficiënter dan het tweede die maakt u een nieuwe _abonnement_ kolom en filters op het:

``` Kusto
customEvents 
| where split(operation_Name, "/")[2] == "acb"

customEvents 
| extend subscription = split(operation_Name, "/")[2] 
| where subscription == "acb"
```

## <a name="using-joins"></a>Met behulp van joins
Wanneer u de [join](/azure/kusto/query/joinoperator) -operator, kiest u de tabel met minder rijen worden aan de linkerkant van de query.


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over best practices query [query uitvoeren op aanbevolen procedures](/azure/kusto/query/best-practices).