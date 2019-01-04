---
title: Spark gebruiken om te lezen en schrijven van gegevens in HBase - Azure HDInsight
description: De Spark-HBase-Connector gebruiken om te lezen en schrijven van gegevens van een Spark-cluster naar een HBase-cluster.
services: hdinsight
author: maxluk
ms.author: maxluk
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/05/2018
ms.openlocfilehash: 547cc30bdf3dedff30c28165a7a76093a6512b83
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53993076"
---
# <a name="use-apache-spark-to-read-and-write-apache-hbase-data"></a>Apache Spark gebruiken om Apache HBase-gegevens te lezen en schrijven

Apache HBase is meestal een query uitgevoerd met de laag niveau API (scans, opgehaald en plaatst) of met een SQL-syntaxis met behulp van Apache Phoenix. Apache biedt ook de Apache Spark HBase-Connector dit een handige is en goed presterende alternatieve om te vragen en gegevens die zijn opgeslagen door HBase te wijzigen.

## <a name="prerequisites"></a>Vereisten

* Twee HDInsight-clusters, één HBase en Spark op een afzonderlijke met Spark 2.1 (HDInsight 3.6) geïnstalleerd.
* Het Spark-cluster moet communiceren rechtstreeks met de HBase-cluster met een minimale latentie, zodat beide clusters in hetzelfde virtuele netwerk is de implementatie van de aanbevolen configuratie. Zie voor meer informatie, [maken Linux gebaseerde clusters in HDInsight met behulp van de Azure-portal](hdinsight-hadoop-create-linux-clusters-portal.md).
* SSH-toegang tot elk cluster.
* Toegang tot de standaardopslag voor van elke cluster.

## <a name="overall-process"></a>Algehele proces

Het proces op hoog niveau voor het inschakelen van uw Spark-cluster query uitvoeren op uw HDInsight-cluster is als volgt:

1. Voorbeeldgegevens in HBase maken.
2. De hbase-site.xml-bestand van de configuratiemap voor HBase-cluster (/ etc/hbase/conf) aan te schaffen.
3. Plaats een kopie van hbase-site.xml in de map van uw Spark-2-configuratie (/ etc/spark2/conf).
4. Voer `spark-shell` verwijst naar de HBase-Connector voor Spark, door de Maven-coördinaten het `packages` optie.
5. Definieer een catalogus die het schema van Spark voor HBase-kaarten.
6. Interactie met de HBase-gegevens met behulp van de RDD of DataFrame APIs.

## <a name="prepare-sample-data-in-apache-hbase"></a>Voorbeeldgegevens in Apache HBase voorbereiden

In deze stap maakt u maken en vullen van een eenvoudige tabel in Apache HBase die u kunt vervolgens een query met behulp van Spark.

1. Verbinding maken met het hoofdknooppunt van het HBase-cluster via SSH. Zie voor meer informatie, [verbinding maken met HDInsight met behulp van SSH](hdinsight-hadoop-linux-use-ssh-unix.md).
2. Voer de HBase-shell:

        hbase shell

3. Maak een `Contacts` tabel met de kolomfamilies `Personal` en `Office`:

        create 'Contacts', 'Personal', 'Office'

4. Enkele voorbeeldrijen met gegevens laden:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        put 'Contacts', '8396', 'Personal:Name', 'Calvin Raji'
        put 'Contacts', '8396', 'Personal:Phone', '230-555-0191'
        put 'Contacts', '8396', 'Office:Phone', '230-555-0191'
        put 'Contacts', '8396', 'Office:Address', '5415 San Gabriel Dr.'

## <a name="acquire-hbase-sitexml-from-your-hbase-cluster"></a>Hbase-site.xml vanuit de HBase-cluster te verkrijgen

1. Verbinding maken met het hoofdknooppunt van het HBase-cluster via SSH.
2. Kopieer de hbase-site.xml uit de lokale opslag in de hoofdmap van de standaardopslag voor het HBase-cluster:

        hdfs dfs -copyFromLocal /etc/hbase/conf/hbase-site.xml /

3. Navigeer naar de HBase-cluster met de [Azure-portal](https://portal.azure.com).
4. Selecteer de Storage-accounts. 

    ![Opslagaccounts](./media/hdinsight-using-spark-query-hbase/storage-accounts.png)

5. Selecteer het opslagaccount in de lijst met een vinkje onder de standaardkolom.

    ![Standaardopslagaccount](./media/hdinsight-using-spark-query-hbase/default-storage.png)

6. Selecteer de tegel van de Blobs in het deelvenster Storage-account.

    ![BLOBs-tegel](./media/hdinsight-using-spark-query-hbase/blobs-tile.png)

7. Selecteer de container die wordt gebruikt door uw HBase-cluster in de lijst met containers.
8. Selecteer in de lijst met bestanden `hbase-site.xml`.

    ![HBase-site.xml](./media/hdinsight-using-spark-query-hbase/hbase-site-xml.png)

9. Blob-eigenschappen in het venster, selecteer downloaden en opslaan `hbase-site.xml` naar een locatie op uw lokale computer.

    ![Downloaden](./media/hdinsight-using-spark-query-hbase/download.png)

## <a name="put-hbase-sitexml-on-your-spark-cluster"></a>Hbase-site.xml plaatsen op uw Spark-cluster

1. Navigeer naar uw Spark-cluster met de [Azure-portal](https://portal.azure.com).
2. Selecteer de Storage-accounts.

    ![Opslagaccounts](./media/hdinsight-using-spark-query-hbase/storage-accounts.png)

3. Selecteer het opslagaccount in de lijst met een vinkje onder de standaardkolom.

    ![Standaardopslagaccount](./media/hdinsight-using-spark-query-hbase/default-storage.png)

4. Selecteer de tegel van de Blobs in het deelvenster Storage-account.

    ![BLOBs-tegel](./media/hdinsight-using-spark-query-hbase/blobs-tile.png)

5. Selecteer de container die wordt gebruikt door uw Spark-cluster in de lijst met containers.
6. Selecteer uploaden.

    ![Uploaden](./media/hdinsight-using-spark-query-hbase/upload.png)

7. Selecteer de `hbase-site.xml` bestand dat u eerder hebt gedownload naar uw lokale computer.

    ![Hbase-site.xml uploaden](./media/hdinsight-using-spark-query-hbase/upload-selection.png)

8. Selecteer uploaden.
9. Verbinding maken met het hoofdknooppunt van uw Spark-cluster via SSH.
10. Kopie `hbase-site.xml` uit uw Spark-cluster standaardopslag naar de map Spark 2-configuratie op de lokale opslag van het cluster:

        sudo hdfs dfs -copyToLocal /hbase-site.xml /etc/spark2/conf

## <a name="run-spark-shell-referencing-the-spark-hbase-connector"></a>Uitvoeren van Spark-Shell die verwijst naar de Spark-Connector voor HBase

1. Verbinding maken met het hoofdknooppunt van uw Spark-cluster via SSH.
2. Start de spark-shell, het Spark-Connector voor HBase-pakket op te geven:

        spark-shell --packages com.hortonworks:shc-core:1.1.0-2.1-s_2.11 --repositories https://repo.hortonworks.com/content/groups/public/

3. Houd dit exemplaar van de Spark-Shell geopend en Ga door met de volgende stap.

## <a name="define-a-catalog-and-query"></a>Een catalogus en een Query definiëren

In deze stap definieert u een catalogusobject dat het schema van Apache Spark voor Apache HBase-kaarten. 

1. In uw open Spark-Shell, voert u de volgende `import` instructies:

        import org.apache.spark.sql.{SQLContext, _}
        import org.apache.spark.sql.execution.datasources.hbase._
        import org.apache.spark.{SparkConf, SparkContext}
        import spark.sqlContext.implicits._

2. Definieer een catalogus voor de tabel met contacten die u hebt gemaakt in HBase:
    1. Een catalogusschema voor de HBase-tabel met de naam definiëren `Contacts`.
    2. Identificeren van de rowkey als `key`, en de namen van de kolommen die worden gebruikt in Spark kolomfamilie, kolomnaam en het kolomtype, zoals de term in HBase toewijzen.
    3. De rowkey heeft ook worden gedefinieerd in detail als een benoemde kolom (`rowkey`), die is voorzien van een specifieke kolom-familie `cf` van `rowkey`.

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

3. Definieer een methode waarmee een DataFrame rond uw `Contacts` tabel in HBase:

            def withCatalog(cat: String): DataFrame = {
                spark.sqlContext
                .read
                .options(Map(HBaseTableCatalog.tableCatalog->cat))
                .format("org.apache.spark.sql.execution.datasources.hbase")
                .load()
            }

4. Maak een instantie van het gegevensframe:

        val df = withCatalog(catalog)

5. Query van het gegevensframe:

        df.show()

6. Hier ziet u twee rijen met gegevens:

        +------+--------------------+--------------+-------------+--------------+
        |rowkey|       officeAddress|   officePhone| personalName| personalPhone|
        +------+--------------------+--------------+-------------+--------------+
        |  1000|1111 San Gabriel Dr.|1-425-000-0002|    John Dole|1-425-000-0001|
        |  8396|5415 San Gabriel Dr.|  230-555-0191|  Calvin Raji|  230-555-0191|
        +------+--------------------+--------------+-------------+--------------+

7. Een tijdelijke tabel registreren, zodat u kunt de HBase-tabel met behulp van Spark SQL query:

        df.registerTempTable("contacts")

8. Een SQL-query op basis van de `contacts` tabel:

        val query = spark.sqlContext.sql("select personalName, officeAddress from contacts")
        query.show()

9. Hier ziet u de resultaten zoals deze:

        +-------------+--------------------+
        | personalName|       officeAddress|
        +-------------+--------------------+
        |    John Dole|1111 San Gabriel Dr.|
        |  Calvin Raji|5415 San Gabriel Dr.|
        +-------------+--------------------+

## <a name="insert-new-data"></a>Nieuwe gegevens invoegen

1. Voor het invoegen van een nieuwe record van de contactpersoon, definieert een `ContactRecord` klasse:

        case class ContactRecord(
            rowkey: String,
            officeAddress: String,
            officePhone: String,
            personalName: String,
            personalPhone: String
            )

2. Maak een instantie van `ContactRecord` en plaatst deze in een matrix:

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

* [Apache Spark-Connector van HBase](https://github.com/hortonworks-spark/shc)
