---
title: Integratie van Apache Spark en Apache Hive met de Hive-Warehouse-Connector
description: Leer hoe u Apache Spark en Apache Hive integreren met de datawarehouse-Connector Hive op Azure HDInsight.
ms.service: hdinsight
author: nakhanha
ms.author: nakhanha
ms.reviewer: hrasheed
ms.topic: conceptual
ms.date: 04/29/2019
ms.openlocfilehash: b2b3d1ac0a7c0e917f87be1dd131120f63a70f8e
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65142814"
---
# <a name="integrate-apache-spark-and-apache-hive-with-the-hive-warehouse-connector"></a>Integratie van Apache Spark en Apache Hive met de Hive-Warehouse-Connector

Apache Hive datawarehouse Connector (HWC) is een bibliotheek waarmee u eenvoudig kunt werken met Apache Spark en Apache Hive door de ondersteuning van taken, zoals het verplaatsen van gegevens tussen DataFrames Spark en Hive-tabellen en ook doorsturen Spark streaming-gegevens in Hive-tabellen. Hive Warehouse-Connector werkt als een brug tussen Spark en Hive. Deze biedt ondersteuning voor Scala, Java en Python voor ontwikkeling.

De Hive-Warehouse-Connector kunt u profiteren van de unieke eigenschappen van Hive- en Spark om krachtige big data-toepassingen te bouwen. Apache Hive biedt ondersteuning voor databasetransacties die Atomic, Consistent, geïsoleerd en duurzaam (ACID). Zie voor meer informatie over het ACID en transacties in Hive [Hive transacties](https://cwiki.apache.org/confluence/display/Hive/Hive+Transactions). Hive biedt ook gedetailleerde beveiligingsmaatregelen door middel van Apache Ranger en lage latentie analytische verwerking niet beschikbaar is in Apache Spark.

Apache Spark, heeft een Structured Streaming-API waarmee streaming mogelijkheden die niet beschikbaar zijn in Apache Hive. U begint met Hortonworks Data Platform (HDP) 3.0, hebben Apache Spark en Apache Hive afzonderlijke metastores, waardoor interoperabiliteit moeilijk. De Hive-Warehouse-Connector maakt het eenvoudiger te gebruiken met Spark en Hive samen. De HWC-bibliotheek worden gegevens geladen uit LLAP daemons op Spark Executor parallel, waardoor het nog efficiënter en beter schaalbaar dan het gebruik van een standard JDBC-verbinding tussen Spark en Hive.

![Architectuur](./media/apache-hive-warehouse-connector/hive-warehouse-connector-architecture.png)

Enkele van de bewerkingen die worden ondersteund door de Hive-Warehouse-Connector zijn:

* Met een beschrijving van een tabel
* Het maken van een tabel met gegevens in ORC-indeling
* Hive-gegevens selecteren en bij het ophalen van een DataFrame
* Een DataFrame schrijven naar Hive in batch
* Uitvoeren van een Hive-update-instructie
* Lezen van gegevens in een tabel van Hive, waar ze worden getransformeerd in Spark en het schrijven naar een nieuwe Hive-tabel
* Een DataFrame of Spark stream schrijven naar HiveStreaming met Hive

## <a name="hive-warehouse-connector-setup"></a>Hive Warehouse-Connector instellen

Volg deze stappen om in te stellen tussen een Spark- en interactieve Query-cluster in Azure HDInsight de door de Hive-Warehouse-Connector:

1. Maak een 4.0 voor HDInsight Spark-cluster met behulp van de Azure-portal met een opslagaccount en een aangepaste Azure-netwerk. Zie voor meer informatie over het maken van een cluster in een Azure-netwerk [HDInsight toevoegen aan een bestaand virtueel netwerk](../../hdinsight/hdinsight-extend-hadoop-virtual-network.md#existingvnet).
1. Maak een HDInsight Interactive Query (LLAP) 4.0-cluster met behulp van de Azure-portal met hetzelfde opslagaccount en Azure-netwerk als het Spark-cluster.
1. Kopieer de inhoud van de `/etc/hosts` -bestand op headnode0 van uw interactieve Query-cluster de `/etc/hosts` -bestand op de headnode0 van uw Spark-cluster. Deze stap kunt uw Spark-cluster op te lossen IP-adressen van de knooppunten in Interactive Query-cluster. De inhoud van het bijgewerkte bestand met `cat /etc/hosts`. De uitvoer ziet er ongeveer als in de onderstaande schermafbeelding wordt weergegeven.

    ![het hosts-bestand weergeven](./media/apache-hive-warehouse-connector/hive-warehouse-connector-hosts-file.png)

1. Configureer de instellingen van de Spark-cluster door de volgende stappen: 
    1. Ga naar Azure portal, selecteert u HDInsight-clusters en klik vervolgens op de clusternaam van uw.
    1. Aan de rechterkant, onder **Clusterdashboards**, selecteer **Ambari home**.
    1. Klik in de Ambari-Webgebruikersinterface op **SPARK2** > **PEERINGCONFIGURATIES** > **aangepaste spark2-standaardinstellingen**.

        ![Configuratie van Spark2 Ambari](./media/apache-hive-warehouse-connector/hive-warehouse-connector-spark2-ambari.png)

    1. Stel `spark.hadoop.hive.llap.daemon.service.hosts` op dezelfde waarde als de eigenschap **LLAP app-naam** onder **hive interactieve env geavanceerde**. Bijvoorbeeld: `@llap0`

    1. Stel `spark.sql.hive.hiveserver2.jdbc.url` naar het JDBC-verbindingsreeks die verbinding maakt met Hiveserver2 op het Interactive Query-cluster. De verbindingsreeks voor uw cluster eruit URI hieronder. `CLUSTERNAME` de naam van uw Spark-cluster en de `user` en `password` parameters zijn ingesteld op de juiste waarden voor uw cluster.

        ```
        jdbc:hive2://LLAPCLUSTERNAME.azurehdinsight.net:443/;user=admin;password=PWD;ssl=true;transportMode=http;httpPath=/hive2
        ```

        >[!Note] 
        > De JDBC-URL mag bevatten referenties voor het verbinden met Hiveserver2 met inbegrip van een gebruikersnaam en wachtwoord.

    1. Stel `spark.datasource.hive.warehouse.load.staging.dir` naar een geschikte HDFS-compatibele staging-map. Hebt u twee verschillende clusters, moet de staging-map een map in de staging-map van het opslagaccount van het cluster LLAP zijn zodat HiveServer2 toegang toe heeft. Bijvoorbeeld, `wasb://STORAGE_CONTAINER_NAME@STORAGE_ACCOUNT_NAME.blob.core.windows.net/tmp` waar `STORAGE_ACCOUNT_NAME` is de naam van het opslagaccount dat wordt gebruikt door het cluster en `STORAGE_CONTAINER_NAME` is de naam van de storage-container.

    1. Stel `spark.datasource.hive.warehouse.metastoreUri` met de waarde van de metastore URI van het Interactive Query-cluster. Voor de metastoreUri voor uw cluster LLAP, zoekt u voor de **hive.metastore.uris** eigenschap in de Ambari-UI voor uw LLAP cluster onder **Hive** > **GEAVANCEERD**  >  **Algemene**. De waarde ziet er ongeveer als de volgende URI uit:

        ```
        thrift://hn0-hwclla.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:9083,
        thrift://hn1-hwclla.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:9083
        ```

    1. Stel `spark.security.credentials.hiveserver2.enabled` naar `false` voor YARN-client implementeren modus.
    1. Stel `spark.hadoop.hive.zookeeper.quorum` naar de Zookeeper-quorum van uw Cluster LLAP. Voor de Zookeeper-quorum voor uw cluster LLAP, zoekt u voor de **hive.zookeeper.quorum** eigenschap in de Ambari-UI voor uw LLAP cluster onder **Hive** > **GEAVANCEERD**  >  **Geavanceerde hive-site**. De waarde ziet er ongeveer als de volgende tekenreeks:

        ```
        zk1-nkhvne.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:2181,
        zk4-nkhvne.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:2181,
        zk6-nkhvne.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:2181
        ```

Als u wilt testen van de configuratie van uw datawarehouse Hive-Connector, volg de stappen in de sectie [verbinden en actieve query's](#connecting-and-running-queries).

## <a name="using-the-hive-warehouse-connector"></a>Met behulp van de Hive-Warehouse-Connector

### <a name="connecting-and-running-queries"></a>Verbinding te maken en uitvoeren van query 's

U kunt kiezen tussen de verschillende manieren een verbinding maken met uw Interactive Query-cluster en het uitvoeren van query's met behulp van de Hive-Warehouse-Connector. Ondersteunde methoden zijn onder andere de volgende hulpprogramma's:

* [spark-shell](../spark/apache-spark-shell.md)
* PySpark
* Spark-submit
* [Zeppelin](../spark/apache-spark-zeppelin-notebook.md)
* [Livy](../spark/apache-spark-livy-rest-interface.md)

Alle voorbeelden in dit artikel zal worden uitgevoerd via een spark-shell.

Om een spark-shell-sessie starten, voert u de volgende stappen uit:

1. Voeg SSH toe aan het hoofdknooppunt van het voor uw cluster. Zie voor meer informatie over het verbinden met uw cluster met SSH [verbinding maken met HDInsight (Apache Hadoop) via SSH](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).
1. Wijzig bij de juiste directory door te typen `cd /usr/hdp/current/hive_warehouse_connector` of geef het volledige pad naar alle jar-bestanden die worden gebruikt als parameters in de spark-shell-opdracht.
1. Voer de volgende opdracht om te beginnen de spark-shell:

    ```bash
    spark-shell --master yarn \
    --jars /usr/hdp/3.0.1.0-183/hive_warehouse_connector/hive-warehouse-connector-assembly-1.0.0.3.0.1.0-183.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    ```

1. Ziet u een welkomstbericht en een `scala>` prompt waarin u opdrachten kunt invoeren.

1. Na het starten van de spark-shell, kan een Hive-Warehouse-Connector-exemplaar worden gestart met de volgende opdrachten:

    ```scala
    import com.hortonworks.hwc.HiveWarehouseSession
    val hive = HiveWarehouseSession.session(spark).build()
    ```

### <a name="connecting-and-running-queries-on-enterprise-security-package-esp-clusters"></a>Verbinding maken en query's uitvoeren op clusters Enterprise Security Package (ESP)

Enterprise Security Package (ESP) biedt geavanceerde mogelijkheden, zoals Active Directory gebaseerde verificatie, ondersteuning voor meerdere gebruikers en rollen gebaseerd toegangsbeheer voor Apache Hadoop-clusters in Azure HDInsight. Zie voor meer informatie over ESP [een inleiding tot Apache Hadoop-beveiliging met Enterprise-beveiligingspakket](../domain-joined/apache-domain-joined-introduction.md).

1. Volg de eerste stappen 1 en 2 onder [verbinden en actieve query's](#connecting-and-running-queries).
1. Type `kinit` en meld u aan met een domeingebruiker.
1. Start spark-shell met de volledige lijst met parameters voor de configuratie zoals hieronder wordt weergegeven. Alle waarden in hoofdletters tussen punthaken moet worden opgegeven op basis van uw cluster. Als u weten welke waarden u wilt moet invoeren voor het gebruik van de onderstaande parameters, Zie de sectie over [Hive Warehouse-Connector setup](#hive-warehouse-connector-setup).:

    ```bash
    spark-shell --master yarn \
    --jars /usr/hdp/3.0.1.0-183/hive_warehouse_connector/hive-warehouse-connector-assembly-1.0.0.3.0.1.0-183.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    --conf spark.hadoop.hive.llap.daemon.service.hosts='<LLAP_APP_NAME>'
    --conf spark.sql.hive.hiveserver2.jdbc.url='jdbc:hive2://<ZOOKEEPER_QUORUM>;serviceDiscoveryMode=zookeeper;zookeeperNamespace=hiveserver2-interactive'
    --conf spark.datasource.hive.warehouse.load.staging.dir='<STAGING_DIR>'
    --conf spark.datasource.hive.warehouse.metastoreUri='<METASTORE_URI>'
    --conf spark.hadoop.hive.zookeeper.quorum='<ZOOKEEPER_QUORUM>'
   ```

### <a name="creating-spark-dataframes-from-hive-queries"></a>Het maken van Spark DataFrames van Hive-query 's

De resultaten van alle query's met behulp van de HWC-bibliotheek worden geretourneerd als een DataFrame. De volgende voorbeelden laten zien hoe u een eenvoudige query maken.

```scala
hive.setDatabase("default")
val df = hive.executeQuery("select * from hivesampletable")
df.filter("state = 'Colorado'").show()
```

De resultaten van de query zijn Spark DataFrames, die kan worden gebruikt met Spark-bibliotheken, zoals MLIB en SparkSQL.

### <a name="writing-out-spark-dataframes-to-hive-tables"></a>Uitschrijven van Spark DataFrames met Hive-tabellen

Spark biedt geen systeemeigen ondersteuning voor het schrijven naar de Hive-tabellen voor beheerde ACID. Met HWC echter, kunt u schrijven om een DataFrame in een Hive-tabel. U kunt deze functionaliteit op het werk in het volgende voorbeeld kunt zien:

1. Maken van een tabel met de naam `sampletable_colorado` en geeft u de kolommen met de volgende opdracht:

    ```scala
    hive.createTable("sampletable_colorado").column("clientid","string").column("querytime","string").column("market","string").column("deviceplatform","string").column("devicemake","string").column("devicemodel","string").column("state","string").column("country","string").column("querydwelltime","double").column("sessionid","bigint").column("sessionpagevieworder","bigint").create()
    ```

2. Filter de tabel `hivesampletable` waar de kolom `state` gelijk is aan `Colorado`. Deze query van de Hive-tabel geretourneerd als een Spark DataFrame. En vervolgens het gegevensframe wordt opgeslagen in de Hive-tabel `sampletable_colorado` met behulp van de `write` functie.
    
    ```scala
    hive.table("hivesampletable").filter("state = 'Colorado'").write.format(HiveWarehouseSession.HIVE_WAREHOUSE_CONNECTOR).option("table","sampletable_colorado").save()
    ```

U ziet de resulterende tabel in de onderstaande schermafbeelding.

![resulterende tabel weergeven](./media/apache-hive-warehouse-connector/hive-warehouse-connector-show-hive-table.png)

### <a name="structured-streaming-writes"></a>Gestructureerd streamen schrijfbewerkingen

Met behulp van Hive-Warehouse-Connector, kunt u Spark-streaming voor het schrijven van gegevens in Hive-tabellen.

Volg onderstaande stappen voor het maken van een voorbeeld van een Hive-Warehouse-Connector die gegevens van een Spark-stream op localhost poort 9999 in een Hive-tabel opnemen.

1. Open een terminal in uw Spark-cluster.
1. Beginnen met spark stream met de volgende opdracht:

    ```scala
    val lines = spark.readStream.format("socket").option("host", "localhost").option("port",9988).load()
    ```

1. Genereren van gegevens voor de Spark-stroom die u hebt gemaakt met de volgende stappen:
    1. Open een terminal in hetzelfde Spark-cluster.
    1. Typ het volgende achter de opdrachtprompt `nc -lk 9999`. Met deze opdracht maakt gebruik van het hulpprogramma netcat voor het verzenden van gegevens vanaf de opdrachtregel met de opgegeven poort.
    1. Typ de woorden die u de Spark-stream om op te nemen wilt, gevolgd door regelterugloop.
        ![spark stream invoergegevens](./media/apache-hive-warehouse-connector/hive-warehouse-connector-spark-stream-data-input.png)
1. Maak een nieuwe Hive-tabel voor het opslaan van de streaming-gegevens. Typ de volgende opdrachten in de spark-shell:

    ```scala
    hive.createTable("stream_table").column("value","string").create()
    ```

1. De streaming-gegevens schrijven naar de zojuist gemaakte tabel met de volgende opdracht:

    ```scala
    lines.filter("value = 'HiveSpark'").writeStream.format(HiveWarehouseSession.STREAM_TO_STREAM).option("database", "default").option("table","stream_table").option("metastoreUri",spark.conf.get("spark.datasource.hive.warehouse.metastoreUri")).option("checkpointLocation","/tmp/checkpoint1").start()
    ```

    >[!Important]
    > De `metastoreUri` en `database` opties moeten op dit moment handmatig instellen vanwege een bekend probleem in Apache Spark. Zie voor meer informatie over dit probleem [SPARK-25460](https://issues.apache.org/jira/browse/SPARK-25460).

1. Hier vindt u de gegevens ingevoegd in de tabel met de volgende opdracht:

    ```scala
    hive.table("stream_table").show()
    ```

### <a name="securing-data-on-spark-esp-clusters"></a>Het beveiligen van gegevens op ESP Spark-clusters

1. Maak een tabel `demo` met wat voorbeeldgegevens met de volgende opdrachten:

    ```scala
    create table demo (name string);
    INSERT INTO demo VALUES ('HDinsight');
    INSERT INTO demo VALUES ('Microsoft');
    INSERT INTO demo VALUES ('InteractiveQuery');
    ```

1. Bekijk de inhoud van de tabel met de volgende opdracht. Voordat u het beleid, de `demo` tabel ziet u de volledige kolom.

    ```scala
    hive.executeQuery("SELECT * FROM demo").show()
    ```

    ![demo-tabel vóór het toepassen van beleid voor ranger](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-before-ranger-policy.png)

1. Een kolom maskeren beleid waarin alleen de laatste vier tekens van de kolom die van toepassing.  
    1. Ga naar de Webinterface voor het beheer van Ranger op `https://CLUSTERNAME.azurehdinsight.net/ranger/`.
    1. Klik op de Hive-service voor uw cluster onder **Hive**.
        ![demo-tabel vóór het toepassen van beleid voor ranger](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-service-manager.png)
    1. Klik op de **maskeren** tabblad en vervolgens **nieuw beleid toevoegen** ![lijst met beleidsregels](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-hive-policy-list.png)
    1. Geef een beleidsnaam van het gewenste. Database selecteren: **Standaard**, Hive-tabel: **demo**, Hive-kolom: **naam**, gebruiker: **rsadmin2**, toegangstypen: **Selecteer**, en **Gedeeltelijke masker: afgelopen 4 weergeven** uit de **maskeren optie selecteren** menu. Klik op **Add**.
                ![lijst met beleidsregels](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-create-policy.png)
1. De inhoud van de tabel weer. Na het toepassen van het beleid voor ranger, zien we alleen de laatste vier tekens van de kolom.

    ![demo-tabel na het toepassen van beleid voor ranger](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-after-ranger-policy.png)

## <a name="next-steps"></a>Volgende stappen

* [Interactive Query gebruiken met HDInsight](https://docs.microsoft.com/azure/hdinsight/interactive-query/apache-interactive-query-get-started)
* [Voorbeelden van interactie met Hive Warehouse-Connector met behulp van Zeppelin, Livy, spark-submit, en pyspark](https://community.hortonworks.com/articles/223626/integrating-apache-hive-with-apache-spark-hive-war.html)
