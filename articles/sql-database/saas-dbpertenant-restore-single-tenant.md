---
title: Herstellen van een Azure SQL database in een multitenant SaaS-app | Microsoft Docs
description: Informatie over het herstellen van een tenant één SQL-database na het per ongeluk verwijderen van gegevens
keywords: zelfstudie sql-database
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 04/01/2018
ms.author: sstein
ms.reviewer: billgib
ms.openlocfilehash: eb7e87934269a5e1ba453e20f6f409a10dfbda5b
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="restore-a-single-tenant-with-a-database-per-tenant-saas-application"></a>Een één-tenant met een database per tenant SaaS-toepassing terugzetten

De model-database per tenant gemakkelijk een enkel tenant naar een eerder tijdstip herstellen zonder andere tenants.

In deze zelfstudie leert u twee data recovery patronen:

> [!div class="checklist"]

> * Een database herstellen naar een parallelle database (gelijktijdige).
> * Een database aanwezig is, vervangt de bestaande database herstellen.


|||
|:--|:--|
| In een parallelle database herstellen | Dit patroon kan worden gebruikt voor taken zoals controle, controle en naleving om toe te staan een tenant om te controleren van hun gegevens uit een eerder punt. De huidige database van de tenant blijft online en ongewijzigd. |
| In plaats herstellen | Dit patroon wordt meestal gebruikt voor een tenant naar een eerder punt herstellen nadat een tenant per ongeluk worden verwijderd of raken de gegevens. De oorspronkelijke database is offline worden genomen en vervangen door de teruggezette database. |
|||

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De Wingtip SaaS-app wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en Verken de Wingtip SaaS-toepassing](saas-dbpertenant-get-started-deploy.md).
* Azure PowerShell is geïnstalleerd. Zie voor meer informatie [aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-tenant-restore-patterns"></a>Inleiding tot de SaaS-tenant terugzetten patronen

Er zijn twee eenvoudige patronen voor het herstellen van gegevens van een afzonderlijke tenant. Omdat de tenant-databases zijn geïsoleerd van elkaar, heeft herstellen van een tenant geen invloed op een andere tenant-gegevens. De functie Azure SQL Database-punt in tijd terugzetten (PITR) wordt gebruikt in beide patronen. PITR altijd een nieuwe database gemaakt.   

* **Herstellen parallel**: In het eerste patroon een nieuwe parallelle database samen met de huidige database van de tenant is gemaakt. De tenant vervolgens alleen-lezen toegang krijgt tot de teruggezette database. De herstelde gegevens kunnen worden gecontroleerd en mogelijk wordt gebruikt voor het overschrijven van de huidige waarden. Het is aan de app-ontwerper om te bepalen hoe de tenant toegang heeft tot de teruggezette database en welke opties voor herstel beschikbaar. Gewoon waardoor de tenant te bekijken van hun gegevens op een eerder punt mogelijk alle die vereist is in sommige scenario's. 

* **Herstellen-in-place**: het tweede patroon is nuttig als de gegevens is verloren gegaan of beschadigd en de tenant wil terugkeren naar een eerder punt. De tenant wordt offline gemaakt terwijl de database wordt hersteld. De oorspronkelijke database wordt verwijderd en de naam van de herstelde database wordt gewijzigd. De back-keten van de oorspronkelijke database blijft toegankelijk na de verwijdering, zodat u kunt de database herstellen naar een eerder tijdstip, indien nodig.

Als de database gebruikt [geo-replicatie](sql-database-geo-replication-overview.md) herstellen parallel wordt aangeraden dat u alle vereiste gegevens van de herstelde kopie in de oorspronkelijke database kopiëren. Als u de oorspronkelijke database door de teruggezette database vervangen, moet u opnieuw configureert en geo-replicatie opnieuw synchroniseren.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>De database per tenant-toepassingsscripts Wingtip Tickets SaaS ophalen

De scripts Wingtip Tickets SaaS Multitenant Database en de broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Zie voor stappen voor het downloaden en de scripts Wingtip Tickets SaaS deblokkeren de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md).

## <a name="before-you-start"></a>Voordat u begint

Wanneer een database is gemaakt, duurt het kan 10 tot 15 minuten voordat de eerste volledige back-up beschikbaar is in om te herstellen. Als u de toepassing zojuist hebt geïnstalleerd, moet u mogelijk wachten tot een paar minuten voordat u dit scenario.

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>Simuleren van een tenant per ongeluk verwijderen van gegevens

"Om te demonstreren van deze scenario's voor herstel, eerst per ongeluk' verwijdert een gebeurtenis in een van de tenant-databases. 

### <a name="open-the-events-app-to-review-the-current-events"></a>Open de app gebeurtenissen om de huidige gebeurtenissen te bekijken

1. Open de Hub gebeurtenissen (http://events.wtp.&lt; gebruiker&gt;. trafficmanager.net), en selecteer **Contoso overleg Hall**.

   ![Gebeurtenissen Hub](media/saas-dbpertenant-restore-single-tenant/events-hub.png)

2. De lijst met gebeurtenissen en noteer de laatste gebeurtenis in de lijst.

   ![Laatste gebeurtenis wordt weergegeven](media/saas-dbpertenant-restore-single-tenant/last-event.png)


### <a name="accidentally-delete-the-last-event"></a>De laatste gebeurtenis 'Per ongeluk' verwijderen

1. Open in de PowerShell ISE... \\Learning-Modules\\voor bedrijfscontinuïteit en noodherstel\\RestoreTenant\\*Demo RestoreTenant.ps1*, en stel de volgende waarde:

   * **$DemoScenario** = **1**, *laatste gebeurtenis verwijderen (waarbij geen verkopen ticket)*.
2. Druk op F5 voor het uitvoeren van het script en de laatste gebeurtenis verwijderen. Het volgende bevestigingsbericht wordt weergegeven:

   ```Console
   Deleting last unsold event from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

3. De pagina van de Contoso-gebeurtenissen wordt geopend. Schuif naar beneden en controleer of de gebeurtenis is verwijderd. Als de gebeurtenis nog steeds in de lijst wordt, selecteert u **vernieuwen** en controleer of deze is geworden.

   ![Laatste gebeurtenis is verwijderd](media/saas-dbpertenant-restore-single-tenant/last-event-deleted.png)


## <a name="restore-a-tenant-database-in-parallel-with-the-production-database"></a>Een tenant-database in combinatie met de productiedatabase herstellen

Deze oefening wordt de database Contoso overleg Hall hersteld naar een punt in tijd voordat de gebeurtenis is verwijderd. Dit scenario wordt ervan uitgegaan dat u wilt bekijken van de verwijderde gegevens in een parallelle-database.

 De *terugzetten TenantInParallel.ps1* script maakt een parallelle tenant-database met naam *ContosoConcertHall\_oude*, met een parallelle catalogusvermelding. Dit patroon van terugzetten is het meest geschikt is voor het herstellen van een secundaire gegevensverlies. U kunt ook dit patroon gebruiken als u wilt bekijken voor controledoeleinden of compatibiliteit. Dit is de aanbevolen aanpak wanneer u [geo-replicatie](sql-database-geo-replication-overview.md).

1. Voltooi de [simuleren per ongeluk verwijderen van gegevens van een tenant](#simulate-a-tenant-accidentally-deleting-data) sectie.
2. Open in de PowerShell ISE... \\Learning-Modules\\voor bedrijfscontinuïteit en noodherstel\\RestoreTenant\\_Demo RestoreTenant.ps1_.
3. Stel **$DemoScenario** = **2**, *terugzetten tenant parallel*.
4. Het script wordt uitgevoerd, druk op F5.

Het script wordt hersteld van de tenant-database naar een punt in tijd voordat de gebeurtenis wordt verwijderd. De database wordt hersteld naar een nieuwe database met de naam _ContosoConcertHall\_oude_. De metagegevens van de catalogus in deze herstelde database bestaat wordt verwijderd en vervolgens de database is toegevoegd aan de catalogus met behulp van een sleutel die is gemaakt op basis van de *ContosoConcertHall\_oude* naam.

De demoscript wordt de pagina gebeurtenissen voor deze nieuwe tenant-database in uw browser geopend. Opmerking van de URL ```http://events.wingtip-dpt.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` dat op deze pagina gegevens uit de herstelde database worden waar *_old* wordt toegevoegd aan de naam.

Schuif de gebeurtenissen in de browser om te bevestigen dat de gebeurtenis is verwijderd uit de vorige sectie is hersteld.

De herstelde tenant als een extra tenant, met een eigen app gebeurtenissen blootstellen is waarschijnlijk niet hoe u een tenant toegang bieden tot herstelde gegevens. Het dient ter illustratie van het patroon terugzetten. Normaal gesproken alleen-lezen toegang geven tot de oude gegevens en de teruggezette database behouden voor een opgegeven periode. In het voorbeeld kunt u de vermelding herstelde tenant verwijderen nadat u bent nu klaar door het uitvoeren van de _verwijderen hersteld tenant_ scenario.

1. Stel **$DemoScenario** = **4**, *verwijderen hersteld tenant*.
2. Het script wordt uitgevoerd, druk op F5.
3. De *ContosoConcertHall\_oude* vermelding is nu verwijderd uit de catalogus. De pagina gebeurtenissen voor deze tenant in uw browser sluit.


## <a name="restore-a-tenant-in-place-replacing-the-existing-tenant-database"></a>Een tenant aanwezig is, vervangt de bestaande tenant-database herstellen

In deze oefening herstelt de Contoso overleg Hall tenant naar een punt voordat de gebeurtenis is verwijderd. De *terugzetten TenantInPlace* script Hiermee herstelt u de database van een tenant naar een nieuwe database en de oorspronkelijke verwijdert. Dit patroon terugzetten is het meest geschikt voor het herstellen van ernstige gegevensbeschadiging, en de tenant wellicht om verlies van belangrijke gegevens mogelijk te maken.

1. Open in de PowerShell ISE, de **Demo RestoreTenant.ps1** bestand.
2. Stel **$DemoScenario** = **5**, *terugzetten tenant erin*.
3. Het script wordt uitgevoerd, druk op F5.

Het script wordt de tenant-database naar een punt hersteld voordat de gebeurtenis is verwijderd. Het duurt eerst de tenant Contoso overleg Hall offline zet om te voorkomen dat verdere updates. Vervolgens wordt er een parallelle database gemaakt door te herstellen vanaf het herstelpunt. De teruggezette database heet met een tijdstempel om ervoor te zorgen dat de databasenaam niet conflicteert met de naam van de bestaande tenant-database. Vervolgens worden de oude tenant-database wordt verwijderd en de teruggezette database is gewijzigd in de naam van de oorspronkelijke database. Ten slotte Contoso overleg Hall online brengen van de app toegang tot de teruggezette database.

U hersteld de database naar een punt in tijd voordat de gebeurtenis is verwijderd. Wanneer de **gebeurtenissen** pagina wordt geopend, bevestig dat de laatste gebeurtenis is hersteld.

Nadat u de database teruggezet, duurt het een andere 10 tot 15 minuten voordat de eerste volledige back-up beschikbaar is in opnieuw om te herstellen. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]

> * Een database herstellen naar een parallelle database (gelijktijdige).
> * Een database terugzetten in plaats.

Probeer de [beheren tenant-databaseschema](saas-tenancy-schema-management.md) zelfstudie.

## <a name="additional-resources"></a>Aanvullende resources

* [Aanvullende zelfstudies die zijn gebaseerd op de Wingtip SaaS-toepassing](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Overzicht van zakelijke continuïteit met Azure SQL Database](sql-database-business-continuity.md)
* [Meer informatie over back-ups van SQL-Database](sql-database-automated-backups.md)
