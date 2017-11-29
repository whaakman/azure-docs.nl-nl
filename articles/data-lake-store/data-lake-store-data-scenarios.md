---
title: Data-scenario's met betrekking tot Data Lake Store | Microsoft Docs
description: Overzicht van de verschillende scenario's en hulpprogramma's met behulp van welke gegevens kunnen ingenomen, verwerkt, gedownload en weergegeven in een Data Lake Store
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 37409a71-a563-4bb7-bc46-2cbd426a2ece
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 6428c6d9fcb577f18221ee48a61456c460bd8176
ms.sourcegitcommit: 651a6fa44431814a42407ef0df49ca0159db5b02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="using-azure-data-lake-store-for-big-data-requirements"></a>Met behulp van Azure Data Lake Store voor big data-vereisten
Er zijn vier belangrijke fasen in grote gegevensverwerking:

* Het opnemen van grote hoeveelheden gegevens in een gegevensarchief aan realtime of in batches
* Verwerken van de gegevens
* De gegevens te downloaden
* De gegevens te visualiseren

In dit artikel kijken we deze fasen met betrekking tot Azure Data Lake Store om te begrijpen van de opties en de hulpprogramma's die beschikbaar zijn om te voldoen aan de behoeften van uw big data.

## <a name="ingest-data-into-data-lake-store"></a>Opnemen van gegevens in Data Lake Store
Deze sectie worden de verschillende bronnen van gegevens en de verschillende manieren waarop die gegevens naar een Data Lake Store-account mag worden ingenomen.

![Opnemen van gegevens in Data Lake Store](./media/data-lake-store-data-scenarios/ingest-data.png "opnemen van gegevens in Data Lake Store")

### <a name="ad-hoc-data"></a>Ad-hoc gegevens
Dit staat voor kleinere gegevenssets die worden gebruikt voor het maken van een prototype een big data-toepassing. Er zijn verschillende manieren van ad-hoc gegevens opnemen, afhankelijk van de bron van de gegevens.

| Gegevensbron | Met behulp van opnemen |
| --- | --- |
| Lokale computer |<ul> <li>[Azure Portal](/data-lake-store-get-started-portal.md)</li> <li>[Azure PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Azure platformoverschrijdende CLI 2.0](data-lake-store-get-started-cli-2.0.md)</li> <li>[Met behulp van Data Lake Tools voor Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md) </li></ul> |
| Azure Storage-Blob |<ul> <li>[Azure Data Factory](../data-factory/connector-azure-data-lake-store.md)</li> <li>[AdlCopy hulpprogramma](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[DistCp uitgevoerd op HDInsight-cluster](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

### <a name="streamed-data"></a>Gestroomde gegevens
Hiermee worden gegevens die kunnen worden gegenereerd met verschillende bronnen zoals toepassingen, apparaten, sensoren, enzovoort. Deze gegevens kan door verschillende hulpprogramma's in een Data Lake Store worden ingenomen. Deze hulpprogramma's worden meestal vastleggen en verwerken van de gegevens op basis van door gebeurtenis in realtime, en schrijf vervolgens de gebeurtenissen in batches in Data Lake Store zodat ze verder kunnen worden verwerkt.

Hieronder vindt u hulpprogramma's die u kunt gebruiken:

* [Azure Stream Analytics](../stream-analytics/stream-analytics-data-lake-output.md) -gebeurtenissen ingenomen in Event Hubs kunnen worden geschreven naar Azure Data Lake met behulp van de uitvoer van een Azure Data Lake Store.
* [Azure HDInsight Storm](../hdinsight/storm/apache-storm-write-data-lake-store.md) -u kunt gegevens rechtstreeks naar Data Lake Store schrijven van het Storm-cluster.
* [EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md) – u kunt ontvangen van gebeurtenissen van Event Hubs en vervolgens te schrijven naar het Data Lake Store met behulp van de [Data Lake Store .NET SDK](data-lake-store-get-started-net-sdk.md).

### <a name="relational-data"></a>Relationele gegevens
U kunt ook brongegevens van relationele databases. Gedurende een periode verzamelen relationele databases van grote hoeveelheden gegevens die belangrijke inzichten bieden kan als verwerkt via een pijplijn big data. De volgende hulpprogramma's kunt u deze gegevens naar een Data Lake Store.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Web server-logboekgegevens (uploaden met behulp van aangepaste toepassingen)
Dit type dataset wordt specifiek genoemd, omdat analyse van de logboekgegevens van web server een algemene gebruiksvoorbeeld voor big data-toepassingen is en grote volumes van logboekbestanden wilt uploaden naar Data Lake Store is vereist. U kunt een van de volgende hulpprogramma's gebruiken om uw eigen scripts of toepassingen voor het uploaden van deze gegevens te schrijven.

* [Azure platformoverschrijdende CLI 2.0](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Data Lake Store .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)

Voor het uploaden van web server-logboekgegevens, en ook voor andere soorten gegevens (bijvoorbeeld sociale patronen gegevens) uploaden, is het een goede benadering van uw eigen aangepaste scripts/toepassingen schrijven omdat dit biedt u de flexibiliteit om op te nemen van uw gegevens uploaden onderdeel als onderdeel van uw groter big data-toepassing. In sommige gevallen kan deze code kan duren voordat de vorm van een script of een eenvoudig opdrachtregelprogramma. In andere gevallen kan de code big gegevensverwerking integreren in een zakelijke toepassing of oplossing worden gebruikt.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Gegevens die zijn gekoppeld aan Azure HDInsight-clusters
De meeste HDInsight-clustertypen (Hadoop, HBase, Storm) ondersteuning voor Data Lake Store als een gegevens-opslagplaats. HDInsight-clusters toegang tot gegevens uit Azure Storage BLOB's (WASB). Voor betere prestaties kunt u de gegevens uit WASB kopiëren naar een Data Lake Store-account die is gekoppeld aan het cluster. U kunt de volgende hulpprogramma's gebruiken om de gegevens te kopiëren.

* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy-Service](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md)

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Gegevens opgeslagen in de on-premises of IaaS Hadoop-clusters
Grote hoeveelheden gegevens kunnen worden opgeslagen in de bestaande Hadoop-clusters, lokaal op machines met HDFS. De Hadoop-clusters mogelijk in een on-premises implementatie of mogelijk binnen een IaaS-cluster in Azure. Er zijn vereisten dergelijke gegevens kopiëren naar Azure Data Lake Store voor een eenmalige benadering of op een terugkerende wijze. Er zijn verschillende opties die u gebruiken kunt om dit te bereiken. Hieronder volgt een lijst met alternatieven en de bijbehorende verschillen.

| Benadering | Details | Voordelen | Overwegingen |
| --- | --- | --- | --- |
| Azure Data Factory (ADF) gebruiken om gegevens rechtstreeks van Hadoop-clusters kopiëren naar Azure Data Lake Store |[ADF ondersteunt HDFS als een gegevensbron](../data-factory/connector-hdfs.md) |ADF biedt out-of-the-box-ondersteuning voor HDFS en eerste klas end-to-end-beheer en controle |Data Management Gateway in de IaaS cluster of de geïmplementeerde on-premises worden vereist |
| Gegevens exporteren uit Hadoop als bestanden. Kopieer de bestanden naar Azure Data Lake Store met geschikt mechanisme. |U kunt bestanden kopiëren voor het gebruik van Azure Data Lake Store: <ul><li>[Azure PowerShell voor Windows-besturingssysteem](data-lake-store-get-started-powershell.md)</li><li>[Azure platformoverschrijdende CLI 2.0 voor niet - Windows-besturingssysteem](data-lake-store-get-started-cli-2.0.md)</li><li>Aangepaste app met behulp van een Data Lake Store SDK</li></ul> |Snel aan de slag. Aangepaste uploads kunt doen |Meerdere stappen proces waarbij meerdere technologieën. Beheer en controle zal toenemen tot een uitdaging zijn gedurende een bepaalde periode vanwege de aangepaste aard van de hulpprogramma 's |
| Distcp gebruiken om gegevens van Hadoop kopiëren naar Azure Storage. Kopieer de gegevens uit Azure Storage naar Data Lake Store met geschikt mechanisme. |U kunt gegevens uit Azure Storage kopiëren voor het gebruik van Data Lake Store: <ul><li>[Azure Data Factory](../data-factory/copy-activity-overview.md)</li><li>[AdlCopy hulpprogramma](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Apache DistCp uitgevoerd op HDInsight-clusters](data-lake-store-copy-data-wasb-distcp.md)</li></ul> |U kunt de open-source hulpprogramma's gebruiken. |Meerdere stappen proces waarbij meerdere technologieën |

### <a name="really-large-datasets"></a>Grote gegevenssets
Voor het uploaden van gegevenssets die van meerdere terabytes variëren kan met behulp van de hierboven beschreven methoden soms worden langzaam en kostbaar. In dergelijke gevallen kunt kunt u de onderstaande opties gebruiken.

* **Met behulp van Azure ExpressRoute**. Azure ExpressRoute kunt u particuliere verbindingen maken tussen Azure-datacenters en infrastructuur on-premises. Dit biedt een betrouwbare optie voor het overbrengen van grote hoeveelheden gegevens. Zie voor meer informatie [Azure ExpressRoute documentatie](../expressroute/expressroute-introduction.md).
* **'Offline' het uploaden van gegevens**. Als u met behulp van Azure ExpressRoute is niet haalbaar om welke reden, kunt u [Azure Import/Export-service](../storage/common/storage-import-export-service.md) voor het verzenden van harde schijven met uw gegevens naar een Azure-Datacenter. Uw gegevens is eerst naar Azure Storage-Blobs geüpload. Vervolgens kunt u [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) of [AdlCopy hulpprogramma](data-lake-store-copy-data-azure-storage-blob.md) om gegevens te kopiëren van Azure Storage-Blobs naar Data Lake Store.

  > [!NOTE]
  > Bij met de Import/Export-service, de grootte op de schijven die u naar de Azure-Datacenter verzendt mag niet groter zijn dan 195 GB.
  >
  >

## <a name="process-data-stored-in-data-lake-store"></a>Verwerken van gegevens die zijn opgeslagen in Data Lake Store
Zodra de gegevens beschikbaar in Data Lake Store is kunt u de analyse van die gegevens met de ondersteunde big data-toepassingen uitvoeren. Op dit moment kunt u Azure HDInsight en Azure Data Lake Analytics gegevens analysis taken uitvoeren op gegevens die zijn opgeslagen in Data Lake Store.

![Gegevens analyseren in Data Lake Store](./media/data-lake-store-data-scenarios/analyze-data.png "analyseren van gegevens in Data Lake Store")

U kunt de volgende voorbeelden bekijken.

* [Een HDInsight-cluster maken met Data Lake Store als opslag](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

## <a name="download-data-from-data-lake-store"></a>Gegevens in Data Lake Store downloaden
U kunt ook moeten worden gedownload of verplaatsen van gegevens uit Azure Data Lake Store voor scenario's, zoals:

* Gegevens verplaatsen naar andere opslagplaatsen voor uw bestaande gegevensverwerking pijplijnen-interface. U wilt verplaatsen van gegevens van Data Lake Store met Azure SQL Database of op het lokale SQL Server.
* Het downloaden van gegevens naar uw lokale computer voor de verwerking van de IDE-omgevingen tijdens het bouwen van prototypen.

![Uitgaande gegevens van Data Lake Store](./media/data-lake-store-data-scenarios/egress-data.png "uitgaande gegevens van Data Lake Store")

In dergelijke gevallen kunt u een van de volgende opties gebruiken:

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)
* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)

U kunt ook de volgende methoden gebruiken om te schrijven van uw eigen scripttoepassing voor het downloaden van gegevens in Data Lake Store.

* [Azure platformoverschrijdende CLI 2.0](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Data Lake Store .NET SDK](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-store"></a>Visualiseer gegevens in Data Lake Store
U kunt een combinatie van services gebruiken voor het maken van visuele weergaven van gegevens die zijn opgeslagen in Data Lake Store.

![Gegevens in Data Lake Store visualiseren](./media/data-lake-store-data-scenarios/visualize-data.png "visualiseren van gegevens in Data Lake Store")

* U kunt starten met behulp van [Azure Data Factory om gegevens te verplaatsen van Data Lake Store met Azure SQL Data Warehouse](../data-factory/copy-activity-overview.md)
* Daarna kunt u [Power BI integreren met Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-integrate-power-bi.md) visuele representatie van de gegevens maken.
