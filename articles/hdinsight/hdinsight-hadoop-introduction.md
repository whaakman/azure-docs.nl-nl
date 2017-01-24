---
title: Wat is Hadoop? Inleiding tot Azure HDInsight | Microsoft Docs
description: Een inleiding tot het ecosysteem en de onderdelen van Hadoop in HDInsight. HDInsight omvat Hadoop Spark, HBase en meer voor de verwerking en analyse van big data.
keywords: big data-analyse, inleiding tot hadoop, wat is hadoop, hadoop-technologiestack, hadoop-ecosysteem
services: hdinsight
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: e56a396a-1b39-43e0-b543-f2dee5b8dd3a
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/14/2016
ms.author: cgronlun
translationtype: Human Translation
ms.sourcegitcommit: 10d684bd6c9408f2fbd48a0a8804b011f098d71c
ms.openlocfilehash: 8f38b9eaf8bcb498a7983756cbf57f3e372d7fe0


---
# <a name="an-introduction-to-the-hadoop-ecosystem-on-azure-hdinsight"></a>Een inleiding tot het Hadoop-ecosysteem in Azure HDInsight
 Dit artikel bevat een inleiding tot Hadoop op Azure HDInsight en het ecosysteem en de big data van Hadoop. Lees meer over de Hadoop-onderdelen, algemene concepten en scenario's voor big data-analyse.

## <a name="what-is-hadoop-on-hdinsight"></a>Wat is Hadoop op HDInsight?
Hadoop verwijst naar een ecosysteem van open-source-software dat een raamwerk biedt voor gedistribueerde verwerking, opslag en analyse van big data-sets op clusters van computers. Azure HDInsight maakt Hadoop-onderdelen uit het **HDP-distributie (Hortonworks Data Platform)** beschikbaar in de cloud en implementeert beheerde clusters met hoge betrouwbaarheid en beschikbaarheid. Bovendien biedt het beveiliging en beheer op ondernemingsniveau met Active Directory.  

Apache Hadoop was het oorspronkelijke open-source-project voor big data-verwerking. Hierna volgde de ontwikkeling van gerelateerde software en hulpprogramma's die als onderdeel van de Hadoop-technologiestack worden beschouwd, waaronder Apache Hive, Apache HBase, Apache Spark, Apache Kafka en vele andere. Zie [Overzicht van het Hadoop-ecosysteem in HDInsight](#overview) voor meer informatie.

## <a name="what-is-big-data"></a>Wat is big data?
Big data kan betrekking hebben op elke willekeurige bulk aan digitale gegevens: van tekst uit Twitter-feeds tot de sensorinformatie van industriële apparatuur, en informatie over surfgedrag van klanten en aankopen op een website. Bij big data kan het gaan om historische gegevens (opgeslagen gegevens), maar ook om realtime gegevens (rechtstreeks gestreamd vanuit de bron). Big data worden verzameld in steeds sneller groeiende volumes en in een steeds groter wordend aantal indelingen.

Om bruikbare informatie of inzichten te verkrijgen uit big data, moet u de relevante gegevens verzamelen en de juiste vragen stellen. U moet er ook voor zorgen dat de gegevens beschikbaar zijn, zijn opgeschoond, zijn geanalyseerd en vervolgens op een bruikbare manier worden gepresenteerd. Big data-analyse met Hadoop in HDInsight kan u hierbij helpen.

## <a name="a-nameoverviewaoverview-of-the-hadoop-ecosystem-in-hdinsight"></a><a name="overview"></a>Overzicht van het Hadoop-ecosysteem in HDInsight
HDInsight is een clouddistributie van de snelgroeiende Apache Hadoop-technologiestack in Microsoft Azure voor big data-analyse. HDInsight bevat implementaties van Apache Spark, HBase, Kafka, Storm, Pig, Hive, Interactive Hive, Sqoop, Oozie, Ambari, enzovoort. HDInsight kan ook worden geïntegreerd met hulpprogramma's voor Business Intelligence (BI), zoals Power BI, Excel, SQL Server Analysis Services en SQL Server Reporting Services.

### <a name="hadoop-hbase-spark-kafka-interactive-hive-storm-customized-and-other-clusters"></a>Clusters van Hadoop, HBase, Spark, Kafka, Interactive Hive, Storm, aangepaste clusters en andere clusters
HDInsight biedt de volgende clustertypen:

* **[Apache Hadoop](https://wiki.apache.org/hadoop)**: biedt betrouwbare gegevensopslag met [HDFS](#hdfs) en een eenvoudig [MapReduce](#mapreduce)-programmeermodel om gegevens parallel te verwerken en te analyseren.
* **[Apache Spark](http://spark.apache.org/)**: een framework voor parallelle verwerking dat ondersteuning biedt voor in-memory verwerking om de prestaties van toepassingen voor de analyse van big data te verbeteren. Spark werkt voor SQL, het streamen van gegevens en machine learning. Zie [Overzicht: wat is Apache Spark in HDInsight?](hdinsight-apache-spark-overview.md)
* **[Apache HBase](http://hbase.apache.org/)**: een NoSQL-database gebouwd op Hadoop. Deze biedt willekeurige toegang en sterke consistentie voor grote hoeveelheden (mogelijk miljarden rijen bij miljoenen kolommen) ongestructureerde en semi-gestructureerde gegevens. Zie [Overzicht van HBase in HDInsight](hdinsight-hbase-overview.md).
* **[Microsoft R Server](https://msdn.microsoft.com/en-us/microsoft-r/rserver)**: een server van ondernemingsniveau voor de hosting van websites en het beheren van parallelle, gedistribueerde R-processen. Het biedt gegevenswetenschappers, statistici en R-programmeurs on-demand toegang tot schaalbare, gedistribueerde analysemethoden in HDInsight. Zie [Overzicht van R Server in HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-r-server-overview).
* **[Apache Storm](https://storm.incubator.apache.org/)**: een gedistribueerd, realtime berekeningssysteem voor het snel verwerken van grote gegevensstromen. Storm wordt aangeboden als beheerd cluster in HDInsight. Zie [Realtime-sensorgegevens analyseren met Storm en Hadoop](hdinsight-storm-sensor-data-analysis.md).
* **[Voorbeeld van Apache Interactive Hive (ook bekend onder de naam: Live Long and Process)](https://cwiki.apache.org/confluence/display/Hive/LLAP)**: caching in geheugen voor interactieve en snellere Hive-query's. Zie [Interactive Hive gebruiken in HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-use-interactive-hive).
* **[Voorbeeld van Apache Kafka](https://kafka.apache.org/)**: een open source-platform dat wordt gebruikt voor het bouwen van pijplijnen en toepassingen voor het streamen van gegevens. Kafka biedt ook berichtenwachtrijfunctionaliteit waarmee u gegevensstromen kunt publiceren en zich kunt abonneren op gegevensstromen. Zie [Inleiding tot Apache Kafka in HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-kafka-introduction).
* **[Voorbeeld van in domein opgenomen clusters](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-domain-joined-introduction)**: een cluster toegevoegd aan een Active Directory-domein, zodat u de toegang kunt controleren en gegevens kunt beheren.
* **[Aangepaste clusters met scriptacties](hdinsight-hadoop-customize-cluster-linux.md)**: clusters met scripts die extra onderdelen installeren en worden uitgevoerd tijdens de inrichting.

### <a name="example-customization-scripts"></a>Voorbeeld van aanpassingsscripts
Scriptacties zijn Bash-scripts in Linux die worden uitgevoerd tijdens de clusterinrichting. Ze kunnen worden gebruikt om extra onderdelen op het cluster te installeren.

Het HDInsight-team biedt de volgende voorbeeldscripts:

* [Hue](hdinsight-hadoop-hue-linux.md): een reeks web-apps die worden gebruikt voor interactie met een cluster. Alleen voor Linux-clusters.
* [Giraph](hdinsight-hadoop-giraph-install-linux.md): grafiekverwerking om relaties tussen items of personen te visualiseren.
* [R](hdinsight-hadoop-r-scripts-linux.md): een open source-taal en -omgeving voor statistische computing gebruikt in machine learning.
* [Solr](hdinsight-hadoop-solr-install-linux.md): een bedrijfszoekplatform dat zoeken naar volledige tekst in gegevens mogelijk maakt.

Zie [Ontwikkeling van scriptacties met HDInsight](hdinsight-hadoop-script-actions-linux.md) voor informatie over het ontwikkelen van uw eigen scriptacties.

## <a name="what-are-the-hadoop-components-and-utilities"></a>Wat zijn de onderdelen en hulpprogramma's van Hadoop?
De volgende onderdelen en hulpprogramma's zijn opgenomen in de HDInsight-clusters.

* **[Ambari](#ambari)**: inrichting, beheer, controle en hulpprogramma's voor clusters.
* **[Avro](#avro)** (Microsoft .NET-bibliotheek voor Avro): serialisatie van gegevens voor de Microsoft .NET-omgeving.
* **[Hive & HCatalog](#hive)**: uitvoeren van query‘s op basis van SQL (Structured Query Language), en een beheerlaag voor tabellen en opslag.
* **[Mahout](#mahout)**: voor schaalbare machine learning-toepassingen.
* **[MapReduce](#mapreduce)**: een ouder framework voor Hadoop, gedistribueerde verwerking en resourcebeheer. Zie [YARN](#yarn), het nieuwste resourceframework.
* **[Oozie](#oozie)**: werkstroombeheer.
* **[Phoenix](#phoenix)**: relationele databaselaag over HBase.
* **[Pig](#pig)**: eenvoudiger scripts uitvoeren voor MapReduce-transformaties.
* **[Sqoop](#sqoop)**: gegevens importeren en exporteren.
* **[Tez](#tez)**: maakt het mogelijk om efficiënt gegevensintensieve processen op schaal uit te voeren.
* **[YARN](#yarn)**: deel van de kernbibliotheek van Hadoop en de volgende generatie MapReduce-softwareframework.
* **[ZooKeeper](#zookeeper)**: coördinatie van de processen in gedistribueerde systemen.

> [!NOTE]
> Zie [Hadoop components, versioning, and service offerings in HDInsight (Hadoop-onderdelen, versiebeheer en serviceaanbiedingen in HDInsight)][component-versioning] voor meer informatie over de specifieke onderdelen en versie-informatie
>
>

### <a name="a-nameambariaambari"></a><a name="ambari"></a>Ambari
Apache Ambari is bedoeld voor het inrichten, beheren en controleren van Apache Hadoop-clusters. Het bevat een intuïtieve verzameling hulpprogramma‘s voor operators en een krachtige reeks API‘s die de complexiteit van Hadoop verbergen en de werking van clusters vereenvoudigen. HDInsight-clusters in Linux bieden zowel de Ambari-webgebruikersinterface als de Ambari-REST-API, terwijl clusters in Windows een subset van de REST-API bieden. Ambari-weergaven op HDInsight-clusters bieden interfacemogelijkheden voor invoegtoepassingen.

Zie [HDInsight-clusters beheren met Ambari](hdinsight-hadoop-manage-ambari.md) (alleen voor Linux), [Hadoop-clusters in HDInsight beheren met de Ambari-API](hdinsight-monitor-use-ambari-api.md) en <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Informatie over de Apache Ambari-API</a>.

### <a name="a-nameavroaavro-microsoft-net-library-for-avro"></a><a name="avro"></a>Avro (Microsoft .NET-bibliotheek voor Avro)
De Microsoft .NET-bibliotheek voor Avro implementeert de DIF-indeling van Apache Avro voor compacte binaire gegevens voor serialisatie in de Microsoft .NET-omgeving. De bibliotheek maakt gebruik van <a target="_blank" href="http://www.json.org/">JSON (JavaScript Object Notation)</a> om een schema voor taalherkenning te definiëren dat de interoperabiliteit van talen waarborgt. Dit betekent dat gegevens die zijn geserialiseerd in één taal, kunnen worden gelezen in een andere. Gedetailleerde informatie over deze indeling vindt u in de <a target=_"blank" href="http://avro.apache.org/docs/current/spec.html">Apache Avro-specificatie</a>.
De Avro-bestandsindeling biedt ondersteuning voor het gedistribueerde MapReduce-programmeermodel. Bestanden zijn splitsbaar. Dit betekent dat elk punt in een bestand kan worden gevonden en dat u vanaf een bepaald blok kunt beginnen met lezen. Zie [Gegevens serialiseren met de Microsoft .NET-bibliotheek voor Avro](hdinsight-dotnet-avro-serialization.md) voor informatie over hoe dit in zijn werk gaat.

### <a name="a-namehdfsahdfs"></a><a name="hdfs"></a>HDFS
Hadoop Distributed File System (HDFS) is een gedistribueerd bestandssysteem dat samen met MapReduce en YARN de kern vormt van het Hadoop-ecosysteem. HDFS is het standaardbestandssysteem voor Hadoop-clusters op HDInsight.

### <a name="a-namehiveahive--hcatalog"></a><a name="hive"></a>Hive & HCatalog
<a target="_blank" href="http://hive.apache.org/">Apache Hive</a> is datawarehousesoftware die is gebouwd op Hadoop. Hiermee kunt u grote gegevenssets in gedistribueerde opslag doorzoeken en beheren door gebruik te maken van een taal genaamd HiveQL (vergelijkbaar met SQL). Hive is, net zoals Pig, een abstractie die is gebouwd op MapReduce. Wanneer Hive wordt uitgevoerd, vertaalt het query‘s naar een reeks MapReduce-taken. Uit conceptueel oogpunt staat Hive dichter bij een relationeel databasebeheersysteem dan Pig. Daarom is het met name geschikt voor gebruik met meer gestructureerde gegevens. Voor ongestructureerde gegevens kunt u beter Pig gebruiken. Zie [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> is een tabel- en opslagbeheerlaag voor Hadoop die u een relationele weergave van gegevens biedt. In HCatalog kunt u lees- en schrijfbewerkingen uitvoeren in bestanden in elke indeling waarvoor een Hive SerDe-schrijfbewerking (serializer-deserializer) kan worden uitgevoerd.

### <a name="a-namemahoutamahout"></a><a name="mahout"></a>Mahout
<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> is een schaalbare bibliotheek met machine learning-algoritmen die worden uitgevoerd op Hadoop. Machine learning-toepassingen maken gebruik van statistieken om systemen te laten leren van gegevens en om resultaten uit het verleden te gebruiken om toekomstig gedrag te voorspellen. Zie [Filmaanbevelingen genereren met Mahout op Hadoop](hdinsight-mahout.md).

### <a name="a-namemapreduceamapreduce"></a><a name="mapreduce"></a>MapReduce
MapReduce is het oudere softwareframework voor Hadoop voor schrijftoepassingen waarmee big data in batches parallel kan worden verwerkt. Met een MapReduce-taak worden grote gegevenssets gesplitst en worden de gegevens voor verwerking georganiseerd in sleutel-/waardeparen.

[YARN](#yarn) is het nieuwste resourceframework voor beheerders en toepassingen, en wordt ook wel MapReduce 2.0 genoemd. MapReduce-taken worden uitgevoerd op YARN.

Zie <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> in de Hadoop-wiki voor meer informatie over MapReduce.

### <a name="a-nameoozieaoozie"></a><a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> is een coördinatiesysteem voor werkstromen waarmee Hadoop-taken worden beheerd. Het is geïntegreerd met de Hadoop-stack en biedt ondersteuning voor Hadoop-taken voor MapReduce, Pig, Hive en Sqoop. Oozie kan ook worden gebruikt voor het plannen van taken die specifiek zijn voor een systeem, zoals Java-programma's of shell-scripts. Zie [Use Oozie with Hadoop](hdinsight-use-oozie.md) (Oozie gebruiken met Hadoop).

### <a name="a-namephoenixaphoenix"></a><a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> is een relationele databaselaag over HBase. Phoenix bevat een JDBC-stuurprogramma waarmee u rechtstreeks SQL-tabellen kunt doorzoeken en beheren. Phoenix vertaalt query‘s en andere instructies naar systeemeigen NoSQL API-aanroepen (in plaats van gebruik te maken van MapReduce). Hierdoor worden snellere toepassingen mogelijk die zijn gebouwd op NoSQL-opslag. Zie [Apache Phoenix en SQuirreL gebruiken met HBase-clusters](hdinsight-hbase-phoenix-squirrel.md).

### <a name="a-namepigapig"></a><a name="pig"></a>Pig
<a  target="_blank" href="http://pig.apache.org/">Apache Pig</a> is een platform van hoog niveau waarmee u complexe MapReduce-transformaties kunt uitvoeren op zeer grote gegevenssets door gebruik te maken van een eenvoudige scripttaal genaamd Pig Latin. Pig vertaalt de Pig Latin-scripts, zodat ze kunnen worden uitgevoerd in Hadoop. U kunt door de gebruiker gedefinieerde functies (UDF's) maken om Pig Latin uit te breiden. Zie [Use Pig with Hadoop](hdinsight-use-pig.md) (Pig gebruiken met Hadoop).

### <a name="a-namesqoopasqoop"></a><a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> is een hulpprogramma waarmee zo efficiënt mogelijk bulksgewijs gegevens kunnen worden overgebracht tussen Hadoop en relationele databases, zoals SQL of andere gestructureerde gegevensopslag. Zie [Sqoop gebruiken met Hadoop](hdinsight-use-sqoop.md).

### <a name="a-nametezatez"></a><a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> is een toepassingsframework dat is gebouwd op Hadoop YARN. Hiermee kunnen complexe, acyclische grafieken voor algemene gegevensverwerking worden uitgevoerd. Tez is een flexibelere en krachtigere opvolger van het MapReduce-framework die ervoor zorgt dat gegevensintensieve processen, zoals Hive, efficiënter op schaal worden uitgevoerd. Zie [het gedeelte 'Apache Tez gebruiken voor verbeterde prestaties' in het artikel 'Hive en HiveQL gebruiken'](hdinsight-use-hive.md#usetez).

### <a name="a-nameyarnayarn"></a><a name="yarn"></a>YARN
Apache YARN is de volgende generatie van MapReduce (MapReduce 2.0 of MRv2) en biedt ondersteuning voor uitgebreidere scenario‘s voor gegevensverwerking dan MapReduce, met grotere schaalbaarheid en realtime verwerking. YARN biedt resourcebeheer en een gedistribueerd toepassingsframework. MapReduce-taken worden uitgevoerd op YARN.

Zie <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (YARN)</a> voor meer informatie over YARN.

### <a name="a-namezookeeperazookeeper"></a><a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> coördineert processen in grote gedistribueerde systemen met behulp van een gedeelde hiërarchische naamruimte met gegevensregisters (znodes). Znodes bevatten kleine hoeveelheden metagegevens die nodig zijn voor het coördineren van processen: status, locatie, configuratie enzovoort.

## <a name="programming-languages-on-hdinsight"></a>Programmeertalen in HDInsight
HDInsight-clusters (Hadoop-, HBase-, Storm- en Spark-clusters) bieden ondersteuning voor een aantal programmeertalen, maar sommige hiervan zijn niet standaard geïnstalleerd. Gebruik voor bibliotheken, modules en pakketten die niet standaard zijn geïnstalleerd, een scriptactie om het betreffende onderdeel te installeren. Zie [Scriptactieontwikkeling met HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="default-programming-language-support"></a>Standaardondersteuning voor programmeertalen
Standaard bieden HDInsight-clusters ondersteuning voor:

* Java
* Python

Extra talen kunnen worden geïnstalleerd met behulp van scriptacties: [Scriptactieontwikkeling met HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>JVM-talen (Java Virtual Machine)
Naast Java kunnen met behulp van een Java Virtual Machine (JVM) ook veel andere talen worden uitgevoerd. Voor het uitvoeren van sommige talen kan echter zijn vereist dat er op het cluster extra onderdelen worden geïnstalleerd.

De volgende JVM-talen worden op HDInsight-clusters ondersteund: 

* Clojure
* Jython (Python voor Java)
* Scala

### <a name="hadoop-specific-languages"></a>Hadoop-specifieke talen
HDInsight-clusters bieden ondersteuning voor de volgende talen die specifiek zijn voor het Hadoop-ecosysteem:

* Pig Latin voor Pig-taken
* HiveQL voor Hive-taken en SparkSQL

## <a name="a-nameadvantageaadvantages-of-hadoop-in-the-cloud"></a><a name="advantage"></a>Voordelen van Hadoop in de cloud
Als onderdeel van het Azure-cloudecosysteem biedt Hadoop in HDInsight een aantal voordelen, waaronder:

* Automatische inrichting van Hadoop-clusters. HDInsight-clusters zijn veel gemakkelijker te maken dan handmatig geconfigureerde Hadoop-clusters. Zie [Hadoop-clusters inrichten in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) voor meer informatie.
* Geavanceerde Hadoop-onderdelen. Zie [Hadoop components, versioning, and service offerings in HDInsight (Hadoop-onderdelen, versiebeheer en serviceaanbiedingen in HDInsight)][component-versioning] voor meer informatie.
* Hoge beschikbaarheid en betrouwbaarheid van clusters. Zie [Beschikbaarheid en betrouwbaarheid van Hadoop-clusters in HDInsight](hdinsight-high-availability-linux.md) voor meer informatie.
* Efficiënte en voordelige gegevensopslag met Azure Blob Storage of Azure Data Lake Store, allebei opslagopties die compatibel zijn met Hadoop. Zie [Azure Blob Storage gebruiken met Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md) of [Data Lake Store gebruiken met een HDInsight-cluster](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal) voor meer informatie.
* Integratie met andere Azure-services, waaronder [web-apps](https://azure.microsoft.com/documentation/services/app-service/web/) en [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).
* Aanvullende VM-grootten en -typen voor het uitvoeren van HDInsight-clusters. Zie [Hadoop components, versioning, and service offerings in HDInsight (Hadoop-onderdelen, versiebeheer en serviceaanbiedingen in HDInsight)][component-versioning] voor meer informatie.
* Clusterschaling. Met clusterschaling kunt u het aantal knooppunten van een actief HDInsight-cluster wijzigen zonder dat u het cluster hoeft te verwijderen of opnieuw hoeft te maken.
* Ondersteuning voor Virtual Network. HDInsight-clusters kunnen worden gebruikt met Azure Virtual Network ter ondersteuning van de isolatie van cloudresources of hybride scenario's die cloudresources koppelen aan de resources in uw datacenter.
* Lage instapkosten. Start een [gratis proefversie](https://azure.microsoft.com/free/) of raadpleeg de [prijsgegevens voor HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

Zie de [pagina met Azure-functies voor HDInsight][marketing-page] voor meer informatie over de voordelen van Hadoop in HDInsight.

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard en HDInsight Premium
HDInsight heeft twee categorieën aanbiedingen voor big data-clouds: Standard en Premium. HDInsight Standard biedt een bedrijfscluster dat organisaties kunnen gebruiken om hun big data-werkbelastingen uit te voeren. HDInsight Premium bouwt hierop voort en biedt geavanceerde mogelijkheden voor analyse en beveiliging voor een HDInsight-cluster. Zie [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium) voor meer informatie.

## <a name="a-idresourcesaresources-for-learning-more-about-big-data-analysis-hadoop-and-hdinsight"></a><a id="resources"></a>Resources voor meer informatie over big data-analyse, Hadoop en HDInsight
Bouw voort op deze inleiding tot Hadoop in de cloud en big data-analyse met onderstaande resources.

### <a name="hadoop-documentation-for-hdinsight"></a>Hadoop-documentatie voor HDInsight
* [HDInsight-documentatie](https://azure.microsoft.com/documentation/services/hdinsight/): de documentatiepagina voor Hadoop op Azure HDInsight met koppelingen naar artikelen, video‘s en aanvullende resources.
* [Aan de slag met Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md): een Snel starten-zelfstudie voor het inrichten van HDInsight Hadoop-clusters en het uitvoeren van Hive-voorbeeldquery‘s.
* [Aan de slag met Spark in HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md): een Snel starten-zelfstudie voor het maken van een Spark-cluster en het uitvoeren van interactieve Spark SQL-query's.
* [R Server gebruiken op HDInsight](hdinsight-hadoop-r-server-get-started.md): informatie over het gebruik van R Server in HDInsight Premium.
* [HDInsight-clusters inrichten](hdinsight-hadoop-provision-linux-clusters.md): informatie over het inrichten van een HDInsight Hadoop-cluster via Azure Portal, Azure CLI of Azure PowerShell.

### <a name="apache-hadoop"></a>Apache Hadoop
* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>: meer informatie over de Apache Hadoop-softwarebibliotheek, een framework dat gedistribueerde verwerking van grote gegevenssets tussen verschillende clusters computers mogelijk maakt.
* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">HDFS</a>: meer informatie over de architectuur en het ontwerp van Hadoop Distributed File System, het primaire opslagsysteem dat wordt gebruikt voor Hadoop-toepassingen.
* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">MapReduce-zelfstudie</a>: meer informatie over het programmeerframework voor het schrijven van Hadoop-toepassingen, waarmee razendsnel grote hoeveelheden gegevens parallel kunnen worden verwerkt op grote clusters rekenknooppunten.

### <a name="microsoft-business-intelligence"></a>Microsoft Business Intelligence
Indien deze zijn geïntegreerd met HDInsight, kunt u met de vertrouwde hulpprogramma‘s voor Business Intelligence (BI), zoals Excel, PowerPivot, en SQL Server Analysis Services, gegevens ophalen, analyseren en rapporteren door de invoegtoepassing Power Query of het stuurprogramma Microsoft Hive ODBC te gebruiken.

Deze BI-hulpprogramma's kunnen u helpen met big data-analyse:

* [Excel en Hadoop koppelen met Power Query](hdinsight-connect-excel-power-query.md): informatie over hoe u met behulp van Microsoft Power Query voor Excel verbinding maakt tussen Excel en het Azure Storage-account dat de gegevens bevat die zijn gekoppeld aan het HDInsight-cluster. Windows-werkstation is vereist. Werkt met clusters in Linux of Windows.
* [Excel en Hadoop koppelen met het stuurprogramma Microsoft Hive ODBC](hdinsight-connect-excel-hive-odbc-driver.md): informatie over hoe u gegevens importeert uit HDInsight met behulp van het stuurprogramma Microsoft Hive ODBC. Windows-werkstation is vereist. Werkt met clusters in Linux of Windows.
* [Microsoft-cloudplatform](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): informatie over Power BI voor Office 365, het downloaden van de proefversie van SQL Server, en het instellen van SharePoint Server 2013 en SQL Server BI.
* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx).
* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx).

[marketing-page]: https://azure.microsoft.com/services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/



<!--HONumber=Dec16_HO2-->


