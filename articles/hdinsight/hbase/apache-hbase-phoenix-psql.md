---
title: Bulksgewijs laden in het Apache Phoenix met psql - Azure HDInsight
description: Het hulpprogramma psql gebruiken voor het bulksgewijs gegevens laden in Phoenix-tabellen laden.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: ashishth
ms.openlocfilehash: 5faea45a55d69cece56137d70862d80dfe335971
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56342453"
---
# <a name="bulk-load-data-into-apache-phoenix-using-psql"></a>Bulksgewijs gegevens laden in Apache Phoenix met psql

[Apache Phoenix](https://phoenix.apache.org/) is een open source, uiterst parallelle relationele database die is gebouwd op [Apache HBase](../hbase/apache-hbase-overview.md). Phoenix bevat een SQL-achtige query's over HBase. Phoenix maakt gebruik van JDBC-stuurprogramma's zodat gebruikers kunnen maken, verwijderen en SQL-tabellen, indexen, weergaven en reeksen en upsert rijen afzonderlijk en bulksgewijs wijzigen. Phoenix noSQL systeemeigen compilatie in plaats van MapReduce gebruiken voor het compileren van query's, gebruikt om toepassingen boven op HBase met lage latentie te maken. Phoenix CO-processoren ter ondersteuning van de uitvoering van code client wordt geleverd in de adresruimte van de server uitvoeren van de code die CO-locatie met de gegevens wordt toegevoegd. Dit verkleint client/server-gegevens worden overgebracht.  Als u wilt werken met gegevens in HDInsight met behulp van Phoenix, eerst tabellen maken en vervolgens gegevens laden in deze.

## <a name="bulk-loading-with-apache-phoenix"></a>Bulksgewijs laden met Apache Phoenix

Er zijn meerdere manieren voor het ophalen van gegevens in HBase, inclusief met behulp van de client-API's, een MapReduce-taak met TableOutputFormat, of de gegevens handmatig met behulp van de HBase-shell invoeren. Phoenix biedt twee methoden voor het CSV-gegevens laden in Phoenix tabellen: een client met de naam hulpmiddel laden `psql`, en een hulpprogramma voor MapReduce gebaseerde bulksgewijs laden.

De `psql` hulpprogramma is één thread en is het meest geschikt is voor het laden van megabytes of gigabytes aan gegevens. Alle CSV-bestanden moeten worden geladen, moeten het bestandsextensie hebben.  U kunt ook opgeven de SQL-scriptbestanden in de `psql` vanaf de opdrachtregel met de extensie '.sql'.

Bulksgewijs laden met MapReduce wordt gebruikt voor veel grotere gegevensvolumes, meestal in productiescenario's, zoals MapReduce maakt gebruik van meerdere threads.

Voordat u het laden van gegevens, controleert u of dat Phoenix is ingeschakeld en dat query-time-outinstellingen zoals verwacht.  Toegang tot uw HDInsight-cluster [Apache Ambari](https://ambari.apache.org/) dashboard, selecteer HBase en vervolgens het tabblad configuratie.  Schuif omlaag om te controleren of Apache Phoenix is ingesteld op `enabled` zoals wordt weergegeven:

![Instellingen van Apache Phoenix HDInsight-Cluster](./media/apache-hbase-phoenix-psql/ambari-phoenix.png)

### <a name="use-psql-to-bulk-load-tables"></a>Gebruik `psql` voor bulksgewijs laden tabellen

1. Een nieuwe tabel maken, en sla uw query bestandsnaam `createCustomersTable.sql`.

    ```sql
    CREATE TABLE Customers (
        ID varchar NOT NULL PRIMARY KEY,
        Name varchar,
        Income decimal,
        Age INTEGER,
        Country varchar);
    ```

2. Kopiëren van uw CSV-bestand (voorbeeld van de inhoud wordt weergegeven) als `customers.csv` in een `/tmp/` Active directory voor het laden in de zojuist gemaakte tabel.  Gebruik de `hdfs` opdracht het CSV-bestand kopiëren naar de gewenste locatie.

    ```
    1,Samantha,260000.0,18,US
    2,Sam,10000.5,56,US
    3,Anton,550150.0,Norway
    ... 4997 more rows 
    ```

    ```bash
    hdfs dfs -copyToLocal /example/data/customers.csv /tmp/
    ```

3. Maak een SQL SELECT-query om te controleren of de ingevoerde gegevens goed zijn geladen en sla vervolgens uw query bestandsnaam `listCustomers.sql`. U kunt een SQL-query gebruiken.
     ```sql
    SELECT Name, Income from Customers group by Country;
    ```

4. Bulksgewijs laden van de gegevens door het openen van een *nieuwe* Hadoop-opdrachtvenster. Eerst wijzigen in de uitvoering van Active directory-locatie met de `cd` opdracht in en gebruik vervolgens de `psql` hulpprogramma (Python `psql.py` opdracht). 

    Het volgende voorbeeld wordt verwacht dat u hebt gekopieerd de `customers.csv` bestand van een storage-account naar uw lokale tijdelijke map met `hdfs` zoals in stap 2 hierboven.

    ```bash
    cd /usr/hdp/current/phoenix-client/bin

    python psql.py ZookeeperQuorum createCustomersTable.sql /tmp/customers.csv listCustomers.sql
    ```

    > [!NOTE]   
    > Om te bepalen de `ZookeeperQuorum` -naam, zoek de [Apache ZooKeeper](https://zookeeper.apache.org/) quorum-tekenreeks in het bestand `/etc/hbase/conf/hbase-site.xml` met de naam van eigenschap `hbase.zookeeper.quorum`.

5. Na de `psql` bewerking is voltooid, ziet u een bericht in het opdrachtvenster:

    ```
    CSV Upsert complete. 5000 rows upserted
    Time: 4.548 sec(s)
    ```

## <a name="use-mapreduce-to-bulk-load-tables"></a>MapReduce gebruiken met bulksgewijs laden tabellen

Gebruik voor het laden van hogere doorvoer verdeeld zijn over het cluster, het hulpprogramma voor het laden van MapReduce. Deze loader zet eerst alle gegevens in HFiles en geeft vervolgens de gemaakte HFiles op HBase.

1. Starten van het CSV-MapReduce-laadprogramma met behulp van de `hadoop` opdracht met de jar Phoenix-client:

    ```bash
    hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table CUSTOMERS --input /data/customers.csv
    ```

2. Een nieuwe tabel maken met een SQL-instructie, net als bij `CreateCustomersTable.sql` in de vorige stap 1.

3. Uitvoeren om te controleren of het schema van uw tabel, `!describe inputTable`.

4. Bepalen van het pad naar de locatie voor uw invoergegevens, zoals in het voorbeeld `customers.csv` bestand. De invoerbestanden mogelijk zijn in uw WASB/ADLS-opslagaccount. In dit voorbeeldscenario wordt de invoerbestanden zich in de `<storage account parent>/inputFolderBulkLoad` directory.

5. Ga naar de map kan worden uitgevoerd voor de MapReduce-opdracht voor bulksgewijs laden:

    ```bash
    cd /usr/hdp/current/phoenix-client/bin
    ```

6. Zoek uw `ZookeeperQuorum` waarde in de `/etc/hbase/conf/hbase-site.xml`, met de naam van eigenschap `hbase.zookeeper.quorum`.

7. Instellen van het klassepad en voer de `CsvBulkLoadTool` hulpprogramma opdracht:

    ```bash
    /usr/hdp/current/phoenix-client$ HADOOP_CLASSPATH=/usr/hdp/current/hbase-client/lib/hbase-protocol.jar:/etc/hbase/conf hadoop jar /usr/hdp/2.4.2.0-258/phoenix/phoenix-4.4.0.2.4.2.0-258-client.jar

    org.apache.phoenix.mapreduce.CsvBulkLoadTool --table Customers --input /inputFolderBulkLoad/customers.csv –zookeeper ZookeeperQuorum:2181:/hbase-unsecure
    ```

8. Als u wilt MapReduce gebruiken met Azure Data Lake Storage, zoek de Data Lake Storage-hoofdmap is de `hbase.rootdir` waarde in de `hbase-site.xml`. In de volgende opdracht, de Data Lake Storage-hoofdmap is `adl://hdinsightconf1.azuredatalakestore.net:443/hbase1`. In deze opdracht geeft u de Data Lake Storage-invoer en uitvoer mappen als parameters:

    ```bash
    cd /usr/hdp/current/phoenix-client

    $HADOOP_CLASSPATH=$(hbase mapredcp):/etc/hbase/conf hadoop jar /usr/hdp/2.4.2.0-258/phoenix/phoenix-4.4.0.2.4.2.0-258-client.jar

    org.apache.phoenix.mapreduce.CsvBulkLoadTool --table Customers --input adl://hdinsightconf1.azuredatalakestore.net:443/hbase1/data/hbase/temp/input/customers.csv –zookeeper ZookeeperQuorum:2181:/hbase-unsecure --output  adl://hdinsightconf1.azuredatalakestore.net:443/hbase1/data/hbase/output1
    ```

## <a name="recommendations"></a>Aanbevelingen

* Gebruik de dezelfde opslagmedium voor invoer en uitvoer mappen, Azure Storage (WASB) of Azure Data Lake-opslag (ADL). Als u wilt gegevens overbrengen naar Azure-opslag naar Data Lake-opslag, kunt u de `distcp` opdracht:

    ```bash
    hadoop distcp wasb://@.blob.core.windows.net/example/data/gutenberg adl://.azuredatalakestore.net:443/myfolder
    ```

* Groter worker-knooppunten gebruiken. De kaart processen van de MapReduce bulksgewijs kopiëren produceren van grote hoeveelheden tijdelijke uitvoer die vol zijn de beschikbare niet-DFS-ruimte. Gebruik voor een grote hoeveelheid bulksgewijs laden, meer en groter worker-knooppunten. Het aantal worker-knooppunten die u aan uw cluster rechtstreeks toewijst van invloed op de verwerkingssnelheid van de.

* Splits de invoerbestanden in segmenten met ~ 10 GB. Bulksgewijs laden is een opslag-intensieve bewerking, zodat uw invoerbestanden te splitsen in meerdere segmenten resulteert in betere prestaties.

* Regio server hotspots voorkomen. Als de rijsleutel gestaag toenemende is, mogelijk HBase sequentiële schrijfbewerkingen regio server hotspotting veroorzaken. *Zouten* de rijsleutel sequentiële schrijfbewerkingen vermindert. Phoenix biedt een manier om transparant salt de rijsleutel met een salting byte voor een bepaalde tabel, zoals hieronder waarnaar wordt verwezen.

## <a name="next-steps"></a>Volgende stappen

* [Bulksgewijs laden van gegevens met Apache Phoenix](https://phoenix.apache.org/bulk_dataload.html)
* [Apache Phoenix gebruiken met Apache HBase op basis van Linux-clusters in HDInsight](../hbase/apache-hbase-phoenix-squirrel-linux.md)
* [Gezouten tabellen](https://phoenix.apache.org/salted.html)
* [Apache Phoenix-grammatica](https://phoenix.apache.org/language/index.html)
