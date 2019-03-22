---
title: Uitpakken, transformeren en laden (ETL) op schaal - Azure HDInsight
description: Meer informatie over hoe ETL wordt gebruikt in HDInsight met Apache Hadoop.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: ashishth
ms.openlocfilehash: c200ca98f2a5ea32886ec12d3e732af6598254f7
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58337609"
---
# <a name="extract-transform-and-load-etl-at-scale"></a>Extraheren, transformeren en laden (ETL) op schaal

Extraheren, transformeren en laden (ETL) is het proces waarmee gegevens wordt verkregen uit verschillende bronnen, die worden verzameld in een standaardlocatie opgeschoond en verwerkt en uiteindelijk worden geladen in een gegevensarchief van waaruit deze kan worden opgevraagd. Verouderde ETL-processen importeren van gegevens, deze op te schonen in plaats en vervolgens opslaan in een engine voor relationele gegevens. Met HDInsight, een groot aantal onderdelen van Apache Hadoop-ecosysteem uitvoeren van ETL op schaal ondersteunen. 

Het gebruik van HDInsight in het ETL-proces kan door deze pijplijn worden samengevat:

![HDInsight ETL-overzicht](./media/apache-hadoop-etl-at-scale/hdinsight-etl-at-scale-overview.png)

De volgende secties verkennen elk van de ETL-fasen en de bijbehorende onderdelen.

## <a name="orchestration"></a>Indeling

Orchestration reeksen in alle fasen van de ETL-pijplijn. ETL-taken in HDInsight omvatten vaak verschillende producten werken in combinatie met elkaar.  U kunt Hive gebruiken voor het opschonen van een gedeelte van de gegevens, terwijl Pig een ander deel opschonen.  U kunt Azure Data Factory gebruiken om gegevens te laden in Azure SQL Database van Azure Data Lake Store.

Orchestration is nodig voor het uitvoeren van de juiste taak op het juiste moment.

### <a name="apache-oozie"></a>Apache Oozie

Apache Oozie is een coördinatiesysteem voor werkstromen waarmee Hadoop-taken worden beheerd. Oozie binnen een HDInsight-cluster wordt uitgevoerd en is geïntegreerd met de Hadoop-stack. Oozie biedt ondersteuning voor Hadoop-taken voor Apache Hadoop MapReduce, Apache Pig, Apache Hive en Apache Sqoop. Oozie kan ook worden gebruikt voor het plannen van taken die specifiek voor een systeem, zoals Java-programma's of shell-scripts zijn.

Zie voor meer informatie, [Apache Oozie gebruiken met Apache Hadoop voor het definiëren en een werkstroom uitvoeren op HDInsight](../hdinsight-use-oozie-linux-mac.md) voor gedetailleerde informatie waarin wordt getoond hoe Oozie gebruiken om een end-to-end-pijplijn te stimuleren, Zie [operationeel maken van de Gegevenspijplijn](../hdinsight-operationalize-data-pipeline.md). 

### <a name="azure-data-factory"></a>Azure Data Factory

Azure Data Factory biedt planningsmogelijkheden in de vorm van platform-as-a-service. Het is een cloudgebaseerde gegevensintegratieservice waarmee u gegevensgestuurde werkstromen maken in de cloud voor het indelen en automatiseren gegevensverplaatsingen en gegevenstransformaties. 

Met Azure Data Factory, kunt u het volgende doen:

1. Maken en plannen van gegevensgestuurde werkstromen (pijplijnen genoemd) die gegevens uit verschillende gegevensarchieven opnemen.
2. Verwerk en transformeer de gegevens met behulp van rekenservices zoals Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics, Azure Batch en Azure Machine Learning.
3. Uitvoergegevens publiceren naar gegevensopslaglocaties als Azure SQL Data Warehouse, waar BI-toepassingen (business intelligence) er gebruik van kunnen maken.

Zie voor meer informatie over Azure Data Factory, de [documentatie](../../data-factory/introduction.md).

## <a name="ingest-file-storage-and-result-storage"></a>Opslag van bestanden en opslag van resultaat opnemen

Bronbestanden van de gegevens worden meestal in een locatie in Azure Storage of Azure Data Lake Storage geladen. Bestanden kunnen zijn in elke indeling, maar ze zijn meestal platte bestanden, zoals CSV's. 

### <a name="azure-storage"></a>Azure Storage 

[Azure Storage](https://azure.microsoft.com/services/storage/blobs/) heeft [specifieke schaalbaarheidsdoelen](../../storage/common/storage-scalability-targets.md).  Voor meest analytische knooppunten, Azure Storage schaalt beste tijdens het afhandelen van veel kleinere bestanden.  Azure Storage zorgt ervoor dat de dezelfde prestaties, ongeacht het aantal bestanden of hoe groot de bestanden (zo lang als u binnen uw grenzen).  Dit betekent dat u kunt terabytes aan gegevens opslaan en nog steeds consistente prestaties, ongeacht of u er een subset van de gegevens of alle van de gegevens.

Azure Storage heeft verschillende typen blobs.  Een *toevoeg-blob* is een goede optie voor het opslaan van weblogboeken of sensorgegevens.  

Meerdere blobs kunnen worden gedistribueerd voor grote aantallen servers uit de toegang tot te schalen, maar één blob kan alleen worden geleverd door één server. Blobs kunnen logisch worden gegroepeerd in blob-containers, maar er zijn geen partitioneren gevolgen van deze groepering.

Azure Storage heeft ook een API WebHDFS-laag voor de blob-opslag.  Alle services in HDInsight hebben toegang tot bestanden in Azure Blob Storage voor het opschonen van gegevens en gegevensverwerking, vergelijkbaar met hoe deze services Hadoop Distributed bestanden System (HDFS) gebruiken.

Doorgaans zijn gegevens opgenomen in Azure Storage met behulp van PowerShell, de Azure Storage-SDK of AZCopy.

### <a name="azure-data-lake-storage"></a>Azure Data Lake Storage

Azure Data Lake Storage (ADLS) is een beheerde, zeer grootschalige opslagplaats voor analytics-gegevens die compatibel is met HDFS.  ADLS maakt gebruik van een ontwerp paradigma is vergelijkbaar met HDFS, en biedt onbeperkte schaalbaarheid wat betreft de totale capaciteit en de grootte van afzonderlijke bestanden. ADLS is zeer goed bij het werken met grote bestanden, omdat een groot bestand kan worden opgeslagen op meerdere knooppunten.  Partitioneren van gegevens in ADLS loopt achter de schermen.  Data Lake Store biedt u een enorme doorvoer voor het uitvoeren van analysetaken waarbij meer dan duizend simultane uitvoerders honderden terabytes aan gegevens efficiënt lezen en schrijven.

Doorgaans zijn gegevens opgenomen in ADLS met behulp van Azure Data Factory, een ADLS-SDK's, AdlCopy Service, Apache DistCp of Apache Sqoop.  Welke van deze services te gebruiken grotendeels afhankelijk van de gegevens.  Als de gegevens zich momenteel in een bestaande Hadoop-cluster, kunt u Apache DistCp, AdlCopy Service of Azure Data Factory.  Als het in Azure Blob Storage, kunt u Azure Data Lake Storage .NET SDK, Azure PowerShell of Azure Data Factory.

ADLS is ook geoptimaliseerd voor gebeurtenisopname met behulp van Azure Event Hub of Apache Storm.

#### <a name="considerations-for-both-storage-options"></a>Overwegingen voor beide opties voor opslag

De netwerklatentie kan een groot probleem zijn met name als de gegevens afkomstig is van een on-premises locatie voor het uploaden van gegevenssets in het bereik terabyte.  In dergelijke gevallen kunt u de onderstaande opties:

* Azure ExpressRoute:  Azure ExpressRoute kunt u particuliere verbindingen maken tussen Azure-datacenters en uw on-premises infrastructuur. Deze verbindingen bieden een betrouwbare optie voor het overbrengen van grote hoeveelheden gegevens. Zie voor meer informatie, [documentatie voor Azure ExpressRoute](../../expressroute/expressroute-introduction.md).

* 'Offline' uploaden van gegevens. U kunt [Azure Import/Export-service](../../storage/common/storage-import-export-service.md) voor verzending van harde schijven met uw gegevens om een Azure-Datacenter. Uw gegevens eerst geüpload naar Azure Storage-Blobs. Vervolgens kunt u [Azure Data Factory](../../data-factory/connector-azure-data-lake-store.md) of de [AdlCopy](../../data-lake-store/data-lake-store-copy-data-azure-storage-blob.md) hulpprogramma gegevens kopiëren van Azure Storage-blobs naar Data Lake-opslag.

### <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse

Azure SQL DW is een uitstekende keuze voor het opslaan van gereinigd en worden resultaten voorbereid voor toekomstige analyse.  Azure HDInsight kan worden gebruikt om uit te voeren van deze services voor Azure SQL DW.

Azure SQL Data Warehouse (SQL DW) is een relationele database-opslag geoptimaliseerd voor analytische workloads.  Azure SQL DW kan worden geschaald op basis van gepartitioneerde tabellen.  Tabellen kunnen worden gepartitioneerd over meerdere knooppunten.  Azure SQL DW-knooppunten zijn geselecteerd op het moment van maken.  Ze kunnen worden geschaald na de gebeurtenis, maar dat een actief proces waarvoor verplaatsing van gegevens mogelijk is. Zie [SQL Data Warehouse - beheren Compute](../../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md) voor meer informatie.

### <a name="apache-hbase"></a>Apache HBase

Apache HBase is een sleutel / waarde-opslag die beschikbaar zijn in Azure HDInsight.  Apache HBase is een open-source NoSQL-database die is gebaseerd op Hadoop en die is gemodelleerd naar Google BigTable. HBase biedt willekeurige toegang van goed presterende en sterke consistentie voor grote hoeveelheden ongestructureerde en semigestructureerde gegevens in een database zonder schema onderverdeeld op basis van kolomfamilies.

De gegevens worden opgeslagen in de rijen van een tabel en de gegevens in een rij worden gegroepeerd op basis van de kolomfamilie. HBase is een database zonder schema in de zin dat zowel de kolommen als het type gegevens dat hierin wordt opgeslagen niet hoeven te worden gedefinieerd voordat u ze kunt gebruiken. De open-source code wordt lineair geschaald om petabytes aan gegevens op duizenden knooppunten te verwerken. HBase kan afhankelijk zijn van de gegevensredundantie, batchverwerking en andere functies die worden geboden door gedistribueerde toepassingen in het Hadoop-ecosysteem.   

HBase is een uitstekende bestemming voor sensor- en logboekbestanden voor toekomstig gebruik.

HBase-schaalbaarheid is afhankelijk van het aantal knooppunten in het HDInsight-cluster.

### <a name="azure-sql-database-and-azure-database"></a>Azure SQL Database en Azure-Database

Azure biedt drie verschillende relationele databases als platform-as-a-service (PAAS).

* [Azure SQL Database](../../sql-database/sql-database-technical-overview.md) is een implementatie van Microsoft SQL Server. Zie voor meer informatie over prestaties, [afstemmen van prestaties in Azure SQL Database](../../sql-database/sql-database-performance-guidance.md).
* [Azure Database voor MySQL](../../mysql/overview.md) is een implementatie van Oracle, MySQL.
* [Azure Database voor PostgreSQL](../../postgresql/quickstart-create-server-database-portal.md) is een implementatie van de PostgreSQL.

Deze producten opschalen, wat betekent dat ze worden geschaald door meer CPU en geheugen toe te voegen.  U kunt ook het gebruik van premium-schijven met de producten voor betere i/o-prestaties.

## <a name="azure-analysis-services"></a>Azure Analysis Services 

Azure Analysis Services (AAS) is een engine voor analytische gegevens wordt gebruikt voor ondersteuning voor besluitvorming en business analytics, met de analytische gegevens voor zakelijke rapporten en clienttoepassingen zoals Power BI, Excel, Reporting Services-rapporten en andere gegevens hulpprogramma's voor visualisatie.

Analyse van kubussen kunnen schalen door het veranderen van lagen voor elke afzonderlijke kubus.  Zie voor meer informatie, [prijzen van Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="extract-and-load"></a>Uitpakken en laden

Zodra de gegevens aanwezig zijn in Azure, kunt u veel services kunt gebruiken om te halen en deze te laden in andere producten.  HDInsight ondersteunt Sqoop en Flume. 

### <a name="apache-sqoop"></a>Apache Sqoop

Apache Sqoop is een hulpprogramma dat is ontworpen voor het efficiënt gegevensoverdracht tussen gestructureerde, semi-gestructureerde en ongestructureerde gegevensbronnen. 

Sqoop maakt gebruik van MapReduce om te importeren en exporteren van de gegevens voor parallelle bewerking en fouttolerantie.

### <a name="apache-flume"></a>Apache Flume

Apache Flume is een gedistribueerde, betrouwbare en beschikbare service voor efficiënt verzamelen, aggregeren en verplaatsen van grote hoeveelheden gegevens aan het logboek. Flume is een eenvoudige en flexibele architectuur op basis van streaming gegevensstromen. Flume is robuuste en fouttolerantie met instelbare betrouwbaarheid mechanismen en veel mechanismen voor failover en herstel. Flume maakt gebruik van een eenvoudige extensible gegevensmodel waarmee voor online analytische toepassing.

Apache Flume kan niet worden gebruikt met Azure HDInsight.  Een on-premises Hadoop-installatie kunt Flume gebruiken om gegevens te verzenden naar Azure Storage-Blobs of Azure Data Lake-opslag.  Zie voor meer informatie, [met behulp van Apache Flume met HDInsight](https://web.archive.org/web/20190217104751/ https://blogs.msdn.microsoft.com/bigdatasupport/2014/03/18/using-apache-flume-with-hdinsight/).

## <a name="transform"></a>Transformeren

Zodra de gegevens in de gekozen locatie bestaat, moet u deze op te schonen, Combineer de oplossing of voorbereiden voor een specifieke gebruikspatroon.  Zijn alle goede keuze voor dit soort werk hive, Pig- en Spark SQL.  Ze worden ondersteund op HDInsight. 

## <a name="next-steps"></a>Volgende stappen

* [Apache Pig gebruiken met Apache Hadoop op HDInsight](hdinsight-use-pig.md)
* [Apache Hive gebruiken als een ETL-hulpprogramma](apache-hadoop-using-apache-hive-as-an-etl-tool.md) 
* [Azure Data Lake Storage Gen2 gebruiken met Azure HDInsight-clusters](../hdinsight-hadoop-use-data-lake-storage-gen2.md)
