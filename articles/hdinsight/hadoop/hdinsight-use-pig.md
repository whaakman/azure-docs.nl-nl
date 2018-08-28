---
title: Pig met Hadoop in HDInsight gebruiken
description: Leer hoe u Pig gebruiken met Hadoop op HDInsight.
services: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.openlocfilehash: e97763adbe7998ed93e3ba8b87d89ffe8d8de6aa
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43045442"
---
# <a name="use-pig-with-hadoop-on-hdinsight"></a>Pig gebruiken met Hadoop op HDInsight

Meer informatie over het gebruik van [Apache Pig](http://pig.apache.org/) met HDInsight.

Pig is een platform voor het maken van programma's voor Hadoop met behulp van een procedurele taal wel *Pig Latin*. Pig is een alternatief voor Java voor het maken van *MapReduce* oplossingen, en deze is opgenomen in Azure HDInsight. Gebruik de volgende tabel voor het detecteren van de verschillende manieren waarop u Pig kan worden gebruikt met HDInsight:

| **Gebruik deze** als u wilt dat... | ...an **interactieve** shell | ...**batch** verwerken | ...door dit **cluster-besturingssysteem** | ...from dit **clientbesturingssysteem** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](apache-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X of Windows |
| [REST API](apache-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux of Windows |Linux, Unix, Mac OS X of Windows |
| [.NET-SDK voor Hadoop](apache-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux of Windows |Windows (voorlopig) |
| [Windows PowerShell](apache-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux of Windows |Windows |

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a id="why"></a>Waarom Pig gebruiken

Een van de uitdagingen bij het verwerken van gegevens met behulp van MapReduce in Hadoop is de verwerkingslogica implementeren met behulp van alleen een kaart en een functie verminderen. Voor de verwerking van complexe, hebt u vaak verwerking opsplitsen in meerdere MapReduce-bewerkingen die zijn gekoppeld samen aan het gewenste resultaat te bereiken.

Pig kunt u verwerking definiëren als een reeks transformaties die de gegevens via de gewenste uitvoer produceren.

De Pig Latin-taal kunt u beschrijven de gegevensstroom van onbewerkte invoer, via een of meer transformaties, om de gewenste uitvoer te produceren. Pig Latin-programma's volgen deze algemene patroon:

* **Load**: gegevens van het bestandssysteem worden gemanipuleerd lezen

* **Transformeren**: de gegevens bewerken

* **Dump of op te slaan**: uitvoergegevens naar het scherm of op te slaan voor verwerking

### <a name="user-defined-functions"></a>De gebruiker gedefinieerde functies

Pig Latin biedt ook ondersteuning voor gebruiker gedefinieerde functies (UDF's), zodat u kunt het aanroepen van externe onderdelen die logica die moeilijk te model in Pig Latin implementeren.

Zie voor meer informatie over Pig Latin [Pig Latin naslaginformatie over handmatige 1](http://archive.cloudera.com/cdh/3/pig/piglatin_ref1.html) en [Pig Latin naslaginformatie over handmatige 2](http://archive.cloudera.com/cdh/3/pig/piglatin_ref2.html).

Zie de volgende documenten voor een voorbeeld van het gebruik van UDF's met Pig:

* [DataFu gebruiken met Pig in HDInsight](apache-hadoop-use-pig-datafu-udf.md) -DataFu is een verzameling van nuttige UDF onderhouden door Apache
* [Python gebruiken met Pig en Hive in HDInsight](python-udf-hdinsight.md)
* [Gebruik C# met Hive en Pig in HDInsight](apache-hadoop-hive-pig-udf-dotnet-csharp.md)

## <a id="data"></a>Voorbeeldgegevens

HDInsight biedt verschillende voorbeeld gegevenssets die zijn opgeslagen in de `/example/data` en `/HdiSamples` mappen. Deze mappen zijn in de standaardopslag voor uw cluster. Het voorbeeld Pig in dit document wordt de *log4j* -bestand uit `/example/data/sample.log`.

Elk logboek in het bestand bestaat uit een line-of velden met een `[LOG LEVEL]` om weer te geven van het type en de ernst, bijvoorbeeld:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

In het vorige voorbeeld is het Logniveau fout.

> [!NOTE]
> U kunt ook een log4j-bestand genereren met behulp van de [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) hulpprogramma voor registratie en uploadt dit bestand vervolgens naar uw blob. Zie [gegevens uploaden naar HDInsight](../hdinsight-upload-data.md) voor instructies. Zie voor meer informatie over hoe in Azure storage-blobs worden gebruikt met HDInsight, [Azure Blob Storage gebruiken met HDInsight](../hdinsight-hadoop-use-blob-storage.md).

## <a id="job"></a>Voorbeeld van de taak

De volgende Pig Latin-taak laadt de `sample.log` bestand van de standaardopslag voor uw HDInsight-cluster. Vervolgens wordt een reeks transformaties die resulteren in een telling van het aantal keren dat elk logboek-niveau is opgetreden in de ingevoerde gegevens uitgevoerd. De resultaten worden geschreven naar de STDOUT.

    LOGS = LOAD 'wasb:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

De volgende afbeelding toont een overzicht van wat elke transformatie tot de gegevens heeft.

![Grafische weergave van de transformaties][image-hdi-pig-data-transformation]

## <a id="run"></a>De Pig Latin-taak uitvoeren

HDInsight uitvoeren Pig Latin-taken met behulp van een aantal methoden. Gebruik de volgende tabel om te bepalen welke methode is geschikt voor u, en vervolgens de koppeling voor een overzicht.

| **Gebruik deze** als u wilt dat... | ...an **interactieve** shell | ...**batch** verwerken | ...door dit **cluster-besturingssysteem** | ...from dit **client** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](apache-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X of Windows |
| [CURL](apache-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux of Windows |Linux, Unix, Mac OS X of Windows |
| [.NET-SDK voor Hadoop](apache-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux of Windows |Windows (voorlopig) |
| [Windows PowerShell](apache-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux of Windows |Windows |

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="pig-and-sql-server-integration-services"></a>Pig en SQL Server integratieservices

U kunt SQL Server Integration Services (SSIS) gebruiken een Pig-taak uit te voeren. Het Azure Feature Pack voor SSIS biedt de volgende onderdelen die met Pig-taken in HDInsight werken.

* [Azure HDInsight Pig-taak][pigtask]

* [Azure-abonnement Connection Manager][connectionmanager]

Meer informatie over het Azure Feature Pack voor SSIS [hier][ssispack].

## <a id="nextsteps"></a>Volgende stappen
Nu hebt u geleerd hoe u Pig gebruiken met HDInsight, gebruiken de volgende koppelingen op andere manieren om te werken met Azure HDInsight.

* [Gegevens uploaden naar HDInsight](../hdinsight-upload-data.md)
* [Hive gebruiken met HDInsight][hdinsight-use-hive]
* [Sqoop gebruiken met HDInsight](hdinsight-use-sqoop.md)
* [Oozie gebruiken met HDInsight](../hdinsight-use-oozie.md)
* [MapReduce-taken gebruiken met HDInsight][hdinsight-use-mapreduce]

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]:../hdinsight-use-hive.md
[hdinsight-use-mapreduce]:../hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx


[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
