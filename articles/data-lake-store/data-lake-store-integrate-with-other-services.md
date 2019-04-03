---
title: Azure Data Lake Storage Gen1 integreren met andere Azure-Services | Microsoft Docs
description: Begrijpen hoe Azure Data Lake Storage Gen1 kan worden geïntegreerd met andere Azure-services
documentationcenter: ''
services: data-lake-store
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 48a5d1f4-3850-4c22-bbc4-6d1d394fba8a
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: e28863f9980d6403bef1f88de01b7a9b5271b444
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58879298"
---
# <a name="integrating-azure-data-lake-storage-gen1-with-other-azure-services"></a>Azure Data Lake Storage Gen1 integreren met andere Azure-services
Azure Data Lake Storage Gen1 kan worden gebruikt in combinatie met andere Azure-services om in te schakelen van een breder scala aan scenario's. Het volgende artikel geeft een lijst van de Data Lake Storage Gen1 kan worden geïntegreerd met services.

## <a name="use-data-lake-storage-gen1-with-azure-hdinsight"></a>Data Lake Storage Gen1 met Azure HDInsight gebruiken
U kunt inrichten een [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) cluster dat gebruik maakt van Data Lake Storage Gen1 als de HDFS-compatibele opslag. Voor deze release kunt voor Hadoop- en Storm-clusters in Windows en Linux, u Data Lake Storage Gen1 alleen als een extra opslag. Azure Storage (WASB) clusters nog steeds gebruiken als de standaardopslag. U kunt echter voor HBase-clusters in Windows en Linux, Data Lake Storage Gen1 gebruiken als de standaardopslag en/of de extra opslagruimte.

Zie voor instructies over het inrichten van een HDInsight-cluster met Data Lake Storage Gen1:

* [Inrichten van een HDInsight-cluster met Data Lake Storage Gen1 met behulp van Azure Portal](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Inrichten van een HDInsight-cluster met Data Lake Storage Gen1 als standaardopslag met behulp van Azure PowerShell](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [Inrichten van een HDInsight-cluster met Data Lake Storage Gen1 als extra opslag met behulp van Azure PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)

## <a name="use-data-lake-storage-gen1-with-azure-data-lake-analytics"></a>Data Lake Storage Gen1 met Azure Data Lake Analytics gebruiken
[Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-overview.md) kunt u werken met Big Data in de cloud. Dynamisch resources inricht en kunt u uitvoeren van analyses op terabytes of zelfs exabytes aan gegevens die kunnen worden opgeslagen in een aantal ondersteunde gegevensbronnen, een van beide wordt Data Lake Storage Gen1. Data Lake Analytics is speciaal geoptimaliseerd om te werken met Data Lake Storage Gen1 - biedt het hoogste niveau van prestaties, doorvoer en parallellisering voor u big data-workloads.

Zie voor instructies over het gebruik van Data Lake Analytics met Data Lake Storage Gen1 [aan de slag met Data Lake Analytics met Data Lake Storage Gen1](../data-lake-analytics/data-lake-analytics-get-started-portal.md).

## <a name="use-data-lake-storage-gen1-with-azure-data-factory"></a>Data Lake Storage Gen1 met Azure Data Factory gebruiken
U kunt [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) voor opname van gegevens uit Azure-tabellen, Azure SQL Database, Azure SQL-DataWarehouse, Azure Storage-Blobs en on-premises-databases. Wordt een Eersterangs taal in het Azure-ecosysteem, worden Azure Data Factory gebruikt voor het indelen van de opname van gegevens van deze gegevensbron naar Data Lake Storage Gen1.

Zie voor instructies over het gebruik van Azure Data Factory met Data Lake Storage Gen1 [verplaatsen van gegevens en naar Data Lake Storage Gen1 met behulp van Data Factory](../data-factory/connector-azure-data-lake-store.md).

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-storage-gen1"></a>Gegevens uit Azure Storage-Blobs kopiëren naar Data Lake Storage Gen1
Azure Data Lake Storage Gen1 biedt een opdrachtregelprogramma, AdlCopy, waarmee u gegevens uit Azure Blob Storage kopiëren naar een Gen1 van Data Lake Storage-account. Zie voor meer informatie, [gegevens kopiëren van Azure Storage-Blobs naar Data Lake Storage Gen1](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="copy-data-between-azure-sql-database-and-data-lake-storage-gen1"></a>Kopiëren van gegevens tussen Azure SQL Database en Data Lake Storage Gen1
Apache Sqoop kunt u gegevens tussen Azure SQL Database en Data Lake Storage Gen1 importeren en exporteren. Zie voor meer informatie, [kopiëren van gegevens tussen Data Lake Storage Gen1 en Azure SQL-database met behulp van Sqoop](data-lake-store-data-transfer-sql-sqoop.md).

## <a name="use-data-lake-storage-gen1-with-stream-analytics"></a>Data Lake Storage Gen1 met Stream Analytics gebruiken
U kunt Data Lake Storage Gen1 gebruiken als een van de uitvoer voor het opslaan van gegevens gestreamd met behulp van Azure Stream Analytics. Zie voor meer informatie, [gegevens uit Azure Storage Blob naar Data Lake Storage Gen1 met behulp van Azure Stream Analytics Stream](data-lake-store-stream-analytics.md).

## <a name="use-data-lake-storage-gen1-with-power-bi"></a>Gebruik van Data Lake Storage Gen1 met Power BI
Power BI kunt u gegevens importeren uit een Data Lake Storage Gen1-account om te analyseren en visualiseren van de gegevens. Zie voor meer informatie, [analyseren van gegevens in Data Lake Storage Gen1 met behulp van Power BI](data-lake-store-power-bi.md).

## <a name="use-data-lake-storage-gen1-with-data-catalog"></a>Data Lake Storage Gen1 gebruiken met Data Catalog
U kunt gegevens van Data Lake Storage Gen1 registreren in de Azure Data Catalog om de gegevens kunnen worden gedetecteerd in de hele organisatie. Zie voor meer informatie [gegevens van Data Lake Storage Gen1 registreren in Azure Data Catalog](data-lake-store-with-data-catalog.md).

## <a name="use-data-lake-storage-gen1-with-sql-server-integration-services-ssis"></a>Data Lake Storage Gen1 met SQL Server integratieservices (SSIS) gebruiken
U kunt het Data Lake Storage Gen1 Verbindingsbeheer gebruiken in SSIS een SSIS-pakket verbinden met Data Lake Storage Gen1. Zie voor meer informatie, [met Data Lake Storage Gen1 met SSIS](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-store-connection-manager).

## <a name="use-data-lake-storage-gen1-with-sql-data-warehouse"></a>Data Lake Storage Gen1 met SQL datawarehouse gebruiken
PolyBase kunt u gegevens uit Data Lake Storage Gen1 laden in SQL Data Warehouse. Zie voor meer informatie [met Data Lake Storage Gen1 met SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store.md).

## <a name="use-data-lake-storage-gen1-with-azure-event-hubs"></a>Gebruik van Data Lake Storage Gen1 met Azure Eventhubs
U kunt Azure Data Lake Storage Gen1 archief- en vastleggen van gegevens die zijn ontvangen door Azure Event Hubs. Zie voor meer informatie [met Data Lake Storage Gen1 met Azure Event Hubs](data-lake-store-archive-eventhub-capture.md).

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure Data Lake Storage Gen1](data-lake-store-overview.md)
* [Aan de slag met Data Lake Storage Gen1 met Portal](data-lake-store-get-started-portal.md)
* [Aan de slag met Data Lake Storage Gen1 met behulp van PowerShell](data-lake-store-get-started-powershell.md)  

