---
title: HDInsight, de Hadoop-technologiestack en Hadoop-clusters - Azure | Microsoft Docs
description: Een inleiding tot HDInsight en de Hadoop-technologiestack en -onderdelen, inclusief Spark, Kafka, Hive, HBase voor analyse van big data.
keywords: azure hadoop, hadoop azure, hadoop-inleiding, hadoop-informatie, hadoop-technologiestack, inleiding tot hadoop, informatie over hadoop, wat is een hadoop-cluster, wat is hadoop-cluster, waar wordt hadoop voor gebruikt
services: hdinsight
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: e56a396a-1b39-43e0-b543-f2dee5b8dd3a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: cgronlun
ms.openlocfilehash: e5ed09ddb1556e6c76813e71bcb31cf4f792b616
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-azure-hdinsight-the-hadoop-technology-stack-and-hadoop-clusters"></a>Informatie over Azure HDInsight, de Hadoop-technologiestack en Hadoop-clusters
 In dit artikel vindt u algemene informatie over Azure HDInsight, een cloud-distributie van de Hadoop-technologiestack. Er wordt ook uitgelegd wat een Hadoop-cluster is en wanneer dit van pas komt. 

## <a name="what-is-hdinsight-and-the-hadoop-technology-stack"></a>Wat is HDInsight en de Hadoop-technologiestack? 
Azure HDInsight is een cloud-distributie van de Hadoop-onderdelen van het [Hortonworks Data Platform (HDP)](https://hortonworks.com/products/data-center/hdp/). [Apache Hadoop](http://hadoop.apache.org/) was het originele open-source framework voor gedistribueerde verwerking en analyse van sets met big data in clusters van computers. 


De Hadoop-technologiestack omvat verwante software en hulpprogramma's, waaronder Apache Hive, HBase, Spark en Kafka om er maar een paar te noemen. Raadpleeg [Components and versions available with HDInsight][component-versioning] (Onderdelen en versies die beschikbaar zijn in HDInsight) om na te gaan welke onderdelen van de Hadoop-technologiestack in HDInsight beschikbaar zijn. Zie de [pagina met Azure-functies voor HDInsight](https://azure.microsoft.com/services/hdinsight/) voor meer informatie over Hadoop in HDInsight.

## <a name="what-is-a-hadoop-cluster-and-when-do-you-use-it"></a>Wat is een Hadoop-cluster en wanneer gebruikt u het?
*Hadoop* is ook een clustertype dat beschikt over:

* YARN voor taakplanning en resourcebeheer
* MapReduce voor parallelle verwerking
* HDFS (Hadoop Distributed File System)
  
Hadoop-clusters worden het meest gebruikt voor batchverwerking van opgeslagen gegevens. Andere soorten clusters in HDInsight hebben extra mogelijkheden. Zo is Spark steeds populairder geworden vanwege de snellere, in-memory verwerking. Zie [Clustertypen in HDInsight](#overview) voor meer informatie.

## <a name="what-is-big-data"></a>Wat is big data?
Big data verwijst naar een grote hoeveelheid digitale gegevens van verschillende typen, zoals:

* Sensorgegevens van industriële apparatuur
* Klantenactiviteit verzameld op een website
* Een Twitter-nieuwsfeed

Big data wordt verzameld in steeds sneller groeiende volumes, met een steeds hogere snelheid en in een steeds groter wordend aantal indelingen. Bij big data kan het gaan om historische gegevens (opgeslagen gegevens), maar ook om realtime gegevens (gestreamd vanuit de bron). 

## <a name="overview"></a>Clustertypen in HDInsight
HDInsight omvat specifieke clustertypen en opties voor clusteraanpassing, zoals het toevoegen van onderdelen, hulpprogramma's en talen.

### <a name="spark-kafka-interactive-query-hbase-customized-and-other-cluster-types"></a>Clusters van Apache Spark, Kafka, Interactive Query en HBase, aangepaste clusters en andere clustertypen
HDInsight biedt de volgende clustertypen:

* **[Apache Hadoop](https://wiki.apache.org/hadoop)**: maakt gebruik van [HDFS](#hdfs), [YARN](#yarn) voor resourcebeheer en een eenvoudig [MapReduce](#mapreduce)-programmeermodel om batchgegevens parallel te verwerken en te analyseren.
* **[Apache Spark](http://spark.apache.org/)**: een framework voor parallelle verwerking dat ondersteuning biedt voor in-memory verwerking om de prestaties van toepassingen voor de analyse van big data te verbeteren. Spark werkt voor SQL, het streamen van gegevens en machine learning. Zie [Wat is Apache Spark in HDInsight?](hdinsight-apache-spark-overview.md)
* **[Apache HBase](http://hbase.apache.org/)**: een NoSQL-database gebouwd op Hadoop. Deze biedt willekeurige toegang en sterke consistentie voor grote hoeveelheden (mogelijk miljarden rijen bij miljoenen kolommen) ongestructureerde en semi-gestructureerde gegevens. Zie [Wat is HBase in HDInsight?](hdinsight-hbase-overview.md)
* **[Microsoft R Server](https://msdn.microsoft.com/microsoft-r/rserver)**: een server voor de hosting van websites en het beheren van parallelle, gedistribueerde R-processen. Het biedt gegevenswetenschappers, statistici en R-programmeurs on-demand toegang tot schaalbare, gedistribueerde analysemethoden in HDInsight. Zie [Overzicht van R Server in HDInsight](hdinsight-hadoop-r-server-overview.md).
* **[Apache Storm](https://storm.incubator.apache.org/)**: een gedistribueerd, realtime berekeningssysteem voor het snel verwerken van grote gegevensstromen. Storm wordt aangeboden als beheerd cluster in HDInsight. Zie [Realtime-sensorgegevens analyseren met Storm en Hadoop](hdinsight-storm-sensor-data-analysis.md).
* **[Voorbeeld van Apache Interactive Query (ook bekend onder de naam: Live Long and Process)](https://cwiki.apache.org/confluence/display/Hive/LLAP)**: caching in geheugen voor interactieve en snellere Hive-query's. Zie [Interactive Query gebruiken in HDInsight](hdinsight-hadoop-use-interactive-hive.md).
* **[Apache Kafka](https://kafka.apache.org/)**: een open-source platform dat wordt gebruikt voor het bouwen van pijplijnen en toepassingen voor het streamen van gegevens. Kafka biedt ook berichtenwachtrijfunctionaliteit waarmee u gegevensstromen kunt publiceren en zich kunt abonneren op gegevensstromen. Zie [Inleiding tot Apache Kafka in HDInsight](hdinsight-apache-kafka-introduction.md).

U kunt ook clusters configureren met behulp van de volgende methoden:
* **[Voorbeeld van in domein opgenomen clusters](hdinsight-domain-joined-introduction.md)**: een cluster toegevoegd aan een Active Directory-domein, zodat u de toegang kunt controleren en gegevens kunt beheren.
* **[Aangepaste clusters met scriptacties](hdinsight-hadoop-customize-cluster-linux.md)**: clusters met scripts die extra onderdelen installeren en worden uitgevoerd tijdens de inrichting.

### <a name="example-cluster-customization-scripts"></a>Voorbeeldscripts voor het aanpassen van clusters
Scriptacties zijn Bash-scripts in Linux die worden uitgevoerd tijdens de clusterinrichting. Ze kunnen worden gebruikt om extra onderdelen in het cluster te installeren. 

Het HDInsight-team biedt de volgende voorbeeldscripts:

* **[Hue](hdinsight-hadoop-hue-linux.md)**: een reeks web-apps die worden gebruikt voor interactie met een cluster. Alleen voor Linux-clusters.
* **[Giraph](hdinsight-hadoop-giraph-install-linux.md)**: grafiekverwerking om relaties tussen items of personen te visualiseren.
* **[Solr](hdinsight-hadoop-solr-install-linux.md)**: een bedrijfszoekplatform dat zoeken naar volledige tekst in gegevens mogelijk maakt.

Zie [Ontwikkeling van scriptacties met HDInsight](hdinsight-hadoop-script-actions-linux.md) voor informatie over het ontwikkelen van uw eigen scriptacties.

## <a name="components-and-utilities-on-hdinsight-clusters"></a>Onderdelen en hulpprogramma's in HDInsight-clusters
De volgende onderdelen en hulpprogramma's maken deel uit van HDInsight-clusters.

* **[Ambari](#ambari)**: inrichting, beheer, controle en hulpprogramma's voor clusters.
* **[Avro](#avro)** (Microsoft .NET-bibliotheek voor Avro): serialisatie van gegevens voor de Microsoft .NET-omgeving. 
* **[Hive & HCatalog](#hive)**: uitvoeren van query‘s op basis van SQL, en een beheerlaag voor tabellen en opslag.
* **[Mahout](#mahout)**: voor schaalbare machine learning-toepassingen.
* **[MapReduce](#mapreduce)**: een ouder framework voor Hadoop, gedistribueerde verwerking en resourcebeheer. Zie [YARN](#yarn).
* **[Oozie](#oozie)**: werkstroombeheer.
* **[Phoenix](#phoenix)**: relationele databaselaag over HBase.
* **[Pig](#pig)**: eenvoudiger scripts uitvoeren voor MapReduce-transformaties.
* **[Sqoop](#sqoop)**: gegevens importeren en exporteren.
* **[Tez](#tez)**: maakt het mogelijk om efficiënt gegevensintensieve processen op schaal uit te voeren.
* **[YARN](#yarn)**: resourcebeheer als kernactiviteit van Hadoop.
* **[ZooKeeper](#zookeeper)**: coördinatie van de processen in gedistribueerde systemen.

> [!NOTE]
> Zie [What are the Hadoop components and versions available with HDInsight? (Welke Hadoop-onderdelen en -versies zijn beschikbaar met HDInsight?)][component-versioning] voor informatie over de specifieke onderdelen en versie-informatie.
>
>

### <a name="ambari"></a>Ambari
Apache Ambari is bedoeld voor het inrichten, beheren en controleren van Apache Hadoop-clusters. Het bevat een intuïtieve verzameling hulpprogramma‘s voor operators en een krachtige reeks API‘s die de complexiteit van Hadoop verbergen en de werking van clusters vereenvoudigen. HDInsight-clusters in Linux bieden zowel de Ambari-browserinterface als de Ambari REST-API. Ambari-weergaven op HDInsight-clusters bieden interfacemogelijkheden voor invoegtoepassingen.
Zie [Manage HDInsight clusters by using the Ambari Web UI](hdinsight-hadoop-manage-ambari.md) (HDInsight-clusters beheren met Ambari) en <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Ambari API Reference</a> (Ambari API-naslaghandleiding).

### <a name="avro"></a>Avro (Microsoft .NET-bibliotheek voor Avro)
De Microsoft .NET-bibliotheek voor Avro implementeert de DIF-indeling van Apache Avro voor compacte binaire gegevens voor serialisatie in de Microsoft .NET-omgeving. Het betreft een taal-agnostisch schema waarmee gegevens die zijn geserialiseerd in de ene taal in een andere taal kunnen worden gelezen. Gedetailleerde informatie over deze indeling vindt u in de <a target=_"blank" href="http://avro.apache.org/docs/current/spec.html">Apache Avro-specificatie</a>. De indeling van Avro-bestanden ondersteunt het gedistribueerde MapReduce-programmeermodel, wat inhoudt dat bestanden splitsbaar zijn. Dit betekent dat elk punt in een bestand kan worden gevonden en dat u vanaf een bepaald blok kunt beginnen met lezen. Zie [Gegevens serialiseren met de Microsoft .NET-bibliotheek voor Avro](hdinsight-dotnet-avro-serialization.md) voor informatie over hoe dit in zijn werk gaat. Er wordt gewerkt aan ondersteuning voor Linux-clusters.

### <a name="hdfs"></a>HDFS
HDFS (Hadoop Distributed File System) is een bestandssysteem dat, samen met YARN en MapReduce, de kern vormt van de Hadoop-technologie. HDFS is het standaardbestandssysteem voor Hadoop-clusters in HDInsight. Zie [Use Azure storage with Azure HDInsight clusters](hdinsight-hadoop-use-blob-storage.md) (Azure-opslag gebruiken met Azure HDInsight-clusters).

### <a name="hive"></a>Hive & HCatalog
<a target="_blank" href="http://hive.apache.org/">Apache Hive</a> is datawarehousesoftware die is gebouwd op Hadoop. Hiermee kunt u grote gegevenssets in gedistribueerde opslag doorzoeken en beheren door gebruik te maken van een taal genaamd HiveQL (vergelijkbaar met SQL). Hive is, net zoals Pig, een abstractielaag bovenop MapReduce en zet query's om in een reeks MapReduce-taken. Hive heeft meer weg van een Relational Database Management Systeem dan Pig en wordt gebruik met meer gestructureerde gegevens. Voor ongestructureerde gegevens kunt u beter Pig gebruiken. Zie [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> is een tabel- en opslagbeheerlaag voor Hadoop die u een relationele weergave van gegevens biedt. In HCatalog kunt u lees- en schrijfbewerkingen uitvoeren op bestanden in elke indeling die compatibel is met een Hive SerDe (serializer-deserializer).

### <a name="mahout"></a>Mahout
<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> is een bibliotheek met machine learning-algoritmen die worden uitgevoerd in Hadoop. Machine learning-toepassingen maken gebruik van statistieken om systemen te laten leren van gegevens en om resultaten uit het verleden te gebruiken om toekomstig gedrag te voorspellen. Zie [Filmaanbevelingen genereren met Mahout op Hadoop](hdinsight-mahout.md).

### <a name="mapreduce"></a>MapReduce
MapReduce is het oudere softwareframework voor Hadoop voor schrijftoepassingen waarmee big data in batches parallel kan worden verwerkt. Met een MapReduce-taak worden grote gegevenssets gesplitst en worden de gegevens voor verwerking georganiseerd in sleutel-/waardeparen. MapReduce-taken worden uitgevoerd in [YARN](#yarn). Zie <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> in de Hadoop-wiki.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> is een coördinatiesysteem voor werkstromen waarmee Hadoop-taken worden beheerd. Het is geïntegreerd met de Hadoop-stack en biedt ondersteuning voor Hadoop-taken voor MapReduce, Pig, Hive en Sqoop. Oozie kan ook worden gebruikt voor het plannen van taken die specifiek zijn voor een systeem, zoals Java-programma's of shell-scripts. Zie [Use Oozie with Hadoop](hdinsight-use-oozie-linux-mac.md) (Oozie gebruiken met Hadoop).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> is een relationele databaselaag over HBase. Phoenix bevat een JDBC-stuurprogramma waarmee u rechtstreeks SQL-tabellen kunt doorzoeken en beheren. Phoenix vertaalt query‘s en andere instructies naar systeemeigen NoSQL API-aanroepen (in plaats van gebruik te maken van MapReduce). Hierdoor worden snellere toepassingen mogelijk die zijn gebouwd op NoSQL-opslag. Zie [Apache Phoenix en SQuirreL gebruiken met HBase-clusters](hdinsight-hbase-phoenix-squirrel.md).

### <a name="pig"></a>Pig
<a  target="_blank" href="http://pig.apache.org/">Apache Pig</a> is een geavanceerd platform waarmee u complexe MapReduce-transformaties kunt uitvoeren op grote gegevenssets door gebruik te maken van een eenvoudige scripttaal genaamd Pig Latin. Pig vertaalt de Pig Latin-scripts, zodat ze kunnen worden uitgevoerd in Hadoop. U kunt door de gebruiker gedefinieerde functies (UDF's) maken om Pig Latin uit te breiden. Zie [Use Pig with Hadoop](hdinsight-use-pig.md) (Pig gebruiken met Hadoop).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> is een hulpprogramma waarmee zo efficiënt mogelijk bulksgewijs gegevens kunnen worden overgebracht tussen Hadoop en relationele databases, zoals SQL of andere gestructureerde gegevensopslag. Zie [Sqoop gebruiken met Hadoop](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> is een toepassingsframework dat is gebouwd op Hadoop YARN. Hiermee kunnen complexe, acyclische grafieken voor algemene gegevensverwerking worden uitgevoerd. Tez is een flexibelere en krachtigere opvolger van het MapReduce-framework die ervoor zorgt dat gegevensintensieve processen, zoals Hive, efficiënter op schaal worden uitgevoerd. Zie [het gedeelte 'Apache Tez gebruiken voor verbeterde prestaties' in het artikel 'Hive en HiveQL gebruiken'](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>YARN
Apache YARN is de volgende generatie van MapReduce (MapReduce 2.0 of MRv2) en biedt ondersteuning voor uitgebreidere scenario‘s voor gegevensverwerking dan MapReduce, met grotere schaalbaarheid en realtime verwerking. YARN biedt resourcebeheer en een gedistribueerd toepassingsframework. MapReduce-taken worden uitgevoerd op YARN. Zie <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop YARN</a>.

### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> coördineert processen in grote gedistribueerde systemen met behulp van een gedeelde hiërarchische naamruimte met gegevensregisters (znodes). Znodes bevatten kleine hoeveelheden metagegevens die nodig zijn voor het coördineren van processen: status, locatie, configuratie enzovoort. Bekijk een voorbeeld van [ZooKeeper met een HBase-cluster en Apache Phoenix](hdinsight-hbase-phoenix-squirrel-linux.md). 

## <a name="programming-languages-on-hdinsight"></a>Programmeertalen in HDInsight
HDInsight-clusters (Spark, HBase, Kafka, Hadoop en andere clusters) bieden ondersteuning voor een aantal programmeertalen, maar sommige hiervan worden niet standaard geïnstalleerd. Gebruik voor bibliotheken, modules en pakketten die niet standaard zijn geïnstalleerd, [een scriptactie om het betreffende onderdeel te installeren](hdinsight-hadoop-script-actions-linux.md). 

### <a name="default-programming-language-support"></a>Standaardondersteuning voor programmeertalen
Standaard bieden HDInsight-clusters ondersteuning voor:

* Java
* Python

Extra talen kunnen worden geïnstalleerd via [scriptacties](hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>JVM-talen (Java Virtual Machine)
Naast Java kunnen in een Java Virtual Machine (JVM) ook veel andere talen worden uitgevoerd. Voor het uitvoeren van sommige talen kan echter zijn vereist dat er in het cluster extra onderdelen worden geïnstalleerd.

De volgende JVM-talen worden op HDInsight-clusters ondersteund: 

* Clojure
* Jython (Python voor Java)
* Scala

### <a name="hadoop-specific-languages"></a>Hadoop-specifieke talen
HDInsight-clusters bieden ondersteuning voor de volgende talen die specifiek zijn voor de Hadoop-technologiestack:

* Pig Latin voor Pig-taken
* HiveQL voor Hive-taken en SparkSQL

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard en HDInsight Premium
HDInsight heeft twee categorieën aanbiedingen voor big data-clouds: Standard en Premium. HDInsight Standard biedt een bedrijfscluster dat organisaties kunnen gebruiken om hun big data-werkbelastingen uit te voeren. HDInsight Premium bouwt voort op de Standard-uitvoering en biedt geavanceerde mogelijkheden voor analyse en beveiliging voor een HDInsight-cluster. Zie [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium) voor meer informatie.

## <a name="microsoft-business-intelligence-and-hdinsight"></a>Microsoft Business Intelligence en HDInsight
Vertrouwde hulpprogramma‘s voor Business Intelligence (BI) zijn beschikbaar voor het ophalen, analyseren en rapporteren van met HDInsight geïntegreerde gegevens door gebruik te maken van de invoegtoepassing Power Query of het ODBC-stuurprogramma Microsoft Hive.

* [Excel en Hadoop koppelen met Power Query](hdinsight-connect-excel-power-query.md): informatie over hoe u met behulp van Microsoft Power Query voor Excel verbinding maakt tussen Excel en het Azure Storage-account dat de gegevens uit uw HDInsight-cluster bevat. Windows-werkstation is vereist. 
* [Excel en Hadoop koppelen met het stuurprogramma Microsoft Hive ODBC](hdinsight-connect-excel-hive-odbc-driver.md): informatie over hoe u gegevens importeert uit HDInsight met behulp van het stuurprogramma Microsoft Hive ODBC. Windows-werkstation is vereist. 
* [Microsoft-cloudplatform](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): informatie over Power BI voor Office 365, het downloaden van de proefversie van SQL Server, en het instellen van SharePoint Server 2013 en SQL Server BI.
* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx)
* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx)


## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md): een Snel starten-zelfstudie voor het inrichten van HDInsight Hadoop-clusters en het uitvoeren van Hive-voorbeeldquery‘s.
* [Aan de slag met Spark in HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md): een Snel starten-zelfstudie voor het maken van een Spark-cluster en het uitvoeren van interactieve Spark SQL-query's.
* [R Server gebruiken op HDInsight](hdinsight-hadoop-r-server-get-started.md): informatie over het gebruik van R Server in HDInsight Premium.
* [HDInsight-clusters inrichten](hdinsight-hadoop-provision-linux-clusters.md): informatie over het inrichten van een HDInsight Hadoop-cluster via Azure Portal, Azure CLI of Azure PowerShell.


[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/