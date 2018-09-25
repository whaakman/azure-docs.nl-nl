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
ms.devlang: na
ms.topic: article
ms.date: 08/11/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 663f0b04c528c180e4130c1c157441cbc0ceb98b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46955864"
---
# <a name="standard-properties-in-log-analytics-records"></a>Standaardeigenschappen in Log Analytics-records
Gegevens in [Log Analytics](../log-analytics/log-analytics-queries.md) wordt opgeslagen als een set records, elk met een bepaald type met een unieke set eigenschappen. Veel gegevenstypen heeft standaard-eigenschappen die betrekking hebben op meerdere typen. Dit artikel worden deze eigenschappen en voorbeelden van hoe u ze in query's gebruiken kunt.

Sommige van deze eigenschappen zijn nog steeds wordt geïmplementeerd, zodat u ze in bepaalde gegevenstypen, maar nog niet in andere kan zien.


## <a name="resourceid"></a>_ResourceId
De **_ResourceId** eigenschap bevat een unieke id voor de resource die de record is gekoppeld. Dit biedt u een standard-eigenschap gebruikt als bereik voor de query alleen records uit een bepaalde resource, of voor gerelateerde gegevens tussen meerdere tabellen samenvoegen.

Voor Azure-resources, de waarde van **_ResourceId** is de [Azure-resource-ID URL](../azure-resource-manager/resource-group-template-functions-resource.md). De eigenschap is momenteel beperkt tot Azure-resources, maar dit zal worden uitgebreid tot resources buiten Azure, zoals on-premises computers. 

### <a name="examples"></a>Voorbeelden
Het volgende voorbeeld ziet een query die lid wordt van prestatie- en gebeurtenisgegevens voor elke computer. U ziet alle gebeurtenissen met de ID van _101_ en CPU-gebruik meer dan 50%.

```Kusto
Perf 
| where CounterName == "% User Time" and CounterValue  > 50 and _ResourceId != "" 
| join kind=inner (     
    Event 
    | where EventID == 101 
) on _ResourceId
```

Het volgende voorbeeld ziet u een query die lid wordt van _AzureActivity_ records met _SecurityEvent_ records. Hier ziet u alle activiteit bewerkingen met gebruikers die zijn aangemeld bij deze machines.

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

- Meer informatie over hoe u [Log Analytics-gegevens worden opgeslagen](../log-analytics/log-analytics-queries.md).
- Ophalen van een les op [schrijven van query's in Log Analytics](../log-analytics/query-language/get-started-queries.md).
- Ophalen van een les op [samenvoegen van tabellen in Log Analytics-query's](../log-analytics/query-language/joins.md).