---
title: Gebruik C# met MapReduce met Hadoop in HDInsight - Azure
description: Meer informatie over het gebruik van C# MapReduce om oplossingen te maken met Apache Hadoop in Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: hrasheed
ms.openlocfilehash: 130ca849b39336637f53b32043874b5d037a8f0d
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56342920"
---
# <a name="use-c-with-mapreduce-streaming-on-apache-hadoop-in-hdinsight"></a>Gebruik C# met MapReduce, streaming van Apache Hadoop in HDInsight

Leer hoe u C# gebruikt voor het maken van een oplossing MapReduce in HDInsight.

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie voor meer informatie, [versiebeheer van HDInsight-onderdeel](../hdinsight-component-versioning.md).

Apache Hadoop-streaming is een hulpprogramma waarmee u MapReduce-taken met behulp van een script of uitvoerbare bestand uit te voeren. In dit voorbeeld wordt .NET gebruikt om de toewijzing en reducer voor een word-count-oplossing te implementeren.

## <a name="net-on-hdinsight"></a>.NET in HDInsight

__HDInsight op basis van Linux__ clusters gebruik [Mono (https://mono-project.com) ](https://mono-project.com) .NET-toepassingen uit te voeren. Mono versie 4.2.1 is opgenomen in HDInsight versie 3.6. Zie voor meer informatie over de versie van Mono geleverd bij HDInsight [HDInsight onderdeel versies](../hdinsight-component-versioning.md). Voor het gebruik van een specifieke versie van Mono, Zie de [installeren of bijwerken Mono](../hdinsight-hadoop-install-mono.md) document.

Zie voor meer informatie over de Mono-compatibiliteit met versies van .NET Framework [Mono-compatibiliteit](https://www.mono-project.com/docs/about-mono/compatibility/).

## <a name="how-hadoop-streaming-works"></a>Hoe Hadoop-streaming werkt

Het basisproces gebruikt voor het streamen van in dit document is als volgt:

1. Hadoop wordt gegevens doorgegeven aan het toewijzen van de (mapper.exe in dit voorbeeld) op STDIN.
2. Het toewijzen van de verwerkt de gegevens en door tabs gescheiden sleutel/waarde-paren naar STDOUT verzendt.
3. De uitvoer wordt gelezen door Hadoop, en vervolgens doorgegeven aan de reducer (reducer.exe in dit voorbeeld) op STDIN.
4. De reducer leest de door tabs gescheiden sleutel/waarde-paren, verwerkt de gegevens en verzendt vervolgens het resultaat als door tabs gescheiden sleutel/waarde-paren op STDOUT.
5. De uitvoer is door Hadoop gelezen en geschreven naar de uitvoermap.

Zie voor meer informatie over het streamen van [Hadoop-Streaming](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).

## <a name="prerequisites"></a>Vereisten

* Een vertrouwd zijn met het schrijven en het bouwen van C#-code die gericht is op .NET Framework 4.5. De stappen in dit document gebruikt Visual Studio 2017.

* Een manier om het .exe-bestanden uploaden naar het cluster. De stappen in dit document gebruikt u de Data Lake Tools voor Visual Studio de bestanden te uploaden naar de primaire opslag voor het cluster.

* Azure PowerShell of een SSH-client.

* Een Hadoop op HDInsight-cluster. Zie voor meer informatie over het maken van een cluster [maken van een HDInsight-cluster](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="create-the-mapper"></a>Het toewijzen van de maken

In Visual Studio, maak een nieuwe __consoletoepassing__ met de naam __mapper__. Gebruik de volgende code voor de toepassing:

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

Na het maken van de toepassing, bouw het voor het produceren van de `/bin/Debug/mapper.exe` bestand in de projectmap.

## <a name="create-the-reducer"></a>De reducer maken

In Visual Studio, maak een nieuwe __consoletoepassing__ met de naam __reducer__. Gebruik de volgende code voor de toepassing:

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

Na het maken van de toepassing, bouw het voor het produceren van de `/bin/Debug/reducer.exe` bestand in de projectmap.

## <a name="upload-to-storage"></a>Uploaden naar storage

1. Open in Visual Studio, **Server Explorer**.

2. Vouw **Azure** uit en vouw vervolgens **HDInsight** uit.

3. Als u hierom wordt gevraagd, voert u de referenties van uw Azure-abonnement en klik vervolgens op **aanmelden**.

4. Vouw het HDInsight-cluster dat u wilt deze toepassing te implementeren. Een item met de tekst __(Storage-standaardaccount)__ wordt vermeld.

    ![Server Explorer met de storage-account voor het cluster](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/storage.png)

    * Als dit item kan worden uitgebreid, gebruikt u een __Azure Storage-Account__ als standaardopslag voor het cluster. Als u de bestanden op de standaardopslag voor het cluster, vouw de vermelding en dubbelklik vervolgens op de __(standaardcontainer)__.

    * Als dit item kan niet worden uitgebreid, gebruikt u __Azure Data Lake Storage__ als de standaardopslag voor het cluster. Als u de bestanden op de standaardopslag voor het cluster, dubbelklikt u op de __(Standaardopslagaccount)__ vermelding.

5. Als u wilt het .exe-bestanden uploaden, moet u een van de volgende methoden gebruiken:

    * Als een __Azure Storage-Account__, klik op het uploadpictogram en blader vervolgens naar de **bin\debug** map voor de **mapper** project. Selecteer ten slotte de **mapper.exe** -bestand en klik op **Ok**.

        ![pictogram uploaden](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/upload.png)
    
    * Als u __Azure Data Lake Storage__, met de rechtermuisknop op een leeg gebied in de lijst en selecteer vervolgens __uploaden__. Selecteer ten slotte de **mapper.exe** -bestand en klik op **Open**.

    Zodra de __mapper.exe__ uploaden is voltooid, herhaalt u het uploadproces voor de __reducer.exe__ bestand.

## <a name="run-a-job-using-an-ssh-session"></a>Een taak uitvoeren: Met behulp van een SSH-sessie

1. Gebruik SSH verbinding maken met de HDInsight-cluster. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. Gebruik een van de volgende opdrachten om de MapReduce-taak te starten:

    * Als u __Data Lake Storage Gen2__ als standaardopslag:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files abfs:///mapper.exe,abfs:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```

    * Als u __Data Lake Storage Gen1__ als standaardopslag:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files adl:///mapper.exe,adl:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```
    
    * Als u __Azure Storage__ als standaardopslag:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files wasb:///mapper.exe,wasb:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```

    De volgende lijst beschrijft wat elke parameter doet:

    * `hadoop-streaming.jar`: Het jar-bestand met de functionaliteit voor streaming MapReduce.
    * `-files`: Voegt de `mapper.exe` en `reducer.exe` bestanden naar deze taak. De `abfs:///`,`adl:///` of `wasb:///` voordat elk bestand het pad naar de hoofdmap van de standaardopslag voor het cluster is.
    * `-mapper`: Hiermee geeft u op welk bestand het toewijzen van de implementeert.
    * `-reducer`: Hiermee geeft u op welk bestand implementeert de reducer.
    * `-input`: De ingevoerde gegevens.
    * `-output`: De uitvoermap.

3. Nadat de MapReduce-taak is voltooid, gebruikt u de volgende om de resultaten weer te geven:

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

## <a name="run-a-job-using-powershell"></a>Een taak uitvoeren: PowerShell gebruiken

Gebruik de volgende PowerShell-script uitvoeren van een MapReduce-taak en de resultaten downloaden.

[!code-powershell[main](../../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]

Met dit script vraagt u om de cluster-aanmelding-accountnaam en wachtwoord, samen met de naam van het HDInsight-cluster. Nadat de taak is voltooid, de uitvoer wordt gedownload naar een bestand met de naam `output.txt`. De volgende tekst is een voorbeeld van de gegevens in de `output.txt` bestand:

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

Zie voor meer informatie over het gebruik van MapReduce in HDInsight [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md).

Voor informatie over het gebruik van C# met Hive en Pig, Zie [gebruik een C# door de gebruiker gedefinieerde functie met Apache Hive en Apache Pig](apache-hadoop-hive-pig-udf-dotnet-csharp.md).

Voor informatie over het gebruik van C# met Storm op HDInsight, raadpleegt u [ontwikkelen C# topologieën voor Apache Storm op HDInsight](../storm/apache-storm-develop-csharp-visual-studio-topology.md).
