---
title: Wat is Azure SQL Data Warehouse? | Microsoft Docs
description: Gedistribueerde database op ondernemingsniveau die geschikt is voor het verwerken van petabytes aan relationele en niet-relationele gegevens. Het is het eerste geavanceerde clouddatawarehouse in de branche dat in enkele seconden kan worden vergroot, verkleind en onderbroken.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: bjhubbard
editor: 
ms.assetid: 4006c201-ec71-4982-b8ba-24bba879d7bb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: overview
ms.date: 2/28/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 575c49f83c8845edcea984459f3907490c62d269
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-azure-sql-data-warehouse"></a>Wat is Azure SQL Data Warehouse?
Azure SQL Data Warehouse is een schaalbare, relationele MMP-clouddatabase (Massively Parallel Processing) die geschikt is voor het verwerken van grote hoeveelheden gegevens. 

SQL Data Warehouse:

* Combineert de relationele SQL Server-database met schaalbare Azure-cloudfuncties. 
* Maakt het mogelijk opslag los te koppelen van rekenactiviteiten.
* Maakt het uitbreiden, beperken, onderbreken of hervatten van rekenactiviteiten mogelijk. 
* Is in het gehele Azure-platform te integreren.
* Maakt gebruik van SQL Server Transact-SQL (T-SQL) en hulpprogramma's.
* Voldoet aan diverse juridische en zakelijke beveiligingsvereisten, zoals SOC en ISO.

In dit artikel worden de belangrijkste functies van SQL Data Warehouse beschreven.

## <a name="massively-parallel-processing-architecture"></a>MPP-architectuur (Massively Parallel Processing)
SQL Data Warehouse is een gedistribueerd MPP-databasesysteem (Massively Parallel Processing). Achter de schermen verspreidt SQL Data Warehouse uw gegevens over veel opslag- en verwerkingseenheden. De gegevens worden opgeslagen in een Premium laag voor lokaal redundante opslag. Hierbovenop voeren dynamisch gekoppelde rekenknooppunten query's uit. SQL Data Warehouse hanteert een verdeel-en-heersbenadering met betrekking tot laadtaken en complexe query's. Aanvragen worden ontvangen door een beheerknooppunt, geoptimaliseerd voor distributie, en vervolgens doorgegeven aan de rekenknooppunten, zodat ze hun werk parallel kunnen uitvoeren.

Als u opslag en rekenactiviteiten loskoppelt van elkaar, kan SQL Data Warehouse:

* De beschikbare opslag vergroten of verkleinen, onafhankelijk van de rekencapaciteit.
* De rekencapaciteit vergroten of verkleinen zonder gegevens te verplaatsen.
* De rekencapaciteit onderbreken terwijl gegevens intact blijven en u alleen betaalt voor opslag.
* De rekencapaciteit hervatten tijdens werktijden.

In het volgende diagram wordt de architectuur gedetailleerder weergegeven.

![Architectuur van SQL Data Warehouse][1]

**Beheerknooppunt:** het beheerknooppunt beheert en optimaliseert query's. Het is de front-end met interactie met alle toepassingen en verbindingen. In SQL Data Warehouse werkt het beheerknooppunt op basis van SQL Database en het maken van een verbinding gaat ongeveer hetzelfde. Op de achtergrond coördineert het beheerknooppunt alle benodigde verplaatsingen van gegevens en berekeningen voor het uitvoeren van parallelle query's op uw gedistribueerde gegevens. Wanneer u een TSQL-query naar SQL Data Warehouse verzendt, wordt deze door het beheerknooppunt getransformeerd tot afzonderlijke query's die parallel worden uitgevoerd op de afzonderlijke rekenknooppunten.

**Rekenknooppunten:** de rekenknooppunten vormen de kracht achter SQL Data Warehouse. Het zijn SQL Databases die uw gegevens opslaan en uw query verwerken. Wanneer u gegevens toevoegt, worden de rijen door SQL Data Warehouse gedistribueerd naar uw rekenknooppunten. De rekenknooppunten zijn de workers waarop de parallelle query's op uw gegevens worden uitgevoerd. Na de verwerking worden de resultaten teruggestuurd naar het beheerknooppunt. Om de query te voltooien, worden de resultaten door het beheerknooppunt geaggregeerd en het definitieve resultaat geretourneerd.

**Opslag:** uw gegevens worden opgeslagen in Azure-blobopslag. Bij interacties tussen de rekenknooppunten en uw gegevens worden de gegevens rechtstreeks vanuit de blob-opslag gelezen of ernaar weggeschreven. Omdat opslag in Azure transparant en enorm kan worden uitgebreid, kan dat ook in SQL Data Warehouse. Omdat reken- en opslagcapaciteit niet van elkaar afhankelijk zijn, kan de opslagcapaciteit in SQL Data Warehouse automatisch los van de rekencapaciteit worden geschaald en omgekeerd. Azure-blobopslag is ook volledig fouttolerant. Dit vergemakkelijkt het back-up- en herstelproces.

**Data Movement Service:** Data Movement Service (DMS) verplaatst gegevens van het ene naar het andere knooppunt. DMS geeft de rekenknooppunten toegang tot de benodigde gegevens voor samenvoegingen en aggregaties. DMS is geen Azure-service. Het is een Windows-service die naast SQL Database wordt uitgevoerd op alle knooppunten. DMS is een achtergrondproces waarmee u niet rechtstreeks aan de slag kunt. U kunt echter queryplannen bekijken om na te gaan wanneer er DMS-bewerkingen plaatsvinden. Het is namelijk nodig om gegevens te verplaatsen om query's parallel te kunnen uitvoeren.

## <a name="optimized-for-data-warehouse-workloads"></a>Geoptimaliseerd voor datawarehouse-workloads
De MPP-methode wordt ondersteund door een aantal specifieke optimalisaties voor datawarehouseprestaties, waaronder:

* een gedistribueerde queryoptimalisatie en een set complexe statistieken voor alle gegevens. Met behulp van informatie over de omvang en distributie van gegevens kunnen query's in de service worden geoptimaliseerd door de kosten van specifieke gedistribueerde querybewerkingen te berekenen.
* In het gegevensverplaatsingsproces zijn geavanceerde algoritmen en technieken geïntegreerd om de gegevens efficiënt te verplaatsen tussen de computerbronnen die nodig zijn voor het uitvoeren van de query. Deze gegevensverplaatsingsbewerkingen zijn ingebouwd en alle optimalisaties voor Data Movement Service vinden automatisch plaats.
* Er wordt standaard gebruikgemaakt van geclusterde **kolomopslag**indexen. Dankzij kolomopslag is de compressie in SQL Data Warehouse gemiddeld vijf keer beter dan bij traditionele rij-opslag en zijn de prestaties tot wel tien of meer keer beter. De prestaties van analysequery's waarbij een groot aantal rijen moeten worden gescand, zijn beter bij gebruik van kolomopslagindexen.


## <a name="predictable-and-scalable-performance-with-data-warehouse-units"></a>Voorspelbare en schaalbare prestaties met Data Warehouse Units
SQL Data Warehouse is opgebouwd op basis van vergelijkbare technologieën als die van SQL Database, wat betekent dat gebruikers consistente en voorspelbare prestaties kunnen verwachten voor analytische query's. Gebruikers kunnen verwachten dat de prestaties lineair worden geschaald wanneer er rekenknooppunten worden toegevoegd of verwijderd. Toewijzing van resources aan uw SQL Data Warehouse wordt gemeten in DWU's (Data Warehouse Units). DWU's zijn een maatstaf van de onderliggende resources, zoals CPU, geheugen en IOP's, die worden toegewezen aan de SQL Data Warehouse. Hoe meer DWU's, hoe meer resources en hoe beter de prestaties. DWU's zorgen met name voor het volgende:

* U kunt uw datawarehouse schalen zonder dat u zich zorgen hoeft te maken over de onderliggende hardware of software.
* U kunt prestatieverbetering voor een DWU-niveau voorspellen voordat u de rekencapaciteit van het datawarehouse wijzigt.
* De onderliggende hardware en software van uw exemplaar kunnen worden gewijzigd of verplaatst zonder negatieve gevolgen voor de workload.
* Microsoft kan de onderliggende architectuur van de service verbeteren zonder de prestaties van uw workload te beïnvloeden.
* Microsoft kan de prestaties van SQL Data Warehouse snel verbeteren op een manier waarop de workload schaalbaar is en evenredig wordt verdeeld over het systeem.

Data Warehouse Units (DWU's) bieden een maatstaf voor drie metrische gegevens die nauw verband houden met de prestaties van datawarehouses bij een bepaalde workload. De volgende belangrijke metrische gegevens over workloads worden lineair geschaald met de DWU's.

**Scan/aggregatie:** een standaarddatawarehousequery waarmee een groot aantal rijen wordt gescand waarop vervolgens een complexe aggregatie wordt uitgevoerd. Dit is een I/O-bewerking waarbij de CPU intensief wordt belast.

**Belasting:** de capaciteit voor het opnemen van gegevens in de service. Laadacties worden het beste uitgevoerd met PolyBase van Azure Storage Blobs of Azure Data Lake. Deze metriek fungeert als stresstest voor de netwerk- en CPU-aspecten van de service.

**Create Table As Select (CTAS):** met CTAS wordt de capaciteit voor het kopiëren van een tabel gemeten. Dit omvat het lezen van gegevens uit de opslag, het verdelen van de gegevens over de knooppunten van het systeem en het weer terugschrijven van de gegevens naar de opslag. Dit is een bewerking waarbij de CPU, de IO en het netwerk intensief worden belast.

## <a name="built-on-sql-server"></a>Gebouwd op SQL Server
SQL Data Warehouse is gebaseerd op de relationele database-engine van de SQL Server en bevat veel functies die u van een datawarehouse voor ondernemingen verwacht. Als u al bekend bent met T-SQL, zult u weinig moeite hebben met de overgang naar SQL Data Warehouse. Of u nu een ervaren of beginnende gebruiker bent, met de voorbeelden in de documentatie kunt u snel aan de slag. Over het algemeen kunt u nadenken over de manier waarop de elementen van de taal van SQL Data Warehouse zijn opgebouwd:

* SQL Data Warehouse gebruikt T-SQL-syntaxis voor veel bewerkingen. Deze biedt ook ondersteuning voor diverse traditionele SQL-constructs, zoals opgeslagen procedures, door de gebruiker gedefinieerde functies, tabelpartities, indexen en sorteringen.
* SQL Data Warehouse bevat ook een aantal nieuwere functies van SQL Server, zoals geclusterde **kolomopslag**indexen, PolyBase-integratie en gegevensauditing (inclusief beoordeling van bedreigingen).
* Bepaalde T-SQL-taalelementen die minder vaak voorkomen in werkbelastingen voor datawarehouses, of die nieuwer zijn voor SQL Server, zijn mogelijk niet beschikbaar. Voor meer informatie raadpleegt u de [migratiedocumentatie][Migration documentation].

Doordat SQL Server, SQL Data Warehouse, SQL Database en Analytics Platform System de Transact-SQL en diverse functies met elkaar gemeen hebben, kunt u een passende oplossing ontwikkelen voor uw gegevensbehoeften. U kunt besluiten waar u uw gegevens wilt bewaren, op basis van de gewenste prestaties, beveiliging en schaal, en uw gegevens vervolgens van het ene naar het andere systeem verplaatsen.

## <a name="data-protection"></a>Gegevensbeveiliging
Alle SQL Data Warehouse-gegevens worden opgeslagen in Azure Premium-opslag met lokaal redundante opslag. In het lokale datacentrum worden meerdere synchrone kopieën van de gegevens bewaard voor gegarandeerd transparante gegevensbeveiliging tegen gelokaliseerde fouten. Bovendien maakt SQL Data Warehouse automatisch en regelmatig back-ups van uw actieve (niet-onderbroken) databases met Azure Storage-momentopnamen. Zie voor meer informatie over hoe Back-ups en herstellen werkt, het [Overzicht van Back-ups en herstellen][Backup and restore overview].

## <a name="integrated-with-microsoft-tools"></a>Geïntegreerd met Microsoft-hulpprogramma's
SQL Data Warehouse integreert ook veel van de hulpmiddelen waar gebruikers van SQL Server mee bekend zijn. Tot deze hulpmiddelen behoren onder meer:

**Traditionele SQL Server-hulpprogramma's:** SQL Data Warehouse is volledig geïntegreerd met SQL Server Analysis Services, Integration Services en Reporting Services.

**Cloudhulpprogramma's:** SQL Data Warehouse kan worden geïntegreerd in verschillende services in Azure, waaronder Data Factory, Stream Analytics, Machine Learning en Power BI. Voor een volledig overzicht raadpleegt u [Overzicht met geïntegreerde hulpmiddelen][Integrated tools overview].

**Hulpprogramma's van derden:** veel externe leveranciers hebben de integratie van hun hulpprogramma's met SQL Data Warehouse laten certificeren. Zie voor een volledige lijst [SQL Data Warehouse-oplossingspartners][SQL Data Warehouse solution partners].

## <a name="hybrid-data-sources-scenarios"></a>Hybride scenario's met gegevensbronnen
PolyBase stelt u in staat gebruik te maken van verschillende gegevensbronnen met vertrouwde TSQL-opdrachten. Met PolyBase kunt u niet-relationele gegevens opvragen uit de Azure-blobopslag, net zoals u gegevens kunt opvragen uit gewone tabellen. Met PolyBase kunt u ook niet-relationele gegevens opvragen of niet-relationele gegevens importeren in SQL Data Warehouse.

* In PolyBase worden externe tabellen gebruikt voor toegang tot niet-relationele gegevens. De tabeldefinities worden opgeslagen in SQL Data Warehouse en zijn toegankelijk via SQL en hulpprogramma's, net als bij normale relationele gegevens.
* Polybase is agnostisch qua integratie. De functies en functionaliteit ervan zijn hetzelfde voor alle bronnen die door Polybase worden ondersteund. In Polybase kunnen gegevens in diverse indelingen worden gelezen, waaronder bestanden met scheidingstekens en ORC-bestanden.
* PolyBase kan worden gebruikt voor toegang tot Blob Storage die ook wordt gebruikt als opslag voor een HDInsight-cluster. Hierdoor hebt u toegang tot dezelfde gegevens met relationele en niet-relationele hulpprogramma's.

## <a name="sla"></a>SLA
SQL Data Warehouse biedt een SLA (Service Level Agreement, serviceovereenkomst) op productniveau als onderdeel van de Microsoft Online Services SLA. Ga voor meer informatie naar [SLA voor SQL Data Warehouse][SLA for SQL Data Warehouse]. Ga voor SLA-informatie over alle andere producten naar de Azure-pagina [Serviceovereenkomsten] download deze via de pagina [Volumelicenties][Volume Licensing]. 

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
[MSDN-forum]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Stack Overflow-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Serviceovereenkomsten]: https://azure.microsoft.com/en-us/support/legal/sla/
