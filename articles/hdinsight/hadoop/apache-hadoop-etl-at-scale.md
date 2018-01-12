---
title: Uitpakken, transformeren en laden (ETL) op grote schaal - Azure HDInsight | Microsoft Docs
description: Meer informatie over hoe ETL wordt gebruikt in HDInsight met Hadoop.
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/14/2017
ms.author: ashishth
ms.openlocfilehash: 47c2d129cb296f6387142e03b14356bcd83ad698
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="extract-transform-and-load-etl-at-scale"></a>Uitpakken, transformeren en laden (ETL) op grote schaal

Uitpakken, transformeren en laden (ETL) is het proces waarmee gegevens is verkregen uit diverse bronnen, verzameld in een standaardlocatie gereinigd en verwerkt en uiteindelijk is geladen in een gegevensarchief waaruit deze kan worden opgevraagd. Gegevens importeren verouderde ETL-processen, deze op te schonen aanwezig en vervolgens opslaan in een relationele gegevens-engine. Met HDInsight, een groot aantal verschillende onderdelen van Hadoop-ecosysteem ETL uitvoeren op grote schaal ondersteunen. 

Het gebruik van HDInsight in het ETL-proces kan worden samengevat door deze pijplijn:

![HDInsight ETL-overzicht](./media/apache-hadoop-etl-at-scale/hdinsight-etl-at-scale-overview.png)

De volgende secties verkennen elk van de ETL-fasen en de bijbehorende onderdelen.

## <a name="orchestration"></a>Indeling

Orchestration omvat tijdens alle fasen van de ETL-pijplijn. ETL-taken in HDInsight vaak hebben betrekking op verschillende producten werken in combinatie met elkaar.  Hive kunt u een gedeelte van de gegevens wilt opschonen terwijl Pig een ander gedeelte schoongemaakt.  Azure Data Factory kunt u gegevens laden in Azure SQL Database in Azure Data Lake Store.

Orchestration is nodig voor het uitvoeren van de juiste taak op het juiste moment.

### <a name="oozie"></a>Oozie

Apache Oozie is een coördinatiesysteem voor werkstromen waarmee Hadoop-taken worden beheerd. Oozie binnen een HDInsight-cluster wordt uitgevoerd en is geïntegreerd met de Hadoop-stack. Oozie ondersteunt Hadoop-taken voor Apache MapReduce, Apache Pig, Apache Hive en Apache Sqoop. Oozie kan ook worden gebruikt voor het plannen van taken die specifiek voor een systeem, zoals Java-programma's of shell-scripts zijn.

Zie voor meer informatie [Oozie gebruiken met Hadoop om te definiëren en uitvoeren van een werkstroom in HDInsight](../hdinsight-use-oozie-linux-mac.md)

<!-- For a deep dive showing how to use Oozie to drive an end-to-end pipeline, see [Operationalize the Data Pipeline](hdinsight-operationalize-data-pipeline.md). -->

### <a name="azure-data-factory"></a>Azure Data Factory

Azure Data Factory biedt mogelijkheden voor orchestration in de vorm van platform as a service. Het is een cloud-gebaseerde gegevens integration-service waarmee u gegevensgestuurde werkstromen in de cloud voor regelen en te automatiseren verplaatsing van gegevens en gegevenstransformatie maken. 

Met Azure Data Factory, kunt u het volgende doen:

1. Maken en plannen van gegevensgestuurde werkstromen (pijplijnen genoemd) die voor het opnemen van gegevens uit verschillende gegevensarchieven.
2. Proces en de gegevens met transformatie compute-services zoals Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics, Azure Batch en Azure Machine Learning.
3. Uitvoergegevens publiceren naar gegevensopslaglocaties als Azure SQL Data Warehouse, waar BI-toepassingen (business intelligence) er gebruik van kunnen maken.

Zie voor meer informatie over Azure Data Factory, de [documentatie](../../data-factory/introduction.md).

## <a name="ingest-file-storage-and-result-storage"></a>Opslag van bestanden en opslag van resultaat opnemen

Bronbestanden van de gegevens worden doorgaans geladen op een locatie in Azure Storage of Azure Data Lake Store. Bestanden kunnen worden in een willekeurige indeling, maar ze zijn meestal platte bestanden zoals CSV's. 

### <a name="azure-storage"></a>Azure Storage 

[Azure Storage](https://azure.microsoft.com/services/storage/blobs/) heeft [schaalbaarheidsdoelen van specifieke](../../storage/common/storage-scalability-targets.md).  Voor de meest analytische knooppunten schaalt Azure Storage beste veel kleinere bestanden betreft.  Azure Storage wordt gegarandeerd dat de dezelfde prestaties, ongeacht hoeveel bestanden of hoe groot de bestanden (zolang u binnen de grenzen zijn).  Dit betekent dat u kunt terabytes aan gegevens opslaan en nog steeds consistente prestaties, ongeacht of u een subset van de gegevens of alle gegevens.

Azure-opslag heeft verschillende typen blobs.  Een *toevoeg-blob* is een geweldige optie voor het opslaan van weblogboeken of sensorgegevens.  

Meerdere blobs kunnen op veel servers moet worden uitgebreid met toegang tot deze worden gedistribueerd, maar één blob kan alleen worden geleverd door één server. Blobs kunnen logisch worden gegroepeerd in blob-containers, maar er zijn geen partitionering gevolgen van deze groepering.

Azure-opslag heeft ook een laag WebHDFS-API voor de blobopslag.  Alle services in HDInsight toegang tot bestanden in Azure Blob-opslag voor gegevens opruimen en gegevensverwerking, net zoals bij hoe deze services Hadoop Distributed bestanden System (HDFS) gebruiken.

Gegevens wordt doorgaans in Azure Storage via PowerShell, de Azure-opslag-SDK, of AZCopy ingenomen.

### <a name="azure-data-lake-store"></a>Azure Data Lake Store

Azure Data Lake Store (ADLS) is een beheerde, grootschalig opslagplaats voor analytische gegevens die compatibel is met HDFS.  ADLS maakt gebruik van een ontwerp paradigma die is vergelijkbaar met HDFS en biedt onbeperkte schaalbaarheid in termen van de totale capaciteit en de grootte van afzonderlijke bestanden. ADLS is zeer goed bij het werken met grote bestanden, omdat er een groot bestand kan worden opgeslagen op meerdere knooppunten.  Gegevens in ADLS gepartitioneerd achter de schermen.  Data Lake Store biedt u een enorme doorvoer voor het uitvoeren van analysetaken waarbij meer dan duizend simultane uitvoerders honderden terabytes aan gegevens efficiënt lezen en schrijven.

Gegevens wordt doorgaans in ADLS met behulp van Azure Data Factory, ADLS-SDK's, AdlCopy Service, Apache DistCp of Apache Sqoop ingenomen.  Welke van deze services te gebruiken grotendeels afhankelijk van waar de gegevens zijn.  Als de gegevens zich in een bestaand Hadoop-cluster, kunt u Apache DistCp, AdlCopy Service of Azure Data Factory.  Als het zich in Azure Blob Storage, kunt u Azure Data Lake Store .NET SDK, Azure PowerShell of Azure Data Factory.

ADLS is ook geoptimaliseerd voor gebeurtenis opname met Azure Event Hub of Apache Storm.

#### <a name="considerations-for-both-storage-options"></a>Overwegingen voor beide opties voor opslag

Voor het uploaden van gegevenssets binnen het bereik terabyte kan netwerklatentie een groot probleem zijn met name als de gegevens afkomstig is van een on-premises locatie.  In dergelijke gevallen kunt u de onderstaande opties:

* Azure ExpressRoute: Azure ExpressRoute kunt u particuliere verbindingen maken tussen Azure-datacenters en uw on-premises infrastructuur. Deze verbindingen bieden een betrouwbare optie voor het overbrengen van grote hoeveelheden gegevens. Zie voor meer informatie [Azure ExpressRoute documentatie](../../expressroute/expressroute-introduction.md).

* 'Offline' uploaden van gegevens. U kunt [Azure Import/Export-service](../../storage/common/storage-import-export-service.md) voor het verzenden van harde schijven met uw gegevens naar een Azure-Datacenter. Uw gegevens is eerst naar Azure Storage-Blobs geüpload. Vervolgens kunt u [Azure Data Factory](../../data-factory/v1/data-factory-azure-datalake-connector.md) of de [AdlCopy](../../data-lake-store/data-lake-store-copy-data-azure-storage-blob.md) hulpprogramma om gegevens te kopiëren van Azure Storage-blobs naar Data Lake Store.

### <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse

Azure SQL DW is een uitstekende keuze is voor het opslaan van gereinigd en resultaten voorbereid voor toekomstige analytics.  Azure HDInsight kan worden gebruikt voor het uitvoeren van deze services voor Azure SQL DW.

Azure SQL Data Warehouse (SQL DW) is een relationele database-opslag geoptimaliseerd voor analytische workloads.  Azure SQL DW wordt geschaald op basis van gepartitioneerde tabellen.  Tabellen kunnen worden gepartitioneerd op meerdere knooppunten.  Azure SQL DW-knooppunten zijn geselecteerd op het moment van maken.  Achteraf kan worden geschaald, maar dat is een actief proces waarvoor u mogelijk de verplaatsing van gegevens. Zie [SQL Data Warehouse - beheren Compute](../../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md) voor meer informatie.

### <a name="hbase"></a>HBase

Apache HBase is een sleutel-waardearchief beschikbaar zijn in Azure HDInsight.  Apache HBase is een open-source NoSQL-database die is gebaseerd op Hadoop en die is gemodelleerd naar Google BigTable. HBase biedt willekeurige toegang zodat en sterke consistentie voor grote hoeveelheden ongestructureerde en semigestructureerde gegevens in een database zonder schema ingedeeld op basis van kolomfamilies.

De gegevens worden opgeslagen in de rijen van een tabel en de gegevens in een rij worden gegroepeerd op basis van de kolomfamilie. HBase is een database zonder schema in de zin dat zowel de kolommen als het type gegevens dat hierin wordt opgeslagen niet hoeven te worden gedefinieerd voordat u ze kunt gebruiken. De open-source code wordt lineair geschaald om petabytes aan gegevens op duizenden knooppunten te verwerken. HBase kan afhankelijk zijn van gegevensredundantie, batchverwerking en andere functies die worden geboden door gedistribueerde toepassingen in het Hadoop-ecosysteem.   

HBase is een uitstekende bestemming voor gegevens van sensor- en logboekbestanden voor toekomstige analyse.

HBase schaalbaarheid is afhankelijk van het aantal knooppunten in het HDInsight-cluster.

### <a name="azure-sql-database-and-azure-database"></a>Azure SQL Database en de Azure-Database

Azure biedt drie verschillende relationele databases als platform-as-a-service (PAAS).

* [Azure SQL Database](../../sql-database/sql-database-technical-overview.md) is een implementatie van Microsoft SQL Server. Zie voor meer informatie over prestaties [prestaties afstemmen in Azure SQL Database](../../sql-database/sql-database-performance-guidance.md).
* [Azure MySQL-Database](../../mysql/overview.md) is een implementatie van Oracle, MySQL.
* [Azure-Database voor PostgreSQL](../../postgresql/quickstart-create-server-database-portal.md) is een implementatie van PostgreSQL.

Deze producten opschalen, wat betekent dat ze worden geschaald door meer CPU en geheugen toe te voegen.  U kunt ook gebruik van premium-schijven met de producten voor betere i/o-prestaties.

## <a name="azure-analysis-services"></a>Azure Analysis Services 

Azure Analysis Services (AAS) is een analytische gegevens-engine gebruikt in ondersteuning voor de besluitvorming en business analytics, voorziet in analytische gegevens zakelijke rapporten en clienttoepassingen, zoals Power BI, Excel, Reporting Services-rapporten en andere gegevens visualisatie hulpprogramma's.

Analyse kubussen kunnen schalen door lagen voor elke afzonderlijke kubus te wijzigen.  Zie voor meer informatie [prijzen van Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="extract-and-load"></a>Uitpakken en laden

Nadat de gegevens aanwezig zijn in Azure, kunt u veel services kunt gebruiken om te halen en te laden in andere producten.  HDInsight ondersteunt Sqoop en Flume. 

### <a name="sqoop"></a>Sqoop

Apache Sqoop is een hulpprogramma dat is ontworpen voor het efficiënt gegevensoverdracht tussen gestructureerde, semi-gestructureerde en ongestructureerde gegevens. 

MapReduce Sqoop gebruikt voor het importeren en exporteren van de gegevens, zodat de parallelle bewerking en fouttolerantie.

### <a name="flume"></a>Flume

Apache Flume is een service voor gedistribueerde, betrouwbare en beschikbaar voor het efficiënt verzamelen, aggregeren en verplaatsen van grote hoeveelheden gegevens aan het logboek. Flume heeft een eenvoudige en flexibele architectuur op basis van het streaming-gegevensstromen. Flume is robuust en fouttolerante met instelbare betrouwbaarheid mechanismen en veel mechanismen voor failover en herstel. Flume maakt gebruik van een eenvoudige extensible gegevensmodel waarmee voor online analytische toepassing.

Apache Flume kan niet worden gebruikt met Azure HDInsight.  De installatie van een on-premises Hadoop kunt Flume gebruiken om gegevens te verzenden naar Azure Storage-Blobs of Azure Data Lake Store.  Zie voor meer informatie [Apache Flume gebruiken met HDInsight](https://blogs.msdn.microsoft.com/bigdatasupport/2014/03/18/using-apache-flume-with-hdinsight/).

## <a name="transform"></a>Transformatie

Wanneer u gegevens bestaat in de gekozen locatie, moet u deze op te schonen, combineren of voorbereiden voor een specifieke gebruikspatroon.  Hive, Pig en Spark SQL zijn alle geschikt voor dit soort werk.  Ze worden ondersteund op HDInsight. 

## <a name="next-steps"></a>Volgende stappen

* [Pig gebruiken met Hadoop in HDInsight](hdinsight-use-pig.md)
<!-- * [Using Apache Hive as an ETL Tool](hdinsight-using-apache-hive-as-an-etl-tool.md) -->
