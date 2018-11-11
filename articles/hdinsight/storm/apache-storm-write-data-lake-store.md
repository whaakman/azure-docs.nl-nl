---
title: Apache Storm schrijven naar opslag/de Data Lake Store - Azure HDInsight
description: Leer hoe u de Apache Storm gebruiken om te schrijven naar de HDFS-compatibele opslag voor HDInsight.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.openlocfilehash: e2dd9b1d6031d8c8695ff6821e9af4e464ef270a
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51009348"
---
# <a name="write-to-hdfs-from-apache-storm-on-hdinsight"></a>Schrijven naar HDFS van Apache Storm op HDInsight

Informatie over het gebruik van Storm om gegevens te schrijven naar de HDFS-compatibele opslag gebruikt met Apache Storm op HDInsight. HDInsight kunt beide gebruiken Azure Storage en Azure Data Lake opslaan als HDFS-compatibele opslag. Storm biedt een [HdfsBolt](http://storm.apache.org/releases/current/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) onderdeel waarmee gegevens naar HDFS schrijft. Dit document bevat informatie over het schrijven naar beide typen opslag van de HdfsBolt. 

> [!IMPORTANT]
> De voorbeeldtopologie die in dit document, is afhankelijk van onderdelen die deel van Storm op HDInsight uitmaken. Het mogelijk moeten worden aangepast om te werken met Azure Data Lake Store gebruikt in combinatie met andere Apache Storm-clusters.

## <a name="get-the-code"></a>Code ophalen

Het project met deze topologie is beschikbaar als een download van [ https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store).

Als u wilt dit project compileren, moet u de volgende configuratie voor uw ontwikkelomgeving:

* [Java JDK 1.8](https://aka.ms/azure-jdks) of hoger. HDInsight 3.5 of hoger vereist Java 8.

* [Maven 3.x](https://maven.apache.org/download.cgi)

U kunt de volgende omgevingsvariabelen instellen wanneer u Java en de JDK installeert op uw ontwikkelwerkstation. U moet dan echter wel controleren of ze bestaan en of ze de juiste waarden voor uw systeem bevatten.

* `JAVA_HOME` -moet verwijzen naar de map waarin de JDK is geïnstalleerd.
* `PATH` -moet de volgende paden bevatten:
  
    * `JAVA_HOME` (of het equivalente pad).
    * `JAVA_HOME\bin` (of het equivalente pad).
    * De map waarin Maven is geïnstalleerd.

## <a name="how-to-use-the-hdfsbolt-with-hdinsight"></a>De HdfsBolt gebruiken met HDInsight

> [!IMPORTANT]
> Voordat u de HdfsBolt met Storm op HDInsight, moet u eerst een scriptactie gebruiken om te kopiëren van de vereiste jar-bestanden in de `extpath` voor Storm. Zie voor meer informatie de [configureren van het cluster](#configure) sectie.

De HdfsBolt maakt gebruik van het bestand-schema dat u opgeeft als u wilt weten over het schrijven van HDFS. Met HDInsight, een van de volgende schema's te gebruiken:

* `wasb://`: Wordt gebruikt met een Azure Storage-account.
* `adl://`: Wordt gebruikt met Azure Data Lake Store.

De volgende tabel bevat voorbeelden van het gebruik van het bestand-schema voor verschillende scenario's:

| Schema | Opmerkingen |
| ----- | ----- |
| `wasb:///` | Het standaardaccount voor opslag is een blob-container in een Azure Storage-account |
| `adl:///` | Het standaardopslagaccount is een map in Azure Data Lake Store. Tijdens het maken geeft u de map in Data Lake Store is de hoofdmap van het cluster HDFS. Bijvoorbeeld, de `/clusters/myclustername/` directory. |
| `wasb://CONTAINER@ACCOUNT.blob.core.windows.net/` | Een niet-standaard (Extra) Azure storage-account dat is gekoppeld aan het cluster. |
| `adl://STORENAME/` | De hoofdmap van de Data Lake Store gebruikt door het cluster. Dit schema kunt u toegang tot gegevens die zich buiten de map waarin het bestandssysteem van het cluster zich bevindt. |

Zie voor meer informatie de [HdfsBolt](http://storm.apache.org/releases/current/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) verwijzing op Apache.org.

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

* `syncPolicy`: Hiermee definieert wanneer bestanden worden gesynchroniseerd/leeggemaakt naar het bestandssysteem. In dit voorbeeld wordt elke 1000 tuples.
* `fileNameFormat`: Hiermee definieert u het pad en de bestandsnaam de naampatroon gebruiken bij het schrijven van bestanden. In dit voorbeeld wordt het pad is opgegeven tijdens runtime met behulp van een filter en de bestandsextensie is `.txt`.
* `recordFormat`: Hiermee definieert u de interne indeling van de bestanden die worden weggeschreven. In dit voorbeeld velden worden gescheiden door het `|` teken.
* `rotationPolicy`: Hiermee definieert u bij het draaien van bestanden. In dit voorbeeld wordt geen draaiing uitgevoerd.
* `hdfs-bolt`: De eerdere onderdelen gebruikt als parameters voor de configuratie voor de `HdfsBolt` klasse.

Zie voor meer informatie over het framework lichtstroom [ https://storm.apache.org/releases/1.1.2/flux.html ](https://storm.apache.org/releases/1.1.2/flux.html).

## <a name="configure-the-cluster"></a>Het cluster configureren

Storm op HDInsight omvat standaard niet de onderdelen die HdfsBolt gebruikt om te communiceren met Azure Storage of Data Lake Store in het klassepad van Storm. De volgende scriptactie gebruiken om toe te voegen deze onderdelen de `extlib` Active directory voor Storm op het cluster:

* Script-URI: `https://hdiconfigactions.blob.core.windows.net/linuxstormextlibv01/stormextlib.sh`
* Knooppunten toe te passen op: Nimbus, Supervisor
* Parameters: geen

Zie voor meer informatie over het gebruik van dit script met uw cluster de [aanpassen HDInsight-clusters met scriptacties](./../hdinsight-hadoop-customize-cluster-linux.md) document.

## <a name="build-and-package-the-topology"></a>Maken en de topologie

1. Download het voorbeeldproject van [ https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) naar uw ontwikkelomgeving.

2. Vanuit een opdrachtprompt, terminal of shell-sessie mappen wijzigen naar de hoofdmap van het gedownloade project. Als u wilt maken en de topologie, gebruik de volgende opdracht:
   
        mvn compile package
   
    Nadat de build en -verpakking is voltooid, er is een nieuwe map met de naam `target`, die een bestand met de naam bevat `StormToHdfs-1.0-SNAPSHOT.jar`. Dit bestand bevat de gecompileerde topologie.

## <a name="deploy-and-run-the-topology"></a>Implementeren en uitvoeren van de topologie

1. Gebruik de volgende opdracht om te kopiëren van de topologie naar het HDInsight-cluster. Vervang **gebruiker** met de SSH-gebruikersnaam die u hebt gebruikt bij het maken van het cluster. Vervang **CLUSTERNAME** door de naam van uw cluster.
   
        scp target\StormToHdfs-1.0-SNAPSHOT.jar USER@CLUSTERNAME-ssh.azurehdinsight.net:StormToHdfs-1.0-SNAPSHOT.jar
   
    Wanneer u wordt gevraagd, typt u het wachtwoord dat wordt gebruikt bij het maken van de SSH-gebruiker voor het cluster. Als u een openbare sleutel in plaats van een wachtwoord hebt gebruikt, moet u mogelijk gebruik van de `-i` parameter opgeven voor het pad naar de overeenkomende persoonlijke sleutel.
   
   > [!NOTE]
   > Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie over het gebruik van `scp` met HDInsight.

2. Nadat het uploaden is voltooid, gebruikt u de volgende verbinding maken met het HDInsight-cluster via SSH. Vervang **gebruiker** met de SSH-gebruikersnaam die u hebt gebruikt bij het maken van het cluster. Vervang **CLUSTERNAME** door de naam van uw cluster.
   
        ssh USER@CLUSTERNAME-ssh.azurehdinsight.net
   
    Wanneer u wordt gevraagd, typt u het wachtwoord dat wordt gebruikt bij het maken van de SSH-gebruiker voor het cluster. Als u een openbare sleutel in plaats van een wachtwoord hebt gebruikt, moet u mogelijk gebruik van de `-i` parameter opgeven voor het pad naar de overeenkomende persoonlijke sleutel.
   
   Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

3. Eenmaal verbinding hebben, gebruikt u de volgende opdracht uit om te maken van een bestand met de naam `dev.properties`:

        nano dev.properties

4. Gebruik de volgende tekst als de inhoud van de `dev.properties` bestand:

        hdfs.write.dir: /stormdata/
        hdfs.url: wasb:///

    > [!IMPORTANT]
    > In dit voorbeeld wordt ervan uitgegaan dat uw cluster maakt gebruik van een Azure Storage-account als de standaardopslag. Als uw cluster maakt gebruik van Azure Data Lake Store, gebruikt u `hdfs.url: adl:///` in plaats daarvan.
    
    Gebruiken om het bestand hebt opgeslagen, __Ctrl + X__, klikt u vervolgens __Y__, en tot slot __Enter__. De waarden in dit bestand instellen de URL van de Data Lake store en de naam van de map die gegevens worden geschreven naar.

3. Gebruik de volgende opdracht uit om te starten van de topologie:
   
        storm jar StormToHdfs-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writetohdfs.yaml --filter dev.properties

    Met deze opdracht start de topologie met behulp van het framework lichtstroom door in te dienen het naar het Nimbus-knooppunt van het cluster. De topologie wordt gedefinieerd door de `writetohdfs.yaml` -bestand in de jar. De `dev.properties` bestand wordt doorgegeven als een filter en de waarden die zijn opgenomen in het bestand worden gelezen door de topologie.

## <a name="view-output-data"></a>Gegevens van de uitvoer weergeven

Als u wilt de gegevens bekijkt, gebruik de volgende opdracht:

    hdfs dfs -ls /stormdata/

Een lijst van de bestanden die zijn gemaakt door deze topologie wordt weergegeven.

Hieronder volgt een voorbeeld van de gegevens die door de voorgaande opdrachten retuned:

    Found 30 items
    -rw-r-----+  1 sshuser sshuser       488000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-0-1488568403092.txt
    -rw-r-----+  1 sshuser sshuser       444000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-1-1488568404567.txt
    -rw-r-----+  1 sshuser sshuser       502000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-10-1488568408678.txt
    -rw-r-----+  1 sshuser sshuser       582000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-11-1488568411636.txt
    -rw-r-----+  1 sshuser sshuser       464000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-12-1488568411884.txt

## <a name="stop-the-topology"></a>De topologie stoppen

Storm-topologieën uitgevoerd totdat deze wordt gestopt of het cluster wordt verwijderd. Als u wilt de topologie stoppen, gebruikt u de volgende opdracht uit:

    storm kill hdfswriter

## <a name="delete-your-cluster"></a>Verwijder uw cluster

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u kunt Storm gebruiken om te schrijven naar Azure Storage en Azure Data Lake Store, Ontdek andere [Storm-voorbeelden voor HDInsight](apache-storm-example-topology.md).

