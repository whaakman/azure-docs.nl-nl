---
title: Ad-hoc Analytics-query&quot;s uitvoeren voor meerdere Azure SQL-databases | Microsoft Docs
description: Ad-hoc Analytics-query&quot;s uitvoeren voor meerdere databases in een toepassing met meerdere tenants
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: dd41e7f1f131f6c18e03d2434982c3d681342b8b
ms.contentlocale: nl-nl
ms.lasthandoff: 05/12/2017


---
# <a name="run-ad-hoc-analytics-queries-across-all-wtp-tenants"></a>Ad-hoc Analytics-query's uitvoeren voor alle WTP-tenants

In deze zelfstudie maakt u een ad-hoc Analytics-database en voert u meerdere query’s uit voor alle tenants. Met deze query's kunt u inzichten ophalen uit dagelijkse operationele gegevens van de WTP-app.

De WTP-app gebruikt [Elastische query](sql-database-elastic-query-overview.md) samen met een Analytics-database om ad-hoc Analytics-query's uit te voeren (voor meerdere tenants).


In deze zelfstudie leert u het volgende:

> [!div class="checklist"]

> * Een ad-hoc Analytics-database implementeren
> * Gedistribueerde query's uitvoeren voor alle databases van de tenant



U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De WTP-app is geïmplementeerd. Zie [De WTP SaaS-toepassing implementeren en verkennen](sql-database-saas-tutorial.md) om dit in minder dan vijf minuten te doen
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.


## <a name="ad-hoc-analytics-pattern"></a>Ad-hoc Analytics-patroon

Een van de grote voordelen van SaaS-toepassingen is het gebruik van de grote hoeveelheid tenantgegevens die u centraal hebt opgeslagen in de cloud. Gebruik deze gegevens om inzicht te krijgen in de werking en het gebruik van toepassingen, tenants, de gebruikers hiervan, en de voorkeuren en het gedrag van deze gebruikers. Met de verkregen inzichten kunt u functies verder ontwikkelen, bruikbaarheid verbeteren en andere investeringen doen in uw apps en services.

U kunt gemakkelijk toegang tot deze gegevens krijgen in een database met meerdere tenants, maar dit is niet zo gemakkelijk als de gegevens op schaal zijn verdeeld over misschien wel duizenden databases. Eén aanpak is het gebruik van elastische query's, wat het ad-hoc uitvoeren van query's mogelijk maakt over een gedistribueerde set databases met hetzelfde schema. Elastische query's gebruiken één *hoofd*database waarin externe tabellen zijn gedefinieerd die de tabellen weerspiegelen in de gedistribueerde (tenant)databases. Query's die naar deze hoofddatabase worden verzonden, worden gecompileerd tot een gedistribueerd queryplan, waarbij delen van de query, wanneer nodig, naar de onderliggende tenantdatabases worden gepusht. Elastische query's maken gebruik van een shard-kaart in de catalogusdatabase voor de locatie van de tenantdatabases. Installatie en query-uitvoering zijn eenvoudig. Er wordt gebruikgemaakt van normale T-SQL en ondersteuning geboden voor ad-hoc uitvoeren van query's vanuit hulpprogramma's zoals Power BI en Excel.

## <a name="get-the-wingtip-application-scripts"></a>De scripts van de Wingtip-toepassing downloaden

De scripts en broncode van de Wingtip Tickets-toepassing zijn beschikbaar in de GitHub-opslagplaats [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). U vindt de scriptbestanden in de map [Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Download de map **Learning Modules** naar de lokale computer en behoud de mapstructuur.

## <a name="deploy-the-database-used-for-ad-hoc-analytics-queries"></a>De database implementeren die wordt gebruikt voor ad-hoc Analytics-query's

Hier implementeert u de ad-hoc Analytics-database die het schema bevat dat wordt gebruikt voor het uitgeven van ad-hocquery's die van toepassing zijn op alle tenantdatabases.

1. Open ...\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalytics.ps1* in *PowerShell ISE* en stel de volgende waarden in:
   * **$DemoScenario** = 2, **Ad-hoc Analytics-database implementeren**.

1. Druk op **F5** om het script uit te voeren en maak een nieuwe SQL-database voor ad-hocanalyse en voeg deze toe aan de WTP-catalogus. De tabellen TicketPurchases, Tickets en Venues worden toegevoegd als externe tabellen waarop een query kan worden uitgevoerd.
   Het is niet erg als u hier deze waarschuwing krijgt: *De RCP-server is niet beschikbaar*.


U hebt nu een *adhocanalytics*-database die kan worden gebruikt om gedistribueerde query's uit te voeren en inzichten te krijgen van alle tenants.

## <a name="run-ad-hoc-analytics-queries"></a>Ad-hoc Analytics-query's uitvoeren

Hier voert u ad-hoc Analytics-query's uit om tenantinzichten in de WTP-toepassing zichtbaar te maken.

1. Open ...\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalyticsQueries.sql* in SSMS.
1. Zorg ervoor dat u bent verbonden met de **adhocanalytics**-database
1. Selecteer de afzonderlijke query die u wilt uitvoeren, en druk op **F5**:

    ![query](media/sql-database-saas-tutorial-adhoc-analytics/query.png)




## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * Een ad-hoc Analytics-database implementeren
> * Gedistribueerde query's uitvoeren voor alle databases van de tenant

[Zelfstudie Log Analytics (OMS)](sql-database-saas-tutorial-log-analytics.md)

## <a name="additional-resources"></a>Aanvullende bronnen

* [Aanvullende zelfstudies die voortbouwen op de eerste implementatie van WTP (Wingtip Tickets Platform)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Elastische query](sql-database-elastic-query-overview.md)

