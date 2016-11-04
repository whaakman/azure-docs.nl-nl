---
title: Wat is Azure SQL Data Warehouse? | Microsoft Docs
description: Gedistribueerde database op ondernemingsniveau die geschikt is voor het verwerken van petabytes aan relationele en niet-relationele gegevens. Het is het eerste geavanceerde clouddatawarehouse in de branche dat in enkele seconden kan worden vergroot, verkleind en onderbroken.
services: sql-data-warehouse
documentationcenter: NA
author: lodipalm
manager: barbkess
editor: ''

ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 09/27/2016
ms.author: lodipalm;barbkess;mausher;jrj;sonyama;kevin

---
# Wat is Azure SQL Data Warehouse?
Azure SQL Data Warehouse is een schaalbare clouddatabase die geschikt is voor het verwerken van grote hoeveelheden relationele en/of niet-relationele gegevens. Dankzij de MPP-architectuur (Massively Parallelle Processing) is SQL Data Warehouse berekend op de workload van elke onderneming.

SQL Data Warehouse:

* Combineert de relationele SQL Server-database met schaalbare Azure-cloudfuncties. U kunt de rekencapaciteit vergroten, verkleinen, onderbreken of hervatten in enkele seconden. U bespaart kosten door de CPU-capaciteit te vergroten of te verkleinen tijdens piek- of daluren.
* Maakt gebruik van het Azure-platform. Het is eenvoudig te implementeren, wordt naadloos onderhouden en is volledig fouttolerant door automatische back-ups.
* Vormt een aanvulling op de SQL Server-omgeving. Het is te ontwikkelen met de vertrouwde SQL Server Transact-SQL (T-SQL) en hulpprogramma's van SQL Server.

In dit artikel worden de belangrijkste functies van SQL Data Warehouse beschreven.

## MPP-architectuur (Massively Parallel Processing)
SQL Data Warehouse is een gedistribueerd MPP-databasesysteem (Massively Parallel Processing). Door gegevens- en processorcapaciteit over meerdere knooppunten te verdelen, kan SQL Data Warehouse grote schaalbaarheid bieden, veel groter dan enkel ander systeem.  Achter de schermen verspreidt SQL Data Warehouse uw gegevens over veel opslag- en verwerkingseenheden. De gegevens worden opgeslagen in lokaal redundante Premium-opslag en gekoppeld aan rekenknooppunten voor het uitvoeren van query's. Met deze architectuur heeft SQL Data Warehouse een verdeel-en-heersbenadering met betrekking tot laadtaken en complexe query's. Aanvragen worden ontvangen door het beheerknooppunt, geoptimaliseerd en vervolgens doorgegeven aan de rekenknooppunten om hun werk parallel uit te voeren.

Door MPP-architectuur te combineren met de opslagmogelijkheden van Azure, kunt u in SQL Data Warehouse:

* De opslag vergroten of verkleinen, onafhankelijk van de rekencapaciteit.
* De rekencapaciteit vergroten of verkleinen zonder gegevens te verplaatsen.
* De rekencapaciteit onderbreken zonder gegevensverlies.
* De rekencapaciteit op elk gewenst moment hervatten.

In het volgende diagram wordt de architectuur gedetailleerder weergegeven.

![Architectuur van SQL Data Warehouse][1]

**Beheerknooppunt:** het beheerknooppunt beheert en optimaliseert query's. Het is de front-end met interactie met alle toepassingen en verbindingen. In SQL Data Warehouse werkt het beheerknooppunt op basis van SQL Database en het maken van een verbinding gaat ongeveer hetzelfde. Op de achtergrond coördineert het beheerknooppunt alle benodigde verplaatsingen van gegevens en berekeningen voor het uitvoeren van parallelle query's op uw gedistribueerde gegevens. Wanneer u een TSQL-query naar SQL Data Warehouse verzendt, wordt deze door het beheerknooppunt getransformeerd tot afzonderlijke query's die parallel worden uitgevoerd op de afzonderlijke rekenknooppunten.

**Rekenknooppunten:** de rekenknooppunten vormen de kracht achter SQL Data Warehouse. Het zijn SQL Databases die uw gegevens opslaan en uw query verwerken. Wanneer u gegevens toevoegt, worden de rijen door SQL Data Warehouse gedistribueerd naar uw rekenknooppunten. De rekenknooppunten zijn de workers waarop de parallelle query's op uw gegevens worden uitgevoerd. Na de verwerking worden de resultaten teruggestuurd naar het beheerknooppunt. Om de query te voltooien, worden de resultaten door het beheerknooppunt geaggregeerd en het definitieve resultaat geretourneerd.

**Opslag:** uw gegevens worden opgeslagen in Azure-blobopslag. Bij interacties tussen de rekenknooppunten en uw gegevens worden de gegevens rechtstreeks vanuit de blob-opslag gelezen of ernaar weggeschreven. Omdat opslag in Azure transparant en enorm kan worden uitgebreid, kan dat ook in SQL Data Warehouse. Omdat reken- en opslagcapaciteit niet van elkaar afhankelijk zijn, kan de opslagcapaciteit in SQL Data Warehouse automatisch los van de rekencapaciteit worden geschaald en omgekeerd. Azure-blobopslag is ook volledig fouttolerant. Dit vergemakkelijkt het back-up- en herstelproces.

**Data Movement Service:** Data Movement Service (DMS) verplaatst gegevens van het ene naar het andere knooppunt. DMS geeft de rekenknooppunten toegang tot de benodigde gegevens voor samenvoegingen en aggregaties. DMS is geen Azure-service. Het is een Windows-service die naast SQL Database wordt uitgevoerd op alle knooppunten. Omdat DMS op de achtergrond wordt uitgevoerd, werkt u er niet rechtstreeks mee. Maar wanneer u de queryplannen bekijkt, ziet u dat deze enkele DMS-bewerkingen bevatten omdat er toch gegevensverplaatsing nodig is om elke query parallel te kunnen uitvoeren.

## Geoptimaliseerd voor datawarehouse-workloads
De MPP-methode wordt ondersteund door een aantal specifieke optimalisaties voor datawarehouseprestaties, waaronder:

* een gedistribueerde queryoptimalisatie en een set complexe statistieken voor alle gegevens. Met behulp van informatie over de omvang en distributie van gegevens kunnen query's in de service worden geoptimaliseerd door de kosten van specifieke gedistribueerde querybewerkingen te berekenen.
* In het gegevensverplaatsingsproces zijn geavanceerde algoritmen en technieken geïntegreerd om de gegevens efficiënt te verplaatsen tussen de computerbronnen die nodig zijn voor het uitvoeren van de query. Deze gegevensverplaatsingsbewerkingen zijn ingebouwd en alle optimalisaties voor Data Movement Service vinden automatisch plaats.
* Er wordt standaard gebruikgemaakt van geclusterde **kolomopslag**indexen. Dankzij kolomopslag is de compressie in SQL Data Warehouse gemiddeld vijf keer beter dan bij traditionele rij-opslag en zijn de prestaties tot wel tien of meer keer beter. De prestaties van analysequery's waarbij een groot aantal rijen moeten worden gescand, zijn uitstekend bij kolomopslagindexen.

## Voorspelbare en schaalbare prestaties
SQL Data Warehouse scheidt opslagruimte en rekencapaciteit, waardoor elk afzonderlijk kan schalen. SQL Data Warehouse kan op elk gewenst moment snel en eenvoudig worden opgeschaald voor het toevoegen van extra rekenresources. Een bijkomend voordeel is het gebruik van Azure-blobopslag. Blobs zorgen niet alleen voor een stabiele, gerepliceerde opslag, maar bieden ook mogelijkheden voor probleemloze uitbreiding tegen lage kosten. Door deze combinatie van schaalbare cloudopslag en Azure-rekencapaciteit, kunt u met SQL Data Warehouse betalen voor queryprestaties en opslag wanneer u dit nodig hebt. De rekencapaciteit is eenvoudig te wijzigen met een schuifregelaar in de Azure Portal, en kan ook worden gepland met TSQL en PowerShell.

Naast de mogelijkheid om de reken- en opslagcapaciteit onafhankelijk van elkaar in te stellen, kunt u uw datawarehouse in SQL Data Warehouse ook volledig onderbreken. Dit betekent dat u niet betaalt voor rekencapaciteit als u dit niet nodig hebt. Uw opslagcapaciteit blijft behouden, maar alle rekencapaciteit wordt vrijgegeven in de hoofdpool van Azure, waardoor u geld bespaart. U kunt de berekeningen hervatten wanneer u wilt en direct weer over uw gegevens en rekencapaciteit beschikken voor uw workload.

## Data Warehouse Units
Toewijzing van resources aan uw SQL Data Warehouse wordt gemeten in DWU's (Data Warehouse Units). DWU's zijn een maatstaf van de onderliggende resources, zoals CPU, geheugen en IOP's, die worden toegewezen aan de SQL Data Warehouse. Hoe meer DWU's, hoe meer resources en hoe beter de prestaties. DWU's zorgen met name voor het volgende:

* U kunt uw datawarehouse eenvoudig schalen zonder dat u zich zorgen hoeft te maken over de onderliggende hardware of software.
* U kunt prestatieverbetering voor een DWU-niveau voorspellen voordat u de grootte van het datawarehouse wijzigt.
* De onderliggende hardware en software van uw exemplaar kunnen worden gewijzigd of verplaatst zonder negatieve gevolgen voor de workload.
* Microsoft kan de onderliggende architectuur van de service aanpassen zonder de prestaties van uw workload te beïnvloeden.
* Microsoft kan de prestaties van SQL Data Warehouse snel verbeteren op een manier waarop de workload schaalbaar is en evenredig wordt verdeeld over het systeem.

Data Warehouse Units (DWU's) bieden een maatstaf voor drie metrieken die nauw verband houden met de prestaties van datawarehouses bij een bepaalde workload. Het doel is de volgende workloadmetrieken lineair te laten schalen met het gekozen aantal DWU's voor uw datawarehouse.

**Scan/aggregatie:** deze workloadmetriek is gebaseerd op een standaard-datawarehouse-query waarmee een groot aantal rijen wordt gescand waarop vervolgens een complexe aggregatie wordt uitgevoerd. Dit is een I/O-bewerking waarbij de CPU intensief wordt belast.

**Belasting:** met deze metriek wordt de capaciteit voor het opnemen van gegevens in de service gemeten. Hiertoe wordt met PolyBase een representatieve gegevensset uit een Azure-blobopslag geladen. Deze metriek fungeert als stresstest voor de netwerk- en CPU-aspecten van de service.

**Create Table As Select (CTAS):** met CTAS wordt de capaciteit voor het kopiëren van een tabel gemeten. Dit omvat het lezen van gegevens uit de opslag, het verdelen van de gegevens over de knooppunten van het systeem en het weer terugschrijven van de gegevens naar de opslag. Dit is een bewerking waarbij de CPU, de IO en het netwerk intensief worden belast.

## Onderbreken en schalen op basis van vraag
Wanneer u snellere resultaten wilt, verhoogt u het aantal DWU's en betaalt u voor betere prestaties. Wanneer u minder rekencapaciteit nodig hebt, verlaagt u het aantal DWU's en betaalt u alleen voor wat u nodig hebt. Bij een van deze scenario's is het nuttig uw DWU's te wijzigen:

* Wanneer u geen query's hoeft uit te voeren, bijvoorbeeld 's avonds of in het weekend, kunt u de query's stilleggen. Vervolgens onderbreekt u de rekenresources om te voorkomen dat u betaalt voor DWU's wanneer u deze niet nodig hebt.
* Bij een lage belasting van uw systeem kunt u overwegen uw aantal DWU’s te verkleinen. U hebt nog steeds toegang tot de gegevens, maar tegen aanzienlijk lagere kosten.
* Wanneer u een zware laad- of transformatiebewerking wilt uitvoeren, kunt u de capaciteit vergroten (omhoog schalen) zodat uw gegevens sneller beschikbaar zijn.

Om te bepalen wat voor u de ideale DWU-waarde is, kunt u omhoog en omlaag schalen en na het laden van uw gegevens enkele query's uitvoeren. Omdat schalen zo snel kan, kunt u binnen een uur een aantal verschillende prestatieniveaus uitproberen.  Houd er rekening mee dat SQL Data Warehouse is ontworpen voor het verwerken van grote hoeveelheden gegevens. Om de werkelijke capaciteit voor schalen te zien, met name de grotere schalen die wij bieden, is het nuttig een grote gegevensset van om en nabij de 1 TB te gebruiken.

## Gebouwd op SQL Server
SQL Data Warehouse is gebaseerd op de relationele database-engine van de SQL Server en bevat veel functies die u van een datawarehouse voor ondernemingen verwacht. Als u al bekend bent met T-SQL, zult u weinig moeite hebben met de overgang naar SQL Data Warehouse. Of u nu een ervaren of beginnende gebruiker bent, met de voorbeelden in de documentatie kunt u snel aan de slag. Over het algemeen kunt u nadenken over de manier waarop de elementen van de taal van SQL Data Warehouse zijn opgebouwd:

* SQL Data Warehouse gebruikt T-SQL-syntaxis voor veel bewerkingen. Deze biedt ook ondersteuning voor diverse traditionele SQL-constructs, zoals opgeslagen procedures, door de gebruiker gedefinieerde functies, tabelpartities, indexen en sorteringen.
* SQL Data Warehouse bevat ook een aantal nieuwere functies van SQL Server, zoals geclusterde **kolomopslag**indexen, PolyBase-integratie en gegevensauditing (inclusief beoordeling van bedreigingen).
* Bepaalde T-SQL-taalelementen die minder vaak voorkomen in werkbelastingen voor datawarehouses, of die nieuwer zijn voor SQL Server, zijn mogelijk niet beschikbaar. Zie voor meer informatie de [migratiedocumentatie][migratiedocumentatie].

Doordat SQL Server, SQL Data Warehouse, SQL Database en Analytics Platform System de Transact-SQL en diverse functies met elkaar gemeen hebben, kunt u een passende oplossing ontwikkelen voor uw gegevensbehoeften. U kunt besluiten waar u uw gegevens wilt bewaren, op basis van de gewenste prestaties, beveiliging en schaal, en uw gegevens vervolgens van het ene naar het andere systeem verplaatsen.

## Gegevensbeveiliging
Alle SQL Data Warehouse-gegevens worden opgeslagen in Azure Premium-opslag met lokaal redundante opslag. In het lokale datacentrum worden meerdere synchrone kopieën van de gegevens bewaard voor gegarandeerd transparante gegevensbeveiliging tegen gelokaliseerde fouten. Bovendien maakt SQL Data Warehouse automatisch en regelmatig back-ups van uw actieve (niet-gepauzeerde) databases met Azure Storage-momentopnamen. Zie voor meer informatie over hoe Back-ups en herstellen werkt, het [Overzicht van Back-ups en herstellen][Overzicht van Back-ups en herstellen].

## Geïntegreerd met Microsoft-hulpprogramma's
SQL Data Warehouse integreert ook veel van de hulpmiddelen waar gebruikers van SQL Server mee bekend zijn. Deze omvatten:

**Traditionele SQL Server-hulpprogramma's:** SQL Data Warehouse is volledig geïntegreerd met SQL Server Analysis Services, Integration Services en Reporting Services.

**Cloud-hulpprogramma's:** SQL Data Warehouse kan worden gebruikt in combinatie met een aantal nieuwe hulpprogramma's in Azure, waaronder Data Factory, Stream Analytics, Machine Learning en Power BI. Zie voor een volledig overzicht [Overzicht met geïntegreerde hulpmiddelen][Overzicht met geïntegreerde hulpmiddelen].

**Hulpprogramma's van derden:** veel externe leveranciers hebben de integratie van hun hulpprogramma's met SQL Data Warehouse laten certificeren. Zie voor een volledige lijst [SQL Data Warehouse-oplossingspartners][SQL Data Warehouse-oplossingspartners].

## Hybride scenario's met gegevensbronnen
Gecombineerd gebruik van SQL Data Warehouse en PolyBase biedt gebruikers ongekende mogelijkheden om gegevens te verplaatsen binnen hun systeemomgeving, inclusief de mogelijkheid voor geavanceerde hybride scenario's met niet-relationele en on-premises gegevensbronnen.

PolyBase stelt u in staat gebruik te maken van verschillende gegevensbronnen met vertrouwde TSQL-opdrachten. Met PolyBase kunt u niet-relationele gegevens opvragen uit de Azure-blobopslag, net zoals u gegevens kunt opvragen uit gewone tabellen. Met PolyBase kunt u ook niet-relationele gegevens opvragen of niet-relationele gegevens importeren in SQL Data Warehouse.

* In PolyBase worden externe tabellen gebruikt voor toegang tot niet-relationele gegevens. De tabeldefinities worden opgeslagen in SQL Data Warehouse en zijn toegankelijk via SQL en hulpprogramma's, net als bij normale relationele gegevens.
* Polybase is agnostisch qua integratie. De functies en functionaliteit ervan zijn hetzelfde voor alle bronnen die door Polybase worden ondersteund. In Polybase kunnen gegevensbestanden in diverse indelingen worden gelezen, waaronder bestanden met scheidingstekens of ORC-bestanden.
* PolyBase kan worden gebruikt voor toegang tot Blob Storage die ook wordt gebruikt als opslag voor een HDInsight-cluster. Hierdoor hebt u toegang tot dezelfde gegevens met relationele en niet-relationele hulpprogramma's.

## Volgende stappen
Nu u een en ander weet over SQL Data Warehouse, kunt u leren hoe u snel [een SQL Data Warehouse maakt][een SQL Data Warehouse maakt] en [voorbeeldgegevens laden][voorbeeldgegevens laden]. Als u niet bekend bent met Azure, kan de [Azure-woordenlijst][Azure-woordenlijst] handig zijn bij het opzoeken van nieuwe terminologie. Zie desgewenst ook deze informatiebronnen voor SQL Data Warehouse.  

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
[voorbeeldgegevens laden]: ./sql-data-warehouse-load-sample-databases.md
[een SQL Data Warehouse maakt]: ./sql-data-warehouse-get-started-provision.md
[migratiedocumentatie]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse-oplossingspartners]: ./sql-data-warehouse-partner-business-intelligence.md
[Overzicht met geïntegreerde hulpmiddelen]: ./sql-data-warehouse-overview-integrate.md
[Overzicht van Back-ups en herstellen]: ./sql-data-warehouse-restore-database-overview.md
[Azure-woordenlijst]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Succesverhalen van klanten]: https://customers.microsoft.com/search?sq=&ff=story_products_services%26%3EAzure%2FAzure%2FAzure%20SQL%20Data%20Warehouse%26%26story_product_families%26%3EAzure%2FAzure%26%26story_product_categories%26%3EAzure&p=0
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Teamblogs met adviezen voor klanten]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Functieverzoeken]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-forum]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Stack Overflow-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse



<!--HONumber=Sep16_HO4-->


