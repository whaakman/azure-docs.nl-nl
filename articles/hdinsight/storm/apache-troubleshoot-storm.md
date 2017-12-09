---
title: Storm oplossen met behulp van Azure HDInsight | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over het gebruik van Apache Storm met Azure HDInsight.
keywords: HDInsight, Storm, veelgestelde vragen over Azure, handleiding worden veelvoorkomende problemen oplossen
services: Azure HDInsight
documentationcenter: na
author: raviperi
manager: 
editor: 
ms.assetid: 74E51183-3EF4-4C67-AA60-6E12FAC999B5
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/2/2017
ms.author: raviperi
ms.openlocfilehash: c0295af2e71d891d07dad7012b7a27402c375178
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="troubleshoot-storm-by-using-azure-hdinsight"></a>Storm oplossen met behulp van Azure HDInsight

Meer informatie over de meest voorkomende problemen en hun oplossingen voor het werken met Apache Storm-nettoladingen in Apache Ambari.

## <a name="how-do-i-access-the-storm-ui-on-a-cluster"></a>Hoe krijg ik toegang tot de Storm-gebruikersinterface op een cluster?
U hebt twee opties voor toegang tot de Storm-gebruikersinterface vanuit een browser:

### <a name="ambari-ui"></a>Ambari-gebruikersinterface
1. Ga naar de Ambari-dashboard.
2. Selecteer in de lijst met services **Storm**.
3. In de **snelkoppelingen** selecteert u **Storm-gebruikersinterface**.

### <a name="direct-link"></a>Directe koppeling
U kunt toegang tot de Storm-gebruikersinterface op de volgende URL:

https://\<cluster DNS-naam\>/stormui

Voorbeeld:

 https://stormcluster.azurehdinsight.NET/stormui

## <a name="how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-to-another"></a>Hoe worden ik Storm event hub spout controlepunt informatie uit een topologie overgedragen naar een andere?

Bij het ontwikkelen van topologieën die uit Azure Event Hubs gelezen met behulp van de HDInsight Storm event hub spout JAR-bestand, moet u een topologie met dezelfde naam op een nieuw cluster implementeren. Echter, moet u de gegevens van de controlepunten die is toegewezen aan Apache ZooKeeper op het oude cluster behouden.

### <a name="where-checkpoint-data-is-stored"></a>Waar controlepuntgegevens worden opgeslagen
Controlepuntgegevens voor offsets worden opgeslagen door de event hub spout in ZooKeeper in twee hoofdmappen:
- Niet-transactionele spout controlepunten worden opgeslagen in /eventhubspout.
- Transactionele spout controlepuntgegevens worden opgeslagen in / transactionele.

### <a name="how-to-restore"></a>Herstellen
Als u de scripts en bibliotheken die u gebruikt om te exporteren van gegevens buiten ZooKeeper en importeer de gegevens terug naar ZooKeeper met een nieuwe naam, Zie [HDInsight Storm voorbeelden](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).

De map lib heeft JAR-bestanden met de implementatie voor de bewerking exporteren/importeren. De map bash bevat een voorbeeldscript dat laat zien hoe u gegevens uit de ZooKeeper server op het oude cluster exporteren en vervolgens importeren naar de server ZooKeeper op het nieuwe cluster.

Voer de [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) script uit de ZooKeeper-knooppunten om te exporteren en vervolgens gegevens importeren. Het script met de juiste Hortonworks Data Platform HDP ()-versie bijwerken. (We werken over het maken van deze scripts algemene in HDInsight. Algemene scripts kunnen uitvoeren vanaf elk knooppunt in het cluster zonder wijzigingen door de gebruiker.)

De metagegevens van schrijft de opdracht exporteren naar een pad Apache Hadoop Distributed File System (HDFS) (in een store Azure Blob Storage of Azure Data Lake Store) op een locatie die u instelt.

### <a name="examples"></a>Voorbeelden

#### <a name="export-offset-metadata"></a>Offset metagegevens exporteren
1. SSH gebruiken om te gaan met het cluster ZooKeeper op het cluster waaruit het controlepunt van de offset moet worden geëxporteerd.
2. Voer de volgende opdracht (na het bijwerken van de versietekenreeks HDP) ZooKeeper offset om gegevens te exporteren naar het /stormmetadta/zkdata HDFS-pad:

    ```apache   
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
    ```

#### <a name="import-offset-metadata"></a>Offset metagegevens importeren
1. SSH gebruiken om te gaan met het cluster ZooKeeper op het cluster waaruit het controlepunt van de offset moet worden geëxporteerd.
2. Voer de volgende opdracht (na het bijwerken van de versietekenreeks HDP) ZooKeeper offset om gegevens te importeren uit de /stormmetadata/zkdata HDFS-pad naar de ZooKeeper server op de doelcluster:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
    ```
   
#### <a name="delete-offset-metadata-so-that-topologies-can-start-processing-data-from-the-beginning-or-from-a-timestamp-that-the-user-chooses"></a>Offset metagegevens verwijderen zodat topologieën verwerken van gegevens starten kunnen vanaf het begin of vanuit een tijdstempel dat de gebruiker kiest
1. SSH gebruiken om te gaan met het cluster ZooKeeper op het cluster waaruit het controlepunt van de offset moet worden geëxporteerd.
2. Voer de volgende opdracht (na het bijwerken van de versietekenreeks HDP) om alle ZooKeeper offset gegevens in het huidige cluster te verwijderen:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
    ```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a>Hoe ik Storm binaire bestanden vinden op een cluster?
Er zijn een storm-binaire bestanden voor de huidige HDP-stack in /usr/hdp/current/storm-client. De locatie is hetzelfde voor hoofdknooppunten en worker-knooppunten.
 
Er is mogelijk meerdere binaire bestanden voor specifieke HDP versies in /usr/hdp (bijvoorbeeld /usr/hdp/2.5.0.1233/storm). De map /usr/hdp/current/storm-client is symlinked naar de nieuwste versie die wordt uitgevoerd op het cluster.

Zie voor meer informatie [verbinding maken met een HDInsight-cluster via SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) en [Storm](http://storm.apache.org/).
 
## <a name="how-do-i-determine-the-deployment-topology-of-a-storm-cluster"></a>Hoe bepaal ik de implementatietopologie van een Storm-cluster
Bepaal eerst alle onderdelen die zijn geïnstalleerd met HDInsight Storm. Een Storm-cluster bestaat uit vier categorieën van knooppunt:

* Gateway-knooppunten
* HEAD-knooppunten
* ZooKeeper-knooppunten
* Worker-knooppunten
 
### <a name="gateway-nodes"></a>Gateway-knooppunten
Een gateway-knooppunt is een gateway en de reverse proxyservice waarmee voor openbare toegang tot een actieve Ambari management-service. Ambari opvulteken keuze ook verwerkt.
 
### <a name="head-nodes"></a>HEAD-knooppunten
De hoofdknooppunten storm Voer de volgende services:
* Nimbus
* Ambari-server
* Metrische gegevens Ambari-server
* Ambari metrische gegevens verzamelen
 
### <a name="zookeeper-nodes"></a>ZooKeeper-knooppunten
HDInsight wordt geleverd met een quorum van de ZooKeeper drie knooppunten. De grootte van het quorum is vast en kan niet worden geconfigureerd.
 
Storm-services in het cluster zijn geconfigureerd om automatisch te gebruiken het quorum ZooKeeper.
 
### <a name="worker-nodes"></a>Worker-knooppunten
Storm worker-knooppunten uitvoeren van de volgende services:
* Supervisor
* Werknemer Java virtuele machines (JVMs) voor het uitvoeren van topologieën
* Ambari-agent
 
## <a name="how-do-i-locate-storm-event-hub-spout-binaries-for-development"></a>Hoe ik Storm event hub spout binaire bestanden voor de ontwikkeling van vinden?
 
Zie de volgende bronnen voor meer informatie over het gebruik van Storm event hub spout JAR-bestanden met uw topologie.
 
### <a name="java-based-topology"></a>Op basis van Java-topologie
[Process events from Azure Event Hubs with Storm on HDInsight (Java)](https://docs.microsoft.com/azure/hdinsight/hdinsight-storm-develop-java-event-hub-topology) (Gebeurtenissen uit Azure Event Hubs verwerken met Storm op HDInsight (Java))
 
### <a name="c-based-topology-mono-on-hdinsight-34-linux-storm-clusters"></a>C#-topologie (Mono op HDInsight 3.4 + Linux Storm-clusters) gebaseerd
[Process events from Azure Event Hubs with Storm on HDInsight (C#)](https://docs.microsoft.com/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology) (Gebeurtenissen uit Azure Event Hubs verwerken met Storm op HDInsight (C#))
 
### <a name="latest-storm-event-hub-spout-binaries-for-hdinsight-35-linux-storm-clusters"></a>Meest recente Storm event hub spout binaire bestanden voor HDInsight 3.5 + Linux Storm-clusters
Zie voor meer informatie over het gebruik van de meest recente Storm event hub spout die geschikt is voor HDInsight 3.5 + Linux Storm-clusters, de mvn-opslagplaats [Leesmij-bestand](https://github.com/hdinsight/mvn-repo/blob/master/README.md).
 
### <a name="source-code-examples"></a>Bron-codevoorbeelden
Zie [voorbeelden](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) van het te lezen en schrijven van Azure Event Hub met behulp van een Apache Storm-topologie (geschreven in Java) op een Azure HDInsight-cluster.
 
## <a name="how-do-i-locate-storm-log4j-configuration-files-on-clusters"></a>Hoe ik Storm Log4J configuratiebestanden op clusters vinden?
 
Omgaan met Apache-Log4J configuratiebestanden voor Storm-services.
 
### <a name="on-head-nodes"></a>Over hoofdknooppunten
De configuratie van de Nimbus Log4J is gelezen uit /usr/hdp/\<HDP versie\>/storm/log4j2/cluster.xml.
 
### <a name="on-worker-nodes"></a>Op de worker-knooppunten
De leidinggevende Log4J-configuratie wordt gelezen uit /usr/hdp/\<HDP versie\>/storm/log4j2/cluster.xml.
 
Het configuratiebestand van de werknemer Log4J is gelezen uit /usr/hdp/\<HDP versie\>/storm/log4j2/worker.xml.
 
Voorbeelden: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml

### <a name="see-also"></a>Zie ook
[Problemen oplossen met behulp van Azure HDInsight](../../hdinsight/hdinsight-troubleshoot-guide.md)