---
title: Bulksgewijs laden in Apache Phoenix met psql - Azure HDInsight | Microsoft Docs
description: Gebruik het hulpprogramma psql bulksgewijs laden gegevens laadt in Phoenix tabellen.
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/10/2017
ms.author: ashishth
ms.openlocfilehash: 0a623113ee9f3fe2c0f5f616ecd79b8311a8ffc1
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="bulk-load-data-into-phoenix-using-psql"></a>Bulksgewijs laden gegevens in Phoenix psql gebruiken

[Apache Phoenix](http://phoenix.apache.org/) is een open-source, massively parallelle relationele database gebouwd op [HBase](../hbase/apache-hbase-overview.md). Phoenix bevat een SQL-achtige query's over HBase. Phoenix gebruikt JDBC-stuurprogramma's voor gebruikers te maken, verwijderen en SQL-tabellen, indexen, weergaven en reeksen en upsert rijen afzonderlijk en bulksgewijs veranderen. Phoenix maakt gebruik van systeemeigen noSQL-compilatie in plaats van de MapReduce voor het compileren van query's, lage latentie toepassingen boven op HBase te maken. Phoenix voegt CO-processoren ter ondersteuning van de client opgegeven code uitgevoerd in de adresruimte van de server, de code CO-locatie met de gegevens uitvoert. Hierdoor minimaliseert client/server-gegevens worden overgedragen.  Als u wilt werken met gegevens met Phoenix in HDInsight, eerst tabellen maken en vervolgens gegevens laden in deze.

## <a name="bulk-loading-with-phoenix"></a>Bulksgewijs laden met Phoenix

Er zijn meerdere manieren om gegevens naar HBase inclusief met behulp van client-API's, een MapReduce-taak met TableOutputFormat, of de gegevens handmatig met de HBase-shell invoeren. Phoenix biedt twee methoden voor het CSV-gegevens in Phoenix tabellen te laden: een client met de naam hulpmiddel laden `psql`, en een hulpprogramma voor MapReduce gebaseerde bulksgewijs laden.

De `psql` hulpprogramma is één thread en is het meest geschikt is voor het laden van megabytes of gigabytes van gegevens. Alle CSV-bestanden moeten worden geladen, moeten de bestandsextensie '.csv' hebben.  U kunt ook opgeven SQL scriptbestanden in de `psql` vanaf de opdrachtregel met de extensie '.sql'.

Bulksgewijs laden met MapReduce wordt gebruikt voor veel grotere volumes, doorgaans in productiescenario's zoals MapReduce maakt gebruik van meerdere threads.

Voordat u begint met het laden van gegevens, moet u controleren of Phoenix is ingeschakeld en dat de query time-outinstellingen zijn zoals verwacht.  Toegang tot uw HDInsight-cluster Ambari-dashboard, selecteert u HBase en klik vervolgens op het tabblad configuratie.  Schuif omlaag om te controleren of de Apache Phoenix is ingesteld op `enabled` zoals wordt weergegeven:

![Apache Phoenix HDInsight-Cluster-instellingen](./media/apache-hbase-phoenix-psql/ambari-phoenix.png)

### <a name="use-psql-to-bulk-load-tables"></a>Gebruik `psql` aan tabellen voor bulksgewijs laden

1. Een nieuwe tabel maken en sla vervolgens uw query met filename `createCustomersTable.sql`.

    ```sql
    CREATE TABLE Customers (
        ID varchar NOT NULL PRIMARY KEY,
        Name varchar,
        Income decimal,
        Age INTEGER,
        Country varchar);
    ```

2. Kopiëren van uw CSV-bestand (bijvoorbeeld inhoud weergegeven) als `customers.csv` in een `/tmp/` map voor laden naar uw nieuwe tabel.  Gebruik de `hdfs` opdracht voor het CSV-bestand kopiëren naar de gewenste bronlocatie.

    ```
    1,Samantha,260000.0,18,US
    2,Sam,10000.5,56,US
    3,Anton,550150.0,Norway
    ... 4997 more rows 
    ```

    ```bash
    hdfs dfs -copyToLocal /example/data/customers.csv /tmp/
    ```

3. Een SQL SELECT query maken om te controleren of de ingevoerde gegevens correct wordt geladen en sla vervolgens uw query met filename `listCustomers.sql`. U kunt SQL-query.
     ```sql
    SELECT Name, Income from Customers group by Country;
    ```

4. Bulksgewijs laden van de gegevens via een *nieuwe* Hadoop-opdrachtvenster. Wijzigt u eerst op de locatie van de directory worden uitgevoerd met de `cd` opdracht en gebruik vervolgens de `psql` hulpprogramma (Python `psql.py` opdracht). 

    Het volgende voorbeeld wordt verwacht dat u hebt gekopieerd de `customers.csv` bestand van een opslagaccount naar uw lokale tijdelijke map met `hdfs` zoals in stap 2 hierboven.

    ```bash
    cd /usr/hdp/current/phoenix-client/bin

    python psql.py ZookeeperQuorum createCustomersTable.sql /tmp/customers.csv listCustomers.sql
    ```

    > [!NOTE] 
    > Om te bepalen de `ZookeeperQuorum` naam, de zookeeper quorum-tekenreeks niet vinden in het bestand `/etc/hbase/conf/hbase-site.xml` met de naam van eigenschap `hbase.zookeeper.quorum`.

5. Na de `psql` bewerking is voltooid, ziet u een bericht in uw opdrachtvenster:

    ```
    CSV Upsert complete. 5000 rows upserted
    Time: 4.548 sec(s)
    ```

## <a name="use-mapreduce-to-bulk-load-tables"></a>MapReduce gebruiken voor bulksgewijs laden tabellen

Voor het laden van de hogere doorvoer verdeeld over het cluster, het MapReduce-load-hulpprogramma te gebruiken. Deze loader zet eerst alle gegevens in HFiles en vindt u de gemaakte HFiles naar HBase.

1. Starten van het CSV-MapReduce-laadprogramma met behulp van de `hadoop` opdracht met de jar Phoenix-client:

    ```bash
    hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table CUSTOMERS --input /data/customers.csv
    ```

2. Een nieuwe tabel maken met een SQL-instructie, net als bij `CreateCustomersTable.sql` in de vorige stap 1.

3. Uitvoeren om te controleren of het schema van de tabel, `!describe inputTable`.

4. Bepaal het locatiepad naar de invoergegevens, zoals in het voorbeeld `customers.csv` bestand. De invoerbestanden mogelijk in uw WASB/ADLS-opslagaccount. In dit voorbeeldscenario de invoerbestanden zich in de `<storage account parent>/inputFolderBulkLoad` directory.

5. Ga naar de map kan worden uitgevoerd voor de MapReduce-opdracht voor bulksgewijs laden:

    ```bash
    cd /usr/hdp/current/phoenix-client/bin
    ```

6. Zoek uw `ZookeeperQuorum` waarde in `/etc/hbase/conf/hbase-site.xml`, met de naam van eigenschap `hbase.zookeeper.quorum`.

7. Instellen van het klassepad en voer de `CsvBulkLoadTool` hulpprogramma opdracht:

    ```bash
    /usr/hdp/current/phoenix-client$ HADOOP_CLASSPATH=/usr/hdp/current/hbase-client/lib/hbase-protocol.jar:/etc/hbase/conf hadoop jar /usr/hdp/2.4.2.0-258/phoenix/phoenix-4.4.0.2.4.2.0-258-client.jar

    org.apache.phoenix.mapreduce.CsvBulkLoadTool --table Customers --input /inputFolderBulkLoad/customers.csv –zookeeper ZookeeperQuorum:2181:/hbase-unsecure
    ```

8. Als u wilt u MapReduce gebruikt met ADLS, zoek de ADLS-hoofdmap is de `hbase.rootdir` waarde in `hbase-site.xml`. In de volgende opdracht de ADLS-hoofdmap is `adl://hdinsightconf1.azuredatalakestore.net:443/hbase1`. Geef de ADLS-invoer en uitvoer van mappen als parameters in deze opdracht:

    ```bash
    cd /usr/hdp/current/phoenix-client

    $HADOOP_CLASSPATH=$(hbase mapredcp):/etc/hbase/conf hadoop jar /usr/hdp/2.4.2.0-258/phoenix/phoenix-4.4.0.2.4.2.0-258-client.jar

    org.apache.phoenix.mapreduce.CsvBulkLoadTool --table Customers --input adl://hdinsightconf1.azuredatalakestore.net:443/hbase1/data/hbase/temp/input/customers.csv –zookeeper ZookeeperQuorum:2181:/hbase-unsecure --output  adl://hdinsightconf1.azuredatalakestore.net:443/hbase1/data/hbase/output1
    ```

## <a name="recommendations"></a>Aanbevelingen

* Gebruik het opslagmedium voor dezelfde voor invoer en uitvoer mappen, WASB of ADLS. Als u wilt gegevens van WASB overbrengen naar ADLS, kunt u de `distcp` opdracht:

    ```bash
    hadoop distcp wasb://@.blob.core.windows.net/example/data/gutenberg adl://.azuredatalakestore.net:443/myfolder
    ```

* Gebruik groter formaat worker-knooppunten. De kaart processen van het MapReduce bulkgewijs kopiëren produceren grote hoeveelheden tijdelijke uitvoer die bijna vol zijn de beschikbare niet-DFS-ruimte. Gebruik meer en groter formaat worker-knooppunten voor een grote hoeveelheid bulksgewijs laden. De verwerkingssnelheid van invloed op het aantal worker-knooppunten die u rechtstreeks aan het cluster toewijzen.

* Splits invoerbestanden in segmenten ~ 10 GB. Bulksgewijs laden is een opslag-intensieve bewerking, zodat uw invoerbestanden te verdelen in meerdere segmenten resulteert in betere prestaties.

* Vermijd regio server hotspots. Als uw rijsleutel gestaag stijgt, kunnen de opeenvolgende schrijfbewerkingen HBase regio server hotspotting tot. *Zouten* de rijsleutel opeenvolgende schrijfbewerkingen vermindert. Phoenix biedt een manier om de rijsleutel met een salting byte voor een bepaalde tabel, waarnaar wordt verwezen hieronder transparant salt.

## <a name="next-steps"></a>Volgende stappen

* [Bulksgewijs laden van gegevens met Apache Phoenix](http://phoenix.apache.org/bulk_dataload.html)
* [Apache Phoenix gebruiken met HBase op basis van Linux-clusters in HDInsight](../hbase/apache-hbase-phoenix-squirrel-linux.md)
* [Gezouten tabellen](https://phoenix.apache.org/salted.html)
* [Phoenix grammatica](http://phoenix.apache.org/language/index.html)
