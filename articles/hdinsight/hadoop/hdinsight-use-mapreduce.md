---
title: MapReduce met Hadoop op HDInsight | Microsoft Docs
description: Informatie over het uitvoeren van MapReduce-taken op Hadoop in HDInsight-clusters.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7f321501-d62c-4ffc-b5d6-102ecba6dd76
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/01/2017
ms.author: larryfr
ms.openlocfilehash: ad12dee2eb01f839db07985fcb0805bf961354cc
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a>MapReduce in Hadoop in HDInsight gebruiken

Informatie over het uitvoeren van MapReduce-taken op HDInsight-clusters. Gebruik de volgende tabel voor het detecteren van de verschillende manieren waarop MapReduce kan worden gebruikt met HDInsight:

| **Gebruik deze**... | **...om hiervoor** | ...door dit **cluster-besturingssysteem** | ...from dit **clientbesturingssysteem** |
|:--- |:--- |:--- |:--- |
| [SSH](apache-hadoop-use-mapreduce-ssh.md) |Gebruik de opdracht Hadoop via **SSH** |Linux |Linux, Unix, Mac OS X of Windows |
| [REST](apache-hadoop-use-mapreduce-curl.md) |Verzenden van de taak op afstand via **REST** (voorbeelden gebruiken cURL) |Linux- of Windows |Linux, Unix, Mac OS X of Windows |
| [Windows PowerShell](apache-hadoop-use-mapreduce-powershell.md) |Verzenden van de taak op afstand via **Windows PowerShell** |Linux- of Windows |Windows |
| [Extern bureaublad](apache-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 en 3.3) |Gebruik de opdracht Hadoop via **extern bureaublad** |Windows |Windows |

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a id="whatis"></a>Wat is MapReduce

Hadoop-MapReduce is een softwareframework voor het schrijven van taken die enorme hoeveelheden gegevens verwerken. Invoergegevens in onafhankelijke segmenten verdeeld. Elk segment worden parallel verwerkt op de knooppunten in het cluster. Een MapReduce-taak bestaat uit twee functies:

* **Toewijzer**: invoergegevens verbruikt, analyseert ze (meestal met filteren en sorteren operations) en verzendt tuples (sleutel / waarde-paren)

* **Reducer**: tuples die door de Mapper verbruikt en wordt een samenvatting uitgevoerd waarmee een kleinere, gecombineerde resultaat van de gegevens toewijzen maakt

Een voorbeeld van een eenvoudige word aantal MapReduce-taak wordt weergegeven in het volgende diagram:

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

De uitvoer van deze taak is een telling van het aantal keren dat elk woord is opgetreden in de tekst.

* De toewijzing wordt elke regel van de ingevoerde tekst als invoer en splitst deze in woorden. Het verzendt een sleutelwaarde paar telkens wanneer een woord van het woord plaatsvindt wordt gevolgd door een 1. De uitvoer is gesorteerd voordat deze naar reducer verzonden.
* De reducer deze afzonderlijke tellingen voor elk woord elkaar worden opgeteld en verzendt een één sleutel/waarde-paar die het woord gevolgd door de som van de instanties bevat.

MapReduce kan worden geïmplementeerd in diverse talen. Java is de meest voorkomende implementatie en wordt gebruikt voor demonstratiedoeleinden in dit document.

## <a name="development-languages"></a>Ontwikkelingstalen

Talen of frameworks die zijn gebaseerd op Java en de virtuele Java-Machine kan rechtstreeks als een MapReduce-taak worden uitgevoerd. Het voorbeeld gebruikt in dit document is een MapReduce Java-toepassing. Niet-Java-talen, zoals C#, Python of zelfstandige uitvoerbare bestanden, moeten gebruiken **Hadoop-streaming**.

Hadoop-streaming communiceert met de toewijzen en reducer via STDIN en STDOUT. De toewijzen en reducer gegevens van een regel op een tijdstip van STDIN lezen en schrijven van de uitvoer naar STDOUT. Elke regel lezen of verzonden door de toewijzen en reducer moet de indeling van een sleutel-waardepaar, gescheiden door een tab-teken zijn:

    [key]/t[value]

Zie voor meer informatie [Hadoop-Streaming](http://hadoop.apache.org/docs/r1.2.1/streaming.html).

Zie de volgende documenten voor voorbeelden van het gebruik van Hadoop-streaming met HDInsight:

* [C# MapReduce-taken ontwikkelen](apache-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [Python-MapReduce-taken ontwikkelen](apache-hadoop-streaming-python.md)

## <a id="data"></a>Voorbeeldgegevens

HDInsight biedt verschillende voorbeeld gegevenssets die zijn opgeslagen in de `/example/data` en `/HdiSamples` directory. Deze mappen zich in de standaard-opslag voor uw cluster. In dit document, gebruiken we de `/example/data/gutenberg/davinci.txt` bestand. Dit bestand bevat de laptops van Leonardo Da Vinci.

## <a id="job"></a>Voorbeeld MapReduce

Een voorbeeld MapReduce word-count-toepassing is opgenomen in uw HDInsight-cluster. In dit voorbeeld bevindt zich op `/example/jars/hadoop-mapreduce-examples.jar` op de standaard-opslag voor uw cluster.

De volgende Java-code is de bron van de MapReduce-toepassing die is opgenomen in de `hadoop-mapreduce-examples.jar` bestand:

```java
package org.apache.hadoop.examples;

import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.GenericOptionsParser;

public class WordCount {

    public static class TokenizerMapper
        extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
        StringTokenizer itr = new StringTokenizer(value.toString());
        while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
        }
    }
    }

    public static class IntSumReducer
        extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                        Context context
                        ) throws IOException, InterruptedException {
        int sum = 0;
        for (IntWritable val : values) {
        sum += val.get();
        }
        result.set(sum);
        context.write(key, result);
    }
    }

    public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
    if (otherArgs.length != 2) {
        System.err.println("Usage: wordcount <in> <out>");
        System.exit(2);
    }
    Job job = new Job(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
    FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
```

Zie de volgende documenten voor instructies om uw eigen MapReduce-toepassingen te schrijven:

* [Ontwikkelen van Java-MapReduce-toepassingen voor HDInsight](apache-hadoop-develop-deploy-java-mapreduce-linux.md)

* [Python-MapReduce-toepassingen voor HDInsight ontwikkelen](apache-hadoop-streaming-python.md)

## <a id="run"></a>De MapReduce uitvoeren

HDInsight kunt HiveQL taken uitvoeren met behulp van verschillende methoden. Gebruik de volgende tabel om te bepalen welke methode is geschikt voor u en volg de koppeling voor een overzicht.

| **Gebruik deze**... | **...om hiervoor** | ...door dit **cluster-besturingssysteem** | ...from dit **clientbesturingssysteem** |
|:--- |:--- |:--- |:--- |
| [SSH](apache-hadoop-use-mapreduce-ssh.md) |Gebruik de opdracht Hadoop via **SSH** |Linux |Linux, Unix, Mac OS X of Windows |
| [CURL](apache-hadoop-use-mapreduce-curl.md) |Verzenden van de taak op afstand via **REST** |Linux- of Windows |Linux, Unix, Mac OS X of Windows |
| [Windows PowerShell](apache-hadoop-use-mapreduce-powershell.md) |Verzenden van de taak op afstand via **Windows PowerShell** |Linux- of Windows |Windows |
| [Extern bureaublad](apache-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 en 3.3) |Gebruik de opdracht Hadoop via **extern bureaublad** |Windows |Windows |

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a id="nextsteps"></a>Volgende stappen

Zie de volgende documenten voor meer informatie over het werken met gegevens in HDInsight:

* [Het ontwikkelen van Java-MapReduce-programma's voor HDInsight](apache-hadoop-develop-deploy-java-mapreduce-linux.md)

* [Python streaming MapReduce-programma's voor HDInsight ontwikkelen](apache-hadoop-streaming-python.md)

* [Hive gebruiken met HDInsight][hdinsight-use-hive]

* [Pig gebruiken met HDInsight][hdinsight-use-pig]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: apache-hadoop-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]:../hdinsight-use-hive.md
[hdinsight-use-pig]:hdinsight-use-pig.md


[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
