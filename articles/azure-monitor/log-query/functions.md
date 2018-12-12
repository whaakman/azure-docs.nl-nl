---
title: Functies van Azure Log Analytics | Microsoft Docs
description: In dit artikel wordt beschreven hoe u functions gebruiken voor het aanroepen van een query uit een andere query in Log Analytics.
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
ms.openlocfilehash: 8f2855ed56d298ec4c6abee02dd59ce9471f0d2e
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52882417"
---
# <a name="using-functions-in-azure-monitor-log-analytics"></a>Met behulp van functies in Azure Monitor Log Analytics

> [!NOTE]
> U moet voltooien [aan de slag met de Analytics-portal](get-started-portal.md) en [aan de slag met query's](get-started-queries.md) voordat het voltooien van deze les gaat uitvoeren.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]


Voor het gebruik van een Log Analytics-query met een andere query kunt u deze opslaan als een functie. Hiermee kunt u voor het vereenvoudigen van complexe query's door ze te analyseren in delen en kunt u algemene code met meerdere query's.

## <a name="create-a-function"></a>Een functie maken

Een functie maken in Azure portal door te klikken op **opslaan** en vervolgens de informatie in de volgende tabel.

| Instelling | Beschrijving |
|:---|:---|
| Naam           | Weergavenaam voor de query in **queryverkenner**. |
| Opslaan als        | Functie |
| Functiealias | Korte naam voor het gebruik van de functie in andere query's. Mag geen spaties bevatten en moet uniek zijn. |
| Categorie       | Een categorie om te organiseren opgeslagen query's en functies in **queryverkenner**. |

> [!NOTE]
> Een functie in Log Analytics kan niet een andere functie bevatten.

> [!NOTE]
> Opslaan van een functie is mogelijk in Log Analytics-query's, maar momenteel niet voor Application Insights-query's.



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
Zie andere lessen voor het gebruik van de querytaal van Log Analytics:

- [Bewerkingen op tekenreeksen uitvoeren](string-operations.md)
- [Datum- en tijdbewerkingen](datetime-operations.md)
- [Aggregatiefuncties](aggregations.md)
- [Geavanceerde aggregaties](advanced-aggregations.md)
- [JSON en gegevensstructuren](json-data-structures.md)
- [Joins](joins.md)
- [Grafieken](charts.md)
