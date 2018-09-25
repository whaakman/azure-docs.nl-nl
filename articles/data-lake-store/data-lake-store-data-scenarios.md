---
title: Scenario's voor gegevens met betrekking tot Data Lake Storage Gen1 | Microsoft Docs
description: Informatie over de verschillende scenario's en hulpprogramma's die in gegevens die u kunt die zijn opgenomen, verwerkt, gedownload en gevisualiseerd in Data Lake Storage Gen1 (voorheen bekend als Azure Data Lake Store)
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: 64c7985508ed7f03b32340cbb2c78de61242f7e1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46984276"
---
# <a name="using-azure-data-lake-storage-gen1-for-big-data-requirements"></a>Met behulp van Azure Data Lake Storage Gen1 voor big data-vereisten

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Er zijn vier belangrijke fasen in verwerking van big data:

* Grote hoeveelheden gegevens opnemen in een gegevensarchief, in realtime of in batches
* Verwerken van de gegevens
* De gegevens downloaden
* De gegevens te visualiseren

In dit artikel kijken we deze fasen met betrekking tot Azure Data Lake Storage Gen1 om te begrijpen van de mogelijkheden en hulpprogramma's beschikbaar om te voldoen aan de behoeften van uw big data.

## <a name="ingest-data-into-data-lake-storage-gen1"></a>Opnemen van gegevens in Data Lake Storage Gen1
In deze sectie worden de verschillende bronnen van gegevens en de verschillende manieren waarop deze gegevens kan worden opgenomen in een Gen1 van Data Lake Storage-account.

![Opnemen van gegevens in Data Lake Storage Gen1](./media/data-lake-store-data-scenarios/ingest-data.png "opnemen van gegevens in Data Lake Storage Gen1")

### <a name="ad-hoc-data"></a>Ad hoc-gegevens
Dit staat voor kleinere gegevenssets die worden gebruikt voor het maken van een prototype van een big data-toepassing. Er zijn verschillende manieren van opnemen ad-hoc gegevens afhankelijk van de bron van de gegevens.

| Gegevensbron | Met behulp van opnemen |
| --- | --- |
| Lokale computer |<ul> <li>[Azure Portal](data-lake-store-get-started-portal.md)</li> <li>[Azure PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Azure-CLI](data-lake-store-get-started-cli-2.0.md)</li> <li>[Met behulp van Data Lake Tools voor Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md) </li></ul> |
| Azure Storage-Blob |<ul> <li>[Azure Data Factory](../data-factory/connector-azure-data-lake-store.md)</li> <li>[AdlCopy hulpprogramma](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[DistCp die worden uitgevoerd op HDInsight-cluster](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

### <a name="streamed-data"></a>Gestreamde gegevens
Dit verwijst naar gegevens die worden gegenereerd door verschillende bronnen, zoals toepassingen, apparaten, sensoren, enzovoort. Deze gegevens kan worden opgenomen in Data Lake Storage Gen1 met tal van hulpprogramma's. Deze hulpprogramma's wordt meestal opnemen en verwerken van de gegevens op basis van door gebeurtenis in realtime, en schrijf vervolgens de gebeurtenissen in batches in Data Lake Storage Gen1 zodat ze verder kunnen worden verwerkt.

Hieronder vindt u hulpprogramma's die u kunt gebruiken:

* [Azure Stream Analytics](../stream-analytics/stream-analytics-data-lake-output.md) -gebeurtenissen die zijn opgenomen in Event Hubs kunnen worden geschreven naar Azure Data Lake Storage Gen1 met behulp van een Azure Data Lake Storage Gen1-uitvoer.
* [Azure HDInsight Storm](../hdinsight/storm/apache-storm-write-data-lake-store.md) -u kunt gegevens rechtstreeks naar Data Lake Storage Gen1 schrijven van de Storm-cluster.
* [EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md) – u kunt het ontvangen van gebeurtenissen van Event Hubs en dit vervolgens te schrijven naar Data Lake Storage Gen1 met behulp van de [.NET-SDK voor Data Lake Storage Gen1](data-lake-store-get-started-net-sdk.md).

### <a name="relational-data"></a>Relationele gegevens
U kunt ook brongegevens van relationele databases. Relationele databases verzamelen gedurende een bepaalde periode, enorme hoeveelheden gegevens die belangrijke inzichten bieden kan als verwerkt via een big data-pijplijn. U kunt de volgende hulpprogramma's gebruiken om te verplaatsen van dergelijke gegevens in Data Lake Storage Gen1.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Logboekgegevens voor web server (uploaden met behulp van aangepaste toepassingen)
Dit type gegevensset wordt specifiek genoemd, omdat het analyseren van logboekgegevens van web server is een gebruikelijk voor big data-toepassingen en grote hoeveelheden logboekbestanden worden geüpload naar Data Lake Storage Gen1 vereist. U kunt een van de volgende hulpprogramma's gebruiken om te schrijven van uw eigen scripts of toepassingen voor het uploaden van dergelijke gegevens.

* [Azure-CLI](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Data Lake Storage Gen1 .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)

Voor het uploaden van logboekgegevens van web server, en ook voor het uploaden van andere soorten gegevens (bijvoorbeeld sociale sentimenten gegevens), is het een goede benadering voor het schrijven van uw eigen aangepaste scripts/toepassingen omdat deze biedt u de flexibiliteit om op te nemen van uw gegevens onderdeel worden geüpload als onderdeel van de grotere big data-toepassing. In sommige gevallen kan deze code kan duren voordat de vorm van een script of een eenvoudig opdrachtregelprogramma. In andere gevallen kan de code worden gebruikt om de verwerking van big data integreren in een zakelijke toepassing of oplossing.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Gegevens die zijn gekoppeld met Azure HDInsight-clusters
Clustertypen meeste HDInsight (Hadoop, HBase, Storm) ondersteuning voor Data Lake Storage Gen1 als een gegevensopslagplaats voor opslag. HDInsight-clusters toegang tot gegevens uit Azure Storage Blobs (WASB). Voor betere prestaties kunt u de gegevens van WASB kopiëren naar een Gen1 van Data Lake Storage-account dat is gekoppeld aan het cluster. U kunt de volgende hulpprogramma's gebruiken om de gegevens te kopiëren.

* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy Service](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md)

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Gegevens die zijn opgeslagen in de on-premises of IaaS Hadoop clusters
Grote hoeveelheden gegevens kunnen worden opgeslagen in bestaande Hadoop-clusters, lokaal op virtuele machines met HDFS. De Hadoop-clusters mogelijk in een on-premises implementatie of mogelijk binnen een cluster IaaS op Azure. Kunnen er vereisten voor dergelijke gegevens kopiëren naar Azure Data Lake Storage Gen1 voor een eenmalige benadering of in een terugkerende manier herbruikbaar zijn. Er zijn verschillende opties die u gebruiken kunt om dit te bereiken. Hieronder volgt een lijst van alternatieven en de bijbehorende wisselwerking.

| Methode | Details | Voordelen | Overwegingen |
| --- | --- | --- | --- |
| Azure Data Factory (ADF) gebruiken om te kopiëren van gegevens rechtstreeks vanuit de Hadoop-clusters op Azure Data Lake Storage Gen1 |[ADF ondersteuning biedt voor HDFS als een gegevensbron](../data-factory/connector-hdfs.md) |ADF biedt out-of-the-box-ondersteuning voor HDFS en eersteklas end-to-end-beheer en bewaking |Data Management Gateway worden on-premises geïmplementeerd of in de IaaS-cluster is vereist |
| Gegevens exporteren uit Hadoop als bestanden. Kopieer de bestanden naar Azure Data Lake Storage Gen1 met behulp van de juiste mechanisme. |U kunt bestanden voor het gebruik van Azure Data Lake Storage Gen1 kopiëren: <ul><li>[Azure PowerShell voor Windows-besturingssysteem](data-lake-store-get-started-powershell.md)</li><li>[Azure-CLI](data-lake-store-get-started-cli-2.0.md)</li><li>Aangepaste app met behulp van een Data Lake Storage Gen1 SDK</li></ul> |Snel aan de slag. Aangepaste uploads kunt doen |WebTest met meerdere stappen proces dat bestaat uit meerdere technologieën. Beheer en controle neemt toe en wordt een uitdaging zijn na verloop van tijd vanwege de aangepaste aard van de hulpprogramma 's |
| Distcp gebruiken om gegevens te kopiëren uit Hadoop met Azure Storage. Gegevens kopiëren van Azure-opslag naar Data Lake Storage Gen1 met geschikt mechanisme. |U kunt gegevens kopiëren van Azure Storage voor het gebruik van Data Lake Storage Gen1: <ul><li>[Azure Data Factory](../data-factory/copy-activity-overview.md)</li><li>[AdlCopy hulpprogramma](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Apache DistCp die worden uitgevoerd op HDInsight-clusters](data-lake-store-copy-data-wasb-distcp.md)</li></ul> |U kunt de open-source hulpprogramma's gebruiken. |WebTest met meerdere stappen proces dat bestaat uit meerdere technologieën |

### <a name="really-large-datasets"></a>Grote gegevenssets
Voor het uploaden van gegevenssets die van meerdere terabytes variëren, kan met behulp van de hierboven beschreven methoden soms zijn traag en duur. In dergelijke gevallen kunt u de onderstaande opties.

* **Met behulp van Azure ExpressRoute**. Azure ExpressRoute kunt u particuliere verbindingen maken tussen Azure-datacenters en infrastructuur op uw locatie. Dit biedt een betrouwbare optie voor het overbrengen van grote hoeveelheden gegevens. Zie voor meer informatie, [documentatie voor Azure ExpressRoute](../expressroute/expressroute-introduction.md).
* **'Offline' uploaden van gegevens**. Als u met behulp van Azure ExpressRoute niet haalbaar is om een bepaalde reden, kunt u [Azure Import/Export-service](../storage/common/storage-import-export-service.md) voor verzending van harde schijven met uw gegevens om een Azure-Datacenter. Uw gegevens eerst geüpload naar Azure Storage-Blobs. Vervolgens kunt u [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) of [AdlCopy hulpprogramma](data-lake-store-copy-data-azure-storage-blob.md) gegevens uit Azure Storage-Blobs kopiëren naar Data Lake Storage Gen1.

  > [!NOTE]
  > Hoewel met de Import/Export-service, de grootte van de schijven die u naar Azure-Datacenter verzendt niet groter zijn dan 195 GB moet.
  >
  >

## <a name="process-data-stored-in-data-lake-storage-gen1"></a>Gegevens die zijn opgeslagen in Data Lake Storage Gen1 verwerken
Zodra de gegevens beschikbaar in Data Lake Storage Gen1 zijn kunt u analyses uitvoeren op gegevens met behulp van de ondersteunde big data-toepassingen. Op dit moment kunt u Azure HDInsight en Azure Data Lake Analytics data analysis-taken uitvoeren op de gegevens die zijn opgeslagen in Data Lake Storage Gen1.

![Gegevens analyseren in Data Lake Storage Gen1](./media/data-lake-store-data-scenarios/analyze-data.png "analyseren van gegevens in Data Lake Storage Gen1")

U kunt de volgende voorbeelden bekijken.

* [Een HDInsight-cluster maken met Data Lake Storage Gen1 als opslag](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Storage Gen1](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

## <a name="download-data-from-data-lake-storage-gen1"></a>Gegevens uit Data Lake Storage Gen1 downloaden
U kunt ook om te downloaden of verplaatsen van gegevens uit Azure Data Lake Storage Gen1 voor scenario's zoals:

* Gegevens verplaatsen naar andere opslagplaatsen voor interactie met uw bestaande pijplijnen voor gegevensverwerking. Bijvoorbeeld, als u wilt gegevens van Data Lake Storage Gen1 verplaatsen naar Azure SQL Database- of on-premises SQL Server.
* Gegevens downloaden naar uw lokale computer voor verwerking in de IDE-omgevingen tijdens het maken van prototypen van de toepassing.

![Uitgaand verkeer van gegevens van Data Lake Storage Gen1](./media/data-lake-store-data-scenarios/egress-data.png "uitgaand verkeer van gegevens van Data Lake Storage Gen1")

In dergelijke gevallen kunt u een van de volgende opties gebruiken:

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)
* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)

U kunt ook de volgende methoden gebruiken om te schrijven van uw eigen script/toepassing om te downloaden van gegevens van Data Lake Storage Gen1.

* [Azure-CLI](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Data Lake Storage Gen1 .NET SDK](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-storage-gen1"></a>Gegevens visualiseren in Data Lake Storage Gen1
Een combinatie van services kunt u visuele weergaven van gegevens die zijn opgeslagen in Data Lake Storage Gen1 maken.

![Gegevens visualiseren in Data Lake Storage Gen1](./media/data-lake-store-data-scenarios/visualize-data.png "visualiseren van gegevens in Data Lake Storage Gen1")

* U kunt beginnen met behulp van [Azure Data Factory om gegevens te verplaatsen van Data Lake Storage Gen1 met Azure SQL Data Warehouse](../data-factory/copy-activity-overview.md)
* Hierna kunt u [Power BI integreren met Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-visualize-with-power-bi.md) visuele weergave van de gegevens maken.
