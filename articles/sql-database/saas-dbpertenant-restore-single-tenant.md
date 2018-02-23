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
ms.openlocfilehash: 46471073f88247510f45d6c4152afa43be6e1aaa
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2018
---
# <a name="restore-a-single-tenant-with-a-database-per-tenant-saas-application"></a>Een enkel tenant met een database per tenant SaaS-toepassing terugzetten

De database per tenant model maakt het is gemakkelijk een enkel tenant naar een eerder tijdstip herstellen zonder enige impact op andere tenants.

In deze zelfstudie leert u twee data recovery patronen:

> [!div class="checklist"]

> * Een database herstellen naar een parallelle database (side-by-side)
> * Een database aanwezig is, vervangt de bestaande database terugzetten


|||
|:--|:--|
| **In een parallelle database herstellen** | Dit patroon kan worden gebruikt voor controle, controle, compliance, enzovoort om toe te staan een tenant om te controleren van hun gegevens uit een eerder punt.  De huidige database van de tenant blijft online en ongewijzigd. |
| **In-place herstellen** | Dit patroon wordt meestal gebruikt voor een tenant naar een eerder punt herstellen nadat een tenant per ongeluk worden verwijderd of raken de gegevens. De oorspronkelijke database wordt offline gezet en wordt vervangen door de teruggezette database. |
|||

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De Wingtip SaaS-app wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en Verken de Wingtip SaaS-toepassing](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

## <a name="introduction-to-the-saas-tenant-restore-patterns"></a>Inleiding tot de SaaS-tenant terugzetten patronen

Er zijn twee eenvoudige patronen voor het herstellen van gegevens van een afzonderlijke tenant. Omdat de tenant-databases zijn geïsoleerd van elkaar, heeft herstellen van een tenant geen invloed op een andere tenant-gegevens.  De functie SQL-Database punt in tijd terugzetten (PITR) wordt gebruikt in beide patronen.  PITR altijd een nieuwe database gemaakt.   

In het eerste patroon **herstellen parallel**, een nieuwe parallelle database samen met de huidige database van de tenant is gemaakt. De tenant vervolgens alleen-lezen toegang krijgt tot de teruggezette database. De herstelde gegevens kunnen worden gecontroleerd en mogelijk wordt gebruikt voor het overschrijven van de huidige waarden. Het is aan de app-ontwerper om te bepalen hoe de tenant toegang heeft tot de teruggezette database en welke opties voor herstel beschikbaar. Gewoon waardoor de tenant te bekijken van hun gegevens op een eerder punt mogelijk alle die vereist is in sommige scenario's. 

Het tweede patroon **herstellen-in-place**, is handig als gegevens verloren of beschadigd en de tenant wil terugkeren naar een eerder punt.  De tenant is offline gezet terwijl de database wordt hersteld. De oorspronkelijke database wordt verwijderd en de naam van de herstelde database wordt gewijzigd. De back-keten van de oorspronkelijke database blijft toegankelijk na de verwijdering, zodat u de database naar een eerder tijdstip herstellen indien nodig.

Als de database gebruikt [geo-replicatie](sql-database-geo-replication-overview.md) herstellen parallel wordt aangeraden geen vereiste gegevens kopiëren van de herstelde kopie in de oorspronkelijke database. Als u de oorspronkelijke database door de teruggezette database vervangen, moet u opnieuw configureert en geo-replicatie opnieuw synchroniseren.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>De toepassingsscripts Wingtip Tickets SaaS Database Per Tenant ophalen

De scripts Wingtip Tickets SaaS multitenant Database en de broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de scripts Wingtip Tickets SaaS deblokkeren.

## <a name="before-you-start"></a>Voordat u begint

Wanneer een database is gemaakt, duurt het kan 10-15 minuten voordat de eerste volledige back-up beschikbaar is in om te herstellen.  Als u de toepassing alleen zojuist hebt geïnstalleerd, moet u wellicht wacht een paar minuten voordat u dit scenario.

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>Simuleren van een tenant per ongeluk verwijderen van gegevens

Ter illustratie van deze herstelscenario's eerst *'per ongeluk'* een gebeurtenis in een van de tenant-databases verwijderen. 

### <a name="open-the-events-app-to-review-the-current-events"></a>Open de app gebeurtenissen om de huidige gebeurtenissen te bekijken

1. Open de *gebeurtenissen Hub* (http://events.wtp.&lt; gebruiker&gt;. trafficmanager.net) en klik op **Contoso overleg Hall**:

   ![events hub](media/saas-dbpertenant-restore-single-tenant/events-hub.png)

1. Schuif naar de lijst met gebeurtenissen en noteer de laatste gebeurtenis in de lijst:

   ![laatste gebeurtenis](media/saas-dbpertenant-restore-single-tenant/last-event.png)


### <a name="accidentally-delete-the-last-event"></a>De laatste gebeurtenis 'Per ongeluk' verwijderen

1. Open... \\Learning-Modules\\voor bedrijfscontinuïteit en noodherstel\\RestoreTenant\\*Demo RestoreTenant.ps1* in de *PowerShell ISE*, en stel de volgende waarde:
   * **$DemoScenario** = **1**, laatste gebeurtenis verwijderen (waarbij geen verkopen ticket).
1. Druk op **F5** voor het uitvoeren van het script en de laatste gebeurtenis verwijderen. U ziet het volgende bevestigingsbericht:

   ```Console
   Deleting last unsold event from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

1. De pagina van de Contoso-gebeurtenissen wordt geopend. Schuif naar beneden en controleer of dat de gebeurtenis is verwijderd. Als de gebeurtenis nog steeds in de lijst wordt, klikt u op vernieuwen en controleer of dat het is verwijderd.

   ![laatste gebeurtenis](media/saas-dbpertenant-restore-single-tenant/last-event-deleted.png)


## <a name="restore-a-tenant-database-in-parallel-with-the-production-database"></a>Een tenant-database in combinatie met de productiedatabase herstellen

Deze oefening wordt de database Contoso overleg Hall hersteld naar een punt in tijd voordat de gebeurtenis is verwijderd. In dit scenario wordt ervan uitgegaan dat u alleen wilt bekijken van de verwijderde gegevens in een parallelle-database.

 De *terugzetten TenantInParallel.ps1* script maakt een parallelle tenant-database met naam *ContosoConcertHall\_oude*, met een parallelle catalogusvermelding. Dit patroon van terugzetten is het meest geschikt is voor het herstellen van een secundaire gegevensverlies, of als u in acht moet nemen van gegevens voor controledoeleinden of compatibiliteit. Het is ook de aanbevolen aanpak wanneer u [geo-replicatie](sql-database-geo-replication-overview.md).

1. Voltooi de [simuleren per ongeluk verwijderen van gegevens van een tenant](#simulate-a-tenant-accidentally-deleting-data) sectie.
1. In de *PowerShell ISE*open... \\Learning-Modules\\voor bedrijfscontinuïteit en noodherstel\\RestoreTenant\\_Demo RestoreTenant.ps1_.
1. Stel **$DemoScenario** = **2**, *terugzetten tenant parallel*.
1. Druk op **F5** om het script uit te voeren.

Het script wordt hersteld van de tenant-database naar een punt in tijd voordat de gebeurtenis wordt verwijderd. De database wordt hersteld naar de nieuwe database met de naam _ContosoConcertHall\_oude_. De metagegevens van de catalogus in deze herstelde database bestaat wordt verwijderd en vervolgens de database is toegevoegd aan de catalogus met een sleutel die is gemaakt op basis van de *ContosoConcertHall\_oude* naam.

De demoscript wordt de pagina gebeurtenissen voor deze nieuwe tenant-database in uw browser geopend. Opmerking van de URL: ```http://events.wingtip-dpt.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` dat gegevens uit de herstelde database door deze pagina wordt weergegeven waarin *_old* wordt toegevoegd aan de naam.

Schuif de gebeurtenissen in de browser om te bevestigen dat de gebeurtenis is verwijderd uit de vorige sectie is hersteld.

Let op: de herstelde tenant blootstellen als een extra tenant, met een eigen app gebeurtenissen waarschijnlijk niet hoe u zou een tenant toegang bieden tot gegevens hebt hersteld, maar dient ter illustratie van het patroon terugzetten. In werkelijkheid zou u waarschijnlijk alleen-lezen toegang geven tot de oude gegevens en deze herstelde database behouden voor een opgegeven periode. In het voorbeeld kunt u de herstelde tenant-vermelding verwijderen zodra u klaar bent door het uitvoeren van de _verwijderen hersteld tenant_ scenario.

1. Stel **$DemoScenario** = **4**, *verwijderen hersteld tenant*
1. **Uitvoeren van** **met** **F5**
1. De *ContosoConcertHall\_oude* vermelding is nu verwijderd uit de catalogus. Doorgaan en de pagina gebeurtenissen voor deze tenant in uw browser sluit.


## <a name="restore-a-tenant-in-place-replacing-the-existing-tenant-database"></a>Een tenant aanwezig is, vervangt de bestaande tenant-database herstellen

In deze oefening herstelt de Contoso overleg Hall tenant naar een punt voordat de gebeurtenis is verwijderd. De *terugzetten TenantInPlace* script Hiermee herstelt u de database van een tenant naar een nieuwe database en de oorspronkelijke verwijdert.  Dit patroon terugzetten is het meest geschikt is om te herstellen vanaf ernstige gegevensbeschadiging omdat er mogelijk verlies van belangrijke gegevens die de tenant hebben kan om onder te brengen.

1. Open **Demo RestoreTenant.ps1** bestand in PowerShell ISE
1. Stel **$DemoScenario** = **5**, *terugzetten tenant erin*.
1. Uitvoeren met behulp van **F5**.

Het script wordt de tenant-database naar een punt hersteld voordat de gebeurtenis is verwijderd. Het eerst duurt de tenant van Contoso overleg Hall offline om te voorkomen dat verdere updates. Vervolgens wordt er een parallelle database gemaakt door te herstellen vanaf het herstelpunt.  De teruggezette database heet met een tijdstempel om te controleren of de dat naam van de database geen conflict met de naam van de bestaande tenant-database. Vervolgens worden de oude tenant-database wordt verwijderd en de teruggezette database is gewijzigd in de naam van de oorspronkelijke database. Ten slotte Contoso overleg Hall online brengen van de app toegang tot de teruggezette database.

U hersteld de database naar een punt in tijd voordat de gebeurtenis is verwijderd. De gebeurtenissen wordt geopend, zodat de laatste bevestigen gebeurtenis is hersteld.

Houd er rekening mee dat wanneer u de database herstellen het duurt nog 10-15 minuten voordat de eerste volledige back beschikbaar is in opnieuw om te herstellen. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]

> * Een database herstellen naar een parallelle database (side-by-side)
> * Een database in place herstellen

[Tenant-databaseschema beheren](saas-tenancy-schema-management.md)

## <a name="additional-resources"></a>Aanvullende resources

* [Aanvullende zelfstudies waarin voort op de Wingtip SaaS-toepassing bouwen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Overzicht van zakelijke continuïteit met Azure SQL Database](sql-database-business-continuity.md)
* [Meer informatie over back-ups van SQL-Database](sql-database-automated-backups.md)
