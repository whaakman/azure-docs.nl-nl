---
title: Geavanceerde query's in Azure Monitor | Microsoft Docs
description: Dit artikel bevat een zelfstudie voor het gebruik van de Analytics-portal voor het schrijven van query's in Azure Monitor.
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
ms.date: 11/15/2018
ms.author: bwren
ms.openlocfilehash: 62dffffee411519cdcafe80a11404f9e273dd9dd
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56880781"
---
# <a name="writing-advanced-queries-in-azure-monitor"></a>Geavanceerde query's in Azure Monitor schrijven

> [!NOTE]
> U moet voltooien [aan de slag met Azure Monitor Log-Analytics](get-started-portal.md) en [aan de slag met query's](get-started-queries.md) voordat het voltooien van deze les gaat uitvoeren.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

## <a name="reusing-code-with-let"></a>Hergebruik van code met kunt
Gebruik `let` resultaten toewijzen aan een variabele en verwijzen naar deze later in de query:

```Kusto
// get all events that have level 2 (indicates warning level)
let warning_events=
Event
| where EventLevel == 2;
// count the number of warning events per computer
warning_events
| summarize count() by Computer 
```

U kunt ook constante waarden toewijzen aan variabelen. Dit biedt ondersteuning voor een methode voor het instellen van parameters voor de velden die u wijzigen wilt, telkens wanneer u de query uitvoert. Deze parameters zo nodig wijzigen. Als u bijvoorbeeld voor het berekenen van de vrije schijfruimte en het beschikbaar geheugen (in percentielen), in een bepaalde periode:

```Kusto
let startDate = datetime(2018-08-01T12:55:02);
let endDate = datetime(2018-08-02T13:21:35);
let FreeDiskSpace =
Perf
| where TimeGenerated between (startDate .. endDate)
| where ObjectName=="Logical Disk" and CounterName=="Free Megabytes"
| summarize percentiles(CounterValue, 50, 75, 90, 99);
let FreeMemory =
Perf
| where TimeGenerated between (startDate .. endDate)
| where ObjectName=="Memory" and CounterName=="Available MBytes Memory"
| summarize percentiles(CounterValue, 50, 75, 90, 99);
union FreeDiskSpace, FreeMemory
```

Hierdoor kunt gemakkelijk wijzigen van het begin van de eindtijd van de volgende keer dat u de query uitvoert.

### <a name="local-functions-and-parameters"></a>Lokale functies en parameters
Gebruik `let` instructies toe aan functies die kunnen worden gebruikt in dezelfde query maken. Bijvoorbeeld, een functie die een datum / tijdveld (in de UTC-notatie) neemt en geconverteerd naar een standaardindeling in de Verenigde Staten definiëren. 

```Kusto
let utc_to_us_date_format = (t:datetime)
{
  strcat(getmonth(t), "/", dayofmonth(t),"/", getyear(t), " ",
  bin((t-1h)%12h+1h,1s), iff(t%24h<12h, "AM", "PM"))
};
Event 
| where TimeGenerated > ago(1h) 
| extend USTimeGenerated = utc_to_us_date_format(TimeGenerated)
| project TimeGenerated, USTimeGenerated, Source, Computer, EventLevel, EventData 
```

## <a name="print"></a>Afdrukken
`print` retourneert een tabel met één kolom en een enkele rij weergegeven, het resultaat van een berekening. Dit wordt vaak gebruikt in gevallen waarin u een eenvoudige calcuation. Als u bijvoorbeeld wilt zoeken naar het huidige tijdstip in PST en een kolom met EST toevoegen:

```Kusto
print nowPst = now()-8h
| extend nowEst = nowPst+3h
```

## <a name="datatable"></a>DataTable
`datatable` kunt u een set gegevens definiëren. U een schema en een set waarden opgeven en vervolgens kunt openen in de tabel in een andere query-elementen. Als u bijvoorbeeld een tabel van het RAM-gebruik van maken en de gemiddelde waarde per uur berekenen:

```Kusto
datatable (TimeGenerated: datetime, usage_percent: double)
[
  "2018-06-02T15:15:46.3418323Z", 15.5,
  "2018-06-02T15:45:43.1561235Z", 20.2,
  "2018-06-02T16:16:49.2354895Z", 17.3,
  "2018-06-02T16:46:44.9813459Z", 45.7,
  "2018-06-02T17:15:41.7895423Z", 10.9,
  "2018-06-02T17:44:23.9813459Z", 24.7,
  "2018-06-02T18:14:59.7283023Z", 22.3,
  "2018-06-02T18:45:12.1895483Z", 25.4
]
| summarize avg(usage_percent) by bin(TimeGenerated, 1h)
```

DataTable-constructies zijn ook nuttig bij het maken van een opzoektabel. Bijvoorbeeld, om toe te wijzen tabelgegevens zoals gebeurtenis-id's van de _SecurityEvent_ tabel, gebeurtenistypen ergens anders vermeld, een lookup-tabel maken met de typen gebeurtenissen met behulp van `datatable` en deelnemen aan deze datatable met  _SecurityEvent_ gegevens:

```Kusto
let eventCodes = datatable (EventID: int, EventType:string)
[
    4625, "Account activity",
    4688, "Process action",
    4634, "Account activity",
    4672, "Access",
    4624, "Account activity",
    4799, "Access management",
    4798, "Access management",
    5059, "Key operation",
    4648, "A logon was attempted using explicit credentials",
    4768, "Access management",
    4662, "Other",
    8002, "Process action",
    4904, "Security event management",
    4905, "Security event management",
];
SecurityEvent
| where TimeGenerated > ago(1h) 
| join kind=leftouter (
  eventCodes
) on EventID
| project TimeGenerated, Account, AccountType, Computer, EventType
```

## <a name="next-steps"></a>Volgende stappen
Zie andere lessen voor het gebruik van de [Kusto-querytaal](/azure/kusto/query/) met Azure Monitor gegevens vastleggen:

- [Bewerkingen op tekenreeksen uitvoeren](string-operations.md)
- [Datum- en tijdbewerkingen](datetime-operations.md)
- [Aggregatiefuncties](aggregations.md)
- [Geavanceerde aggregaties](advanced-aggregations.md)
- [JSON en gegevensstructuren](json-data-structures.md)
- [Joins](joins.md)
- [Grafieken](charts.md)
