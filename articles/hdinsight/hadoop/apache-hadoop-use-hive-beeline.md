---
title: Apache Beeline gebruiken met Apache Hive - Azure HDInsight
description: Informatie over het gebruik van de client Beeline Hive-query's uitvoeren met Hadoop op HDInsight. Beeline is een hulpprogramma voor het werken met HiveServer2 via JDBC.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
keywords: beeline hive, hive beeline
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: hrasheed
ms.openlocfilehash: ba9746566f0f69ea2131b8f77a14939ea561638a
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58200478"
---
# <a name="use-the-apache-beeline-client-with-apache-hive"></a>De client Apache Beeline gebruiken met Apache Hive

Meer informatie over het gebruik van [Apache Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) Apache Hive-query's uitvoeren op HDInsight.

Beeline is een Hive-client die is opgenomen op de hoofdknooppunten van het HDInsight-cluster. Beeline maakt JDBC verbinding met HiveServer2, een service die wordt gehost op uw HDInsight-cluster. U kunt ook Beeline gebruiken voor toegang tot de Hive in HDInsight op afstand via internet. De volgende voorbeelden ziet u de meest voorkomende verbindingsreeksen gebruikt vanaf Beeline verbinding maken met HDInsight:

* __Beeline van een SSH-verbinding met een hoofdknooppunt of edge-knooppunt__: `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'`
* __Beeline gebruiken op een client via een virtueel Azure-netwerk verbinding maken met HDInsight__: `-u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'`
* __Beeline gebruiken op een client, verbinding maken met een Enterprise Security Package (ESP) van HDInsight-cluster via een Azure Virtual Network__: `-u 'jdbc:hive2://<headnode-FQDN>:10001/default;principal=hive/_HOST@<AAD-Domain>;auth-kerberos;transportMode=http' -n <username>`
* __Beeline gebruiken op een client, via het openbare internet verbinding maken met HDInsight__: `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password`

> [!NOTE]  
> Vervang `admin` met het aanmeldingsaccount van het cluster voor uw cluster.
>
> Vervang `password` door het wachtwoord voor de aanmeldingsaccount van het cluster.
>
> Vervang `clustername` door de naam van uw HDInsight-cluster.
>
> Wanneer u verbinding maakt met het cluster via een virtueel netwerk, Vervang `<headnode-FQDN>` met de volledig gekwalificeerde domeinnaam van het hoofdknooppunt van een cluster.
>
> Wanneer u verbinding maakt met een Enterprise Security Package (ESP)-cluster, Vervang `<AAD-Domain>` met de naam van de Azure Active Directory (AAD) die het cluster is gekoppeld aan. Vervang `<username>` met de naam van een account in het domein met machtigingen voor toegang tot het cluster.

## <a id="prereq"></a>Vereisten

* Een op Linux gebaseerde Hadoop op HDInsight-clusterversie 3.4 of hoger.

  > [!IMPORTANT]  
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Een SSH-client of een lokale Beeline-client. De meeste van de stappen in dit document wordt ervan uitgegaan dat u van Beeline van een SSH-sessie met het cluster gebruikmaakt. Zie voor meer informatie over het uitvoeren van Beeline van buiten het cluster de [Beeline op afstand gebruiken](#remote) sectie.

    Zie voor meer informatie over het gebruik van SSH [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="beeline"></a>Een Hive-query uitvoeren

1. Bij het starten van Beeline, moet u een verbindingsreeks opgeven voor HiveServer2 op uw HDInsight-cluster:

    * Wanneer u verbinding maakt via het openbare internet, kunt u de accountnaam van de cluster-aanmelding moet opgeven (standaard `admin`) en het wachtwoord. Beeline bijvoorbeeld door middel van een clientsysteem verbinding maken met de `<clustername>.azurehdinsight.net` adres. Deze verbinding wordt gemaakt via poort `443`, en is versleuteld met SSL:

        ```bash
        beeline -u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password
        ```

    * Bij het verbinden van een SSH-sessie met het hoofdknooppunt van een cluster, kunt u verbinding maken met de `headnodehost` adres op poort `10001`:

        ```bash
        beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
        ```

    * Wanneer u verbinding maakt via een virtueel Azure-netwerk, moet u de volledig gekwalificeerde domeinnaam (FQDN) van het hoofdknooppunt van een cluster opgeven. Omdat deze verbinding wordt gemaakt rechtstreeks aan de clusterknooppunten, de verbinding gebruikmaakt van poort `10001`:

        ```bash
        beeline -u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'
        ```
    * Wanneer u verbinding maken met een Enterprise Security Package (ESP)-cluster is toegevoegd aan Azure Active Directory (AAD), moet u ook de domeinnaam opgeven `<AAD-Domain>` en de naam van een domeingebruikersaccount met machtigingen voor toegang tot het cluster `<username>`:
        
        ```bash
        kinit <username>
        beeline -u 'jdbc:hive2://<headnode-FQDN>:10001/default;principal=hive/_HOST@<AAD-Domain>;auth-kerberos;transportMode=http' -n <username>
        ```

2. Beeline opdrachten beginnen met een `!` teken, bijvoorbeeld `!help` wordt help weergegeven. Maar het `!` voor bepaalde opdrachten kunnen worden weggelaten. Bijvoorbeeld, `help` werkt ook.

    Er is een `!sql`, die wordt gebruikt voor het uitvoeren van HiveQL-instructies. HiveQL wordt echter zo vaak gebruikt dat u de voorgaande kunt weglaten `!sql`. De volgende twee instructies zijn gelijkwaardig:

    ```hiveql
    !sql show tables;
    show tables;
    ```

    Een nieuw cluster, slechts één tabel wordt vermeld: **hivesampletable**.

3. Gebruik de volgende opdracht om het schema voor de hivesampletable weer te geven:

    ```hiveql
    describe hivesampletable;
    ```

    Met deze opdracht retourneert de volgende informatie:

        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Deze informatie beschrijft de kolommen in de tabel.

4. Voer de volgende instructies voor het maken van een tabel met de naam **log4jLogs** met behulp van voorbeeldgegevens die zijn voorzien van het HDInsight-cluster:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (
        t1 string,
        t2 string,
        t3 string,
        t4 string,
        t5 string,
        t6 string,
        t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs 
        WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' 
        GROUP BY t4;
    ```

    Deze instructies uit uitvoeren de volgende acties:

    * `DROP TABLE` -Als de tabel bestaat, wordt deze verwijderd.

    * `CREATE EXTERNAL TABLE` -Maakt een **externe** tabel in Hive. Externe tabellen worden alleen de definitie van de tabel opslaan in Hive. De gegevens is achtergebleven in de oorspronkelijke locatie.

    * `ROW FORMAT` -Hoe de gegevens zijn opgemaakt. In dit geval worden de velden in elk logboek gescheiden door een spatie.

    * `STORED AS TEXTFILE LOCATION` -Wanneer de gegevens worden opgeslagen en in welke indeling.

    * `SELECT` : Hiermee kunt u een aantal van alle rijen waar kolom **t4** bevat de waarde **[fout]**. Deze query retourneert een waarde van **3** als er drie rijen die deze waarde bevatten.

    * `INPUT__FILE__NAME LIKE '%.log'` -Hive probeert het schema toepassen op alle bestanden in de map. In dit geval bevat de map bestanden die niet overeenkomen met het schema. Om te voorkomen dat garbagecollection-gegevens in de resultaten, deze instructie vertelt Hive dat deze alleen gegevens uit bestanden hebben die eindigt op moet retourneren. log.

   > [!NOTE]  
   > Externe tabellen moeten worden gebruikt wanneer u verwacht dat de onderliggende gegevens worden bijgewerkt door een externe bron. Bijvoorbeeld, een geautomatiseerd proces of een MapReduce-bewerking uploaden.
   >
   > Verwijderen van een externe tabel heeft **niet** de gegevens, alleen de definitie van de tabel verwijderen.

    De uitvoer van deze opdracht is vergelijkbaar met de volgende tekst:

        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :

        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)

        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

5. Gebruiken om af te sluiten Beeline `!exit`.

### <a id="file"></a>Beeline gebruiken een HiveQL-bestand uit te voeren

Gebruik de volgende stappen uit om te maken van een bestand en voer vervolgens met behulp van Beeline.

1. Gebruik de volgende opdracht om te maken van een bestand met de naam **query.hql**:

    ```bash
    nano query.hql
    ```

2. Gebruik de volgende tekst als de inhoud van het bestand. Deze query maakt u een nieuwe 'intern' tabel met de naam **foutenlogboeken**:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Deze instructies uit uitvoeren de volgende acties:

   * **MAKEN van tabel als niet bestaat** -als de tabel nog niet bestaat, wordt dit gemaakt. Omdat de **externe** trefwoord wordt niet gebruikt, wordt deze instructie maakt u een interne tabel. Interne tabellen worden opgeslagen in het datawarehouse Hive en volledig worden beheerd door Hive.
   * **OPGESLAGEN als ORC** -de gegevens worden opgeslagen in geoptimaliseerd rij kolommen (ORC)-indeling. ORC-indeling is een uiterst geoptimaliseerde en efficiënte indeling voor het opslaan van gegevens met Hive.
   * **INSERT OVERWRITE ... Selecteer** -selecteert rijen uit de **log4jLogs** tabel met **[fout]**, voegt u vervolgens de gegevens in de **foutenlogboeken** tabel.

     > [!NOTE]  
     > In tegenstelling tot externe tabellen, het verwijderen van een interne tabel Hiermee verwijdert u ook de onderliggende gegevens.

3. Gebruiken om het bestand hebt opgeslagen, **Ctrl**+**_X**, voert u **Y**, en tot slot **Enter**.

4. Gebruik de volgende in het bestand met behulp van Beeline uit te voeren:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i query.hql
    ```

    > [!NOTE]  
    > De `-i` parameter Beeline wordt gestart en voert u de instructies de `query.hql` bestand. Nadat de query is voltooid, wordt er uitziet de `jdbc:hive2://headnodehost:10001/>` prompt. U kunt ook uitvoeren voor een bestand met de `-f` parameter, die Beeline afgesloten nadat de query is voltooid.

5. Om te controleren of de **foutenlogboeken** tabel is gemaakt, gebruikt u de volgende instructie om te retourneren van alle rijen uit **foutenlogboeken**:

    ```hiveql
    SELECT * from errorLogs;
    ```

    Drie rijen met gegevens moeten worden geretourneerd, allemaal met **[fout]** in kolom t4:

        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a id="remote"></a>Op afstand Beeline gebruiken

Als u Beeline lokaal is geïnstalleerd, en verbinding via het openbare internet maken, gebruikt u de volgende parameters:

* __Verbindingsreeks__: `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`

* __De naam van de cluster-aanmelding__: `-n admin`

* __Wachtwoord voor clusteraanmelding__ `-p 'password'`

Vervang de `clustername` in de verbindingsreeks met de naam van uw HDInsight-cluster.

Vervang `admin` met de naam van uw aanmelding bij cluster en vervang `password` door het wachtwoord voor uw aanmelding bij cluster.

Als u Beeline lokaal is geïnstalleerd, en verbinding via een virtueel Azure-netwerk maken, gebruikt u de volgende parameters:

* __Verbindingsreeks__: `-u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'`

Als u zoekt de volledig gekwalificeerde domeinnaam van een hoofdknooppunt, gebruik de informatie in de [beheren HDInsight met behulp van de Apache Ambari REST-API](../hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) document.

## <a id="sparksql"></a>Beeline gebruiken met Apache Spark

Apache Spark biedt een eigen implementatie van HiveServer2, die wordt soms aangeduid als de Spark Thrift-server. Deze service maakt gebruik van Spark SQL om op te lossen query's in plaats van Hive en kan zorgen voor betere prestaties, afhankelijk van uw query.

De __verbindingsreeks__ gebruikt bij het verbinding maken via internet is enigszins anders. In plaats van `httpPath=/hive2` is `httpPath/sparkhive2`. Hier volgt een voorbeeld van verbinding maken via internet:

```bash 
beeline -u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/sparkhive2' -n admin -p password
```

Wanneer u verbinding maakt rechtstreeks vanuit het hoofdknooppunt van het cluster, of vanuit een bron in het hetzelfde virtuele Azure-netwerk als het HDInsight-cluster, poort `10002` moet worden gebruikt voor Spark Thrift-server in plaats van `10001`. Hier volgt een voorbeeld van een verbinding met rechtstreeks naar het hoofdknooppunt:

```bash
beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'
```

## <a id="summary"></a><a id="nextsteps"></a>De volgende stappen

Zie het volgende document voor meer algemene informatie over Hive in HDInsight:

* [Apache Hive gebruiken met Apache Hadoop op HDInsight](hdinsight-use-hive.md)

Zie de volgende documenten voor meer informatie over andere manieren waarop u kunt werken met Hadoop op HDInsight:

* [Apache Pig gebruiken met Apache Hadoop op HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met Apache Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Als u met Hive Tez gebruikt, raadpleegt u het volgende document: [Gebruik de weergave van de Apache Ambari Tez op Linux gebaseerde HDInsight](../hdinsight-debug-ambari-tez-view.md).

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[apache-tez]: https://tez.apache.org
[apache-hive]: https://hive.apache.org/
[apache-log4j]: https://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: https://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md

[putty]: https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: https://technet.microsoft.com/library/ee692792.aspx
