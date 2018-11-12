---
title: Met Storm oplossen met behulp van Azure HDInsight
description: Vind antwoorden op veelgestelde vragen over het gebruik van Apache Storm met Azure HDInsight.
keywords: HDInsight, Storm, veelgestelde vragen over Azure, problemen oplossen handleiding worden veelvoorkomende problemen
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 11/2/2017
ms.openlocfilehash: 99ceeea33d3e2d9af798d5eb4161b0c16afc952d
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51011371"
---
# <a name="troubleshoot-storm-by-using-azure-hdinsight"></a>Met Storm oplossen met behulp van Azure HDInsight

Meer informatie over de meest voorkomende problemen en hun oplossingen voor het werken met Apache Storm-nettoladingen in Apache Ambari.

## <a name="how-do-i-access-the-storm-ui-on-a-cluster"></a>Hoe krijg ik toegang tot de Storm-gebruikersinterface op een cluster?
U hebt twee opties om de Storm-gebruikersinterface vanuit een browser te openen:

### <a name="ambari-ui"></a>Ambari-gebruikersinterface
1. Ga naar de Ambari-dashboard.
2. Selecteer in de lijst met services, **Storm**.
3. In de **snelkoppelingen** in het menu **Storm-gebruikersinterface**.

### <a name="direct-link"></a>Directe koppeling
U kunt toegang tot de Storm-gebruikersinterface op de volgende URL:

https://\<cluster DNS-naam\>/stormui

Voorbeeld:

 https://stormcluster.azurehdinsight.net/stormui

## <a name="how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-to-another"></a>Hoe ik Storm event hub spout controlepunt gegevens van één topologie overbrengen naar een andere?

Bij het ontwikkelen van topologieën die uit Azure Event Hubs gelezen met behulp van de HDInsight Storm event hub spout JAR-bestand, moet u een topologie met dezelfde naam op een nieuw cluster implementeren. U moet echter de gegevens van de controlepunten die is toegewezen aan Apache ZooKeeper op het oude cluster behouden.

### <a name="where-checkpoint-data-is-stored"></a>Waar controlepuntgegevens worden opgeslagen
Controlepuntgegevens voor verschuiving is opgeslagen door de event hub spout in ZooKeeper in twee paden van de hoofdmap:
- Niet-transactionele spout controlepunten worden opgeslagen in /eventhubspout.
- Transactionele spout controlepuntgegevens worden opgeslagen in / transactionele.

### <a name="how-to-restore"></a>Herstellen
Als u de scripts en -bibliotheken die u gebruikt om te exporteren van gegevens buiten ZooKeeper en importeer de gegevens terug naar ZooKeeper met een nieuwe naam, Zie [HDInsight Storm-voorbeelden](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).

De bibliotheekmap heeft JAR-bestanden met de implementatie voor de bewerking voor exporteren/importeren. De bash-map bevat een voorbeeldscript dat laat zien hoe u gegevens uit de ZooKeeper-server op het oude cluster exporteren en vervolgens importeren naar de ZooKeeper-server op het nieuwe cluster.

Voer de [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) script uit de ZooKeeper-knooppunten om te exporteren en vervolgens gegevens te importeren. Het script bijwerken naar de juiste versie van Hortonworks Data Platform (HDP). (We werken op het maken van deze scripts algemene in HDInsight. Algemene scripts kunnen uitvoeren op elk knooppunt in het cluster zonder wijzigingen door de gebruiker.)

De exportopdracht de metagegevens worden geschreven naar een Apache Hadoop Distributed File System (HDFS)-pad (in een Azure Blob Storage of Azure Data Lake Store-store) op een locatie die u hebt ingesteld.

### <a name="examples"></a>Voorbeelden

#### <a name="export-offset-metadata"></a>Offset metagegevens exporteren
1. SSH gebruiken om te gaan naar de ZooKeeper-cluster op het cluster waaruit het controlepunt offset moet worden geëxporteerd.
2. Voer de volgende opdracht (na het bijwerken van de tekenreeks van de versie van de HDP) ZooKeeper offset om gegevens te exporteren naar het /stormmetadta/zkdata HDFS-pad:

    ```apache   
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
    ```

#### <a name="import-offset-metadata"></a>Offset metagegevens importeren
1. SSH gebruiken om te gaan naar de ZooKeeper-cluster op het cluster waaruit het controlepunt offset moet worden geïmporteerd.
2. Voer de volgende opdracht (na het bijwerken van de tekenreeks van de versie van de HDP) ZooKeeper offset om gegevens te importeren uit de /stormmetadata/zkdata HDFS-pad naar de ZooKeeper-server op de doelcluster:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
    ```
   
#### <a name="delete-offset-metadata-so-that-topologies-can-start-processing-data-from-the-beginning-or-from-a-timestamp-that-the-user-chooses"></a>Offset metagegevens verwijderen zodat topologieën kunnen verwerken van gegevens vanaf het begin of in een timestamp die de gebruiker ervoor kiest starten
1. SSH gebruiken om te gaan naar de ZooKeeper-cluster op het cluster waaruit het controlepunt offset moet worden verwijderd.
2. Voer de volgende opdracht (na het bijwerken van de tekenreeks van de versie van de HDP) om alle offset ZooKeeper-gegevens in het huidige cluster te verwijderen:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
    ```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a>Hoe vind ik binaire Storm-bestanden op een cluster?
Binaire storm-bestanden voor de huidige HDP-stack zijn in /usr/hdp/current/storm-client. De locatie is hetzelfde voor hoofdknooppunten en voor worker-knooppunten.
 
Er zijn meerdere binaire bestanden voor specifieke HDP-versies in /usr/hdp (bijvoorbeeld /usr/hdp/2.5.0.1233/storm). De map /usr/hdp/current/storm-client is symlinked naar de nieuwste versie die wordt uitgevoerd op het cluster.

Zie voor meer informatie, [verbinding maken met een HDInsight-cluster met behulp van SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) en [Storm](http://storm.apache.org/).
 
## <a name="how-do-i-determine-the-deployment-topology-of-a-storm-cluster"></a>Hoe bepaal ik de topologie van de implementatie van een Storm-cluster?
Bepaal eerst alle onderdelen die zijn geïnstalleerd met HDInsight Storm. Een Storm-cluster bestaat uit vier categorieën van knooppunt:

* Gateway-knooppunten
* Hoofdknooppunten
* ZooKeeper-knooppunten
* Worker-knooppunten
 
### <a name="gateway-nodes"></a>Gateway-knooppunten
Een gateway-knooppunt is een gateway en de reverse proxyservice waarmee u openbare toegang tot een actieve Ambari management-service. Ook verwerkt deze Ambari selectie van leider.
 
### <a name="head-nodes"></a>Hoofdknooppunten
Storm-hoofdknooppunten, voer de volgende services:
* Nimbus
* Ambari-server
* Metrische gegevens voor Ambari-server
* Ambari metrische gegevens verzamelen
 
### <a name="zookeeper-nodes"></a>ZooKeeper-knooppunten
HDInsight wordt geleverd met een ZooKeeper-quorum drie knooppunten. De grootte van het quorum is vast en kan niet opnieuw worden geconfigureerd.
 
Storm-services in het cluster zijn geconfigureerd voor het gebruik van automatisch de ZooKeeper-quorum.
 
### <a name="worker-nodes"></a>Worker-knooppunten
Storm worker-knooppunten uitvoeren van de volgende services:
* Supervisor
* Werknemer Java virtual machines (JVMs), voor het actieve topologieën
* Ambari-agent
 
## <a name="how-do-i-locate-storm-event-hub-spout-binaries-for-development"></a>Hoe vind ik de binaire Storm event hub spout-bestanden voor ontwikkeling?
 
Zie de volgende bronnen voor meer informatie over het gebruik van Storm event hub spout JAR-bestanden met uw topologie.
 
### <a name="java-based-topology"></a>Op basis van een Java-topologie
[Process events from Azure Event Hubs with Storm on HDInsight (Java)](https://docs.microsoft.com/azure/hdinsight/hdinsight-storm-develop-java-event-hub-topology) (Gebeurtenissen uit Azure Event Hubs verwerken met Storm op HDInsight (Java))
 
### <a name="c-based-topology-mono-on-hdinsight-34-linux-storm-clusters"></a>C#-topologie (Mono op Storm voor HDInsight 3.4 + Linux-clusters) op basis van
[Process events from Azure Event Hubs with Storm on HDInsight (C#)](https://docs.microsoft.com/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology) (Gebeurtenissen uit Azure Event Hubs verwerken met Storm op HDInsight (C#))
 
### <a name="latest-storm-event-hub-spout-binaries-for-hdinsight-35-linux-storm-clusters"></a>Meest recente Storm event hub spout binaire bestanden voor HDInsight 3.5 + Linux Storm-clusters
Zie voor meer informatie over het gebruik van de meest recente Storm event hub spout dat met HDInsight 3.5 + Linux Storm-clusters werkt, de mvn-opslagplaats [Leesmij-bestand](https://github.com/hdinsight/mvn-repo/blob/master/README.md).
 
### <a name="source-code-examples"></a>Bron-codevoorbeelden
Zie [voorbeelden](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) van lezen en schrijven uit Azure Event Hub met behulp van een Apache Storm-topologie (geschreven in Java) op een Azure HDInsight-cluster.
 
## <a name="how-do-i-locate-storm-log4j-configuration-files-on-clusters"></a>Hoe vind ik de Storm Log4J-configuratiebestanden op clusters?
 
Omgaan met Apache Log4J-configuratiebestanden voor Storm-services.
 
### <a name="on-head-nodes"></a>Op de hoofdknooppunten
Het Nimbus Log4J-configuratie wordt gelezen vanuit/usr/hdp/\<HDP versie\>/storm/log4j2/cluster.xml.
 
### <a name="on-worker-nodes"></a>Op de worker-knooppunten
De supervisor Log4J-configuratie worden gelezen uit de/USR/hdp/\<HDP versie\>/storm/log4j2/cluster.xml.
 
Het bestand van de werknemer Log4J-configuratie worden gelezen uit de/USR/hdp/\<HDP versie\>/storm/log4j2/worker.xml.
 
Voorbeelden: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml

### <a name="see-also"></a>Zie ook
[Problemen oplossen met behulp van Azure HDInsight](../../hdinsight/hdinsight-troubleshoot-guide.md)
