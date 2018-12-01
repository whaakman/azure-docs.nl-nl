---
title: Azure Monitor Log Analytics-querytaal | Microsoft Docs
description: Verwijzingen naar resources voor het leren over het schrijven van query's in Log Analytics.
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
ms.date: 10/29/2018
ms.author: bwren
ms.openlocfilehash: 4ef1e302a305f683155139063891b838d8144008
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52680460"
---
# <a name="log-analytics-query-language"></a>Meld u Analytics-querytaal
Log Analytics biedt log verzamelen en analyseren voor Azure Monitor. Het gebouwd op Azure Data Explorer en gebruikt een versie van dezelfde querytaal. De [query language-documentatie Azure Data Explorer](/azure/kusto/query) heeft alle van de details voor de taal en moet de primaire bron voor het schrijven van Log Analytics-query's. Deze pagina bevat koppelingen naar andere bronnen voor het leren over het schrijven van query's en over de verschillen met de Log Analytics-implementatie van de taal.

## <a name="getting-started"></a>Aan de slag

- [Aan de slag met Log Analytics in Azure portal](get-started-analytics-portal.md) is een les voor het schrijven van query's en werken met resultaten in de Azure-portal.
-  [Aan de slag met query's in Log Analytics](get-started-queries.md) is een les voor het schrijven van query's met behulp van Log Analytics-gegevens.

## <a name="concepts"></a>Concepten
- [Analyseren van Log Analytics-gegevens in Azure Monitor](../log-analytics-queries.md) geeft een kort overzicht van het logboek een query uitgevoerd en wordt beschreven hoe u Log Analytics-gegevens is opgebouwd.
- [Weergeven en analyseren van gegevens in Log Analytics](../log-analytics-log-search-portals.md) wordt uitgelegd van de portals waar u maakt en Log Analytics-query's uitvoeren.

## <a name="reference"></a>Referentie

- [Query language reference](/azure/kusto/query) is de volledige Naslaggids voor de querytaal van Data Explorer.
- [Meld u Analytics query language verschillen](data-explorer-difference.md) worden de verschillen tussen versies van de querytaal van Data Explorer beschreven.
- [Standaardeigenschappen in Log Analytics-records](../log-analytics-standard-properties.md) bevat eigenschappen die standaard voor alle Log Analytics-gegevens zijn.
- [Uitvoeren van meerdere bronnen zoekopdrachten in Logboeken in Log Analytics](../log-analytics-cross-workspace-search.md) wordt beschreven hoe u query's die gebruikmaken van gegevens uit meerdere Log Analytics-werkruimten en Application Insights-toepassingen te schrijven.


## <a name="examples"></a>Voorbeelden

- [Meld u voorbeelden van Analytics](examples.md) vindt u voorbeelden van query's met behulp van Log Analytics-gegevens.



## <a name="lessons"></a>Lessen

- [Werken met tekenreeksen in Log Analytics-query's](string-operations.md) wordt beschreven hoe u wilt werken met tekenreeksgegevens.
- [Werken met datum-/ tijdwaarden in Log Analytics-query's](datetime-operations.md) wordt beschreven hoe u wilt werken met gegevens van de datum en tijd. 
- [Aggregaties in Log Analytics-query's](aggregations.md) en [geavanceerde aggregaties in Log Analytics-query's](advanced-aggregations.md) wordt beschreven hoe u samengevoegd en overzichtelijk weergegeven gegevens.
- [Joins in Log Analytics-query's](joins.md) wordt beschreven hoe u gegevens uit meerdere tabellen samenvoegen.
- [Werken met JSON en gegevensstructuren in Log Analytics-query's](json-data-structures.md) wordt beschreven hoe u json-gegevens parseren.
- [Geavanceerde query's in Log Analytics schrijven](advanced-query-writing.md) beschrijft strategieën voor het maken van complexe query's en hergebruik van code.
- [Het maken van grafieken en diagrammen van Log Analytics-query's](charts.md) wordt beschreven hoe u voor het visualiseren van gegevens uit een query.

## <a name="cheatsheets"></a>Cheatsheets

-  [SQL voor het referentiemateriaal voor querytaal Log Analytics](sql-cheatsheet.md) biedt ondersteuning voor gebruikers die al bekend met SQL bent.
-  [Splunk voor referentiemateriaal voor querytaal Log Analytics](sql-cheatsheet.md) biedt ondersteuning voor gebruikers die al bekend met Splunk bent.
 
## <a name="next-steps"></a>Volgende stappen

- Toegang tot de volledige [referentiedocumentatie voor de Data Explorer-querytaal](/azure/kusto/query/).