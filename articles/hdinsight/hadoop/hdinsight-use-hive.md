---
title: Wat is Apache Hive en HiveQL - Azure HDInsight | Microsoft Docs
description: Apache Hive is een systeem voor het datawarehouse van gegevens voor Hadoop. U kunt de gegevens die zijn opgeslagen in met behulp van HiveQL, Hive query die vergelijkbaar is met Transact-SQL. In dit document wordt beschreven hoe Hive en HiveQL gebruiken met Azure HDInsight.
keywords: hiveql, wat hive, hadoop hiveql, is het gebruik van hive, informatie over hive, wat is er hive
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2c10f989-7636-41bf-b7f7-c4b67ec0814f
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2018
ms.author: larryfr
ms.openlocfilehash: ecf08b765ba17ac410f45bc3604a2aa0f3b4823e
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="what-is-apache-hive-and-hiveql-on-azure-hdinsight"></a>Wat is Apache Hive en HiveQL in Azure HDInsight?

[Apache Hive](http://hive.apache.org/) is een systeem voor het datawarehouse van gegevens voor Hadoop. Hive kunt gegevenssamenvatting, query's en analyse van gegevens. Hive-query's zijn geschreven in HiveQL, namelijk een querytaal die vergelijkbaar is met SQL.

Hive kunt u de projectstructuur van het op grotendeels ongestructureerde gegevens. Nadat u de structuur gedefinieerd, kunt u HiveQL query uitvoeren op de gegevens zonder kennis van Java of MapReduce.

HDInsight biedt verschillende clustertypen die zijn afgestemd op specifieke werkbelasting in gedachten. De volgende clustertypen worden meestal gebruikt voor Hive-query's:

* __Interactieve Query__: een Hadoop-cluster waarmee [lage latentie analytische verwerking (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) functionaliteit voor de reactietijd voor interactieve query's verbeteren. Zie voor meer informatie de [starten met interactieve Query in HDInsight](../interactive-query/apache-interactive-query-get-started.md) document.

* __Hadoop__: een Hadoop-cluster dat is afgestemd op batchverwerking werkbelastingen. Zie voor meer informatie de [starten met Hadoop in HDInsight](../hadoop/apache-hadoop-linux-tutorial-get-started.md) document.

* __Spark__: Apache Spark zijn ingebouwde functionaliteit voor het werken met Hive. Zie voor meer informatie de [beginnen met Spark in HDInsight](../spark/apache-spark-jupyter-spark-sql.md) document.

* __HBase__: HiveQL kan worden gebruikt voor opgeslagen querygegevens in HBase. Zie voor meer informatie de [beginnen met HBase in HDInsight](../hbase/apache-hbase-tutorial-get-started-linux.md) document.

## <a name="how-to-use-hive"></a>Het gebruik van Hive

Gebruik de volgende tabel voor het detecteren van het gebruik van Hive met HDInsight:

| **Gebruik deze methode** als u wilt dat... | ...an **interactieve** shell | ...**batch** verwerken | ...door dit **cluster-besturingssysteem** | ...from dit **clientbesturingssysteem** |
|:--- |:---:|:---:|:--- |:--- |
| [Hive-weergave](../hadoop/apache-hadoop-use-hive-ambari-view.md) |✔ |✔ |Linux |(Browser gebaseerd) |
| [Beeline client](../hadoop/apache-hadoop-use-hive-beeline.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X of Windows |
| [REST API](../hadoop/apache-hadoop-use-hive-curl.md) |&nbsp; |✔ |Linux- of Windows * |Linux, Unix, Mac OS X of Windows |
| [HDInsight tools voor Visual Studio](../hadoop/apache-hadoop-use-hive-visual-studio.md) |&nbsp; |✔ |Linux- of Windows * |Windows |
| [Windows PowerShell](../hadoop/apache-hadoop-use-hive-powershell.md) |&nbsp; |✔ |Linux- of Windows * |Windows |

> [!IMPORTANT]
> \*Linux is het enige besturingssysteem gebruikt op HDInsight versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.
>
> Als u een cluster met HDInsight op basis van Windows gebruikt, kunt u de [Query console](../hadoop/apache-hadoop-use-hive-query-console.md) vanuit de browser of [extern bureaublad](../hadoop/apache-hadoop-use-hive-remote-desktop.md) Hive-query's uitvoeren.

## <a name="hiveql-language-reference"></a>HiveQL-taalreferentie

Taalreferentie HiveQL is beschikbaar in de [taal handmatig (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).

## <a name="hive-and-data-structure"></a>Structuur van hive en gegevens

Hive begrijpt het werken met gestructureerde en semi-gestructureerde gegevens. Bijvoorbeeld: tekstbestanden waarin de velden worden gescheiden door specifieke tekens. De volgende instructie van HiveQL maakt een tabel door spaties gescheiden gegevens:

```hiveql
CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
```

Hive biedt ook ondersteuning voor aangepaste **serialisatiefunctie/deserializers (serde-schrijfbewerking)** voor complexe of onregelmatig gestructureerde gegevens. Zie voor meer informatie de [een aangepaste JSON serde-schrijfbewerking gebruiken met HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx) document.

Zie voor meer informatie over ondersteunde bestandsindelingen in Hive, de [taal handmatig (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)

## <a name="hive-internal-tables-vs-external-tables"></a>Interne tabellen tegenover externe tabellen hive

Er zijn twee soorten tabellen die u met Hive maken kunt:

* __Interne__: gegevens worden opgeslagen in het datawarehouse Hive. Het datawarehouse bevindt zich op `/hive/warehouse/` op de standaard-opslag voor het cluster.

    Gebruik interne tabellen wanneer:

    * Gegevens is tijdelijk.
    * Wilt u Hive voor het beheren van de levenscyclus van de tabel en de gegevens.

* __Externe__: gegevens worden opgeslagen buiten het datawarehouse. De gegevens kunnen worden opgeslagen op opslag die toegankelijk is door het cluster.

    Gebruik externe tabellen wanneer:

    * De gegevens worden ook gebruikt buiten Hive. Bijvoorbeeld, worden de gegevensbestanden bijgewerkt door een ander proces (die wordt niet vergrendeld de bestanden.)
    * Gegevens moeten blijven in de onderliggende locatie, zelfs na het verwijderen van de tabel.
    * U moet een aangepaste locatie, zoals een niet-standaard opslagaccount.
    * Een ander programma dan hive beheert de indeling van gegevens, locatie, enzovoort.

Zie voor meer informatie de [Hive interne en externe tabellen Intro] [ cindygross-hive-tables] blogbericht.

## <a name="user-defined-functions-udf"></a>Gebruiker gedefinieerde functies (UDF)

Hive kan verder worden uitgebreid via **gebruiker gedefinieerde functies (UDF)**. Een UDF, kunt u functionaliteit of logica die eenvoudig niet is gemodelleerd in HiveQL implementeren. Zie de volgende documenten voor een voorbeeld van het gebruik van UDF's met Hive:

* [Een Java door de gebruiker gedefinieerde functie gebruiken met Hive](../hadoop/apache-hadoop-hive-java-udf.md)

* [Een Python door de gebruiker gedefinieerde functie gebruiken met Hive en Pig](../hadoop/python-udf-hdinsight.md)

* [Een C# gebruiker gedefinieerde functie gebruiken met Hive en Pig](../hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Het toevoegen van een aangepaste Hive gebruiker gedefinieerde functie aan HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Een voorbeeld van de gebruiker gedefinieerde functie datum/tijd-indeling converteren naar Hive tijdstempel Hive](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a id="data"></a>Voorbeeldgegevens

Hive in HDInsight wordt vooraf geladen geleverd met een interne tabel met de naam `hivesampletable`. HDInsight biedt ook een voorbeeld van gegevenssets die kunnen worden gebruikt met Hive. Deze gegevenssets worden opgeslagen in de `/example/data` en `/HdiSamples` mappen. Deze mappen aanwezig zijn in de standaard-opslag voor uw cluster.

## <a id="job"></a>Voorbeeld van de Hive-query

De volgende HiveQL-instructies project kolommen naar de `/example/data/sample.log` bestand:

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

In het vorige voorbeeld uitvoeren de HiveQL-instructies de volgende acties:

* `set hive.execution.engine=tez;`: Hiermee stelt u de engine voor het uitvoeren Tez gebruiken. Tez gebruik in plaats van MapReduce krijgt u een toename van de prestaties van query's. Zie voor meer informatie over Tez de [Apache Tez gebruiken voor betere prestaties](#usetez) sectie.

    > [!NOTE]
    > Deze instructie is alleen vereist wanneer u een HDInsight op basis van Windows-cluster. Tez is de engine voor het uitvoeren van standaard voor HDInsight op basis van Linux.

* `DROP TABLE`: Als de tabel al bestaat, verwijderen.

* `CREATE EXTERNAL TABLE`: Hiermee maakt u een nieuw **externe** tabel in Hive. De tabeldefinitie opslaan externe tabellen alleen in Hive. De gegevens blijft in de oorspronkelijke locatie en in de oorspronkelijke indeling.

* `ROW FORMAT`: Hiermee geeft u Hive hoe de gegevens wordt opgemaakt. In dit geval worden de velden in elk logboek gescheiden door een spatie.

* `STORED AS TEXTFILE LOCATION`: Vertelt Hive waar de gegevens worden opgeslagen (de `example/data` directory) en dat deze is opgeslagen als tekst. De gegevens worden in één bestand of worden verdeeld over meerdere bestanden in de map.

* `SELECT`: Hiermee selecteert u een telling van alle rijen waarin de kolom **t4** bevat de waarde **[fout]**. Deze instructie retourneert een waarde van **3** omdat er drie rijen met deze waarde.

* `INPUT__FILE__NAME LIKE '%.log'`-Hive probeert het schema toepassen op alle bestanden in de map. In dit geval wordt bevat de map bestanden die niet overeenkomen met het schema. Om te voorkomen dat een garbagecollection-gegevens in de resultaten, deze instructie vertelt Hive dat we alleen gegevens uit bestanden eindigt op moet retourneren. log.

> [!NOTE]
> Externe tabellen moeten worden gebruikt wanneer u de onderliggende gegevens wordt bijgewerkt door een externe bron verwacht. Bijvoorbeeld, een geautomatiseerd proces of MapReduce-bewerking uploaden.
>
> Verwijderen van een externe tabel komt **niet** gegevens verwijderd, wordt de definitie van de tabel alleen verwijderd.

Maken van een **interne** in plaats van de externe tabel, gebruikt u de volgende HiveQL:

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

Deze instructies uitvoeren de volgende acties:

* `CREATE TABLE IF NOT EXISTS`: Als de tabel niet bestaat, maakt u dit. Omdat de **externe** trefwoord wordt niet gebruikt, wordt deze instructie maakt u een interne tabel. De tabel wordt opgeslagen in het datawarehouse Hive en volledig wordt beheerd door Hive.

* `STORED AS ORC`: De gegevens opslaat in geoptimaliseerd rij kolommen (ORC)-indeling. ORC is een maximaal geoptimaliseerd en efficiënte indeling voor het opslaan van gegevens met Hive.

* `INSERT OVERWRITE ... SELECT`: Selecteert rijen uit de **log4jLogs** tabel met **[fout]**, en vervolgens voegt u de gegevens in de **foutenlogboeken** tabel.

> [!NOTE]
> In tegenstelling tot externe tabellen verwijdert voor het verwijderen van een interne tabel tevens de onderliggende gegevens.

## <a name="improve-hive-query-performance"></a>Hive-query's sneller

### <a id="usetez"></a>Apache Tez

[Apache Tez](http://tez.apache.org) ligt een framework dat kunt van gegevensintensieve toepassingen, zoals Hive, om u te veel efficiënter op schaal worden uitgevoerd. Tez is standaard ingeschakeld voor Linux gebaseerde HDInsight-clusters.

> [!NOTE]
> Tez is momenteel uitgeschakeld voor Windows gebaseerde HDInsight-clusters standaard en moet zijn ingeschakeld. Om te profiteren van Tez, moet de volgende waarde worden ingesteld voor een Hive-query:
>
> `set hive.execution.engine=tez;`
>
> Tez is de standaard-engine voor Linux gebaseerde HDInsight-clusters.

De [Hive op Tez-Ontwerpdocumenten](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) bevat details over de implementatie keuzen en afstemmen configuraties.

Om u te helpen bij foutopsporing taken uitgevoerd met Tez, HDInsight biedt de volgende web UI's waarmee u details wilt weergeven van Tez-taken:

* [De weergave Ambari Tez op Linux gebaseerde HDInsight gebruiken](../hdinsight-debug-ambari-tez-view.md)

* [De Tez-gebruikersinterface op HDInsight op basis van Windows gebruiken](../hdinsight-debug-tez-ui.md)

### <a name="low-latency-analytical-processing-llap"></a>Lage latentie Analytical Processing (LLAP)

[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (soms ook wel lange Live en proces) is een nieuwe functie in Hive 2.0 waarmee de caching van query's in het geheugen. LLAP maakt Hive-query's sneller tot [26 x sneller dan Hive 1.x in sommige gevallen](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).

HDInsight biedt LLAP in het type van het cluster interactieve Query. Zie voor meer informatie de [beginnen met een interactieve Query](../interactive-query/apache-interactive-query-get-started.md) document.

## <a name="hive-jobs-and-sql-server-integration-services"></a>Hive-taken en SQL Server Integration Services

SQL Server Integration Services (SSIS) kunt u een Hive-taak uitvoert. Het Azure-functiepakket voor SSIS biedt de volgende onderdelen die met Hive-taken in HDInsight werken.

* [Azure HDInsight Hive-taak][hivetask]

* [Azure Subscription Connection Manager][connectionmanager]

Meer informatie over het Azure Feature Pack voor SSIS [hier][ssispack].

## <a id="nextsteps"></a>Volgende stappen

Nu u weet wat Hive is en het gebruik ervan met Hadoop in HDInsight, gebruiken de volgende koppelingen om te verkennen andere manieren om te werken met Azure HDInsight.

* [Gegevens uploaden naar HDInsight][hdinsight-upload-data]
* [Pig gebruiken met HDInsight][hdinsight-use-pig]
* [MapReduce-taken gebruiken met HDInsight][hdinsight-use-mapreduce]

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: ../hdinsight-upload-data.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
