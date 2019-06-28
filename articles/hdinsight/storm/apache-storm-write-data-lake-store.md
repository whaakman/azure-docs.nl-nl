---
title: 'Zelfstudie: Apache Storm gebruiken om te schrijven naar opslag/Data Lake Storage - Azure HDInsight'
description: 'Zelfstudie: Leer hoe u Apache Storm gebruiken om te schrijven naar de HDFS-compatibele opslag voor Azure HDInsight.'
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 06/24/2019
ms.openlocfilehash: 5c1376c7d1afe9c9702cfb43a146ac1cd17d6e58
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67428351"
---
# <a name="tutorial-write-to-apache-hadoop-hdfs-from-apache-storm-on-azure-hdinsight"></a>Zelfstudie: Schrijven naar Apache Hadoop HDFS van Apache Storm op Azure HDInsight

Deze zelfstudie wordt gedemonstreerd hoe u Apache Storm gebruiken om gegevens te schrijven naar de HDFS-compatibele opslag gebruikt met Apache Storm op HDInsight. HDInsight kan zowel Azure Storage en Azure Data Lake Storage als HDFS-compatibele opslag gebruiken. Storm biedt een [HdfsBolt](https://storm.apache.org/releases/current/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) onderdeel waarmee gegevens naar HDFS schrijft. Dit document bevat informatie over het schrijven naar beide typen opslag van de HdfsBolt.

De voorbeeldtopologie die in dit document, is afhankelijk van onderdelen die deel van Storm op HDInsight uitmaken. Het mogelijk moeten worden aangepast om te werken met Azure Data Lake-opslag gebruikt in combinatie met andere Apache Storm-clusters.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Het cluster configureren met scriptacties
> * Maken en de topologie
> * Implementeren en uitvoeren van de topologie
> * Gegevens van de uitvoer weergeven
> * De topologie stoppen

## <a name="prerequisites"></a>Vereisten

* [Java Developer Kit (JDK) versie 8](https://aka.ms/azure-jdks)

* [Apache Maven](https://maven.apache.org/download.cgi) goed [geïnstalleerd](https://maven.apache.org/install.html) op basis van Apache.  Maven is een project maken voor Java-projecten.

* Een SSH-client. Zie voor meer informatie [Verbinding maken met HDInsight (Apache Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

* De [URI-schema](../hdinsight-hadoop-linux-information.md#URI-and-scheme) voor uw clusters primaire opslag. Dit zou zijn `wasb://` voor Azure Storage, `abfs://` voor Azure Data Lake Storage Gen2 of `adl://` voor Azure Data Lake Storage Gen1. Als veilige overdracht is ingeschakeld voor Azure Storage of Data Lake Storage Gen2, de URI zou worden `wasbs://` of `abfss://`respectievelijk Zie ook [veilige overdracht](../../storage/common/storage-require-secure-transfer.md).

### <a name="example-configuration"></a>Voorbeeldconfiguratie

De volgende YAML is een fragment uit het `resources/writetohdfs.yaml` bestand opgenomen in het voorbeeld. Dit bestand definieert de Storm-topologie via de [lichtstroom](https://storm.apache.org/releases/1.1.2/flux.html) framework voor Apache Storm.

```yaml
components:
  - id: "syncPolicy"
    className: "org.apache.storm.hdfs.bolt.sync.CountSyncPolicy"
    constructorArgs:
      - 1000

  # Rotate files when they hit 5 MB
  - id: "rotationPolicy"
    className: "org.apache.storm.hdfs.bolt.rotation.FileSizeRotationPolicy"
    constructorArgs:
      - 5
      - "MB"

  - id: "fileNameFormat"
    className: "org.apache.storm.hdfs.bolt.format.DefaultFileNameFormat"
    configMethods:
      - name: "withPath"
        args: ["${hdfs.write.dir}"]
      - name: "withExtension"
        args: [".txt"]

  - id: "recordFormat"
    className: "org.apache.storm.hdfs.bolt.format.DelimitedRecordFormat"
    configMethods:
      - name: "withFieldDelimiter"
        args: ["|"]

# spout definitions
spouts:
  - id: "tick-spout"
    className: "com.microsoft.example.TickSpout"
    parallelism: 1


# bolt definitions
bolts:
  - id: "hdfs-bolt"
    className: "org.apache.storm.hdfs.bolt.HdfsBolt"
    configMethods:
      - name: "withConfigKey"
        args: ["hdfs.config"]
      - name: "withFsUrl"
        args: ["${hdfs.url}"]
      - name: "withFileNameFormat"
        args: [ref: "fileNameFormat"]
      - name: "withRecordFormat"
        args: [ref: "recordFormat"]
      - name: "withRotationPolicy"
        args: [ref: "rotationPolicy"]
      - name: "withSyncPolicy"
        args: [ref: "syncPolicy"]
```

Deze YAML definieert de volgende items:

* `syncPolicy`: Definieert wanneer bestanden worden gesynchroniseerd/leeggemaakt naar het bestandssysteem. In dit voorbeeld wordt elke 1000 tuples.
* `fileNameFormat`: Hiermee definieert u het pad en de bestandsnaam de naampatroon gebruiken bij het schrijven van bestanden. In dit voorbeeld wordt het pad is opgegeven tijdens runtime met behulp van een filter en de bestandsextensie is `.txt`.
* `recordFormat`: Hiermee definieert u de interne indeling van de bestanden die worden weggeschreven. In dit voorbeeld velden worden gescheiden door het `|` teken.
* `rotationPolicy`: Definieert wanneer draaien van bestanden. In dit voorbeeld wordt geen draaiing uitgevoerd.
* `hdfs-bolt`: Maakt gebruik van de vorige onderdelen als parameters voor de configuratie voor de `HdfsBolt` klasse.

Zie voor meer informatie over het framework lichtstroom [ https://storm.apache.org/releases/current/flux.html ](https://storm.apache.org/releases/current/flux.html).

## <a name="configure-the-cluster"></a>Het cluster configureren

Standaard Storm op HDInsight omvat niet de onderdelen die `HdfsBolt` gebruikt om te communiceren met Azure Storage of Data Lake-opslag in het klassepad van Storm. De volgende scriptactie gebruiken om toe te voegen deze onderdelen de `extlib` Active directory voor Storm op het cluster:

| Eigenschap | Value |
|---|---|
|Scripttype |-Aangepaste|
|Bash-script-URI |`https://hdiconfigactions.blob.core.windows.net/linuxstormextlibv01/stormextlib.sh`|
|Knooppunttype(n) |Nimbus, Supervisor|
|Parameters |Geen|

Zie voor meer informatie over het gebruik van dit script met uw cluster de [aanpassen HDInsight-clusters met scriptacties](./../hdinsight-hadoop-customize-cluster-linux.md) document.

## <a name="build-and-package-the-topology"></a>Maken en de topologie

1. Download het voorbeeldproject van [ https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) naar uw ontwikkelomgeving.

2. Vanuit een opdrachtprompt, terminal of shell-sessie mappen wijzigen naar de hoofdmap van het gedownloade project. Als u wilt maken en de topologie, gebruik de volgende opdracht:

    ```cmd
    mvn compile package
    ```

    Nadat de build en -verpakking is voltooid, er is een nieuwe map met de naam `target`, die een bestand met de naam bevat `StormToHdfs-1.0-SNAPSHOT.jar`. Dit bestand bevat de gecompileerde topologie.

## <a name="deploy-and-run-the-topology"></a>Implementeren en uitvoeren van de topologie

1. Gebruik de volgende opdracht om te kopiëren van de topologie naar het HDInsight-cluster. Vervang `CLUSTERNAME` met de naam van het cluster.

    ```cmd
    scp target\StormToHdfs-1.0-SNAPSHOT.jar sshuser@CLUSTERNAME-ssh.azurehdinsight.net:StormToHdfs-1.0-SNAPSHOT.jar
    ```

1. Nadat het uploaden is voltooid, gebruikt u de volgende verbinding maken met het HDInsight-cluster via SSH. Vervang `CLUSTERNAME` met de naam van het cluster.

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Eenmaal verbinding hebben, gebruikt u de volgende opdracht uit om te maken van een bestand met de naam `dev.properties`:

    ```bash
    nano dev.properties
    ```

1. Gebruik de volgende tekst als de inhoud van de `dev.properties` bestand. Herzien zo nodig op basis van uw [URI-schema](../hdinsight-hadoop-linux-information.md#URI-and-scheme).

    ```
    hdfs.write.dir: /stormdata/
    hdfs.url: wasbs:///
    ```

    Gebruiken om het bestand hebt opgeslagen, __Ctrl + X__, klikt u vervolgens __Y__, en tot slot __Enter__. De waarden in dit bestand instellen de URL van de opslag en de naam van de map die gegevens worden geschreven naar.

1. Gebruik de volgende opdracht uit om te starten van de topologie:

    ```bash
    storm jar StormToHdfs-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writetohdfs.yaml --filter dev.properties
    ```

    Met deze opdracht start de topologie met behulp van het framework lichtstroom door in te dienen het naar het Nimbus-knooppunt van het cluster. De topologie wordt gedefinieerd door de `writetohdfs.yaml` -bestand in de jar. De `dev.properties` bestand wordt doorgegeven als een filter en de waarden die zijn opgenomen in het bestand worden gelezen door de topologie.

## <a name="view-output-data"></a>Gegevens van de uitvoer weergeven

Als u wilt de gegevens bekijkt, gebruik de volgende opdracht:

  ```bash
  hdfs dfs -ls /stormdata/
  ```

Een lijst van de bestanden die zijn gemaakt door deze topologie wordt weergegeven. Hieronder volgt een voorbeeld van de gegevens die zijn geretourneerd door de voorgaande opdrachten:

```output
Found 23 items
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-0-1561407909895.txt
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-1-1561407915577.txt
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-10-1561407943327.txt
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-11-1561407946312.txt
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-12-1561407949320.txt
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-13-1561407952662.txt
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-14-1561407955502.txt
```

## <a name="stop-the-topology"></a>De topologie stoppen

Storm-topologieën uitgevoerd totdat deze wordt gestopt of het cluster wordt verwijderd. Als u wilt de topologie stoppen, gebruikt u de volgende opdracht uit:

```bash
storm kill hdfswriter
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u de in deze zelfstudie gemaakte resources wilt opschonen, kunt u de resourcegroep verwijderen. Als u de resourcegroep verwijdert, worden ook het bijbehorende HDInsight-cluster en eventuele andere resources die aan de resourcegroep zijn gekoppeld, verwijderd.

Ga als volgt te werk om de resourcegroep te verwijderen in Azure Portal:

1. Vouw het menu aan de linkerkant in Azure Portal uit om het menu met services te openen en kies __Resourcegroepen__ om de lijst met resourcegroepen weer te geven.
2. Zoek de resourcegroep die u wilt verwijderen en klik met de rechtermuisknop op de knop __Meer__ (... ) aan de rechterkant van de vermelding.
3. Selecteer __Resourcegroep verwijderen__ en bevestig dit.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u Apache Storm gebruiken om gegevens te schrijven naar de HDFS-compatibele opslag gebruikt met Apache Storm op HDInsight.

> [!div class="nextstepaction"]
> Ontdek andere [voorbeelden voor Apache Storm voor HDInsight](apache-storm-example-topology.md)