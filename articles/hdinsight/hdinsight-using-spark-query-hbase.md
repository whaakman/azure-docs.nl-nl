---
title: Spark te lezen en schrijven van gegevens van de HBase - Azure HDInsight gebruiken | Microsoft Docs
description: De Spark HBase-Connector gebruiken om te lezen en schrijven van gegevens van een Spark-cluster naar een HBase-cluster.
services: hdinsight
documentationcenter: 
author: maxluk
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: maxluk
ms.openlocfilehash: ccbcd1d9cb45da7076d73f71a2ed692e71816650
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="use-spark-to-read-and-write-hbase-data"></a>Spark gebruiken om te lezen en HBase-gegevens schrijven

Apache HBase query wordt doorgaans uitgevoerd met de op laag niveau API (scans, opgehaald en puts) of met een SQL-syntaxis met Phoenix. Apache biedt ook de Spark HBase-Connector een handige is en alternatieve zodat om te vragen en gegevens die zijn opgeslagen door HBase te wijzigen.

## <a name="prerequisites"></a>Vereisten

* Twee afzonderlijke HDInsight-clusters, één HBase en één Spark met Spark 2.1 (HDInsight 3.6) geïnstalleerd.
* Het Spark-cluster moet communiceren rechtstreeks met de HBase-cluster met een minimale latentie, zodat de aanbevolen configuratie beide clusters in hetzelfde virtuele netwerk implementeert. Zie voor meer informatie [maken Linux gebaseerde clusters in HDInsight met behulp van de Azure-portal](hdinsight-hadoop-create-linux-clusters-portal.md).
* SSH-toegang tot elk cluster.
* Toegang tot elke clusteropslag standaard.

## <a name="overall-process"></a>Algemene proces

Het hoofdproces voor het inschakelen van uw Spark-cluster query uitvoeren op uw HDInsight-cluster is als volgt:

1. Voorbeeldgegevens in HBase maken.
2. Het bestand hbase-site.xml van uw map met HBase-cluster (/ hbase/etc/conf) verkrijgen.
3. Plaats een kopie van hbase-site.xml in de map van uw Spark-2-configuratie (/ spark2/etc/conf).
4. Voer `spark-shell` in die verwijzen naar de Connector Spark HBase door de Maven coördineert de `packages` optie.
5. Definieer een catalogus die het schema van Spark HBase wordt toegewezen.
6. Interactie met de HBase-gegevens met behulp van de RDD of DataFrame APIs.

## <a name="prepare-sample-data-in-hbase"></a>Voorbeeldgegevens in HBase voorbereiden

In deze stap maakt u maakt en een eenvoudige tabel in HBase die u kunt opvragen met Spark vullen.

1. Verbinding maken met het hoofdknooppunt van het HBase-cluster via SSH. Zie voor meer informatie [verbinding maken met HDInsight met behulp van SSH](hdinsight-hadoop-linux-use-ssh-unix.md).
2. Voer de HBase-shell:

        hbase shell

3. Maak een `Contacts` tabel met de kolomfamilies `Personal` en `Office`:

        create 'Contacts', 'Personal', 'Office'

4. Een paar voorbeeld-rijen van de gegevens worden geladen:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        put 'Contacts', '8396', 'Personal:Name', 'Calvin Raji'
        put 'Contacts', '8396', 'Personal:Phone', '230-555-0191'
        put 'Contacts', '8396', 'Office:Phone', '230-555-0191'
        put 'Contacts', '8396', 'Office:Address', '5415 San Gabriel Dr.'

## <a name="acquire-hbase-sitexml-from-your-hbase-cluster"></a>Verkrijgen van hbase-site.xml van het HBase-cluster

1. Verbinding maken met het hoofdknooppunt van het HBase-cluster via SSH.
2. Kopieer de hbase-site.xml van lokale opslag naar de hoofdmap van uw HBase-cluster standaard opslag:

        hdfs dfs -copyFromLocal /etc/hbase/conf/hbase-site.xml /

3. Navigeer naar uw HBase-cluster met de [Azure-portal](https://portal.azure.com).
4. Selecteer de Storage-accounts. 

    ![Opslagaccounts](./media/hdinsight-using-spark-query-hbase/storage-accounts.png)

5. Selecteer het opslagaccount in de lijst met een vinkje onder de standaard-kolom.

    ![Storage-standaardaccount](./media/hdinsight-using-spark-query-hbase/default-storage.png)

6. Selecteer de tegel Blobs in het deelvenster Storage-account.

    ![BLOBs tegel](./media/hdinsight-using-spark-query-hbase/blobs-tile.png)

7. Selecteer de container die wordt gebruikt door uw HBase-cluster in de lijst van containers.
8. Selecteer in de lijst met bestanden `hbase-site.xml`.

    ![HBase-site.xml](./media/hdinsight-using-spark-query-hbase/hbase-site-xml.png)

9. In het deelvenster van de eigenschappen van Blob Selecteer downloaden en opslaan `hbase-site.xml` naar een locatie op uw lokale computer.

    ![Downloaden](./media/hdinsight-using-spark-query-hbase/download.png)

## <a name="put-hbase-sitexml-on-your-spark-cluster"></a>Hbase-site.xml op uw Spark-cluster plaatsen

1. Navigeer naar uw Spark-cluster met de [Azure-portal](https://portal.azure.com).
2. Selecteer de Storage-accounts.

    ![Opslagaccounts](./media/hdinsight-using-spark-query-hbase/storage-accounts.png)

3. Selecteer het opslagaccount in de lijst met een vinkje onder de standaard-kolom.

    ![Storage-standaardaccount](./media/hdinsight-using-spark-query-hbase/default-storage.png)

4. Selecteer de tegel Blobs in het deelvenster Storage-account.

    ![BLOBs tegel](./media/hdinsight-using-spark-query-hbase/blobs-tile.png)

5. Selecteer de container die wordt gebruikt door uw Spark-cluster in de lijst van containers.
6. Selecteer uploaden.

    ![Uploaden](./media/hdinsight-using-spark-query-hbase/upload.png)

7. Selecteer de `hbase-site.xml` bestand dat u eerder hebt gedownload naar uw lokale computer.

    ![Hbase-site.xml uploaden](./media/hdinsight-using-spark-query-hbase/upload-selection.png)

8. Selecteer uploaden.
9. Verbinding maken met het hoofdknooppunt van uw Spark-cluster via SSH.
10. Kopiëren `hbase-site.xml` uit de opslag van uw Spark-cluster standaard naar de configuratiemap Spark 2 op de lokale opslag van het cluster:

        sudo hdfs dfs -copyToLocal /hbase-site.xml /etc/spark2/conf

## <a name="run-spark-shell-referencing-the-spark-hbase-connector"></a>Spark-Shell die verwijzen naar de HBase-Connector Spark uitvoeren

1. Verbinding maken met het hoofdknooppunt van uw Spark-cluster via SSH.
2. Start de spark-shell opgeven van het pakket Spark HBase-Connector:

        spark-shell --packages com.hortonworks:shc-core:1.1.0-2.1-s_2.11

3. Dit exemplaar Spark Shell open houden en doorgaan naar volgende stap.

## <a name="define-a-catalog-and-query"></a>Definieer een catalogus en de Query

In deze stap definieert u een catalogusobject dat het schema van Spark HBase wordt toegewezen. 

1. Uw open Spark-shell, voert u de volgende `import` instructies:

        import org.apache.spark.sql.{SQLContext, _}
        import org.apache.spark.sql.execution.datasources.hbase._
        import org.apache.spark.{SparkConf, SparkContext}
        import spark.sqlContext.implicits._

2. Een catalogus voor de tabel contactpersonen die u hebt gemaakt in HBase definiëren:
    1. Definieer een catalogusschema voor de HBase-tabel met de naam `Contacts`.
    2. Identificeren van de rowkey als `key`, en de kolomnamen in Spark gebruikt de kolomfamilie, kolomnaam en kolomtype zoals gebruikt in HBase toewijzen.
    3. De rowkey heeft ook worden gedefinieerd in detail als een kolom met de naam (`rowkey`), die een specifieke kolomfamilie heeft `cf` van `rowkey`.

            def catalog = s"""{
                |"table":{"namespace":"default", "name":"Contacts"},
                |"rowkey":"key",
                |"columns":{
                |"rowkey":{"cf":"rowkey", "col":"key", "type":"string"},
                |"officeAddress":{"cf":"Office", "col":"Address", "type":"string"},
                |"officePhone":{"cf":"Office", "col":"Phone", "type":"string"},
                |"personalName":{"cf":"Personal", "col":"Name", "type":"string"},
                |"personalPhone":{"cf":"Personal", "col":"Phone", "type":"string"}
                |}
            |}""".stripMargin

3. Een methode waarmee een DataFrame rond definiëren uw `Contacts` tabel in HBase:

            def withCatalog(cat: String): DataFrame = {
                spark.sqlContext
                .read
                .options(Map(HBaseTableCatalog.tableCatalog->cat))
                .format("org.apache.spark.sql.execution.datasources.hbase")
                .load()
            }

4. Maak een instantie van de DataFrame:

        val df = withCatalog(catalog)

5. De DataFrame query:

        df.show()

6. Hier ziet u twee rijen met gegevens:

        +------+--------------------+--------------+-------------+--------------+
        |rowkey|       officeAddress|   officePhone| personalName| personalPhone|
        +------+--------------------+--------------+-------------+--------------+
        |  1000|1111 San Gabriel Dr.|1-425-000-0002|    John Dole|1-425-000-0001|
        |  8396|5415 San Gabriel Dr.|  230-555-0191|  Calvin Raji|  230-555-0191|
        +------+--------------------+--------------+-------------+--------------+

7. Registreer een tijdelijke tabel zodat u kunt de HBase-tabel met behulp van Spark SQL query:

        df.registerTempTable("contacts")

8. Uitgeven van een SQL-query op de `contacts` tabel:

        val query = spark.sqlContext.sql("select personalName, officeAddress from contacts")
        query.show()

9. Hier ziet u resultaten zoals deze:

        +-------------+--------------------+
        | personalName|       officeAddress|
        +-------------+--------------------+
        |    John Dole|1111 San Gabriel Dr.|
        |  Calvin Raji|5415 San Gabriel Dr.|
        +-------------+--------------------+

## <a name="insert-new-data"></a>Invoegen van nieuwe gegevens

1. Voor het invoegen van een nieuwe contactpersoonrecord definiëren een `ContactRecord` klasse:

        case class ContactRecord(
            rowkey: String,
            officeAddress: String,
            officePhone: String,
            personalName: String,
            personalPhone: String
            )

2. Maak een instantie van `ContactRecord` en plaats deze in een matrix:

        val newContact = ContactRecord("16891", "40 Ellis St.", "674-555-0110", "John Jackson","230-555-0194")

        var newData = new Array[ContactRecord](1)
        newData(0) = newContact

3. De matrix van nieuwe gegevens in HBase opslaan:

        sc.parallelize(newData).toDF.write
        .options(Map(HBaseTableCatalog.tableCatalog -> catalog))
        .format("org.apache.spark.sql.execution.datasources.hbase").save()

4. Bekijk de resultaten:
    
        df.show()

5. De uitvoer ziet er als volgt uit:

        +------+--------------------+--------------+------------+--------------+
        |rowkey|       officeAddress|   officePhone|personalName| personalPhone|
        +------+--------------------+--------------+------------+--------------+
        |  1000|1111 San Gabriel Dr.|1-425-000-0002|   John Dole|1-425-000-0001|
        | 16891|        40 Ellis St.|  674-555-0110|John Jackson|  230-555-0194|
        |  8396|5415 San Gabriel Dr.|  230-555-0191| Calvin Raji|  230-555-0191|
        +------+--------------------+--------------+------------+--------------+

## <a name="next-steps"></a>Volgende stappen

* [Spark HBase Connector](https://github.com/hortonworks-spark/shc)
