---
title: Logboeken van Azure Monitor gebruiken met een SQL-Database-app voor meerdere tenants | Microsoft Docs
description: Instellen en gebruiken van Azure Monitor-logboeken met een Azure SQL Database SaaS-app voor meerdere tenants
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: billgib
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: b283f2691d2cb3135007a752348a8d9759e870f5
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57214160"
---
# <a name="set-up-and-use-azure-monitor-logs-with-a-multitenant-sql-database-saas-app"></a>Instellen en gebruiken van Azure Monitor-logboeken met een SQL Database SaaS-app voor meerdere tenants

In deze zelfstudie hebt u instellen en gebruiken [logboeken van Azure Monitor](/azure/log-analytics/log-analytics-overview) voor het bewaken van elastische pools en databases. In deze zelfstudie bouwt voort op de [zelfstudie voor het beheer en bewaking van prestaties](saas-dbpertenant-performance-monitoring.md). Het laat zien hoe u logboeken van Azure Monitor gebruiken om te verbeteren van de bewaking en waarschuwingen die in Azure portal. Azure Monitor registreert ondersteunt duizenden van elastische pools en honderdduizenden databases bewaken. Logboeken in Azure Monitor biedt een centrale bewakingsoplossing, die kan worden geïntegreerd bewaking van verschillende toepassingen en Azure-services over meerdere Azure-abonnementen.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Installeer en configureer Azure Monitor-Logboeken.
> * Gebruik Azure Monitor-logboeken voor het bewaken van pools en databases.

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De Wingtip Tickets SaaS-database-per-tenant-app wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en verkennen van de toepassing Wingtip Tickets SaaS-database-per-tenant](saas-dbpertenant-get-started-deploy.md).
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

Zie de [zelfstudie voor het beheer en bewaking van prestaties](saas-dbpertenant-performance-monitoring.md) voor een beschrijving van de SaaS-scenario's en patronen en hoe ze van invloed op de vereisten voor een oplossing voor bewaking.

## <a name="monitor-and-manage-database-and-elastic-pool-performance-with-azure-monitor-logs"></a>Controleren en beheren van databases en elastische pool-prestaties met Azure Monitor-Logboeken

Bewaking en waarschuwingen is beschikbaar voor databases en pools in Azure portal voor Azure SQL-Database. Deze ingebouwde bewaking en waarschuwingen is handig, maar het is ook resourcespecifieke. Dit betekent dat deze zijn minder geschikt voor het bewaken van grote installaties of zo een integrale weergave in resources en abonnementen.

Voor scenario's met hoge volumes, kunt u logboeken van Azure Monitor kunt gebruiken voor bewaking en waarschuwingen. Azure Monitor is een afzonderlijke Azure-service waarmee analytics van diagnostische logboeken en telemetrie die mogelijk veel services in een werkruimte worden verzameld. Logboeken in Azure Monitor biedt een ingebouwde query querytaal en visualisatiehulpmiddelen waarmee operationele gegevens analytics. De oplossing SQL Analytics biedt verschillende vooraf gedefinieerde elastische pool en database controleren en meldingen weergaven en query's. Logboeken in Azure Monitor biedt ook een aangepaste weergave-ontwerper.

OMS-werkruimten worden nu aangeduid als Log Analytics-werkruimten. Log Analytics-werkruimten en analyse-oplossingen openen in de Azure-portal. De Azure-portal is het nieuwere toegangspunt, maar is het mogelijk wat er achter de Operations Management Suite-portal in bepaalde gebieden.

### <a name="create-performance-diagnostic-data-by-simulating-a-workload-on-your-tenants"></a>Diagnostische gegevens voor prestaties door te simuleren van een werkbelasting op uw tenants maken 

1. Open in de PowerShell ISE *... \\WingtipTicketsSaaS-MultiTenantDb-master\\Learning Modules\\Performance Monitoring and Management\\Demo-PerformanceMonitoringAndManagement.ps1*. Houd dit script open aangezien u mogelijk wilt meerdere van de belasting genereren van scenario's uitvoeren tijdens deze zelfstudie.
1. Als u nog niet hebt gedaan, richt u een batch met tenants waarmee de controle context interessanter. Dit proces duurt enkele minuten.

   a. Stel **$DemoScenario = 1**, _batch met tenants inrichten_.

   b. Het uitvoeren van het script en implementeren van een extra 17 tenants, druk op F5.

1. Nu beginnen met de load-generator voor het uitvoeren van een gesimuleerde belasting op alle tenants.

    a. Stel **$DemoScenario = 2**, _belasting met normale intensiteit genereren (ongeveer 30 DTU)_.

    b. Druk op F5 om het script uitvoert.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>De database-per-tenant-toepassing Wingtip Tickets SaaS scripts ophalen

De Wingtip Tickets SaaS multitenant-databasescripts en broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Zie voor stappen voor het downloaden en blokkering opheffen van de Wingtip Tickets PowerShell-scripts, de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md).

## <a name="install-and-configure-log-analytics-workspace-and-the-azure-sql-analytics-solution"></a>Log Analytics-werkruimte en de oplossing Azure SQL Analytics installeren en configureren

Azure Monitor is een afzonderlijke service die moet worden geconfigureerd. Azure Monitor-logboeken verzamelt logboekgegevens, Telemetrie en metrische gegevens in een Log Analytics-werkruimte. Net als andere resources in Azure moet Log Analytics-werkruimte worden gemaakt. De werkruimte hoeft niet te worden gemaakt in dezelfde resourcegroep bevinden als de toepassingen die wordt bewaakt. Wel het handigst echter zo vaak doen. Gebruik één resourcegroep bestaan om te controleren of dat de werkruimte is verwijderd met de toepassing voor de Wingtip Tickets-app.

1. Open in de PowerShell ISE *... \\WingtipTicketsSaaS-MultiTenantDb-master\\Learning Modules\\Performance Monitoring and Management\\Log Analytics\\Demo-LogAnalytics.ps1*.
1. Druk op F5 om het script uitvoert.

Nu kunt u openen beschikbaar Azure Monitor zijn in Azure portal. Het duurt een paar minuten voor het verzamelen van telemetrie in de Log Analytics-werkruimte en deze zichtbaar te maken. Hoe langer laat u het systeem verzamelen van diagnostische gegevens, hoe interessanter de ervaring is. 

## <a name="use-log-analytics-workspace-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Log Analytics-werkruimte en de oplossing SQL Analytics gebruiken voor het bewaken van pools en databases


In deze oefening opent u Log Analytics-werkruimte in de Azure-portal om te kijken naar de telemetrie die is verzameld voor de databases en pools.

1. Blader naar de [Azure-portal](https://portal.azure.com). Selecteer **alle services** om Log Analytics-werkruimte te openen. Zoek vervolgens naar Log Analytics.

   ![Log Analytics-werkruimte openen](media/saas-dbpertenant-log-analytics/log-analytics-open.png)

1. Selecteer de werkruimte met de naam _wtploganalytics -&lt;gebruiker&gt;_.

1. Selecteer **overzicht** openen van de oplossing log analytics in Azure portal.

   ![Overzicht](media/saas-dbpertenant-log-analytics/click-overview.png)

    > [!IMPORTANT]
    > Het duurt enkele minuten voordat de oplossing actief is. 

1. Selecteer de **Azure SQL Analytics** tegel om deze te openen.

    ![Overzichtstegel](media/saas-dbpertenant-log-analytics/overview.png)

1. De weergaven in de oplossing schuift horizontaal, met hun eigen interne schuifbalk aan de onderkant. Vernieuw de pagina, indien nodig.

1. Als u wilt de overzichtspagina verkennen, selecteer de tegels of de afzonderlijke databases om een inzoomen explorer te openen.

    ![Log analytics-dashboard](media/saas-dbpertenant-log-analytics/log-analytics-overview.png)

1. Het filterinstelling te wijzigen van het tijdsbereik wijzigen. Selecteer voor deze zelfstudie **laatste 1 uur**.

    ![Tijdfilter](media/saas-dbpertenant-log-analytics/log-analytics-time-filter.png)

1. Selecteer een afzonderlijke database verkennen van het querygebruik en de metrische gegevens voor die database.

    ![Database-analyse](media/saas-dbpertenant-log-analytics/log-analytics-database.png)

1. Overzicht van metrische gegevens over gebruik, de analytics-pagina naar rechts te schuiven.
 
     ![Metrische databasegegevens](media/saas-dbpertenant-log-analytics/log-analytics-database-metrics.png)

1. Schuif de pagina analytics aan de linkerkant en selecteer de tegel van de server in de **Broninfo** lijst.  

    ![Lijst met resources](media/saas-dbpertenant-log-analytics/log-analytics-resource-info.png)

    Er verschijnt een pagina waarop wordt weergegeven de pools en databases op de server.

    ![Server met pools en databases](media/saas-dbpertenant-log-analytics/log-analytics-server.png)

1. Selecteer een groep. Op de pagina voor groep van toepassingen die wordt geopend, schuift u naar rechts om te zien van de groep van metrische gegevens. 

    ![Metrische gegevens van toepassingen](media/saas-dbpertenant-log-analytics/log-analytics-pool-metrics.png)


1. Selecteer terug in de Log Analytics-werkruimte **OMS-Portal** om er de werkruimte te openen.

    ![Log Analytics-werkruimte](media/saas-dbpertenant-log-analytics/log-analytics-workspace-oms-portal.png)

U kunt de logboek- en metrische gegevens verder verkennen in de Log Analytics-werkruimte. 

Bewaking en waarschuwingen in Azure Monitor worden de logboeken gebaseerd op query's voor de gegevens in de werkruimte, in tegenstelling tot de waarschuwingen op elke resource in de Azure-portal gedefinieerd. Waarschuwingen gebaseerd op query's, kunt u een afzonderlijke waarschuwing die voor alle databases in plaats van definiëren een per-database. Query's zijn beperkt tot alleen de gegevens die beschikbaar zijn in de werkruimte.

Zie voor meer informatie over het gebruik van Logboeken van Azure Monitor opvragen en waarschuwingen instellen [werken met regels voor waarschuwingen in Azure Monitor-logboeken](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts-creating).

Azure Monitor-logboeken voor SQL Database kosten in rekening gebracht op basis van het gegevensvolume in de werkruimte. In deze zelfstudie maakt u een gratis werkruimte gemaakt, die is beperkt tot 500 MB per dag. Nadat u deze limiet is bereikt, is niet meer gegevens toegevoegd aan de werkruimte.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Installeer en configureer Azure Monitor-Logboeken.
> * Gebruik Azure Monitor-logboeken voor het bewaken van pools en databases.

Probeer de [zelfstudie Tenant-analytics](saas-dbpertenant-log-analytics.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Aanvullende zelfstudies op de eerste implementatie van de Wingtip Tickets SaaS-toepassing van de database-per-tenant](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure Monitor-logboeken](../azure-monitor/insights/azure-sql.md)
