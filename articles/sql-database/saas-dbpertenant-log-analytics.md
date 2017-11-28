---
title: Log Analytics gebruiken met een SQL Database-app met meerdere tenants | Microsoft Docs
description: Instellen en gebruiken van logboekanalyse (OMS) met een multitenant SaaS van Azure SQL Database-app
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: billgib; sstein
ms.openlocfilehash: 48e8eb91a5febcc1109bee3404bb534bd0391f88
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="set-up-and-use-log-analytics-oms-with-a-multi-tenant-azure-sql-database-saas-app"></a>Instellen en gebruiken van logboekanalyse (OMS) met een multitenant SaaS van Azure SQL Database-app

In deze zelfstudie maakt u instellen en gebruiken *logboekanalyse ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* voor het bewaken van elastische pools en databases. Deze zelfstudie bouwt voort op de [prestatiebewaking en beheer zelfstudie](saas-dbpertenant-performance-monitoring.md). Er wordt weergegeven hoe gebruiken *logboekanalyse* voor het verbeteren van de bewaking en waarschuwingen die in de Azure portal. Log Analytics is geschikt voor bewaking en waarschuwingen op grote schaal omdat honderden pools en honderden of duizenden databases ondersteund. Bovendien is het een centrale bewakingsoplossing, met mogelijkheden om bewakingsfuncties van andere toepassingen en Azure-services te integreren. Deze integratie is overigens ook mogelijk voor verschillende Azure-abonnementen.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Log Analytics (OMS) installeren en configureren
> * Log Analytics gebruiken voor het bewaken van groepen en databases

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De app Wingtip Tickets SaaS Database Per Tenant wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de toepassing Wingtip Tickets SaaS Database Per Tenant verkennen](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

Zie de zelfstudie [Monitor performance of the WTP sample SaaS application](saas-dbpertenant-performance-monitoring.md) (Prestaties bewaken met de voorbeeld-SaaS-app WTP) voor een beschrijving van de SaaS-scenario's en -patronen, en hoe die van invloed zijn op de vereisten voor een bewakingsoplossing.

## <a name="monitoring-and-managing-performance-with-log-analytics-oms"></a>Prestaties bewaken en beheren met Log Analytics (OMS)

Voor SQL Database zijn bewaking en waarschuwingen beschikbaar voor databases en pools. Deze ingebouwde bewakings- en waarschuwingsfuncties zijn resource-specifiek en handig voor kleine aantallen resources. Ze zijn minder geschikt voor het bewaken van grote installaties of om een geïntegreerd overzicht te bieden van verschillende resources en abonnementen.

Voor scenario's met grote volumes kan Log Analytics worden gebruikt. Dit is een afzonderlijke Azure-service die analyse biedt van verzonden diagnostische logboeken en telemetrische gegevens die zijn verzameld in een werkruimte voor logboekanalyse. In een dergelijke ruimte kunnen telemetrische gegevens van allerlei services worden verzameld om op basis hiervan query's uit te voeren en waarschuwingen te genereren. Log Analytics beschikt over een ingebouwde querytaal en hulpprogramma's voor gegevensvisualisatie voor analyse en visualisatie van operationele gegevens. De SQL-Analytics-oplossing biedt verschillende vooraf gedefinieerde elastische pool en database bewaking en waarschuwingen, weergaven en query's en kunt u uw eigen ad-hocquery's toevoegen en sla ze indien nodig. OMS biedt ook een functie voor het ontwerpen van aangepaste weergaven.

Werkruimten en analyse-oplossingen van Log Analytics kunnen worden geopend in zowel Azure Portal als OMS. Azure Portal is het nieuwere toegangspunt, maar loopt qua functionaliteit mogelijk iets achter op de OMS-portal.

### <a name="create-data-by-starting-the-load-generator"></a>Maken van gegevens op basis van de load-generator 

1. In de **PowerShell ISE**Open **Demo PerformanceMonitoringAndManagement.ps1**. Laat dit script open aangezien u mogelijk verschillende scenario's voor het genereren van gegevens wilt uitvoeren tijdens deze zelfstudie.
1. Als u minder dan vijf tenants hebt, bewaking inrichten van een batch te bieden een meer interessante tenants context:
   1. Stel het volgende in: **$DemoScenario = 1,** **Batch met tenants inrichten**
   1. Het script wordt uitgevoerd, drukt u op **F5**.

1. Stel **$DemoScenario** = 2, **genereren normale intensiteit laden (ongeveer 40 DTU)**.
1. Het script wordt uitgevoerd, drukt u op **F5**.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>De toepassingsscripts Wingtip Tickets SaaS Database Per Tenant ophalen

De scripts Wingtip Tickets SaaS multitenant Database en de broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de scripts Wingtip Tickets SaaS deblokkeren.

## <a name="installing-and-configuring-log-analytics-and-the-azure-sql-analytics-solution"></a>Log Analytics en de oplossing Azure SQL Analytics installeren en configureren

Log Analytics is een afzonderlijke service die moet worden geconfigureerd. Log Analytics verzamelt logboekgegevens en telemetrische en metrische gegevens in een speciale werkruimte voor logboekanalyse. Een werkruimte is een resource, net als andere resources in Azure, en moet eerst worden gemaakt. Hoewel het niet verplicht is om de werkruimte te maken in dezelfde resourcegroep als de toepassing(en) die ermee worden bewaakt, is dit vaak wel het handigst. Hierdoor kunnen de werkruimte om eenvoudig met de toepassing worden verwijderd door het verwijderen van de resourcegroep voor de Database Wingtip Tickets SaaS Per Tenant SaaS-app.

1. In de **PowerShell ISE**open... \\Learning-Modules\\prestatiebewaking en beheer\\Meld Analytics\\*Demo LogAnalytics.ps1*.
1. Het script wordt uitgevoerd, drukt u op **F5**.

U moet op dit moment kunnen openen logboekanalyse in de Azure-portal (of de OMS-portal). Het duurt enkele minuten duren voordat de telemetrie moeten worden verzameld in de werkruimte voor logboekanalyse en zichtbaar worden. Hoe langer laat u het systeem verzamelen van gegevens de interessanter de ervaring is. U kunt nu misschien even iets anders gaan doen, maar controleer eerst of de load-generator nog wel actief is.


## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Pools en databases bewaken met Log Analytics en de SQL Analytics-oplossing


Open in deze oefening Log Analytics en de OMS-portal om te kijken naar de telemetrie die worden verzameld voor de databases en pools.

1. Blader naar [Azure Portal](https://portal.azure.com) en open Log Analytics door te klikken op Meer services en te zoeken naar Log Analytics:

   ![Log Analytics openen](media/saas-dbpertenant-log-analytics/log-analytics-open.png)

1. Selecteer de werkruimte met de naam *wtploganalytics-&lt;GEBRUIKER&gt;*.

1. Selecteer **Overzicht** om de oplossing Log Analytics te openen in Azure Portal.
   ![koppeling Overzicht](media/saas-dbpertenant-log-analytics/click-overview.png)

    > [!IMPORTANT]
    > Het duurt een paar minuten voordat de oplossing is actief. Geduld is een schone zaak.

1. Klik op de tegel Azure SQL Analytics om deze te openen.

    ![overview](media/saas-dbpertenant-log-analytics/overview.png)

    ![analytics](media/saas-dbpertenant-log-analytics/analytics.png)

1. De weergave op de blade van de oplossing schuift horizontaal, met een eigen schuifbalk aan de onderkant (vernieuw de blade indien nodig).

1. Bekijk de verschillende weergaven door erop te klikken of door te klikken op afzonderlijke resources. Er wordt dan een detailweergave geopend waarin u met de tijdbalk in de linkerbovenhoek kunt inzoomen op een kleiner tijdsegment. Dit kan trouwens ook door te klikken op een verticale balk. In deze weergave kunt u afzonderlijke databases of pools selecteren om te focussen op specifieke resources:

    ![grafiek](media/saas-dbpertenant-log-analytics/chart.png)

1. Als u terug bent op de blade van de oplossing en helemaal naar rechts schuift, ziet u een paar opgeslagen query's. Klik op een query om deze te openen en te verkennen. U kunt experimenteren met het wijzigen van deze en eventuele interessante query's opslaan u produceren, die vervolgens opnieuw openen en gebruiken met andere bronnen.

1. Ga terug naar de blade met de werkruimte van Log Analytics en selecteer de OMS-portal om de oplossing daar te openen.

    ![oms](media/saas-dbpertenant-log-analytics/oms.png)

1. In de OMS-portal kunt u waarschuwingen configureren. Klik op het gedeelte met waarschuwingen van de DTU-weergave van de database.

1. In de weergave Zoeken in logboeken ziet u nu een staafdiagram van de metrische gegevens die worden weergegeven.

    ![zoeken in logboeken](media/saas-dbpertenant-log-analytics/log-search.png)

1. Als u op de waarschuwing in de werkbalk klikt, kunt u kan zien van de configuratie van de waarschuwing en deze kunt wijzigen.

    ![waarschuwingsregel toevoegen](media/saas-dbpertenant-log-analytics/add-alert.png)

De bewaking en waarschuwingen in Log Analytics en OMS zijn gebaseerd op query's die worden uitgevoerd op de gegevens in de werkruimte. De waarschuwingen op de blades van afzonderlijke resources gelden daarentegen alleen voor de betreffende resource. Het is dus mogelijk om een waarschuwing te definiëren die voor alle databases geldt, zodat u niet voor elke database afzonderlijk waarschuwingen hoeft te definiëren. U kunt ook een waarschuwing maken die gebruikmaakt van een samengestelde query op meerdere resourcetypen. De uitvoering van query's wordt alleen beperkt door de gegevens die beschikbaar zijn in de werkruimte.

De kosten die in rekening worden gebracht voor Log Analytics voor SQL Database worden gebaseerd op het gegevensvolume in de werkruimte. In deze zelfstudie hebt u een gratis werkruimte gemaakt, die is beperkt tot 500 MB per dag. Zodra deze limiet is bereikt, worden er geen gegevens meer toegevoegd aan de werkruimte.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Log Analytics (OMS) installeren en configureren
> * Log Analytics gebruiken voor het bewaken van groepen en databases

[Zelfstudie over tenant-analyse](saas-dbpertenant-log-analytics.md)

## <a name="additional-resources"></a>Aanvullende bronnen

* [Aanvullende zelfstudies waarin voort op de eerste implementatie van Wingtip Tickets SaaS Database Per Tenant bouwen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md)
* [OMS](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)
