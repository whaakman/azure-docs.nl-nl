---
title: Release-opmerkingen voor Hadoop-onderdelen op Azure HDInsight | Microsoft Docs
description: Meest recente release-opmerkingen en versies van Hadoop-onderdelen voor Azure HDInsight. Ontwikkeling tips en informatie opvragen voor Spark, R Server, Hive en meer.
services: hdinsight
documentationcenter: 
editor: cgronlun
manager: jhubbard
author: nitinme
tags: azure-portal
ms.assetid: a363e5f6-dd75-476a-87fa-46beb480c1fe
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: nitinme
ms.openlocfilehash: e0977417ec8678db54d91677b1f9bdc709e196b5
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="release-notes-for-hadoop-components-on-azure-hdinsight"></a>Releaseopmerkingen voor Hadoop-onderdelen op Azure HDInsight

In dit artikel bevat informatie over de **meest recente** Azure HDInsight LDR-updates. Zie voor informatie over eerdere versies, [HDInsight Release-opmerkingen bij de archivering](hdinsight-release-notes-archive.md).

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie voor meer informatie [HDInsight versioning artikel](hdinsight-component-versioning.md).


## <a name="notes-for-08012017-release-of-hdinsight"></a>Opmerkingen bij de release 01/08/2017 van HDInsight

| Titel | Beschrijving | Betrokken gebied  | Clustertype  | 
| --- | --- | --- | --- | --- |
| Versie van Microsoft R Server 9.1 in HDInsight |HDInsight biedt nu ondersteuning voor inrichting R Server 9.1 clusters in HDInsight. Zie voor meer informatie over Microsoft R Server 9.1 release [deze blog](https://blogs.technet.microsoft.com/dataplatforminsider/2017/04/19/introducing-microsoft-r-server-9-1-release/). |Service |R Server |
| HDInsight 3.6 bevat nu nieuwere versies van de Hadoop-stack|<ul><li>Zie voor een gedetailleerde lijst met bijgewerkte versies, [Hadoop component versies beschikbaar zijn in HDInsight](hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions).</li><li>Zie voor een lijst van fouten die in de nieuwste versies van de Hadoop-stack is verholpen, [informatie over de Apache-beveiligingspatch](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/patch_parent.html).</li><li>Zie voor een lijst met wijzigingen tussen HDP 2.6.1 (dit is nu beschikbaar in HDInsight 3.6) op te splitsen, [https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/behavior_changes.html](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/behavior_changes.html).</li><li>Zie voor een lijst met bekende problemen in HDP 2.6.1 [bekende problemen](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/known_issues.html).</li></ul> |Service |Alle |N.v.t. |
| Updates voor clusters interactieve Hive (Preview) |<ul><li><b>Verbetering van de functie.</b> Implementatie van cache metastore en wordt de belasting van de back-end SQL door de metagegevens van de cache te plaatsen en verbetert de prestaties voor alle bewerkingen voor metagegevens.  Dankzij deze verbetering is nu een standaard op alle clusters interactieve Hive. Zie voor meer informatie [https://issues.apache.org/jira/browse/HIVE-16520](https://issues.apache.org/jira/browse/HIVE-16520).</li><li><b>Verbetering van de functie.</b> Laden van de dynamische partitie is geoptimaliseerd. Zie [https://issues.apache.org/jira/browse/HIVE-14204] (https://issues.apache.org/jira/browse/HIVE-14204) voor meer informatie.</li><li><b>Verbetering van de functie.</b> De optimalisatie van de configuratie voor HDInsight op Linux.</li><li><b>Fout te herstellen.</b> `CredentialProviderFactory$getProviders`is geen thread-safe. Dit probleem is nu opgelost. Zie voor meer informatie [https://issues.apache.org/jira/browse/HADOOP-14195](https://issues.apache.org/jira/browse/HADOOP-14195).</li><li><b>Fout te herstellen.</b> Hoog CPU-gebruik met WASB stuurprogramma `liststatus` leiden tot slechte prestaties van ATS API. Dit probleem is nu opgelost. Zie voor meer informatie [https://github.com/Azure/azure-storage-java/pull/154](https://github.com/Azure/azure-storage-java/pull/154).</li></ul> |Service |Interactieve Hive (Preview) |
| Updates voor Hadoop-clusters |Templeton taak bewerking betrouwbaarheid is verbeterd. Zie voor meer informatie [https://issues.apache.org/jira/browse/HIVE-15947](https://issues.apache.org/jira/browse/HIVE-15947) |Service |Hadoop |
| YARN-updates | HDInsight maakt nu een 250 GB Ambari database (zonder kosten verhogen), wat tot een betere ervaring voor klanten leidt. Deze wijziging te voorkomen dat ATS ophalen gevuld en hebt waarschijnlijk een betere prestaties. |Service |Alle |
| Spark-updates | De release van Spark 2.1.1. Zie voor meer informatie [Spark Release 2.1.1](https://spark.apache.org/releases/spark-release-2-1-1.html). | Service | Spark |

  



## <a name="04062017---general-availability-of-hdinsight-36"></a>06-04-2017 - algemene beschikbaarheid van HDInsight 3.6

* Met deze release voegt Azure HDInsight versie 3.6, die is gebaseerd op HDP 2.6. Opmerkingen bij de release van HDP 2.6 beschikbaar zijn [hier](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html) en meer informatie over de versies van HDInsight kan worden gevonden [hier](hdinsight-component-versioning.md). HDInsight 3.6 is beschikbaar voor de volgende werkbelastingen:

    * Hadoop v2.7.3
    * HBase v1.1.2
    * Storm-v1.1.0
    * Spark v2.1.0
    * Interactieve Hive v2.1.0

* **Ondersteuning voor Hive-weergave 2.0**. Dit moet de gebruikerservaring voor interactieve Hive verbeteren. Zie voor meer informatie [Hortonworks documentatie](http://docs.hortonworks.com/HDPDocuments/Ambari-2.5.0.3/bk_ambari-views/content/ch_using_hive_view.html).

* **Prestatieverbeteringen met Hive LLAP**. Zie voor meer informatie [Hortonworks documentatie](https://hortonworks.com/blog/top-5-performance-boosters-with-apache-hive-llap/).

* **Nieuwe functies in Hive**. Zie [Hortonworks documentatie](https://hortonworks.com/apache/hive/#section_4).

* **Afschaffing van de CLI hive**: Hive CLI wordt afgeschaft en klanten wordt aangeraden gebruik in plaats daarvan Beeline. Zie voor meer informatie [Apache-documentatie](https://cwiki.apache.org/confluence/display/Hive/Replacing+the+Implementation+of+Hive+CLI+Using+Beeline). Zie voor instructies over het gebruik van Beeline met HDInsight [Beeline gebruiken met HDInsight Hadoop-clusters](hadoop/apache-hadoop-use-hive-beeline.md).

* **Nieuwe functies in Apache Phoenix en HBase**.
    * Ondersteuning voor het quotum van opslag: meestal gebruikt in omgevingen met meerdere tenants, waardoor de beperkte opslagruimte van een per tabel en per niveau van de naamruimte.
    * Phoenix verbeteringen indexeren: incrementele index is gemaakt en opnieuw maken/hervatten indexeren van eerdere fouten.
    * Hulpprogramma voor de integriteit van Phoenix data: biedt ondersteuning voor validatie van het schema, index en andere metagegevens.


* **Probleem met HBase**: tijdens het uitvoeren van een CSV-bulksgewijs upload MapReduce-taak, de volgende syntaxis kan leiden tot een fout opgetreden.

        HADOOP_CLASSPATH=$(hbase mapredcp):/path/to/hbase/conf hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table EXAMPLE --input /data/example.csv

    Gebruik in plaats daarvan de volgende syntaxis:

        HADOOP_CLASSPATH=/path/to/hbase-protocol.jar:/path/to/hbase/conf hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table EXAMPLE --input /data/example.csv


## <a name="02282017---release-of-spark-21-on-hdinsight-36-preview"></a>28-02-2017 - release van Spark 2.1 in HDInsight 3.6 (Preview)
* [Spark 2.1](http://spark.apache.org/releases/spark-release-2-1-0.html) verbetert veel stabiliteit en bruikbaarheid van problemen met eerdere versies. Dit brengt ook nieuwe functies in alle Spark-werkbelastingen, zoals Spark Core, SQL, ML en Streaming.
* Gestructureerde Streaming verbeterde schaalbaarheid met ondersteuning voor gebeurtenis tijd watermerken en Kafka 0.10 connector opgehaald.
* Spark SQL partitioneren wordt nu verwerkt met nieuwe schaalbare afhandeling van partitie mechanisme. Meer informatie [hier](http://spark.apache.org/releases/spark-release-2-1-0.html) voor het upgraden.
* 2.1 Spark op Azure HDInsight 3.6 Preview momenteel biedt geen ondersteuning voor hulpprogramma BI verbinding met ODBC-stuurprogramma.
* Toegang tot Azure Data Lake Store uit 2.1 Spark-clusters wordt niet ondersteund in deze Preview.


## <a name="11182016---release-of-spark-201-on-hdinsight-35"></a>18-11-2016 - release van 2.0.1 Spark in HDInsight 3.5
Spark 2.0.1 is nu beschikbaar op Spark-clusters (HDInsight versie 3.5).

## <a name="11162016---release-of-r-server-90-on-hdinsight-35-spark-20"></a>16-11-2016 - release van op HDInsight 3.5 R Server 9.0 (Spark 2.0)
*   R Server clusters omvatten nu de optie voor twee versies: R Server 9.0 op HDI 3.5 (Spark 2.0) en R Server 8.0 op HDI 3.4 (Spark 1.6).
*   R Server 9.0 op HDI 3.5 (Spark 2.0) is gebaseerd op R 3.3.2 en bevat nieuwe ScaleR data source-functies RxHiveData en RxParquetData door ScaleR aangeroepen voor het laden van gegevens van Hive en parketvloeren rechtstreeks naar Spark DataFrames voor analyse. Zie voor meer informatie de inline Help-informatie over deze functies in R door het gebruik van de **? RxHiveData** en **? RxParquetData** opdrachten.
*   RStudio Server community edition wordt nu standaard (met de optie opt-out) geïnstalleerd op de blade clusterconfiguratie als onderdeel van de inrichting stroom.

## <a name="11092016---release-of-spark-20-on-hdinsight"></a>09-11-2016 - release van 2.0 Spark in HDInsight
* 2.0 Spark-clusters op HDInsight 3.5 ondersteunen nu Livy en Jupyter-services.

## <a name="10262016---release-of-r-server-on-hdinsight"></a>26-10-2016 - release van op HDInsight R Server
* De URI voor de edge-knooppunt toegang is gewijzigd in **clustername**-ed-ssh.azurehdinsight.net
* R Server over het inrichten van HDInsight-cluster is gestroomlijnd.
* Op HDInsight R Server is nu beschikbaar als reguliere HDInsight 'R Server' type cluster en niet meer als een afzonderlijke HDInsight-toepassing is geïnstalleerd. De edge-knooppunt en R Server binaire bestanden worden nu ingericht als onderdeel van de implementatie van het cluster R Server. Dit verbetert de snelheid en betrouwbaarheid van de inrichting. Prijsmodel voor R Server wordt dienovereenkomstig bijgewerkt.
* R Server type clusterprijs is nu gebaseerd op standaardcategorie prijzen plus R Server aanvulling prijs. Deze wijziging heeft geen invloed op de effectieve prijzen van R Server; alleen hoe de kosten worden weergegeven in de factuur worden gewijzigd. Alle bestaande R Server-clusters blijven werken en Resource Manager-sjablonen blijven werken totdat afschaffing aankondiging. **Het verdient aanbeveling tot bijwerken van uw script implementaties als nieuwe Resource Manager-sjabloon wilt gebruiken.**






