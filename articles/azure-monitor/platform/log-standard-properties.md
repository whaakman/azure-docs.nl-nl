---
title: Standaard-eigenschappen in Azure Monitor Log Analytics-records | Microsoft Docs
description: Beschrijft eigenschappen die gemeenschappelijk voor meerdere gegevenstypen in Azure Monitor Log Analytics zijn.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/27/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: e4e921223676f4b5d64025c67914fc8b7c29a6fe
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52974974"
---
# <a name="standard-properties-in-log-analytics-records"></a>Standaardeigenschappen in Log Analytics-records
Gegevens in [Log Analytics](../log-query/log-query-overview.md) wordt opgeslagen als een set records, elk met een bepaald type met een unieke set eigenschappen. Veel gegevenstypen heeft standaard-eigenschappen die betrekking hebben op meerdere typen. Dit artikel worden deze eigenschappen en voorbeelden van hoe u ze in query's gebruiken kunt.

Sommige van deze eigenschappen zijn nog steeds wordt geïmplementeerd, zodat u ze in bepaalde gegevenstypen, maar nog niet in andere kan zien.

## <a name="timegenerated"></a>TimeGenerated
De **TimeGenerated** eigenschap bevat de datum en tijd waarop de record is gemaakt. Het biedt een algemene eigenschap moet worden gebruikt voor filteren of samen te vatten door de tijd. Wanneer u een tijdsbereik voor een weergave of dashboard in Azure portal selecteert, wordt TimeGenerated gebruikt om de resultaten te filteren.

### <a name="examples"></a>Voorbeelden

De volgende query retourneert het aantal foutgebeurtenissen die zijn gemaakt voor elke dag in de vorige week.

```Kusto
Event
| where EventLevelName == "Error" 
| where TimeGenerated between(startofweek(ago(7days))..endofweek(ago(7days))) 
| summarize count() by bin(TimeGenerated, 1day) 
| sort by TimeGenerated asc 
```

## <a name="type"></a>Type
De **Type** eigenschap bevat de naam van de tabel die de record is opgehaald van waaruit kan ook worden beschouwd als het recordtype. Deze eigenschap is handig in query's met een combinatie van records uit meerdere tabellen, zoals die waarin de `search` -operator, onderscheid maken tussen de records van verschillende typen. **$table** kan worden gebruikt in plaats van **Type** op bepaalde plaatsen.

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

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe u [Log Analytics-gegevens worden opgeslagen](../log-query/log-query-overview.md).
- Ophalen van een les op [schrijven van query's in Log Analytics](../../azure-monitor/log-query/get-started-queries.md).
- Ophalen van een les op [samenvoegen van tabellen in Log Analytics-query's](../../azure-monitor/log-query/joins.md).
