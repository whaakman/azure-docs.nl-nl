---
title: Azure Monitor logboeken-query's | Microsoft Docs
description: Verwijzingen naar resources voor het leren over het schrijven van Logboeken-query's in Azure Monitor.
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
ms.date: 01/11/2019
ms.author: bwren
ms.openlocfilehash: 01d452f97134093e9b40b6ec32f1c9fed41564aa
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56343048"
---
# <a name="azure-monitor-log-queries"></a>Azure Monitor logboeken-query 's
Logboeken in Azure Monitor zijn gebouwd op Azure Data Explorer en logboeken-query's van Azure Monitor gebruikt een versie van dezelfde Kusto-query-taal. De [Kusto-query language-documentatie](/azure/kusto/query) heeft alle van de details voor de taal en moet de primaire bron voor het schrijven van Azure Monitor logboeken-query's. Deze pagina bevat koppelingen naar andere bronnen voor het leren over het schrijven van query's en over de verschillen met de Azure Monitor-implementatie van de taal.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="getting-started"></a>Aan de slag

- [Aan de slag met Azure Monitor log-analytics](get-started-portal.md) is een les voor het schrijven van query's en werken met resultaten in de Azure-portal.
- [Aan de slag met Azure Monitor logboeken-query's](get-started-queries.md) is een les voor het schrijven van query's met behulp van Azure Monitor-logboekgegevens.

## <a name="concepts"></a>Concepten
- [Analyseren van logboekgegevens in Azure Monitor](../../azure-monitor/log-query/log-query-overview.md) geeft een kort overzicht van het logboek een query uitgevoerd en wordt beschreven hoe de logboekgegevens van Azure Monitor is opgebouwd.
- [Weergeven en analyseren van logboekgegevens in Azure Monitor](../../azure-monitor/log-query/portals.md) wordt uitgelegd van de portals waar u maken en uitvoeren van Logboeken-query's.

## <a name="reference"></a>Referentie

- [Query language reference](/azure/kusto/query) is de volledige Naslaggids voor de Kusto-query-taal.
- [Azure Monitor log-query language verschillen](data-explorer-difference.md) beschrijving van de verschillen tussen versies van de Kusto-query-taal.
- [Standaard-eigenschappen in Azure Monitor logboekregistratie van records](../../azure-monitor/platform/log-standard-properties.md) bevat eigenschappen die standaard voor alle Azure Monitor-logboekgegevens zijn.
- [Meerdere bronnen logboeken-query's uitvoeren in Azure Monitor](../../azure-monitor/log-query/cross-workspace-query.md) wordt beschreven hoe u Logboeken-query's die gebruikmaken van gegevens uit meerdere Log Analytics-werkruimten en Application Insights-toepassingen schrijven.


## <a name="examples"></a>Voorbeelden

- [Azure Monitor log-queryvoorbeelden](examples.md) vindt u voorbeelden van query's met behulp van Azure Monitor-logboekgegevens.



## <a name="lessons"></a>Lessen

- [Werken met tekenreeksen in Logboeken-query's van Azure Monitor](string-operations.md) wordt beschreven hoe u wilt werken met tekenreeksgegevens.
- [Werken met datum-/ tijdwaarden in Logboeken-query's van Azure Monitor](datetime-operations.md) wordt beschreven hoe u wilt werken met gegevens van de datum en tijd. 
- [Aggregaties in Azure Monitor query's bijgehouden](aggregations.md) en [geavanceerde aggregaties in Logboeken-query's van Azure Monitor](advanced-aggregations.md) wordt beschreven hoe u samengevoegd en overzichtelijk weergegeven gegevens.
- [Joins in Logboeken-query's van Azure Monitor](joins.md) wordt beschreven hoe u gegevens uit meerdere tabellen samenvoegen.
- [Werken met JSON en gegevensstructuren in Azure Monitor logboeken-query's](json-data-structures.md) wordt beschreven hoe u json-gegevens parseren.
- [Schrijven van geavanceerde query's bijgehouden in Azure Monitor](advanced-query-writing.md) beschrijft strategieën voor het maken van complexe query's en hergebruik van code.
- [Het maken van grafieken en diagrammen van Azure Monitor logboeken-query's](charts.md) wordt beschreven hoe u voor het visualiseren van gegevens uit een logbestand-query.

## <a name="cheatsheets"></a>Cheatsheets

-  [SQL Azure Monitor log-query](sql-cheatsheet.md) biedt ondersteuning voor gebruikers die al bekend met SQL bent.
-  [Splunk naar Azure Monitor log-query](sql-cheatsheet.md) biedt ondersteuning voor gebruikers die al bekend met Splunk bent.
 
## <a name="next-steps"></a>Volgende stappen

- Toegang tot de volledige [referentiedocumentatie voor de Kusto-query-taal](/azure/kusto/query/).