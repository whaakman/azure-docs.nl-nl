---
title: Gegevens in de Hive-tabellen met Hive-query's verkennen | Microsoft Docs
description: Verken de gegevens in de Hive-tabellen met Hive-query's.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 0d46cea5-2b4c-4384-9bfa-fa20f6f75148
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: bradsev
ms.openlocfilehash: 9cf205abcf9782ceac4d9ac5a920e136b69c57b6
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Gegevens in Hive-tabellen verkennen met Hive-query's
Dit document vindt u voorbeelden van Hive scripts die worden gebruikt voor gegevens in de Hive-tabellen in een HDInsight Hadoop-cluster.

De volgende **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u gegevens uit verschillende omgevingen met opslag verkennen met behulp van hulpprogramma's.

[!INCLUDE [cap-explore-data-selector](../../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u hebt:

* Een Azure storage-account gemaakt. Als u instructies nodig hebt, raadpleegt u [een Azure Storage-account maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account)
* Een aangepaste Hadoop-cluster met de HDInsight-service wordt ingericht. Als u instructies nodig hebt, raadpleegt u [aanpassen Azure HDInsight Hadoop-Clusters voor geavanceerde analyses](customize-hadoop-cluster.md).
* De gegevens is geüpload naar de Hive-tabellen in Azure HDInsight Hadoop-clusters. Als dit niet het geval is, volg de instructies in [maken en gegevens laden met Hive-tabellen](move-hive-tables.md) eerst gegevens uploaden naar Hive-tabellen.
* Externe toegang tot het cluster is ingeschakeld. Als u instructies nodig hebt, raadpleegt u [toegang tot de hoofd-knooppunt van Hadoop-Cluster](customize-hadoop-cluster.md).
* Als u instructies voor het indienen van Hive-query's nodig hebt, raadpleegt u [het Hive-query's verzenden](move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Voorbeeld Hive query scripts voor gegevensverkenning
1. Het aantal opmerkingen per partitie ophalen`SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`
2. Het aantal opmerkingen per dag`SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`
3. De niveaus in een categorische kolom ophalen  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`
4. Het aantal niveaus in de combinatie van twee categorische kolommen ophalen`SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`
5. De verdeling voor numerieke kolommen ophalen  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`
6. Haal de records uit twee tabellen samenvoegen
   
        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Extra query scripts voor taxi reis gegevensscenario 's
Voorbeelden van query's die specifiek zijn voor [NYC Taxi reis gegevens](http://chriswhong.com/open-data/foil_nyc_taxi/) scenario's zijn ook beschikbaar [GitHub-opslagplaats](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Deze query's al hebt opgegeven gegevensschema en klaar bent om te worden verzonden om uit te voeren.

