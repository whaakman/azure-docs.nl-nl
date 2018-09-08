---
title: Azure SQL datawarehouse - MPP-architectuur | Microsoft Docs
description: Meer informatie over hoe Azure SQL Data Warehouse combineert met massively parallelle verwerking (MPP) met Azure-opslag om hoge prestaties en schaalbaarheid te realiseren.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 1fa31b23aa8df73b13e73da80096596bf1ce2db3
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44093293"
---
# <a name="azure-sql-data-warehouse---massively-parallel-processing-mpp-architecture"></a>Azure SQL datawarehouse - krachtige parallelle verwerking (MPP)-architectuur
Meer informatie over hoe Azure SQL Data Warehouse combineert met massively parallelle verwerking (MPP) met Azure-opslag om hoge prestaties en schaalbaarheid te realiseren. 

> [!VIDEO https://www.youtube.com/embed/PlyQ8yOb8kc]

## <a name="mpp-architecture-components"></a>Onderdelen van de MPP-architectuur
SQL Data Warehouse maakt gebruik van een uitbreidbare architectuur voor de distributie van rekenkundige verwerking van gegevens over meerdere knooppunten. De eenheid van de schaal is een abstractie van de compute-kracht die bekend als een eenheid van datawarehouse staat. SQL Data Warehouse scheidt compute uit de opslag zodat waar u om te schalen in uw systeem onafhankelijk van de gegevens COMPUTE.

![Architectuur van SQL Data Warehouse](media/massively-parallel-processing-mpp-architecture/massively-parallel-processing-mpp-architecture.png)

SQL Data Warehouse maakt gebruik van een architectuur op basis van een knooppunt. Toepassingen verbinding maken en uitgeven van T-SQL-opdrachten op een beheerknooppunt, het één punt van de vermelding voor het datawarehouse is. Het beheerknooppunt wordt de MPP-engine die optimaliseert query's voor parallelle verwerking en geeft vervolgens bewerkingen voor Compute-knooppunten voor hun werk parallel uitgevoerd. De rekenknooppunten alle gebruikersgegevens worden opgeslagen in Azure Storage opslaat en de parallelle query's uitvoeren. Data Movement Service (DMS) is een interne op systeemniveau-service die gegevens naar de verschillende knooppunten die nodig is verplaatst voor query's parallel uitvoeren en nauwkeurige resultaten worden geretourneerd. 

Als u opslag en rekenactiviteiten loskoppelt van elkaar, kan SQL Data Warehouse:

* Onafhankelijk van elkaar grootte compute-kracht, ongeacht uw opslagbehoeften.
* De rekencapaciteit vergroten of verkleinen zonder gegevens te verplaatsen.
* De rekencapaciteit onderbreken terwijl gegevens intact, zodat u alleen betaalt voor opslag.
* De rekencapaciteit hervatten tijdens werktijden.

### <a name="azure-storage"></a>Azure Storage
SQL Data Warehouse maakt gebruik van Azure storage om uw gebruikersgegevens te beschermen.  Nadat uw gegevens worden opgeslagen en beheerd door Azure storage, SQL Data Warehouse in rekening gebracht afzonderlijk voor gebruik van de opslag. De gegevens zelf is in een shard **distributies** om de prestaties van het systeem te optimaliseren. U kunt kiezen welke sharding-patroon gebruiken voor het distribueren van de gegevens wanneer u de tabel definieert. SQL Data Warehouse biedt ondersteuning voor deze sharding-patronen voor:

* Hash
* Round robin
* Repliceren

### <a name="control-node"></a>Beheerknooppunt

Het beheerknooppunt is het brein van het datawarehouse. Het is de front-end met interactie met alle toepassingen en verbindingen. De MPP-engine wordt uitgevoerd op het beheerknooppunt te optimaliseren en coördinatie van parallelle query's. Wanneer u een T-SQL-query naar SQL Data Warehouse verzendt, wordt het in het beheerknooppunt getransformeerd tot query's die voor elke distributie parallel uitgevoerd.

### <a name="compute-nodes"></a>Rekenknooppunten

De Compute-knooppunten bieden de rekenkracht. Distributies worden toegewezen aan rekenknooppunten voor verwerking. Als u voor meer rekenresources betaalt, wijst SQL Data Warehouse opnieuw de distributies naar de beschikbare Compute-knooppunten. Het aantal knooppunten kan variëren van 1 tot 60 compute en wordt bepaald door het serviceniveau voor het datawarehouse.

Elk knooppunt heeft een knooppunt-ID die wordt weergegeven in systeemweergaven. U ziet de Compute-knooppunt-ID voor de kolom $node_id in systeemweergaven waarvan de namen met sys.pdw_nodes beginnen. Zie voor een lijst van deze systeemweergaven, [MPP systeemweergaven](sql-data-warehouse-reference-tsql-statements.md).

### <a name="data-movement-service"></a>Data Movement Service
Data Movement Service (DMS) is de technologie voor het transport van gegevens die coördineert de verplaatsing van gegevens tussen de rekenknooppunten. Sommige query's is de verplaatsing van gegevens om te controleren of de parallelle query's retourneren nauwkeurige resultaten. Bij het verplaatsen van gegevens is vereist, DMS zorgt ervoor dat de juiste gegevens naar de juiste locatie opgehaald. 

## <a name="distributions"></a>Distributies

Een distributiepunt is de basiseenheid voor opslag en verwerking voor parallelle query's die wordt uitgevoerd op gedistribueerde gegevens. Wanneer een query wordt uitgevoerd in SQL Data Warehouse, wordt het werk is onderverdeeld in 60 kleinere query's die parallel worden uitgevoerd. Elk van de 60 kleinere query's wordt uitgevoerd op een van de gegevens-distributies. Elk rekenknooppunt beheert een of meer van de 60 distributies. Een datawarehouse met maximale rekenresources heeft één distributie per knooppunt. Een datawarehouse met minimale rekenresources heeft alle distributies op een rekenknooppunt.  

## <a name="hash-distributed-tables"></a>Hash-gedistribueerde tabellen
Een tabel kan de hoogste prestaties van query's voor samenvoegingen en aggregaties op grote tabellen leveren. 

Gegevens in een gedistribueerde hash-tabel gebruikt SQL Data Warehouse een hash-functie deterministische wijze elke rij toewijzen aan een distributiepunt. In het tabeldefinitie van de is een van de kolommen ingesteld als de distributiekolom. De hash-functie maakt gebruik van de waarden in de distributiekolom elke rij toewijzen aan een distributiepunt.

Het volgende diagram illustreert hoe een volledige (niet-gedistribueerde tabel) wordt opgeslagen als een tabel met hash worden verdeeld. 

![Gedistribueerde tabel](media/sql-data-warehouse-distributed-data/hash-distributed-table.png "gedistribueerde tabel")  

* Elke rij behoort tot één distributiepunt.  
* Elke rij een deterministische hash-algoritme toegewezen aan een distributiepunt.  
* Het aantal rijen per distributie varieert zoals aangegeven in de verschillende formaten van tabellen.

Er zijn prestatie-overwegingen voor de selectie van een distributiekolom, zoals onderscheidbaarheid gegevensverschil en de typen query's die worden uitgevoerd op het systeem.

## <a name="round-robin-distributed-tables"></a>Round robin gedistribueerde tabellen
Een round robin-tabel is de eenvoudigste tabel te maken en biedt een snelle prestaties bij gebruik als een tijdelijke tabel bij belastingen.

Een distributietabel round robin distribueert gegevens gelijkmatig in de tabel, maar zonder eventuele verdere optimalisatie. Een distributiepunt wordt eerst willekeurig geselecteerd en klik vervolgens buffers van rijen zijn toegewezen aan distributies sequentieel worden verwerkt. Het is snel gegevens laden in een round robin-tabel, maar de prestaties van query's voor het vaak beter met gedistribueerde hashtabellen kan worden. Joins op round robin-tabellen vereisen reshuffling gegevens en dit meer tijd in beslag.


## <a name="replicated-tables"></a>Gerepliceerde tabellen
Een gerepliceerde tabel bevat de snelste prestaties van query's voor kleine tabellen.

Een tabel die is gerepliceerd in de cache opgeslagen voor een volledige kopie van de tabel op elk knooppunt. Als gevolg daarvan kunnen repliceren van een tabel Hiermee verwijdert u de noodzaak om over te dragen van gegevens tussen rekenknooppunten voordat een join of aggregatie. Gerepliceerde tabellen het beste met kleine tabellen reguleren. Extra opslag is vereist en er zijn extra overhead die zijn gemaakt bij het schrijven van gegevens waarmee u grote tabellen niet praktisch.  

Het volgende diagram toont een gerepliceerde tabel. Voor SQL Data Warehouse de gerepliceerde tabel is in de cache op de eerste distributie op elk knooppunt.  

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
