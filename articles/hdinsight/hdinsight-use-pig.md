---
title: Hadoop Pig gebruiken in HDInsight | Microsoft Docs
description: Informatie over het Pig gebruiken met Hadoop op HDInsight.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: acfeb52b-4b81-4a7d-af77-3e9908407404
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: 474f901ffdaf1ed372ace19076ef723b8b10cb9a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="use-pig-with-hadoop-on-hdinsight"></a>Pig gebruiken met Hadoop in HDInsight

Informatie over het gebruik [Apache Pig](http://pig.apache.org/) met HDInsight...

Pig is een platform voor het maken van programma's voor Hadoop met behulp van een procedurele taal bekend als *Pig Latin*. Pig vormt een alternatief voor Java voor het maken van *MapReduce* oplossingen en deze is opgenomen in Azure HDInsight. Gebruik de volgende tabel voor het detecteren van de verschillende manieren waarop varkens kan worden gebruikt met HDInsight:

| **Gebruik deze** als u wilt dat... | .. .an **interactieve** shell | ... **batch** verwerken | .. .door dit **cluster-besturingssysteem** | .. .from dit **clientbesturingssysteem** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](hdinsight-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X of Windows |
| [REST API](hdinsight-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux- of Windows |Linux, Unix, Mac OS X of Windows |
| [.NET-SDK voor Hadoop](hdinsight-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux- of Windows |Windows (voor nu) |
| [Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux- of Windows |Windows |
| [Extern bureaublad](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 en 3.3) |✔ |✔ |Windows |Windows |

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a id="why"></a>Waarom Pig gebruiken

Een van de uitdagingen bij het verwerken van gegevens met behulp van MapReduce in Hadoop is uw verwerking logica implementeren met behulp van alleen een kaart en een functie verminderen. Voor complexe verwerking, hebt u vaak verwerking opdelen in meerdere MapReduce-bewerkingen die zijn gekoppeld samen om te bewerkstelligen het gewenste resultaat oplevert.

Pig kunt u verwerking definiëren als een reeks transformaties dat de gegevens via loopt om de gewenste uitvoer te produceren.

De taal Pig Latin kunt u beschrijven de gegevensstroom van onbewerkte invoer, via een of meer transformaties, om de gewenste uitvoer te produceren. Pig Latin-programma's volgen deze algemene patroon:

* **Load**: gegevens van het bestandssysteem worden gemanipuleerd lezen

* **Transformeren**: de gegevens bewerken

* **Dump of opgeslagen**: uitvoergegevens naar het scherm of opgeslagen voor verwerking

### <a name="user-defined-functions"></a>Gebruiker gedefinieerde functies

Pig Latin ondersteunt ook gebruiker gedefinieerde functies (UDF), zodat u kunt het aanroepen van externe onderdelen die logica die is moeilijk om Pig Latin-model te implementeren.

Zie voor meer informatie over Pig Latin [Pig Latin verwijzing handmatige 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) en [Pig Latin verwijzing handmatige 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).

Zie de volgende documenten voor een voorbeeld van het gebruik van UDF's met Pig:

* [Gebruik DataFu met Pig in HDInsight](hdinsight-hadoop-use-pig-datafu-udf.md) -DataFu is een verzameling nuttig UDF's die worden beheerd door de Apache
* [Gebruik van Python met Pig en Hive in HDInsight](hdinsight-python.md)
* [Gebruik C# met Hive en Pig in HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

## <a id="data"></a>Voorbeeldgegevens

HDInsight biedt verschillende voorbeeld gegevenssets die zijn opgeslagen in de `/example/data` en `/HdiSamples` mappen. Deze mappen zich in de standaard-opslag voor uw cluster. In het voorbeeld Pig in dit document wordt de *log4j* bestand van `/example/data/sample.log`.

Elk logboek in het bestand bestaat uit een line-of velden met een `[LOG LEVEL]` om weer te geven van het type en de ernst, bijvoorbeeld:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

In het vorige voorbeeld is het Logniveau fout.

> [!NOTE]
> U kunt ook een log4j-bestand genereren met behulp van de [Apache-Log4j](http://en.wikipedia.org/wiki/Log4j) hulpprogramma logboekregistratie en uploadt u dit bestand vervolgens naar de blob. Zie [gegevens uploaden naar HDInsight](hdinsight-upload-data.md) voor instructies. Zie voor meer informatie over het gebruik van blobs in Azure storage met HDInsight [Azure Blob Storage gebruiken met HDInsight](hdinsight-hadoop-use-blob-storage.md).

## <a id="job"></a>Voorbeeld van de taak

De volgende taak voor Pig Latin laadt de `sample.log` -bestand van de standaard-opslag voor uw HDInsight-cluster. Vervolgens voert een reeks transformaties die resulteren in een telling van het aantal keren dat elke logboekniveau is opgetreden in de invoergegevens uit. De resultaten worden geschreven naar STDOUT.

    LOGS = LOAD 'wasb:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

De volgende afbeelding toont een overzicht van de werking van elke transformatie op de gegevens.

![Grafische weergave van de transformaties][image-hdi-pig-data-transformation]

## <a id="run"></a>Voer de taak Pig Latin

HDInsight kunt Pig Latin taken uitvoeren met behulp van een aantal methoden. Gebruik de volgende tabel om te bepalen welke methode is geschikt voor u en volg de koppeling voor een overzicht.

| **Gebruik deze** als u wilt dat... | .. .an **interactieve** shell | ... **batch** verwerken | .. .door dit **cluster-besturingssysteem** | .. .from dit **client** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](hdinsight-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X of Windows |
| [CURL](hdinsight-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux- of Windows |Linux, Unix, Mac OS X of Windows |
| [.NET-SDK voor Hadoop](hdinsight-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux- of Windows |Windows (voor nu) |
| [Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux- of Windows |Windows |
| [Extern bureaublad](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 en 3.3) |✔ |✔ |Windows |Windows |

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="pig-and-sql-server-integration-services"></a>Pig- en SQL Server integratieservices

SQL Server Integration Services (SSIS) kunt u een Pig-taak uitvoert. Het Azure-functiepakket voor SSIS biedt de volgende onderdelen die met Pig-taken in HDInsight werken.

* [Azure HDInsight Pig-taak][pigtask]

* [Azure abonnement Verbindingsbeheer][connectionmanager]

Meer informatie over het Azure Feature Pack voor SSIS [hier][ssispack].

## <a id="nextsteps"></a>Volgende stappen
U hebt geleerd hoe u Pig gebruiken met HDInsight, gebruik de volgende koppelingen om te verkennen andere manieren om te werken met Azure HDInsight.

* [Gegevens uploaden naar HDInsight][hdinsight-upload-data]
* [Hive gebruiken met HDInsight][hdinsight-use-hive]
* [Sqoop gebruiken met HDInsight](hdinsight-use-sqoop.md)
* [Oozie gebruiken met HDInsight](hdinsight-use-oozie.md)
* [MapReduce-taken gebruiken met HDInsight][hdinsight-use-mapreduce]

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx


[hdinsight-upload-data]: hdinsight-upload-data.md

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx


[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
