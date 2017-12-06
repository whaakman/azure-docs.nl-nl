---
title: Gebruik C# met MapReduce van Hadoop in HDInsight - Azure | Microsoft Docs
description: Informatie over het gebruik van C# MapReduce om oplossingen te maken met Hadoop in Azure HDInsight.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d83def76-12ad-4538-bb8e-3ba3542b7211
ms.custom: hdinsightactive
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/05/2017
ms.author: larryfr
ms.openlocfilehash: cb07f173a0ff669cc8fa56d1ba37a4a3df8d8753
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="use-c-with-mapreduce-streaming-on-hadoop-in-hdinsight"></a>Gebruik C# met MapReduce streaming van Hadoop in HDInsight

Informatie over het gebruik C# voor het maken van een MapReduce-oplossing op HDInsight.

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie voor meer informatie [versiebeheer van HDInsight-onderdeel](../hdinsight-component-versioning.md).

Hadoop-streaming is een hulpprogramma waarmee u een script of uitvoerbaar bestand met MapReduce-taken uitvoeren. In dit voorbeeld wordt .NET gebruikt om de toewijzen en reducer voor een word-count-oplossing te implementeren.

## <a name="net-on-hdinsight"></a>.NET in HDInsight

__HDInsight op basis van Linux__ clusters gebruik [Mono (https://mono-project.com)](https://mono-project.com) .NET-toepassingen uit te voeren. Mono versie 4.2.1 is opgenomen in HDInsight versie 3.5. Zie voor meer informatie over de versie van Mono opgenomen met HDInsight [HDInsight onderdeel versies](../hdinsight-component-versioning.md). Zie voor het gebruik van een specifieke versie van Mono de [installeren of update Mono](../hdinsight-hadoop-install-mono.md) document.

Zie voor meer informatie over de Mono compatibiliteit met versies van .NET Framework [Mono compatibiliteit](http://www.mono-project.com/docs/about-mono/compatibility/).

## <a name="how-hadoop-streaming-works"></a>Hadoop-streaming werking

Het basisproces gebruikt voor streaming in dit document is als volgt:

1. Hadoop geeft gegevens aan de mapper (mapper.exe in dit voorbeeld) op STDIN.
2. De toewijzing verwerkt de gegevens en door tabs gescheiden sleutel-waardeparen voor STDOUT verzendt.
3. De uitvoer is gelezen door Hadoop en vervolgens doorgegeven aan de reducer (reducer.exe in dit voorbeeld) op STDIN.
4. De reducer door tabs gescheiden sleutel-/ waardeparen leest de gegevens worden verwerkt en verzendt vervolgens het resultaat als door tabs gescheiden sleutel-waardeparen op STDOUT.
5. De uitvoer is door Hadoop gelezen en geschreven naar de uitvoermap.

Zie voor meer informatie over het streaming- [Hadoop-Streaming (https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).

## <a name="prerequisites"></a>Vereisten

* U moet bekend zijn met schrijven en het bouwen van C#-code die gericht is op .NET Framework 4.5. De stappen in dit document gebruikt u Visual Studio 2017.

* Een manier .exe-bestanden uploaden naar het cluster. De stappen in dit document gebruikt het Data Lake Tools voor Visual Studio de bestanden uploaden naar de primaire opslagruimte voor het cluster.

* Azure PowerShell of SSH-client.

* Een Hadoop op HDInsight-cluster. Zie voor meer informatie over het maken van een cluster [maken van een HDInsight-cluster](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="create-the-mapper"></a>De mapper maken

Maak in Visual Studio een nieuw __consoletoepassing__ met de naam __mapper__. De volgende code gebruiken voor de toepassing:

```csharp
using System;
using System.Text.RegularExpressions;

namespace mapper
{
    class Program
    {
        static void Main(string[] args)
        {
            string line;
            //Hadoop passes data to the mapper on STDIN
            while((line = Console.ReadLine()) != null)
            {
                // We only want words, so strip out punctuation, numbers, etc.
                var onlyText = Regex.Replace(line, @"\.|;|:|,|[0-9]|'", "");
                // Split at whitespace.
                var words = Regex.Matches(onlyText, @"[\w]+");
                // Loop over the words
                foreach(var word in words)
                {
                    //Emit tab-delimited key/value pairs.
                    //In this case, a word and a count of 1.
                    Console.WriteLine("{0}\t1",word);
                }
            }
        }
    }
}
```

Na het maken van de toepassing bouwen voor het produceren van het `/bin/Debug/mapper.exe` bestand in de projectmap.

## <a name="create-the-reducer"></a>De reducer maken

Maak in Visual Studio een nieuw __consoletoepassing__ met de naam __reducer__. De volgende code gebruiken voor de toepassing:

```csharp
using System;
using System.Collections.Generic;

namespace reducer
{
    class Program
    {
        static void Main(string[] args)
        {
            //Dictionary for holding a count of words
            Dictionary<string, int> words = new Dictionary<string, int>();

            string line;
            //Read from STDIN
            while ((line = Console.ReadLine()) != null)
            {
                // Data from Hadoop is tab-delimited key/value pairs
                var sArr = line.Split('\t');
                // Get the word
                string word = sArr[0];
                // Get the count
                int count = Convert.ToInt32(sArr[1]);

                //Do we already have a count for the word?
                if(words.ContainsKey(word))
                {
                    //If so, increment the count
                    words[word] += count;
                } else
                {
                    //Add the key to the collection
                    words.Add(word, count);
                }
            }
            //Finally, emit each word and count
            foreach (var word in words)
            {
                //Emit tab-delimited key/value pairs.
                //In this case, a word and a count of 1.
                Console.WriteLine("{0}\t{1}", word.Key, word.Value);
            }
        }
    }
}
```

Na het maken van de toepassing bouwen voor het produceren van het `/bin/Debug/reducer.exe` bestand in de projectmap.

## <a name="upload-to-storage"></a>Uploaden naar de opslag

1. Open in Visual Studio **Server Explorer**.

2. Vouw **Azure** uit en vouw vervolgens **HDInsight** uit.

3. Als u wordt gevraagd, Voer uw referenties in Azure-abonnement en klik vervolgens op **aanmelden**.

4. Vouw het HDInsight-cluster die u wilt deze toepassing te implementeren. Een item met de tekst __(Default Storage Account)__ wordt vermeld.

    ![Server Explorer met de storage-account voor het cluster](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/storage.png)

    * Als dit item kan worden uitgebreid, gebruikt u een __Azure Storage-Account__ als standaard opslag voor het cluster. Als u de bestanden op de standaard-opslag voor het cluster, vouw de vermelding en dubbelklikt u vervolgens op de __(standaardcontainer)__.

    * Als dit item kan niet worden uitgebreid, gebruikt u __Azure Data Lake Store__ als de opslag van de standaard voor het cluster. Als u wilt weergeven van de bestanden op de standaard-opslag voor het cluster, dubbelklikt u op de __(Standaardopslagaccount)__ vermelding.

5. Als u wilt de .exe-bestanden uploaden, moet u een van de volgende methoden gebruiken:

    * Als u een __Azure Storage-Account__, klik op het pictogram uploaden en blader vervolgens naar de **bin\debug** map voor de **mapper** project. Tot slot selecteert u de **mapper.exe** -bestand en klik op **Ok**.

        ![pictogram uploaden](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/upload.png)
    
    * Als u __Azure Data Lake Store__, met de rechtermuisknop op een leeg gebied in de lijst en selecteer vervolgens __uploaden__. Tot slot selecteert u de **mapper.exe** -bestand en klik op **Open**.

    Eenmaal de __mapper.exe__ upload is voltooid, herhaalt het uploadproces voor de __reducer.exe__ bestand.

## <a name="run-a-job-using-an-ssh-session"></a>Een taak uitvoeren: met behulp van een SSH-sessie

1. SSH gebruiken voor verbinding met het HDInsight-cluster. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. Gebruik een van de volgende opdrachten om de MapReduce-taak starten:

    * Als u __Data Lake Store__ als standaard opslag:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files adl:///mapper.exe,adl:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```
    
    * Als u __Azure Storage__ als standaard opslag:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files wasb:///mapper.exe,wasb:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```

    De volgende lijst beschrijft wat elke parameter doet:

    * `hadoop-streaming.jar`: De jar-bestand waarin de functionaliteit voor streaming MapReduce.
    * `-files`: Hiermee voegt u toe de `mapper.exe` en `reducer.exe` bestanden naar deze taak. De `adl:///` of `wasb:///` voordat elk bestand het pad naar de hoofdmap van de standaard opslagruimte voor het cluster is.
    * `-mapper`: Hiermee geeft u op welk bestand implementeert de mapper.
    * `-reducer`: Hiermee geeft u op welk bestand implementeert de reducer.
    * `-input`: De invoergegevens.
    * `-output`: De uitvoermap.

3. Zodra de MapReduce-taak is voltooid, gebruikt u de volgende om de resultaten weer te geven:

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    De volgende tekst is een voorbeeld van de gegevens die zijn geretourneerd door deze opdracht:

        you     1128
        young   38
        younger 1
        youngest        1
        your    338
        yours   4
        yourself        34
        yourselves      3
        youth   17

## <a name="run-a-job-using-powershell"></a>Een taak uitvoeren: met behulp van PowerShell

Gebruik de volgende PowerShell-script voor het uitvoeren van een MapReduce-taak en de resultaten te downloaden.

[!code-powershell[main](../../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]

Dit script wordt u gevraagd om de cluster account aanmeldingsnaam en wachtwoord, samen met de naam van het HDInsight-cluster. Zodra de taak is voltooid, de uitvoer wordt gedownload naar een bestand met de naam `output.txt`. De volgende tekst is een voorbeeld van de gegevens in de `output.txt` bestand:

    you     1128
    young   38
    younger 1
    youngest        1
    your    338
    yours   4
    yourself        34
    yourselves      3
    youth   17

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van MapReduce met HDInsight [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md).

Zie voor meer informatie over het gebruik van C# met Hive en Pig [een C# gebruiker gedefinieerde functie gebruiken met Hive en Pig](apache-hadoop-hive-pig-udf-dotnet-csharp.md).

Zie voor meer informatie over het gebruik van C# met Storm op HDInsight [C#-topologieën ontwikkelen voor Storm op HDInsight](../storm/apache-storm-develop-csharp-visual-studio-topology.md).