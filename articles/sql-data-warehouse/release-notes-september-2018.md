---
title: Azure SQL Data Warehouse opmerkingen bij de Release van September 2018 | Microsoft Docs
description: Opmerkingen bij de release voor Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 10/08/2018
ms.author: mausher
ms.reviewer: twounder
ms.openlocfilehash: bd6531bc950e006f15924bb9a0d6428f9e69d544
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56330831"
---
# <a name="whats-new-in-azure-sql-data-warehouse-september-2018"></a>Wat is er nieuw in Azure SQL Data Warehouse? September 2018
Azure SQL Data Warehouse ontvangt voortdurend verbeteringen. Dit artikel beschrijft de nieuwe functies en wijzigingen die zijn geïntroduceerd in September 2018.

## <a name="new-lower-entry-point-for-sql-data-warehouse-gen2"></a>Nieuwe lagere toegangspunt voor SQL Data Warehouse Gen2
In April 2018, [presenteerde Microsoft](https://azure.microsoft.com/blog/turbocharge-cloud-analytics-with-azure-sql-data-warehouse/) Azure SQL Data Warehouse Gen2 met 5 x de prestaties, 5 x de schaal van compute, 4 x de gelijktijdigheid van taken en onbeperkte opslag. Zoals vermeld in de [datawarehouse in de cloud Benchmark](https://gigaom.com/report/data-warehouse-in-the-cloud-benchmark/) door Onderzoeksanalist, SQL Data Warehouse Gen2 **behaalt u betere resultaten dan Amazon Redshift 42%**.

Gen2 is nu algemeen beschikbaar op een lagere vermelding punt van DWU500c zodat u kunt een kleinere grootte data warehouse of dev/test-omgevingen met alle van de meest recente verbeteringen van de service worden uitgevoerd. Het nieuwe toegangspunt behoudt alle Gen2 functies waaronder [adaptieve cache-opslag](https://azure.microsoft.com/blog/adaptive-caching-powers-azure-sql-data-warehouse-performance-gains/), [belichting snel gegevens volgorde wijzigen](https://azure.microsoft.com/blog/lightning-fast-query-performance-with-azure-sql-data-warehouse/), en ondersteuning voor [realtime datawarehouse](https://azure.microsoft.com/blog/enabling-real-time-data-warehousing-with-azure-sql-data-warehouse/).

## <a name="sql-vulnerability-assessment"></a>Evaluatie van SQL-beveiligingsproblemen
[SQL beveiligingslek Assessment (VA)](https://blogs.msdn.microsoft.com/sqlsecurity/2018/09/25/sql-vulnerability-assessment-now-supports-azure-sql-data-warehouse-and-azure-sql-database-managed-instance/) is een eenvoudig-en-klare-service die doorlopend uw datawarehouse controleert. Hiermee kunt u een hoog beveiligingsniveau te allen tijde garanderen en die uw organisatie beleidsregels wordt voldaan. Het biedt een uitgebreide beveiligingsrapport samen met bruikbare herstelstappen voor elk probleem gevonden. Dit rapport eenvoudig voor u proactief beheren van de status van uw database security en uw aandacht richten op het hoogste invloed acties, zelfs als u niet een deskundige. Voor dynamische omgevingen waarin wijzigingen worden regelmatig en moeilijk zijn om bij te houden, is de evaluatie van beveiligingsproblemen waardevol zijn bij het detecteren van de instellingen die u kunnen uw datawarehouse kwetsbaar voor aanvallen laten.

## <a name="improved-availability-with-query-restartability"></a>Verbeterde beschikbaarheid met query restartability
Tijdens het uitvoeren van query's, een willekeurig aantal problemen kan optreden die kan leiden tot een query is mislukt. Een netwerkuitval van het, een hardwarestoring of andere ontkoppeling kan leiden tot een onderbreking. SQL Data Warehouse ondersteunt nu query restartability voor stap of niveau van de instructie SELECT-query's. 

Een query op die tijdens de overdracht die is onderbroken vanwege een fout met Query Restartability, automatisch opnieuw wordt opgestart. Afhankelijk van het aantal stappen in de vorm van de query, of wanneer de query is gestopt tijdens de uitvoering, de engine van SQL Data Warehouse ofwel de volledige query opnieuw opgestart of wordt hervat vanaf de laatste querystap van de voltooide. Uit oogpunt hebben een eindgebruiker, de query alleen is voltooid. 

## <a name="maintenance-scheduling-preview"></a>Onderhoud plannen (Preview)
Azure SQL Data Warehouse onderhoud plannen is nu in preview. Deze nieuwe functie wordt naadloos geïntegreerd het geplande onderhoud Servicestatusmeldingen, Resource Health controleren bewaken en de Azure SQL Data Warehouse onderhoud Scheduler-service. Onderhoud plannen kunt u een bepaalde periode plannen wanneer is het handig is voor het ontvangen van nieuwe functies, upgrades en patches.

Planning voor onderhoud maakt gebruik van de Azure Monitor en kan klanten om te bepalen hoe ze willen worden geïnformeerd over aanstaande onderhoud en welke geautomatiseerde stromen moet worden geactiveerd voor het beheren van downtime en de impact op hun activiteiten te minimaliseren. De meldingen kunnen een e-mailadres of tekst bevatten. 

## <a name="wide-row-support-in-polybase"></a>Ondersteuning voor brede rij in Polybase
Bij het laden van gegevens in SQL Data Warehouse, de algemene richtlijnen, is het gebruik van [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/design-elt-data-loading#options-for-loading-with-polybase) met is ondersteuning voor het parallel laden van gegevens. Deze release biedt ondersteuning voor brede kolommen van 32 kB naar 1MB - zodat u kunt tabellen met breed rijgrootte laden. De ondersteuning voor brede rijen vereenvoudigt het proces voor tabellen met breed rijen laden van gegevens.

> [!Note]
> De grootte van de rij kan niet groter zijn dan 1 MB in grootte.

## <a name="additional-language-support"></a>Ondersteuning voor andere talen
Deze versie introduceert ondersteuning voor de elementen van de volgende T-SQL-taal:

### <a name="stringsplit"></a>STRING_SPLIT
De [STRING_SPLIT](https://docs.microsoft.com/sql/t-sql/functions/string-split-transact-sql) functie een tekenreeks met behulp van het opgegeven scheidingsteken gesplitst. STRING_SPLIT is handig in scenario's waarbij een kolom kan meerdere waarden te parseren en invoegen in een andere tabel laden van gegevens.

#### <a name="example"></a>Voorbeeld
```sql
DECLARE @tags NVARCHAR(400) = 'clothing,road,,touring,bike';

SELECT
    value
FROM
    STRING_SPLIT(@tags, ',')
WHERE
    RTRIM(value) <> '';
```

### <a name="compressdecompress-functions"></a>COMPRIMEREN/DECOMPRESS-functies
De [COMPRIMEREN](https://docs.microsoft.com/sql/t-sql/functions/compress-transact-sql) / [DECOMPRESS](https://docs.microsoft.com/sql/t-sql/functions/decompress-transact-sql) functies kunnen u wilt comprimeren of een tekenreeks invoeren met behulp van de GZIP-algoritme decomprimeren.

#### <a name="example"></a>Voorbeeld

```sql
SELECT
    name [name_original]
    , COMPRESS(name) [name_compressed]
    , CAST(DECOMPRESS(COMPRESS(name)) AS NVARCHAR(MAX)) [name_decompressed]
FROM
    sys.objects;
```

### <a name="if-exists-clause-for-dropping-views"></a>IF EXISTS component voor het verwijderen van weergaven
Het toevoegen van de component IF bestaat in de [DROP VIEW](https://docs.microsoft.com/sql/t-sql/statements/drop-view-transact-sql) instructie vereenvoudigt de T-SQL-code vereist voor het verwijderen van een weergave van het datawarehouse. De syntaxis als bestaat, wanneer toegepast op een weergave niet verwijderen-instructie wordt de weergave verwijderen of deze bestaat en of de instructie wordt genegeerd als de weergave niet bestaat.

#### <a name="example"></a>Voorbeeld
```sql
DROP VIEW IF EXISTS dbo.TestView;
```
```
Message
--------------------------------------------------
Commands completed successfully.

```

## <a name="improved-compilation-time-for-singleton-inserts-and-ddl-statements"></a>Verbeterde compilatietijd voor Singleton ingevoegd en DDL-componenten 
Een traditionele model van extraheren, transformeren en laden (ETL) te volgen voor gegevens invoegen vaak leidt tot het uitvoeren van een singleton-invoegen ([INSERT-waarden](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql)) in een tabel in de database. Deze release verbetert de prestaties van singleton-insert-bewerkingen door te verminderen de compilatietijd die nodig is voor het uitvoeren van dit type instructie. In sommige gevallen kan de waargenomen compilatie verbetering is maximaal 3 x sneller. Deze verbetering verlaagt de tijd die nodig is een singleton-insert-instructie uit te voeren. 

De gebruikerservaring ook voordelen data warehouses met een groot aantal objecten door op dezelfde manier de Querytijd compilatie voor overzichten van Data Definition Language (DDL) met inbegrip van maken, wijzigen en verwijderen van bewerkingen. 

Ten slotte vermindert het verbeteren de algehele uitvoering van de instructies die worden uitgevoerd via brede tabellen - tabellen die een groot aantal kolommen hebben. De gebruikerservaring is een vermindering van de tijd in de query-compilatiestap waardoor de totale uitvoeringstijd voor query's.

## <a name="bug-fixes"></a>Opgeloste fouten

| Titel | Description |
|:---|:---|
| **Bij het maken van statistieken op distributies voor unieke beperkingen oplossen** | Deze oplossing wordt een fout die gebruikers ondervinden bij het actieve UPDATE STATISTICS met alleen de tabel wordt opgegeven, als de tabel had een unique-beperking gedefinieerd. |
| **Probleem bij het compileren van query's over externe tabellen** | Deze oplossing wordt een defect die beïnvloed compilatietijd voor query's met betrekking tot externe tabellen.|
| **Bij het uitvoeren van een instructie met grote typen oplossen** | Adres van een fout in de voorbereide instructie compileren met parameters die zijn gedeclareerd als een van de *grote* typen (nvarchar(max), varchar(max) en varbinary(max)). |
| **Wanneer er een fout optreedt voor diep geneste query's oplossen** | Biedt een duidelijke foutbericht weergegeven wanneer een diep geneste query groter is dan systeemlimieten.|
| **Voor compilatie fouten oplossen wanneer een instructie een gecorreleerde subquery en een constante uitvoeringstijd bevat** |Adressen compilatiefout voor query's met een combinatie van gecorreleerde subquery's en -uitvoeringstijd constanten (zoals GETDATE()).|
| **Time-out voor het verkrijgen van PDW-object wordt vergrendeld en gelijktijdigheid van taken sleuf voor autostats adres** |Time-out van de vergrendeling om te voorkomen dat autostats aanvragen de oorspronkelijke aanvragen blokkeert gedurende een lange periode toevoegen oplossing|

## <a name="next-steps"></a>Volgende stappen
Nu u een en ander weet over SQL Data Warehouse, kunt u leren hoe u snel [maken van een SQL Data Warehouse][create a SQL Data Warehouse]. Als u niet bekend bent met Azure, kan de [Azure-woordenlijst][Azure glossary] handig zijn bij het opzoeken van nieuwe terminologie. U kunt ook enkele andere SQL Data Warehouse-resources bekijken.  

* [Succesverhalen van klanten]
* [Blogs]
* [Functieverzoeken]
* [Video's]
* [Teamblogs met adviezen voor klanten]
* [Stack Overflow-forum]
* [Twitter]


[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Teamblogs met adviezen voor klanten]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Succesverhalen van klanten]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Functieverzoeken]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
