---
title: MPP-architectuur - Azure SQL Data Warehouse? | Microsoft Docs
description: Meer informatie over hoe Azure SQL Data Warehouse combineert massively parallelle processing (MPP) met Azure storage voor hoge prestaties en schaalbaarheid.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: architecture
ms.date: 11/15/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 4c230eb0633b2917b90a5c1f9f4176882bfd0290
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="azure-sql-data-warehouse---massively-parallel-processing-mpp-architecture"></a>Azure SQL datawarehouse - Massively parallelle verwerking van de architectuur (MPP)
Meer informatie over hoe Azure SQL Data Warehouse combineert massively parallelle processing (MPP) met Azure storage voor hoge prestaties en schaalbaarheid. 

## <a name="mpp-architecture-components"></a>Onderdelen van de MPP-architectuur
SQL Data Warehouse maakt gebruik van een scale-out-architectuur voor het distribueren van rekenkundige verwerking van gegevens op meerdere knooppunten. De eenheid van de schaal is een abstractie van rekenkracht waarvan bekend is als eenheid datawarehouse. SQL Data Warehouse gescheiden compute uit de opslag zodat u als de gebruiker om te schalen onafhankelijk van de gegevens in uw systeem berekenen.

![Architectuur van SQL Data Warehouse](media/massively-parallel-processing-mpp-architecture/massively-parallel-processing-mpp-architecture.png)

SQL Data Warehouse maakt gebruik van een architectuur op basis van knooppunt. Toepassingen verbinding maken en uitgeven van T-SQL-opdrachten voor een beheerknooppunt, het één punt van de vermelding voor het datawarehouse is. Het beheerknooppunt wordt de MPP-engine optimaliseert de query's voor parallelle verwerking en geeft vervolgens bewerkingen aan rekenknooppunten voor hun werk parallel uitgevoerd. De Compute nodes alle gebruikersgegevens worden opgeslagen in Azure Storage opslaat en de parallelle query's uitvoeren. Data Movement Service (DMS) is een interne systeemniveau-service waarmee gegevens op de knooppunten die nodig zijn verplaatst voor query's parallel worden uitgevoerd en nauwkeurige resultaten retourneren. 

Als u opslag en rekenactiviteiten loskoppelt van elkaar, kan SQL Data Warehouse:

* Onafhankelijk grootte rekenkracht ongeacht uw opslagbehoeften.
* De rekencapaciteit vergroten of verkleinen zonder gegevens te verplaatsen.
* Rekencapaciteit onderbreken gegevens blijven behouden, zodat u betaalt alleen voor opslag.
* De rekencapaciteit hervatten tijdens werktijden.

### <a name="azure-storage"></a>Azure-opslag
SQL Data Warehouse maakt gebruik van Azure-opslag om uw gebruikersgegevens te beschermen.  Aangezien uw gegevens worden opgeslagen en beheerd door de Azure-opslag, brengt SQL Data Warehouse afzonderlijk voor uw opslagverbruik. De gegevens zelf is shard in **distributies** om de prestaties van het systeem te optimaliseren. U kunt kiezen welke sharding-patroon gebruiken voor het distribueren van de gegevens bij het definiëren van de tabel. SQL Data Warehouse ondersteunt deze patronen sharding:

* Hash
* Round robin
* Repliceren

### <a name="control-node"></a>Door het beheerknooppunt

Het beheerknooppunt is de brain van het datawarehouse. Het is de front-end met interactie met alle toepassingen en verbindingen. De MPP-engine wordt uitgevoerd op het knooppunt controle te optimaliseren en coördineren van parallelle query's. Wanneer u een T-SQL-query naar SQL Data Warehouse verzendt, wordt het in het beheerknooppunt getransformeerd tot query's die voor elke distributie parallel uitgevoerd.

### <a name="compute-nodes"></a>Rekenknooppunten

De Compute nodes bieden de verwerkingskracht. Distributies worden toegewezen aan rekenknooppunten voor verwerking. Als u voor meer computerresources betaalt, wordt in SQL Data Warehouse opnieuw de distributies over de beschikbare Compute nodes toegewezen. Het aantal knooppunten kan variëren van 1 tot 60 berekenen en wordt bepaald door het serviceniveau voor het datawarehouse.

Elk rekenknooppunt heeft een knooppunt-ID die is zichtbaar in systeemweergaven. U ziet de Compute-knooppunt-ID voor de kolom node_id in systeemweergaven waarvan de naam met sys.pdw_nodes begint. Zie voor een lijst van deze systeemweergaven [MPP systeemweergaven](sql-data-warehouse-reference-tsql-statements.md).

### <a name="data-movement-service"></a>Data Movement Service
Data Movement Service (DMS) is de technologie voor het transport van gegevens die coördineert de verplaatsing van gegevens tussen de rekenknooppunten. Sommige query's is de verplaatsing van gegevens om te controleren of de parallelle query's nauwkeurige resultaten retourneren. Wanneer de verplaatsing van gegevens is vereist, DMS zorgt ervoor dat de juiste gegevens naar de juiste locatie opgehaald. 

## <a name="distributions"></a>Distributies

Een distributiepunt is de basiseenheid voor opslag en verwerking voor parallelle query's dat wordt uitgevoerd op gedistribueerde gegevens. Wanneer SQL Data Warehouse wordt een query uitgevoerd, is het werk verdeeld in 60 kleinere query's die parallel worden uitgevoerd. Elk van de 60 kleinere query's wordt uitgevoerd op een van de gegevens-distributies. Elk rekenknooppunt beheert een of meer van de 60 distributies. Een datawarehouse met maximale computerresources heeft één verdeling per rekenknooppunt. Een datawarehouse met minimale computerresources heeft alle distributies op één rekenknooppunt.  

## <a name="hash-distributed-tables"></a>Gedistribueerd hash tabellen
Een gedistribueerde hash-tabel kunt u de beste queryprestaties voor samenvoegingen en aggregaties leveren op grote tabellen. 

SQL Data Warehouse gebruikt een hash-functie shard-gegevens in een tabel met hash is gedistribueerd, deterministische elke rij toewijzen aan een distributiepunt. In de tabeldefinitie, is een van de kolommen aangewezen als de distributie-kolom. De hash-functie maakt gebruik van de waarden in de kolom distributie elke rij toewijzen aan een distributiepunt.

Het volgende diagram illustreert hoe een volledige (tabel niet gedistribueerd) wordt opgeslagen als een tabel met hash gedistribueerd. 

![Gedistribueerde tabel](media/sql-data-warehouse-distributed-data/hash-distributed-table.png "gedistribueerde tabel")  

* Elke rij hoort bij een distributiepunt.  
* Elke rij een deterministische hash-algoritme toegewezen aan een distributiepunt.  
* Het aantal rijen per distributiepunt verschilt zoals aangegeven door de verschillende grootten van tabellen.

Er zijn prestatie-overwegingen voor de selectie van een kolom van de distributie, zoals onderscheidbaarheid tijdverschil gegevens en de typen query's die worden uitgevoerd op het systeem.

## <a name="round-robin-distributed-tables"></a>Round robin gedistribueerde tabellen
Een round robin-tabel is de eenvoudigste tabel te maken en biedt een snelle prestaties als gebruikt als een faseringstabel voor belastingen.

Een gedistribueerde tabel round robin verdeelt gelijkmatig gegevens in de tabel, maar zonder eventuele verdere optimalisatie. Een distributiepunt wordt eerst willekeurig geselecteerd en klik vervolgens buffers rijen zijn toegewezen aan distributies sequentieel worden verwerkt. Het is snel gegevens laden in een tabel round robin, maar queryprestaties kan vaak beter met gedistribueerd hash-tabellen zijn. Joins in round robin tabellen vereisen reshuffling gegevens en dit kost meer tijd.


## <a name="replicated-tables"></a>Gerepliceerde tabellen
Een gerepliceerde tabel bevat de snelste queryprestaties voor kleine tabellen.

Een tabel die is gerepliceerd in de cache opslaat voor een volledige kopie van de tabel op elk rekenknooppunt. Als gevolg daarvan kan verwijdert repliceren van een tabel de behoefte om over te dragen van gegevens tussen rekenknooppunten voordat een join of aggregatie. Gerepliceerde tabellen het beste reguleren met kleine tabellen. Er is extra opslag vereist en er zijn extra overhead die zijn gemaakt bij het schrijven van gegevens die grote niet praktisch tabellen.  

Het volgende diagram toont een gerepliceerde tabel. Voor SQL Data Warehouse, gerepliceerde tabel is in de cache op de eerste distributie op elk rekenknooppunt.  

![Gerepliceerde tabel](media/sql-data-warehouse-distributed-data/replicated-table.png "gerepliceerde tabel") 

## <a name="next-steps"></a>Volgende stappen
Nu u een en ander weet over SQL Data Warehouse, kunt u leren hoe u snel [een SQL Data Warehouse maakt][create a SQL Data Warehouse] en [voorbeeldgegevens laadt][load sample data]. Als u niet bekend bent met Azure, kan de [Azure-woordenlijst][Azure glossary] handig zijn bij het opzoeken van nieuwe terminologie. U kunt ook enkele andere SQL Data Warehouse-resources bekijken.  

* [Succesverhalen van klanten]
* [Blogs]
* [Functieverzoeken]
* [Video's]
* [Teamblogs met adviezen voor klanten]
* [Ondersteuningsticket maken]
* [MSDN-forum]
* [Stack Overflow-forum]
* [Twitter]

<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Ondersteuningsticket maken]: ./sql-data-warehouse-get-started-create-support-ticket.md
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Succesverhalen van klanten]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Teamblogs met adviezen voor klanten]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Functieverzoeken]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-forum]: https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureSQLDataWarehouse
[Stack Overflow-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreements]: https://azure.microsoft.com/support/legal/sla/
