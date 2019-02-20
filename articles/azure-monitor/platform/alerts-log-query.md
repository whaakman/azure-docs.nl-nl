---
title: Meld u waarschuwingsquery's in Azure Monitor | Microsoft Docs
description: Bevat aanbevelingen voor het schrijven van efficiënte query's voor waarschuwingen in Azure Monitor-updates en een proces voor het converteren van bestaande query's.
author: yossi-y
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: bwren
ms.subservice: alerts
ms.openlocfilehash: c50c1a111f037b74176b5ca2cf8af518b2d3ffa0
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56429380"
---
# <a name="log-alert-queries-in-azure-monitor"></a>Meld u waarschuwingsquery's in Azure Monitor
[Waarschuwingsregels op basis van Azure Monitor logboeken](alerts-unified-log.md) met regelmatige tussenpozen uitvoeren, dus moet u ervoor zorgen dat ze worden geschreven naar de overhead en Latentie minimaliseren. In dit artikel bevat aanbevelingen voor het schrijven van efficiënte query's voor waarschuwingen en een proces voor het converteren van bestaande query's. 

## <a name="types-of-log-queries"></a>Typen logboeken-query 's
[Query's in Log Analytics](../log-query/log-query-overview.md) starten met ofwel een tabel of een [zoeken](/azure/kusto/query/searchoperator) of [union](/azure/kusto/query/unionoperator) operator.

Bijvoorbeeld de volgende query is afgestemd op de _SecurityEvent_ tabel en wordt gezocht naar specifieke gebeurtenis-ID. Dit is de enige tabel waarmee de query moet worden verwerkt.

``` Kusto
SecurityEvent | where EventID == 4624 
```

Query's die met beginnen `search` of `union` kunt u zoeken in meerdere kolommen in een tabel of zelfs meerdere tabellen. De volgende voorbeelden ziet u meerdere methoden voor het zoeken naar de term _geheugen_:

```Kusto
search "Memory"

search * | where == "Memory"

search ObjectName: "Memory"

search ObjectName == "Memory"

union * | where ObjectName == "Memory"
```
 

Hoewel `search` en `union` zijn handig zijn tijdens het gegevensverkenning, voorwaarden zoeken in het hele gegevensmodel, ze zijn minder efficiënt dan het gebruik van een tabel, omdat ze tussen meerdere tabellen moeten scannen. Omdat de query's in de regels voor waarschuwingen met regelmatige tussenpozen worden uitgevoerd, kan dit resulteren in overmatige overhead latentie toe te voegen aan de waarschuwing. Vanwege deze overhead moeten altijd query's voor waarschuwingsregels in Azure beginnen met een tabel om een duidelijke bereik, wat zorgt voor betere prestaties van query's zowel de relevantie van de resultaten te definiëren.

## <a name="unsupported-queries"></a>Niet-ondersteunde query 's
Vanaf januari 11,2019, het maken of wijzigen van logboekwaarschuwingsregels die gebruikmaken van `search`, of `union` operators worden niet ondersteund in Azure portal. Met behulp van deze operators in een waarschuwingsregel wordt een foutbericht geretourneerd. Bestaande waarschuwingsregels en waarschuwingsregels gemaakt en bewerkt met de Log Analytics-API worden niet beïnvloed door deze wijziging. U moet nog steeds rekening houden met een waarschuwing wijzigen regels die gebruikmaken van deze typen query's als doorgangsschijf naar hun efficiëntie te verbeteren.  

Meld u met behulp van regels voor waarschuwingen [query's voor meerdere bronnen](../log-query/cross-workspace-query.md) worden niet beïnvloed door deze wijziging omdat meerdere bronnen query's gebruiken `union`, die het querybereik op specifieke resources beperkt. Dit is geen equivalent van `union *` die kan niet worden gebruikt.  Het volgende voorbeeld zou zijn geldig in een waarschuwingsregel:

```Kusto
union 
app('Contoso-app1').requests, 
app('Contoso-app2').requests, 
workspace('Contoso-workspace1').Perf 
```

>[!NOTE]
>[Meerdere bronnen query](../log-query/cross-workspace-query.md) in logboek waarschuwingen wordt ondersteund in de nieuwe [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules). Azure Monitor gebruikt standaard de [verouderde Log Analytics-waarschuwing API](api-alerts.md) voor het maken van nieuwe logboek waarschuwingsregels van Azure-portal, tenzij u van overschakelt [verouderde Log waarschuwingen API](alerts-log-api-switch.md#process-of-switching-from-legacy-log-alerts-api). Na de switch, de nieuwe API wordt de standaardwaarde voor nieuwe regels voor waarschuwingen in Azure portal en u kunt meerdere bronnen query log regels waarschuwingen maken. U kunt maken [meerdere bronnen query](../log-query/cross-workspace-query.md) melden van regels voor waarschuwingen zonder dat de switch met behulp van de [ARM-sjabloon voor scheduledQueryRules API](alerts-log.md#log-alert-with-cross-resource-query-using-azure-resource-template) –, maar deze waarschuwingsregel wordt al beheerd [ scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) en niet vanuit Azure portal.

## <a name="examples"></a>Voorbeelden
De volgende voorbeelden zijn onder meer logboeken-query's die gebruikmaken van `search` en `union` en vindt u stappen kunt u deze query's voor gebruik met regels voor waarschuwingen wijzigen.

### <a name="example-1"></a>Voorbeeld 1
U wilt maken van een waarschuwingsregel met behulp van de volgende query haalt de prestaties van informatie over het gebruik `search`: 

``` Kusto
search * | where Type == 'Perf' and CounterName == '% Free Space' 
| where CounterValue < 30 
| summarize count()
```
  

Start met behulp van de volgende query uit om te identificeren in de tabel die deel uitmaken van de eigenschappen op voor het wijzigen van deze query:

``` Kusto
search * | where CounterName == '% Free Space'
| summarize by $table
```
 

Het resultaat van deze query zou laten zien dat de _CounterName_ eigenschap afkomstig zijn uit de _Perf_ tabel. 

Dit resultaat kunt u de volgende query die u voor de waarschuwingsregel gebruiken wilt maken:

``` Kusto
Perf 
| where CounterName == '% Free Space' 
| where CounterValue < 30 
| summarize count()
```


### <a name="example-2"></a>Voorbeeld 2
U wilt maken van een waarschuwingsregel met behulp van de volgende query haalt de prestaties van informatie over het gebruik `search`: 

``` Kusto
search ObjectName =="Memory" and CounterName=="% Committed Bytes In Use"  
| summarize Avg_Memory_Usage =avg(CounterValue) by Computer 
| where Avg_Memory_Usage between(90 .. 95)  
| count 
```
  

Start met behulp van de volgende query uit om te identificeren in de tabel die deel uitmaken van de eigenschappen op voor het wijzigen van deze query:

``` Kusto
search ObjectName=="Memory" and CounterName=="% Committed Bytes In Use" 
| summarize by $table 
```
 

Het resultaat van deze query zou laten zien dat de _ObjectName_ en _CounterName_ eigenschap afkomstig zijn uit de _Perf_ tabel. 

Dit resultaat kunt u de volgende query die u voor de waarschuwingsregel gebruiken wilt maken:

``` Kusto
Perf 
| where ObjectName =="Memory" and CounterName=="% Committed Bytes In Use" 
| summarize Avg_Memory_Usage=avg(CounterValue) by Computer 
| where Avg_Memory_Usage between(90 .. 95)  
| count 
```
 

### <a name="example-3"></a>Voorbeeld 3

U wilt maken van een waarschuwingsregel met behulp van de volgende query die gebruikmaakt van zowel `search` en `union` ophalen van informatie over de prestaties: 

``` Kusto
search (ObjectName == "Processor" and CounterName == "% Idle Time" and InstanceName == "_Total")  
| where Computer !in ((union * | where CounterName == "% Processor Utility" | summarize by Computer))
| summarize Avg_Idle_Time = avg(CounterValue) by Computer|  count  
```
 

Voor het wijzigen van deze query starten met behulp van de volgende query uit de tabel die deel uitmaken van de eigenschappen in het eerste deel van de query te identificeren: 

``` Kusto
search (ObjectName == "Processor" and CounterName == "% Idle Time" and InstanceName == "_Total")  
| summarize by $table 
```

Het resultaat van deze query zou worden weergegeven of alle eigenschappen van deze afkomstig van is de _Perf_ tabel. 

Nu gebruiken `union` met `withsource` opdracht uit om te bepalen welke brontabel elke rij heeft bijgedragen.

``` Kusto
union withsource=table * | where CounterName == "% Processor Utility" 
| summarize by table 
```
 

Het resultaat van deze query zou worden weergegeven of deze eigenschappen ook afkomstig van is de _Perf_ tabel. 

Deze resultaten kunt u de volgende query die u voor de waarschuwingsregel gebruiken wilt maken: 

``` Kusto
Perf 
| where ObjectName == "Processor" and CounterName == "% Idle Time" and InstanceName == "_Total" 
| where Computer !in ( 
    (Perf 
    | where CounterName == "% Processor Utility" 
    | summarize by Computer)) 
| summarize Avg_Idle_Time = avg(CounterValue) by Computer 
| count 
``` 

### <a name="example-4"></a>Voorbeeld 4
U wilt maken van een waarschuwingsregel met behulp van de volgende query die lid wordt van de resultaten van de twee `search` query's:

```Kusto
search Type == 'SecurityEvent' and EventID == '4625' 
| summarize by Computer, Hour = bin(TimeGenerated, 1h) 
| join kind = leftouter ( 
    search in (Heartbeat) OSType == 'Windows' 
    | summarize arg_max(TimeGenerated, Computer) by Computer , Hour = bin(TimeGenerated, 1h) 
    | project Hour , Computer  
)  
on Hour 
| count 
```
 

Als u wilt wijzigen van de query, start u met behulp van de volgende query uit om te identificeren van de tabel die de eigenschappen in aan de linkerkant van de join bevat: 

``` Kusto
search Type == 'SecurityEvent' and EventID == '4625' 
| summarize by $table 
```
 

Het resultaat geeft aan dat de eigenschappen in aan de linkerkant van de join horen bij _SecurityEvent_ tabel. 

Gebruik nu de volgende query voor het identificeren van de tabel die de eigenschappen in aan de rechterkant van de join bevat: 

 
``` Kusto
search in (Heartbeat) OSType == 'Windows' 
| summarize by $table 
```

 
Het resultaat geeft aan dat de eigenschappen in aan de rechterkant van de join deel uitmaken van Heartbeat-tabel. 

Deze resultaten kunt u de volgende query die u voor de waarschuwingsregel gebruiken wilt maken: 


``` Kusto
SecurityEvent
| where EventID == '4625'
| summarize by Computer, Hour = bin(TimeGenerated, 1h)
| join kind = leftouter (
    Heartbeat  
    | where OSType == 'Windows' 
    | summarize arg_max(TimeGenerated, Computer) by Computer , Hour = bin(TimeGenerated, 1h) 
    | project Hour , Computer  
)  
on Hour 
| count 
```

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [waarschuwingen voor activiteitenlogboeken](alerts-log.md) in Azure Monitor.
- Meer informatie over [query's bijgehouden](../log-query/log-query-overview.md).

