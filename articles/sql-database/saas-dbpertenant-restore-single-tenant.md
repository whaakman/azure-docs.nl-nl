---
title: Een Azure SQL-database in een multitenant SaaS-app herstellen | Microsoft Docs
description: Leer hoe u een individuele tenant SQL-database herstellen na het per ongeluk verwijderen van gegevens
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
ms.date: 12/04/2018
ms.openlocfilehash: 4059b0f979e7e6856905f1759129167d62d7b5f5
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53274425"
---
# <a name="restore-a-single-tenant-with-a-database-per-tenant-saas-application"></a>Een individuele tenant met een database-per-tenant SaaS-toepassing terugzetten

Het database-per-tenant-model kunt eenvoudig een individuele tenant terugzetten naar een eerdere punt in tijd zonder gevolgen voor andere tenants.

In deze zelfstudie leert u twee data recovery patronen:

> [!div class="checklist"]
> * Een database herstellen in een parallelle database (naast elkaar).
> * Een database aanwezig is, vervangt de bestaande database terugzetten.

|||
|:--|:--|
| In een parallelle database herstellen | Dit patroon kan worden gebruikt voor taken, zoals controle, controle en naleving om toe te staan een tenant om te controleren van hun gegevens uit een eerder tijdstip. De huidige database van de tenant blijft online en ongewijzigd. |
| Herstellen op locatie | Dit patroon wordt doorgaans gebruikt voor het herstellen van een tenant naar een eerder tijdstip, nadat een tenant per ongeluk worden verwijderd of raken de gegevens. De oorspronkelijke database is offline worden genomen en vervangen door de herstelde database. |
|||

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De Wingtip SaaS-app wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en verkennen van de Wingtip SaaS-toepassing](saas-dbpertenant-get-started-deploy.md).
* Azure PowerShell is geïnstalleerd. Zie voor meer informatie, [aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-tenant-restore-patterns"></a>Inleiding tot de SaaS-patronen voor het herstellen van tenant

Er zijn twee eenvoudige patronen voor het herstellen van een afzonderlijke tenant-gegevens. Omdat de tenant-databases worden geïsoleerd van elkaar worden verbonden, heeft herstellen van één tenant geen invloed op een andere tenant-gegevens. De functie van Azure SQL Database point-in-time-restore (PITR) wordt gebruikt in beide patronen. PITR altijd een nieuwe database gemaakt.

* **Herstellen parallel**: In het eerste patroon wordt een nieuwe parallelle database samen met de huidige database van de tenant gemaakt. De tenant is vervolgens alleen-lezen toegang krijgen tot de herstelde database. De herstelde gegevens kunnen worden bekeken en mogelijk gebruikt voor het overschrijven van de huidige gegevenswaarden van. Het is aan de app-ontwerper om te bepalen hoe de tenant toegang heeft tot de herstelde database en welke opties voor herstel beschikbaar. Gewoon waardoor de tenant om te controleren van hun gegevens op een eerder tijdstip mogelijk is alles wat in sommige scenario's vereist.

* **Herstellen in plaats**: Het tweede patroon is handig als gegevens verloren of beschadigd en de tenant wil om terug te zetten naar een eerder tijdstip. De tenant wordt gemaakt offline terwijl de database is hersteld. De oorspronkelijke database wordt verwijderd en de naam van de herstelde database is gewijzigd. De back-keten van de oorspronkelijke database blijft toegankelijk na de verwijdering, zodat u kunt de database herstellen naar een eerder tijdstip, indien nodig.

Als de database maakt gebruik van [actieve geo-replicatie](sql-database-active-geo-replication.md) en parallel is teruggezet, wordt aangeraden dat u alle vereiste gegevens van het herstelde kopie naar de oorspronkelijke database kopiëren. Als u de oorspronkelijke database door de herstelde database vervangen, moet u opnieuw configureren en geo-replicatie opnieuw synchroniseren.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>De database-per-tenant-toepassing Wingtip Tickets SaaS scripts ophalen

De Wingtip Tickets SaaS Multitenant-Database-scripts en broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Zie voor stappen voor het downloaden en blokkering opheffen van de Wingtip Tickets SaaS-scripts, de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md).

## <a name="before-you-start"></a>Voordat u begint

Wanneer een database wordt gemaakt, duurt het 10 tot 15 minuten voordat de eerste volledige back-up beschikbaar is voor het herstellen van. Als u de toepassing zojuist hebt geïnstalleerd, moet u mogelijk wachten tot een paar minuten voordat u dit scenario.

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>Een tenant per ongeluk verwijderen van gegevens simuleren

"Om te demonstreren van deze scenario's voor herstel, eerst per ongeluk" verwijdert een gebeurtenis in een van de tenant-databases. 

### <a name="open-the-events-app-to-review-the-current-events"></a>Open de app gebeurtenissen om de huidige gebeurtenissen te bekijken

1. Open de Hub-gebeurtenissen (http://events.wtp.&lt; gebruiker&gt;. trafficmanager.net), en selecteer **Contoso Concert Hall**.

   ![Events Hub](media/saas-dbpertenant-restore-single-tenant/events-hub.png)

2. De lijst met gebeurtenissen en noteer de laatste gebeurtenis in de lijst.

   ![Laatste gebeurtenis wordt weergegeven](media/saas-dbpertenant-restore-single-tenant/last-event.png)

### <a name="accidentally-delete-the-last-event"></a>"Per ongeluk" verwijdert de laatste gebeurtenis

1. Open in de PowerShell ISE... \\Learning Modules\\zakelijke continuïteit en herstel na noodgevallen\\RestoreTenant\\*Demo-RestoreTenant.ps1*, en stel de volgende waarde:

   * **$DemoScenario** = **1**, *laatste gebeurtenis van verwijderen (waarbij geen kaartverkoop)*.
2. Druk op F5 op het script uitvoert en de laatste gebeurtenis verwijderen. Het volgende bevestigingsbericht wordt weergegeven:

   ```Console
   Deleting last unsold event from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

3. De pagina van de Contoso-gebeurtenissen wordt geopend. Schuif omlaag en controleer of de gebeurtenis is verdwenen. Als de gebeurtenis nog steeds in de lijst wordt, selecteert u **vernieuwen** en controleer of deze is geworden.
   ![Laatste gebeurtenis is verwijderd](media/saas-dbpertenant-restore-single-tenant/last-event-deleted.png)

## <a name="restore-a-tenant-database-in-parallel-with-the-production-database"></a>Een tenantdatabase in combinatie met de productiedatabase herstellen

In deze oefening wordt de database Contoso Concert Hall hersteld naar een tijdstip voordat de gebeurtenis is verwijderd. In dit scenario wordt ervan uitgegaan dat u wilt bekijken van de verwijderde gegevens in een parallelle-database.

 De *terugzetten TenantInParallel.ps1* script maakt u een parallelle tenantdatabase met de naam *ContosoConcertHall\_oude*, met een parallelle catalogusvermelding. Dit patroon van herstel is het meest geschikt is voor het herstellen van een secundaire gegevens verloren gaan. U kunt ook dit patroon gebruiken als u nodig hebt om gegevens voor naleving of controledoeleinden te bekijken. Het is de aanbevolen benadering wanneer u [actieve geo-replicatie](sql-database-active-geo-replication.md).

1. Voltooi de [simuleren van een tenant per ongeluk verwijderen van gegevens](#simulate-a-tenant-accidentally-deleting-data) sectie.
2. Open in de PowerShell ISE... \\Learning Modules\\zakelijke continuïteit en herstel na noodgevallen\\RestoreTenant\\_Demo-RestoreTenant.ps1_.
3. Stel **$DemoScenario** = **2**, *terugzetten tenant parallel*.
4. Druk op F5 om het script uitvoert.

Het script wordt de tenant-database hersteld naar een tijdstip voordat u de gebeurtenis wordt verwijderd. De database wordt hersteld naar een nieuwe database met de naam _ContosoConcertHall\_oude_. De metagegevens van de catalogus die deel uitmaakt van deze herstelde database wordt verwijderd en vervolgens de database is toegevoegd aan de catalogus met behulp van een sleutel die is samengesteld uit de *ContosoConcertHall\_oude* naam.

De demoscript wordt de pagina gebeurtenissen voor deze nieuwe tenantdatabase in uw browser geopend. Opmerking van de URL ```http://events.wingtip-dpt.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` dat deze pagina ziet u gegevens uit de herstelde database waar *_old* wordt toegevoegd aan de naam.

Schuif de gebeurtenissen die worden vermeld in de browser om te bevestigen dat de gebeurtenis verwijderd in de vorige sectie is hersteld.

De herstelde tenant als een andere tenant, met een eigen app gebeurtenissen bloot is waarschijnlijk niet hoe u een tenant toegang bieden tot herstelde gegevens. Deze dient ter illustratie van het patroon van herstel. Normaal gesproken alleen-lezen toegang geven tot de oude gegevens en de herstelde database behouden voor een opgegeven periode. In het voorbeeld kunt u de vermelding herstelde tenant verwijderen nadat u bent nu klaar door het uitvoeren van de _verwijderen hersteld tenant_ scenario.

1. Stel **$DemoScenario** = **4**, *verwijderen hersteld tenant*.
2. Druk op F5 om het script uitvoert.
3. De *ContosoConcertHall\_oude* vermelding wordt nu verwijderd uit de catalogus. Sluit de pagina gebeurtenissen voor deze tenant in uw browser.

## <a name="restore-a-tenant-in-place-replacing-the-existing-tenant-database"></a>Een tenant aanwezig is, vervangt de bestaande tenantdatabase terugzetten

In deze oefening wordt de tenant Contoso Concert Hall hersteld naar een voordat de gebeurtenis is verwijderd. De *terugzetten TenantInPlace* script een tenantdatabase wordt hersteld naar een nieuwe database en de oorspronkelijke verwijderd. Dit patroon terugzetten is het meest geschikt om te herstellen van ernstige gegevensbeschadiging en de tenant mogelijk om verlies van belangrijke gegevens mogelijk te maken.

1. Open in de PowerShell ISE, de **Demo-RestoreTenant.ps1** bestand.
2. Stel **$DemoScenario** = **5**, *terugzetten tenant aanwezig*.
3. Druk op F5 om het script uitvoert.

Het script worden de tenant-database hersteld naar een punt voordat de gebeurtenis is verwijderd. Het duurt eerst de tenant Contoso Concert Hall offline zet om te voorkomen dat verdere updates. Vervolgens wordt een parallelle database door te herstellen van het herstelpunt gemaakt. De herstelde database is met de naam met een tijdstempel om ervoor te zorgen dat de databasenaam niet conflicteert met de naam van de bestaande tenant-database. Vervolgens worden de oude tenantdatabase wordt verwijderd en de herstelde database is gewijzigd in de naam van de oorspronkelijke database. Ten slotte wordt Contoso Concert Hall online gezet om toe te staan van de apptoegang tot de herstelde database.

Is hersteld u de database naar een tijdstip voordat de gebeurtenis is verwijderd. Wanneer de **gebeurtenissen** pagina wordt geopend, bevestigt u dat de laatste gebeurtenis is hersteld.

Nadat u de database hebt hersteld, duurt het een ander 10 tot 15 minuten voordat de eerste volledige back-up is beschikbaar voor het herstellen van het opnieuw.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een database herstellen in een parallelle database (naast elkaar).
> * Een database herstellen in plaats.

Probeer de [beheren tenant databaseschema](saas-tenancy-schema-management.md) zelfstudie.

## <a name="additional-resources"></a>Aanvullende resources

* [Aanvullende zelfstudies op de Wingtip SaaS-toepassing](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Overzicht van bedrijfscontinuïteit met Azure SQL Database](sql-database-business-continuity.md)
* [Meer informatie over back-ups van SQL-Database](sql-database-automated-backups.md)
