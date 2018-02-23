---
title: Melden van query's over meerdere Azure SQL-databases uitvoeren | Microsoft Docs
description: Cross-tenant reporting met behulp van gedistribueerde query's.
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
ms.topic: articles
ms.date: 11/13/2017
ms.author: billgib; sstein; AyoOlubeko
ms.openlocfilehash: 531d284798e455622faa1bfe7b0c8f178b3642fd
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2018
---
# <a name="cross-tenant-reporting-using-distributed-queries"></a>Cross-tenant reporting met behulp van gedistribueerde query 's

In deze zelfstudie maakt uitvoeren u gedistribueerde query's op de volledige set van tenant databases voor rapportage. Deze query's kunnen inzicht in de dagelijkse operationele gegevens van de tenants Wingtip Tickets SaaS overspoeld extraheren. U doet dit door een extra rapportagedatabase implementeren in de catalogusserver en gebruik elastische Query inschakelen gedistribueerde query's.


In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]

> * Het implementeren van een rapportagedatabase
> * Het uitvoeren van gedistribueerde query's voor alle databases voor tenant
> * Hoe algemene weergaven in elke database efficiënt doorzoeken via tenants kunnen inschakelen


U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:


* De app Wingtip Tickets SaaS Database Per Tenant wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de toepassing Wingtip Tickets SaaS Database Per Tenant verkennen](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.
* SQL Server Management Studio (SSMS) is geïnstalleerd. Als u wilt downloaden en installeren van SSMS, Zie [Download SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


## <a name="cross-tenant-reporting-pattern"></a>Cross-tenant reporting patroon

![cross-tenant gedistribueerde query patroon](media/saas-tenancy-cross-tenant-reporting/cross-tenant-distributed-query.png)

Een kans met SaaS-toepassingen is het gebruik van de enorme hoeveelheid tenant gegevens opgeslagen in de cloud om inzicht in de bewerking en het gebruik van uw toepassing te krijgen. Deze inzichten kunnen leiden functie ontwikkeling, bruikbaarheid verbeteringen en andere investeringen in uw apps en services.

U kunt gemakkelijk toegang tot deze gegevens krijgen in een database met meerdere tenants, maar dit is niet zo gemakkelijk als de gegevens op schaal zijn verdeeld over misschien wel duizenden databases. Een aanpak is het gebruik van [elastische Query](sql-database-elastic-query-overview.md), waardoor query in een gedistribueerde set van databases met algemene schema. Deze databases kunnen worden verdeeld over verschillende resourcegroepen en abonnementen, maar moeten delen een gemeenschappelijke aanmelding. Elastische Query gebruikt een enkel *head* database waarin externe tabellen worden gedefinieerd die spiegelen tabellen of weergaven in de gedistribueerde (tenant)-databases. Query's die naar deze hoofddatabase worden verzonden, worden gecompileerd tot een gedistribueerd queryplan, waarbij delen van de query, wanneer nodig, naar de onderliggende tenantdatabases worden gepusht. Elastische Query gebruikt de shard-toewijzing in de catalogusdatabase om te bepalen van de locatie van de tenant-databases. Installatie en query van de database head zijn simpel met behulp van standaard [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference), en ondersteuning voor het uitvoeren van query's van de hulpprogramma's zoals Power BI en Excel.

Door query's over de tenant-databases worden verdeeld, biedt elastische Query onmiddellijk inzicht in live productiegegevens. Als elastische Query gegevens op uit mogelijk veel databases haalt, kan Querylatentie hoger zijn dan gelijkwaardige query's verzonden naar een enkele multitenant-database. Ontwerp van query's kunnen beperken van de gegevens die wordt geretourneerd naar de head-database. Elastische Query is vaak het beste geschikt voor kleine hoeveelheden realtime gegevens, in plaats van veelgebruikte gebouw of complexe analytics query's of rapporten uitvoeren van query's. Als het uitvoeren van query's niet goed, bekijkt u de [uitvoeringsplan](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) om te zien welke deel van de query wordt doorgeschoven, is niet actief op de externe database en hoeveel gegevens geretourneerd. Query's waarvoor complexe aggregatie of analytische verwerking mogelijk betere verwerkt door de gegevens van de tenant te pakken op een database of de data warehouse geoptimaliseerd voor analysequery's. Dit patroon wordt uitgelegd in de [tenant analytics zelfstudie](saas-tenancy-tenant-analytics.md). 

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>De toepassingsscripts Wingtip Tickets SaaS Database Per Tenant ophalen

De scripts Wingtip Tickets SaaS multitenant Database en de broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de scripts Wingtip Tickets SaaS deblokkeren.

## <a name="create-ticket-sales-data"></a>Ticket verkoopgegevens maken

Voor informatie over het uitvoeren van query's op basis van een gegevensset interessanter maken verkoopgegevens ticket met de ticket-generator.

1. In de *PowerShell ISE*, open de... \\Learning-Modules\\operationele Analytics\\ad-hoc Reporting\\*Demo AdhocReporting.ps1* script en stel de volgende waarde:
   * **$DemoScenario** = 1, **tickets voor gebeurtenissen op alle plaatsen aanschaffen**.
2. Druk op **F5** voor het uitvoeren van het script en ticket verkoop genereren. Terwijl het script wordt uitgevoerd, blijven de stappen in deze zelfstudie. De gegevens van het ticket query wordt uitgevoerd in de *ad-hoc gedistribueerde query's uitvoeren* sectie, dus wacht u totdat de ticket-generator om te voltooien.

## <a name="explore-the-global-views"></a>Algemene weergaven verkennen

In de toepassing Wingtip Tickets SaaS Database Per Tenant krijgt elke tenant een database. De gegevens in de databasetabellen is dus afgestemd op het perspectief van een enkele tenant. Bij het uitvoeren van query's voor alle databases, is het echter belangrijk dat de gegevens in elastische Query kan worden behandeld alsof deze een onderdeel van één logische database shard door tenant is. 

Om te simuleren dit patroon, een reeks 'global' weergaven worden toegevoegd aan de tenant-database dat project een tenant-ID in elk van de tabellen die globaal worden opgevraagd. Bijvoorbeeld, de *VenueEvents* weergave toegevoegd een berekende *VenueId* aan de kolommen geprojecteerd vanuit de *gebeurtenissen* tabel. Op dezelfde manier de *VenueTicketPurchases* en *VenueTickets* weergaven toevoegen een berekende *VenueId* kolom geprojecteerd uit hun respectieve tabellen. Deze weergaven worden gebruikt door elastische Query om query's en push ze naar beneden op de juiste externe tenant-database wanneer parallelize een *VenueId* kolom aanwezig is. Dit vermindert aanzienlijk de hoeveelheid gegevens die wordt geretourneerd en resulteert in een aanzienlijke toename van de prestaties voor veel query's. Deze globale weergaven zijn vooraf gemaakte in alle databases van de tenant.

1. Open SSMS en [verbinding maken met de tenants1 -&lt;gebruiker&gt; server](saas-tenancy-wingtip-app-guidance-tips.md#explore-database-schema-and-execute-sql-queries-using-ssms).
1. Vouw **Databases**, met de rechtermuisknop op _contosoconcerthall_, en selecteer **nieuwe Query**.
1. De volgende query's uitvoeren om het verschil tussen de tabellen voor één tenant en de globale weergaven verkennen:

   ```T-SQL
   -- The base Venue table, that has no VenueId associated.
   SELECT * FROM Venue

   -- Notice the plural name 'Venues'. This view projects a VenueId column.
   SELECT * FROM Venues

   -- The base Events table, which has no VenueId column.
   SELECT * FROM Events

   -- This view projects the VenueId retrieved from the Venues table.
   SELECT * FROM VenueEvents
   ```

In deze weergaven de *VenueId* wordt berekend als een hash van de naam wilt, maar methode kan worden gebruikt om te introduceren een unieke waarde. Deze methode is vergelijkbaar met de manier waarop die de tenantsleutel wordt berekend voor gebruik in de catalogus.

Om te onderzoeken van de definitie van de *plaatsen* weergeven:

1. In **Objectverkenner**, vouw **contosoconcerthall** > **weergaven**:

   ![weergaven](media/saas-tenancy-cross-tenant-reporting/views.png)

2. Met de rechtermuisknop op **dbo. Plaatsen**.
3. Selecteer **weergeven als een Script** > **maken naar** > **nieuw venster van de Query-Editor**

Een van de andere script *wetten* weergaven om te zien hoe ze toevoegen de *VenueId*.

## <a name="deploy-the-database-used-for-distributed-queries"></a>De database die wordt gebruikt voor gedistribueerde query's implementeren

In deze oefening implementeert de _ad-hoc reporting_ database. Dit is de head-database met het schema voor het uitvoeren van query's voor alle databases van de tenant. De database wordt geïmplementeerd op de bestaande catalogusserver de server gebruikt voor alle databases die betrekking hebben in de voorbeeld-app is.

1. in *PowerShell ISE*open... \\Learning-Modules\\operationele Analytics\\ad-hoc Reporting\\*Demo AdhocReporting.ps1*. 

1. Stel **$DemoScenario = 2**, _implementeren Ad-hoc rapportagedatabase_.

1. Druk op **F5** het script uitvoeren en maken het *ad-hoc reporting* database.

In de volgende sectie voegt u schema met de database zodat deze kan worden gebruikt voor gedistribueerde query's uitvoeren.

## <a name="configure-the-head-database-for-running-distributed-queries"></a>De database 'head' voor het uitvoeren van gedistribueerde query's configureren

In deze oefening wordt schema (de externe gegevensbron en een externe tabeldefinities) toegevoegd aan de _ad-hoc reporting_ database inschakelen voor het uitvoeren van query's voor alle databases van de tenant.

1. Open SQL Server Management Studio en verbinding maken met de ad-hoc rapportagedatabase die u in de vorige stap hebt gemaakt. De naam van de database is *ad-hoc reporting*.
2. Open ...\Learning Modules\Operational Analytics\Adhoc Reporting\ _initialiseren AdhocReportingDB.sql_ in SSMS.
3. Raadpleeg de SQL-script en de opmerking:

   Een database-scoped referentie elastische Query gebruikt voor toegang tot elk van de tenant-databases. Deze referentie moet beschikbaar zijn in alle databases en moeten normaal gesproken verleend de minimale rechten nodig zijn voor deze query's inschakelen.

    ![referentie maken](media/saas-tenancy-cross-tenant-reporting/create-credential.png)

   Query's worden met de van de catalogusdatabase als de externe gegevensbron gedistribueerd naar alle databases die zijn geregistreerd in de catalogus op het moment dat de query wordt uitgevoerd. Als de servernamen van de zijn verschillend voor elke implementatie, dit script de locatie van de catalogusdatabase opgehaald van de huidige server (@@servername) waarop het script wordt uitgevoerd.

    ![externe gegevensbron maken](media/saas-tenancy-cross-tenant-reporting/create-external-data-source.png)

   De externe tabellen die verwijzen naar de globale weergaven in de vorige sectie beschreven en gedefinieerd met **DISTRIBUTION = SHARDED(VenueId)**. Omdat elke *VenueId* is toegewezen aan een individuele database, dit verbetert de prestaties voor veel scenario's zoals weergegeven in de volgende sectie.

    ![externe tabellen maken](media/saas-tenancy-cross-tenant-reporting/external-tables.png)

   De lokale tabel _VenueTypes_ die is gemaakt en ingevuld. Deze gegevens verwijzingstabel is veel voor in alle databases voor tenant, zodat hier aangegeven als een lokale tabel en gevuld met de algemene gegevens kunnen worden weergegeven. Met deze tabel die is gedefinieerd in de database head Verklein de hoeveelheid gegevens die moeten worden verplaatst naar de head-database voor een aantal query's.

    ![Tabel maken](media/saas-tenancy-cross-tenant-reporting/create-table.png)

   Als u verwijzingsdimensies op deze manier opneemt, moet u het tabelschema en de gegevens bijwerken wanneer u de databases tenant bijwerkt.

4. Druk op **F5** voor het uitvoeren van het script en initialiseren van de *ad-hoc reporting* database. 

U kunt nu gedistribueerde query's uitvoeren en inzichten te verzamelen over alle huurders!

## <a name="run-distributed-queries"></a>Gedistribueerde query's uitvoeren

Nu de *ad-hoc reporting* database is ingesteld, gaat u verder gaan en sommige gedistribueerde query's uitvoeren. Het plan kan worden uitgevoerd voor een beter begrip van waar de queryverwerking gebeurt er bevatten. 

Bij de inspectie van het uitvoeringsplan Beweeg de muisaanwijzer over de pictogrammen plan voor meer informatie. 

Belangrijk te weten, is deze instelling **DISTRIBUTION = SHARDED(VenueId)** verbetert de prestaties voor veel scenario's als de externe gegevensbron is gedefinieerd. Aangezien aan elk *VenueId* is toegewezen aan een individuele database, is eenvoudig gefilterd op afstand, alleen de benodigde gegevens retourneren.

1. Open... \\Learning-Modules\\operationele Analytics\\ad-hoc Reporting\\*Demo AdhocReportingQueries.sql* in SSMS.
2. Zorg ervoor dat u verbonden bent met de **ad-hoc reporting** database.
3. Selecteer de **Query** en klik op **werkelijke uitvoeringsplan opnemen**
4. Markeer de *welke plaatsen zijn momenteel geregistreerd?* query en druk op **F5**.

   De query retourneert de hele wetten lijst ter illustratie van hoe snel en eenvoudig het is in alle tenants opvragen en gegevens retourneren vanaf elke tenant.

   Bekijk het plan en ziet dat de hele kosten op de externe query is. Elke tenant-database de query op afstand wordt uitgevoerd en retourneert de gegevens u wilt met de hoofd-database.

   ![Selecteer * uit dbo. Plaatsen](media/saas-tenancy-cross-tenant-reporting/query1-plan.png)

5. Selecteer de volgende query's en druk op **F5**.

   Gegevens van de tenant-databases en de lokale lid wordt van deze query *VenueTypes* tabel (lokale computer, zoals deze wordt een tabel de *ad-hoc reporting* database).

   Bekijk het plan en dat het merendeel van de kosten van de externe query is. Elke tenant database retourneert de informatie wilt en voert een lokale koppeling met de lokale *VenueTypes* tabel de beschrijvende naam te geven.

   ![Lid worden van lokale en externe gegevens](media/saas-tenancy-cross-tenant-reporting/query2-plan.png)

6. Nu selecteren de *op welke dag zijn de meest tickets verkocht?* query en druk op **F5**.

   Deze query heeft iets meer complexe lid worden en cumulatie-instellingen. De meeste van de verwerking doet zich voor op afstand.  Slechts worden één rijen, met elke wetten ticket verkoop aantal per dag per dag, geretourneerd met de hoofd-database.

   ![query](media/saas-tenancy-cross-tenant-reporting/query3-plan.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * Gedistribueerde query's uitvoeren voor alle databases van de tenant
> * Implementeer een rapportagedatabase en het schema dat is vereist voor het uitvoeren van gedistribueerde query's te definiëren.


Nu proberen de [Tenant Analytics zelfstudie](saas-tenancy-tenant-analytics.md) uitpakken gegevens verkennen met een afzonderlijke analytics-database voor complexere analytics-verwerking.

## <a name="additional-resources"></a>Aanvullende resources

* Aanvullende [zelfstudies waarin voort op de toepassing Wingtip Tickets SaaS Database Per Tenant bouwen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastische query](sql-database-elastic-query-overview.md)
