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
ms.openlocfilehash: 90510520e5f6bbfa8aea4026d7437a4a4881984f
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2018
---
# <a name="set-up-and-use-log-analytics-oms-with-a-multi-tenant-azure-sql-database-saas-app"></a>Instellen en gebruiken van logboekanalyse (OMS) met een multitenant SaaS van Azure SQL Database-app

In deze zelfstudie maakt u instellen en gebruiken *logboekanalyse ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* voor het bewaken van elastische pools en databases. Deze zelfstudie bouwt voort op de [prestatiebewaking en beheer zelfstudie](saas-dbpertenant-performance-monitoring.md). Er wordt weergegeven hoe gebruiken *logboekanalyse* voor het verbeteren van de bewaking en waarschuwingen die in de Azure portal. Log Analytics ondersteunt bewaking duizenden elastische pools en honderden of duizenden databases. Log Analytics biedt een enkel bewakingsoplossing, die bewaking van andere toepassingen en Azure-services, over meerdere Azure-abonnementen kunt integreren.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Log Analytics (OMS) installeren en configureren
> * Log Analytics gebruiken voor het bewaken van groepen en databases

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De app Wingtip Tickets SaaS Database Per Tenant wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de toepassing Wingtip Tickets SaaS Database Per Tenant verkennen](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

Zie de zelfstudie [Monitor performance of the WTP sample SaaS application](saas-dbpertenant-performance-monitoring.md) (Prestaties bewaken met de voorbeeld-SaaS-app WTP) voor een beschrijving van de SaaS-scenario's en -patronen, en hoe die van invloed zijn op de vereisten voor een bewakingsoplossing.

## <a name="monitoring-and-managing-database-and-elastic-pool-performance-with-log-analytics-or-operations-management-suite-oms"></a>Controleren en beheren van databases en elastische pool prestaties met logboekanalyse of Operations Management Suite (OMS)

Bewaking en waarschuwingen is beschikbaar op de databases en opslaggroepen in de Azure-portal voor SQL-Database. Deze ingebouwde bewaking en waarschuwingen is handig, maar wordt de resource-specifieke, het is minder goed geschikt om grote installaties te bewaken of voor het ontwikkelen van een uniform overzicht over de resources en abonnementen.

Voor scenario's met hoog volume kunt Log Analytics worden gebruikt voor bewaking en waarschuwingen. Log Analytics is een afzonderlijke Azure-service waarmee analytics via diagnostische logboeken en de telemetrie die worden verzameld in een werkruimte mogelijk veel services. Log Analytics biedt een ingebouwde query taal en gegevens visualisatie hulpprogramma's waardoor operationele gegevensanalyse. De SQL-Analytics-oplossing biedt verschillende vooraf gedefinieerde elastische pool en database bewaken en waarschuwen weergaven en query's. OMS biedt ook een functie voor het ontwerpen van aangepaste weergaven.

Werkruimten en analyse-oplossingen van Log Analytics kunnen worden geopend in zowel Azure Portal als OMS. Azure Portal is het nieuwere toegangspunt, maar loopt qua functionaliteit mogelijk iets achter op de OMS-portal.

### <a name="create-performance-diagnostic-data-by-simulating-a-workload-on-your-tenants"></a>Prestaties diagnostische gegevens door te simuleren van een werklast op uw tenants maken 

1. In de **PowerShell ISE**open *... \\WingtipTicketsSaaS MultiTenantDb master\\Learning-Modules\\prestatiebewaking en beheer\\** Demo PerformanceMonitoringAndManagement.ps1***. Laat dit script open aangezien u mogelijk verschillende scenario's voor het genereren van gegevens wilt uitvoeren tijdens deze zelfstudie.
1. Als u dit nog niet hebt gedaan, voorzien in een batch te bieden een interessanter bewaking context tenants. Dit duurt enkele minuten:
   1. Stel **$DemoScenario = 1**, _inrichten van een batch van tenants_
   1. Het script uitvoeren en implementeren van een extra 17 tenants, drukt u op **F5**.  

1. De load-generator voor het uitvoeren van een gesimuleerde belasting op de tenants nu starten.  
    1. Stel **$DemoScenario = 2**, _genereren normale intensiteit laden (ongeveer 30 DTU)_.
    1. Het script wordt uitgevoerd, drukt u op **F5**.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>De toepassingsscripts Wingtip Tickets SaaS Database Per Tenant ophalen

De scripts Wingtip Tickets SaaS multitenant Database en de broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de blokkering van de Wingtip Tickets PowerShell-scripts.

## <a name="installing-and-configuring-log-analytics-and-the-azure-sql-analytics-solution"></a>Log Analytics en de oplossing Azure SQL Analytics installeren en configureren

Log Analytics is een afzonderlijke service die moet worden geconfigureerd. Log Analytics verzamelt logboekgegevens, Telemetrie en metrische gegevens in een log analytics-werkruimte. Log analytics-werkruimte is een resource, net als andere resources in Azure, en moet worden gemaakt. Terwijl de werkruimte niet hoeft te worden gemaakt in dezelfde resourcegroep bevinden als de toepassing(en) wordt die bewaakt, dus vaak zorgt ervoor dat het is verstandig doen. Voor de app Wingtip Tickets één resourcegroep zorgt u ervoor dat de werkruimte wordt verwijderd met de toepassing.

1. In de **PowerShell ISE**open *... \\WingtipTicketsSaaS MultiTenantDb master\\Learning-Modules\\prestatiebewaking en beheer\\Meld Analytics\\** Demo LogAnalytics.ps1***.
1. Het script wordt uitgevoerd, drukt u op **F5**.

U moet op dit moment kunnen openen logboekanalyse in de Azure-portal (of de OMS-portal). Het duurt enkele minuten duren voordat de telemetrie moeten worden verzameld in de werkruimte voor logboekanalyse en zichtbaar worden. Hoe langer laat u het systeem verzamelen van diagnostische gegevens de interessanter de ervaring is. U kunt nu misschien even iets anders gaan doen, maar controleer eerst of de load-generator nog wel actief is.

## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Pools en databases bewaken met Log Analytics en de SQL Analytics-oplossing


Open in deze oefening Log Analytics en de OMS-portal om te kijken naar de telemetrie die worden verzameld voor de databases en pools.

1. Blader naar de [Azure-portal](https://portal.azure.com) en Log Analytics openen door te klikken op **alle services**, zoekt u naar het Log Analytics:

   ![Log Analytics openen](media/saas-dbpertenant-log-analytics/log-analytics-open.png)

1. Selecteer de werkruimte met de naam _wtploganalytics -&lt;gebruiker&gt;_.

1. Selecteer **Overzicht** om de oplossing Log Analytics te openen in Azure Portal.

   ![Overzicht-koppeling](media/saas-dbpertenant-log-analytics/click-overview.png)

    > [!IMPORTANT]
    > Het duurt een paar minuten voordat de oplossing is actief. Geduld is een schone zaak.

1. Klik op de tegel Azure SQL Analytics om deze te openen.

    ![overview](media/saas-dbpertenant-log-analytics/overview.png)

    ![analytics](media/saas-dbpertenant-log-analytics/log-analytics-overview.png)

1. De weergaven in de oplossing Schuif opzij, met hun eigen interne schuifbalk onder (Vernieuw de pagina, indien nodig).

1. Gebruik de pagina overzicht door te klikken op de tegels of op een individuele database om een inzoomen explorer te openen.

1. Wijzig het filter instellen voor het wijzigen van het tijdsbereik - voor deze zelfstudie pick _laatste 1 uur_

    ![tijdfilter](media/saas-dbpertenant-log-analytics/log-analytics-time-filter.png)

1. Selecteer een individuele database verkennen query gebruiks- en metrische gegevens voor die database.

    ![database analytics](media/saas-dbpertenant-log-analytics/log-analytics-database.png)

1. Ga voor informatie over het gebruik Schuif metrische gegevens de analytics-pagina naar rechts.
 
     ![database metrische gegevens](media/saas-dbpertenant-log-analytics/log-analytics-database-metrics.png)

1. Schuif naar de analytics-pagina naar links en klik op de server-tegel in de lijst van de Resource-informatie. Hiermee opent u een pagina met de groepen en de databases op de server. 

     ![resource-informatie](media/saas-dbpertenant-log-analytics/log-analytics-resource-info.png)

 
     ![Server met toepassingen en databases](media/saas-dbpertenant-log-analytics/log-analytics-server.png)

1. Op de server toont pagina die wordt geopend die de toepassingen en databases op de server, klik op de groep.  Schuif naar rechts om te zien van de metrische gegevens van groep van toepassingen op de pagina pool dat wordt geopend.  

     ![metrische gegevens van toepassingen](media/saas-dbpertenant-log-analytics/log-analytics-pool-metrics.png)



1. Selecteer terug op de werkruimte voor logboekanalyse **OMS-Portal** om er in de werkruimte te openen.

    ![oms](media/saas-dbpertenant-log-analytics/log-analytics-workspace-oms-portal.png)

U kunt de logboek- en meetwaarde op die gegevens in de werkruimte verder verkennen in de OMS-portal.  

De bewaking en waarschuwingen in Log Analytics en OMS is gebaseerd op query's over de gegevens in de werkruimte, in tegenstelling tot de waarschuwingen voor elke resource in de Azure-portal gedefinieerd. Door waarschuwingen op query's als uitgangspunt, kunt u één waarschuwing die er ongeveer via alle databases in plaats van definiërende één per database uitziet definiëren. De uitvoering van query's wordt alleen beperkt door de gegevens die beschikbaar zijn in de werkruimte.

Voor meer informatie over het gebruik van OMS wilt opvragen en instellen van waarschuwingen, zien, [werken met regels voor waarschuwingen in logboekanalyse](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-alerts-creating).

De kosten die in rekening worden gebracht voor Log Analytics voor SQL Database worden gebaseerd op het gegevensvolume in de werkruimte. In deze zelfstudie maakt u gemaakt een gratis werkruimte die is beperkt tot 500 MB per dag. Zodra deze limiet is bereikt, wordt niet meer gegevens toegevoegd aan de werkruimte.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Log Analytics (OMS) installeren en configureren
> * Log Analytics gebruiken voor het bewaken van groepen en databases

[Zelfstudie over tenant-analyse](saas-dbpertenant-log-analytics.md)

## <a name="additional-resources"></a>Aanvullende resources

* [Aanvullende zelfstudies waarin voort op de eerste implementatie van Wingtip Tickets SaaS Database Per Tenant bouwen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md)
* [OMS](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)
