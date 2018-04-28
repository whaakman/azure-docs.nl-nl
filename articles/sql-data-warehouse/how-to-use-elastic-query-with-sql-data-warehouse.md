---
title: Elastische query - toegang tot gegevens in Azure SQL Data Warehouse van Azure SQL Database | Microsoft Docs
description: Meer informatie over aanbevolen procedures voor het gebruik van elastische Query gebruiken voor toegang tot gegevens in Azure SQL Data Warehouse van Azure SQL Database.
services: sql-data-warehouse
author: hirokib
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/11/2018
ms.author: elbutter
ms.reviewer: igorstan
ms.openlocfilehash: ceda0399ae98e2a36fd41b954a741e0379c77fe7
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="best-practices-for-using-elastic-query-in-azure-sql-database-to-access-data-in-azure-sql-data-warehouse"></a>Aanbevolen procedures voor het gebruik van elastische Query in Azure SQL Database voor toegang tot gegevens in Azure SQL Data Warehouse
Meer informatie over aanbevolen procedures voor het gebruik van elastische Query voor toegang tot gegevens in Azure SQL Data Warehouse van Azure SQL Database. 

## <a name="what-is-an-elastic-query"></a>Wat is een elastische query?
Een elastische query kunt u een query schrijven in een Azure SQL-database die op afstand wordt verzonden naar een Azure SQL datawarehouse met T-SQL en externe tabellen. Met deze functie biedt kostenbesparingen en meer zodat architecturen, afhankelijk van het scenario.

Met deze functie kunt twee scenario's beschreven:

1. Domeinisolatie
2. Externe query's uitvoeren

### <a name="domain-isolation"></a>Domeinisolatie

Domeinisolatie verwijst naar het klassieke gegevens datamart scenario. In bepaalde gevallen, mogelijk een wilt bieden een logische domein van de gegevens op downstream gebruikers die zijn geïsoleerd van de rest van het datawarehouse voor een aantal redenen, met inbegrip van doch niet beperkt tot:

1. Isolatie van resources - SQL-database is geoptimaliseerd voor een grote basis van gelijktijdige gebruikers behoeve enigszins andere werkbelastingen dan de grote analytische query's die voor het datawarehouse is gereserveerd. Isolatie zorgt ervoor dat de juiste werkbelastingen door de juiste hulpprogramma's worden behandeld.
2. Isolatie - te scheiden van een subset van de gemachtigde gegevens selectief via bepaalde schema's.
3. Sandbox - een reeks voorbeeldgegevens opgeven als een 'playground' om te verkennen productie query's enz.

Elastische query kunt bieden de mogelijkheid gemakkelijk subsets van SQL datawarehouse-gegevens te selecteren en verplaatsen naar een SQL database-exemplaar. Deze isolatie belet bovendien niet de mogelijkheid voor ook externe query uitvoeren waardoor het interessanter 'cache'-scenario's.

### <a name="remote-query-execution"></a>Externe query's uitvoeren

Elastische query kunt u externe query uitvoeren op een exemplaar van SQL datawarehouse. Een kan gebruikmaken van het beste van zowel SQL-database en SQL datawarehouse door het scheiden van uw warme en koude gegevens tussen de twee databases. Gebruikers kunnen meer recente gegevens in een SQL-database, die, rapporten en grote aantallen gemiddelde zakelijke gebruikers voorzien kan behouden. Wanneer er meer gegevens of berekening is vereist, kan een gebruiker echter deel uitmaken van de query voor een SQL datawarehouse-exemplaar waar grootschalige statistische functies kunnen worden verwerkt veel sneller en efficiënter offloaden.

## <a name="elastic-query-process"></a>Elastische queryproces
Een elastische query kan worden gebruikt om gegevens die zich in een SQL data warehouse te SQL database-exemplaren. Elastische query kunt query's van een SQL-database verwijzen naar tabellen in een extern exemplaar van SQL datawarehouse. 

De eerste stap is het maken van een definitie van de externe gegevensbron die verwijst naar het SQL datawarehouse-exemplaar, dat gebruikmaakt van de referenties van een bestaande gebruiker in de SQL datawarehouse. Er zijn geen wijzigingen nodig op het externe exemplaar van SQL datawarehouse. 

> [!IMPORTANT] 
> 
> U moet beschikken over een externe gegevensbron ALTER machtiging. Deze machtiging is opgenomen in de machtiging ALTER DATABASE. EEN externe gegevensbron ALTER machtigingen nodig om te verwijzen naar externe gegevensbronnen.

Maak vervolgens een definitie van de externe externe tabel in een SQL database-instantie, die naar een externe tabel in de SQL datawarehouse wijst. Wanneer een query maakt gebruik van een externe tabel, is het gedeelte van de query verwijst naar de externe tabel verzonden naar het exemplaar van SQL datawarehouse worden verwerkt. Nadat de query is voltooid, wordt de resulterende set verzonden terug naar de aanroepende SQL database-instantie. Zie voor een korte zelfstudie voor het instellen van een elastische Query tussen de SQL-database en SQL datawarehouse de [elastische Query configureren met SQL Data Warehouse][Configure Elastic Query with SQL Data Warehouse].

Zie voor meer informatie over elastische Query met SQL database, de [overzicht van Azure SQL Database elastische query][Azure SQL Database elastic query overview].

## <a name="best-practices"></a>Aanbevolen procedures
Gebruik deze best practices elastische query effectief.

### <a name="general"></a>Algemeen

- Wanneer u externe query's gebruikt, zorg ervoor dat u alleen nodig kolommen te selecteren en de juiste filters zijn toegepast. Niet alleen biedt deze toename van de berekening die nodig zijn, maar verhoogt ook de grootte van de resultatenset en daarom de hoeveelheid gegevens die moeten worden verplaatst tussen de twee exemplaren.
- Gegevens van de geclusterde columnstore voor analytiIcal prestaties voor analysedoeleinden in SQL Data Warehouse en SQL-Database behouden.
- Zorg ervoor dat de brontabellen voor query's en verkeer worden gepartitioneerd.
- Zorg ervoor dat SQL database-exemplaren gebruikt als cache om gedetailleerdere updates en eenvoudiger beheer mogelijk te worden gepartitioneerd. 
- In het ideale geval PremiumRS databases niet gebruiken omdat ze bieden de analytische voordelen van de geclusterde columnstore indexeren bij richten op de i/o-intensieve werkbelastingen met een korting van Premium-databases.
- Na is geladen voor gebruik van load ingangsdatum identificatie kolommen voor upserts in de SQL-Database-exemplaren de integriteit van de cache-bron te handhaven. 
- Maak een afzonderlijke aanmelding en de gebruiker in uw SQL datawarehouse-exemplaar voor uw SQL-database extern aanmeldingsreferenties gedefinieerd in de externe gegevensbron. 

### <a name="elastic-querying"></a>Elastische opvragen

- In veel gevallen kan een wilt voor het beheren van een type van de gespreide tabel, waarbij een deel van de tabel in de SQL-Database de gegevens in de cache voor de prestaties met de rest van de gegevens die zijn opgeslagen in SQL Data Warehouse is. U moet twee objecten in een SQL-Database: een externe tabel in SQL-Database die verwijst naar de basistabel in SQL Data Warehouse en het gedeelte 'in de cache' van de tabel in de SQL-Database. U kunt een weergave maakt via de bovenkant van het in de cache-gedeelte van de tabel en de externe tabel welke samenvoegingen zowel tabellen en filters die scheiden van gegevens in SQL-Database en SQL Data Warehouse-gegevens toegankelijk zijn via externe tabellen gematerialiseerd toegepast.

  Stel dat u wilt behouden van de meest recente jaar van de gegevens in een SQL database-exemplaar. De **toest. Orders** tabel verwijst naar het datawarehouse orders tabellen. De **dbo. Orders** vertegenwoordigt de meest recente jaren aan gegevens in de SQL database-instantie. In plaats van gebruikers vragen om te beslissen of u query uitvoeren op één tabel of de andere, moet u een weergave maken via de bovenkant van beide tabellen op het punt partitie van het meest recente jaar.

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

  Een weergave die wordt geproduceerd zodanig gaan we de query-compiler bepalen of moet het exemplaar van het datawarehouse gebruiken om uw gebruikers-query te beantwoorden. 

  Er is overhead van indienen, compileren uitgevoerd en verplaatsen van gegevens die zijn gekoppeld aan elke elastische query op het exemplaar van datawarehouse. Worden cognizant dat elke elastische query in mindering op uw sleuven gelijktijdigheid van taken gebracht en bronnen worden gebruikt.  


- Als een plan is om in te zoomen verder in de resultatenset van exemplaar van het datawarehouse, overweeg dan het materialiseren in een tijdelijke tabel in de SQL-Database voor prestaties en om te voorkomen dat onnodige Resourcegebruik.

### <a name="moving-data"></a>Verplaatsen van gegevens 

- Indien mogelijk, houd gegevensbeheer eenvoudiger met alleen toevoegen brontabellen dat updates eenvoudig bruikbaar tussen de data warehouse en database-exemplaren zijn.
- Verplaatsing van gegevens op het niveau van de partitie met leegmaken en opvulling semantiek te minimaliseren, de kosten van de query op de gegevens datawarehouse-niveau en de hoeveelheid gegevens verplaatst naar de database-instantie up-to-date te houden. 

### <a name="when-to-choose-azure-analysis-services-vs-sql-database"></a>Wanneer Azure Analysis Services-tegenover SQL-Database kiezen

Gebruik Azure Analysis Services-wanneer:

- U wilt uw cache gebruiken met een BI-hulpprogramma waarmee een groot aantal kleine query 's
- U moet de latentie van query subsecond
- U bent ervaren bij het beheren van/ontwikkelen van modellen voor Analysis Services 

Gebruik van Azure SQL Database wanneer:

- U wilt zoeken van uw gegevens in cache met behulp van SQL
- U moet extern worden uitgevoerd voor bepaalde query 's
- Hebt u grotere cache-vereisten

## <a name="faq"></a>Veelgestelde vragen

V: kan ik databases in een elastische groep met elastische Query gebruiken?

A: Ja. SQL-Databases in een elastische Pool kunt elastische Query gebruiken. 

V: is er een limiet voor het aantal databases kan ik voor elastische Query?

A: Er is geen vaste limiet voor het hoeveel databases kunnen worden gebruikt voor de elastische Query. Elke elastische Query (query's die SQL Data Warehouse bereikt) wordt echter meetellen voor normale gelijktijdigheid limieten.

V: zijn er DTU-limieten die zijn betrokken bij elastische Query?

A: DTU-limieten zijn niet alle anders met elastische Query die zijn opgelegd. Het standaardbeleid is zodanig dat logische servers beschikken over DTU-limieten om te voorkomen dat klanten onbedoeld overspending. Als u verschillende databases voor elastische query samen met een exemplaar van SQL Data Warehouse inschakelen wilt, kunt u het kapje onverwacht bereikt. Als dit het geval is, moet u een aanvraag om de limiet DTU op uw logische server te verhogen indienen. U kunt uw quotum door verhogen [een ondersteuningsticket maken](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket) en te selecteren *quotum* als het aanvraagtype

V: kan ik gebruiken rij niveau beveiliging/dynamische gegevens maskeren met elastische Query?

A: klanten die willen meer geavanceerde beveiligingsfuncties gebruiken met SQL Database kunnen doen door de eerste verplaatsen en opslaan van gegevens in de SQL-Database. U kunt beveiliging op rijniveau of DDM momenteel toepassen op gegevens die zijn opgevraagd via externe tabellen. 

V: kan ik schrijven van mijn SQL database-instantie naar het datawarehouse?

A: deze functie wordt momenteel niet ondersteund. Ga naar onze [feedbackpagina] [ Feedback page] te maken/stemmen voor deze functionaliteit als dit is een functie die u zou willen zien in de toekomst. 

V: kan ik ruimtelijke typen zoals geometrie/Geografie gebruiken?

A: spatiale typen kunt in SQL Data Warehouse u opslaan als varbinary(max) waarden. Als u deze kolommen met elastische query een query, kunt u ze kunt converteren naar de juiste typen tijdens runtime.

![spatiale typen](./media/sql-data-warehouse-elastic-query-with-sql-database/geometry-types.png)

<!--Article references-->

[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Configure Elastic Query with SQL Data Warehouse]: tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md
[Feedback Page]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Azure SQL Database elastic query overview]: ../sql-database/sql-database-elastic-query-overview.md


