---
title: Uitvoeren met het melden van query's voor meerdere Azure SQL-databases | Microsoft Docs
description: Cross-tenant-rapportage met behulp van gedistribueerde query's.
keywords: zelfstudie sql-database
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: billgib
ms.reviewer: sstein; AyoOlubeko
ms.openlocfilehash: 2b8cc66909fc4f066670424fd9127c311cf618e0
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44378042"
---
# <a name="cross-tenant-reporting-using-distributed-queries"></a>Cross-tenant-rapportage met behulp van gedistribueerde query 's

In deze zelfstudie hebt u gedistribueerde query's uitvoeren voor de volledige set van tenant databases voor rapportage. Deze query's kunnen extraheren van inzichten ophalen uit dagelijkse operationele gegevens van de Wingtip Tickets SaaS-tenants. U doet dit door een extra reporting-database implementeren op de catalogusserver en elastische query's gebruiken om in te schakelen van gedistribueerde query's.


In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]

> * Over het implementeren van een rapportagedatabase
> * Het uitvoeren van gedistribueerde query's in alle tenantdatabases
> * Hoe algemene weergaven in elke database kunnen inschakelen efficiënte query's voor tenants


U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:


* De Wingtip Tickets SaaS Database Per Tenant-app is geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en verkennen van de toepassing Wingtip Tickets SaaS Database Per Tenant](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.
* SQL Server Management Studio (SSMS) is geïnstalleerd. Als u wilt downloaden en installeren van SSMS, Zie [Download SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


## <a name="cross-tenant-reporting-pattern"></a>Cross-tenant-rapportage-patroon

![patroon voor gedistribueerde query cross-tenant](media/saas-tenancy-cross-tenant-reporting/cross-tenant-distributed-query.png)

Een verkoopkans met SaaS-toepassingen is het gebruik van de grote hoeveelheid tenantgegevens die zijn opgeslagen in de cloud voor inzicht in de werking en het gebruik van uw toepassing. Deze inzichten kunnen dienen als richtlijn ontwikkelen van functies, bruikbaarheid verbeteren en andere investeringen doen in uw apps en services.

U kunt gemakkelijk toegang tot deze gegevens krijgen in een database met meerdere tenants, maar dit is niet zo gemakkelijk als de gegevens op schaal zijn verdeeld over misschien wel duizenden databases. Een aanpak is het gebruik van [elastische Query](sql-database-elastic-query-overview.md), waarmee query's uitvoeren in een gedistribueerde set databases met hetzelfde schema. Deze databases in verschillende resourcegroepen en abonnementen kunnen worden gedistribueerd, maar moeten delen een gemeenschappelijke aanmelding. Elastische query's gebruiken één *head* database waarin externe tabellen zijn gedefinieerd die tabellen of weergaven in de gedistribueerde (tenant) databases spiegelen. Query's die naar deze hoofddatabase worden verzonden, worden gecompileerd tot een gedistribueerd queryplan, waarbij delen van de query, wanneer nodig, naar de onderliggende tenantdatabases worden gepusht. Elastische Query gebruikt de shard-toewijzing in de catalogusdatabase om te bepalen van de locatie van alle tenantdatabases. Installatie en query van de hoofddatabase zijn simpel met behulp van standaard [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference), en ondersteuning voor het uitvoeren van query's vanuit hulpprogramma's zoals Power BI en Excel.

Door het distribueren van query's in de tenant-databases, biedt elastische Query onmiddellijk inzicht in live productiegegevens. Als een elastische Query gegevens op uit mogelijk veel databases haalt, kan latentie van query hoger zijn dan gelijkwaardige query's verzonden naar een database met meerdere tenants. Ontwerp zo min mogelijk de gegevens die wordt geretourneerd naar de hoofddatabase als er query's. Elastische query's is vaak het best geschikt voor kleine hoeveelheden realtime-gegevens, in plaats van veelgebruikte bouwen of complexe analytics-query's of rapporten uitvoeren van query's. Als het uitvoeren van query's niet goed, bekijkt u de [uitvoeringsplan](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) om te zien welk deel van de query op de externe database is verlaagd en hoeveel gegevens geretourneerd. Query's waarvoor complexe aggregatie of analytische verwerking mogelijk beter verwerkt door de gegevens van de tenant te pakken op een database of het datawarehouse geoptimaliseerd voor analysequery's. Dit patroon wordt beschreven in de [zelfstudie tenant-analytics](saas-tenancy-tenant-analytics.md). 

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>De scripts van de toepassing Wingtip Tickets SaaS Database Per Tenant ophalen

De Wingtip Tickets SaaS multitenant-Database-scripts en broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de blokkering opheffen van de Wingtip Tickets SaaS-scripts.

## <a name="create-ticket-sales-data"></a>Verkoopgegevens ticket maken

Voor query's uitvoeren op een interessante verzameling, verkoopgegevens ticket te maken door het uitvoeren van de ticket-generator.

1. In de *PowerShell ISE*, open de... \\Learning Modules\\Operational Analytics\\ad-hoc Reporting\\*Demo-AdhocReporting.ps1* script en de volgende waarde instellen:
   * **$DemoScenario** = 1, **tickets kopen voor voorstellingen bij alle theaters**.
2. Druk op **F5** voor het uitvoeren van het script en ticket verkoop genereren. Terwijl het script wordt uitgevoerd, blijven de stappen in deze zelfstudie. De gegevens van het ticket query wordt uitgevoerd in de *ad-hoc gedistribueerde query's uitvoeren* sectie, dus wacht totdat de ticket-generator om te voltooien.

## <a name="explore-the-global-views"></a>Verken de algemene weergaven

In de toepassing Wingtip Tickets SaaS Database Per Tenant krijgt elke tenant een database. Daarom worden de gegevens in de databasetabellen is afgestemd op het perspectief van één tenant. Bij het uitvoeren van query's voor alle databases, is het echter belangrijk dat de gegevens in elastische query's kunnen worden behandeld alsof deze deel van een enkele logische database shard door de tenant uitmaakt. 

Als u wilt simuleren dit patroon, een set 'global' weergaven worden toegevoegd aan de tenant-database dat project een tenant-ID in elk van de tabellen die wereldwijd worden opgevraagd. Bijvoorbeeld, de *VenueEvents* weergave toegevoegd een berekende *VenueId* aan de kolommen geprojecteerd vanuit de *gebeurtenissen* tabel. Op dezelfde manier de *VenueTicketPurchases* en *VenueTickets* weergaven toevoegen een berekende *VenueId* kolom geprojecteerd vanuit hun respectieve tabellen. Deze weergaven door elastische query's worden gebruikt voor het parallel query's en push ze naar de juiste externe tenant-database als een *VenueId* kolom aanwezig is. Dit vermindert aanzienlijk de hoeveelheid gegevens die wordt geretourneerd en resulteert in een aanzienlijke toename in de prestaties voor veel query's. Deze algemene weergaven zijn vooraf gemaakte in alle tenantdatabases.

1. Open SSMS en [verbinding maken met de tenants1 -&lt;gebruiker&gt; server](saas-tenancy-wingtip-app-guidance-tips.md#explore-database-schema-and-execute-sql-queries-using-ssms).
1. Vouw **Databases**, met de rechtermuisknop op _contosoconcerthall_, en selecteer **nieuwe Query**.
1. De volgende query's uitvoeren om het verschil tussen de tabellen met één tenant en de algemene weergaven verkennen:

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

In deze weergaven, de *VenueId* wordt berekend als een hash van de naam van de locatie, maar elke methode kan worden gebruikt om te introduceren een unieke waarde. Deze aanpak is vergelijkbaar met de manier waarop die de tenant-sleutel voor gebruik in de catalogus wordt berekend.

Het onderzoeken van de definitie van de *Venues* weergeven:

1. In **Objectverkenner**, vouw **contosoconcerthall** > **weergaven**:

   ![weergaven](media/saas-tenancy-cross-tenant-reporting/views.png)

2. Met de rechtermuisknop op **dbo. Venues**.
3. Selecteer **Script weergeven als** > **maken naar** > **nieuw Query-Editor-venster**

Script voor een van de andere *Venue* weergaven om te zien hoe ze toevoegen de *VenueId*.

## <a name="deploy-the-database-used-for-distributed-queries"></a>De database die wordt gebruikt voor gedistribueerde query's implementeren

In deze oefening implementeert de _ad-hoc reporting_ database. Dit is de hoofddatabase met het schema voor het uitvoeren van query's in alle tenantdatabases. De database wordt geïmplementeerd op de bestaande catalogusserver, waarop de server die wordt gebruikt voor alle-gerelateerde databases in de voorbeeld-app.

1. in *PowerShell ISE*, openen... \\Learning Modules\\Operational Analytics\\ad-hoc Reporting\\*Demo-AdhocReporting.ps1*. 

1. Stel **$DemoScenario = 2**, _implementeren Ad-hoc rapportagedatabase_.

1. Druk op **F5** en voer het script uit de *ad-hoc reporting* database.

In de volgende sectie voegt u schema met de database, zodat het kan worden gebruikt voor gedistribueerde query's uitvoeren.

## <a name="configure-the-head-database-for-running-distributed-queries"></a>De database 'head' voor het uitvoeren van gedistribueerde query's configureren

In deze oefening wordt schema (de externe gegevensbron en definities van de externe tabel) toegevoegd aan de _ad-hoc reporting_ database om in te schakelen voor het uitvoeren van query's in alle tenantdatabases.

1. Open SQL Server Management Studio en maak verbinding met de ad-hoc-rapportdatabase die u in de vorige stap hebt gemaakt. De naam van de database is *ad-hoc reporting*.
2. Open ...\Learning Modules\Operational Analytics\Adhoc Reporting\ _initialiseren AdhocReportingDB.sql_ in SSMS.
3. Raadpleeg de SQL-script en de opmerking:

   Een database-scoped referentie elastische query's gebruiken voor toegang tot elk van de tenant-databases. Deze referentie moet beschikbaar zijn in alle databases en moeten normaal gesproken worden verleend de minimale rechten vereist om in te schakelen van deze query's.

    ![referentie maken](media/saas-tenancy-cross-tenant-reporting/create-credential.png)

   Met de catalogusdatabase als de externe gegevensbron, worden query's worden gedistribueerd naar alle databases die zijn geregistreerd in de catalogus op het moment dat de query wordt uitgevoerd. Als de namen van voor elke implementatie verschillen, met dit script opgehaald van de locatie van de catalogusdatabase van de huidige server (@@servername) waarop het script wordt uitgevoerd.

    ![externe gegevensbron maken](media/saas-tenancy-cross-tenant-reporting/create-external-data-source.png)

   De externe tabellen die verwijzen naar de globale weergaven beschreven in de vorige sectie en gedefinieerd met behulp van **distributie = SHARDED(VenueId)**. Omdat elke *VenueId* wordt toegewezen aan een individuele database, dit verbetert de prestaties voor veel scenario's zoals weergegeven in de volgende sectie.

    ![externe tabellen maken](media/saas-tenancy-cross-tenant-reporting/external-tables.png)

   De lokale tabel _VenueTypes_ die is gemaakt en ingevuld. Deze gegevenstabel verwijzing is gebruikelijk in alle tenantdatabases, zodat deze kan worden weergegeven als een lokale tabel en is ingevuld met de algemene gegevens. Deze tabel die is gedefinieerd in de hoofddatabase met verminderen de hoeveelheid gegevens die moeten worden verplaatst naar de hoofddatabase voor sommige query's.

    ![Tabel maken](media/saas-tenancy-cross-tenant-reporting/create-table.png)

   Als u verwijzingstabellen op deze manier, zorg er dan voor dat de tabelschema en de gegevens bijgewerkt wanneer u de tenant-databases bijwerken.

4. Druk op **F5** voor het uitvoeren van het script en initialiseren van de *ad-hoc reporting* database. 

U kunt nu gedistribueerde query's uitvoeren en inzichten te verwerven voor alle tenants.

## <a name="run-distributed-queries"></a>Gedistribueerde query's uitvoeren

Nu de *ad-hoc reporting* database is ingesteld, gaat u verder en sommige gedistribueerde query's uitvoeren. Het plan kan worden uitgevoerd voor een beter begrip van waar de verwerking van query's er gebeurt bevatten. 

Bij de inspectie van het abonnement kan worden uitgevoerd, Beweeg de muisaanwijzer over de pictogrammen plan voor meer informatie. 

Belangrijk te weten, is deze instelling **distributie = SHARDED(VenueId)** verbetert de prestaties voor veel scenario's wanneer de externe gegevensbron is gedefinieerd. Aangezien aan elk *VenueId* wordt toegewezen aan een individuele database, is eenvoudig gefilterd op afstand, alleen de benodigde gegevens retourneren.

1. Open... \\Learning Modules\\Operational Analytics\\ad-hoc Reporting\\*Demo-AdhocReportingQueries.sql* in SSMS.
2. Zorg ervoor dat u bent verbonden met de **ad-hoc reporting** database.
3. Selecteer de **Query** en klik op **werkelijke Execution Plan omvatten**
4. Markeer de *welke venues momenteel zijn geregistreerd?* query en druk op **F5**.

   De query retourneert de lijst met volledige venue, waarin u ziet hoe snel en eenvoudig het is om query's uitvoeren voor alle tenants en als resultaat de gegevens van elke tenant.

   Het plan controleren en zien dat de kosten voor het hele in de externe query. Elke tenantdatabase voert de query op afstand en stuurt de locatie-informatie naar de hoofddatabase.

   ![Selecteer * uit dbo. Venues](media/saas-tenancy-cross-tenant-reporting/query1-plan.png)

5. Selecteer de volgende query en druk op **F5**.

   Deze query lid wordt van gegevens van de tenant-databases en de lokale *VenueTypes* tabel (lokale computer, zoals deze wordt een tabel de *ad-hoc reporting* database).

   Het plan controleren en zien dat de meerderheid van de kosten voor de externe query's. Elke tenantdatabase retourneert de locatie-informatie en voert een lokale koppeling met de lokale *VenueTypes* tabel om de beschrijvende naam weer te geven.

   ![Neem deel aan op de lokale en externe gegevens](media/saas-tenancy-cross-tenant-reporting/query2-plan.png)

6. Selecteer nu de *op welke dag zijn de meeste tickets die worden verkocht?* query en druk op **F5**.

   Deze query heeft iets meer complexe lid worden en cumulatie-instellingen. De meeste van de verwerking doet zich voor op afstand.  Alleen worden één rijen, van de venue aantal per dag ticket verkoop per dag, die geretourneerd met de hoofd-database.

   ![query](media/saas-tenancy-cross-tenant-reporting/query3-plan.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * Gedistribueerde query's uitvoeren voor alle databases van de tenant
> * Implementeren van een reporting-database en het schema dat is vereist voor het uitvoeren van gedistribueerde query's definiëren.


Probeer nu de [Tenantanalyses zelfstudie](saas-tenancy-tenant-analytics.md) extraheren om gegevens te verkennen met een aparte analytics-database voor de analyseverwerking van meer complexe.

## <a name="additional-resources"></a>Aanvullende resources

* Aanvullende [zelfstudies voort op de toepassing Wingtip Tickets SaaS Database Per Tenant bouwen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastische query](sql-database-elastic-query-overview.md)
