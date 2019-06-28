---
title: Overzicht van het logboek query's in Azure Monitor | Microsoft Docs
description: Antwoorden op veelgestelde vragen met betrekking tot logboek query's en helpt u op weg in het gebruik ervan.
services: log-analytics
author: bwren
ms.service: log-analytics
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: bwren
ms.openlocfilehash: 7605bf36c41c5b1276d29076173efd52409afaa9
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67310340"
---
# <a name="overview-of-log-queries-in-azure-monitor"></a>Overzicht van Logboeken-query's in Azure Monitor
Logboeken-query's kunnen u volledig gebruikmaken van de waarde van de gegevens die zijn verzameld [logboeken van Azure Monitor](../platform/data-platform-logs.md). Een krachtige querytaal kunt u gegevens uit meerdere tabellen samenvoegen, aggregeren van grote gegevenssets en complexe bewerkingen met minimale code uitvoeren. Kan vrijwel alle vragen worden beantwoord en analyse, zolang de ondersteunende gegevens zijn verzameld, en u begrijpt hoe u een van de juiste query uitgevoerd.

Sommige functies in Azure Monitor, zoals [insights](../insights/insights-overview.md) en [oplossingen](../insights/solutions-inventory.md) logboekgegevens verwerken zonder dat u op de onderliggende query's. Als u wilt volledig gebruikmaken van andere functies van Azure Monitor, dient u te begrijpen hoe query's zijn gebouwd en hoe u deze kunt gebruiken voor het analyseren van gegevens in Azure controleren logboeken interactief.

Gebruik dit artikel als uitgangspunt om te leren kennen logboeken-query's in Azure Monitor. Het antwoorden op veelgestelde vragen en vindt u koppelingen naar andere documentatie die meer details en geleerde lessen biedt.

## <a name="how-can-i-learn-how-to-write-queries"></a>Hoe kan ik meer informatie over het schrijven van query's?
Als u meteen in dingen wilt, kunt u beginnen met de volgende zelfstudies:

- [Aan de slag met Log Analytics in Azure Monitor](get-started-portal.md).
- [Aan de slag met Logboeken-query's in Azure Monitor](get-started-queries.md).

Zodra u de basisprincipes omlaag hebt, helpt u bij meerdere lessen met behulp van uw eigen gegevens of de gegevens van onze demo-omgeving die beginnen met: 

- [Werken met tekenreeksen in Logboeken-query's van Azure Monitor](string-operations.md)
 
## <a name="what-language-do-log-queries-use"></a>Welke taal zich aanmelden query's gebruiken?
Logboeken in Azure Monitor is gebaseerd op [Azure Data Explorer](/azure/data-explorer), en logboeken-query's zijn geschreven met behulp van de dezelfde Kusto-querytaal (KQL). Dit is een uitgebreide taal die is ontworpen om gemakkelijk te lezen en auteur, en u zou het mogelijk om deze te gebruiken met minimale begeleiding.

Zie [documentatie Azure Data Explorer KQL](/azure/kusto/query) voor volledige documentatie over KQL en naslaginformatie over de verschillende functies die beschikbaar zijn.<br>
Zie [aan de slag met Logboeken-query's in Azure Monitor](get-started-queries.md) voor een snel overzicht van de taal met gegevens uit Azure Monitor-Logboeken.
Zie [Azure Monitor log-query language verschillen](data-explorer-difference.md) voor kleine verschillen in de versie van KQL die worden gebruikt door Azure Monitor.

## <a name="what-data-is-available-to-log-queries"></a>Welke gegevens is beschikbaar voor het melden van query's?
Alle gegevens die zijn verzameld in Logboeken van Azure Monitor is beschikbaar voor het ophalen en analyseren in Logboeken-query's. Verschillende gegevensbronnen hun gegevens naar verschillende tabellen worden geschreven, maar u kunt meerdere tabellen opnemen in één query om gegevens te analyseren via meerdere bronnen. Wanneer u een query bouwen, start u door te bepalen welke tabellen bevatten de gegevens die u zoekt, dus u moet ten minste een basiskennis hebben van hoe de gegevens in Logboeken van Azure Monitor is opgebouwd.

Zie [bronnen van Azure Monitor logboeken](../platform/data-platform-logs.md#sources-of-azure-monitor-logs)voor een lijst van verschillende gegevensbronnen die Azure Monitor-logboeken te vullen.<br>
Zie [structuur van Azure Monitor logboeken](logs-structure.md) voor een uitleg van hoe de gegevens is opgebouwd.

## <a name="what-does-a-log-query-look-like"></a>Hoe een logboekquery eruit?
Een query kan worden net zo eenvoudig als naam van één tabel voor het ophalen van alle records uit deze tabel:

```Kusto
Syslog
```

Of het kan filteren op bepaalde records, samenvatten en visualiseren van de resultaten in een grafiek:

```
SecurityEvent
| where TimeGenerated > ago(7d)
| where EventID == 4625
| summarize count() by Computer, bin(TimeGenerated, 1h)
| render timechart 
```

Voor meer complexe analyse, kunt u gegevens ophalen uit meerdere tabellen met behulp van een join voor het analyseren van de resultaten samen.

```Kusto
app("ContosoRetailWeb").requests
| summarize count() by bin(timestamp,1hr)
| join kind= inner (Perf
    | summarize avg(CounterValue) 
      by bin(TimeGenerated,1hr))
on $left.timestamp == $right.TimeGenerated
```
Zelfs als u niet bekend bent met KQL, zou het mogelijk om de basislogica wordt gebruikt door deze query's ten minste te achterhalen. Ze beginnen met de naam van een tabel en voeg vervolgens meerdere opdrachten om te filteren en verwerken van die gegevens. Een query een willekeurig aantal opdrachten kunt gebruiken en kunt u complexere query's als u vertrouwd raken met de verschillende KQL opdrachten die beschikbaar zijn.

Zie [aan de slag met Logboeken-query's in Azure Monitor](get-started-queries.md) voor een zelfstudie over Logboeken-query's die de taal en de algemene functies geïntroduceerd.<br>


## <a name="what-is-log-analytics"></a>Wat is Log Analytics?
Log Analytics is het primaire hulpprogramma in de Azure-portal voor het schrijven van Logboeken-query's en interactief analyseren van de resultaten ervan. Zelfs als een query voor elders in Azure Monitor wordt gebruikt, u gewoonlijk schrijven en testen van de query eerst met behulp van Log Analytics.

Log Analytics kunt u starten vanaf verschillende plaatsen in de Azure-portal. Het bereik van de gegevens beschikbaar voor Log Analytics wordt bepaald door de manier waarop u deze start. Zie [querybereik](scope.md) voor meer informatie.

- Selecteer **logboeken** uit de **Azure Monitor** menu of **Log Analytics-werkruimten** menu.
- Selecteer **Analytics** uit de **overzicht** pagina van een Application Insights-toepassing.
- Selecteer **logboeken** in het menu van een Azure-resource.

![Log Analytics](media/log-query-overview/log-analytics.png)

Zie [aan de slag met Log Analytics in Azure Monitor](get-started-portal.md) voor een zelfstudie overzicht van Log Analytics, waarmee meerdere functies worden geïntroduceerd.

## <a name="where-else-are-log-queries-used"></a>Waar worden de logboeken-query's gebruikt?
Naast interactief werken met Logboeken-query's en de resultaten in Log Analytics, omvatten gebieden in Azure Monitor waar u query's wilt gebruiken in het volgende:

- **Regels voor waarschuwingen.** [Waarschuwingsregels](../platform/alerts-overview.md) proactief problemen van gegevens in uw werkruimte te identificeren.  De waarschuwingsregel is gebaseerd op een logboekzoekopdracht die automatisch met regelmatige tussenpozen wordt uitgevoerd.  De resultaten worden gecontroleerd om te bepalen of een waarschuwing moet worden gemaakt.
- **Dashboards.** U kunt de resultaten van elke query in vastmaken een [Azure-dashboard](../learn/tutorial-logs-dashboards.md) waarmee u logboek- en metrische gegevens bij elkaar te visualiseren en (optioneel) delen met andere Azure-gebruikers.
- **Weergaven.**  Kunt u visualisaties van gegevens moeten worden opgenomen in de Gebruikersdashboards met [Weergaveontwerper](../platform/view-designer.md).  Logboeken-query's leveren de gegevens die worden gebruikt door [tegels](../platform/view-designer-tiles.md) en [visualisatie delen](../platform/view-designer-parts.md) in elke weergave.  
- **Exporteren.**  Wanneer u logboekgegevens van Azure Monitor in Excel importeert of [Power BI](../platform/powerbi.md), maakt u een logboekquery voor het definiëren van de gegevens te exporteren.
- **PowerShell.** U kunt een PowerShell-script uitvoeren vanaf een opdrachtregel of een Azure Automation-runbook die gebruikmaakt van [Get-AzOperationalInsightsSearchResults](/powershell/module/az.operationalinsights/get-azoperationalinsightssearchresult) om op te halen van logboekgegevens van Azure Monitor.  Deze cmdlet is vereist voor een query om te bepalen van de gegevens moeten worden opgehaald.
- **API voor Azure Monitor-Logboeken.**  De [logboeken-API van Azure Monitor](../platform/alerts-overview.md) kan een client REST-API om op te halen van logboekgegevens uit de werkruimte.  De API-aanvraag bevat een query die wordt uitgevoerd op Azure Monitor om te bepalen van de gegevens moeten worden opgehaald.


## <a name="next-steps"></a>Volgende stappen
- Doorloop een [zelfstudie over het gebruik van Log Analytics in Azure portal](get-started-portal.md).
- Doorloop een [zelfstudie over het schrijven van query's](get-started-queries.md).
