---
title: Log Analytics gebruiken met een SQL-Database multitenant-app | Microsoft Docs
description: Instellen en Log Analytics gebruiken met een multitenant SaaS van Azure SQL Database-app
keywords: zelfstudie sql-database
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sstein
ms.reviewer: billgib
ms.openlocfilehash: 38adf3dd2be0770dd815644ece452a82bc98baf9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34645314"
---
# <a name="set-up-and-use-log-analytics-with-a-multitenant-sql-database-saas-app"></a>Instellen en Log Analytics gebruiken met een multitenant SaaS van SQL Database-app

In deze zelfstudie maakt u instellen en gebruiken van Azure [logboekanalyse](/azure/log-analytics/log-analytics-overview) elastische pools en databases bewaken. Deze zelfstudie bouwt voort op de [prestaties bewaking en beheer zelfstudie](saas-dbpertenant-performance-monitoring.md). Er wordt weergegeven hoe Log Analytics gebruiken voor het verbeteren van de bewaking en waarschuwingen die zijn opgegeven in de Azure-portal. Log Analytics ondersteunt bewaking duizenden elastische pools en honderden of duizenden databases. Log Analytics biedt een enkel bewakingsoplossing, die bewaking van andere toepassingen en Azure-services over meerdere Azure-abonnementen kunt integreren.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Installeren en configureren van logboekanalyse.
> * Log Analytics gebruiken voor het bewaken van toepassingen en databases.

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De database per tenant Wingtip Tickets SaaS app wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de toepassing van de database per tenant Wingtip Tickets SaaS verkennen](saas-dbpertenant-get-started-deploy.md).
* Azure PowerShell is geïnstalleerd. Zie voor meer informatie [aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

Zie de [prestaties bewaking en beheer zelfstudie](saas-dbpertenant-performance-monitoring.md) voor een beschrijving van de SaaS-scenario's en gebruikspatronen en invloed op de vereisten voor een oplossing voor controle.

## <a name="monitor-and-manage-database-and-elastic-pool-performance-with-log-analytics"></a>Bewaken en beheren van de database en de elastische pool-prestaties met Log Analytics

Bewaking en waarschuwingen is beschikbaar op de databases en opslaggroepen in de Azure-portal voor Azure SQL Database. Deze ingebouwde bewaking en waarschuwingen is handig, maar het is ook resource-specifieke. Dit betekent dat het minder goed geschikt als u wilt bewaken van grote installaties of voorzien in een uniform overzicht resources en abonnementen.

U kunt Log Analytics voor scenario's met hoog volume gebruiken voor bewaking en waarschuwingen. Log Analytics is een afzonderlijke Azure-service waarmee analytics via diagnostische logboeken en telemetrie die worden verzameld in een werkruimte mogelijk veel services. Log Analytics biedt een ingebouwde query taal en gegevens visualisatie hulpprogramma's waarmee operationele gegevensanalyse. De SQL-Analytics-oplossing biedt verschillende vooraf gedefinieerde elastische pool en database bewaken en waarschuwen weergaven en query's. Log Analytics biedt ook een aangepaste weergave-ontwerper.

Log Analytics-werkruimten en -analyse oplossingen openen in de Azure-portal en in de Operations Management Suite. De Azure-portal is het nieuwe toegangspunt, maar mogelijk achter in bepaalde gebieden van de Operations Management Suite-portal.

### <a name="create-performance-diagnostic-data-by-simulating-a-workload-on-your-tenants"></a>Prestaties diagnostische gegevens door te simuleren van een werklast op uw tenants maken 

1. Open in de PowerShell ISE *... \\WingtipTicketsSaaS MultiTenantDb master\\Learning-Modules\\prestatiebewaking en beheer\\Demo PerformanceMonitoringAndManagement.ps1*. Houd dit script open omdat u mogelijk wilt uitvoeren verschillende van de belasting generatie scenario's tijdens deze zelfstudie.
2. Als u dit nog niet hebt gedaan, voorzien in een batch van tenants de bewaking context om interessanter te maken. Dit proces duurt enkele minuten duren.

   a. Stel **$DemoScenario = 1**, _inrichten van een batch van tenants_.

   b. Voer het script en implementeren van een extra 17 tenants, druk op F5.

3. De load-generator voor het uitvoeren van een gesimuleerde belasting op de tenants nu starten.

    a. Stel **$DemoScenario = 2**, _genereren normale intensiteit laden (ongeveer 30 DTU)_.

    b. Het script wordt uitgevoerd, druk op F5.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>De database per tenant-toepassingsscripts Wingtip Tickets SaaS ophalen

De scripts voor Wingtip Tickets SaaS-multitenant-database en de broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Zie voor stappen voor het downloaden en de Wingtip Tickets PowerShell-scripts deblokkeren de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md).

## <a name="install-and-configure-log-analytics-and-the-azure-sql-analytics-solution"></a>Log Analytics en de Azure SQL Analytics-oplossing installeren en configureren

Log Analytics is een afzonderlijke service, die moet worden geconfigureerd. Log Analytics verzamelt logboekgegevens, Telemetrie en metrische gegevens in een werkruimte voor logboekanalyse. Net als andere resources in Azure moet een werkruimte voor logboekanalyse worden gemaakt. De werkruimte hoeft niet te worden gemaakt in dezelfde resourcegroep bevinden als de toepassingen die deze controleert. Zo vaak doet zinvol het meest al. Gebruik één resourcegroep voor de app Wingtip Tickets om ervoor te zorgen dat de werkruimte wordt verwijderd met de toepassing.

1. Open in de PowerShell ISE *... \\WingtipTicketsSaaS MultiTenantDb master\\Learning-Modules\\prestatiebewaking en beheer\\Meld Analytics\\Demo LogAnalytics.ps1*.
2. Het script wordt uitgevoerd, druk op F5.

U kunt nu logboekanalyse openen in de Azure portal of de Operations Management Suite-portal. Het duurt enkele minuten voor het verzamelen van telemetriegegevens in de werkruimte voor logboekanalyse en zichtbaar te maken. Hoe langer laat u het systeem verzamelen van diagnostische gegevens, hoe meer interessante de ervaring is. 

## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Pools en databases bewaken met Log Analytics en de SQL Analytics-oplossing


Open in deze oefening Log Analytics en de Operations Management Suite-portal om te kijken naar de telemetrie die voor de databases en pools verzameld.

1. Blader naar de [Azure-portal](https://portal.azure.com). Selecteer **alle services** logboekanalyse openen. Zoek vervolgens naar logboekanalyse.

   ![Open Log Analytics](media/saas-dbpertenant-log-analytics/log-analytics-open.png)

2. Selecteer de werkruimte met de naam _wtploganalytics -&lt;gebruiker&gt;_.

3. Selecteer **Overzicht** om de oplossing Log Analytics te openen in Azure Portal.

   ![Overzicht](media/saas-dbpertenant-log-analytics/click-overview.png)

    > [!IMPORTANT]
    > Het duurt een paar minuten voordat de oplossing is actief. 

4. Selecteer de **Azure SQL Analytics** tegel om dit te openen.

    ![Overzichtstegel](media/saas-dbpertenant-log-analytics/overview.png)

5. De weergaven in de oplossing Schuif opzij, met hun eigen interne schuifbalk onderaan. Vernieuw de pagina, indien nodig.

6. Als u wilt verkennen de overzichtspagina, selecteer de tegels of de afzonderlijke databases om een inzoomen explorer te openen.

    ![Log Analytics-dashboard](media/saas-dbpertenant-log-analytics/log-analytics-overview.png)

7. Wijzig de filterinstelling het tijdsbereik wijzigen. Selecteer voor deze zelfstudie **laatste 1 uur**.

    ![tijdfilter](media/saas-dbpertenant-log-analytics/log-analytics-time-filter.png)

8. Selecteer een individuele database wilt experimenteren met het gebruik van de query en metrische gegevens voor die database.

    ![database analytics](media/saas-dbpertenant-log-analytics/log-analytics-database.png)

9. Voor informatie over het gebruik metrische gegevens, de analytics-pagina naar rechts schuiven.
 
     ![database metrische gegevens](media/saas-dbpertenant-log-analytics/log-analytics-database-metrics.png)

10. Analytics Blader aan de linkerkant en selecteer de tegel server in de **Resource-informatie** lijst.  

    ![Lijst met resources](media/saas-dbpertenant-log-analytics/log-analytics-resource-info.png)

    Een pagina wordt geopend waarin de groepen en de databases op de server.

    ![Server met toepassingen en databases](media/saas-dbpertenant-log-analytics/log-analytics-server.png)

11. Selecteer een groep. Schuif naar rechts om te zien van de metrische gegevens van groep van toepassingen op de pagina pool dat wordt geopend. 

    ![metrische gegevens van toepassingen](media/saas-dbpertenant-log-analytics/log-analytics-pool-metrics.png)


12. Selecteer terug in de werkruimte voor logboekanalyse **OMS-Portal** om er in de werkruimte te openen.

    ![Operations Management Suite-Portal tegel](media/saas-dbpertenant-log-analytics/log-analytics-workspace-oms-portal.png)

U kunt de logboek- en meetwaarde op die gegevens in de werkruimte verder verkennen in de Operations Management Suite-portal. 

Bewaking en waarschuwingen in logboekanalyse zijn gebaseerd op query's over de gegevens in de werkruimte, in tegenstelling tot de waarschuwingen voor elke resource in de Azure-portal gedefinieerd. Door waarschuwingen op query's als uitgangspunt, kunt u één waarschuwing die er ongeveer via alle databases in plaats van definiërende één per database uitziet definiëren. Query's zijn alleen beperkt door de gegevens die beschikbaar zijn in de werkruimte.

Zie voor meer informatie over het gebruik van logboekanalyse doorzoeken en waarschuwingen instellen [werken met regels voor waarschuwingen in logboekanalyse](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts-creating).

Log Analytics voor SQL-Database kosten op basis van het gegevensvolume in de werkruimte. In deze zelfstudie maakt u gemaakt een gratis werkruimte die is beperkt tot 500 MB per dag. Nadat deze limiet is bereikt, wordt niet meer gegevens toegevoegd aan de werkruimte.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Installeren en configureren van logboekanalyse.
> * Log Analytics gebruiken voor het bewaken van toepassingen en databases.

Probeer de [Tenant analytics zelfstudie](saas-dbpertenant-log-analytics.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Aanvullende zelfstudies die zijn gebaseerd op de implementatie van de eerste Wingtip Tickets SaaS-toepassing van de database per tenant](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md)
