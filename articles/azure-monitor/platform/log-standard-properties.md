---
title: Logboekregistratie van records standaardeigenschappen in Azure Monitor | Microsoft Docs
description: Beschrijft eigenschappen die gemeenschappelijk voor meerdere gegevenstypen in Logboeken van Azure Monitor zijn.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 03/20/2019
ms.author: bwren
ms.openlocfilehash: 50804e1f6ab4f352239d3f405e5b41e4e0c58d14
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67292808"
---
# <a name="standard-properties-in-azure-monitor-logs"></a>Standaardeigenschappen in Logboeken van Azure Monitor
Gegevens in Azure controleren logboeken [opgeslagen als een set records in een Log Analytics-werkruimte of in Application Insights-toepassing](../log-query/logs-structure.md), elk met een bepaald type met een unieke set eigenschappen. Veel gegevenstypen heeft standaard-eigenschappen die betrekking hebben op meerdere typen. Dit artikel worden deze eigenschappen en voorbeelden van hoe u ze in query's gebruiken kunt.

Sommige van deze eigenschappen zijn nog steeds wordt geïmplementeerd, zodat u ze in bepaalde gegevenstypen, maar nog niet in andere kan zien.

## <a name="timegenerated-and-timestamp"></a>TimeGenerated en tijdstempel
De **TimeGenerated** (Log Analytics-werkruimte) en **timestamp** (Application Insights-toepassing) eigenschappen bevatten de datum en tijd waarop de record is gemaakt. Het biedt een algemene eigenschap moet worden gebruikt voor filteren of samen te vatten door de tijd. Wanneer u een tijdsbereik voor een weergave of dashboard in Azure portal selecteert, gebruikt deze TimeGenerated of tijdstempel om de resultaten te filteren.

### <a name="examples"></a>Voorbeelden

De volgende query retourneert het aantal foutgebeurtenissen die zijn gemaakt voor elke dag in de vorige week.

```Kusto
Event
| where EventLevelName == "Error" 
| where TimeGenerated between(startofweek(ago(7days))..endofweek(ago(7days))) 
| summarize count() by bin(TimeGenerated, 1day) 
| sort by TimeGenerated asc 
```

De volgende query retourneert het aantal uitzonderingen die zijn gemaakt voor elke dag in de vorige week.

```Kusto
exceptions
| where timestamp between(startofweek(ago(7days))..endofweek(ago(7days))) 
| summarize count() by bin(TimeGenerated, 1day) 
| sort by timestamp asc 
```

## <a name="type-and-itemtype"></a>Type en het itemType
De **Type** (Log Analytics-werkruimte) en **itemType** blokkering van de eigenschappen (Application Insights-toepassing) de naam van de tabel die de record is opgehaald van kan ook worden beschouwd als de record type. Deze eigenschap is handig in query's met een combinatie van records uit meerdere tabellen, zoals die waarin de `search` -operator, onderscheid maken tussen de records van verschillende typen. **$table** kan worden gebruikt in plaats van **Type** op bepaalde plaatsen.

### <a name="examples"></a>Voorbeelden
De volgende query retourneert het aantal records per type in het afgelopen uur worden verzameld.

```Kusto
search * 
| where TimeGenerated > ago(1h)
| summarize count() by Type
```

## <a name="resourceid"></a>\_ResourceId
De  **\_ResourceId** eigenschap bevat een unieke id voor de resource die de record is gekoppeld. Dit biedt u een standard-eigenschap gebruikt als bereik voor de query alleen records uit een bepaalde resource, of voor gerelateerde gegevens tussen meerdere tabellen samenvoegen.

Voor Azure-resources, de waarde van **_ResourceId** is de [Azure-resource-ID URL](../../azure-resource-manager/resource-group-template-functions-resource.md). De eigenschap is momenteel beperkt tot Azure-resources, maar dit zal worden uitgebreid tot resources buiten Azure, zoals on-premises computers.

> [!NOTE]
> Sommige gegevenstypes al velden die Azure-resource-ID bevatten of ten minste onderdelen van het abonnements-ID. Terwijl deze velden voor achterwaartse compatibiliteit behouden, verdient het _ResourceId gebruiken voor het uitvoeren van meerdere correlatie, omdat deze wordt consistenter zijn.

### <a name="examples"></a>Voorbeelden
Prestatie- en gegevens voor elke computer lid wordt van de volgende query uit. U ziet alle gebeurtenissen met de ID van _101_ en CPU-gebruik meer dan 50%.

```Kusto
Perf 
| where CounterName == "% User Time" and CounterValue  > 50 and _ResourceId != "" 
| join kind=inner (     
    Event 
    | where EventID == 101 
) on _ResourceId
```

De volgende query joins _AzureActivity_ records met _SecurityEvent_ records. Hier ziet u alle activiteit bewerkingen met gebruikers die zijn aangemeld bij deze machines.

```Kusto
AzureActivity 
| where  
    OperationName in ("Restart Virtual Machine", "Create or Update Virtual Machine", "Delete Virtual Machine")  
    and ActivityStatus == "Succeeded"  
| join kind= leftouter (    
   SecurityEvent 
   | where EventID == 4624  
   | summarize LoggedOnAccounts = makeset(Account) by _ResourceId 
) on _ResourceId  
```

De volgende query parseert **_ResourceId** en statistische functies in rekening gebracht gegevensvolumes per Azure-abonnement.

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| parse tolower(_ResourceId) with "/subscriptions/" subscriptionId "/resourcegroups/" 
    resourceGroup "/providers/" provider "/" resourceType "/" resourceName   
| summarize Bytes=sum(_BilledSize) by subscriptionId | sort by Bytes nulls last 
```

Gebruik deze `union withsource = tt *` spaarzaam scans in verschillende gegevenstypen zijn duur in het uitvoeren van query's.

## <a name="isbillable"></a>\_IsBillable
De  **\_IsBillable** eigenschap geeft aan of opgenomen gegevens betaald zijn. Gegevens met  **\_IsBillable** gelijk zijn aan _false_ worden verzameld voor gratis en niet in rekening gebracht bij uw Azure-account.

### <a name="examples"></a>Voorbeelden
Als u een lijst met computers die worden gefactureerd gegevenstypen verzenden, gebruikt u de volgende query uit:

> [!NOTE]
> Met query's met `union withsource = tt *` spaarzaam scans in verschillende gegevenstypen zijn kostbaar zijn om uit te voeren. 

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName
```

Dit kan worden uitgebreid om terug te keren de telling van computers per uur die verzenden in rekening gebracht gegevenstypen:

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc
```

## <a name="billedsize"></a>\_BilledSize
De  **\_BilledSize** eigenschap geeft de grootte in bytes aan gegevens dat wordt gefactureerd naar uw Azure-account als  **\_IsBillable** is ingesteld op true.

### <a name="examples"></a>Voorbeelden
Als de grootte van factureerbare gebeurtenissen die per computer weergeven, gebruikt u de `_BilledSize` eigenschap waarmee u de grootte in bytes:

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| summarize Bytes=sum(_BilledSize) by  Computer | sort by Bytes nulls last 
```

Als u wilt zien van de grootte van factureerbare gebeurtenissen die per abonnement zijn opgenomen, gebruikt u de volgende query:

```Kusto
union withsource=table * 
| where _IsBillable == true 
| parse _ResourceId with "/subscriptions/" SubscriptionId "/" *
| summarize Bytes=sum(_BilledSize) by  SubscriptionId | sort by Bytes nulls last 
```

Als de grootte van factureerbare gebeurtenissen die per resourcegroep weergeven, gebruikt u de volgende query uit:

```Kusto
union withsource=table * 
| where _IsBillable == true 
| parse _ResourceId with "/subscriptions/" SubscriptionId "/resourcegroups/" ResourceGroupName "/" *
| summarize Bytes=sum(_BilledSize) by  SubscriptionId, ResourceGroupName | sort by Bytes nulls last 

```


Als het aantal gebeurtenissen die per computer weergeven, gebruikt u de volgende query uit:

```Kusto
union withsource = tt *
| summarize count() by Computer | sort by count_ nulls last
```

Als het aantal factureerbare gebeurtenissen die per computer weergeven, gebruikt u de volgende query uit: 

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| summarize count() by Computer  | sort by count_ nulls last
```

Als het aantal factureerbare gegevenstypen van een specifieke computer weergeven, gebruikt u de volgende query uit:

```Kusto
union withsource = tt *
| where Computer == "computer name"
| where _IsBillable == true 
| summarize count() by tt | sort by count_ nulls last 
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe u [Azure Monitor log-gegevens worden opgeslagen](../log-query/log-query-overview.md).
- Ophalen van een les op [logboeken-query's schrijven](../../azure-monitor/log-query/get-started-queries.md).
- Ophalen van een les op [samenvoegen van tabellen in Logboeken-query's](../../azure-monitor/log-query/joins.md).
