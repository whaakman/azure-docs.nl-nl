---
title: Poorten die worden gebruikt met Hadoop op HDInsight - Azure-services | Microsoft Docs
description: Een lijst met poorten die worden gebruikt door services Hadoop op HDInsight.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: dd14aed9-ec25-4bb3-a20c-e29562735a7d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/07/2018
ms.author: larryfr
ms.openlocfilehash: 70bb69c78a23c9ffe012c0b775c98355da7cbce6
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="ports-used-by-hadoop-services-on-hdinsight"></a>Poorten die worden gebruikt door services Hadoop in HDInsight

Dit document bevat een lijst met de poorten gebruikt door services Hadoop op Linux gebaseerde HDInsight-clusters. Het bevat ook informatie op waarmee verbinding wordt gemaakt met het cluster via SSH-poorten.

## <a name="public-ports-vs-non-public-ports"></a>Openbare poorten versus niet-openbaar poorten

HDInsight-clusters op basis van Linux alleen zichtbaar drie poorten openbaar op internet. 22, 23 en 443. Deze poorten worden gebruikt voor veilige toegang tot het cluster via SSH en services die worden weergegeven via het beveiligde HTTPS-protocol.

Intern HDInsight door verschillende Azure Virtual Machines (de knooppunten in het cluster) is geïmplementeerd op een virtuele Azure-netwerk worden uitgevoerd. Van binnen het virtuele netwerk, kunt u niet beschikbaar zijn via internet poorten openen. Bijvoorbeeld, als u verbinding met een van de hoofdknooppunten via SSH, vanaf het hoofdknooppunt van u kunt vervolgens rechtstreeks toegang tot services die worden uitgevoerd op de clusterknooppunten.

> [!IMPORTANT]
> Als u geen een Azure-netwerk als een configuratieoptie voor HDInsight opgeeft, wordt een automatisch gemaakt. U kunt andere machines (zoals andere Azure Virtual Machines of uw ontwikkelcomputer client) echter niet koppelen in dit virtuele netwerk.


Voor aanvullende machines toevoegen aan het virtuele netwerk, moet u eerst het virtuele netwerk maken en vervolgens bij het maken van uw HDInsight-cluster opgeven. Zie voor meer informatie [mogelijkheden uitbreiden HDInsight met behulp van een Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Openbare poorten

Alle knooppunten in een HDInsight-cluster bevinden zich in een Azure-netwerk en niet rechtstreeks toegankelijk zijn vanaf internet. Een openbare gateway biedt internettoegang tot de volgende poorten die door alle HDInsight-clustertypen gedeeld worden.

| Service | Poort | Protocol | Beschrijving |
| --- | --- | --- | --- | --- |
| sshd |22 |SSH |Clients verbinding met sshd op de primaire headnode. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie. |
| sshd |22 |SSH |Clients verbinding met sshd op de edge-knooppunt. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie. |
| sshd |23 |SSH |Clients verbinding met sshd op de secundaire headnode. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie. |
| Ambari |443 |HTTPS |Ambari-webgebruikersinterface. Zie [beheren HDInsight met de Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md) |
| Ambari |443 |HTTPS |Ambari REST-API. Zie [beheren HDInsight met de Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat |443 |HTTPS |HCatalog REST-API. Zie [Hive gebruiken met Curl](hadoop/apache-hadoop-use-pig-curl.md), [Pig gebruiken met Curl](hadoop/apache-hadoop-use-pig-curl.md), [MapReduce gebruiken met Curl](hadoop/apache-hadoop-use-mapreduce-curl.md) |
| HiveServer2 |443 |ODBC |Maakt verbinding met ODBC met Hive. Zie [Excel verbinding naar HDInsight met Microsoft ODBC driver](hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md). |
| HiveServer2 |443 |JDBC |Maakt verbinding met JDBC met Hive. Zie [verbinding maken met Hive in HDInsight met behulp van de component JDBC-stuurprogramma](hadoop/apache-hadoop-connect-hive-jdbc-driver.md) |

De volgende zijn beschikbaar voor specifieke clustertypen:

| Service | Poort | Protocol | Clustertype | Beschrijving |
| --- | --- | --- | --- | --- |
| Stargate |443 |HTTPS |HBase |HBase REST-API. Zie [aan de slag met HBase](hbase/apache-hbase-tutorial-get-started-linux.md) |
| Livy |443 |HTTPS |Spark |Spark REST-API. Zie [Spark verzenden van taken op afstand met behulp van Livy](spark/apache-spark-livy-rest-interface.md) |
| Storm |443 |HTTPS |Storm |Storm-webgebruikersinterface. Zie [implementeren en beheren van Storm-topologieën op HDInsight](storm/apache-storm-deploy-monitor-topology-linux.md) |

### <a name="authentication"></a>Verificatie

Alle services die openbaar worden weergegeven op het internet moeten worden geverifieerd:

| Poort | Referenties |
| --- | --- |
| 22 of 23 |De SSH-gebruikersreferenties die is opgegeven tijdens het maken van het cluster |
| 443 |De aanmeldingsnaam (standaard: admin) en het wachtwoord die zijn ingesteld tijdens het maken van het cluster |

## <a name="non-public-ports"></a>Niet-openbaar poorten

> [!NOTE]
> Sommige services zijn alleen beschikbaar op specifieke clustertypen. Bijvoorbeeld: HBase is alleen beschikbaar op de HBase-clustertypen.

> [!IMPORTANT]
> Sommige services wordt alleen uitgevoerd op één headnode tegelijk. Als u probeert verbinding maken met de service op de primaire headnode en een foutbericht ontvangt, probeert u opnieuw met behulp van de secundaire headnode.

### <a name="ambari"></a>Ambari

| Service | Knooppunten | Poort | URL-pad | Protocol | 
| --- | --- | --- | --- | --- |
| Ambari-webgebruikersinterface | HEAD-knooppunten | 8080 | / | HTTP |
| Ambari REST-API | HEAD-knooppunten | 8080 | / api/v1 | HTTP |

Voorbeelden:

* Ambari REST-API:`curl -u admin "http://10.0.0.11:8080/api/v1/clusters"`

### <a name="hdfs-ports"></a>HDFS-poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| --- | --- | --- | --- | --- |
| NameNode webgebruikersinterface |HEAD-knooppunten |30070 |HTTPS |Web-UI status weergeven |
| Service voor NameNode metagegevens |HEAD-knooppunten |8020 |IPC |Metagegevens van het systeem |
| DataNode |Alle worker-knooppunten |30075 |HTTPS |Web-UI status weergeven, Logboeken, enzovoort. |
| DataNode |Alle worker-knooppunten |30010 |&nbsp; |Gegevensoverdracht |
| DataNode |Alle worker-knooppunten |30020 |IPC |Bewerkingen voor metagegevens |
| Secundaire NameNode |HEAD-knooppunten |50090 |HTTP |Controlepunt voor NameNode metagegevens |

### <a name="yarn-ports"></a>YARN-poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| --- | --- | --- | --- | --- |
| Resource Manager-webgebruikersinterface |HEAD-knooppunten |8088 |HTTP |Webgebruikersinterface voor Resource Manager |
| Resource Manager-webgebruikersinterface |HEAD-knooppunten |8090 |HTTPS |Webgebruikersinterface voor Resource Manager |
| Resource Manager-beheerinterface |HEAD-knooppunten |8141 |IPC |Voor voorbeelden van de toepassing (Hive, Pig, Hive server enz.) |
| Planner van Resource Manager |HEAD-knooppunten |8030 |HTTP |Beheerinterface |
| Toepassingsinterface van de Resource Manager |HEAD-knooppunten |8050 |HTTP |Adres van de interface van de manager toepassingen |
| NodeManager |Alle worker-knooppunten |30050 |&nbsp; |Het adres van de container-manager |
| NodeManager webgebruikersinterface |Alle worker-knooppunten |30060 |HTTP |Resource manager-interface |
| Adres van de tijdlijn |HEAD-knooppunten |10200 |RPC |De tijdlijn service RPC-service. |
| Tijdlijn webgebruikersinterface |HEAD-knooppunten |8181 |HTTP |De tijdlijn service-webgebruikersinterface |

### <a name="hive-ports"></a>Hive-poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| --- | --- | --- | --- | --- |
| HiveServer2 |HEAD-knooppunten |10001 |Thrift |Service voor het verbinden met Hive (Thrift/JDBC) |
| Hive-Metastore |HEAD-knooppunten |9083 |Thrift |Service voor het verbinden met Hive-metagegevens (Thrift/JDBC) |

### <a name="webhcat-ports"></a>WebHCat-poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| --- | --- | --- | --- | --- |
| WebHCat-server |HEAD-knooppunten |30111 |HTTP |Web-API boven op HCatalog en andere Hadoop-services |

### <a name="mapreduce-ports"></a>MapReduce-poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| --- | --- | --- | --- | --- |
| JobHistory |HEAD-knooppunten |19888 |HTTP |MapReduce JobHistory webgebruikersinterface |
| JobHistory |HEAD-knooppunten |10020 |&nbsp; |MapReduce JobHistory server |
| ShuffleHandler |&nbsp; |13562 |&nbsp; |Overdrachten tussenliggende kaart uitgang verkleiningstoestellen aanvragen |

### <a name="oozie"></a>Oozie

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| --- | --- | --- | --- | --- |
| Oozie-server |HEAD-knooppunten |11000 |HTTP |URL voor Oozie-service |
| Oozie-server |HEAD-knooppunten |11001 |HTTP |Poort voor Oozie-beheerder |

### <a name="ambari-metrics"></a>Ambari metrische gegevens

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| --- | --- | --- | --- | --- |
| Tijdlijn (toepassingsrevisies) |HEAD-knooppunten |6188 |HTTP |De tijdlijn service-webgebruikersinterface |
| Tijdlijn (toepassingsrevisies) |HEAD-knooppunten |30200 |RPC |De tijdlijn service-webgebruikersinterface |

### <a name="hbase-ports"></a>HBase-poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| --- | --- | --- | --- | --- |
| HMaster |HEAD-knooppunten |16000 |&nbsp; |&nbsp; |
| HMaster info Webgebruikersinterface |HEAD-knooppunten |16010 |HTTP |De poort voor de HBase-Master-webgebruikersinterface |
| Regio-server |Alle worker-knooppunten |16020 |&nbsp; |&nbsp; |
| &nbsp; |&nbsp; |2181 |&nbsp; |De poort die clients gebruiken om te verbinden met ZooKeeper |

### <a name="kafka-ports"></a>Kafka-poorten

| Service | Knooppunten | Poort | Protocol | Beschrijving |
| --- | --- | --- | --- | --- |
| Broker |Worker-knooppunten |9092 |[Kafka Wire Protocol](http://kafka.apache.org/protocol.html) |Gebruikt voor communicatie van clients |
| &nbsp; |Zookeeper-knooppunten |2181 |&nbsp; |De poort die clients gebruiken om te verbinden met Zookeeper |

### <a name="spark-ports"></a>Spark-poorten

| Service | Knooppunten | Poort | Protocol | URL-pad | Beschrijving |
| --- | --- | --- | --- | --- | --- |
| Spark Thrift-servers |HEAD-knooppunten |10002 |Thrift | &nbsp; | Service voor de verbinding met Spark SQL (Thrift/JDBC) |
| Livy server | HEAD-knooppunten | 8998 | HTTP | &nbsp; | Service voor het uitvoeren van instructies, taken en toepassingen |
| Jupyter-notebook | HEAD-knooppunten | 8001 | HTTP | &nbsp; | Jupyter-notebook website |

Voorbeelden:

* Livy: `curl -u admin -G "http://10.0.0.11:8998/"`. In dit voorbeeld `10.0.0.11` is het IP-adres van de headnode die als host fungeert voor de service Livy.