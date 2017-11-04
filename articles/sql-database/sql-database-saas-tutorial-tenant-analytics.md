---
title: Analytics-query's uitvoeren voor meerdere Azure SQL-databases | Microsoft Docs
description: Gegevens ophalen uit de tenant-databases in een database analytics voor offline-analyse
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: billgib; sstein
ms.openlocfilehash: 4a96efb15268c56e3625832b0b4d6dd8f6a78614
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="extract-data-from-tenant-databases-into-an-analytics-database-for-offline-analysis"></a>Gegevens ophalen uit de tenant-databases in een database analytics voor offline-analyse

In deze zelfstudie kunt u een taak elastische query's uitvoeren op elke tenant-database. De taak ticket verkoopgegevens extraheert en laadt deze in een analytics-database (of het datawarehouse) voor analyse. De analytics-database wordt vervolgens gevraagd insights extraheren uit deze dagelijkse operationele gegevens van alle tenants.


In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * U maakt de analytics-database van de tenant
> * U maakt een geplande taak om gegevens op te halen en deze gegevens in te vullen in de analytics-database

Voor het voltooien van deze zelfstudie moet u ervoor zorgen dat aan de volgende vereisten is voldaan:

* De Wingtip SaaS-app wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en Verken de Wingtip SaaS-toepassing](sql-database-saas-tutorial.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.
* De laatste versie van SQL Server Management Studio (SSMS) moet zijn geïnstalleerd. [SSMS downloaden en installeren](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

## <a name="tenant-operational-analytics-pattern"></a>Operational Analytics-patroon van tenant

Een van de geweldige mogelijkheden met SaaS-toepassingen is het gebruik van de uitgebreide tenant-gegevens die zijn opgeslagen in de cloud. Gebruik deze gegevens om inzicht te krijgen in de werking en het gebruik van uw toepassing en uw tenants. Deze gegevens kunnen dienen als richtlijn voor het ontwikkelen van functies, het aanbrengen van verbeteringen in de bruikbaarheid en voor andere investeringen in de app en het platform. U kunt gemakkelijk toegang tot deze gegevens krijgen in een database met meerdere tenants, maar dit is niet zo gemakkelijk als de gegevens op schaal zijn gedistribueerd over misschien wel duizenden databases. Eén mogelijke benadering voor het toegankelijk maken van deze gegevens is het gebruik van Elastische taken. Hiermee kunnen resultaten van door taken uitgevoerde query’s worden vastgelegd in een output-database en -tabel.

## <a name="get-the-wingtip-application-scripts"></a>De scripts van de Wingtip-toepassing downloaden

De Wingtip SaaS-scripts en de broncode van toepassing zijn beschikbaar in de [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-opslagplaats. [Stappen voor het downloaden van de scripts Wingtip SaaS](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

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

1. Open SSMS en maak verbinding met de catalog-&lt;user&gt;.database.windows.net-server
1. Open ...\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*TicketPurchasesfromAllTenants.sql*
1. Wijzig &lt;gebruiker&gt;, gebruikt u de naam van de gebruiker gebruikt wanneer u de Wingtip SaaS-app aan de bovenkant van het script hebt geïmplementeerd **sp\_toevoegen\_doel\_groep\_lid** en **sp\_toevoegen\_taakstap**
1. Klik met de rechtermuisknop, selecteer **verbinding**, en maak verbinding met de catalogus -&lt;gebruiker&gt;. database.windows.net server, indien nog niet verbonden
1. Zorg dat u verbinding hebt met de **jobaccount**-database en druk op **F5** om het script uit te voeren

* **sp\_add\_target\_group** maakt de doelgroepnaam *TenantGroup*. Nu moeten we de doelleden toevoegen.
* **SP\_toevoegen\_doel\_groep\_lid** voegt een *server* doeltype lid, die alle databases binnen die server acht (dit is de customer1 -&lt;gebruiker&gt; server met de tenant-databases) op moment van de taak kan worden uitgevoerd in de taak moet worden opgenomen.
* **sp\_add\_job** maakt een nieuwe wekelijks geplande taak met de naam “Ticket Purchases from all Tenants”
* **sp\_add\_jobstep** maakt de taakstap die de T-SQL-opdrachttekst bevat om alle kaartverkoopinformatie op te halen van alle tenants en het resultaat te kopiëren naar een tabel met de naam *AllTicketsPurchasesfromAllTenants*
* De resterende weergaven in het script tonen het bestaan van de objecten en controleren de taakuitvoering. Bekijk de statuswaarde in de kolom **lifecycle** om de status te controleren. Zodra het is gelukt, is de taak met succes voltooid op alle tenantdatabases en de twee andere databases die de referentietabel bevatten.

Als het script met succes is uitgevoerd, moet u een soortgelijk resultaat zien:

![resultaten](media/sql-database-saas-tutorial-tenant-analytics/ticket-purchases-job.png)

## <a name="create-a-job-to-retrieve-a-summary-count-of-ticket-purchases-from-all-tenants"></a>Maak een taak om een samenvattende telling van kaartverkopen van alle tenants op te halen

Dit script maakt een taak om het totaal van alle kaartverkopen op te halen van alle tenants.

1. Open SSMS en maak verbinding met de *catalog-&lt;User&gt;.database.windows.net*-server
1. Open het bestand …\\Learning Modules\\Provision en Catalog\\Operational Analytics\\Tenant Analytics\\*Results-TicketPurchasesfromAllTenants.sql*
1. Wijzig &lt;gebruiker&gt;, gebruikt u de naam van de gebruiker gebruikt wanneer u de app Wingtip SaaS in het script wordt geïmplementeerd de **sp\_toevoegen\_taakstap** opgeslagen procedure
1. Klik met de rechtermuisknop, selecteer **verbinding**, en maak verbinding met de catalogus -&lt;gebruiker&gt;. database.windows.net server, indien nog niet verbonden
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

* Aanvullende [zelfstudies waarin voort op de Wingtip SaaS-toepassing bouwen](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastische taken](sql-database-elastic-jobs-overview.md)
