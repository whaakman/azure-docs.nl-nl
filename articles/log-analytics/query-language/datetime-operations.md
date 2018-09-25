---
title: Werken met datum-/ tijdwaarden in Azure Log Analytics-query's | Microsoft Docs
description: Hierin wordt beschreven hoe u werkt met de datum en tijd waarop gegevens in Log Analytics-query's.
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
ms.openlocfilehash: 9b0c58fdbfb0d55b3b8998f4edfc1222b9a3d4aa
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46988594"
---
# <a name="working-with-date-time-values-in-log-analytics-queries"></a>Werken met datum-/ tijdwaarden in Log Analytics-query 's

> [!NOTE]
> U moet voltooien [aan de slag met de Analytics-portal](get-started-analytics-portal.md) en [aan de slag met query's](get-started-queries.md) voordat het voltooien van deze les gaat uitvoeren.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Dit artikel wordt beschreven hoe u werkt met de datum en tijd waarop gegevens in Log Analytics-query's.


## <a name="date-time-basics"></a>Grondbeginselen van de datum-tijd
De querytaal van Log Analytics heeft twee primaire gegevenstypen die zijn gekoppeld aan de datums en tijden: datum/tijd en periode. Alle datums worden uitgedrukt in UTC. Meerdere tijdindelingen voor datum / worden ondersteund, is de ISO8601-notatie voorkeur. 

Timespans worden uitgedrukt als een decimaal getal gevolgd door een tijdeenheid:

|verkorte versie van   | tijdseenheid    |
|:---|:---|
|d           | dag          |
|u           | uur         |
|m           | minuut       |
|s           | seconde       |
|ms          | milliseconden  |
|wachttijden van microseconden | wachttijden van microseconden  |
|maatstreepjes        | nanoseconden   |

Datum/tijd kunnen worden gemaakt met het gebruik van een tekenreeks met de `todatetime` operator. Bijvoorbeeld, als u wilt controleren van de virtuele machine heartbeats verzonden in een specifiek tijdsbestek, kunt u het gebruik van de [tussen operator](https://docs.loganalytics.io/docs/Language-Reference/Scalar-operators/between-operator) dit is handig om op te geven van een tijdsbereik...

```Kusto
Heartbeat
| where TimeGenerated between(datetime("2018-06-30 22:46:42") .. datetime("2018-07-01 00:57:27"))
```

Een ander gebruikelijk scenario is een datum/tijd om deze te vergelijken. Bijvoorbeeld, als u wilt zien alle heartbeats in de afgelopen twee minuten, kunt u de `now` operator samen met een TimeSpan-waarde die aangeeft van twee minuten:

```Kusto
Heartbeat
| where TimeGenerated > now() - 2m
```

Een snelkoppeling is ook beschikbaar voor deze functie:
```Kusto
Heartbeat
| where TimeGenerated > now(-2m)
```

Hoewel de kortste en meest leesbare methode wordt gebruikt de `ago` operator:
```Kusto
Heartbeat
| where TimeGenerated > ago(2m)
```

Stel in plaats van de begin- en -tijd, te weten dat de begintijd en de duur. U kunt Herschrijf de query als volgt:

```Kusto
let startDatetime = todatetime("2018-06-30 20:12:42.9");
let duration = totimespan(25m);
Heartbeat
| where TimeGenerated between(startDatetime .. (startDatetime+duration) )
| extend timeFromStart = TimeGenerated - startDatetime
```

## <a name="converting-time-units"></a>Tijdseenheden converteren
Kan het nuttig zijn om een datum/tijd of de periode in een tijdeenheid dan de standaardwaarde. Bijvoorbeeld: Stel dat u bij het controleren van foutgebeurtenissen van de laatste 30 minuten en moet een berekende kolom die laat zien hoe lang gelden de gebeurtenis heeft plaatsgevonden:

```Kusto
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated 
```

U ziet de _timeAgo_ kolom bevat waarden, zoals: '00:09:31.5118992', wat betekent dat ze zijn opgemaakt als hh:mm:ss.fffffff. Als u wilt opmaken van deze waarden naar de _numver_ minuten sinds de begintijd, te delen die waarde door 'minuut':

```Kusto
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated
| extend timeAgoMinutes = timeAgo/1m 
```


## <a name="aggregations-and-bucketing-by-time-intervals"></a>Aggregaties en bucketing door tijdsintervallen
Een andere zeer gangbaar scenario is de noodzaak om statistische gegevens gedurende een bepaalde periode in een bepaald tijdsinterval. Hiervoor een `bin` operator kan worden gebruikt als onderdeel van een component samenvatten.

Gebruik de volgende query uit om het aantal gebeurtenissen die hebben plaatsgevonden om de 5 minuten gedurende de afgelopen half uur:

```Kusto
Event
| where TimeGenerated > ago(30m)
| summarize events_count=count() by bin(TimeGenerated, 5m) 
```

Dit resulteert in de volgende tabel:  
|TimeGenerated(UTC)|events_count|
|--|--|
|2018-08-01T09:30:00.000|54|
|2018-08-01T09:35:00.000|41|
|2018-08-01T09:40:00.000|42|
|2018-08-01T09:45:00.000|41|
|2018-08-01T09:50:00.000|41|
|2018-08-01T09:55:00.000|16|

Een andere manier om te maken van de buckets van resultaten, is met functies, zoals `startofday`:

```Kusto
Event
| where TimeGenerated > ago(4d)
| summarize events_count=count() by startofday(TimeGenerated) 
```

Dit resulteert in de volgende resultaten:

|tijdstempel|count_|
|--|--|
|2018-07-28T00:00:00.000|7,136|
|2018-07-29T00:00:00.000|12,315|
|2018-07-30T00:00:00.000|16,847|
|2018-07-31T00:00:00.000|12,616|
|2018-08-01T00:00:00.000|5,416  |


## <a name="time-zones"></a>Tijdzones
Omdat alle datum-/ tijdwaarden worden uitgedrukt in UTC, is het vaak nuttig om te converteren naar de lokale tijdzone. Deze berekening bijvoorbeeld gebruiken om te converteren van UTC naar PST tijden:

```Kusto
Event
| extend localTimestamp = TimeGenerated - 8h
```

## <a name="related-functions"></a>Verwante functies

| Categorie | Functie |
|:---|:---|
| Gegevenstypen converteren | [ToDateTime](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/todatetime())[totimespan](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/totimespan())  |
| Ronde waarde die u wilt de grootte van opslaglocatie | [opslaglocatie](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/bin()) |
| Ophalen van een bepaalde datum of tijd | [geleden](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/ago()) [nu](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/now())   |
| Onderdeel van de waarde ophalen | [datetime_part](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/datetime_part()) [getmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/getmonth()) [monthofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/monthofyear()) [getyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/getyear()) [dayofmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/dayofmonth()) [dayofweek](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/dayofweek()) [dayofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/dayofyear()) [weekofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/weekofyear()) |
| Een datum ten opzichte van de waarde ophalen  | [endofday](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofday()) [endofweek](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofweek()) [endofmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofmonth()) [endofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofyear()) [startofday](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofday()) [beginvanweek](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofweek()) [startofmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofmonth()) [startofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofyear()) |

## <a name="next-steps"></a>Volgende stappen
Zie andere lessen voor het gebruik van de querytaal van Log Analytics:

- [Bewerkingen op tekenreeksen uitvoeren](string-operations.md)
- [Aggregatiefuncties](aggregations.md)
- [Geavanceerde aggregaties](advanced-aggregations.md)
- [JSON en gegevensstructuren](json-data-structures.md)
- [Geavanceerde query schrijven](advanced-query-writing.md)
- [Joins](joins.md)
- [Grafieken](charts.md)