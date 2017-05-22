---
title: Log Analytics gebruiken met een SQL Database-app met meerdere tenants | Microsoft Docs
description: Log Analytics (OMS) instellen en gebruiken met de Azure SQL Database-voorbeeld-app Wingtip Tickets (WTP)
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
ms.openlocfilehash: 4ff4519ca40f036d58f82993db78fe08aa7d5733
ms.contentlocale: nl-nl
ms.lasthandoff: 05/12/2017


---
# <a name="setup-and-use-log-analytics-oms-with-the-wtp-sample-saas-app"></a>Log Analytics (OMS) configureren en uitvoeren met de voorbeeld-SaaS-app WTP

In deze zelfstudie gaat u *Log Analytics ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* configureren en uitvoeren met de app WTP voor de bewaking van elastische pools en databases. Deze zelfstudie is een vervolg op de zelfstudie [Monitor performance of the WTP sample SaaS application](sql-database-saas-tutorial-performance-monitoring.md) (Prestaties bewaken met de voorbeeld-SaaS-app WTP) en laat zien hoe u met *Log Analytics* de bewakings- en waarschuwingsfuncties van Azure Portal naar een hoger plan kunt tillen. Logboek Analytics is met name geschikt voor bewaking en waarschuwingen op schaal omdat het ondersteuning biedt voor honderden pools en honderdduizenden databases. Bovendien is het een centrale bewakingsoplossing, met mogelijkheden om bewakingsfuncties van andere toepassingen en Azure-services te integreren. Deze integratie is overigens ook mogelijk voor verschillende Azure-abonnementen.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Log Analytics (OMS) installeren en configureren
> * Log Analytics gebruiken voor het bewaken van groepen en databases

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De WTP-app is geïmplementeerd. Zie [De WTP SaaS-toepassing implementeren en verkennen](sql-database-saas-tutorial.md) om dit in minder dan vijf minuten te doen
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

Zie de zelfstudie [Monitor performance of the WTP sample SaaS application](sql-database-saas-tutorial-performance-monitoring.md) (Prestaties bewaken met de voorbeeld-SaaS-app WTP) voor een beschrijving van de SaaS-scenario's en -patronen, en hoe die van invloed zijn op de vereisten voor een bewakingsoplossing.

## <a name="monitoring-and-managing-performance-with-log-analytics-oms"></a>Prestaties bewaken en beheren met Log Analytics (OMS)

Voor SQL Database zijn bewaking en waarschuwingen beschikbaar voor databases en pools. Deze ingebouwde bewakings- en waarschuwingsfuncties zijn resource-specifiek en handig voor kleine aantallen resources. Ze zijn minder geschikt voor het bewaken van grote installaties of om een geïntegreerd overzicht te bieden van verschillende resources en abonnementen.

Voor scenario's met grote volumes kan Log Analytics worden gebruikt. Dit is een afzonderlijke Azure-service die analyse biedt van verzonden diagnostische logboeken en telemetrische gegevens die zijn verzameld in een werkruimte voor logboekanalyse. In een dergelijke ruimte kunnen telemetrische gegevens van allerlei services worden verzameld om op basis hiervan query's uit te voeren en waarschuwingen te genereren. Log Analytics beschikt over een ingebouwde querytaal en hulpprogramma's voor gegevensvisualisatie voor analyse en visualisatie van operationele gegevens. De oplossing SQL Analytics biedt verschillende vooraf gedefinieerde weergaven en query's voor bewaking en waarschuwingen voor elastische pools en databases. Bovendien kunt u uw eigen ad-hocquery's toevoegen en deze desgewenst opslaan. OMS biedt ook een functie voor het ontwerpen van aangepaste weergaven.

Werkruimten en analyse-oplossingen van Log Analytics kunnen worden geopend in zowel Azure Portal als OMS. Azure Portal is het nieuwere toegangspunt, maar loopt qua functionaliteit mogelijk iets achter op de OMS-portal.

### <a name="start-the-load-generator-to-create-data-to-analyze"></a>De load-generator starten om gegevens te maken om te analyseren

1. Open **Demo-PerformanceMonitoringAndManagement.ps1** in de **PowerShell ISE**. Laat dit script open aangezien u mogelijk verschillende scenario's voor het genereren van gegevens wilt uitvoeren tijdens deze zelfstudie.
1. Als u minder dan vijf tenants hebt, richt dan een batch met tenants in om een interessantere context te bieden:
   1. Stel het volgende in: **$DemoScenario = 1,** **Batch met tenants inrichten**
   1. Druk op **F5** om het script uit te voeren.

1. Stel het volgende in: **$DemoScenario = 2,** **Belasting met normale intensiteit genereren (ongeveer 40 DTU's)**.
1. Druk op **F5** om het script uit te voeren.

## <a name="get-the-wingtip-application-scripts"></a>De scripts van de Wingtip-toepassing downloaden

De scripts en broncode van de Wingtip Tickets-toepassing zijn beschikbaar in de GitHub-opslagplaats [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). U vindt de scriptbestanden in de map [Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Download de map **Learning Modules** naar de lokale computer en behoud de mapstructuur.

## <a name="installing-and-configuring-log-analytics-and-the-azure-sql-analytics-solution"></a>Log Analytics en de oplossing Azure SQL Analytics installeren en configureren

Log Analytics is een afzonderlijke service die moet worden geconfigureerd. Log Analytics verzamelt logboekgegevens en telemetrische en metrische gegevens in een speciale werkruimte voor logboekanalyse. Een werkruimte is een resource, net als andere resources in Azure, en moet eerst worden gemaakt. Hoewel het niet verplicht is om de werkruimte te maken in dezelfde resourcegroep als de toepassing(en) die ermee worden bewaakt, is dit vaak wel het handigst. In het geval van de WTP-app betekent dit dat de werkruimte eenvoudig samen met de toepassing kan worden verwijderd door de resourcegroep te verwijderen.

1. Open ...\\Learning Modules\\Performance Monitoring and Management\\Log Analytics\\*Demo-LogAnalytics.ps1* in de **PowerShell ISE**.
1. Druk op **F5** om het script uit te voeren.

Op dit moment moet u Log Analytics kunnen openen in Azure Portal (of de OMS-portal). Het duurt een paar minuten voordat er telemetriegegevens zijn verzameld en deze worden weergegeven in de werkruimte van Log Analytics. Hoe langer u het systeem gegevens laat verzamelen, hoe interessanter de ervaring. U kunt nu misschien even iets anders gaan doen, maar controleer eerst of de load-generator nog wel actief is.


## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Pools en databases bewaken met Log Analytics en de SQL Analytics-oplossing


In deze oefening opent u Log Analytics en de OMS-portal om te kijken naar de telemetrische gegevens die worden verzameld voor de WTP-databases en -pools.

1. Blader naar [Azure Portal](https://portal.azure.com) en open Log Analytics door te klikken op Meer services en te zoeken naar Log Analytics:

   ![Log Analytics openen](media/sql-database-saas-tutorial-log-analytics/log-analytics-open.png)

1. Selecteer de werkruimte met de naam *wtploganalytics-&lt;GEBRUIKER&gt;*.

1. Selecteer **Overzicht** om de oplossing Log Analytics te openen in Azure Portal.
   ![koppeling Overzicht](media/sql-database-saas-tutorial-log-analytics/click-overview.png)

    **BELANGRIJK**: Het kan enkele minuten duren voordat de oplossing actief is. Geduld is een schone zaak.

1. Klik op de tegel Azure SQL Analytics om deze te openen.

    ![overview](media/sql-database-saas-tutorial-log-analytics/overview.png)

    ![analytics](media/sql-database-saas-tutorial-log-analytics/analytics.png)

1. De weergave op de blade van de oplossing schuift horizontaal, met een eigen schuifbalk aan de onderkant (vernieuw de blade indien nodig).

1. Bekijk de verschillende weergaven door erop te klikken of door te klikken op afzonderlijke resources. Er wordt dan een detailweergave geopend waarin u met de tijdbalk in de linkerbovenhoek kunt inzoomen op een kleiner tijdsegment. Dit kan trouwens ook door te klikken op een verticale balk. In deze weergave kunt u afzonderlijke databases of pools selecteren om te focussen op specifieke resources:

    ![grafiek](media/sql-database-saas-tutorial-log-analytics/chart.png)

1. Als u terug bent op de blade van de oplossing en helemaal naar rechts schuift, ziet u een paar opgeslagen query's. Klik op een query om deze te openen en te verkennen. U kunt experimenteren met deze query's door ze aan te passen. Interessante query's kunt u vervolgens opslaan en later gebruiken met andere resources.

1. Ga terug naar de blade met de werkruimte van Log Analytics en selecteer de OMS-portal om de oplossing daar te openen.

    ![oms](media/sql-database-saas-tutorial-log-analytics/oms.png)

1. In de OMS-portal kunt u waarschuwingen configureren. Klik op het gedeelte met waarschuwingen van de DTU-weergave van de database.

1. In de weergave Zoeken in logboeken ziet u nu een staafdiagram van de metrische gegevens die worden weergegeven.

    ![zoeken in logboeken](media/sql-database-saas-tutorial-log-analytics/log-search.png)

1. Als u op de werkbalk op Waarschuwing klikt, kunt u de configuratie van de waarschuwingen bekijken en deze desgewenst wijzigen.

    ![waarschuwingsregel toevoegen](media/sql-database-saas-tutorial-log-analytics/add-alert.png)

De bewaking en waarschuwingen in Log Analytics en OMS zijn gebaseerd op query's die worden uitgevoerd op de gegevens in de werkruimte. De waarschuwingen op de blades van afzonderlijke resources gelden daarentegen alleen voor de betreffende resource. Het is dus mogelijk om een waarschuwing te definiëren die voor alle databases geldt, zodat u niet voor elke database afzonderlijk waarschuwingen hoeft te definiëren. U kunt ook een waarschuwing maken die gebruikmaakt van een samengestelde query op meerdere resourcetypen. De uitvoering van query's wordt alleen beperkt door de gegevens die beschikbaar zijn in de werkruimte.

De kosten die in rekening worden gebracht voor Log Analytics voor SQL Database worden gebaseerd op het gegevensvolume in de werkruimte. In deze zelfstudie hebt u een gratis werkruimte gemaakt, die is beperkt tot 500 MB per dag. Zodra deze limiet is bereikt, worden er geen gegevens meer toegevoegd aan de werkruimte.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Log Analytics (OMS) installeren en configureren
> * Log Analytics gebruiken voor het bewaken van groepen en databases

[Zelfstudie over tenant-analyse](sql-database-saas-tutorial-tenant-analytics.md)

## <a name="additional-resources"></a>Aanvullende bronnen

* [Aanvullende zelfstudies die voortbouwen op de eerste implementatie van WTP (Wingtip Tickets Platform)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md)
* [OMS](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)

