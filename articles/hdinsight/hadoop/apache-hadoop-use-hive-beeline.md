---
title: Apache Beeline gebruiken met Apache Hive - Azure HDInsight
description: Informatie over het gebruik van de client Beeline Hive-query's uitvoeren met Hadoop op HDInsight. Beeline is een hulpprogramma voor het werken met HiveServer2 via JDBC.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: hrasheed
ms.openlocfilehash: 89303e5c827fc24540d345a9a2b9a0743e453a4d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59257124"
---
# <a name="use-the-apache-beeline-client-with-apache-hive"></a>De client Apache Beeline gebruiken met Apache Hive

Meer informatie over het gebruik van [Apache Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) Apache Hive-query's uitvoeren op HDInsight.

Beeline is een Hive-client die is opgenomen op de hoofdknooppunten van het HDInsight-cluster. Beeline maakt JDBC verbinding met HiveServer2, een service die wordt gehost op uw HDInsight-cluster. U kunt ook Beeline gebruiken voor toegang tot de Hive in HDInsight op afstand via internet. De volgende voorbeelden ziet u de meest voorkomende verbindingsreeksen gebruikt vanaf Beeline verbinding maken met HDInsight:

## <a name="types-of-connections"></a>Typen verbindingen

### <a name="from-an-ssh-session"></a>Vanuit een SSH-sessie

Bij het verbinden van een SSH-sessie met het hoofdknooppunt van een cluster, u kunt vervolgens verbinding maken met de `headnodehost` adres op poort `10001`:

```bash
beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
```

---

### <a name="over-an-azure-virtual-network"></a>Via een Azure-netwerk

Bij het verbinden van een client met HDInsight over een Azure-netwerk, moet u de volledig gekwalificeerde domeinnaam (FQDN) van het hoofdknooppunt van een cluster opgeven. Omdat deze verbinding wordt gemaakt rechtstreeks aan de clusterknooppunten, de verbinding gebruikmaakt van poort `10001`:

```bash
beeline -u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'
```

Vervang `<headnode-FQDN>` met de volledig gekwalificeerde domeinnaam van het hoofdknooppunt van een cluster. Als u zoekt de volledig gekwalificeerde domeinnaam van een hoofdknooppunt, gebruik de informatie in de [beheren HDInsight met behulp van de Apache Ambari REST-API](../hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) document.

---

### <a name="to-hdinsight-enterprise-security-package-esp-cluster"></a>Met HDInsight Enterprise Security Package (ESP)-cluster

Wanneer u verbinding maakt vanaf een client met een Enterprise Security Package (ESP)-cluster is toegevoegd aan Azure Active Directory (AAD), moet u ook de domeinnaam opgeven `<AAD-Domain>` en de naam van een domeingebruikersaccount met machtigingen voor toegang tot het cluster `<username>`:

```bash
kinit <username>
beeline -u 'jdbc:hive2://<headnode-FQDN>:10001/default;principal=hive/_HOST@<AAD-Domain>;auth-kerberos;transportMode=http' -n <username>
```

Vervang `<username>` met de naam van een account in het domein met machtigingen voor toegang tot het cluster. Vervang `<AAD-DOMAIN>` met de naam van de Azure Active Directory (AAD) die het cluster is gekoppeld aan. Gebruik een hoofdletter tekenreeks voor de `<AAD-DOMAIN>` waarde, anders kan de referentie wordt niet vinden. Controleer `/etc/krb5.conf` voor de realmnamen van de indien nodig.

---

### <a name="over-public-internet"></a>Via openbaar internet

Wanneer u verbinding maakt via het openbare internet, kunt u de accountnaam van de cluster-aanmelding moet opgeven (standaard `admin`) en het wachtwoord. Beeline bijvoorbeeld door middel van een clientsysteem verbinding maken met de `<clustername>.azurehdinsight.net` adres. Deze verbinding wordt gemaakt via poort `443`, en is versleuteld met SSL:

```bash
beeline -u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password
```

Vervang `clustername` door de naam van uw HDInsight-cluster. Vervang `admin` met het aanmeldingsaccount van het cluster voor uw cluster. Vervang `password` door het wachtwoord voor de aanmeldingsaccount van het cluster.

---

### <a id="sparksql"></a>Beeline gebruiken met Apache Spark

Apache Spark biedt een eigen implementatie van HiveServer2, die wordt soms aangeduid als de Spark Thrift-server. Deze service maakt gebruik van Spark SQL om op te lossen query's in plaats van Hive en kan zorgen voor betere prestaties, afhankelijk van uw query.

#### <a name="over-public-internet-with-apache-spark"></a>Via het openbare internet met Apache Spark

De verbindingsreeks die wordt gebruikt wanneer u verbinding maakt via internet is enigszins anders. In plaats van `httpPath=/hive2` is `httpPath/sparkhive2`:

```bash 
beeline -u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/sparkhive2' -n admin -p password
```

---

#### <a name="from-cluster-head-or-inside-azure-virtual-network-with-apache-spark"></a>Van hoofd- of binnen Azure Virtual Network met Apache Spark-cluster

Wanneer u verbinding maakt rechtstreeks vanuit het hoofdknooppunt van het cluster, of vanuit een bron in het hetzelfde virtuele Azure-netwerk als het HDInsight-cluster, poort `10002` moet worden gebruikt voor Spark Thrift-server in plaats van `10001`. Het volgende voorbeeld ziet u hoe u rechtstreeks verbinding maken met het hoofdknooppunt:

```bash
beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'
```

---

## <a id="prereq"></a>Vereisten

* Een Hadoop-cluster op HDInsight. Zie [aan de slag met HDInsight op Linux](./apache-hadoop-linux-tutorial-get-started.md).

* U ziet dat de [URI-schema](../hdinsight-hadoop-linux-information.md#URI-and-scheme) voor primaire opslag van uw cluster. Bijvoorbeeld, `wasb://` voor Azure Storage, `abfs://` voor Azure Data Lake Storage Gen2 of `adl://` voor Azure Data Lake Storage Gen1. Als veilige overdracht is ingeschakeld voor Azure Storage of Data Lake Storage Gen2, de URI is `wasbs://` of `abfss://`, respectievelijk. Zie voor meer informatie, [veilige overdracht](../../storage/common/storage-require-secure-transfer.md).


* Optie 1: Een SSH-client. Zie voor meer informatie [Verbinding maken met HDInsight (Apache Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md). De meeste van de stappen in dit document wordt ervan uitgegaan dat u van Beeline van een SSH-sessie met het cluster gebruikmaakt.

* Optie 2:  Een lokale Beeline-client.


## <a id="beeline"></a>Een Hive-query uitvoeren

In dit voorbeeld is gebaseerd op het gebruik van de client Beeline via een SSH-verbinding.

1. Open een SSH-verbinding met het cluster met de volgende code. Vervang `sshuser` door de SSH-gebruiker voor uw cluster en `CLUSTERNAME` door de naam van het cluster. Wanneer u hierom wordt gevraagd, voert u het wachtwoord voor de SSH-gebruikersaccount.

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Verbinding maken met HiveServer2 met uw client Beeline vanuit uw SSH-sessie openen met de volgende opdracht:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

3. Beeline opdrachten beginnen met een `!` teken, bijvoorbeeld `!help` wordt help weergegeven. Maar het `!` voor bepaalde opdrachten kunnen worden weggelaten. Bijvoorbeeld, `help` werkt ook.

    Er is een `!sql`, die wordt gebruikt voor het uitvoeren van HiveQL-instructies. HiveQL wordt echter zo vaak gebruikt dat u de voorgaande kunt weglaten `!sql`. De volgende twee instructies zijn gelijkwaardig:

    ```hiveql
    !sql show tables;
    show tables;
    ```

    Een nieuw cluster, slechts één tabel wordt vermeld: **hivesampletable**.

4. Gebruik de volgende opdracht om het schema voor de hivesampletable weer te geven:

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

5. Voer de volgende instructies voor het maken van een tabel met de naam **log4jLogs** met behulp van voorbeeldgegevens die zijn voorzien van het HDInsight-cluster: (Herzien zo nodig op basis van uw [URI-schema](../hdinsight-hadoop-linux-information.md#URI-and-scheme).)

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
    STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
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

6. Gebruiken om af te sluiten Beeline `!exit`.

## <a id="file"></a>Een bestand HiveQL uitvoeren

Dit is een vervolg van het vorige voorbeeld. Gebruik de volgende stappen uit om te maken van een bestand en voer vervolgens met behulp van Beeline.

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




## <a id="summary"></a><a id="nextsteps"></a>De volgende stappen

Zie het volgende document voor meer algemene informatie over Hive in HDInsight:

* [Apache Hive gebruiken met Apache Hadoop op HDInsight](hdinsight-use-hive.md)

Zie de volgende documenten voor meer informatie over andere manieren waarop u kunt werken met Hadoop op HDInsight:

* [Apache Pig gebruiken met Apache Hadoop op HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met Apache Hadoop op HDInsight](hdinsight-use-mapreduce.md)

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[apache-tez]: https://tez.apache.org
[apache-hive]: https://hive.apache.org/
[apache-log4j]: https://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: https://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md

[putty]: https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: https://technet.microsoft.com/library/ee692792.aspx
