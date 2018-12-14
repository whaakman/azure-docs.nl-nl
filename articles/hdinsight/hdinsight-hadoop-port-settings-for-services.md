---
title: Poorten die worden gebruikt door Hadoop op HDInsight - Azure-services
description: Een lijst met poorten die worden gebruikt door Hadoop-services die worden uitgevoerd op HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: 22e15f58f3d4e7f4db3ac3bd519dbb286a36ef95
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53384136"
---
# <a name="ports-used-by-apache-hadoop-services-on-hdinsight"></a>Poorten die worden gebruikt door de services van Apache Hadoop op HDInsight

Dit document bevat een lijst van de poorten die worden gebruikt door Apache Hadoop-services die worden uitgevoerd op Linux gebaseerde HDInsight-clusters. Het bevat ook informatie over poorten die worden gebruikt verbinding maken met het cluster via SSH.

## <a name="public-ports-vs-non-public-ports"></a>Openbare poorten en niet-openbare poorten

HDInsight-clusters op basis van Linux alleen blootstellen drie poorten openbaar op internet. 22, 23 en 443. Deze poorten worden gebruikt om veilig toegang krijgen tot het cluster met behulp van SSH en services die worden weergegeven via het beveiligde HTTPS-protocol.

Intern, HDInsight door meerdere virtuele Machines van Azure (de knooppunten binnen het cluster) wordt geïmplementeerd die wordt uitgevoerd op een Azure-netwerk. Uit binnen het virtuele netwerk, kunt u niet beschikbaar zijn via internet poorten openen. Bijvoorbeeld, als u verbinding met een van de hoofdknooppunten via SSH maakt, van het hoofdknooppunt u kunt vervolgens rechtstreeks toegang tot services die worden uitgevoerd op de clusterknooppunten.

> [!IMPORTANT]  
> Als u geen een Azure-netwerk als een configuratieoptie voor HDInsight opgeeft, wordt een automatisch gemaakt. U kunt andere virtuele machines (zoals andere Azure-Machines of uw ontwikkelcomputer client) echter kan niet toevoegen aan dit virtuele netwerk.


Als u wilt toevoegen als u meer machines aan het virtuele netwerk, moet u eerst het virtuele netwerk maken en vervolgens opgeven bij het maken van uw HDInsight-cluster. Zie voor meer informatie, [mogelijkheden voor HDInsight uitbreiden met behulp van een Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Openbare poorten

Alle knooppunten in een HDInsight-cluster bevinden zich in een Azure-netwerk en niet rechtstreeks toegankelijk via internet. Een openbare gateway biedt toegang tot de volgende poorten, die betrekking hebben op alle HDInsight-clustertypen internet.

| Service | Poort | Protocol | Description |
| --- | --- | --- | --- |
| sshd |22 |SSH |Verbonden clients met sshd op het primaire hoofdknooppunt. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie. |
| sshd |22 |SSH |Verbonden clients met sshd op het edge-knooppunt. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie. |
| sshd |23 |SSH |Verbonden clients met sshd op het secundaire hoofdknooppunt. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie. |
| Ambari |443 |HTTPS |Ambari-Webgebruikersinterface. Zie [beheren HDInsight met behulp van de Apache Ambari-Webinterface](hdinsight-hadoop-manage-ambari.md) |
| Ambari |443 |HTTPS |Ambari REST-API. Zie [beheren HDInsight met behulp van de Apache Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat |443 |HTTPS |HCatalog REST-API. Zie [Apache Hive gebruiken met Curl](hadoop/apache-hadoop-use-pig-curl.md), [Apache Pig gebruiken met Curl](hadoop/apache-hadoop-use-pig-curl.md), [MapReduce gebruiken met Curl](hadoop/apache-hadoop-use-mapreduce-curl.md) |
| HiveServer2 |443 |ODBC |Maakt verbinding met het Hive ODBC gebruiken. Zie [Excel verbinding maken met HDInsight met het Microsoft ODBC-stuurprogramma](hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md). |
| HiveServer2 |443 |JDBC |Maakt verbinding met ApacheHive met behulp van JDBC. Zie [verbinding maken met Apache Hive in HDInsight met behulp van het Hive JDBC-stuurprogramma](hadoop/apache-hadoop-connect-hive-jdbc-driver.md) |

De volgende zijn beschikbaar voor specifieke clustertypen:

| Service | Poort | Protocol | Clustertype | Description |
| --- | --- | --- | --- | --- |
| Stargate |443 |HTTPS |HBase |HBase REST-API. Zie [aan de slag met Apache HBase](hbase/apache-hbase-tutorial-get-started-linux.md) |
| Livy |443 |HTTPS |Spark |Spark REST-API. Zie [indienen Apache Spark-taken op afstand met behulp van Apache Livy](spark/apache-spark-livy-rest-interface.md) |
| Spark Thrift-server |443 |HTTPS |Spark |Spark Thrift-server gebruikt voor het indienen van Hive-query's. Zie [Beeline gebruiken met Apache Hive in HDInsight](hadoop/apache-hadoop-use-hive-beeline.md) |
| Storm |443 |HTTPS |Storm |Storm-Webgebruikersinterface. Zie [implementeren en beheren van Apache Storm-topologieën op HDInsight](storm/apache-storm-deploy-monitor-topology-linux.md) |

### <a name="authentication"></a>Verificatie

Alle services die openbaar worden weergegeven op het internet moeten worden geverifieerd:

| Poort | Referenties |
| --- | --- |
| 22 en 23 |De referenties van de SSH-gebruiker opgegeven tijdens het maken van clusters |
| 443 |De naam van de (standaard: beheerder) en het wachtwoord die zijn ingesteld tijdens het maken van clusters |

## <a name="non-public-ports"></a>Niet-openbare poorten

> [!NOTE]  
> Bepaalde services zijn alleen beschikbaar op specifieke clustertypen. Bijvoorbeeld: HBase is alleen beschikbaar op HBase-clustertypen.

> [!IMPORTANT]  
> Sommige services wordt alleen uitgevoerd op één hoofdknooppunt op een tijdstip. Als u probeert verbinding maken met de service op het primaire hoofdknooppunt en een foutbericht ontvangt, probeer het opnieuw met behulp van het secundaire hoofdknooppunt.

### <a name="ambari"></a>Ambari

| Service | Knooppunten | Poort | URL-pad | Protocol | 
| --- | --- | --- | --- | --- |
| Ambari-Webgebruikersinterface | Hoofdknooppunten | 8080 | / | HTTP |
| Ambari REST-API | Hoofdknooppunten | 8080 | / api/v1 | HTTP |

Voorbeelden:

* Ambari REST-API: `curl -u admin "http://10.0.0.11:8080/api/v1/clusters"`

### <a name="hdfs-ports"></a>HDFS-poorten

| Service | Knooppunten | Poort | Protocol | Description |
| --- | --- | --- | --- | --- |
| NameNode web-UI |Hoofdknooppunten |30070 |HTTPS |Web-UI status weergeven |
| NameNode metadata-service |Hoofdknooppunten |8020 |IPC |Metagegevens van het bestandssysteem |
| DataNode |Alle worker-knooppunten |30075 |HTTPS |Web-UI voor weergave status, Logboeken, enzovoort. |
| DataNode |Alle worker-knooppunten |30010 |&nbsp; |Gegevensoverdracht |
| DataNode |Alle worker-knooppunten |30020 |IPC |Bewerkingen voor metagegevens |
| Secundaire NameNode |Hoofdknooppunten |50090 |HTTP |Controlepunt voor NameNode metagegevens |

### <a name="yarn-ports"></a>YARN-poorten

| Service | Knooppunten | Poort | Protocol | Description |
| --- | --- | --- | --- | --- |
| Resource Manager-Webgebruikersinterface |Hoofdknooppunten |8088 |HTTP |Web-UI voor Resource Manager |
| Resource Manager-Webgebruikersinterface |Hoofdknooppunten |8090 |HTTPS |Web-UI voor Resource Manager |
| Resource Manager-beheerinterface |Hoofdknooppunten |8141 |IPC |Voor de toepassing inzendingen (Hive, Pig, Hive server enz.) |
| Resource Manager-scheduler |Hoofdknooppunten |8030 |HTTP |Beheerinterface |
| Toepassingsinterface van de Resource Manager |Hoofdknooppunten |8050 |HTTP |Adres van de interface voor het beheer van toepassingen |
| NodeManager |Alle worker-knooppunten |30050 |&nbsp; |Het adres van de container-manager |
| NodeManager web-UI |Alle worker-knooppunten |30060 |HTTP |Resource manager-interface |
| Adres van de tijdlijn |Hoofdknooppunten |10200 |RPC |De tijdlijn service RPC-service. |
| Tijdlijn van web-UI |Hoofdknooppunten |8181 |HTTP |De web-UI van de service in tijdlijn |

### <a name="hive-ports"></a>Hive-poorten

| Service | Knooppunten | Poort | Protocol | Description |
| --- | --- | --- | --- | --- |
| HiveServer2 |Hoofdknooppunten |10001 |Thrift |Service voor het verbinden met Hive (Thrift/JDBC) |
| Hive-Metastore |Hoofdknooppunten |9083 |Thrift |Service voor het verbinden met Hive-metagegevens (Thrift/JDBC) |

### <a name="webhcat-ports"></a>WebHCat-poorten

| Service | Knooppunten | Poort | Protocol | Description |
| --- | --- | --- | --- | --- |
| WebHCat-server |Hoofdknooppunten |30111 |HTTP |Web-API op HCatalog en andere Hadoop-services |

### <a name="mapreduce-ports"></a>MapReduce-poorten

| Service | Knooppunten | Poort | Protocol | Description |
| --- | --- | --- | --- | --- |
| JobHistory |Hoofdknooppunten |19888 |HTTP |MapReduce JobHistory web-UI |
| JobHistory |Hoofdknooppunten |10020 |&nbsp; |MapReduce JobHistory-server |
| ShuffleHandler |&nbsp; |13562 |&nbsp; |Overdrachten tussenliggende kaart uitgang reducers tegelijkertijd aanvragen |

### <a name="oozie"></a>Oozie

| Service | Knooppunten | Poort | Protocol | Description |
| --- | --- | --- | --- | --- |
| Oozie-server |Hoofdknooppunten |11000 |HTTP |URL voor Oozie-service |
| Oozie-server |Hoofdknooppunten |11001 |HTTP |Poort voor Oozie-beheerder |

### <a name="ambari-metrics"></a>Ambari metrische gegevens

| Service | Knooppunten | Poort | Protocol | Description |
| --- | --- | --- | --- | --- |
| Tijdlijn (geschiedenis van toepassing) |Hoofdknooppunten |6188 |HTTP |De web-UI van de service in tijdlijn |
| Tijdlijn (geschiedenis van toepassing) |Hoofdknooppunten |30200 |RPC |De web-UI van de service in tijdlijn |

### <a name="hbase-ports"></a>HBase-poorten

| Service | Knooppunten | Poort | Protocol | Description |
| --- | --- | --- | --- | --- |
| HMaster |Hoofdknooppunten |16000 |&nbsp; |&nbsp; |
| HMaster-info-Webinterface |Hoofdknooppunten |16010 |HTTP |De poort voor de web-HBase Master UI |
| Regioserver |Alle worker-knooppunten |16020 |&nbsp; |&nbsp; |
| &nbsp; |&nbsp; |2181 |&nbsp; |De poort die clients gebruiken voor verbinding met ZooKeeper |

### <a name="kafka-ports"></a>Kafka-poorten

| Service | Knooppunten | Poort | Protocol | Description |
| --- | --- | --- | --- | --- |
| Broker |Worker-knooppunten |9092 |[Kafka Wire Protocol](http://kafka.apache.org/protocol.html) |Gebruikt voor communicatie van clients |
| &nbsp; |ZooKeeper-knooppunten |2181 |&nbsp; |De poort die clients gebruiken voor verbinding met Zookeeper |

### <a name="spark-ports"></a>Spark-poorten

| Service | Knooppunten | Poort | Protocol | URL-pad | Description |
| --- | --- | --- | --- | --- | --- |
| Spark Thrift-servers |Hoofdknooppunten |10002 |Thrift | &nbsp; | Service voor de verbinding met Spark SQL (Thrift/JDBC) |
| Livy-server | Hoofdknooppunten | 8998 | HTTP | &nbsp; | Service voor het uitvoeren van instructies, taken en -toepassingen |
| Jupyter Notebook | Hoofdknooppunten | 8001 | HTTP | &nbsp; | Jupyter notebook website |

Voorbeelden:

* Livy: `curl -u admin -G "http://10.0.0.11:8998/"`. In dit voorbeeld `10.0.0.11` is het IP-adres van het hoofdknooppunt die als host fungeert voor de Livy-service.
