---
title: Data Lake Store integreren met andere Azure-Services | Microsoft Docs
description: Begrijpen hoe Data Lake Store integreert met andere Azure-services
documentationcenter: 
services: data-lake-store
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 48a5d1f4-3850-4c22-bbc4-6d1d394fba8a
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: d43459b900232612d83506438e6a70daa893eb80
ms.sourcegitcommit: 651a6fa44431814a42407ef0df49ca0159db5b02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="integrating-data-lake-store-with-other-azure-services"></a>Data Lake Store met andere Azure-services integreren
Azure Data Lake Store kan worden gebruikt in combinatie met andere Azure-services voor een breed scala aan scenario's. Het volgende artikel worden de services die Data Lake Store kan worden geïntegreerd met.

## <a name="use-data-lake-store-with-azure-hdinsight"></a>Gebruik Data Lake Store met Azure HDInsight
U kunt inrichten een [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) cluster dat gebruik maakt van Data Lake Store als de HDFS-compatibele opslag. Voor deze release kunt voor Hadoop en Storm-clusters op Windows en Linux, u Data Lake Store als extra opslag alleen. Azure Storage (WASB) dergelijke clusters nog steeds gebruiken als de standaard-opslag. Voor HBase-clusters op Windows- en Linux, kunt u Data Lake Store echter gebruiken als de opslag standaard of extra opslagruimte.

Zie voor instructies over het inrichten van een HDInsight-cluster met Data Lake Store:

* [Een HDInsight-cluster inrichten met Data Lake Store met Azure Portal](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Het inrichten van een HDInsight-cluster met Data Lake Store als standaard opslag met Azure PowerShell](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [Richt een HDInsight-cluster met Data Lake Store als extra opslagruimte met Azure PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)

## <a name="use-data-lake-store-with-azure-data-lake-analytics"></a>Gebruik Data Lake Store met Azure Data Lake Analytics
[Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-overview.md) kunt u werken met Big Data in de cloud. Dynamisch resources inricht en kunt u doen analytics van terabytes- of zelfs exabytes-aan gegevens die kunnen worden opgeslagen in een aantal ondersteunde gegevensbronnen, een van hen Data Lake Store. Data Lake Analytics is speciaal geoptimaliseerd voor samenwerking met Azure Data Lake Store - geven het hoogste niveau van prestaties, doorvoer en garandeert u big data-werkbelastingen.

Zie voor instructies over het gebruik van Data Lake Analytics met Data Lake Store [aan de slag met Data Lake Analytics met Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md).

## <a name="use-data-lake-store-with-azure-data-factory"></a>Gebruik Data Lake Store met Azure Data Factory
U kunt [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) om op te nemen van gegevens van Azure-tabellen, Azure SQL Database, Azure SQL DataWarehouse, Azure Storage-Blobs en on-premises-databases. Wordt een eerste-klas burger in het Azure-ecosysteem, worden Azure Data Factory gebruikt voor het indelen van de opname van gegevens van deze bron naar Azure Data Lake Store.

Zie voor instructies over het gebruik van Azure Data Factory met Data Lake Store [gegevens verplaatsen naar en van Data Lake Store met Data Factory](../data-factory/connector-azure-data-lake-store.md).

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-store"></a>Gegevens kopiëren van Azure Storage-Blobs in Data Lake Store
Azure Data Lake Store biedt een opdrachtregelprogramma, AdlCopy, waarmee u gegevens uit Azure Blob Storage kopiëren naar een Data Lake Store-account. Zie voor meer informatie [gegevens kopiëren van Azure Storage-Blobs naar Data Lake Store](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="copy-data-between-azure-sql-database-and-data-lake-store"></a>Gegevens worden gekopieerd tussen Azure SQL Database- en Data Lake Store
U kunt Apache Sqoop gebruiken om te importeren en exporteren van gegevens tussen Azure SQL Database- en Data Lake Store. Zie voor meer informatie [kopiëren van gegevens tussen Data Lake Store en Azure SQL database met behulp van Sqoop](data-lake-store-data-transfer-sql-sqoop.md).

## <a name="use-data-lake-store-with-stream-analytics"></a>Gebruik Data Lake Store met Stream Analytics
U kunt Data Lake Store als een van de uitvoer gebruiken voor het opslaan van gegevens met Azure Stream Analytics gestreamd. Zie voor meer informatie [Stream gegevens uit Azure Storage-Blob in Data Lake Store met Azure Stream Analytics](data-lake-store-stream-analytics.md).

## <a name="use-data-lake-store-with-power-bi"></a>Gebruik Data Lake Store met Power BI
U kunt Power BI kunt gebruiken om gegevens te importeren uit een Data Lake Store-account voor het analyseren en de gegevens te visualiseren. Zie voor meer informatie [analyseren van gegevens in Data Lake Store met Power BI](data-lake-store-power-bi.md).

## <a name="use-data-lake-store-with-data-catalog"></a>Gebruik Data Lake Store met Data Catalog
U kunt gegevens van Data Lake Store registreren in de Azure Data Catalog zodat de gegevens kunnen worden gedetecteerd in de hele organisatie. Zie voor meer informatie [gegevens van Data Lake Store registreren in Azure Data Catalog](data-lake-store-with-data-catalog.md).

## <a name="use-data-lake-store-with-sql-server-integration-services-ssis"></a>Gebruik Data Lake Store met SQL Server integratieservices (SSIS)
U kunt het Azure Data Lake Store-Verbindingsbeheer in SSIS SSIS-pakket verbinden met Azure Data Lake Store. Zie voor meer informatie [gebruik Data Lake Store met SSIS](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-store-connection-manager).

## <a name="use-data-lake-store-with-sql-data-warehouse"></a>Gebruik Data Lake Store met SQL datawarehouse
PolyBase kunt u gegevens uit Azure Data Lake Store laden in SQL Data Warehouse. Zie voor meer informatie [gebruik Data Lake Store met SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store.md).

## <a name="use-data-lake-store-with-azure-event-hubs"></a>Gebruik Data Lake Store met Azure Event Hubs
U kunt Azure Data Lake Store-archief en vastleggen gegevens ontvangen door Azure Event Hubs. Zie voor meer informatie [gebruik Data Lake Store met Azure Event Hubs](data-lake-store-archive-eventhub-capture.md).

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure Data Lake Store](data-lake-store-overview.md)
* [Aan de slag met Data Lake Store met de Portal](data-lake-store-get-started-portal.md)
* [Aan de slag met Data Lake Store met behulp van PowerShell](data-lake-store-get-started-powershell.md)  

