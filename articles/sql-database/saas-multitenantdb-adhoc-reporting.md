---
title: Ad-hoc reporting query's uitvoeren voor meerdere Azure SQL-databases | Microsoft Docs
description: Ad-hoc reporting query's uitvoeren voor meerdere SQL-databases in een voorbeeld van een app met meerdere tenants.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: AyoOlubeko
ms.author: ayolubek
ms.reviewer: sstein
manager: craigg
ms.date: 10/30/2018
ms.openlocfilehash: 2780215e409886be82ac70642dabe05c9257945e
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55568444"
---
# <a name="run-ad-hoc-analytics-queries-across-multiple-azure-sql-databases"></a>Ad-hoc analytics-query's uitvoeren voor meerdere Azure SQL-databases

In deze zelfstudie hebt u gedistribueerde query's uitvoeren voor de volledige set van tenant databases als ad-hoc interactieve rapportage wilt inschakelen. Deze query's kunnen extraheren van inzichten ophalen uit dagelijkse operationele gegevens van de Wingtip Tickets SaaS-app. Deze extracties doet u een extra analytics-database implementeren op de catalogusserver en elastische query's gebruiken om in te schakelen van gedistribueerde query's.


In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]

> * Over het implementeren van een ad-hoc rapportagedatabase
> * Het uitvoeren van gedistribueerde query's in alle tenantdatabases


U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De app Wingtip Tickets SaaS multitenant-Database wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de toepassing Wingtip Tickets SaaS multitenant-Database verkennen](saas-multitenantdb-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.
* SQL Server Management Studio (SSMS) is geïnstalleerd. Als u wilt downloaden en installeren van SSMS, Zie [Download SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


## <a name="ad-hoc-reporting-pattern"></a>Ad-hocrapportage patroon

![ad-hoc reporting patroon](media/saas-multitenantdb-adhoc-reporting/adhocreportingpattern_shardedmultitenantDB.png)

SaaS-toepassingen kunnen analyseren van de grote hoeveelheid tenantgegevens die centraal zijn opgeslagen in de cloud. De analyses geven inzicht in de werking en het gebruik van uw toepassing. Deze inzichten kunnen dienen als richtlijn ontwikkelen van functies, bruikbaarheid verbeteren en andere investeringen doen in uw apps en services.

U kunt gemakkelijk toegang tot deze gegevens krijgen in een database met meerdere tenants, maar dit is niet zo gemakkelijk als de gegevens op schaal zijn verdeeld over misschien wel duizenden databases. Een aanpak is het gebruik van [elastische Query](sql-database-elastic-query-overview.md), waarmee query's uitvoeren in een gedistribueerde set databases met hetzelfde schema. Deze databases kunnen in verschillende resourcegroepen en abonnementen worden gedistribueerd. Nog moet één algemene aanmelding toegang hebben tot gegevens van alle databases ophalen. Elastische query's gebruiken één *head* database waarin externe tabellen zijn gedefinieerd die tabellen of weergaven in de gedistribueerde (tenant) databases spiegelen. Query's die naar deze hoofddatabase worden verzonden, worden gecompileerd tot een gedistribueerd queryplan, waarbij delen van de query, wanneer nodig, naar de onderliggende tenantdatabases worden gepusht. Elastische Query gebruikt de shard-toewijzing in de catalogusdatabase om te bepalen van de locatie van alle tenantdatabases. Installatie en query zijn simpel met behulp van standaard [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference), en ondersteuning voor ad-hoc uitvoeren van query's vanuit hulpprogramma's zoals Power BI en Excel.

Door het distribueren van query's in de tenant-databases, biedt elastische Query onmiddellijk inzicht in live productiegegevens. Echter, als een elastische Query gegevens op uit mogelijk veel databases haalt, latentie van query kan soms hoger zijn dan voor equivalente query's verzonden naar een database met meerdere tenants. Zorg ervoor dat ontwerp query's voor het minimaliseren van de gegevens die wordt geretourneerd. Elastische query's is vaak het best geschikt voor kleine hoeveelheden realtime-gegevens, in plaats van veelgebruikte bouwen of complexe analytics-query's of rapporten uitvoeren van query's. Als query's niet goed uitvoert, bekijkt u de [uitvoeringsplan](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) om te zien welk deel van de query is verlaagd met de externe database. En evalueren hoeveel gegevens geretourneerd. Query's waarvoor complexe analytische verwerking beter kan worden afgehandeld door de uitgepakte tenantgegevens opslaan in een database die is geoptimaliseerd voor analysequery's. SQL-Database en SQL Data Warehouse kan deze de analytics-database host.

Dit patroon voor analytics wordt uitgelegd in de [zelfstudie tenant-analytics](saas-multitenantdb-tenant-analytics.md).

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Download de broncode van de toepassing Wingtip Tickets SaaS multitenant-Database en -scripts

De Wingtip Tickets SaaS multitenant-Database-scripts en broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub-opslagplaats. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de blokkering opheffen van de Wingtip Tickets SaaS-scripts.

## <a name="create-ticket-sales-data"></a>Verkoopgegevens ticket maken

Voor query's uitvoeren op een interessante verzameling, verkoopgegevens ticket te maken door het uitvoeren van de ticket-generator.

1. In de *PowerShell ISE*, open de... \\Learning Modules\\Operational Analytics\\ad-hoc Reporting\\*Demo-AdhocReporting.ps1* script en stel de volgende waarden:
   * **$DemoScenario** = 1, **tickets kopen voor voorstellingen bij alle theaters**.
2. Druk op **F5** voor het uitvoeren van het script en ticket verkoop genereren. Terwijl het script wordt uitgevoerd, blijven de stappen in deze zelfstudie. De gegevens van het ticket query wordt uitgevoerd in de *ad hoc gedistribueerde query's uitvoeren* sectie, dus wacht totdat de ticket-generator om te voltooien.

## <a name="explore-the-tenant-tables"></a>De tenanttabellen verkennen 

In de toepassing Wingtip Tickets SaaS multitenant-Database, worden tenants opgeslagen in een hybride tenant management model - waar gegevens van de tenant wordt opgeslagen in een multitenant-database of een enkele tenantdatabase en kunnen worden verplaatst tussen de twee. Bij het uitvoeren van query's in alle tenantdatabases, is het belangrijk dat de gegevens in elastische query's kunnen worden behandeld alsof deze deel van een enkele logische database shard door de tenant uitmaakt. 

Voor het bereiken van dit patroon, alle tenanttabellen bevatten een *VenueId* kolom die aangeeft dat de gegevens voor de tenantsleutel behoort. De *VenueId* wordt berekend als een hash van de naam van de locatie, maar elke methode kan worden gebruikt om te introduceren een unieke waarde voor deze kolom. Deze aanpak is vergelijkbaar met de manier waarop die de tenant-sleutel voor gebruik in de catalogus wordt berekend. Tabellen die *VenueId* met elastische query's worden gebruikt om query's parallel en push ze naar de juiste externe tenant-database. Dit vermindert aanzienlijk de hoeveelheid gegevens die wordt geretourneerd en resulteert in een toename in de prestaties, met name wanneer er meerdere tenants waarvan gegevens worden opgeslagen in één tenant-databases.

## <a name="deploy-the-database-used-for-ad-hoc-distributed-queries"></a>De database die wordt gebruikt voor ad-hoc gedistribueerde query's implementeren

In deze oefening implementeert de *ad-hoc reporting* database. Dit is de hoofddatabase met het schema voor het uitvoeren van query's in alle tenantdatabases. De database wordt geïmplementeerd op de bestaande catalogusserver, waarop de server die wordt gebruikt voor alle-gerelateerde databases in de voorbeeld-app.

1. Open... \\Learning Modules\\Operational Analytics\\ad-hoc Reporting\\*Demo-AdhocReporting.ps1* in de *PowerShell ISE* en stel de de volgende waarden:
   * **$DemoScenario** = 2, **implementeren Ad-hoc analytics-database**.

2. Druk op **F5** en voer het script uit de *ad-hoc reporting* database.

In de volgende sectie voegt u schema met de database, zodat het kan worden gebruikt voor gedistribueerde query's uitvoeren.

## <a name="configure-the-head-database-for-running-distributed-queries"></a>De database 'head' voor het uitvoeren van gedistribueerde query's configureren

In deze oefening wordt schema (de externe gegevensbron en definities van de externe tabel) toegevoegd aan de ad-hoc-rapportagedatabase waarmee query's in alle tenantdatabases.

1. Open SQL Server Management Studio en maak verbinding met de ad-hoc reporting database die u in de vorige stap hebt gemaakt. De naam van de database is *ad-hoc reporting*.
2. Open ...\Learning Modules\Operational Analytics\Adhoc Reporting\ *initialiseren AdhocReportingDB.sql* in SSMS.
3. Controleer het SQL-script en Let op het volgende:

   Een database-scoped referentie elastische query's gebruiken voor toegang tot elk van de tenant-databases. Deze referentie moet beschikbaar zijn in alle databases en moeten normaal gesproken worden verleend de minimale rechten vereist om deze ad-hocquery's inschakelen.

    ![referentie maken](media/saas-multitenantdb-adhoc-reporting/create-credential.png)

   Met behulp van de catalogusdatabase als de externe gegevensbron, worden query's worden gedistribueerd naar alle databases in de catalogus wordt geregistreerd wanneer de query wordt uitgevoerd. Omdat de namen van voor elke implementatie verschillen, met dit script initialisatie van de locatie van de catalogusdatabase wordt op de bij het ophalen van de huidige server (@@servername) waarop het script wordt uitgevoerd.

    ![externe gegevensbron maken](media/saas-multitenantdb-adhoc-reporting/create-external-data-source.png)

   De externe tabellen die verwijzen naar tenanttabellen zijn gedefinieerd met **distributie = SHARDED(VenueId)**. Deze stuurt een query voor een bepaalde *VenueId* met de juiste database en verbetert de prestaties voor veel scenario's zoals weergegeven in de volgende sectie.

    ![externe tabellen maken](media/saas-multitenantdb-adhoc-reporting/external-tables.png)

   De lokale tabel *VenueTypes* die is gemaakt en ingevuld. Deze gegevenstabel verwijzing is gebruikelijk in alle tenantdatabases, zodat deze kan worden weergegeven als een lokale tabel en is ingevuld met de algemene gegevens. Voor sommige query's, kan dit verminderen de hoeveelheid gegevens die tussen de tenant-databases verplaatst en de *ad-hoc reporting* database.

    ![Tabel maken](media/saas-multitenantdb-adhoc-reporting/create-table.png)

   Als u verwijzingstabellen op deze manier, zorg er dan voor dat de tabelschema en de gegevens bijgewerkt wanneer u de tenant-databases bijwerken.

4. Druk op **F5** voor het uitvoeren van het script en initialiseren van de *ad-hoc reporting* database. 

U kunt nu gedistribueerde query's uitvoeren en inzichten te verwerven voor alle tenants.

## <a name="run-ad-hoc-distributed-queries"></a>Ad-hoc gedistribueerde query's uitvoeren

Nu de *ad-hoc reporting* database is ingesteld, gaat u verder en sommige gedistribueerde query's uitvoeren. Het plan kan worden uitgevoerd voor een beter begrip van waar de verwerking van query's er gebeurt bevatten. 

Bij de inspectie van het abonnement kan worden uitgevoerd, Beweeg de muisaanwijzer over de pictogrammen plan voor meer informatie. 

1. In *SSMS*, openen... \\Learning Modules\\Operational Analytics\\ad-hoc Reporting\\*Demo-AdhocReportingQueries.sql*.
2. Zorg ervoor dat u bent verbonden met de **ad-hoc reporting** database.
3. Selecteer de **Query** en klik op **werkelijke Execution Plan omvatten**
4. Markeer de *welke venues momenteel zijn geregistreerd?* query en druk op **F5**.

   De query retourneert de lijst met volledige venue, waarin u ziet hoe snel en eenvoudig het is om query's uitvoeren voor alle tenants en als resultaat de gegevens van elke tenant.

   Het plan controleren en zien dat de kosten voor de externe query omdat we gewoon bent naar elke tenantdatabase te gaan en de locatie-informatie te selecteren.

   ![Selecteer * uit dbo. Venues](media/saas-multitenantdb-adhoc-reporting/query1-plan.png)

5. Selecteer de volgende query en druk op **F5**.

   Deze query lid wordt van gegevens van de tenant-databases en de lokale *VenueTypes* tabel (lokale computer, zoals deze wordt een tabel de *ad-hoc reporting* database).

   Het plan controleren en zien dat de meerderheid van de kosten voor de externe query omdat we een query informatie van de locatie van elke tenant (dbo. Venues), en voer een snelle lokale koppelen met de lokale *VenueTypes* tabel om de beschrijvende naam weer te geven.

   ![Neem deel aan op de lokale en externe gegevens](media/saas-multitenantdb-adhoc-reporting/query2-plan.png)

6. Selecteer nu de *op welke dag zijn de meeste tickets die worden verkocht?* query en druk op **F5**.

   Deze query heeft iets meer complexe lid worden en cumulatie-instellingen. Wat is belangrijk te weten is dat de meeste van de verwerking op afstand is uitgevoerd en deze opnieuw, we alleen de rijen die we nodig hebben brengen, slechts één rij voor van elke venue cumulatieve ticket verkoop aantal per dag retourneren.

   ![query](media/saas-multitenantdb-adhoc-reporting/query3-plan.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * Gedistribueerde query's uitvoeren voor alle databases van de tenant
> * Implementeren van een ad-hoc reporting-database en schema toevoegen aan deze gedistribueerde query's uitvoeren.

Probeer nu de [Tenantanalyses zelfstudie](saas-multitenantdb-tenant-analytics.md) extraheren om gegevens te verkennen met een aparte analytics-database voor de analyseverwerking van meer complexe.

## <a name="additional-resources"></a>Aanvullende resources

<!-- ??
* Additional [tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application](saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
-->

* [Elastische query](sql-database-elastic-query-overview.md)
