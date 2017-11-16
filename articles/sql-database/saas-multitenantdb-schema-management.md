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
ms.date: 11/14/2017
ms.author: billgib
ms.openlocfilehash: 346177be29ec196464f4f441858222ac5d5eb8c3
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="manage-schema-for-multiple-tenants-in-a-multi-tenant-application-that-uses-azure-sql-database"></a>Schema voor meerdere tenants in een multitenant-toepassing die gebruikmaakt van Azure SQL Database beheren

De [eerste Wingtip Tickets SaaS multitenant databasezelfstudie](saas-multitenantdb-get-started-deploy.md) ziet u hoe de app kunt inrichten van een shard multitenant-database en deze te registreren in de catalogus. Net als elke toepassing, de app Wingtip Tickets SaaS gedurende een periode wordt aangepast en op tijdstippen waarvoor wijzigingen in de database. Dit kunnen bijvoorbeeld wijzigingen zijn aan het schema en de referentiegegevens, of er kan routineus database-onderhoud worden uitgevoerd om ervoor te zorgen dat de app optimaal presteert. Bij een SaaS-toepassing moeten deze wijzigingen op een gecoördineerde wijze worden geïmplementeerd in een mogelijk enorme reeks tenantdatabases. Ze moeten worden opgenomen in het inrichtingsproces deze wijzigingen worden in de toekomst tenant-databases.

In deze zelfstudie worden twee scenario's beschreven: referentiegegevensupdates implementeren voor alle tenants en een index retourneren van de tabel met de referentiegegevens. De [elastische taken](sql-database-elastic-jobs-overview.md) functie wordt gebruikt om uit te voeren deze bewerkingen in de tenant-databases en de *gouden* tenant-database die wordt gebruikt als een sjabloon voor nieuwe databases.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]

> * De account van een taak maken
> * Vragen over meerdere tenants
> * Gegevens in alle tenantdatabases bijwerken
> * Een index in een tabel maken in alle tenantdatabases


Voor het voltooien van deze zelfstudie moet u ervoor zorgen dat aan de volgende vereisten is voldaan:

* De toepassing Wingtip Tickets SaaS multitenant Database wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de toepassing Wingtip Tickets SaaS multitenant Database verkennen](saas-multitenantdb-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.
* De laatste versie van SQL Server Management Studio (SSMS) moet zijn geïnstalleerd. [SSMS downloaden en installeren](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

> [!NOTE]
> *In deze zelfstudie wordt gebruikgemaakt van de functies van de SQL Database-service die deel uitmaken van een beperkte preview (elastische databasetaken). Als u deze zelfstudie wilt volgen, stuurt u uw abonnements-id naar SaaSFeedback@microsoft.com, met als onderwerp 'Elastic Jobs Preview'. Wanneer u de bevestiging hebt ontvangen dat uw abonnement is ingeschakeld, [downloadt en installeert u de taak-cmdlets van de voorlopige versie](https://github.com/jaredmoo/azure-powershell/releases). Dit voorbeeld is beperkt, dus neem contact op met SaaSFeedback@microsoft.com voor vragen over of ondersteuning.*


## <a name="introduction-to-saas-schema-management-patterns"></a>Kennismaking met patronen voor SaaS-schemabeheer

De shard multitenant-databasemodel gebruikt in dit voorbeeld kunt een database tenants naar een willekeurig aantal tenants bevatten. Dit voorbeeld behandelt de mogelijkheid gebruik een combinatie van een multitenant- en één tenant databases, een 'hybride'-model voor het beheer van tenant inschakelen. Onderhoud en beheer van deze databases is ingewikkeld. [Elastische taken](sql-database-elastic-jobs-overview.md) vereenvoudigt het onderhoud en het beheer van de SQL-gegevenslaag. Met Elastische taken kunt u veilig en betrouwbaar taken uitvoeren (T-SQL-scripts) voor een groep databases, onafhankelijk van de interactie met of invoer van gebruikers. Deze methode kan worden gebruikt om wijzigingen in het schema en in de algemene referentiegegevens te implementeren voor alle tenants in een toepassing. Elastische taken kan ook worden gebruikt om een *basis*exemplaar van de gebruikte database op te slaan voor het maken van nieuwe tenants. Op die manier beschikken deze altijd over het nieuwste schema en de meest recente referentiegegevens.

![scherm](media/saas-multitenantdb-schema-management/schema-management.png)

## <a name="elastic-jobs-limited-preview"></a>Beperkte preview voor Elastische taken

Er is een nieuwe versie beschikbaar van Elastische taken; dit is nu een ingebouwde functie in Azure SQL Database (waarvoor geen extra services of onderdelen zijn vereist). Van deze nieuwe versie van Elastische taken is momenteel een beperkte preview beschikbaar. Deze beperkte preview biedt ondersteuning voor PowerShell voor het maken van taakaccounts en voor T-SQL voor het maken en beheren van taken.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-scripts"></a>Ophalen van de toepassingsscripts Wingtip Tickets SaaS multitenant Database

De scripts Wingtip Tickets SaaS multitenant Database en de broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS MultiTenantDB](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub-opslagplaats. <!-- [Steps to download the Wingtip Tickets SaaS Multi-tenant Database scripts](saas-multitenantdb-wingtip-app-guidance-tips.md#download-and-unblock-the-wingtip-saas-scripts)-->

## <a name="create-a-job-account-database-and-new-job-account"></a>Een taakaccountdatabase en een nieuw taakaccount maken

In deze zelfstudie moet u PowerShell gebruiken om de taakaccountdatabase en het taakaccount te maken. Net als MSDB en SQL Agent maakt Elastische taken gebruik van een Azure SQL-database om taakdefinities, de taakstatus en de geschiedenis op te slaan. Wanneer het taakaccount is gemaakt, kunt u direct aan de slag met het maken en bewaken van taken.

1. In **PowerShell ISE**Open *... \\Learning-Modules\\Schemabeheer\\Demo SchemaManagement.ps1*.
1. Druk op **F5** om het script uit te voeren.

Met het script *Demo-SchemaManagement.ps1* wordt het script *Deploy-SchemaManagement.ps1* aangeroepen om op de catalogusserver een *S2*-database te maken met de naam **jobaccount**. Het taakaccount wordt dan gemaakt. De database van dat account wordt doorgegeven als parameter bij de aanroep voor het maken van het taakaccount.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Een taak maken om nieuwe referentiegegevens te implementeren voor alle tenants

Elke database tenants bevat een reeks wilt typen in de tabel **VenueTypes** die het soort gebeurtenissen die worden gehost op een wilt definiëren. In deze oefening maakt u een update implementeert op alle databases op twee extra wilt typen toevoegen: *motor race* en *zwemmen vereniging*. Deze locatietypen sluiten aan op de achtergrondafbeelding die u in de app voor tenantevenementen ziet.

Klik op het vervolgkeuzemenu Locatietype en controleer of er slechts tien locatietypen beschikbaar zijn. Bekijk ook of Motorcycle Racing en Swimming Club niet in de lijst staan.

Nu gaan we maken een taak voor het bijwerken de **VenueTypes** tabel in alle huurders-databases en de nieuwe typen van u wilt toevoegen.

Voor het maken van een nieuwe taak gebruikt u een reeks in het systeem opgeslagen procedures uit de jobaccount-database die is gemaakt tijdens het maken van het taakaccount.

1. In SSMS, verbinding maken met de server van de tenant: tenants1-mt -\<gebruiker\>. database.windows.net
2. Blader naar de *tenants1* database op de *tenants1-mt -\<gebruiker\>. database.windows.net* server en de query de *VenueTypes* tabel naar Bevestig dat *motor race* en *zwemmen vereniging* zijn **niet** in de lijst met resultaten.
3. Verbinding maken met de catalogusserver: catalogus-mt -\<gebruiker\>. database.windows.net
4. Verbinding maken met de database jobaccount in de catalogusserver.
5. Open het bestand in SSMS... \\Learning-Modules\\Schemabeheer\\DeployReferenceData.sql
6. Wijzig de instructie: Stel @User = &lt;gebruiker&gt; en vervang de waarde van de gebruiker gebruikt wanneer u de toepassing Wingtip Tickets SaaS multitenant Database geïmplementeerd.
7. Druk op **F5** om het script uit te voeren.

    * **SP\_toevoegen\_doel\_groep** wordt gemaakt van de doelgroep naam DemoServerGroup, nu doelleden toevoegen aan de groep.
    * **SP\_toevoegen\_doel\_groep\_lid** voegt een *server* doeltype lid, die alle databases binnen die server acht (dit is de tenants1 - mt - &lt;gebruiker&gt; server met de database voor tenants) tijdens het uitvoeren van de taak moet worden opgenomen in de taak een *database* lid doeltype, voor de 'gouden'-database (basetenantdb) die zich op catalogus-mt -&lt;gebruiker&gt; server, en ten slotte een *database* lidtype om op te nemen van de ad-hoc reporting-database die wordt gebruikt in een latere zelfstudie richt.
    * **SP\_toevoegen\_taak** maakt een taak "Implementatie verwijzing gegevens" genoemd.
    * **SP\_toevoegen\_taakstap** wordt gemaakt met de tekst van T-SQL-opdracht voor het bijwerken van de verwijzingstabel, VenueTypes taakstap.
    * De resterende weergaven in het script tonen het bestaan van de objecten en controleren de taakuitvoering. Gebruik deze query's om te controleren van de statuswaarde in de **lifecycle** kolom om te bepalen wanneer de taak is voltooid op de database van tenants en de twee extra databases met de referentietabel.

1. In SSMS, blader naar de tenant-database op de *tenants1-mt -&lt;gebruiker&gt;*  server en de query de *VenueTypes* tabel om te bevestigen dat *motor race* en *zwemmen vereniging* zijn nu **toegevoegd* aan de tabel.


## <a name="create-a-job-to-manage-the-reference-table-index"></a>Een taak maken voor het beheren van de referentietabelindex

Net als bij de vorige oefening, wordt bij deze oefening een taak gemaakt om de index op de primaire sleutel van de referentietabel opnieuw op te bouwen. Dit is een typische databasebeheerbewerking die een beheerder zal moeten uitvoeren bij het laden van veel gegevens in een tabel.


1. In SSMS, verbinding maken met jobaccount-database in de catalogus-mt -&lt;gebruiker&gt;. database.windows.net-server.
2. Open in SSMS... \\Learning-Modules\\Schemabeheer\\OnlineReindex.sql.
3. Druk op **F5** het script uitvoeren

    * **SP\_toevoegen\_taak** maakt een nieuwe taak genaamd ' Online opnieuw indexeren PK\_\_VenueTyp\_\_265E44FD7FD4C885 '.
    * **SP\_toevoegen\_taakstap** taakstap met T-SQL-opdrachttekst voor het bijwerken van de index maakt.
    * De resterende weergaven in de script monitor taak wordt uitgevoerd. Gebruik deze query's om te controleren van de statuswaarde in de **lifecycle** kolom om te bepalen wanneer de taak is voltooid op op alle leden van de doel-groep.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * Een taakaccount maken om query's uit te voeren voor meerdere tenants
> * Gegevens in alle tenantdatabases bijwerken
> * Een index in een tabel maken in alle tenantdatabases

[Zelfstudie over ad-hocanalyses](saas-multitenantdb-adhoc-reporting.md)


## <a name="additional-resources"></a>Aanvullende bronnen

<!--* Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment (*Tutorial link to come*)
(saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
* [Uitgeschaalde clouddatabases beheren](sql-database-elastic-jobs-overview.md)
* [Uitgeschaalde clouddatabases maken en beheren](sql-database-elastic-jobs-create-and-manage.md)
