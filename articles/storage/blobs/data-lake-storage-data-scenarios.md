---
title: Scenario's voor gegevens met betrekking tot Azure Data Lake Storage Gen2 | Microsoft Docs
description: Informatie over de verschillende scenario's en hulpprogramma's die in gegevens die u kunt die zijn opgenomen, verwerkt, gedownload en gevisualiseerd in Data Lake Storage Gen2 (voorheen bekend als Azure Data Lake Store)
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: normesta
ms.openlocfilehash: ec97cf903a37465f98007750fc41edc0952ab3a6
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56889593"
---
# <a name="using-azure-data-lake-storage-gen2-for-big-data-requirements"></a>Met behulp van Azure Data Lake Storage Gen2 voor big data-vereisten

Er zijn vier belangrijke fasen in verwerking van big data:

> [!div class="checklist"]
> * Grote hoeveelheden gegevens opnemen in een gegevensarchief, in realtime of in batches
> * Verwerken van de gegevens
> * De gegevens downloaden
> * De gegevens te visualiseren

Beginnen met het maken van een storage-account en een bestandssysteem. Vervolgens toegang verlenen tot de gegevens. De eerste paar secties van dit artikel kunnen u deze taken. In de resterende secties we leggen de nadruk de mogelijkheden en hulpprogramma's voor elke fase van de verwerking.

## <a name="create-a-data-lake-storage-gen2-account"></a>Een Data Lake Storage Gen2-account maken

Een Data Lake Storage Gen2 is een opslagaccount waarvoor een hiërarchische naamruimte. 

Zie voor het maken van een [Quick Start: Maken van een storage-account van Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-quickstart-create-account?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="create-a-file-system"></a>Een bestandssysteem maken

Een *bestandssysteem* is een container voor bestanden en mappen. U moet ten minste één van deze om te beginnen met het ophalen van gegevens in uw opslagaccount.  Hier volgt een lijst met hulpprogramma's die u gebruiken kunt om ze te maken.

|Hulpprogramma | Richtlijnen |
|---|--|
|Azure Opslagverkenner | [Een bestandssysteem maken met behulp van Storage Explorer](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-explorer#create-a-filesystem) |
|AzCopy | [Maken van een Blob-container of bestandsshare met behulp van AzCopyV10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-a-blob-container-or-file-share)|
|Hadoop-bestand System (HDFS) Command Line Interface (CLI) met HDInsight |[Een bestandssysteem maken met behulp van HDFS met HDInsight](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-use-hdfs-data-lake-storage?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-a-file-system) |
|Code in een Azure Databricks-Notebook|[Een bestandssysteem (Scala) van een storage-account maken](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-quickstart-create-databricks-account?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-storage-account-file-system) <br><br> [Maken van een bestandssysteem en koppel deze (Python)](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-use-databricks-spark?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-a-file-system-and-mount-it)|

Het gemakkelijkst bestandssystemen te maken met behulp van Storage Explorer of AzCopy. Het duurt iets meer werk bestandssystemen met behulp van HDInsight en Databricks te maken. Echter, als u van plan bent gebruik van HDInsight of Databricks-clusters voor het verwerken van uw gegevens toch, vervolgens u kunt eerst uw clusters maken en de HDFS-CLI gebruiken voor uw bestandssystemen maken.  

## <a name="grant-access-to-the-data"></a>Toegang verlenen tot de gegevens

De juiste toegangsmachtigingen tot uw account en de gegevens in uw account instellen voordat u begint met het ophalen van gegevens.

Er is drie manieren om toegang te verlenen:

* Een van deze rollen toewijzen aan een gebruiker, groep, gebruiker beheerde identiteit of service-principal:

  [Gegevenslezer voor Opslagblob](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader-preview)

  [Gegevensbijdrager voor Blob](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor-preview)

  [De eigenaar van een opslag-Blob-gegevens](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner-preview)

* Een Shared Access Signature (SAS)-token gebruiken.

* Gebruik een sleutel van het opslagaccount.

Deze tabel laat zien hoe toegangsmachtigingen voor elke Azure-service of hulpprogramma.

|Hulpprogramma | Om toegang te verlenen | Richtlijnen |
|---|--|---|
|Opslagverkenner| Een rol toewijzen aan gebruikers en groepen | [De beheerder en niet-beheerder rollen toewijzen aan gebruikers met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal) |
|AzCopy| Een rol toewijzen aan gebruikers en groepen <br>**or**<br> Een SAS-token gebruiken| [De beheerder en niet-beheerder rollen toewijzen aan gebruikers met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal)<br><br>[Eenvoudig maken een SAS voor het downloaden van een bestand vanuit Azure Storage – met Azure Storage Explorer](https://blogs.msdn.microsoft.com/jpsanders/2017/10/12/easily-create-a-sas-to-download-a-file-from-azure-storage-using-azure-storage-explorer/)|
|Apache DistCp | Een rol toewijzen aan een gebruiker toegewezen beheerde identiteit | [Het maken van een HDInsight-cluster met Data Lake Storage Gen2](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2) |
|Azure Data Factory| Een rol toewijzen aan een gebruiker toegewezen-beheerde identiteit<br>**or**<br> Een rol is een service-principal toewijzen<br>**or**<br> Gebruik van een opslagaccountsleutel | [Eigenschappen van de gekoppelde service](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#linked-service-properties) |
|Azure HDInsight| Een rol toewijzen aan een gebruiker toegewezen beheerde identiteit | [Het maken van een HDInsight-cluster met Data Lake Storage Gen2](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2)|
|Azure Databricks| Een rol toewijzen aan een service-principal | [Procedure: De portal gebruiken om een Azure AD-toepassing en service-principal die toegang hebben tot resources te maken](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)|

Als u wilt toegang verlenen tot specifieke bestanden en mappen, Zie de volgende artikelen.

* [Bestands- en machtigingen op het niveau met behulp van Azure Storage Explorer met de Azure Data Lake Storage Gen2 instellen](https://review.docs.microsoft.com/azure/storage/blobs/data-lake-storage-how-to-set-permissions-storage-explorer)

* [Toegangsbeheerlijsten voor bestanden en mappen](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control#access-control-lists-on-files-and-directories)

Zie voor meer informatie over het instellen van andere aspecten van beveiliging, [beveiligingshandleiding voor Azure Data Lake Storage Gen2](https://review.docs.microsoft.com/azure/storage/common/storage-data-lake-storage-security-guide?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="ingest-the-data"></a>De gegevens opnemen

In deze sectie worden de verschillende bronnen van gegevens en de verschillende manieren waarop die gegevens kan worden opgenomen in een Data Lake Storage Gen2-account.

![Opnemen van gegevens in Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/ingest-data.png "opnemen van gegevens in Data Lake Storage Gen2")

### <a name="ad-hoc-data"></a>Ad hoc-gegevens

Dit staat voor kleinere gegevenssets die worden gebruikt voor het maken van een prototype van een big data-toepassing. Er zijn verschillende manieren van opnemen ad-hoc gegevens afhankelijk van de bron van de gegevens. 

Hier volgt een lijst met hulpprogramma's die u gebruiken kunt voor opname van ad-hoc gegevens.

| Gegevensbron | Met behulp van opnemen |
| --- | --- |
| Lokale computer |[Storage Explorer](https://azure.microsoft.com/features/storage-explorer/)<br><br>[AzCopy-hulpprogramma](../common/storage-use-azcopy-v10.md)|
| Azure Storage Blob |[Azure Data Factory](../../data-factory/connector-azure-data-lake-store.md)<br><br>[AzCopy-hulpprogramma](../common/storage-use-azcopy-v10.md)<br><br>[DistCp die worden uitgevoerd op HDInsight-cluster](data-lake-storage-use-distcp.md)|

### <a name="streamed-data"></a>Gestreamde gegevens

Dit verwijst naar gegevens die worden gegenereerd door verschillende bronnen, zoals toepassingen, apparaten, sensoren, enzovoort. Deze gegevens kan worden opgenomen in Data Lake Sorage Gen2 door verschillende hulpprogramma's. Deze hulpprogramma's wordt meestal opnemen en verwerken van de gegevens op basis van door gebeurtenis in realtime, en schrijf vervolgens de gebeurtenissen in batches in Data Lake Storage Gen2 zodat ze verder kunnen worden verwerkt.

Hier volgt een lijst met hulpprogramma's die u gebruiken kunt om op te nemen van gestreamde gegevens.

|Hulpprogramma | Richtlijnen |
|---|--|
|Azure HDInsight Storm | [Schrijven naar Apache Hadoop HDFS van Apache Storm op HDInsight](https://docs.microsoft.com/azure/hdinsight/storm/apache-storm-write-data-lake-store) |

### <a name="relational-data"></a>Relationele gegevens

U kunt ook brongegevens van relationele databases. Relationele databases verzamelen gedurende een bepaalde periode, enorme hoeveelheden gegevens die belangrijke inzichten bieden kan als verwerkt via een big data-pijplijn. U kunt de volgende hulpprogramma's gebruiken om te verplaatsen van dergelijke gegevens in Data Lake Storage Gen2.

Hier volgt een lijst met hulpprogramma's die u gebruiken kunt om op te nemen van relationele gegevens.

|Hulpprogramma | Richtlijnen |
|---|--|
|Azure Data Factory | [De Kopieeractiviteit in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/copy-activity-overview) |

### <a name="web-server-log-data-upload-using-custom-applications"></a>Logboekgegevens voor web server (uploaden met behulp van aangepaste toepassingen)

Dit type gegevensset wordt specifiek genoemd, omdat het analyseren van logboekgegevens van web server is een gebruikelijk voor big data-toepassingen en grote hoeveelheden logboekbestanden worden geüpload naar Data Lake Storage Gen2 vereist. U kunt een van de volgende hulpprogramma's gebruiken om te schrijven van uw eigen scripts of toepassingen voor het uploaden van dergelijke gegevens.

Hier volgt een lijst met hulpprogramma's die u gebruiken kunt voor het Web server-logboekgegevens opnemen.

|Hulpprogramma | Richtlijnen |
|---|--|
|Azure Data Factory | [De Kopieeractiviteit in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/copy-activity-overview)  |

Voor het uploaden van logboekgegevens van web server, en ook voor het uploaden van andere soorten gegevens (bijvoorbeeld sociale sentimenten gegevens), is het een goede benadering voor het schrijven van uw eigen aangepaste scripts/toepassingen omdat deze biedt u de flexibiliteit om op te nemen van uw gegevens onderdeel worden geüpload als onderdeel van de grotere big data-toepassing. In sommige gevallen kan deze code kan duren voordat de vorm van een script of een eenvoudig opdrachtregelprogramma. In andere gevallen kan de code worden gebruikt om de verwerking van big data integreren in een zakelijke toepassing of oplossing.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Gegevens die zijn gekoppeld met Azure HDInsight-clusters

Clustertypen meeste HDInsight (Hadoop, HBase, Storm) ondersteuning voor Data Lake Storage Gen2 als een gegevensopslagplaats voor opslag. HDInsight-clusters toegang tot gegevens uit Azure Storage Blobs (WASB). Voor betere prestaties kunt u de gegevens van WASB kopiëren naar een Data Lake Storage Gen2-account dat is gekoppeld aan het cluster. U kunt de volgende hulpprogramma's gebruiken om de gegevens te kopiëren.

Hier volgt een lijst met hulpprogramma's die u gebruiken kunt voor opname van gegevens die zijn gekoppeld aan het HDInsight-clusters.

|Hulpprogramma | Richtlijnen |
|---|--|
|Apache DistCp | [DistCp gebruiken om gegevens tussen Azure Storage-Blobs en Azure Data Lake Storage Gen2 te kopiëren](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-use-distcp) |
|AzCopy-hulpprogramma | [Gegevens overdragen met het AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10) |
|Azure Data Factory | [Gegevens kopiëren naar of van Azure Data Lake Storage Gen1 met behulp van Azure Data Factory](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-store) |

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Gegevens die zijn opgeslagen in de on-premises of IaaS Hadoop clusters

Grote hoeveelheden gegevens kunnen worden opgeslagen in bestaande Hadoop-clusters, lokaal op virtuele machines met HDFS. De Hadoop-clusters mogelijk in een on-premises implementatie of mogelijk binnen een cluster IaaS op Azure. Kunnen er vereisten voor dergelijke gegevens kopiëren naar Azure Data Lake Storage Gen2 voor een eenmalige benadering of in een terugkerende manier herbruikbaar zijn. Er zijn verschillende opties die u gebruiken kunt om dit te bereiken. Hieronder volgt een lijst van alternatieven en de bijbehorende wisselwerking.

| Methode | Details | Voordelen | Overwegingen |
| --- | --- | --- | --- |
| Azure Data Factory (ADF) gebruiken om te kopiëren van gegevens rechtstreeks vanuit de Hadoop-clusters op Azure Data Lake Storage Gen2 |[ADF ondersteuning biedt voor HDFS als een gegevensbron](../../data-factory/connector-hdfs.md) |ADF biedt out-of-the-box-ondersteuning voor HDFS en eersteklas end-to-end-beheer en bewaking |Data Management Gateway worden on-premises geïmplementeerd of in de IaaS-cluster is vereist |
| Distcp gebruiken om gegevens te kopiëren uit Hadoop met Azure Storage. Gegevens kopiëren van Azure-opslag naar Data Lake Storage Gen2 met geschikt mechanisme. |U kunt gegevens kopiëren van Azure Storage voor het gebruik van Data Lake Storage Gen2: <ul><li>[Azure Data Factory](../../data-factory/copy-activity-overview.md)</li><li>[AzCopy-hulpprogramma](../common/storage-use-azcopy-v10.md)</li><li>[Apache DistCp die worden uitgevoerd op HDInsight-clusters](data-lake-storage-use-distcp.md)</li></ul> |U kunt de open-source hulpprogramma's gebruiken. |WebTest met meerdere stappen proces dat bestaat uit meerdere technologieën |

### <a name="really-large-datasets"></a>Grote gegevenssets

Voor het uploaden van gegevenssets die van meerdere terabytes variëren, kan met behulp van de hierboven beschreven methoden soms zijn traag en duur. In dergelijke gevallen kunt u Azure ExpressRoute.  

Azure ExpressRoute kunt u particuliere verbindingen maken tussen Azure-datacenters en infrastructuur op uw locatie. Dit biedt een betrouwbare optie voor het overbrengen van grote hoeveelheden gegevens. Zie voor meer informatie, [documentatie voor Azure ExpressRoute](../../expressroute/expressroute-introduction.md).

## <a name="process-the-data"></a>De gegevens te verwerken

Zodra de gegevens beschikbaar in Data Lake Storage Gen2 zijn kunt u analyses uitvoeren op gegevens met behulp van de ondersteunde big data-toepassingen. 

![Gegevens analyseren in Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/analyze-data.png "analyseren van gegevens in Data Lake Storage Gen2")

Hier volgt een lijst met hulpprogramma's die u kunt gegevens analysis-taken uitvoeren op gegevens die zijn opgeslagen in Data Lake Storage Gen2.

|Hulpprogramma | Richtlijnen |
|---|--|
|Azure HDInsight | [Azure Data Lake Storage Gen2 gebruiken met Azure HDInsight-clusters](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2) |
|Azure Databricks | [Azure Data Lake Storage Gen2](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html)<br><br>[Snelstart: Gegevens in Azure Data Lake Storage Gen2 analyseren met behulp van Azure Databricks](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-quickstart-create-databricks-account?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)<br><br>[Zelfstudie: Uitpakken, transformeren en laden van gegevens met behulp van Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/databricks-extract-load-sql-data-warehouse?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|

## <a name="visualize-the-data"></a>De gegevens visualiseren

U kunt een combinatie van services gebruiken om te maken van visuele weergaven van gegevens die zijn opgeslagen in Data Lake Storage Gen2.

![Gegevens visualiseren in Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/visualize-data.png "visualiseren van gegevens in Data Lake Storage Gen2")

* U kunt beginnen met behulp van [Azure Data Factory om gegevens te verplaatsen van Data Lake Storage Gen2 naar Azure SQL Data Warehouse](../../data-factory/copy-activity-overview.md)
* Hierna kunt u [Power BI integreren met Azure SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-get-started-visualize-with-power-bi.md) visuele weergave van de gegevens maken.

## <a name="download-the-data"></a>De gegevens downloaden

U kunt ook om te downloaden of verplaatsen van gegevens uit Azure Data Lake Storage Gen2 voor scenario's zoals:

* Gegevens verplaatsen naar andere opslagplaatsen voor interactie met uw bestaande pijplijnen voor gegevensverwerking. Bijvoorbeeld, als u wilt gegevens van Data Lake Storage Gen2 verplaatsen naar Azure SQL Database- of on-premises SQL Server.

* Gegevens downloaden naar uw lokale computer voor verwerking in de IDE-omgevingen tijdens het maken van prototypen van de toepassing.

![Uitgaand verkeer van gegevens van Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/egress-data.png "uitgaand verkeer van gegevens van Data Lake Storage Gen2")

Hier volgt een lijst met hulpprogramma's die u gebruiken kunt om gegevens te downloaden van Data Lake Storage Gen2.

|Hulpprogramma | Richtlijnen |
|---|--|
|Azure Data Factory | [De Kopieeractiviteit in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/copy-activity-overview) |
|Apache DistCop | [DistCp gebruiken om gegevens tussen Azure Storage-Blobs en Azure Data Lake Storage Gen2 te kopiëren](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-use-distcp) |
