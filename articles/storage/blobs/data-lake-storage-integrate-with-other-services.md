---
title: Azure Data Lake Storage Gen2 integreren met andere Azure-Services | Microsoft Docs
description: Begrijpen hoe Azure Data Lake Storage Gen2 kan worden geïntegreerd met andere Azure-services
documentationcenter: ''
services: storage
author: normesta
ms.component: data-lake-storage-gen2
ms.service: storage
ms.devlang: na
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: nitinme
ms.openlocfilehash: 7bc4889403e1d52cfa3b18792a554f0faf605132
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55885664"
---
# <a name="integrate-azure-data-lake-storage-gen2-with-other-azure-services"></a>Azure Data Lake Storage Gen2 integreren met andere Azure-services

U kunt Azure-services opnemen, analyseren en visualiseren van gegevens in uw Azure Data Lake Storage Gen2 storage-account. Kies de services die het best past bij deze taken die u probeert te bereiken.

## <a name="ingest-data-into-your-data-lake"></a>Gegevens opnemen in uw data lake

Deze services kunt u uw data lake met gegevens te vullen.

### <a name="azure-data-factory"></a>Azure Data Factory

U kunt [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) voor opname van gegevens van deze bronnen:

* Azure-tabellen
* Azure SQL-databases
* Azure SQL DataWarehouse
* Azure Storage-Blobs
* On-premises database

Zie [verplaatsen van gegevens en naar Data Lake Storage Gen2 met behulp van Data Factory](../../data-factory/connector-azure-data-lake-store.md).

## <a name="analyze-and-visualize-data-in-your-data-lake"></a>Analyseer en Visualiseer gegevens in uw data lake

Deze services kunnen u Data Lake Storage Gen2 gebruiken als een opslageindpunt.

### <a name="azure-hdinsight"></a>Azure HDInsight

U kunt inrichten een [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) cluster dat gebruik maakt van Data Lake Storage Gen2 als de HDFS-compatibele opslag. Voor deze release kunt voor Hadoop- en Storm-clusters in Windows en Linux, u Data Lake Storage Gen2 alleen als een extra opslag. Azure Storage (WASB) clusters nog steeds gebruiken als de standaardopslag. Echter voor HBase-clusters in Windows en Linux, kunt u Data Lake Storage Gen2 als standaardopslag en als extra opslag.

Zie [gebruik Azure Data Lake Storage Gen2 met Azure HDInsight-clusters](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2)

### <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics

U kunt [Azure Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-overview.md) om te werken met Big Data in de cloud. Dynamisch resources inricht en kunt u exabytes aan gegevens te analyseren. Data Lake Analytics is geoptimaliseerd voor het gebruik van Data Lake Storage Gen2 als een gegevensbron. 

Zie [aan de slag met Data Lake Analytics met Data Lake Storage Gen2](../../data-lake-analytics/data-lake-analytics-get-started-portal.md).

## <a name="copy-data-to-other-repositories"></a>Gegevens kopiëren naar een andere opslagplaats

Deze services gebruiken om gegevens uit uw data lake kopiëren en plaats deze in andere opslagplaatsen.

### <a name="sql-data-warehouse"></a>SQL Data Warehouse

PolyBase kunt u gegevens uit Data Lake Storage Gen2 laden in SQL Data Warehouse. Zie voor meer informatie [met Data Lake Storage Gen2 met SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store.md).

## <a name="see-also"></a>Zie ook

* [Overzicht van Azure Data Lake Storage Gen2](data-lake-storage-introduction.md)
* [Met behulp van Azure Data Lake Storage Gen2 voor big data-vereisten](data-lake-storage-data-scenarios.md)
