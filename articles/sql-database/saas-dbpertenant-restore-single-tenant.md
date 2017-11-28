---
title: Herstellen van een Azure SQL database in een multitenant SaaS-app | Microsoft Docs
description: Informatie over het herstellen van een enkele tenants SQL-database na het per ongeluk verwijderen van gegevens
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
ms.date: 05/10/2017
ms.author: billgib;sstein
ms.openlocfilehash: ee2bc6d8b75b92243c0550db0044895e41c9474b
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="restore-a-single-tenants-azure-sql-database-in-a-multi-tenant-saas-app"></a>Een enkele tenant Azure SQL database in een multitenant SaaS-app herstellen

De app Wingtip Tickets SaaS gebouwd met behulp van een database per tenant-model, waarbij elke tenant hun eigen database heeft. Een van de voordelen van dit model is dat deze gemakkelijk kunt herstellen van gegevens van één tenant in isolatie zonder enige impact op andere tenants.

In deze zelfstudie leert u twee data recovery patronen:

> [!div class="checklist"]

> * Een database herstellen naar een parallelle database (side-by-side)
> * Een database in place herstellen


|||
|:--|:--|
| **Tenant naar een eerdere punt in tijd, in een parallelle database herstellen** | Dit patroon kan worden gebruikt door de tenant voor controle, controle, naleving, enzovoort. De oorspronkelijke database blijft online en ongewijzigd. |
| **Tenant in-place herstellen** | Dit patroon wordt doorgaans gebruikt voor het herstellen van een tenant naar een eerdere punt in tijd, nadat de gegevens in een tenant per ongeluk worden verwijderd. De oorspronkelijke database wordt offline gezet en wordt vervangen door de teruggezette database. |
|||

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De Wingtip SaaS-app wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en Verken de Wingtip SaaS-toepassing](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

## <a name="introduction-to-the-saas-tenant-restore-pattern"></a>Inleiding tot het patroon van SaaS-tenant terugzetten

Er zijn twee eenvoudige patronen voor het herstellen van gegevens van een afzonderlijke tenant voor de tenant restore-patroon. Omdat de tenant-databases zijn geïsoleerd van elkaar, heeft herstellen van een tenant geen invloed op een andere tenant-gegevens.

Gegevens worden in het eerste patroon teruggezet naar een nieuwe database. De tenant vervolgens toegang krijgt tot deze database naast hun productiegegevens. Dit patroon kan een tenantbeheerder om te controleren van de herstelde gegevens en mogelijk gebruiken om te overschrijven selectief huidige gegevenswaarden. Het is aan de SaaS-app-ontwerper om te bepalen hoe geavanceerde opties voor het herstel van de gegevens moeten worden. Gewoon kan kunt controleren van de gegevens in de staat waarin die deze zich op een bepaald moment in de tijd worden veroorzaakt door alle die vereist is in sommige scenario's. Als de database gebruikt [geo-replicatie](sql-database-geo-replication-overview.md), wordt aangeraden de benodigde gegevens kopiëren van de herstelde kopie in de oorspronkelijke database. Als u de oorspronkelijke database door de teruggezette database vervangen, moet u opnieuw configureert en geo-replicatie opnieuw synchroniseren.

In het tweede patroon, waarbij ervan wordt uitgegaan dat de tenant heeft geleden een verlies of beschadiging van gegevens, van de tenant-productiedatabase hersteld naar een eerdere punt in tijd. Het herstel in plaats patroon is de tenant offline gezet gedurende een korte periode terwijl de database is hersteld en weer online. De oorspronkelijke database wordt verwijderd, maar kan nog steeds worden hersteld vanuit als u wilt teruggaan naar een zelfs eerder in de tijd. Een variant van dit patroon kan wijzigen in de database in plaats van te verwijderen, hoewel de naam van de database biedt geen extra voordelen in termen van de beveiliging van gegevens.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>De toepassingsscripts Wingtip Tickets SaaS Database Per Tenant ophalen

De scripts Wingtip Tickets SaaS multitenant Database en de broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de scripts Wingtip Tickets SaaS deblokkeren.

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>Simuleren van een tenant per ongeluk verwijderen van gegevens

Om te demonstreren deze herstelscenario's, moeten we *per ongeluk* sommige gegevens in een van de tenant-databases verwijderen. Terwijl u elke record verwijderen kunt, stelt de volgende stap de demo ophalen niet geblokkeerd door referentiële integriteit schendingen! Ook worden sommige ticket inkoopgegevens kunt u later in de *Wingtip SaaS Analytics zelfstudies*.

Het ticket generator-script uitvoeren en aanvullende gegevens te maken. De generator ticket komt opzettelijk niet kopen van tickets voor elke laatste gebeurtenis van tenants.

1. Open... \\Learning-Modules\\hulpprogramma's\\*Demo TicketGenerator.ps1* in de *PowerShell ISE*
1. Druk op **F5** voert u het script en genereren van klanten en verkoopgegevens ticket.


### <a name="open-the-events-app-to-review-the-current-events"></a>Open de app gebeurtenissen om de huidige gebeurtenissen te bekijken

1. Open de *gebeurtenissen Hub* (http://events.wtp.&lt; gebruiker&gt;. trafficmanager.net) en klik op **Contoso overleg Hall**:

   ![events hub](media/saas-dbpertenant-restore-single-tenant/events-hub.png)

1. Schuif naar de lijst met gebeurtenissen en noteer de laatste gebeurtenis in de lijst:

   ![laatste gebeurtenis](media/saas-dbpertenant-restore-single-tenant/last-event.png)


### <a name="run-the-demo-scenario-to-accidentally-delete-the-last-event"></a>Voer de demo-scenario waarin u de laatste gebeurtenis per ongeluk verwijderen

1. Open... \\Learning-Modules\\voor bedrijfscontinuïteit en noodherstel\\RestoreTenant\\*Demo RestoreTenant.ps1* in de *PowerShell ISE*, en stel de volgende waarde:
   * **$DemoScenario** = **1**, ingesteld op **1** -gebeurtenissen met geen verkopen ticket verwijderen.
1. Druk op **F5** voor het uitvoeren van het script en de laatste gebeurtenis verwijderen. U ziet een bevestigingsbericht vergelijkbaar met het volgende:

   ```Console
   Deleting unsold events from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

1. De pagina van de Contoso-gebeurtenissen wordt geopend. Schuif naar beneden en controleer of dat de gebeurtenis is verwijderd. Als de gebeurtenis nog steeds in de lijst wordt, klikt u op vernieuwen en controleer of dat het is verwijderd.

   ![laatste gebeurtenis](media/saas-dbpertenant-restore-single-tenant/last-event-deleted.png)


## <a name="restore-a-tenant-database-in-parallel-with-the-production-database"></a>Een tenant-database in combinatie met de productiedatabase herstellen

Deze oefening wordt de database Contoso overleg Hall hersteld naar een punt in tijd voordat de gebeurtenis is verwijderd. Wanneer de gebeurtenis is verwijderd in de vorige stappen, die u wilt herstellen en de verwijderde gegevens zien. U hoeft niet te herstellen van de productiedatabase met de verwijderde record, maar u moet de oude database voor toegang tot de oude gegevens voor andere zakelijke redenen herstellen.

 De *terugzetten TenantInParallel.ps1* script maakt een parallelle tenant-database en een parallelle catalogusvermelding beide met de naam *ContosoConcertHall\_oude*. Dit patroon van terugzetten is het meest geschikt is voor het herstellen van een secundaire gegevensverlies of voor compatibiliteit en controle herstelscenario's. Het is ook de aanbevolen aanpak wanneer u [geo-replicatie](sql-database-geo-replication-overview.md).

1. Voltooi de [simuleren van een gebruiker per ongeluk verwijderen van gegevens](#simulate-a-tenant-accidentally-deleting-data) sectie.
1. Open... \\Learning-Modules\\voor bedrijfscontinuïteit en noodherstel\\RestoreTenant\\_Demo RestoreTenant.ps1_ in de *PowerShell ISE*.
1. Ingesteld **$DemoScenario** = **2**, stel dit in op **2** naar *terugzetten tenant parallel*.
1. Druk op **F5** om het script uit te voeren.

Het script Hiermee herstelt de database van de tenant (met een parallelle database) naar een punt in tijd voordat u de gebeurtenis in de vorige sectie hebt verwijderd. Deze maakt een tweede database, verwijdert u alle bestaande catalogus metagegevens die bestaat in deze database, en de database toegevoegd aan de catalogus onder de *ContosoConcertHall\_oude* vermelding.

De demoscript wordt de pagina gebeurtenissen in uw browser geopend. Opmerking van de URL: ```http://events.wtp.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` gegevens uit de herstelde database zien waar *_old* wordt toegevoegd aan de naam.

Schuif de gebeurtenissen in de browser om te bevestigen dat de gebeurtenis is verwijderd uit de vorige sectie is hersteld.

Let op: de herstelde tenant blootstellen als een extra tenant, met een eigen app gebeurtenissen om te bladeren tickets, waarschijnlijk niet hoe u zou een tenant toegang bieden tot gegevens hebt hersteld, maar dient ter illustratie van het patroon terugzetten eenvoudig.

In werkelijkheid, zou u waarschijnlijk alleen deze herstelde database behouden voor een opgegeven periode. U kunt de vermelding van de herstelde tenant verwijderen zodra u klaar bent met het aanroepen van de *verwijderen RestoredTenant.ps1* script.

1. Ingesteld **$DemoScenario** naar **4** selecteren de *verwijderen hersteld tenant* scenario.
1. **Uitvoeren van** **met** **F5**
1. De *ContosoConcertHall\_oude* vermelding is nu verwijderd uit de catalogus. Doorgaan en de pagina gebeurtenissen voor deze tenant in uw browser sluit.


## <a name="restore-a-tenant-in-place-replacing-the-existing-tenant-database"></a>Een tenant aanwezig is, vervangt de bestaande tenant-database herstellen

In deze oefening wordt de tenant Contoso overleg Hall hersteld naar een punt in tijd voordat de gebeurtenis is verwijderd. De *terugzetten TenantInPlace* script herstelt u de huidige tenant-database naar een nieuwe database die verwijst naar een eerder punt in tijd en wordt de oorspronkelijke database verwijderd. Dit patroon van terugzetten is het meest geschikt is voor het herstellen van ernstige gegevensbeschadiging omdat er mogelijk verlies van belangrijke gegevens die de tenant hebben kan om onder te brengen.

1. Open **Demo RestoreTenant.ps1** bestand in PowerShell ISE
1. Ingesteld **$DemoScenario** naar **5** selecteren de *tenant in place scenario herstellen*.
1. Uitvoeren met behulp van **F5**.

Het script de tenant database teruggezet naar een punt vijf minuten voordat de gebeurtenis is verwijderd. Dit gebeurt door eerst de tenant Contoso overleg Hall offline zodat er geen andere updates in de gegevens zijn. Vervolgens wordt een parallelle database door te herstellen vanaf het herstelpunt is gemaakt en om te controleren of de dat naam van de database geen conflict met de naam van de bestaande tenant-database met de naam met een tijdstempel. Vervolgens worden de oude tenant-database wordt verwijderd en de teruggezette database is gewijzigd in de naam van de oorspronkelijke database. Ten slotte Contoso overleg Hall online brengen van de app toegang tot de teruggezette database.

U hersteld de database naar een punt in tijd voordat de gebeurtenis is verwijderd. De gebeurtenissen wordt geopend, zodat de laatste bevestigen gebeurtenis is hersteld.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]

> * Een database herstellen naar een parallelle database (side-by-side)
> * Een database in place herstellen

[Tenant-databaseschema beheren](saas-tenancy-schema-management.md)

## <a name="additional-resources"></a>Aanvullende bronnen

* [Aanvullende zelfstudies waarin voort op de Wingtip SaaS-toepassing bouwen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Overzicht van zakelijke continuïteit met Azure SQL Database](sql-database-business-continuity.md)
* [Meer informatie over back-ups van SQL-Database](sql-database-automated-backups.md)
