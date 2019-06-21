---
title: Functies in Logboeken-query's van Azure Monitor | Microsoft Docs
description: In dit artikel wordt beschreven hoe u functies voor het aanroepen van een query uit een andere logboekquery in Azure Monitor gebruiken.
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
ms.openlocfilehash: 4b3116230a085bfbb9a6139fbada4179d802bf5e
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296073"
---
# <a name="using-functions-in-azure-monitor-log-queries"></a>Met behulp van functies in Azure Monitor logboeken-query 's

Voor het gebruik van een logboekquery met een andere query kunt u deze opslaan als een functie. Hiermee kunt u voor het vereenvoudigen van complexe query's door ze te analyseren in delen en kunt u algemene code met meerdere query's.

## <a name="create-a-function"></a>Een functie maken

Een functie maken met Log Analytics in Azure portal door te klikken op **opslaan** en vervolgens de informatie in de volgende tabel.

| Instelling | Description |
|:---|:---|
| Name           | Weergavenaam voor de query in **queryverkenner**. |
| Opslaan als        | Function |
| Functiealias | Korte naam voor het gebruik van de functie in andere query's. Mag geen spaties bevatten en moet uniek zijn. |
| Category       | Een categorie om te organiseren opgeslagen query's en functies in **queryverkenner**. |

> [!NOTE]
> Een functie in Azure Monitor kan niet een andere functie bevatten.

> [!NOTE]
> Opslaan van een functie is mogelijk in Azure Monitor logboeken-query's, maar momenteel niet voor Application Insights-query's.



## <a name="use-a-function"></a>Een functie gebruiken
Een functie wilt gebruiken, met inbegrip van de alias in een andere query. Het kan worden gebruikt als elke andere tabel.

## <a name="example"></a>Voorbeeld
De volgende voorbeeldquery retourneert alle ontbrekende beveiligingsupdates gerapporteerd in de laatste dag. Deze query opslaan als een functie met de alias _security_updates_last_day_. 

```Kusto
Update
| where TimeGenerated > ago(1d) 
| where Classification == "Security Updates" 
| where UpdateState == "Needed"
```

Maken van een andere query's en naslaginformatie over de _security_updates_last_day_ functie om te zoeken naar benodigde beveiligingsupdates met betrekking tot SQL.

```Kusto
security_updates_last_day | where Title contains "SQL"
```

## <a name="next-steps"></a>Volgende stappen
Zie andere lessen voor het schrijven van Azure Monitor logboeken-query's:

- [Bewerkingen op tekenreeksen uitvoeren](string-operations.md)
- [Datum- en tijdbewerkingen](datetime-operations.md)
- [Aggregatiefuncties](aggregations.md)
- [Geavanceerde aggregaties](advanced-aggregations.md)
- [JSON en gegevensstructuren](json-data-structures.md)
- [Joins](joins.md)
- [Grafieken](charts.md)
