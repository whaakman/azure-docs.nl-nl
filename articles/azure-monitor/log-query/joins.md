---
title: Joins in Logboeken-query's van Azure Monitor | Microsoft Docs
description: Dit artikel bevat een les over het gebruik van joins in Azure Monitor logboeken-query's.
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
ms.date: 08/16/2018
ms.author: bwren
ms.openlocfilehash: 2ea5b4e3af6591e6e25a863998baa7cecb3e29e8
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58079108"
---
# <a name="joins-in-azure-monitor-log-queries"></a>Joins in Logboeken-query's van Azure Monitor

> [!NOTE]
> U moet voltooien [aan de slag met Azure Monitor Log-Analytics](get-started-portal.md) en [logboeken-query's van Azure Monitor](get-started-queries.md) voordat het voltooien van deze les gaat uitvoeren.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Met samenvoegingen kunt u gegevens uit meerdere tabellen, in dezelfde query kunt analyseren. Ze samenvoegen de rijen van twee gegevenssets met overeenkomende waarden van de opgegeven kolommen.


```Kusto
SecurityEvent 
| where EventID == 4624     // sign-in events
| project Computer, Account, TargetLogonId, LogonTime=TimeGenerated
| join kind= inner (
    SecurityEvent 
    | where EventID == 4634     // sign-out events
    | project TargetLogonId, LogoffTime=TimeGenerated
) on TargetLogonId
| extend Duration = LogoffTime-LogonTime
| project-away TargetLogonId1 
| top 10 by Duration desc
```

In dit voorbeeld wordt de eerste gegevensset gefilterd voor alle gebeurtenissen voor aanmelden. Dit is gekoppeld met een tweede gegevensset waarmee voor alle afmelding gebeurtenissen worden gefilterd. De verwachte kolommen zijn _Computer_, _Account_, _TargetLogonId_, en _TimeGenerated_. De gegevenssets worden gecorreleerd met een gedeelde kolom _TargetLogonId_. De uitvoer is één record per correlatie met het aanmelden en afmelden tijd.

Als u beide gegevenssets kolommen met dezelfde naam hebben, de kolommen van de gegevensset aan de rechterkant wilt toekennen aan een indexnummer zodat in dit voorbeeld de resultaten toont _TargetLogonId_ met waarden uit de tabel aan de linkerkant en  _TargetLogonId1_ met waarden uit de tabel aan de rechterkant. In dit geval wordt de tweede _TargetLogonId1_ kolom is verwijderd met behulp van de `project-away` operator.

> [!NOTE]
> Voor betere prestaties, blijven alleen de relevante kolommen van de gekoppelde data-sets, met behulp van de `project` operator.


De volgende syntaxis gebruiken om te koppelen van twee gegevenssets en de sleutel is toegevoegd aan een andere naam tussen de twee tabellen bevat:
```
Table1
| join ( Table2 ) 
on $left.key1 == $right.key2
```

## <a name="lookup-tables"></a>Opzoektabellen
Joins vaak gebruikt met behulp van statische toewijzing van waarden met behulp van `datatable` die helpen bij het omzetten van de resultaten in meer uitziet. Bijvoorbeeld, te verrijken van de beveiliging gebeurtenisgegevens met de naam van de gebeurtenis voor elke gebeurtenis-id.

```Kusto
let DimTable = datatable(EventID:int, eventName:string)
  [
    4625, "Account activity",
    4688, "Process action",
    4634, "Account activity",
    4658, "The handle to an object was closed",
    4656, "A handle to an object was requested",
    4690, "An attempt was made to duplicate a handle to an object",
    4663, "An attempt was made to access an object",
    5061, "Cryptographic operation",
    5058, "Key file operation"
  ];
SecurityEvent
| join kind = inner
 DimTable on EventID
| summarize count() by eventName
```

![Lid maken met een datatable](media/joins/dim-table.png)

## <a name="join-kinds"></a>Typen koppelen
Geef het type koppelen met de _soort_ argument. Elk type voert een andere overeenkomst tussen de records van de opgegeven tabellen, zoals beschreven in de volgende tabel.

| Type join | Description |
|:---|:---|
| innerunique | Dit is de standaardmodus voor de join. Eerst de waarden van de overeenkomende kolom in de linkertabel worden gevonden en dubbele waarden zijn verwijderd.  De set met unieke waarden wordt vervolgens vergeleken met de rechtertabel. |
| binnenste | Alleen overeenkomende records in beide tabellen zijn opgenomen in de resultaten. |
| leftouter | Alle records in de linkertabel en overeenkomende records in de rechtertabel zijn opgenomen in de resultaten. Niet-overeenkomende uitvoereigenschappen bevatten null-waarden.  |
| leftanti | Records aan de linkerkant waarvoor geen overeenkomsten vanaf de rechterkant worden opgenomen in de resultaten. De tabel met resultaten heeft alleen kolommen uit de linkertabel. |
| leftsemi | Records aan de linkerkant waarvoor komt overeen met vanaf de rechterkant worden opgenomen in de resultaten. De tabel met resultaten heeft alleen kolommen uit de linkertabel. |


## <a name="best-practices"></a>Aanbevolen procedures

Houd rekening met de volgende punten voor optimale prestaties:

- Een tijdfilter voor elke tabel gebruiken om te beperken van de records die moeten worden geëvalueerd voor de join.
- Gebruik `where` en `project` te verminderen van het aantal rijen en kolommen in de invoer tabellen voordat u de join.
- Als een tabel altijd kleiner zijn dan de andere is, kunt u deze als aan de linkerkant van de join gebruiken.


## <a name="next-steps"></a>Volgende stappen
Zie andere lessen voor het gebruik van Azure Monitor logboeken-query's:

- [Bewerkingen op tekenreeksen uitvoeren](string-operations.md)
- [Aggregatiefuncties](aggregations.md)
- [Geavanceerde aggregaties](advanced-aggregations.md)
- [JSON en gegevensstructuren](json-data-structures.md)
- [Geavanceerde query schrijven](advanced-query-writing.md)
- [Grafieken](charts.md)