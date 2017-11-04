---
title: Ad-hoc Analytics-query's uitvoeren voor meerdere Azure SQL-databases | Microsoft Docs
description: Ad-hoc analysequery's via meerdere SQL-databases in een voorbeeld van een multitenant-app uitvoeren.
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
ms.date: 06/23/2017
ms.author: billgib; sstein
ms.openlocfilehash: 849f0570fb1550f6c3676fc070d0f862450ade9a
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="run-ad-hoc-analytics-queries-across-multiple-azure-sql-databases"></a>Ad-hoc analytics query's uitvoeren op meerdere Azure SQL-databases

In deze zelfstudie kunt u gedistribueerde query's uitvoeren op de volledige set van multitenant-databases zodat ad-hoc analytics. Elastische Query wordt gebruikt voor gedistribueerde query's inschakelen die dat een database voor aanvullende analyses (naar de catalogusserver vereist) is geïmplementeerd. Deze query's kunnen inzicht in de dagelijkse operationele gegevens van de app Wingtip SaaS overspoeld extraheren.


In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]

> * Deze weergaven inschakelen over de globale weergaven in elke database, efficiënt doorzoeken voor tenants
> * Het implementeren van een ad-hoc analytics-database
> * Het uitvoeren van gedistribueerde query's voor alle databases voor tenant



U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De Wingtip SaaS-app wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en Verken de Wingtip SaaS-toepassing](sql-database-saas-tutorial.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.
* SQL Server Management Studio (SSMS) is geïnstalleerd. Als u wilt downloaden en installeren van SSMS, Zie [Download SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


## <a name="ad-hoc-analytics-pattern"></a>Ad-hoc analytics patroon

Een van de grote kansen met SaaS-toepassingen is het gebruik van de enorme hoeveelheid tenant gegevens centraal worden opgeslagen in de cloud. Deze gegevens gebruiken om inzicht in de bewerking en het gebruik van uw toepassing te krijgen. Deze inzichten kunnen leiden functie ontwikkeling, bruikbaarheid verbeteringen en andere investeringen in uw apps en services.

U kunt gemakkelijk toegang tot deze gegevens krijgen in een database met meerdere tenants, maar dit is niet zo gemakkelijk als de gegevens op schaal zijn verdeeld over misschien wel duizenden databases. Een aanpak is het gebruik van [elastische Query](sql-database-elastic-query-overview.md), waardoor query in een gedistribueerde set van databases met algemene schema. Elastische Query gebruikt een enkel *head* database waarin externe tabellen worden gedefinieerd die spiegelen tabellen of weergaven in de gedistribueerde (tenant)-databases. Query's die naar deze hoofddatabase worden verzonden, worden gecompileerd tot een gedistribueerd queryplan, waarbij delen van de query, wanneer nodig, naar de onderliggende tenantdatabases worden gepusht. Elastische query's maken gebruik van een shard-kaart in de catalogusdatabase voor de locatie van de tenantdatabases. Installatie en query zijn simpel met behulp van standaard [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference), en ondersteuning voor ad-hoc opvragen van hulpprogramma's zoals Power BI en Excel.

Door query's over de tenant-databases worden verdeeld, biedt elastische Query onmiddellijk inzicht in live productiegegevens. Echter, zoals elastische Query gegevens op uit mogelijk veel databases haalt, latentie van query kunt soms hoger zijn dan voor gelijkwaardige query's verzonden naar een enkele multitenant-database. Zorg ervoor dat ontwerp query's te minimaliseren, de gegevens die wordt geretourneerd. Elastische Query is vaak het beste geschikt voor kleine hoeveelheden realtime gegevens, in plaats van veelgebruikte gebouw of complexe analytics query's of rapporten uitvoeren van query's. Als het uitvoeren van query's niet goed, bekijkt u de [uitvoeringsplan](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) om te zien welke deel van de query heeft omlaag op de externe database is gedrukt en hoeveel gegevens geretourneerd. Query's waarvoor complexe analytische verwerking is wellicht beter afgehandeld in sommige gevallen door tenant-gegevens in te pakken op een specifieke database of het datawarehouse is geoptimaliseerd voor analysequery's. Dit patroon wordt uitgelegd in de [tenant analytics zelfstudie](sql-database-saas-tutorial-tenant-analytics.md). 

## <a name="get-the-wingtip-application-scripts"></a>De scripts van de Wingtip-toepassing downloaden

De Wingtip SaaS-scripts en de broncode van toepassing zijn beschikbaar in de [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-opslagplaats. [Stappen voor het downloaden van de scripts Wingtip SaaS](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="create-ticket-sales-data"></a>Ticket verkoopgegevens maken

Voor informatie over het uitvoeren van query's op basis van een gegevensset interessanter maken verkoopgegevens ticket met de ticket-generator.

1. In de *PowerShell ISE*, open de... \\Learning-Modules\\operationele Analytics\\ad-hoc Analytics\\*Demo AdhocAnalytics.ps1* script en stel de volgende waarden:
   * **$DemoScenario** = 1, **tickets voor gebeurtenissen op alle plaatsen aanschaffen**.
2. Druk op **F5** voor het uitvoeren van het script en ticket verkoop genereren. Terwijl het script wordt uitgevoerd, blijven de stappen in deze zelfstudie. De gegevens van het ticket query wordt uitgevoerd in de *ad-hoc gedistribueerde query's uitvoeren* sectie, dus wacht u totdat de ticket-generator om te voltooien.

## <a name="explore-the-global-views"></a>Algemene weergaven verkennen

De Wingtip SaaS-toepassing is gebouwd met behulp van een model tenant per database, zodat het schema van de tenant-database vanuit het perspectief van een één-tenant is gedefinieerd. Tenant-specifieke informatie in een tabel bestaat *wilt*, die altijd is één rij en wordt geïmplementeerd als een heap, zonder een primaire sleutel. Andere tabellen in het schema niet hoeft te zijn gerelateerd aan de *wetten* tabel, omdat bij normaal gebruik er nooit twijfel waarover de gegevens behoort.

Bij het uitvoeren van query's voor alle databases, is het echter belangrijk dat de gegevens in elastische Query kan worden behandeld alsof deze een onderdeel van één logische database shard door tenant is. Om te simuleren dit patroon, een reeks 'global' weergaven worden toegevoegd aan de tenant-database dat project een tenant-id in elk van de tabellen die globaal worden opgevraagd. Bijvoorbeeld, de *VenueEvents* weergave toegevoegd een berekende *VenueId* aan de kolommen geprojecteerd vanuit de *gebeurtenissen* tabel. Met het definiëren van de externe tabel in de head-database via *VenueEvents* (in plaats van de onderliggende *gebeurtenissen* tabel), elastische Query is in staat om joins op basis van *VenueId*zodat ze kunnen worden uitgevoerd parallel op elke externe database (in plaats van op de head-database). Dit vermindert aanzienlijk de hoeveelheid gegevens die wordt geretourneerd, wat tot een aanzienlijke toename van de prestaties voor veel query's leidt. Deze globale weergaven zijn vooraf gemaakte in alle databases van de tenant (en in *basetenantdb*).

1. Open SSMS en [verbinding maken met de tenants1 -&lt;gebruiker&gt; server](sql-database-wtp-overview.md#explore-database-schema-and-execute-sql-queries-using-ssms).
2. Vouw **Databases**, met de rechtermuisknop op **contosoconcerthall**, en selecteer **nieuwe Query**.
3. De volgende query's uitvoeren om het verschil tussen de tabellen voor één tenant en de globale weergaven verkennen:

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

1. In **Objectverkenner**, vouw **contosoconcethall** > **weergaven**:

   ![Weergaven](media/sql-database-saas-tutorial-adhoc-analytics/views.png)

2. Met de rechtermuisknop op **dbo. Plaatsen**.
3. Selecteer **weergeven als een Script** > **maken naar** > **nieuw venster van de Query-Editor**

Een van de andere script *wetten* weergaven om te zien hoe ze toevoegen de *VenueId*.

## <a name="deploy-the-database-used-for-ad-hoc-distributed-queries"></a>De database die wordt gebruikt voor ad-hoc gedistribueerde query's implementeren

In deze oefening implementeert de *adhocanalytics* database. Dit is de head-database met het schema voor het uitvoeren van query's voor alle databases van de tenant. De database wordt geïmplementeerd op de bestaande catalogusserver de server gebruikt voor alle databases die betrekking hebben in de voorbeeld-app is.

1. Open ...\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalytics.ps1* in *PowerShell ISE* en stel de volgende waarden in:
   * **$DemoScenario** = 2, **Ad-hoc Analytics-database implementeren**.

2. Druk op **F5** het script uitvoeren en maken het *adhocanalytics* database.

In de volgende sectie voegt u schema met de database zodat deze kan worden gebruikt voor gedistribueerde query's uitvoeren.

## <a name="configure-the-database-for-running-distributed-queries"></a>De database voor het uitvoeren van gedistribueerde query's configureren

In deze oefening wordt schema (de externe gegevensbron en een externe tabeldefinities) toegevoegd aan de ad-hoc analytics-database waarmee een query voor alle databases van de tenant.

1. Open SQL Server Management Studio en verbinding maken met de ad-hoc Analytics-database die u in de vorige stap hebt gemaakt. De naam van de database is *adhocanalytics*.
2. Open ...\Learning Modules\Operational Analytics\Adhoc Analytics\ *initialiseren AdhocAnalyticsDB.sql* in SSMS.
3. Controleer het SQL-script en Let op het volgende:

   Een database-scoped referentie elastische Query gebruikt voor toegang tot elk van de tenant-databases. Deze referentie moet beschikbaar zijn in alle databases en moeten normaal gesproken verleend de minimale rechten nodig zijn voor deze ad-hocquery's inschakelen.

    ![referentie maken](media/sql-database-saas-tutorial-adhoc-analytics/create-credential.png)

   De externe gegevensbron die is gedefinieerd voor het gebruik van de tenant shard-toewijzing in de catalogusdatabase. Query's worden via deze als de externe gegevensbron, gedistribueerd naar alle databases in de catalogus wordt geregistreerd wanneer de query wordt uitgevoerd. Omdat servernamen verschillend voor elke implementatie zijn, dit Initialisatiescript van de locatie van de catalogusdatabase opgehaald door op te halen van de huidige server (@@servername) waarop het script wordt uitgevoerd.

    ![externe gegevensbron maken](media/sql-database-saas-tutorial-adhoc-analytics/create-external-data-source.png)

   De externe tabellen die verwijzen naar de globale weergaven in de vorige sectie beschreven en gedefinieerd met **DISTRIBUTION = SHARDED(VenueId)**. Omdat elke *VenueId* is toegewezen aan een individuele database, dit verbetert de prestaties voor veel scenario's zoals weergegeven in de volgende sectie.

    ![externe tabellen maken](media/sql-database-saas-tutorial-adhoc-analytics/external-tables.png)

   De lokale tabel *VenueTypes* die is gemaakt en ingevuld. Deze gegevens verwijzingstabel is veel voor in alle databases voor tenant, zodat hier aangegeven als een lokale tabel en gevuld met de algemene gegevens kunnen worden weergegeven. Voor sommige query's die hierdoor wordt de hoeveelheid gegevens die worden verplaatst tussen de tenant-databases en de *adhocanalytics* database.

    ![Tabel maken](media/sql-database-saas-tutorial-adhoc-analytics/create-table.png)

   Als u verwijzingsdimensies op deze manier opneemt, moet u het tabelschema en de gegevens bijwerken wanneer u de databases tenant bijwerkt.

4. Druk op **F5** voor het uitvoeren van het script en initialiseren van de *adhocanalytics* database. 

U kunt nu gedistribueerde query's uitvoeren en inzichten te verzamelen over alle huurders!

## <a name="run-ad-hoc-distributed-queries"></a>Ad-hoc gedistribueerde query's uitvoeren

Nu de *adhocanalytics* database is ingesteld, gaat u verder gaan en sommige gedistribueerde query's uitvoeren. Het plan kan worden uitgevoerd voor een beter begrip van waar de queryverwerking gebeurt er bevatten. 

Bij de inspectie van het uitvoeringsplan Beweeg de muisaanwijzer over de pictogrammen plan voor meer informatie. 

Belangrijk te weten, is deze instelling **DISTRIBUTION = SHARDED(VenueId)** wanneer we de externe gegevensbron gedefinieerd, verbetert de prestaties voor veel scenario's. Omdat elke *VenueId* is toegewezen aan een individuele database, is eenvoudig gefilterd op afstand, retourneert alleen de gegevens die we nodig hebben.

1. Open ...\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalyticsQueries.sql* in SSMS.
2. Zorg ervoor dat u verbonden bent met de **adhocanalytics** database.
3. Selecteer de **Query** en klik op **werkelijke uitvoeringsplan opnemen**
4. Markeer de *welke plaatsen zijn momenteel geregistreerd?* query en druk op **F5**.

   De query retourneert de hele wetten lijst ter illustratie van hoe snel en eenvoudig het is in alle tenants opvragen en gegevens retourneren vanaf elke tenant.

   Bekijk het plan en dat alle kosten voor de externe query is omdat we gewoon bent gaan naar elke tenant-database en de informatie die u wilt selecteren.

   ![Selecteer * uit dbo. Plaatsen](media/sql-database-saas-tutorial-adhoc-analytics/query1-plan.png)

5. Selecteer de volgende query's en druk op **F5**.

   Gegevens van de tenant-databases en de lokale lid wordt van deze query *VenueTypes* tabel (lokale computer, zoals deze wordt een tabel de *adhocanalytics* database).

   Het plan controleren en zien dat het merendeel van de kosten van de externe query omdat er een query uitvoeren op elke tenant wilt info (dbo. Plaatsen) en voert u een snelle lokale koppelen met de lokale *VenueTypes* tabel de beschrijvende naam te geven.

   ![Lid worden van lokale en externe gegevens](media/sql-database-saas-tutorial-adhoc-analytics/query2-plan.png)

6. Nu selecteren de *op welke dag zijn de meest tickets verkocht?* query en druk op **F5**.

   Deze query heeft iets meer complexe lid worden en cumulatie-instellingen. Wat is belangrijk te weten is dat de meeste van de verwerking op afstand wordt uitgevoerd en nogmaals we alleen de rijen die we nodig hebben terugbrengen, slechts één rij voor elke wetten cumulatieve ticket verkoop aantal per dag retourneren.

   ![query](media/sql-database-saas-tutorial-adhoc-analytics/query3-plan.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * Gedistribueerde query's uitvoeren voor alle databases van de tenant
> * Implementeer een ad-hoc analytics-database en schema niet toevoegen aan deze gedistribueerde query's uitvoeren.


Nu proberen de [Tenant Analytics zelfstudie](sql-database-saas-tutorial-tenant-analytics.md) uitpakken gegevens verkennen met een afzonderlijke analytics-database voor complexere analytics verwerking...

## <a name="additional-resources"></a>Aanvullende bronnen

* Aanvullende [zelfstudies waarin voort op de Wingtip SaaS-toepassing bouwen](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastische query](sql-database-elastic-query-overview.md)
