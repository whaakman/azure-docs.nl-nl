---
title: Elastische query - toegang tot gegevens in Azure SQL Data Warehouse vanuit Azure SQL Database | Microsoft Docs
description: Informatie over aanbevolen procedures voor het gebruik van elastische query's voor toegang tot gegevens in Azure SQL Data Warehouse vanuit Azure SQL-Database.
services: sql-data-warehouse
author: hirokib
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/11/2018
ms.author: elbutter
ms.reviewer: igorstan
ms.openlocfilehash: 344cb1bed56b0b6af7bd3704f8674ae30695f885
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42055269"
---
# <a name="best-practices-for-using-elastic-query-in-azure-sql-database-to-access-data-in-azure-sql-data-warehouse"></a>Aanbevolen procedures voor het gebruik van elastische query's in Azure SQL Database voor toegang tot gegevens in Azure SQL Data Warehouse
Informatie over aanbevolen procedures voor het gebruik van elastische query's voor toegang tot gegevens in Azure SQL Data Warehouse vanuit Azure SQL-Database. 

## <a name="what-is-an-elastic-query"></a>Wat is een elastische query?
Een elastische query's kunt u een query schrijven in een Azure SQL-database die op afstand wordt verzonden naar een Azure SQL datawarehouse met T-SQL en externe tabellen. Met deze functie biedt kosten te besparen en meer goed presterende architecturen, afhankelijk van het scenario.

Met deze functie kunt twee primaire scenario's:

1. Domeinisolatie
2. Externe query's uitvoeren

### <a name="domain-isolation"></a>Domeinisolatie

Domeinisolatie verwijst naar de klassieke datamart-scenario. In bepaalde scenario's mogelijk een wilt bieden een logische domein van de gegevens aan downstream-gebruikers die zijn geïsoleerd van de rest van het datawarehouse, voor een aantal redenen, met inbegrip van doch niet beperkt tot:

1. Isolatie van resources - SQL-database is geoptimaliseerd om aan te bieden een grote basis van gelijktijdige gebruikers enigszins verschillende werkbelastingen dan de grote analytische query's die het datawarehouse is gereserveerd voor fungeren. Isolatie zorgt ervoor dat de juiste werkbelastingen door de juiste hulpprogramma's worden verwerkt.
2. Isolatie - voor het scheiden van een subset van de toegestane gegevens selectief via bepaalde schema's.
3. Sandbox - een voorbeeldset gegevens opgeven als een 'Speelplaats' om te verkennen, productie-query's enzovoort.

Elastische query's kunt bieden de mogelijkheid eenvoudig subsets van SQL datawarehouse-gegevens te selecteren en verplaats deze naar een exemplaar van SQL database. Deze isolatie belet bovendien niet de mogelijkheid om in te schakelen ook externe query's worden uitgevoerd zodat voor interessanter "-cache"-scenario's.

### <a name="remote-query-execution"></a>Externe query's uitvoeren

Elastische query's kunt u externe query's worden uitgevoerd op een exemplaar van SQL datawarehouse. Een kan gebruikmaken van het beste van zowel SQL database en SQL datawarehouse door te scheiden van de warme en koude gegevens tussen de twee databases. Gebruikers kunnen meer recente gegevens in een SQL-database, die rapporten en een groot aantal gemiddelde zakelijke gebruikers kan dienen houden. Als er meer gegevens of berekening nodig is, kan een gebruiker echter deel van de query naar een SQL datawarehouse-exemplaar waar grootschalige statistische functies kunnen worden verwerkt veel sneller en efficiënter offloaden.

## <a name="elastic-query-process"></a>Elastische query's verwerken
Een elastische query kan worden gebruikt om de gegevens zich bevinden in een SQL data warehouse beschikbaar maken voor SQL database-exemplaren. Elastische query's kunt query's uit een SQL-database verwijzen naar tabellen in een extern exemplaar van SQL datawarehouse. 

De eerste stap is het maken van een definitie van de externe gegevensbron die verwijst naar de SQL datawarehouse-instantie, die gebruikmaakt van bestaande referenties van de gebruiker in de SQL datawarehouse. Er zijn geen wijzigingen nodig op het externe exemplaar van SQL datawarehouse. 

> [!IMPORTANT] 
> 
> U moet beschikken over machtigingen voor een externe gegevensbron niet wijzigen. Deze machtiging is opgenomen in de machtiging ALTER DATABASE. EEN externe gegevensbron ALTER machtigingen nodig om te verwijzen naar externe gegevensbronnen.

Maak vervolgens een definitie van de externe externe tabel in een exemplaar van SQL database, die naar een externe tabel in SQL datawarehouse verwijst. Wanneer een query maakt gebruik van een externe tabel, is het gedeelte van de query verwijst naar de externe tabel verzonden naar de SQL datawarehouse-instantie moet worden verwerkt. Als de query is voltooid, wordt de resultatenset terug naar de aanroepende instantie van de SQL-database verzonden. Zie voor een korte zelfstudie over het instellen van een elastische query's tussen de SQL-database en SQL datawarehouse, de [elastische Query configureren met SQL Data Warehouse][Configure Elastic Query with SQL Data Warehouse].

Zie voor meer informatie over elastische Query met SQL database, de [overzicht van Azure SQL Database elastische query][Azure SQL Database elastic query overview].

## <a name="best-practices"></a>Aanbevolen procedures
Deze aanbevolen werkwijzen gebruikt om efficiënt gebruik van elastische query's.

### <a name="general"></a>Algemeen

- Wanneer u externe query's worden uitgevoerd, zorg ervoor dat u alleen bent door die nodig zijn kolommen te selecteren en de juiste filters zijn toegepast. Niet alleen komt deze toename van de rekenkracht die nodig zijn, maar het verhoogt ook de grootte van de resultatenset en daarom de hoeveelheid gegevens die moeten worden verplaatst tussen de twee exemplaren.
- Gegevens van geclusterde columnstore voor analytiIcal prestaties voor analysedoeleinden in SQL Data Warehouse en SQL-Database behouden.
- Zorg ervoor dat de brontabellen worden gepartitioneerd voor query's en -verplaatsing.
- Zorg ervoor dat SQL database-exemplaren die worden gebruikt als een cache worden gepartitioneerd om in te schakelen gedetailleerdere updates en eenvoudiger beheer. 
- Premium RS-databases in het ideale geval gebruik omdat ze voorzien in de analytische voordelen van de geclusterde columnstore indexeren met nadruk op i/o-intensieve workloads met een korting van Premium-databases.
- Gebruikmaken van belasting of kolommen van de ingangsdatum identificatie voor de upsert-bewerking in de SQL Database-exemplaren om integriteit van de cache-bron te behouden na belastingen. 
- Maak een afzonderlijke aanmelding en gebruiker in uw exemplaar van SQL datawarehouse voor uw SQL-database externe aanmeldingsreferenties die zijn opgegeven in de externe gegevensbron. 

### <a name="elastic-querying"></a>Elastische query's uitvoeren

- In veel gevallen wellicht een type van de gespreide tabel, waarbij een deel van de tabel in de SQL-Database gegevens in de cache voor prestaties met de rest van de gegevens die zijn opgeslagen in SQL Data Warehouse is een beheren wilt. U moet twee objecten in een SQL-Database: een externe tabel in SQL-Database die verwijst naar de basistabel in SQL Data Warehouse en het gedeelte 'in de cache' van de tabel in de SQL-Database. Houd rekening met het maken van een weergave over de bovenkant van het cache-gedeelte van de tabel en de externe tabel welke samenvoegingen zowel tabellen en filters waarmee gegevens in SQL-Database en SQL Data Warehouse-gegevens die toegankelijk is via externe tabellen gerealiseerde gescheiden is van toepassing.

  Stel dat u wilt behouden van de meest recente jaar aan gegevens in een exemplaar van SQL database. De **toest. Orders** tabel verwijst naar het datawarehouse orders tabellen. De **dbo. Orders** vertegenwoordigt de meest recente jaar aan gegevens in de SQL database-instantie. In plaats van gebruikers vragen om te bepalen of het opvragen van één tabel of de andere, moet u een weergave maken via de bovenkant van beide tabellen op het punt van de partitie van de meest recente jaar.

  ```sql
  CREATE VIEW dbo.Orders_Elastic AS
  SELECT 
    [col_a]     
  , [col_b]         
  , ...
  , [col_n]
  FROM
    [dbo].[Orders]
  WHERE 
    YEAR([o_orderdate]) >= '<Most Recent Year>'
  UNION
  SELECT 
    [col_a]     
  , [col_b]         
  , ...
  , [col_n]         
  FROM
    [ext].[Orders]
  WHERE
    YEAR([o_orderdate]) < '<Most Recent Year>'
  ```

  Een weergave die wordt geproduceerd zodanig gaan we de query-compiler bepalen of deze moet de datawarehouse-exemplaar gebruiken om de query voor uw gebruikers te beantwoorden. 

  Er is overhead van het indienen, samenstellen, uitvoeren en verplaatsen van gegevens die zijn gekoppeld aan elke elastische query's op basis van de datawarehouse-exemplaar. Worden cognizant dat elke elastische query in mindering gebracht op uw gelijktijdigheidssleuven en maakt gebruik van resources.  


- Als een plan om in te zoomen verder in de resultatenset van de datawarehouse-exemplaar, kunt u deze in een tijdelijke tabel in de SQL-Database voor betere prestaties en om te voorkomen dat onnodige Resourcegebruik materialiseren.

### <a name="moving-data"></a>Om gegevens te verplaatsen 

- Indien mogelijk, houd gegevensbeheer eenvoudiger met alleen toevoegen brontabellen dat updates eenvoudig onderhouden tussen de data warehouse en database-exemplaren worden.
- Gegevens verplaatsen op het niveau van de partitie met leegmaken en opvulling semantiek voor het minimaliseren van de kosten van de query op de gegevens datawarehouse-niveau en de hoeveelheid gegevens die zijn verplaatst naar de database-instantie up-to-date te houden. 

### <a name="when-to-choose-azure-analysis-services-vs-sql-database"></a>Bij het kiezen van Azure Analysis Services vs SQL-Database

Gebruik Azure Analysis Services wanneer:

- U van plan bent over het gebruik van uw cache met een BI-hulpprogramma dat grote aantallen kleine query's worden verzonden
- U moet de latentie van query subsecond
- U bent ervaring bij het beheren van/ontwikkelen van modellen van Analysis Services 

Gebruik van Azure SQL Database als:

- U wilt de cachegegevens van uw met behulp van SQL op te vragen
- Moet u de uitvoering op afstand voor bepaalde query 's
- Hebt u grotere cache-vereisten

## <a name="faq"></a>Veelgestelde vragen

V: kan ik databases gebruiken in een elastische Pool met elastische Query?

A: Ja. SQL-Databases in een elastische Pool kan elastische query's kunt gebruiken. 

V: is er een limiet voor het aantal databases dat ik voor een elastische Query gebruiken kan?

Antwoord: Er is geen vaste limiet voor hoeveel databases kunnen worden gebruikt voor een elastische Query. Elke elastische query's (query's die SQL Data Warehouse bereikt) wordt echter tellen mee voor normale gelijktijdigheidsbeperkingen.

V: zijn er DTU-limieten die betrokken zijn bij een elastische Query?

A: DTU-limieten zijn niet ingesteld op een andere manier met elastische query's. Het standaardbeleid is dat logische servers DTU-limieten hebben om te voorkomen dat klanten per ongeluk budgetoverschrijding. Als u verschillende databases voor elastische query's samen met een exemplaar van SQL Data Warehouse inschakelen wilt, kunt u de limiet onverwacht bereikt. Als dit het geval is, moet u een aanvraag om de limiet DTU op uw logische server te verhogen indienen. U kunt uw quotum door verhogen [het maken van een ondersteuningsticket](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket) en selecteren *quotum* als het aanvraagtype

V: kan ik gebruiken rij level security/Dynamic Data Masking met elastische Query?

A:-klanten die willen meer geavanceerde beveiligingsfuncties gebruiken met SQL Database kunnen doen door de eerste verplaatsen en opslaan van gegevens in de SQL-Database. U kunt geen beveiliging op rijniveau of DDM momenteel toepassen op gegevens die zijn opgevraagd via externe tabellen. 

V: kan ik schrijven van mijn SQL database-exemplaar naar de datawarehouse-exemplaar?

A: momenteel wordt deze functie niet ondersteund. Ga naar onze [feedbackpagina] [ Feedback page] te maken/stem voor deze functionaliteit als dit is een functie die u graag zou willen zien in de toekomst. 

V: kan ik ruimtelijke typen zoals geometrie/Geografie gebruiken?

A: u kunt ruimtelijke gegevenstypen opslaan in SQL Data Warehouse als varbinary(max) waarden. Wanneer u deze kolommen met elastische query's in query, kunt u ze kunt converteren naar de juiste typen tijdens runtime.

![ruimtelijke typen](./media/sql-data-warehouse-elastic-query-with-sql-database/geometry-types.png)

<!--Article references-->

[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Configure Elastic Query with SQL Data Warehouse]: tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md
[Feedback Page]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Azure SQL Database elastic query overview]: ../sql-database/sql-database-elastic-query-overview.md


