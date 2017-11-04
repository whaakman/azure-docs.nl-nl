---
title: Meld u zoekopdrachten in Azure Log Analytics | Microsoft Docs
description: U hebt een logboek zoekopdracht gegevens ophalen van logboekanalyse nodig.  Dit artikel wordt beschreven hoe nieuwe logboek zoekopdrachten worden gebruikt in Log Analytics en concepten die u begrijpen moet voordat deze is gemaakt.
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: bwren
ms.openlocfilehash: 5f040d1480433ccf4c0b2b22c0cf1e25a7151d74
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2017
---
# <a name="understanding-log-searches-in-log-analytics"></a>Understanding logboek zoekt in Log Analytics

U hebt een logboek zoekopdracht gegevens ophalen van logboekanalyse nodig.  Of u van gegevens in de portal analyseren bent, wordt configureert een waarschuwingsregel te worden gesteld van een bepaalde voorwaarde of bij het ophalen van gegevens met behulp van de Log Analytics-API u een zoekopdracht logboek om de gegevens die u wilt geven.  Dit artikel wordt beschreven hoe logboek zoekopdrachten worden gebruikt in Log Analytics en concepten die u begrijpen moet voordat deze is gemaakt. Zie de [Vervolgstappen](#next-steps) sectie voor meer informatie over het maken en bewerken van logboek zoekopdrachten en verwijzingen voor de query language.

## <a name="where-log-searches-are-used"></a>Waar logboek zoekopdrachten worden gebruikt

De verschillende manieren logboek zoekopdrachten in logboekanalyse te gebruiken, omvatten het volgende:

- **Portals.** U kunt interactieve analyse van gegevens in de opslagplaats in Azure portal uitvoeren of de [Advanced Analytics-portal](https://go.microsoft.com/fwlink/?linkid=856587).  Hiermee kunt u uw query bewerken en analyseer de resultaten in verschillende indelingen en visualisaties.  De meeste query's die u maakt in een van de portals wordt gestart en vervolgens gekopieerd zodra u controleren of deze werkt zoals verwacht.
- **Regels voor waarschuwingen.** [Waarschuwing regels](log-analytics-alerts.md) proactief problemen van gegevens in uw werkruimte.  Elke waarschuwingsregel is gebaseerd op een logboek zoekopdracht die automatisch wordt uitgevoerd met regelmatige tussenpozen.  De resultaten worden gecontroleerd om na te gaan als er een waarschuwing moet worden gemaakt.
- **Weergaven.**  U kunt visualisaties van gegevens moeten worden opgenomen in Gebruikersdashboards met maken [ontwerper](log-analytics-view-designer.md).  Logboek zoekopdrachten bieden de gegevens die worden gebruikt door [tegels](log-analytics-view-designer-tiles.md) en [visualisatie delen](log-analytics-view-designer-parts.md) in elke weergave.  U kunt inzoomen op uit visualisatie delen de zoekpagina van logboek voor verdere analyse van de gegevens.
- **Exporteren.**  Wanneer u gegevens uit de werkruimte voor logboekanalyse naar Excel exporteren of [Power BI](log-analytics-powerbi.md), hebt u een zoekopdracht logboek om te definiëren van de gegevens te exporteren.
- **PowerShell.** U kunt een PowerShell-script uitvoeren vanaf een opdrachtregel of een Azure Automation-runbook die gebruikmaakt van [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightssearchresults?view=azurermps-4.0.0) gegevens ophalen van logboekanalyse.  Deze cmdlet is vereist voor een query voor het bepalen van de gegevens moeten worden opgehaald.
- **Log Analytics-API.**  De [Log Analytics-API van zoekservice Meld](log-analytics-log-search-api.md) kunnen een REST-API-clientcomputers gegevens ophalen van de werkruimte.  De API-aanvraag bevat een query die wordt uitgevoerd op Log Analytics om te bepalen van de gegevens moeten worden opgehaald.

![Logboek zoekopdrachten](media/log-analytics-log-search-new/log-search-overview.png)

## <a name="how-log-analytics-data-is-organized"></a>De structuur van Log Analytics-gegevens
Als u een query bouwen, begint u met het bepalen van welke tabellen bevatten de gegevens die u zoekt. Elke [gegevensbron](log-analytics-data-sources.md) en [oplossing](../operations-management-suite/operations-management-suite-solutions.md) gegevens opslaat in de speciale tabellen in de werkruimte voor logboekanalyse.  Documentatie voor elke gegevensbron en de oplossing bevat de naam van het gegevenstype dat wordt gemaakt en een beschrijving van elk van de eigenschappen ervan.  Groot aantal query's moet alleen gegevens uit een enkele tabellen, maar anderen mogelijk een groot aantal opties gebruiken om gegevens uit meerdere tabellen.

![Tabellen](media/log-analytics-log-search-new/queries-tables.png)


## <a name="writing-a-query"></a>Een query schrijven
De kern van logboek zoekopdrachten in logboekanalyse is [een uitgebreide querytaal](https://docs.loganalytics.io/) waarmee u ophalen en analyseren van gegevens uit de opslagplaats in tal van manieren.  Deze zelfde querytaal wordt gebruikt voor [Application Insights](../application-insights/app-insights-analytics.md).  Leren hoe u kunt een query schrijven is essentieel voor het maken van logboek zoekopdrachten in logboekanalyse.  U doorgaans begint met een eenvoudige query's en vervolgens de voortgang voor het gebruik van meer geavanceerde functies zoals steeds ingewikkelder worden uw vereisten.

De basisstructuur van een query is een brontabel gevolgd door een reeks operators gescheiden door een sluisteken `|`.  U kunt keten van meerdere operators voor het verfijnen van de gegevens en geavanceerde functies uitvoeren.

Stel dat u wilt zoeken naar de top tien computers met de meeste foutgebeurtenissen gedurende de afgelopen dag.

    Event
    | where (EventLevelName == "Error")
    | where (TimeGenerated > ago(1days))
    | summarize ErrorCount = count() by Computer
    | top 10 by ErrorCount desc

Of misschien wilt u zoeken naar computers die nog niet als een heartbeat in de laatste dag hebben.

    Heartbeat
    | where TimeGenerated > ago(7d)
    | summarize max(TimeGenerated) by Computer
    | where max_TimeGenerated < ago(1d)  

Hoe zit een lijndiagram met het processorgebruik voor elke computer in de afgelopen week?

    Perf
    | where ObjectName == "Processor" and CounterName == "% Processor Time"
    | where TimeGenerated  between (startofweek(ago(7d)) .. endofweek(ago(7d)) )
    | summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5min)
    | render timechart    

U kunt van deze voorbeelden snel zien dat ongeacht het type gegevens waarmee u werkt met de structuur van de query vergelijkbaar is.  U kunt opsplitsen in afzonderlijke stappen, waarbij de resulterende gegevens van een opdracht via de pijplijn wordt verzonden naar de volgende opdracht.

U kunt ook gegevens opvragen via Log Analytics-werkruimten binnen uw abonnement.

    union Update, workspace("contoso-workspace").Update
    | where TimeGenerated >= ago(1h)
    | summarize dcount(Computer) by Classification 


Zie voor volledige documentatie over de Azure Log Analytics query language inclusief zelfstudies en Naslaggids voor de [Azure Log Analytics query language-documentatie](https://docs.loganalytics.io/).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [portals die u gebruikt voor het maken en bewerken Meld zoekopdrachten](log-analytics-log-search-portals.md).
- Bekijk een [zelfstudie over het schrijven van query's](log-analytics-tutorial-viewdata.md) met de nieuwe querytaal.
