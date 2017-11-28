---
title: Uitvoeren van ad-hocquery's voor rapportage over meerdere Azure SQL-databases | Microsoft Docs
description: Ad-hoc reporting query's via meerdere SQL-databases in een voorbeeld van een multitenant-app uitvoeren.
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
ms.date: 11/13/2017
ms.author: AyoOlubeko
ms.openlocfilehash: c0ed3eb344ea8ec7e2d3e86125d60c8cc28f723d
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="run-ad-hoc-analytics-queries-across-multiple-azure-sql-databases"></a>Ad-hoc analytics query's uitvoeren op meerdere Azure SQL-databases

In deze zelfstudie maakt uitvoeren u gedistribueerde query's op de hele set tenant databases zodat ad-hoc interactieve rapportage. Deze query's kunnen inzicht in de dagelijkse operationele gegevens van de app Wingtip Tickets SaaS overspoeld extraheren. Deze extracties doet u een database voor aanvullende analyses implementeren in de catalogusserver en elastische Query gedistribueerde query's inschakelen.


In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]

> * Het implementeren van een ad-hoc rapportagedatabase
> * Het uitvoeren van gedistribueerde query's voor alle databases voor tenant


U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De app Wingtip Tickets SaaS multitenant Database wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de toepassing Wingtip Tickets SaaS multitenant Database verkennen](saas-multitenantdb-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.
* SQL Server Management Studio (SSMS) is geïnstalleerd. Als u wilt downloaden en installeren van SSMS, Zie [Download SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


## <a name="ad-hoc-reporting-pattern"></a>Ad-hoc reporting patroon

![ad-hoc reporting patroon](media/saas-multitenantdb-adhoc-reporting/adhocreportingpattern_shardedmultitenantDB.png)

SaaS-toepassingen kunnen analyseren de enorme hoeveelheid tenant-gegevens die centraal zijn opgeslagen in de cloud. Op basis van de analyses duidelijk inzicht in de werking en het gebruik van uw toepassing. Deze inzichten kunnen leiden functie ontwikkeling, bruikbaarheid verbeteringen en andere investeringen in uw apps en services.

U kunt gemakkelijk toegang tot deze gegevens krijgen in een database met meerdere tenants, maar dit is niet zo gemakkelijk als de gegevens op schaal zijn verdeeld over misschien wel duizenden databases. Een aanpak is het gebruik van [elastische Query](sql-database-elastic-query-overview.md), waardoor query in een gedistribueerde set van databases met algemene schema. Deze databases kunnen worden verdeeld over verschillende resourcegroepen en abonnementen. Nog moet één algemene aanmelding toegang hebben tot gegevens uit alle databases ophalen. Elastische Query gebruikt een enkel *head* database waarin externe tabellen worden gedefinieerd die spiegelen tabellen of weergaven in de gedistribueerde (tenant)-databases. Query's die naar deze hoofddatabase worden verzonden, worden gecompileerd tot een gedistribueerd queryplan, waarbij delen van de query, wanneer nodig, naar de onderliggende tenantdatabases worden gepusht. Elastische Query gebruikt de shard-toewijzing in de catalogusdatabase om te bepalen van de locatie van de tenant-databases. Installatie en query zijn simpel met behulp van standaard [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference), en ondersteuning voor ad-hoc opvragen van hulpprogramma's zoals Power BI en Excel.

Door query's over de tenant-databases worden verdeeld, biedt elastische Query onmiddellijk inzicht in live productiegegevens. Echter, zoals elastische Query gegevens op uit mogelijk veel databases haalt, latentie van query kunt soms hoger zijn dan voor gelijkwaardige query's verzonden naar een enkele multitenant-database. Zorg ervoor dat ontwerp query's te minimaliseren, de gegevens die wordt geretourneerd. Elastische Query is vaak het beste geschikt voor kleine hoeveelheden realtime gegevens, in plaats van veelgebruikte gebouw of complexe analytics query's of rapporten uitvoeren van query's. Als u query's niet goed uitvoert, bekijkt u de [uitvoeringsplan](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) om te zien welke deel van de query heeft omlaag op de externe database is gedrukt. En bepalen hoeveel gegevens geretourneerd. Query's waarvoor complexe analytische verwerking beter kan worden afgehandeld door de uitgepakte tenant gegevens opslaan in een database die is geoptimaliseerd voor analytics query's. SQL-Database en SQL Data Warehouse kan deze de analytics-database host.

Dit patroon voor analyses wordt uitgelegd in de [tenant analytics zelfstudie](saas-multitenantdb-tenant-analytics.md).

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Ophalen van de broncode van de Database Wingtip Tickets SaaS-multitenant-toepassing en scripts

De scripts Wingtip Tickets SaaS multitenant Database en de broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub-opslagplaats. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de scripts Wingtip Tickets SaaS deblokkeren.

## <a name="create-ticket-sales-data"></a>Ticket verkoopgegevens maken

Voor informatie over het uitvoeren van query's op basis van een gegevensset interessanter maken verkoopgegevens ticket met de ticket-generator.

1. In de *PowerShell ISE*, open de... \\Learning-Modules\\operationele Analytics\\ad-hoc Reporting\\*Demo AdhocReporting.ps1* script en stel de volgende waarden:
   * **$DemoScenario** = 1, **tickets voor gebeurtenissen op alle plaatsen aanschaffen**.
2. Druk op **F5** voor het uitvoeren van het script en ticket verkoop genereren. Terwijl het script wordt uitgevoerd, blijven de stappen in deze zelfstudie. De gegevens van het ticket query wordt uitgevoerd in de *ad hoc gedistribueerde query's uitvoeren* sectie, dus wacht u totdat de ticket-generator om te voltooien.

## <a name="explore-the-tenant-tables"></a>De tenanttabellen verkennen 

In de toepassing Wingtip Tickets SaaS multitenant Database worden tenants opgeslagen in een hybride tenant model voor het beheer - waar tenant gegevens wordt opgeslagen in een multitenant-database of een database voor één tenant en kan worden verplaatst tussen de twee. Bij het uitvoeren van query's voor alle databases van de tenant, is het belangrijk dat de gegevens in elastische Query kan worden behandeld alsof deze een onderdeel van één logische database shard door tenant is. 

Voor dit patroon, alle tenanttabellen bevatten een *VenueId* kolom met de id die de gegevens voor de tenantsleutel behoort. De *VenueId* wordt berekend als een hash van de naam wilt, maar methode kan worden gebruikt om te introduceren een unieke waarde voor deze kolom. Deze methode is vergelijkbaar met de manier waarop die de tenantsleutel wordt berekend voor gebruik in de catalogus. Tabellen die *VenueId* door elastische Query worden gebruikt voor query's parallelize en push ze de juiste externe tenant-database. Dit vermindert aanzienlijk de hoeveelheid gegevens die wordt geretourneerd en resulteert in een toename van de prestaties, vooral als er meerdere tenants waarvan de gegevens zijn opgeslagen in één tenant-databases.

## <a name="deploy-the-database-used-for-ad-hoc-distributed-queries"></a>De database die wordt gebruikt voor ad-hoc gedistribueerde query's implementeren

In deze oefening implementeert de *ad-hoc reporting* database. Dit is de head-database met het schema voor het uitvoeren van query's voor alle databases van de tenant. De database wordt geïmplementeerd op de bestaande catalogusserver de server gebruikt voor alle databases die betrekking hebben in de voorbeeld-app is.

1. Open... \\Learning-Modules\\operationele Analytics\\ad-hoc Reporting\\*Demo AdhocReporting.ps1* in de *PowerShell ISE* en stel de de volgende waarden:
   * **$DemoScenario** = 2, **implementeren Ad hoc analytics-database**.

2. Druk op **F5** het script uitvoeren en maken het *ad-hoc reporting* database.

In de volgende sectie voegt u schema met de database zodat deze kan worden gebruikt voor gedistribueerde query's uitvoeren.

## <a name="configure-the-head-database-for-running-distributed-queries"></a>De database 'head' voor het uitvoeren van gedistribueerde query's configureren

In deze oefening wordt schema (de externe gegevensbron en een externe tabeldefinities) toegevoegd aan de ad-hoc rapportagedatabase waarmee uitvoeren van query's voor alle databases van de tenant.

1. Open SQL Server Management Studio en verbinding maken met de ad-hoc-rapportagedatabase die u in de vorige stap hebt gemaakt. De naam van de database is *ad-hoc reporting*.
2. Open ...\Learning Modules\Operational Analytics\Adhoc Reporting\ *initialiseren AdhocReportingDB.sql* in SSMS.
3. Controleer het SQL-script en Let op het volgende:

   Een database-scoped referentie elastische Query gebruikt voor toegang tot elk van de tenant-databases. Deze referentie moet beschikbaar zijn in alle databases en moeten normaal gesproken verleend de minimale rechten nodig zijn voor deze ad-hocquery's inschakelen.

    ![referentie maken](media/saas-multitenantdb-adhoc-reporting/create-credential.png)

   Query's worden door de catalogusdatabase als de externe gegevensbron, gedistribueerd naar alle databases in de catalogus wordt geregistreerd wanneer de query wordt uitgevoerd. Omdat servernamen verschillend voor elke implementatie zijn, dit Initialisatiescript van de locatie van de catalogusdatabase opgehaald door op te halen van de huidige server (@@servername) waarop het script wordt uitgevoerd.

    ![externe gegevensbron maken](media/saas-multitenantdb-adhoc-reporting/create-external-data-source.png)

   De externe tabellen die verwijzen naar tenanttabellen worden gedefinieerd met **DISTRIBUTION = SHARDED(VenueId)**. Deze stuurt een query voor een bepaalde *VenueId* met de juiste database en verbetert de prestaties voor veel scenario's zoals weergegeven in de volgende sectie.

    ![externe tabellen maken](media/saas-multitenantdb-adhoc-reporting/external-tables.png)

   De lokale tabel *VenueTypes* die is gemaakt en ingevuld. Deze gegevens verwijzingstabel is veel voor in alle databases voor tenant, zodat hier aangegeven als een lokale tabel en gevuld met de algemene gegevens kunnen worden weergegeven. Voor sommige query's, hierdoor wordt de hoeveelheid gegevens die worden verplaatst tussen de tenant-databases en de *ad-hoc reporting* database.

    ![Tabel maken](media/saas-multitenantdb-adhoc-reporting/create-table.png)

   Als u verwijzingsdimensies op deze manier opneemt, moet u het tabelschema en de gegevens bijwerken wanneer u de databases tenant bijwerkt.

4. Druk op **F5** voor het uitvoeren van het script en initialiseren van de *ad-hoc reporting* database. 

U kunt nu gedistribueerde query's uitvoeren en inzichten te verzamelen over alle huurders!

## <a name="run-ad-hoc-distributed-queries"></a>Ad-hoc gedistribueerde query's uitvoeren

Nu de *ad-hoc reporting* database is ingesteld, gaat u verder gaan en sommige gedistribueerde query's uitvoeren. Het plan kan worden uitgevoerd voor een beter begrip van waar de queryverwerking gebeurt er bevatten. 

Bij de inspectie van het uitvoeringsplan Beweeg de muisaanwijzer over de pictogrammen plan voor meer informatie. 

1. In *SSMS*open... \\Learning-Modules\\operationele Analytics\\ad-hoc Reporting\\*Demo AdhocReportingQueries.sql*.
2. Zorg ervoor dat u verbonden bent met de **ad-hoc reporting** database.
3. Selecteer de **Query** en klik op **werkelijke uitvoeringsplan opnemen**
4. Markeer de *welke plaatsen zijn momenteel geregistreerd?* query en druk op **F5**.

   De query retourneert de hele wetten lijst ter illustratie van hoe snel en eenvoudig het is in alle tenants opvragen en gegevens retourneren vanaf elke tenant.

   Bekijk het plan en dat alle kosten voor de externe query is omdat we gewoon bent gaan naar elke tenant-database en de informatie die u wilt selecteren.

   ![Selecteer * uit dbo. Plaatsen](media/saas-multitenantdb-adhoc-reporting/query1-plan.png)

5. Selecteer de volgende query's en druk op **F5**.

   Gegevens van de tenant-databases en de lokale lid wordt van deze query *VenueTypes* tabel (lokale computer, zoals deze wordt een tabel de *ad-hoc reporting* database).

   Het plan controleren en zien dat het merendeel van de kosten van de externe query omdat er een query uitvoeren op elke tenant wilt info (dbo. Plaatsen) en voert u een snelle lokale koppelen met de lokale *VenueTypes* tabel de beschrijvende naam te geven.

   ![Lid worden van lokale en externe gegevens](media/saas-multitenantdb-adhoc-reporting/query2-plan.png)

6. Nu selecteren de *op welke dag zijn de meest tickets verkocht?* query en druk op **F5**.

   Deze query heeft iets meer complexe lid worden en cumulatie-instellingen. Wat is belangrijk te weten is dat de meeste van de verwerking op afstand wordt uitgevoerd en nogmaals we alleen de rijen die we nodig hebben terugbrengen, slechts één rij voor elke wetten cumulatieve ticket verkoop aantal per dag retourneren.

   ![query](media/saas-multitenantdb-adhoc-reporting/query3-plan.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * Gedistribueerde query's uitvoeren voor alle databases van de tenant
> * Implementeer een ad-hoc rapportagedatabase en schema niet toevoegen aan deze gedistribueerde query's uitvoeren.

Nu proberen de [Tenant Analytics zelfstudie](saas-multitenantdb-tenant-analytics.md) uitpakken gegevens verkennen met een afzonderlijke analytics-database voor complexere analytics-verwerking.

## <a name="additional-resources"></a>Aanvullende bronnen

<!-- ??
* Additional [tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application](saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
-->

* [Elastische query](sql-database-elastic-query-overview.md)
