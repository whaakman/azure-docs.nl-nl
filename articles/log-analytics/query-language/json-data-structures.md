---
title: Werken met tekenreeksen in Azure Log Analytics-query's | Microsoft Docs
description: Dit artikel bevat een zelfstudie voor het gebruik van de Analytics-portal voor het schrijven van query's in Log Analytics.
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
ms.openlocfilehash: f027754f26a9063aa5faa548fd01576624811005
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "40190121"
---
# <a name="working-with-json-and-data-structures-in-log-analytics-queries"></a>Werken met JSON en gegevensstructuren in Log Analytics-query 's

> [!NOTE]
> U moet voltooien [aan de slag met de Analytics-portal](get-started-analytics-portal.md) en [aan de slag met query's](get-started-queries.md) voordat het voltooien van deze les gaat uitvoeren.


Geneste objecten zijn de objecten die andere objecten in een matrix of een overzicht van sleutel-waardeparen bevatten. Deze objecten worden weergegeven als JSON-tekenreeksen. Dit artikel wordt beschreven hoe JSON wordt gebruikt om gegevens op te halen en analyseren van geneste objecten.

## <a name="working-with-json-strings"></a>Werken met JSON-tekenreeksen
Gebruik `extractjson` voor toegang tot een specifieke JSON-element in een bekende pad. Deze functie moet een padexpressie die gebruikmaakt van de volgende conventies.

- _$_ om te verwijzen naar de hoofdmap
- Gebruik de notatie haakje of punt om te verwijzen naar indexen en elementen, zoals wordt geïllustreerd in de volgende voorbeelden.


Gebruik haakjes voor indexen en punten als scheidingsteken tussen elementen:

```OQL
let hosts_report='{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}';
print hosts_report
| extend status = extractjson("$.hosts[0].status", hosts_report)
```

Dit is hetzelfde resultaat met behulp van alleen de vierkante haken notatie:

```OQL
let hosts_report='{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}';
print hosts_report 
| extend status = extractjson("$['hosts'][0]['status']", hosts_report)
```

Als er slechts één element, kunt u alleen de puntnotatie:

```OQL
let hosts_report='{"location":"North_DC", "status":"running", "rate":5}';
print hosts_report 
| extend status = hosts_report.status
```


## <a name="working-with-objects"></a>Met objecten werken

### <a name="parsejson"></a>parsejson
Voor toegang tot meerdere elementen in uw json-structuur, is het eenvoudiger om deze te openen als een dynamische-object. Gebruik `parsejson` tekst gegevens naar een dynamische-object. Na de conversie naar een dynamisch type, kunnen extra functies worden gebruikt om de gegevens te analyseren.

```OQL
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| extend status0=hosts_object.hosts[0].status, rate1=hosts_object.hosts[1].rate
```



### <a name="arraylength"></a>arraylength
Gebruik `arraylength` telt het aantal elementen in een matrix:

```OQL
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| extend hosts_num=arraylength(hosts_object.hosts)
```

### <a name="mvexpand"></a>mvexpand
Gebruik `mvexpand` om de eigenschappen van een object in afzonderlijke rijen.

```OQL
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| mvexpand hosts_object.hosts[0]
```

![mvexpand](media/json-data-structures/mvexpand.png)

### <a name="buildschema"></a>buildschema
Gebruik `buildschema` om op te halen van het schema dat alle waarden van een object admits:

```OQL
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| summarize buildschema(hosts_object)
```

De uitvoer is een schema in JSON-indeling:
```json
{
    "hosts":
    {
        "indexer":
        {
            "location": "string",
            "rate": "int",
            "status": "string"
        }
    }
}
```
Deze uitvoer worden de namen van de velden van het object en de overeenkomende gegevenstypen beschreven. 

Geneste objecten mogelijk verschillende schema's, zoals in het volgende voorbeeld:

```OQL
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"status":"stopped", "rate":"3", "range":100}]}');
print hosts_object 
| summarize buildschema(hosts_object)
```


![Schema maken](media/json-data-structures/buildschema.png)

## <a name="next-steps"></a>Volgende stappen
Zie andere lessen voor het gebruik van de querytaal van Log Analytics:

- [Bewerkingen op tekenreeksen uitvoeren](string-operations.md)
- [Datum- en tijdbewerkingen](datetime-operations.md)
- [Aggregatiefuncties](aggregations.md)
- [Geavanceerde aggregaties](advanced-aggregations.md)
- [Geavanceerde query schrijven](advanced-query-writing.md)
- [Joins](joins.md)
- [Grafieken](charts.md)