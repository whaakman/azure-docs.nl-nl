---
title: Scenario's voor gegevens met betrekking tot Azure Data Lake Storage Gen2 | Microsoft Docs
description: Informatie over de verschillende scenario's en hulpprogramma's die in gegevens die u kunt die zijn opgenomen, verwerkt, gedownload en gevisualiseerd in Data Lake Storage Gen2 (voorheen bekend als Azure Data Lake Store)
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: normesta
ms.openlocfilehash: 1c50a6e14955b2c31222ff1317aa99ad28866ec8
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55864730"
---
# <a name="using-azure-data-lake-storage-gen2-for-big-data-requirements"></a>Met behulp van Azure Data Lake Storage Gen2 voor big data-vereisten

Er zijn vier belangrijke fasen in verwerking van big data:

* Grote hoeveelheden gegevens opnemen in een gegevensarchief, in realtime of in batches
* Verwerken van de gegevens
* De gegevens downloaden
* De gegevens te visualiseren

In dit artikel kijken we deze fasen met betrekking tot Azure Data Lake Storage Gen2 om te begrijpen van de mogelijkheden en hulpprogramma's beschikbaar om te voldoen aan de behoeften van uw big data.

## <a name="ingest-data-into-data-lake-storage-gen2"></a>Opnemen van gegevens in Data Lake Storage Gen2
In deze sectie worden de verschillende bronnen van gegevens en de verschillende manieren waarop die gegevens kan worden opgenomen in een Data Lake Storage Gen2-account.

![Opnemen van gegevens in Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/ingest-data.png "opnemen van gegevens in Data Lake Storage Gen2")

### <a name="ad-hoc-data"></a>Ad hoc-gegevens
Dit staat voor kleinere gegevenssets die worden gebruikt voor het maken van een prototype van een big data-toepassing. Er zijn verschillende manieren van opnemen ad-hoc gegevens afhankelijk van de bron van de gegevens.

| Gegevensbron | Met behulp van opnemen |
| --- | --- |
| Lokale computer |<ul> <li>[Storage Explorer](https://azure.microsoft.com/features/storage-explorer/)</ul> |
| Azure Storage Blob |<ul> <li>[Azure Data Factory](../../data-factory/connector-azure-data-lake-store.md)</li> <li>[AzCopy-hulpprogramma](../common/storage-use-azcopy-v10.md)</li><li>[DistCp die worden uitgevoerd op HDInsight-cluster](data-lake-storage-use-distcp.md)</li> </ul> |

### <a name="streamed-data"></a>Gestreamde gegevens
Dit verwijst naar gegevens die worden gegenereerd door verschillende bronnen, zoals toepassingen, apparaten, sensoren, enzovoort. Deze gegevens kan worden opgenomen in Data Lake Storage Gen2 door verschillende hulpprogramma's. Deze hulpprogramma's wordt meestal opnemen en verwerken van de gegevens op basis van door gebeurtenis in realtime, en schrijf vervolgens de gebeurtenissen in batches in Data Lake Storage Gen2 zodat ze verder kunnen worden verwerkt.

Hieronder vindt u hulpprogramma's die u kunt gebruiken:

* [Azure HDInsight Storm](../../hdinsight/storm/apache-storm-write-data-lake-store.md) -u kunt gegevens rechtstreeks naar Data Lake Storage Gen2 schrijven van de Storm-cluster.

### <a name="relational-data"></a>Relationele gegevens
U kunt ook brongegevens van relationele databases. Relationele databases verzamelen gedurende een bepaalde periode, enorme hoeveelheden gegevens die belangrijke inzichten bieden kan als verwerkt via een big data-pijplijn. U kunt de volgende hulpprogramma's gebruiken om te verplaatsen van dergelijke gegevens in Data Lake Storage Gen2.

* [Azure Data Factory](../../data-factory/copy-activity-overview.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Logboekgegevens voor web server (uploaden met behulp van aangepaste toepassingen)
Dit type gegevensset wordt specifiek genoemd, omdat het analyseren van logboekgegevens van web server is een gebruikelijk voor big data-toepassingen en grote hoeveelheden logboekbestanden worden geüpload naar Data Lake Storage Gen2 vereist. U kunt een van de volgende hulpprogramma's gebruiken om te schrijven van uw eigen scripts of toepassingen voor het uploaden van dergelijke gegevens.

* [Azure Data Factory](../../data-factory/copy-activity-overview.md)

Voor het uploaden van logboekgegevens van web server, en ook voor het uploaden van andere soorten gegevens (bijvoorbeeld sociale sentimenten gegevens), is het een goede benadering voor het schrijven van uw eigen aangepaste scripts/toepassingen omdat deze biedt u de flexibiliteit om op te nemen van uw gegevens onderdeel worden geüpload als onderdeel van de grotere big data-toepassing. In sommige gevallen kan deze code kan duren voordat de vorm van een script of een eenvoudig opdrachtregelprogramma. In andere gevallen kan de code worden gebruikt om de verwerking van big data integreren in een zakelijke toepassing of oplossing.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Gegevens die zijn gekoppeld met Azure HDInsight-clusters
Clustertypen meeste HDInsight (Hadoop, HBase, Storm) ondersteuning voor Data Lake Storage Gen2 als een gegevensopslagplaats voor opslag. HDInsight-clusters toegang tot gegevens uit Azure Storage Blobs (WASB). Voor betere prestaties kunt u de gegevens van WASB kopiëren naar een Data Lake Storage Gen2-account dat is gekoppeld aan het cluster. U kunt de volgende hulpprogramma's gebruiken om de gegevens te kopiëren.

* [Apache DistCp](data-lake-storage-use-distcp.md)
* [AzCopy-hulpprogramma](../common/storage-use-azcopy-v10.md)
* [Azure Data Factory](../../data-factory/connector-azure-data-lake-store.md)

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Gegevens die zijn opgeslagen in de on-premises of IaaS Hadoop clusters
Grote hoeveelheden gegevens kunnen worden opgeslagen in bestaande Hadoop-clusters, lokaal op virtuele machines met HDFS. De Hadoop-clusters mogelijk in een on-premises implementatie of mogelijk binnen een cluster IaaS op Azure. Kunnen er vereisten voor dergelijke gegevens kopiëren naar Azure Data Lake Storage Gen2 voor een eenmalige benadering of in een terugkerende manier herbruikbaar zijn. Er zijn verschillende opties die u gebruiken kunt om dit te bereiken. Hieronder volgt een lijst van alternatieven en de bijbehorende wisselwerking.

| Methode | Details | Voordelen | Overwegingen |
| --- | --- | --- | --- |
| Azure Data Factory (ADF) gebruiken om te kopiëren van gegevens rechtstreeks vanuit de Hadoop-clusters op Azure Data Lake Storage Gen2 |[ADF ondersteuning biedt voor HDFS als een gegevensbron](../../data-factory/connector-hdfs.md) |ADF biedt out-of-the-box-ondersteuning voor HDFS en eersteklas end-to-end-beheer en bewaking |Data Management Gateway worden on-premises geïmplementeerd of in de IaaS-cluster is vereist |
| Distcp gebruiken om gegevens te kopiëren uit Hadoop met Azure Storage. Gegevens kopiëren van Azure-opslag naar Data Lake Storage Gen2 met geschikt mechanisme. |U kunt gegevens kopiëren van Azure Storage voor het gebruik van Data Lake Storage Gen2: <ul><li>[Azure Data Factory](../../data-factory/copy-activity-overview.md)</li><li>[AzCopy-hulpprogramma](../common/storage-use-azcopy-v10.md)</li><li>[Apache DistCp die worden uitgevoerd op HDInsight-clusters](data-lake-storage-use-distcp.md)</li></ul> |U kunt de open-source hulpprogramma's gebruiken. |WebTest met meerdere stappen proces dat bestaat uit meerdere technologieën |

### <a name="really-large-datasets"></a>Grote gegevenssets
Voor het uploaden van gegevenssets die van meerdere terabytes variëren, kan met behulp van de hierboven beschreven methoden soms zijn traag en duur. In dergelijke gevallen kunt u de onderstaande opties.

* **Met behulp van Azure ExpressRoute**. Azure ExpressRoute kunt u particuliere verbindingen maken tussen Azure-datacenters en infrastructuur op uw locatie. Dit biedt een betrouwbare optie voor het overbrengen van grote hoeveelheden gegevens. Zie voor meer informatie, [documentatie voor Azure ExpressRoute](../../expressroute/expressroute-introduction.md).

## <a name="process-data-stored-in-data-lake-storage-gen2"></a>Gegevens die zijn opgeslagen in Data Lake Storage Gen2 verwerken
Zodra de gegevens beschikbaar in Data Lake Storage Gen2 zijn kunt u analyses uitvoeren op gegevens met behulp van de ondersteunde big data-toepassingen. Op dit moment kunt u Azure HDInsight en Azure Databricks data analysis-taken uitvoeren op de gegevens die zijn opgeslagen in Data Lake Storage Gen2.

![Gegevens analyseren in Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/analyze-data.png "analyseren van gegevens in Data Lake Storage Gen2")

Zie voor een voorbeeld [gebruik Azure Data Lake Storage Gen2 met Azure HDInsight-clusters](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2).


## <a name="download-data-from-data-lake-storage-gen2"></a>Gegevens uit Data Lake Storage Gen2 downloaden
U kunt ook om te downloaden of verplaatsen van gegevens uit Azure Data Lake Storage Gen2 voor scenario's zoals:

* Gegevens verplaatsen naar andere opslagplaatsen voor interactie met uw bestaande pijplijnen voor gegevensverwerking. Bijvoorbeeld, als u wilt gegevens van Data Lake Storage Gen2 verplaatsen naar Azure SQL Database- of on-premises SQL Server.
* Gegevens downloaden naar uw lokale computer voor verwerking in de IDE-omgevingen tijdens het maken van prototypen van de toepassing.

![Uitgaand verkeer van gegevens van Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/egress-data.png "uitgaand verkeer van gegevens van Data Lake Storage Gen2")

In dergelijke gevallen kunt u een van de volgende opties gebruiken:

* [Azure Data Factory](../../data-factory/copy-activity-overview.md)
* [Apache DistCp](data-lake-storage-use-distcp.md)

## <a name="visualize-data-in-data-lake-storage-gen2"></a>Gegevens visualiseren in Data Lake Storage Gen2
U kunt een combinatie van services gebruiken om te maken van visuele weergaven van gegevens die zijn opgeslagen in Data Lake Storage Gen2.

![Gegevens visualiseren in Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/visualize-data.png "visualiseren van gegevens in Data Lake Storage Gen2")

* U kunt beginnen met behulp van [Azure Data Factory om gegevens te verplaatsen van Data Lake Storage Gen2 naar Azure SQL Data Warehouse](../../data-factory/copy-activity-overview.md)
* Hierna kunt u [Power BI integreren met Azure SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-get-started-visualize-with-power-bi.md) visuele weergave van de gegevens maken.
