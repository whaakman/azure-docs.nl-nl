---
title: Apache Storm schrijven naar opslag/Data Lake Store - Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van de Apache Storm om te schrijven naar de HDFS-compatibele opslag voor HDInsight. Azure Storage of Azure Data Lake Store bieden de HDFS-comptabile opslag voor HDInsight. Dit document en de bijbehorende voorbeeld ziet u hoe het onderdeel HdfsBolt kan worden gebruikt om te schrijven naar de opslag standaard van een Storm op HDInsight-cluster.
services: hdinsight
documentationcenter: na
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 1df98653-a6c8-4662-a8c6-5d288fc4f3a6
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/04/2018
ms.author: larryfr
ms.openlocfilehash: 5550dc2ffc53c6ccd30ecb4901ec98c4d38e366b
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="write-to-hdfs-from-apache-storm-on-hdinsight"></a>Schrijven naar HDFS van Apache Storm op HDInsight

Informatie over het gebruik van Storm om gegevens te schrijven naar de HDFS-compatibele opslag die wordt gebruikt door Apache Storm op HDInsight. HDInsight kunt gebruiken beide Azure Storage en Azure Data Lake opslaan als HDFS comptabile opslag. Storm biedt een [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) onderdeel dat gegevens naar HDFS schrijft. Dit document bevat informatie over het schrijven naar beide typen opslag van de HdfsBolt. 

> [!IMPORTANT]
> De voorbeeldtopologie die in dit document is gebaseerd op de onderdelen die deel van Storm op HDInsight uitmaken. Deze mogelijk moeten worden aangepast voor gebruik met Azure Data Lake Store gebruikt in combinatie met andere Apache Storm-clusters.

## <a name="get-the-code"></a>Code ophalen

Het project met deze topologie wordt beschikbaar als download van [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store).

Als u wilt compileren dit project, moet u de volgende configuratie voor uw ontwikkelomgeving:

* [Java JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) of hoger. HDInsight 3.5 of hoger vereisen Java 8.

* [Maven 3.x](https://maven.apache.org/download.cgi)

De volgende omgevingsvariabelen kunnen worden ingesteld wanneer u Java en de JDK op uw ontwikkelwerkstation installeert. Echter, moet u controleren of ze bestaan en dat ze de juiste waarden voor uw systeem bevatten.

* `JAVA_HOME`-moet verwijzen naar de map waar de JDK die is geïnstalleerd.
* `PATH`-moet bevatten de volgende paden:
  
    * `JAVA_HOME`(of equivalente paden).
    * `JAVA_HOME\bin`(of equivalente paden).
    * De map waar Maven is geïnstalleerd.

## <a name="how-to-use-the-hdfsbolt-with-hdinsight"></a>De HdfsBolt gebruiken met HDInsight

> [!IMPORTANT]
> Voordat u de HdfsBolt met Storm op HDInsight, moet u eerst een scriptactie gebruiken voor het kopiëren van de vereiste jar-bestanden naar de `extpath` voor Storm. Zie voor meer informatie de [het cluster configureren](#configure) sectie.

De HdfsBolt gebruikt het bestand-schema dat u opgeeft om te begrijpen hoe om te schrijven naar HDFS. Met HDInsight, een van de volgende's te gebruiken:

* `wasb://`: Met een Azure Storage-account gebruikt.
* `adl://`: Gebruikt met Azure Data Lake Store.

De volgende tabel bevat voorbeelden van het gebruik van het bestand-schema voor verschillende scenario's:

| Schema | Opmerkingen |
| ----- | ----- |
| `wasb:///` | Het standaardopslagaccount is een blobcontainer in Azure Storage-account |
| `adl:///` | Het standaardopslagaccount is een map in Azure Data Lake Store. Tijdens het maken geeft u de map in Data Lake Store is de hoofdmap van het cluster HDFS. Bijvoorbeeld, de `/clusters/myclustername/` directory. |
| `wasb://CONTAINER@ACCOUNT.blob.core.windows.net/` | Een niet-standaard (Extra) Azure-opslagaccount die is gekoppeld aan het cluster. |
| `adl://STORENAME/` | De hoofdmap van de Data Lake Store-gebruikt door het cluster. Dit schema hebt u toegang tot gegevens die zich buiten de map waarin het bestandssysteem van het cluster zich bevindt. |

Zie voor meer informatie de [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) verwijzing op Apache.org.

### <a name="example-configuration"></a>Voorbeeldconfiguratie

De volgende YAML is afkomstig uit de `resources/writetohdfs.yaml` bestand opgenomen in het voorbeeld. Dit bestand definieert het Storm-topologie via de [lichtstroom](https://storm.apache.org/releases/1.1.0/flux.html) framework voor Apache Storm.

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

* `syncPolicy`: Hiermee wordt gedefinieerd wanneer bestanden worden gesynchroniseerd/leeggemaakt op het bestandssysteem. In dit voorbeeld elke 1000 tuples.
* `fileNameFormat`: Het pad en de bestandsnaam naampatroon moet worden gebruikt bij het schrijven van bestanden definieert. In dit voorbeeld wordt het pad wordt geleverd tijdens runtime met een filter, en de bestandsextensie `.txt`.
* `recordFormat`: Hiermee definieert u de interne indeling van de bestanden die zijn geschreven. In dit voorbeeld velden worden gescheiden door het `|` teken.
* `rotationPolicy`: Hiermee wordt gedefinieerd of draaien van bestanden. In dit voorbeeld wordt geen draaiing uitgevoerd.
* `hdfs-bolt`: Maakt gebruik van de vorige onderdelen als configuratieparameters voor de `HdfsBolt` klasse.

Zie voor meer informatie over het framework lichtstroom [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).

## <a name="configure-the-cluster"></a>Het cluster configureren

Storm op HDInsight omvat standaard niet de onderdelen die HdfsBolt gebruikt om te communiceren met Azure Storage of de Data Lake Store in klassenpad van Storm. Gebruik de volgende scriptactie toevoegen van deze onderdelen de `extlib` map voor Storm op het cluster:

* Script-URI:`https://hdiconfigactions.blob.core.windows.net/linuxstormextlibv01/stormextlib.sh`
* Knooppunten moeten worden toegepast op: Nimbus, Supervisor
* Parameters: geen

Zie voor meer informatie over het gebruik van dit script met uw cluster de [aanpassen HDInsight-clusters met behulp van scriptacties](./../hdinsight-hadoop-customize-cluster-linux.md) document.

## <a name="build-and-package-the-topology"></a>Bouwen en de topologie van het pakket

1. Download het voorbeeldproject van [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) op uw ontwikkelomgeving.

2. Vanuit een opdrachtprompt, terminal of shell-sessie, mappen wijzigen naar de hoofdmap van het gedownloade project. Voor het bouwen en de topologie van het pakket, gebruikt u de volgende opdracht:
   
        mvn compile package
   
    Zodra de build en verpakking is voltooid, er is een nieuwe map met de naam `target`, die een bestand met de naam bevat `StormToHdfs-1.0-SNAPSHOT.jar`. Dit bestand bevat de gecompileerde topologie.

## <a name="deploy-and-run-the-topology"></a>Implementeren en uitvoeren van de topologie

1. Gebruik de volgende opdracht in de topologie kopiëren naar het HDInsight-cluster. Vervang **gebruiker** met de SSH-gebruikersnaam die u hebt gebruikt bij het maken van het cluster. Vervang **CLUSTERNAME** door de naam van uw cluster.
   
        scp target\StormToHdfs-1.0-SNAPSHOT.jar USER@CLUSTERNAME-ssh.azurehdinsight.net:StormToHdfs1.0-SNAPSHOT.jar
   
    Voer desgevraagd het wachtwoord dat wordt gebruikt bij het maken van de SSH-gebruiker voor het cluster. Als u een openbare sleutel in plaats van een wachtwoord gebruikt, moet u mogelijk gebruik van de `-i` -parameter voor het pad naar de overeenkomende persoonlijke sleutel opgeven.
   
   > [!NOTE]
   > Voor meer informatie over het gebruik van `scp` met HDInsight, raadpleegt u [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Nadat het uploaden is voltooid, gebruikt u de volgende verbinding maken met het HDInsight-cluster via SSH. Vervang **gebruiker** met de SSH-gebruikersnaam die u hebt gebruikt bij het maken van het cluster. Vervang **CLUSTERNAME** door de naam van uw cluster.
   
        ssh USER@CLUSTERNAME-ssh.azurehdinsight.net
   
    Voer desgevraagd het wachtwoord dat wordt gebruikt bij het maken van de SSH-gebruiker voor het cluster. Als u een openbare sleutel in plaats van een wachtwoord gebruikt, moet u mogelijk gebruik van de `-i` -parameter voor het pad naar de overeenkomende persoonlijke sleutel opgeven.
   
   Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

3. De volgende opdracht eenmaal zijn verbonden, gebruiken voor het maken van een bestand met de naam `dev.properties`:

        nano dev.properties

4. Gebruik de volgende tekst als de inhoud van de `dev.properties` bestand:

        hdfs.write.dir: /stormdata/
        hdfs.url: wasb:///

    > [!IMPORTANT]
    > In dit voorbeeld wordt ervan uitgegaan dat uw cluster maakt gebruik van een Azure Storage-account als de standaard-opslag. Als uw cluster gebruikmaakt van Azure Data Lake Store, gebruikt u `hdfs.url: adl:///` in plaats daarvan.
    
    Gebruiken om het bestand opslaat, __Ctrl + X__, klikt u vervolgens __Y__, en tot slot __Enter__. De waarden in dit bestand ingesteld voor de Data Lake store-URL en de naam van de map die gegevens naar worden geschreven.

3. Gebruik de volgende opdracht om te starten van de topologie:
   
        storm jar StormToHdfs-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writetohdfs.yaml --filter dev.properties

    Deze opdracht start de topologie met behulp van het framework lichtstroom door ingediend bij de Nimbus-knooppunt van het cluster. De topologie wordt gedefinieerd door de `writetohdfs.yaml` opgenomen in de Directory met jar bestand. De `dev.properties` bestand wordt doorgegeven als een filter en de waarden in het bestand worden gelezen door de topologie.

## <a name="view-output-data"></a>Gegevens van de uitvoer weergeven

Als u wilt de gegevens bekijkt, gebruik de volgende opdracht:

    hdfs dfs -ls /stormdata/

Een lijst van de bestanden die zijn gemaakt door deze topologie wordt weergegeven.

Hieronder volgt een voorbeeld van de gegevens die door de eerdere opdrachten retuned:

    Found 30 items
    -rw-r-----+  1 sshuser sshuser       488000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-0-1488568403092.txt
    -rw-r-----+  1 sshuser sshuser       444000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-1-1488568404567.txt
    -rw-r-----+  1 sshuser sshuser       502000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-10-1488568408678.txt
    -rw-r-----+  1 sshuser sshuser       582000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-11-1488568411636.txt
    -rw-r-----+  1 sshuser sshuser       464000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-12-1488568411884.txt

## <a name="stop-the-topology"></a>De topologie stoppen

Storm-topologieën uitgevoerd totdat deze wordt gestopt of het cluster wordt verwijderd. Als u wilt de topologie stoppen, moet u de volgende opdracht gebruiken:

    storm kill hdfswriter

## <a name="delete-your-cluster"></a>Uw cluster verwijderen

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u Storm gebruiken om te schrijven naar Azure Storage en Azure Data Lake Store, detecteren andere [Storm-voorbeelden voor HDInsight](apache-storm-example-topology.md).

