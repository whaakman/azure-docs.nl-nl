---
title: Een Azure SQL Database-schema beheren in een app met meerdere tenants | Microsoft Docs
description: Het schema voor meerdere tenants beheren in een toepassing met meerdere tenants die gebruikmaakt van Azure SQL Database
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
ms.date: 07/28/2017
ms.author: billgib; sstein
ms.openlocfilehash: c3eaa4d490b61b746e427d2fe2640ae5cdd6032c
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="manage-schema-for-multiple-tenants-in-a-multi-tenant-application-that-uses-azure-sql-database"></a>Schema voor meerdere tenants in een multitenant-toepassing die gebruikmaakt van Azure SQL Database beheren

De [eerste Wingtip Tickets SaaS Database Per Tenant zelfstudie](saas-dbpertenant-get-started-deploy.md) ziet u hoe de app kunt inrichten van een tenant-database en deze te registreren in de catalogus. Net als elke toepassing, de app Wingtip Tickets SaaS Database Per Tenant gedurende een periode wordt aangepast en op tijdstippen waarvoor wijzigingen in de database. Dit kunnen bijvoorbeeld wijzigingen zijn aan het schema en de referentiegegevens, of er kan routineus database-onderhoud worden uitgevoerd om ervoor te zorgen dat de app optimaal presteert. Bij een SaaS-toepassing moeten deze wijzigingen op een gecoördineerde wijze worden geïmplementeerd in een mogelijk enorme reeks tenantdatabases. Ze moeten worden opgenomen in het inrichtingsproces deze wijzigingen worden in de toekomst tenant-databases.

In deze zelfstudie worden twee scenario's beschreven: referentiegegevensupdates implementeren voor alle tenants en een index retourneren van de tabel met de referentiegegevens. De [elastische taken](sql-database-elastic-jobs-overview.md) functie deze bewerkingen uitvoeren op alle tenants wordt gebruikt en de *gouden* tenant-database die wordt gebruikt als een sjabloon voor nieuwe databases.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]

> * De account van een taak maken
> * Vragen over meerdere tenants
> * Gegevens in alle tenantdatabases bijwerken
> * Een index in een tabel maken in alle tenantdatabases


Voor het voltooien van deze zelfstudie moet u ervoor zorgen dat aan de volgende vereisten is voldaan:

* De app Wingtip Tickets SaaS Database Per Tenant wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de toepassing Wingtip Tickets SaaS Database Per Tenant verkennen](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.
* De laatste versie van SQL Server Management Studio (SSMS) moet zijn geïnstalleerd. [SSMS downloaden en installeren](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

> [!NOTE]
> Deze zelfstudie maakt gebruik van functies van de service SQL-Database die zich in een beperkte preview (elastische Database taken). Als u deze zelfstudie wilt volgen, stuurt u uw abonnements-id naar SaaSFeedback@microsoft.com, met als onderwerp 'Elastic Jobs Preview'. Wanneer u de bevestiging hebt ontvangen dat uw abonnement is ingeschakeld, [downloadt en installeert u de taak-cmdlets van de voorlopige versie](https://github.com/jaredmoo/azure-powershell/releases). Dit voorbeeld is beperkt, dus neem contact op met SaaSFeedback@microsoft.com voor vragen over of ondersteuning.

## <a name="introduction-to-saas-schema-management-patterns"></a>Inleiding tot de SaaS-schema management patronen

Het SaaS-patroon met één tenant per database is enorm handig door de gegevensisolatie die hierdoor ontstaat, maar tegelijkertijd ontstaat er extra complexiteit omdat er vele databases moeten worden onderhouden en beheerd. [Elastische taken](sql-database-elastic-jobs-overview.md) vereenvoudigt het onderhoud en het beheer van de SQL-gegevenslaag. Met Elastische taken kunt u veilig en betrouwbaar taken uitvoeren (T-SQL-scripts) voor een groep databases, onafhankelijk van de interactie met of invoer van gebruikers. Deze methode kan worden gebruikt om wijzigingen in het schema en in de algemene referentiegegevens te implementeren voor alle tenants in een toepassing. Elastische taken kan ook worden gebruikt om een *basis*exemplaar van de gebruikte database op te slaan voor het maken van nieuwe tenants. Op die manier beschikken deze altijd over het nieuwste schema en de meest recente referentiegegevens.

![scherm](media/saas-tenancy-schema-management/schema-management.png)


## <a name="elastic-jobs-limited-preview"></a>Beperkte preview voor Elastische taken

Er is een nieuwe versie beschikbaar van Elastische taken; dit is nu een ingebouwde functie in Azure SQL Database (waarvoor geen extra services of onderdelen zijn vereist). Van deze nieuwe versie van Elastische taken is momenteel een beperkte preview beschikbaar. Deze beperkte preview biedt ondersteuning voor PowerShell voor het maken van taakaccounts en voor T-SQL voor het maken en beheren van taken.

> [!NOTE]
> Deze zelfstudie maakt gebruik van functies van de service SQL-Database die zich in een beperkte preview (elastische Database taken). Als u deze zelfstudie wilt volgen, stuurt u uw abonnements-id naar SaaSFeedback@microsoft.com, met als onderwerp 'Elastic Jobs Preview'. Wanneer u de bevestiging hebt ontvangen dat uw abonnement is ingeschakeld, [downloadt en installeert u de taak-cmdlets van de voorlopige versie](https://github.com/jaredmoo/azure-powershell/releases). Dit voorbeeld is beperkt, dus neem contact op met SaaSFeedback@microsoft.com voor vragen over of ondersteuning.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>De toepassingsscripts Wingtip Tickets SaaS Database Per Tenant ophalen

De scripts Wingtip Tickets SaaS multitenant Database en de broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de scripts Wingtip Tickets SaaS deblokkeren.

## <a name="create-a-job-account-database-and-new-job-account"></a>Een taakaccountdatabase en een nieuw taakaccount maken

In deze zelfstudie moet u PowerShell gebruiken om de taakaccountdatabase en het taakaccount te maken. Net als MSDB en SQL Agent maakt Elastische taken gebruik van een Azure SQL-database om taakdefinities, de taakstatus en de geschiedenis op te slaan. Wanneer het taakaccount is gemaakt, kunt u direct aan de slag met het maken en bewaken van taken.

1. **In PowerShell ISE**open... \\Learning-Modules\\Schemabeheer\\*Demo SchemaManagement.ps1*.
1. Druk op **F5** om het script uit te voeren.

Met het script *Demo-SchemaManagement.ps1* wordt het script *Deploy-SchemaManagement.ps1* aangeroepen om op de catalogusserver een *S2*-database te maken met de naam **jobaccount**. Het taakaccount wordt dan gemaakt. De database van dat account wordt doorgegeven als parameter bij de aanroep voor het maken van het taakaccount.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Een taak maken om nieuwe referentiegegevens te implementeren voor alle tenants

Elke tenantdatabase bevat een reeks locatietypen waarmee wordt gedefinieerd welke soorten evenementen bij die locatie worden georganiseerd. In deze oefening implementeert u een update voor alle tenantdatabases waarbij twee nieuwe locatietypen worden toegevoegd: *Motorcycle Racing* en *Swimming Club*. Deze locatietypen sluiten aan op de achtergrondafbeelding die u in de app voor tenantevenementen ziet.

Klik op het vervolgkeuzemenu Locatietype en controleer of er slechts tien locatietypen beschikbaar zijn. Bekijk ook of Motorcycle Racing en Swimming Club niet in de lijst staan.

U gaat nu een taak maken om de tabel *VenueTypes* in alle tenantdatabases bij te werken met de nieuwe locatietypen.

Voor het maken van een nieuwe taak gebruikt u een reeks in het systeem opgeslagen procedures uit de jobaccount-database die is gemaakt tijdens het maken van het taakaccount.

1. SSMS opent en verbinding maken met de catalogusserver: catalogus-dpt -\<gebruiker\>. database.windows.net server
1. Ook verbinding maken met de server van de tenant: tenants1-dpt -\<gebruiker\>. database.windows.net
1. Blader naar de *contosoconcerthall* database op de *tenants1-dpt -\<gebruiker\>*  server en de query de *VenueTypes* tabel om te bevestigen dat *Motor race* en *zwemmen vereniging* **niet** in de lijst met resultaten.
1. Open het bestand in SSMS... \\Learning-Modules\\Schemabeheer\\DeployReferenceData.sql
1. Wijzig de instructie: Stel @wtpUser = &lt;gebruiker&gt; en vervang de waarde van de gebruiker gebruikt tijdens de implementatie van de app Wingtip Tickets SaaS Database Per Tenant
1. Zorg dat u verbinding hebt met de jobaccount-database en druk op **F5** om het script uit te voeren

Houd rekening met het volgende in de *DeployReferenceData.sql* script:
* Met **sp\_add\_target\_group** maakt u de doelgroepnaam DemoServerGroup. Nu gaat u de beoogde leden toevoegen.
* **SP\_toevoegen\_doel\_groep\_lid** voegt een *server* doeltype lid, die alle databases binnen die server acht (dit is de tenants1 - dpt - &lt;Gebruiker&gt; -server met de tenant-databases) op het moment van taak uitvoering moet worden opgenomen in de taak is het toevoegen van de tweede een *database* lidtype, specifiek de 'golden' als doel database (basetenantdb) die zich in een catalogus-dpt -&lt;gebruiker&gt; server, en ten slotte een andere *database* lidtype groep wilt opnemen, de adhocanalytics-database die wordt gebruikt in een later als doel zelfstudie.
* Met **sp\_add\_job** maakt u een taak die ook wel 'Implementatie van referentiegegevens' heet
* **SP\_toevoegen\_taakstap** wordt gemaakt met de tekst van T-SQL-opdracht voor het bijwerken van de verwijzingstabel, VenueTypes taakstap
* De resterende weergaven in het script tonen het bestaan van de objecten en controleren de taakuitvoering. Gebruik deze query's om te controleren van de statuswaarde in de **lifecycle** kolom om te bepalen wanneer de taak is voltooid op alle databases van de tenant en de twee extra databases met de referentietabel.

In SSMS, bladert u naar de *contosoconcerthall* database op de *tenants1-dpt -\<gebruiker\>*  server en de query de *VenueTypes* tabel naar Bevestig dat *motor race* en *zwemmen vereniging* **zijn** nu in de lijst met resultaten.


## <a name="create-a-job-to-manage-the-reference-table-index"></a>Een taak maken voor het beheren van de referentietabelindex

Net als bij de vorige oefening, wordt bij deze oefening een taak gemaakt om de index op de primaire sleutel van de referentietabel opnieuw op te bouwen. Dit is een typische databasebeheerbewerking die een beheerder zal moeten uitvoeren bij het laden van veel gegevens in een tabel.

Maak een taak met dezelfde in het systeem opgeslagen procedures.

1. SSMS opent en verbinding maken met de catalogus-dpt -&lt;gebruiker&gt;. database.windows.net server
1. Open het bestand …\\Learning Modules\\Schemabeheer\\OnlineReindex.sql
1. Klik met de rechtermuisknop, selecteer verbinding en verbinding maken met de catalogus-dpt -&lt;gebruiker&gt;. database.windows.net server, indien nog niet verbonden
1. Zorg dat u verbinding hebt met de jobaccount-database en druk op F5 om het script uit te voeren

Houd rekening met het volgende in de *OnlineReindex.sql* script:
* **SP\_toevoegen\_taak** maakt een nieuwe taak genaamd ' Online opnieuw indexeren PK\_\_VenueTyp\_\_265E44FD7FD4C885 '
* **SP\_toevoegen\_taakstap** taakstap met T-SQL-opdrachttekst voor het bijwerken van de index wordt gemaakt
* De resterende weergaven in de script monitor taak wordt uitgevoerd. Gebruik deze query's om te controleren van de statuswaarde in de **lifecycle** kolom om te bepalen wanneer de taak is voltooid op op alle leden van de doel-groep.



## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * Een taakaccount maken om query's uit te voeren voor meerdere tenants
> * Gegevens in alle tenantdatabases bijwerken
> * Een index in een tabel maken in alle tenantdatabases

Probeer vervolgens de [Ad-hoc reporting zelfstudie](saas-tenancy-adhoc-analytics.md).


## <a name="additional-resources"></a>Aanvullende bronnen

* [Aanvullende zelfstudies waarin voort op de toepassingsimplementatie Wingtip Tickets SaaS Database Per Tenant bouwen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Uitgeschaalde clouddatabases beheren](sql-database-elastic-jobs-overview.md)
* [Uitgeschaalde clouddatabases maken en beheren](sql-database-elastic-jobs-create-and-manage.md)