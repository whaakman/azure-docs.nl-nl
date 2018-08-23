---
title: Gegevens in Hive-tabellen met Hive-query's verkennen | Microsoft Docs
description: Verken de gegevens in Hive-tabellen met behulp van Hive-query's.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 0d46cea5-2b4c-4384-9bfa-fa20f6f75148
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: deguhath
ms.openlocfilehash: 79d40617ae4f9cd83d04cad213e5d8fd76b03876
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/21/2018
ms.locfileid: "42055141"
---
# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Gegevens in Hive-tabellen verkennen met Hive-query's
Dit document bevat een voorbeeld van Hive-scripts die worden gebruikt om gegevens in Hive-tabellen in een HDInsight Hadoop-cluster te verkennen.

De volgende **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u hulpprogramma's gebruiken om gegevens te verkennen vanuit verschillende omgevingen voor opslag.

[!INCLUDE [cap-explore-data-selector](../../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u hebt:

* Een Azure storage-account gemaakt. Als u instructies nodig hebt, raadpleegt u [maken van een Azure Storage-account](../../storage/common/storage-quickstart-create-account.md)
* Een aangepaste Hadoop-cluster met de HDInsight-service wordt ingericht. Als u instructies nodig hebt, raadpleegt u [aanpassen Azure HDInsight Hadoop-Clusters voor Advanced Analytics](customize-hadoop-cluster.md).
* De gegevens is geüpload naar de Hive-tabellen in Azure HDInsight Hadoop-clusters. Als dit niet het geval is, volgt u de instructies in [maken en gegevens laden met Hive-tabellen](move-hive-tables.md) het uploaden van gegevens met Hive-tabellen eerst.
* Externe toegang tot het cluster ingeschakeld. Als u instructies nodig hebt, raadpleegt u [toegang tot de hoofd-knooppunt van Hadoop Cluster](customize-hadoop-cluster.md).
* Als u instructies voor het indienen van Hive-query's nodig hebt, raadpleegt u [hoe u Hive-query's verzenden](move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Voorbeeld van de Hive-query scripts voor gegevens verkennen
1. Het aantal opmerkingen per partitie ophalen  `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`
2. Het aantal opmerkingen per dag  `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`
3. De niveaus in een categorische kolom ophalen  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`
4. Het aantal niveaus in de combinatie van twee categorische kolommen ophalen  `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`
5. De verdeling van de numerieke kolommen ophalen  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`
6. Records ophalen uit het koppelen van twee tabellen
   
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

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Aanvullende query-scripts voor scenario's reis voor gegevens over taxi 's
Voorbeelden van query's die specifiek zijn voor [NYC Taxi reisgegevens](http://chriswhong.com/open-data/foil_nyc_taxi/) scenario's zijn ook beschikbaar [GitHub-opslagplaats](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Deze query's al hebben gegevensschema opgegeven en kunnen worden verzonden om uit te voeren.

