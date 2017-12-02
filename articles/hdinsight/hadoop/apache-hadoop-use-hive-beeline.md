---
title: Gebruik Beeline met Apache Hive - Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van de client Beeline Hive-query's uitvoeren met Hadoop op HDInsight. Beeline is een hulpprogramma voor het werken met HiveServer2 via JDBC vast.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: beeline hive, hive beeline
ms.assetid: 3adfb1ba-8924-4a13-98db-10a67ab24fca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/01/2017
ms.author: larryfr
ms.openlocfilehash: 19c5f165b47f7de4a014226460f82f3ca12b3eec
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="use-the-beeline-client-with-apache-hive"></a>De client Beeline gebruiken met Apache Hive

Informatie over het gebruik [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) Hive-query's uitvoeren op HDInsight.

Beeline is een Hive-client die is opgenomen op de hoofdknooppunten van uw HDInsight-cluster. Beeline maakt JDBC verbinding met HiveServer2, een service die wordt gehost op uw HDInsight-cluster. U kunt ook Beeline gebruiken voor toegang tot de Hive in HDInsight op afstand via internet. De volgende voorbeelden bieden de meest voorkomende verbindingsreeksen gebruikt vanaf Beeline verbinding maken met HDInsight:

* __Van een headnode of edge-knooppunt van een SSH-verbinding Beeline__:`-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'`
* __Met behulp van Beeline op een client via een virtueel Azure-netwerk verbinding maken met HDInsight__:`-u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'`
* __Met behulp van Beeline op een client, via het openbare internet verbinding maken met HDInsight__:`-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password`

> [!NOTE]
> Vervang `admin` met de cluster-aanmeldingsaccount voor uw cluster.
>
> Vervang `password` met het wachtwoord voor de aanmeldingsaccount van het cluster.
>
> Vervang `clustername` door de naam van uw HDInsight-cluster.
>
> Wanneer u verbinding maakt met het cluster via een virtueel netwerk, vervangt `<headnode-FQDN>` met de volledig gekwalificeerde domeinnaam van een cluster headnode.

## <a id="prereq"></a>Vereisten

* Een op Linux gebaseerde Hadoop op HDInsight-cluster.

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Een SSH-client of een lokale Beeline-client. De meeste van de stappen in dit document wordt ervan uitgegaan dat u van Beeline van een SSH-sessie met het cluster gebruikmaakt. Zie voor meer informatie over het uitvoeren van Beeline van buiten het cluster de [Beeline op afstand gebruiken](#remote) sectie.

    Zie voor meer informatie over het gebruik van SSH [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="beeline"></a>Gebruik Beeline

1. Bij het starten van Beeline, moet u een verbindingsreeks opgeven voor HiveServer2 op uw HDInsight-cluster:

    * Wanneer u verbinding maakt via het openbare internet, moet u de accountnaam van de cluster-aanmelding opgeven (standaard `admin`) en het wachtwoord. Beeline bijvoorbeeld door middel van een clientsysteem verbinding maken met de `<clustername>.azurehdinsight.net` adres. Deze verbinding wordt gemaakt via poort `443`, en is versleuteld met SSL:

        ```bash
        beeline -u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password
        ```

    * Bij het verbinden van een SSH-sessie met een cluster headnode, kunt u verbinding maken met de `headnodehost` adres op poort `10001`:

        ```bash
        beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
        ```

    * Wanneer u verbinding maakt via een virtuele Azure-netwerk, moet u de volledig gekwalificeerde domeinnaam (FQDN) van het hoofdknooppunt van een cluster opgeven. Omdat deze verbinding wordt gemaakt rechtstreeks aan de clusterknooppunten, de verbinding gebruikt poort `10001`:

        ```bash
        beeline -u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'
        ```

2. Beeline opdrachten beginnen met een `!` teken, bijvoorbeeld `!help` geeft help weer. Maar het `!` voor bepaalde opdrachten kunnen worden weggelaten. Bijvoorbeeld: `help` werkt ook.

    Er is een `!sql`, dat wordt gebruikt voor het uitvoeren van HiveQL-instructies. HiveQL wordt echter zo vaak gebruikt dat kunt u de voorgaande weglaten `!sql`. De volgende twee instructies zijn gelijkwaardig:

    ```hiveql
    !sql show tables;
    show tables;
    ```

    Op een nieuw cluster slechts één tabel wordt vermeld: **hivesampletable**.

3. Gebruik de volgende opdracht om het schema voor de hivesampletable weer te geven:

    ```hiveql
    describe hivesampletable;
    ```

    Deze opdracht retourneert de volgende informatie:

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

    Deze informatie beschrijft de kolommen in de tabel. Terwijl sommige query's op deze gegevens kan worden uitgevoerd, gaan we in plaats daarvan een nieuwe tabel maken om te laten zien hoe u gegevens laadt in Hive en een schema hebt toegepast.

4. Voer de volgende instructies voor het maken van een tabel met de naam **log4jLogs** met behulp van voorbeeldgegevens voorzien van het HDInsight-cluster:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    Deze instructies uitvoeren de volgende acties:

    * `DROP TABLE`-Als de tabel bestaat, wordt deze verwijderd.

    * `CREATE EXTERNAL TABLE`-Maakt een **externe** tabel in Hive. De tabeldefinitie opslaan externe tabellen alleen in Hive. De gegevens blijft op de oorspronkelijke locatie.

    * `ROW FORMAT`-Hoe de gegevens zijn ingedeeld. In dit geval worden de velden in elk logboek gescheiden door een spatie.

    * `STORED AS TEXTFILE LOCATION`-Waarin de gegevens worden opgeslagen en in welke bestandsindeling.

    * `SELECT`: Hiermee kunt u een telling van alle rijen waarin kolom **t4** bevat de waarde **[fout]**. Deze query retourneert een waarde van **3** omdat er zijn drie rijen met deze waarde.

    * `INPUT__FILE__NAME LIKE '%.log'`-Hive probeert het schema toepassen op alle bestanden in de map. In dit geval wordt bevat de map bestanden die niet overeenkomen met het schema. Om te voorkomen dat een garbagecollection-gegevens in de resultaten, deze instructie vertelt Hive dat we alleen gegevens uit bestanden eindigt op moet retourneren. log.

  > [!NOTE]
  > Externe tabellen moeten worden gebruikt wanneer u de onderliggende gegevens wordt bijgewerkt door een externe bron verwacht. Bijvoorbeeld, een uploadproces geautomatiseerde gegevens of een MapReduce-bewerking.
  >
  > Verwijderen van een externe tabel komt **niet** de gegevens, de definitie van de tabel verwijderen.

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

## <a id="file"></a>Gebruik Beeline een HiveQL-bestand uit te voeren

Gebruik de volgende stappen uit om te maken van een bestand en voer vervolgens met behulp van Beeline.

1. Gebruik de volgende opdracht voor het maken van een bestand met de naam **query.hql**:

    ```bash
    nano query.hql
    ```

2. Gebruik de volgende tekst als de inhoud van het bestand. Deze query maakt een nieuwe 'interne' tabel met de naam **foutenlogboeken**:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Deze instructies uitvoeren de volgende acties:

    * **MAKEN van tabel als niet bestaat** -als de tabel niet al bestaat, wordt deze gemaakt. Aangezien de **externe** trefwoord wordt niet gebruikt, wordt deze instructie maakt u een interne tabel. Interne tabellen worden opgeslagen in het datawarehouse Hive en volledig worden beheerd door Hive.
    * **OPGESLAGEN AS ORC** -slaat de gegevens geoptimaliseerd rij kolommen (ORC)-indeling. ORC is een maximaal geoptimaliseerd en efficiënte indeling voor het opslaan van gegevens met Hive.
    * **INSERT OVERSCHRIJVEN... Selecteer** -selecteert rijen uit de **log4jLogs** tabel met **[fout]**, voegt u vervolgens de gegevens in de **foutenlogboeken** tabel.

    > [!NOTE]
    > In tegenstelling tot externe tabellen voor het verwijderen van een interne tabel worden verwijderd en de onderliggende gegevens.

3. Gebruiken om het bestand opslaat, **Ctrl**+**_X**, voer dan **Y**, en tot slot **Enter**.

4. Gebruik de volgende in het bestand met Beeline uit te voeren:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i query.hql
    ```

    > [!NOTE]
    > De `-i` parameter Beeline wordt gestart en voert u de instructies de `query.hql` bestand. Nadat de query is voltooid, wordt u aankomt bij de `jdbc:hive2://headnodehost:10001/>` prompt. U kunt ook uitvoeren voor een bestand met de `-f` parameter Beeline afgesloten nadat de query is voltooid.

5. Om te controleren of de **foutenlogboeken** tabel is gemaakt, met de volgende instructie retourneert alle rijen uit **foutenlogboeken**:

    ```hiveql
    SELECT * from errorLogs;
    ```

    Drie rijen met gegevens moeten worden geretourneerd, alle overkoepelende **[fout]** in kolom t4:

        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a id="remote"></a>Beeline op afstand gebruiken

Als u Beeline lokaal geïnstalleerd hebben en verbinding via het openbare internet maken, gebruikt u de volgende parameters:

* __Verbindingsreeks__:`-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`

* __Aanmeldingsnaam van de cluster__:`-n admin`

* __Aanmeldingswachtwoord cluster__`-p 'password'`

Vervang de `clustername` in de verbindingstekenreeks met de naam van uw HDInsight-cluster.

Vervang `admin` met de naam van uw cluster aanmelding en vervang `password` met het wachtwoord voor uw cluster-aanmelding.

Als u Beeline lokaal geïnstalleerd hebben en verbinding via een virtueel Azure-netwerk maken, gebruikt u de volgende parameters:

* __Verbindingsreeks__:`-u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'`

Gebruik de informatie in de volledig gekwalificeerde domeinnaam van een headnode vindt de [beheren HDInsight met de Ambari REST-API](../hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) document.

## <a id="sparksql"></a>Beeline met Spark gebruiken

Spark biedt een eigen implementatie van HiveServer2, die wordt soms aangeduid als de Spark Thrift-server. Deze service wordt Spark SQL gebruikt voor query's in plaats van Hive omzetten en kan zorgen voor betere prestaties, afhankelijk van uw query.

Poort gebruiken voor verbinding met de Spark Thrift-server van een Spark in HDInsight-cluster, `10002` in plaats van `10001`. Bijvoorbeeld `beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'`.

> [!IMPORTANT]
> De Spark Thrift-server is niet rechtstreeks toegankelijk via het internet. U kunt alleen verbinding met deze van een SSH-sessie of in het hetzelfde virtuele Azure-netwerk als het HDInsight-cluster.

## <a id="summary"></a><a id="nextsteps"></a>De volgende stappen

Zie het volgende document voor meer algemene informatie over Hive in HDInsight:

* [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md)

Zie de volgende documenten voor meer informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Pig gebruiken met Hadoop in HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Als u van Tez met Hive gebruikmaakt, raadpleegt u de volgende documenten:

* [De Tez-gebruikersinterface op HDInsight op basis van Windows gebruiken](../hdinsight-debug-tez-ui.md)
* [De weergave Ambari Tez op Linux gebaseerde HDInsight gebruiken](../hdinsight-debug-ambari-tez-view.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx
