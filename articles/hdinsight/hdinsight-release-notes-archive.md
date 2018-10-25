---
title: Gearchiveerde release-opmerkingen voor Azure HDInsight
description: Gearchiveerde releaseopmerkingen en versies van Azure HDInsight.
services: hdinsight
ms.reviewer: jasonh
author: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 03/20/2018
ms.author: jasonh
ms.openlocfilehash: 35fd64f75617fdaa3aaded5f1f7bdcb847733f05
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50025634"
---
# <a name="archived-release-notes-for-azure-hdinsight"></a>Gearchiveerde release-opmerkingen voor Azure HDInsight

Voor de **meest recente** Azure HDInsight LDR-updates, Zie [Release-informatie voor HDInsight](hdinsight-release-notes.md).

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie voor meer informatie, [HDInsight versiebeheer artikel](hdinsight-component-versioning.md).

## <a name="notes-for-06272018---release-of-new-open-source-versions-adls-gen2-etc-on-hdinsight-36"></a>Notities voor 27-06/2018 - Release van nieuwe versies van de open-source, ADLS Gen2 enzovoort in HDInsight 3.6
De release van juni 2018 van HDInsight is een belangrijke release met een groot aantal nieuwe updates en mogelijkheden voor onze klanten. Lees deze [boeken](https://azure.microsoft.com/blog/enterprises-get-deeper-insights-with-hadoop-and-spark-updates-on-azure-hdinsight/) voor meer informatie.

Hier volgen de belangrijkste punten. Voor de gedetailleerde releaseopmerkingen fouten die zijn verholpen, bekende problemen, enz., lees de [opmerkingen bij de Release voor Azure HDInsight](hdinsight-release-notes.md).

- **Bijwerken van Hadoop en andere open-source-projecten** – naast 1000 + oplossingen voor problemen in 20 + open source-projecten, deze update bevat een nieuwe versie van Spark (2.3) en Kafka (1.0).
- **R Server 9.1 bijwerken naar Machine Learning Services 9.3** – met deze release bieden we kunnen gegevenswetenschappers en engineers met het beste van open-source uitgebreid met algoritmische innovaties en het gemak van uitoefening, allemaal verkrijgbaar in hun de voorkeurstaal van de snelheid van Apache Spark. Deze release uitbreiding van de mogelijkheden die in R Server met toegevoegde ondersteuning voor Python, leidt tot wijziging van het cluster van R Server ML-Services. 
- **Ondersteuning voor Azure Data Lake Storage Gen2** : HDInsight biedt ondersteuning voor de Preview-versie van Azure Data Lake Storage Gen2. In de beschikbare regio's zich klanten een Gen2 ADLS-account kiezen als een archief voor hun HDInsight-clusters.
- **HDInsight Enterprise Security Package Updates (Preview)** : Virtual Network-Service-eindpunten (Preview) ondersteuning voor Azure blob Storage, ADLS Gen1, Cosmos DB en Azure DB. 

## <a name="notes-for-03202018---release-of-spark-22-on-hdinsight-36"></a>Notities voor 03/20/2018 - Release van Spark 2.2 in HDInsight 3.6

- Spark 2.2.0 verbetert de stabiliteit voor Spark Core, SQL, ML, en brengt Structured Streaming naar de status van de algemene beschikbaarheid. Spark 2.2.0 is nu beschikbaar in HDInsight 3.6.


## <a name="notes-for-08012017-release-of-hdinsight"></a>Notities voor 08-01-2017-release van HDInsight

| Titel | Beschrijving | Betrokken gebied  | Clustertype  | 
| --- | --- | --- | --- | --- |
| Release van Microsoft R Server 9.1 op HDInsight |HDInsight biedt nu ondersteuning voor inrichting clusters voor R Server 9.1 op HDInsight. Zie voor meer informatie over Microsoft R Server 9.1 release [deze blog](https://blogs.technet.microsoft.com/dataplatforminsider/2017/04/19/introducing-microsoft-r-server-9-1-release/). |Service |R Server |
| HDInsight 3.6 bevat nu nieuwere versies van de Hadoop-stack|<ul><li>Zie voor een gedetailleerde lijst met bijgewerkte versies, [Hadoop component versies beschikbaar zijn in HDInsight](hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions).</li><li>Zie voor een lijst van fouten die zijn verholpen in de meest recente versies van de Hadoop-stack, [informatie over de Apache beveiligingspatch](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/patch_parent.html).</li><li>Zie voor een lijst met belangrijke wijzigingen tussen HDP 2.6.1 (dit is nu beschikbaar in HDInsight 3.6), [ https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/behavior_changes.html ](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/behavior_changes.html).</li><li>Zie voor een lijst van bekende problemen in HDP 2.6.1, [bekende problemen](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/known_issues.html).</li></ul> |Service |Alle |N/A |
| Updates voor clusters Interactive Hive (Preview) |<ul><li><b>Verbetering van de functie.</b> Implementatie van in de cache metastore en vermindert de belasting van de back-end SQL door de metagegevens van de cache te plaatsen en verbetert de prestaties voor alle bewerkingen voor metagegevens.  Deze verbetering is nu een standaard op alle interactieve Hive-clusters. Zie voor meer informatie, [ https://issues.apache.org/jira/browse/HIVE-16520 ](https://issues.apache.org/jira/browse/HIVE-16520).</li><li><b>Verbetering van de functie.</b> Het laden van dynamische partitie is geoptimaliseerd. Zie voor meer informatie, [ https://issues.apache.org/jira/browse/HIVE-14204 ](https://issues.apache.org/jira/browse/HIVE-14204).</li><li><b>Verbetering van de functie.</b> De optimalisatie van de configuratie voor HDInsight op Linux.</li><li><b>Opgelost probleem.</b> `CredentialProviderFactory$getProviders` is niet thread-veilig. Nu is dit probleem opgelost. Zie voor meer informatie, [ https://issues.apache.org/jira/browse/HADOOP-14195 ](https://issues.apache.org/jira/browse/HADOOP-14195).</li><li><b>Opgelost probleem.</b> Hoog CPU-verbruik met WASB-stuurprogramma `liststatus` API, wat resulteert in onjuiste ATS-prestaties. Nu is dit probleem opgelost. Zie voor meer informatie, [ https://github.com/Azure/azure-storage-java/pull/154 ](https://github.com/Azure/azure-storage-java/pull/154).</li></ul> |Service |Interactive Hive (Preview) |
| Updates voor Hadoop-clusters |Betrouwbaarheid van de bewerking Templeton taken is verbeterd. Zie voor meer informatie [https://issues.apache.org/jira/browse/HIVE-15947](https://issues.apache.org/jira/browse/HIVE-15947) |Service |Hadoop |
| YARN-updates | HDInsight maakt nu een 250 GB Ambari database (zonder kosten verhogen), wat tot een betere ervaring voor klanten leidt. Deze wijziging moet voorkomen dat ATS ophalen ingevuld en hebt waarschijnlijk een betere prestaties. |Service |Alle |
| Spark-updates | Release van Spark 2.1.1. Zie voor meer informatie, [Spark versie 2.1.1](https://spark.apache.org/releases/spark-release-2-1-1.html). | Service | Spark |

  



## <a name="04062017---general-availability-of-hdinsight-36"></a>04-06-2017 - algemene beschikbaarheid van HDInsight 3.6

* Met deze release voegt Azure HDInsight versie 3.6, die is gebaseerd op HDP 2.6. Releaseopmerkingen voor HDP 2.6 vindt [hier](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html) en meer informatie over de HDInsight-versies kan worden gevonden [hier](hdinsight-component-versioning.md). HDInsight 3.6 is beschikbaar voor de volgende werkbelastingen:

    * Hadoop v2.7.3
    * HBase v1.1.2
    * Storm v1.1.0
    * Spark v2.1.0
    * Interactieve Hive v2.1.0

* **Ondersteuning voor Hive-weergave 2.0**. Hierdoor verbetert de gebruikerservaring voor Interactive Hive. Zie voor meer informatie, [Hortonworks documentatie](http://docs.hortonworks.com/HDPDocuments/Ambari-2.5.0.3/bk_ambari-views/content/ch_using_hive_view.html).

* **Prestatieverbeteringen met Hive LLAP**. Zie voor meer informatie, [Hortonworks documentatie](https://hortonworks.com/blog/top-5-performance-boosters-with-apache-hive-llap/).

* **Nieuwe functies in Hive**. Zie [Hortonworks documentatie](https://hortonworks.com/apache/hive/#section_4).

* **Afschaffing van de CLI hive**: CLI Hive wordt afgeschaft en klanten wordt aangeraden in plaats daarvan Beeline gebruiken. Zie voor meer informatie, [Apache-documentatie](https://cwiki.apache.org/confluence/display/Hive/Replacing+the+Implementation+of+Hive+CLI+Using+Beeline). Zie voor instructies over het Beeline gebruiken met HDInsight, [Beeline gebruiken met HDInsight Hadoop-clusters](hadoop/apache-hadoop-use-hive-beeline.md).

* **Nieuwe functies in Apache Phoenix en HBase**.
    * Ondersteuning voor Storage-quotum: meestal gebruikt in omgevingen met meerdere tenants, waardoor beperkte opslagruimte op een per tabel en per naamruimteniveau.
    * Phoenix indexeren verbeteringen: incrementele index maken en herstellen/hervatten indexeren van vorige fouten.
    * Hulpprogramma voor Phoenix data integriteit: biedt ondersteuning voor validatie van schema-, index- en andere metagegevens.


* **Probleem met HBase**: tijdens het uitvoeren van een CSV-bulksgewijs uploaden MapReduce-taak, de volgende syntaxis kan leiden tot een fout.

        HADOOP_CLASSPATH=$(hbase mapredcp):/path/to/hbase/conf hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table EXAMPLE --input /data/example.csv

    Gebruik in plaats daarvan de volgende syntaxis:

        HADOOP_CLASSPATH=/path/to/hbase-protocol.jar:/path/to/hbase/conf hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table EXAMPLE --input /data/example.csv


## <a name="02282017---release-of-spark-21-on-hdinsight-36-preview"></a>28-02-2017 - release van Spark 2.1 op HDInsight 3.6 (Preview)
* [Spark 2.1](http://spark.apache.org/releases/spark-release-2-1-0.html) verbetert de stabiliteit en bruikbaarheid probleem met eerdere versies. Het biedt ook nieuwe functies voor alle Spark-workloads, zoals Spark Core, SQL, ML en Streaming.
* Structured Streaming haalt verbeterde schaalbaarheid met ondersteuning voor gebeurtenis tijd watermerken en Kafka 0,10 connector.
* Spark SQL partitioneren wordt nu verwerkt met behulp van nieuwe schaalbare partitie verwerken mechanisme. Meer informatie [hier](http://spark.apache.org/releases/spark-release-2-1-0.html) over het bijwerken.
* Spark 2.1 op Azure HDInsight 3.6 Preview op dit moment biedt geen ondersteuning voor BI-hulpprogramma voor verbinding met ODBC-stuurprogramma.
* Azure Data Lake Store-toegang van Spark 2.1 clusters wordt niet ondersteund in deze Preview-versie.


## <a name="11182016---release-of-spark-201-on-hdinsight-35"></a>18-11-2016 - release van Spark 2.0.1 in HDInsight 3.5
Spark 2.0.1 is nu beschikbaar in Spark-clusters (HDInsight versie 3.5).

## <a name="11162016---release-of-r-server-90-on-hdinsight-35-spark-20"></a>16-11-2016 - release van R Server 9.0 op HDInsight 3.5 (Spark 2.0)
*   R Server-clusters bevat nu de optie voor twee versies: R Server 9.0 op HDI 3.5 (Spark 2.0) en R Server 8.0 op HDI 3.4 (Spark 1.6).
*   R Server 9.0 op HDI 3.5 (Spark 2.0) is gebaseerd op R 3.3.2 en bevat een nieuwe ScaleR data source-functies genaamd RxHiveData en rxparquetdata genaamd voor het laden van gegevens in Hive en Parquet rechtstreeks naar Spark DataFrames voor analyse door ScaleR. Zie voor meer informatie de inline Help-informatie over deze functies in R door gebruik te maken van de **? RxHiveData** en **? RxParquetData** opdrachten.
*   RStudio Server community-editie wordt nu standaard (met een optie opt-out) geïnstalleerd op de blade van de configuratie van het Cluster als onderdeel van de inrichting stroom.

## <a name="11092016---release-of-spark-20-on-hdinsight"></a>11/09/2016 - release van Spark 2.0 in HDInsight
* Spark 2.0-clusters in HDInsight 3.5 bieden nu ondersteuning voor Livy en Jupyter-services.

## <a name="10262016---release-of-r-server-on-hdinsight"></a>26-10-2016 - release van R Server op HDInsight
* De URI voor edge-knooppunt toegang is gewijzigd in **clustername**-ed-ssh.azurehdinsight.net
* R Server op HDInsight-cluster inrichten is gestroomlijnd.
* R Server op HDInsight is nu beschikbaar als normale HDInsight 'R Server' clustertype en niet meer als een afzonderlijke HDInsight-toepassing wordt geïnstalleerd. De edge-knooppunt en R Server binaire bestanden worden nu ingericht als onderdeel van de implementatie van het R Server-cluster. Dit verbetert de snelheid en betrouwbaarheid van de inrichting. Prijsmodel voor R Server wordt bijgewerkt.
* R Server-cluster type prijs is nu gebaseerd op Standard-laag bedrag en R Server-toeslag prijs. Deze wijziging heeft geen invloed op de effectieve prijzen van R Server. verandert alleen hoe de kosten worden weergegeven in de factuur. Alle bestaande clusters voor R Server blijven gewoon werken en Resource Manager-sjablonen blijven functioneren tot kennisgeving over afschaffing. **Het verdient aanbeveling tot bijwerken van uw script implementaties voor het gebruik van nieuwe Resource Manager-sjabloon.**






