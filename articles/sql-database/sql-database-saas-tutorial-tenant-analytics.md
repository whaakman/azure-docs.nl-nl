---
title: Analytics-query&quot;s uitvoeren voor meerdere Azure SQL-databases | Microsoft Docs
description: Gedistribueerde query&quot;s uitvoeren voor meerdere Azure SQL-databases
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
ms.openlocfilehash: a0742a004b618dda304618bca21ae715552c16e6
ms.contentlocale: nl-nl
ms.lasthandoff: 05/12/2017


---
# <a name="run-distributed-queries-across-multiple-azure-sql-databases"></a>Gedistribueerde query's uitvoeren voor meerdere Azure SQL-databases

In deze zelfstudie voert u analytics-query’s uit op elke tenant in de catalogus. Er wordt een elastische taak gemaakt die de query’s uitvoert. De taak haalt gegevens op en laadt deze in een aparte analytics-database die op de catalogusserver wordt aangemaakt. Op deze database kunnen query’s worden uitgevoerd om inzichten op te doen die verborgen liggen in de alledaagse operationele gegevens van alle tenants. De uitvoer van deze taak is een tabel van de resultaten van de query’s op de analytics-database van de tenant.


In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * U maakt de analytics-database van de tenant
> * U maakt een geplande taak om gegevens op te halen en deze gegevens in te vullen in de analytics-database

Voor het voltooien van deze zelfstudie moet u ervoor zorgen dat aan de volgende vereisten is voldaan:

* De WTP-app is geïmplementeerd. Zie [De WTP SaaS-toepassing implementeren en verkennen](sql-database-saas-tutorial.md) om dit in minder dan vijf minuten te doen
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.
* De laatste versie van SQL Server Management Studio (SSMS) moet zijn geïnstalleerd. [SSMS downloaden en installeren](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

## <a name="tenant-operational-analytics-pattern"></a>Operational Analytics-patroon van tenant

Een van de geweldige mogelijkheden met SaaS-toepassingen is het gebruik van de uitgebreide tenant-gegevens die zijn opgeslagen in de cloud. Gebruik deze gegevens om inzicht te krijgen in de werking en het gebruik van uw toepassing en uw tenants. Deze gegevens kunnen dienen als richtlijn voor het ontwikkelen van functies, het aanbrengen van verbeteringen in de bruikbaarheid en voor andere investeringen in de app en het platform. U kunt gemakkelijk toegang tot deze gegevens krijgen in een database met meerdere tenants, maar dit is niet zo gemakkelijk als de gegevens op schaal zijn gedistribueerd over misschien wel duizenden databases. Eén mogelijke benadering voor het toegankelijk maken van deze gegevens is het gebruik van Elastische taken. Hiermee kunnen resultaten van door taken uitgevoerde query’s worden vastgelegd in een output-database en -tabel.

## <a name="get-the-wingtip-application-scripts"></a>De scripts van de Wingtip-toepassing downloaden

De scripts en broncode van de Wingtip Tickets-toepassing zijn beschikbaar in de GitHub-opslagplaats [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). U vindt de scriptbestanden in de map [Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Download de map **Learning Modules** naar uw lokale computer en behoud de mapstructuur.

## <a name="deploy-a-database-for-tenant-analytics-results"></a>Implementeer een database voor de analytics-resultaten van de tenant

Voor deze zelfstudie moet u een database hebben geïmplementeerd om de resultaten van de taakuitvoering van scripts vast te leggen. Deze bevatten de resultaten van de query’s. We maken hiervoor een database met de naam tenantanalytics.

1. Open …\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* in de *PowerShell ISE* en stel de volgende waarde in:
   * **$DemoScenario** = **2** *Operational analytics-database implementeren*
1. Druk op **F5** om het demoscript uit te voeren (dat het *Deploy-TenantAnalyticsDB.ps1*-script aanroept) dat de tenant analytics-database aanmaakt.

## <a name="create-some-data-for-the-demo"></a>Maak wat gegevens aan voor de demo

1. Open …\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* in de *PowerShell ISE* en stel de volgende waarde in:
   * **$DemoScenario** = **1** *Tickets kopen voor voorstellingen bij alle theaters*
1. Druk op **F5** om het script uit te voeren en een geschiedenis van kaartverkoop te maken.


## <a name="create-a-scheduled-job-to-retrieve-tenant-analytics-about-ticket-purchases"></a>Maak een geplande taak om tenant-analytics op te halen over kaartverkoop

Dit script maakt een taak om informatie over kaartverkoop op te halen van alle tenants. Zodra dit in één tabel is verzameld, krijgt u waardevolle gegevens over de patronen van kaartverkoop bij alle tenants.

1. Open SSMS en maak verbinding met de catalog-\<user\>.database.windows.net-server
1. Open ...\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*TicketPurchasesfromAllTenants.sql*
1. Wijzig \<WtpUser\>, gebruik dezelfde gebruikersnaam als toen u de WTP-app implementeerde boven aan in het script, **sp\_add\_target\_group\_member** en **sp\_add\_jobstep**
1. Klik met de rechtermuisknop, selecteer **Verbinding** en maak verbinding met de catalog-\<WtpUser\>.database.windows.net-server, als deze verbinding nog niet is gemaakt
1. Zorg dat u verbinding hebt met de **jobaccount**-database en druk op **F5** om het script uit te voeren

* **sp\_add\_target\_group** maakt de doelgroepnaam *TenantGroup*. Nu moeten we de doelleden toevoegen.
* **sp\_add\_target\_group\_member** voegt het doelledentype *server* toe. Dit type schrijft voor dat alle databases binnen die server (let op, dit is de customer1-&lt;WtpUser&gt;-server, die de tenantdatabases bevat) ten tijde van de taakuitvoering in de taak moeten zijn opgenomen.
* **sp\_add\_job** maakt een nieuwe wekelijks geplande taak met de naam “Ticket Purchases from all Tenants”
* **sp\_add\_jobstep** maakt de taakstap die de T-SQL-opdrachttekst bevat om alle kaartverkoopinformatie op te halen van alle tenants en het resultaat te kopiëren naar een tabel met de naam *AllTicketsPurchasesfromAllTenants*
* De resterende weergaven in het script tonen het bestaan van de objecten en controleren de taakuitvoering. Bekijk de statuswaarde in de kolom **lifecycle** om de status te controleren. Zodra het is gelukt, is de taak met succes voltooid op alle tenantdatabases en de twee andere databases die de referentietabel bevatten.

Als het script met succes is uitgevoerd, moet u een soortgelijk resultaat zien:

![resultaten](media/sql-database-saas-tutorial-tenant-analytics/ticket-purchases-job.png)

## <a name="create-a-job-to-retrieve-a-summary-count-of-ticket-purchases-from-all-tenants"></a>Maak een taak om een samenvattende telling van kaartverkopen van alle tenants op te halen

Dit script maakt een taak om het totaal van alle kaartverkopen op te halen van alle tenants.

1. Open SSMS en maak verbinding met de *catalog-&lt;User&gt;.database.windows.net*-server
1. Open het bestand …\\Learning Modules\\Provision en Catalog\\Operational Analytics\\Tenant Analytics\\*Results-TicketPurchasesfromAllTenants.sql*
1. Wijzig &lt;WtpUser&gt;; gebruik dezelfde gebruikersnaam als toen u de WTP-app implementeerde in het script, in de opgeslagen procedure **sp\_add\_jobstep**
1. Klik met de rechtermuisknop, selecteer **Verbinding** en maak verbinding met de catalog-\<WtpUser\>.database.windows.net-server, als deze verbinding nog niet is gemaakt
1. Zorg dat u verbinding hebt met de **tenantanalytics**-database en druk op **F5** om het script uit te voeren

Als het script met succes is uitgevoerd, moet u een soortgelijk resultaat zien:

![resultaten](media/sql-database-saas-tutorial-tenant-analytics/total-sales.png)



* **sp\_add\_job** maakt een nieuwe wekelijks geplande taak met de naam “ResultsTicketsOrders”

* **sp\_add\_jobstep** maakt de taakstap die de T-SQL-opdrachttekst bevat om alle kaartverkoopinformatie op te halen van alle tenants en het resultaat te kopiëren naar een tabel met de naam CountofTicketOrders

* De resterende weergaven in het script tonen het bestaan van de objecten en controleren de taakuitvoering. Bekijk de statuswaarde in de kolom **lifecycle** om de status te controleren. Zodra het is gelukt, is de taak met succes voltooid op alle tenantdatabases en de twee andere databases die de referentietabel bevatten.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * U hebt een tenant analytics-database geïmplementeerd
> * U hebt een geplande taak gemaakt om analytische gegevens op te halen van meerdere tenants

Gefeliciteerd.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Aanvullende zelfstudies als vervolg op de eerste implementatie van de toepassing Wingtip Tickets Platform (WTP)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Elastische taken](sql-database-elastic-jobs-overview.md)
