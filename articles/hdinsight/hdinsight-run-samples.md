---
title: De voorbeelden van het Hadoop in HDInsight - Azure uitvoeren | Microsoft Docs
description: Aan de slag met de Azure HDInsight-service met de voorbeelden. PowerShell-scripts die MapReduce-programma's op gegevens clusters uitvoeren gebruiken.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: bf76d452-abb4-4210-87bd-a2067778c6ed
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 6e4dbf1fa32f916a206e4f8d796218ab809835ad
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="run-hadoop-mapreduce-samples-in-windows-based-hdinsight"></a>Voorbeelden van Hadoop MapReduce uitvoeren in HDInsight op basis van Windows
[!INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Een reeks voorbeelden zijn bedoeld om optimaal te slag actieve MapReduce-taken op Azure HDInsight met Hadoop-clusters. Deze voorbeelden worden beschikbaar gesteld op elk van de beheerde HDInsight-clusters die u maakt. Deze voorbeelden wordt uitgevoerd, raakt u vertrouwd met het gebruik van Azure PowerShell-cmdlets uit te voeren taken op Hadoop-clusters.

* [**Word-count**][hdinsight-sample-wordcount]: telt word-exemplaren in een tekstbestand.
* [**C#-streaming aantal woorden**][hdinsight-sample-csharp-streaming]: telt word-exemplaren in een tekstbestand met de Hadoop-streaming-interface.
* [**PI estimator**][hdinsight-sample-pi-estimator]: maakt gebruik van een statistische (quasi Monte Carlo) methode voor het schatten van de waarde van pi.
* [**10 GB Graysort**][hdinsight-sample-10gb-graysort]: een algemeen GraySort uitvoeren op een 10 GB-bestand met behulp van HDInsight. Er zijn drie taken uitvoeren: Teragen voor het genereren van de gegevens, Terasort de gegevens worden gesorteerd en Teravalidate om te bevestigen dat de gegevens op de juiste wijze is gesorteerd.

> [!NOTE]
> De broncode vindt u in de bijlage.

Veel aanvullende documentatie bestaat op het web voor Hadoop-gerelateerde technologieën, zoals Java gebaseerde MapReduce programmering en streaming en documentatie over de cmdlets die worden gebruikt in Windows PowerShell-scripts. Zie voor meer informatie over deze resources:

* [Het ontwikkelen van Java-MapReduce-programma's voor Hadoop in HDInsight](hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md)
* [Hadoop-taken opgeven in HDInsight](hadoop/submit-apache-hadoop-jobs-programmatically.md)
* [Inleiding tot Azure HDInsight][hdinsight-introduction]

Veel mensen kiezen tegenwoordig Hive en Pig via MapReduce.  Zie voor meer informatie:

* [Hive in HDInsight gebruiken](hadoop/hdinsight-use-hive.md)
* [Pig gebruiken in HDInsight](hadoop/hdinsight-use-pig.md)

**Vereisten**:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **een HDInsight-cluster**. Zie voor instructies over de verschillende manieren waarin dergelijke clusters kunnen worden gemaakt, [maken Hadoop-clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
* **Een werkstation met Azure PowerShell**.

    > [!IMPORTANT]
    > Azure PowerShell-ondersteuning voor het beheer van HDInsight-resources met behulp van Azure Service Manager is **afgeschaft** en verdwijnt per 1 januari 2017. In de stappen in dit document worden de nieuwe HDInsight-cmdlets gebruikt die met Azure Resource Manager werken.
    >
    > Volg de stappen in [Azure PowerShell installeren en configureren](/powershell/azureps-cmdlets-docs) de meest recente versie van Azure PowerShell installeren. Als u scripts hebt die worden gewijzigd moeten voor het gebruik van de nieuwe cmdlets die met werken Azure Resource Manager, Zie [migreren naar Azure Resource Manager gebaseerde ontwikkelprogramma's voor HDInsight-clusters](hdinsight-hadoop-development-using-azure-resource-manager.md).

## <a name="hdinsight-sample-wordcount"></a>Word-count - Java
Als u wilt een MapReduce-project verzenden, moet u eerst de definitie van een MapReduce-taak maken. In de taakdefinitie u het MapReduce programma jar-bestand en de locatie van de jar-bestand is opgeven **wasb:///example/jars/hadoop-mapreduce-examples.jar**, de klassenaam van de en de argumenten.  Het wordcount MapReduce-programma heeft twee argumenten: het bron-bestand dat wordt gebruikt voor het aantal woorden en de locatie voor uitvoer.

De broncode vindt u in de [bijlage A](#apendix-a---the-word-count-MapReduce-program-in-java).

Programma voor de procedure van het ontwikkelen van een Java-MapReduce, Zie - [ontwikkelen van Java-MapReduce-programma's voor Hadoop in HDInsight](hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md)

**Een word-count-MapReduce-taak verzenden**

1. Open **Windows PowerShell ISE**. Zie voor instructies [installeren en configureren van Azure PowerShell][powershell-install-configure].
2. Plak het volgende PowerShell-script:

    ```powershell
    $subscriptionName = "<Azure Subscription Name>"
    $resourceGroupName = "<Resource Group Name>"
    $clusterName = "<HDInsight cluster name>"             # HDInsight cluster name

    Select-AzureRmSubscription -SubscriptionName $subscriptionName

    # Define the MapReduce job
    $mrJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "wordcount" `
                                -Arguments "wasb:///example/data/gutenberg/davinci.txt", "wasb:///example/data/WordCountOutput"

    # Submit the job and wait for job completion
    $cred = Get-Credential -Message "Enter the HDInsight cluster HTTP user credential:"
    $mrJob = Start-AzureRmHDInsightJob `
                        -ResourceGroupName $resourceGroupName `
                        -ClusterName $clusterName `
                        -HttpCredential $cred `
                        -JobDefinition $mrJobDefinition

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -HttpCredential $cred `
        -JobId $mrJob.JobId

    # Get the job output
    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer

    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -HttpCredential $cred `
        -DefaultStorageAccountName $defaultStorageAccount `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultStorageContainer  `
        -JobId $mrJob.JobId `
        -DisplayOutputType StandardError

    # Download the job output to the workstation
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob example/data/WordCountOutput/part-r-00000 -Context $storageContext -Force

    # Display the output file
    cat ./example/data/WordCountOutput/part-r-00000 | findstr "there"
    ```

    De MapReduce-taak wordt een bestand met de naam *onderdeel-r-00000*, die woorden en het aantal bevat. Het script gebruikt de **findstr** opdracht om een lijst van alle woorden met *"there"*.
3. De eerste drie variabelen worden ingesteld en voer het script.

## <a name="hdinsight-sample-csharp-streaming"></a>Word-count - C#-streaming
Hadoop biedt een streaming-API voor MapReduce, waarmee u kaart schrijven en functies in andere talen dan Java te verminderen.

> [!NOTE]
> De stappen in deze zelfstudie alleen van toepassing op Windows gebaseerde HDInsight-clusters. Zie voor een voorbeeld van streaming voor Linux gebaseerde HDInsight-clusters [ontwikkelen Python streaming-programma's voor HDInsight](hadoop/apache-hadoop-streaming-python.md).

In het voorbeeld wordt de toewijzing en de reducer zijn uitvoerbare bestanden die de invoer van lezen [stdin] [ stdin-stdout-stderr] (door regel) en het verzenden van de uitvoer naar [stdout][stdin-stdout-stderr]. Het programma telt de woorden in de tekst.

Als een uitvoerbaar bestand is opgegeven voor **mappers**, elke mapper-taak wordt het uitvoerbare bestand als een afzonderlijk proces gestart wanneer de toewijzing is geïnitialiseerd. Als de mapper-taak wordt uitgevoerd, converteert u de invoer in regels en -feeds van de regels die moeten worden de [stdin] [ stdin-stdout-stderr] van het proces.

In de tussentijd verzamelt het toewijzen van de uitvoer van de regel gerichte van stdout van het proces. Elke regel worden geconverteerd naar een sleutel/waarde-paar wordt verzameld als de uitvoer van de toewijzing. Standaard is de sleutel van het voorvoegsel van een regel tot aan het eerste teken van tabblad en de rest van de regel (met uitzondering van de Tab-teken) is de waarde. Er is geen Tab-teken in de regel, volledige regel als de sleutel wordt beschouwd als de waarde null is.

Als een uitvoerbaar bestand is opgegeven voor **verkleiningstoestellen**, elke taak reducer start het uitvoerbare bestand als een afzonderlijk proces wanneer de reducer is geïnitialiseerd. Als de taak reducer wordt uitgevoerd, het waardeparen invoer sleutelwaarden worden geconverteerd in regels en het feeds van de regels die moeten worden de [stdin] [ stdin-stdout-stderr] van het proces.

In de tussentijd de reducer verzamelt de uitvoer van de regel gerichte van de [stdout] [ stdin-stdout-stderr] van het proces. Elke regel wordt geconverteerd naar een sleutel/waarde-paar als de uitvoer van de reducer worden verzameld. Standaard is de sleutel van het voorvoegsel van een regel tot aan het eerste teken van tabblad en de rest van de regel (met uitzondering van de Tab-teken) is de waarde.

**Verzenden van een streaming-taak van word-count C#**

* Volg de procedure in [Word-count - Java](#word-count-java), en vervang de taakdefinitie met de volgende regel:

    ```powershell
    $mrJobDefinition = New-AzureRmHDInsightStreamingMapReduceJobDefinition `
                            -Files "/example/apps/cat.exe","/example/apps/wc.exe" `
                            -Mapper "cat.exe" `
                            -Reducer "wc.exe" `
                            -InputPath "/example/data/gutenberg/davinci.txt" `
                            -OutputPath "/example/data/StreamingOutput/wc.txt"
    ```

    Het uitvoerbestand worden:

        example/data/StreamingOutput/wc.txt/part-00000

## <a name="hdinsight-sample-pi-estimator"></a>PI estimator
De estimator pi maakt gebruik van een statistische (quasi Monte Carlo) methode voor het schatten van de waarde van pi. Punten geplaatst willekeurig binnen een eenheid vierkante ook vallen binnen een cirkel met een kans die gelijk is aan het gebied van de cirkel zijn aangebracht in vierkant pi/4. De waarde van pi kan worden geschat van de waarde van 4R, R is waar de verhouding tussen het aantal punten die in het totale aantal punten die binnen het kwadraat van de cirkel. Hoe groter het voorbeeld van punten gebruikt, hoe nauwkeuriger de schatting is.

Het script dat is opgegeven voor dit voorbeeld verzendt een taak van de jar Hadoop en is ingesteld tot 16 maps, die elk 10 miljoen voorbeeldpunten berekenen is vereist voor de parameterwaarden die worden uitgevoerd met een waarde. Deze parameterwaarden kunnen worden gewijzigd om te verbeteren van de geschatte waarde van pi. Ter referentie zijn de eerste 10 decimalen van pi 3.1415926535.

**Een pi estimator taak verzenden**

* Volg de procedure in [Word-count - Java](#word-count-java), en vervang de taakdefinitie met de volgende regel:

    ```powershell
    $mrJobJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "pi" `
                                -Arguments "16", "10000000"
    ```

## <a name="hdinsight-sample-10gb-graysort"></a>10 GB Graysort
Dit voorbeeld wordt een matige 10GB aan gegevens gebruikt, zodat deze kan relatief snel kan worden uitgevoerd. De MapReduce-toepassingen die zijn ontwikkeld door Owen O'Malley en Arun Murthy die de jaarlijkse Algemeen ('daytona') terabyte sorteren benchmark 2009 met een snelheid van 0.578 TB/min (100 TB in 173 minuten gewonnen) wordt gebruikt. Zie voor meer informatie over deze en andere sorteren benchmarks de [Sortbenchmark](http://sortbenchmark.org/) site.

Dit voorbeeld worden drie sets van MapReduce-programma's gebruikt:

1. **TeraGen** is een MapReduce-programma dat u gebruiken kunt voor het genereren van de rijen van de gegevens moeten worden gesorteerd.
2. **TeraSort** voorbeelden van de invoergegevens en MapReduce gebruikt voor het sorteren van de gegevens in een totale volgorde. TeraSort is een standaard soort MapReduce-functies, met uitzondering van een aangepaste partitioner die gebruikmaakt van een gesorteerde lijst met actieve N-1 sleutels die het belangrijkste bereik voor elke verminderen definiëren. In het bijzonder alle sleutels dergelijke waarvan de steekproef [i-1] < sleutel = < voorbeeld [i] worden verzonden naar de i verminderen. Dit garandeert dat de uitvoer van ik beperken zijn alle kleiner is dan de uitvoer van verminderen i + 1.
3. **TeraValidate** is een MapReduce-programma te valideren dat de uitvoer globaal is gesorteerd. Een kaart per bestand wordt gemaakt in de uitvoermap en elke toewijzing zorgt ervoor dat elke sleutel kleiner dan of gelijk zijn aan de vorige sectie is. De functie kaart genereert ook records van de eerste en laatste sleutels van elk bestand en de functie verminderen zorgt ervoor dat de eerste sleutel van het bestand i groter dan de laatste sleutel van het bestand i-1 is. Eventuele problemen worden gemeld als uitvoer van de verminderen met de sleutels die onjuist zijn.

De indeling invoer en uitvoer, die wordt gebruikt door alle drie de toepassingen, leest en schrijft de tekstbestanden in de juiste notatie. De uitvoer van de verminderen heeft de replicatie is ingesteld op 1, in plaats van de standaard 3, omdat de wedstrijd benchmark niet vereist dat de uitvoergegevens worden gerepliceerd naar meerdere knooppunten.

Drie taken vereist zijn voor het voorbeeld elke overeenkomt met een van de MapReduce-programma's die worden beschreven in de inleiding:

1. Genereren van de gegevens voor het sorteren van door het uitvoeren van de **TeraGen** MapReduce-taak.
2. Sorteer de gegevens door het uitvoeren van de **TeraSort** MapReduce-taak.
3. Bevestig dat de gegevens op de juiste wijze is gesorteerd door het uitvoeren van de **TeraValidate** MapReduce-taak.

**Om de taken te verzenden**

* Volg de procedure in [Word-count - Java](#word-count-java), en gebruik de volgende taakdefinities:

    ```powershell
    $teragen = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "teragen" `
                                -Arguments "-Dmapred.map.tasks=50", "100000000", "/example/data/10GB-sort-input"

    $terasort = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "terasort" `
                                -Arguments "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/example/data/10GB-sort-input", "/example/data/10GB-sort-output"

    $teravalidate = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "teravalidate" `
                                -Arguments "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/example/data/10GB-sort-output", "/example/data/10GB-sort-validate"
    ```

## <a name="next-steps"></a>Volgende stappen
In dit artikel en de artikelen in elk van de voorbeelden hebt u geleerd hoe u de voorbeelden van opgenomen in de HDInsight-clusters met behulp van Azure PowerShell uitvoeren. Zie de volgende onderwerpen voor zelfstudies over het gebruik van Pig, Hive en MapReduce met HDInsight:

* [Aan de slag met Hadoop Hive in HDInsight analyseren mobiele telefoon gebruiken][hdinsight-get-started]
* [Pig gebruiken met Hadoop in HDInsight][hdinsight-use-pig]
* [Hive gebruiken met Hadoop in HDInsight][hdinsight-use-hive]
* [Hadoop-taken in HDInsight verzenden][hdinsight-submit-jobs]
* [Azure HDInsight SDK-documentatie][hdinsight-sdk-documentation]

## <a name="appendix-a---the-word-count-source-code"></a>Bijlage A - de broncode van de Word-count

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

## <a name="appendix-b---the-word-count-streaming-source-code"></a>Bijlage B - het aantal woorden broncode streaming
MapReduce wordt de toepassing cat.exe als een toewijzing-interface om te streamen van de tekst in de console en de toepassing wc.exe als de interface verminderen om het aantal woorden die uit een document worden gestreamd. Het toewijzen en de reducer Standaardinvoerstroom (stdin) tekens, regel voor regel lezen en schrijven naar de standaard uitvoerstroom (stdout).

```csharp
// The source code for the cat.exe (Mapper).

using System;
using System.IO;

namespace cat
{
    class cat
    {
        static void Main(string[] args)
        {
            if (args.Length > 0)
            {
                Console.SetIn(new StreamReader(args[0]));
            }

            string line;
            char[] separators = { ' ', '\n'};
            while ((line = Console.ReadLine()) != null)
            {
                string[] words = line.Split(separators);
                foreach (var word in words)
                {
                    Console.WriteLine("{0}\t1", word);
                }
            }
        }
    }
}
```

De code toewijzen in het bestand cat.cs gebruikt een [StreamReader] [ streamreader] object om de tekens van de stroom inkomende naar de console, die vervolgens naar de standaard uitvoerstroom met vaste de stroom schrijft te lezen [Console.Writeline] [ console-writeline] methode.

```csharp
// The source code for wc.exe (Reducer) is:

using System;
using System.IO;
using System.Linq;
using System.Collections;

namespace wc
{
    class wc
    {
        static void Main(string[] args)
        {
            string line;

            if (args.Length > 0)
            {
                Console.SetIn(new StreamReader(args[0]));
            }

            Hashtable wordCount = new Hashtable();
            while ((line = Console.ReadLine()) != null)
            {
                string[] words = line.Split('\t');

                string key = words[0];

                if (wordCount.ContainsKey(key) == true)
                {
                    int n = Convert.ToInt32(wordCount[key]);
                    wordCount[key] = Convert.ToString(n + 1);
                }
                else
                {
                    wordCount[key] = words[1];
                }
            }
            foreach (var key in wordCount.Keys)
            {
                Console.WriteLine("{0} {1}", key, wordCount[key]);
            }
        }
    }
}
```

De code reducer in het bestand wc.cs gebruikt een [StreamReader] [ streamreader] object lezen tekens uit de Standaardinvoerstroom die zijn uitgevoerd door de cat.exe toewijzen. Het lezen en de tekens met de [Console.Writeline] [ console-writeline] methode, wordt dit geteld de woorden door spaties en einde van regel tekens aan het einde van elk woord tellen. Vervolgens worden het totale aantal geschreven naar de standaard uitvoerstroom met de [Console.Writeline] [ console-writeline] methode.

## <a name="appendix-c---the-pi-estimator-source-code"></a>Bijlage C - de broncode voor Pi estimator
De pi estimator Java-code die de functies toewijzen en reducer bevat is beschikbaar voor inspectie hieronder. Het programma mapper genereert een opgegeven aantal punten op willekeurige binnen een vierkant eenheid geplaatst en vervolgens telt het aantal die punten die zich binnen de cirkel. Het programma reducer stelt punten die door de mappers geteld samen en vervolgens maakt een schatting van de waarde van pi van de formule 4R, R is waar de verhouding tussen het aantal punten binnen de cirkel op het totale aantal punten die binnen het kwadraat geteld.

```java
/**
* Licensed to the Apache Software Foundation (ASF) under one
* or more contributor license agreements. See the NOTICE file
* distributed with this work for additional information
* regarding copyright ownership. The ASF licenses this file
* to you under the Apache License, Version 2.0 (the
* "License"); you may not use this file except in compliance
* with the License. You may obtain a copy of the License at
*
* http://www.apache.org/licenses/LICENSE-2.0
*
* Unless required by applicable law or agreed to in writing, software
* distributed under the License is distributed on an "AS IS" BASIS,
* WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or     implied.
* See the License for the specific language governing permissions and
* limitations under the License.
*/

package org.apache.hadoop.examples;

import java.io.IOException;
import java.math.BigDecimal;
import java.util.Iterator;

import org.apache.hadoop.conf.Configured;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.BooleanWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.SequenceFile;
import org.apache.hadoop.io.Writable;
import org.apache.hadoop.io.WritableComparable;
import org.apache.hadoop.io.SequenceFile.CompressionType;
import org.apache.hadoop.mapred.FileInputFormat;
import org.apache.hadoop.mapred.FileOutputFormat;
import org.apache.hadoop.mapred.JobClient;
import org.apache.hadoop.mapred.JobConf;
import org.apache.hadoop.mapred.MapReduceBase;
import org.apache.hadoop.mapred.Mapper;
import org.apache.hadoop.mapred.OutputCollector;
import org.apache.hadoop.mapred.Reducer;
import org.apache.hadoop.mapred.Reporter;
import org.apache.hadoop.mapred.SequenceFileInputFormat;
import org.apache.hadoop.mapred.SequenceFileOutputFormat;
import org.apache.hadoop.util.Tool;
import org.apache.hadoop.util.ToolRunner;

//A Map-reduce program to estimate the value of Pi
//using quasi-Monte Carlo method.
//
//Mapper:
//Generate points in a unit square
//and then count points inside/outside of the inscribed circle of the square.
//
//Reducer:
//Accumulate points inside/outside results from the mappers.
//Let numTotal = numInside + numOutside.
//The fraction numInside/numTotal is a rational approximation of
//the value (Area of the circle)/(Area of the square),
//where the area of the inscribed circle is Pi/4
//and the area of unit square is 1.
//Then, Pi is estimated value to be 4(numInside/numTotal).
//

public class PiEstimator extends Configured implements Tool {
//tmp directory for input/output
static private final Path TMP_DIR = new Path(
PiEstimator.class.getSimpleName() + "_TMP_3_141592654");

//2-dimensional Halton sequence {H(i)},
//where H(i) is a 2-dimensional point and i >= 1 is the index.
//Halton sequence is used to generate sample points for Pi estimation.
private static class HaltonSequence {
// Bases
static final int[] P = {2, 3};
//Maximum number of digits allowed
static final int[] K = {63, 40};

private long index;
private double[] x;
private double[][] q;
private int[][] d;

//Initialize to H(startindex),
//so the sequence begins with H(startindex+1).
HaltonSequence(long startindex) {
index = startindex;
x = new double[K.length];
q = new double[K.length][];
d = new int[K.length][];
for(int i = 0; i < K.length; i++) {
q[i] = new double[K[i]];
d[i] = new int[K[i]];
}

for(int i = 0; i < K.length; i++) {
long k = index;
x[i] = 0;

for(int j = 0; j < K[i]; j++) {
q[i][j] = (j == 0? 1.0: q[i][j-1])/P[i];
d[i][j] = (int)(k % P[i]);
k = (k - d[i][j])/P[i];
x[i] += d[i][j] * q[i][j];
}
}
}

//Compute next point.
//Assume the current point is H(index).
//Compute H(index+1).
//@return a 2-dimensional point with coordinates in [0,1)^2
double[] nextPoint() {
index++;
for(int i = 0; i < K.length; i++) {
for(int j = 0; j < K[i]; j++) {
d[i][j]++;
x[i] += q[i][j];
if (d[i][j] < P[i]) {
break;
}
d[i][j] = 0;
x[i] -= (j == 0? 1.0: q[i][j-1]);
}
}
return x;
}
}

//Mapper class for Pi estimation.
//Generate points in a unit square and then
//count points inside/outside of the inscribed circle of the square.
public static class PiMapper extends MapReduceBase
implements Mapper<LongWritable, LongWritable, BooleanWritable, LongWritable> {

//Map method.
//@param offset samples starting from the (offset+1)th sample.
//@param size the number of samples for this map
//@param out output {ture->numInside, false->numOutside}
//@param reporter
public void map(LongWritable offset,
LongWritable size,
OutputCollector<BooleanWritable, LongWritable> out,
Reporter reporter) throws IOException {

final HaltonSequence haltonsequence = new HaltonSequence(offset.get());
long numInside = 0L;
long numOutside = 0L;

for(long i = 0; i < size.get(); ) {
//generate points in a unit square
final double[] point = haltonsequence.nextPoint();

//count points inside/outside of the inscribed circle of the square
final double x = point[0] - 0.5;
final double y = point[1] - 0.5;
if (x*x + y*y > 0.25) {
numOutside++;
} else {
numInside++;
}

//report status
i++;
if (i % 1000 == 0) {
reporter.setStatus("Generated " + i + " samples.");
}
}

//output map results
out.collect(new BooleanWritable(true), new LongWritable(numInside));
out.collect(new BooleanWritable(false), new LongWritable(numOutside));
}
}

//Reducer class for Pi estimation.
//Accumulate points inside/outside results from the mappers.
public static class PiReducer extends MapReduceBase
implements Reducer<BooleanWritable, LongWritable, WritableComparable<?>, Writable> {

private long numInside = 0;
private long numOutside = 0;
private JobConf conf; //configuration for accessing the file system

//Store job configuration.
@Override
public void configure(JobConf job) {
conf = job;
}

// Accumulate number of points inside/outside results from the mappers.
// @param isInside Is the points inside?
// @param values An iterator to a list of point counts
// @param output dummy, not used here.
// @param reporter

public void reduce(BooleanWritable isInside,
Iterator<LongWritable> values,
OutputCollector<WritableComparable<?>, Writable> output,
Reporter reporter) throws IOException {
if (isInside.get()) {
for(; values.hasNext(); numInside += values.next().get());
} else {
for(; values.hasNext(); numOutside += values.next().get());
}
}

//Reduce task done, write output to a file.
@Override
public void close() throws IOException {
//write output to a file
Path outDir = new Path(TMP_DIR, "out");
Path outFile = new Path(outDir, "reduce-out");
FileSystem fileSys = FileSystem.get(conf);
SequenceFile.Writer writer = SequenceFile.createWriter(fileSys, conf,
outFile, LongWritable.class, LongWritable.class,
CompressionType.NONE);
writer.append(new LongWritable(numInside), new LongWritable(numOutside));
writer.close();
}
}

//Run a map/reduce job for estimating Pi.
//@return the estimated value of Pi.
public static BigDecimal estimate(int numMaps, long numPoints, JobConf jobConf
)
throws IOException {
//setup job conf
jobConf.setJobName(PiEstimator.class.getSimpleName());

jobConf.setInputFormat(SequenceFileInputFormat.class);

jobConf.setOutputKeyClass(BooleanWritable.class);
jobConf.setOutputValueClass(LongWritable.class);
jobConf.setOutputFormat(SequenceFileOutputFormat.class);

jobConf.setMapperClass(PiMapper.class);
jobConf.setNumMapTasks(numMaps);

jobConf.setReducerClass(PiReducer.class);
jobConf.setNumReduceTasks(1);

// turn off speculative execution, because DFS doesn't handle
// multiple writers to the same file.
jobConf.setSpeculativeExecution(false);

//setup input/output directories
final Path inDir = new Path(TMP_DIR, "in");
final Path outDir = new Path(TMP_DIR, "out");
FileInputFormat.setInputPaths(jobConf, inDir);
FileOutputFormat.setOutputPath(jobConf, outDir);

final FileSystem fs = FileSystem.get(jobConf);
if (fs.exists(TMP_DIR)) {
throw new IOException("Tmp directory " + fs.makeQualified(TMP_DIR)
+ " already exists. Remove it first.");
}
if (!fs.mkdirs(inDir)) {
throw new IOException("Cannot create input directory " + inDir);
}

//generate an input file for each map task
try {
for(int i=0; i < numMaps; ++i) {
final Path file = new Path(inDir, "part"+i);
final LongWritable offset = new LongWritable(i * numPoints);
final LongWritable size = new LongWritable(numPoints);
final SequenceFile.Writer writer = SequenceFile.createWriter(
fs, jobConf, file,
LongWritable.class, LongWritable.class, CompressionType.NONE);
try {
writer.append(offset, size);
} finally {
writer.close();
}
System.out.println("Wrote input for Map #"+i);
}

//start a map/reduce job
System.out.println("Starting Job");
final long startTime = System.currentTimeMillis();
JobClient.runJob(jobConf);
final double duration = (System.currentTimeMillis() - startTime)/1000.0;
System.out.println("Job Finished in " + duration + " seconds");

//read outputs
Path inFile = new Path(outDir, "reduce-out");
LongWritable numInside = new LongWritable();
LongWritable numOutside = new LongWritable();
SequenceFile.Reader reader = new SequenceFile.Reader(fs, inFile, jobConf);
try {
reader.next(numInside, numOutside);
} finally {
reader.close();
}

//compute estimated value
return BigDecimal.valueOf(4).setScale(20)
.multiply(BigDecimal.valueOf(numInside.get()))
.divide(BigDecimal.valueOf(numMaps))
.divide(BigDecimal.valueOf(numPoints));
} finally {
fs.delete(TMP_DIR, true);
}
}

//Parse arguments and then runs a map/reduce job.
//Print output in standard out.
//@return a non-zero if there is an error. Otherwise, return 0.
public int run(String[] args) throws Exception {
if (args.length != 2) {
System.err.println("Usage: "+getClass().getName()+" <nMaps> <nSamples>");
ToolRunner.printGenericCommandUsage(System.err);
return -1;
}

final int nMaps = Integer.parseInt(args[0]);
final long nSamples = Long.parseLong(args[1]);

System.out.println("Number of Maps = " + nMaps);
System.out.println("Samples per Map = " + nSamples);

final JobConf jobConf = new JobConf(getConf(), getClass());
System.out.println("Estimated value of Pi is "
+ estimate(nMaps, nSamples, jobConf));
return 0;
}

//main method for running it as a stand alone command.
public static void main(String[] argv) throws Exception {
System.exit(ToolRunner.run(null, new PiEstimator(), argv));
}
}
```

## <a name="appendix-d---the-10gb-graysort-source-code"></a>Bijlage D - de broncode van 10gb graysort
De code voor het programma TeraSort MapReduce wordt voor inspectie in deze sectie weergegeven.

```java
/**
    * Licensed to the Apache Software Foundation (ASF) under one
    * or more contributor license agreements.  See the NOTICE file
    * distributed with this work for additional information
    * regarding copyright ownership.  The ASF licenses this file
    * to you under the Apache License, Version 2.0 (the
    * "License"); you may not use this file except in compliance
    * with the License.  You may obtain a copy of the License at
    *
    *     http://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed to in writing, software
    * distributed under the License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    * See the License for the specific language governing permissions and
    * limitations under the License.
    */

package org.apache.hadoop.examples.terasort;

import java.io.IOException;
import java.io.PrintStream;
import java.net.URI;
import java.util.ArrayList;
import java.util.List;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.apache.hadoop.conf.Configured;
import org.apache.hadoop.filecache.DistributedCache;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.NullWritable;
import org.apache.hadoop.io.SequenceFile;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapred.FileOutputFormat;
import org.apache.hadoop.mapred.JobClient;
import org.apache.hadoop.mapred.JobConf;
import org.apache.hadoop.mapred.Partitioner;
import org.apache.hadoop.util.Tool;
import org.apache.hadoop.util.ToolRunner;

/**
    * Generates the sampled split points, launches the job,
    * and waits for it to finish.
    * <p>
    * To run the program:
    * <b>bin/hadoop jar hadoop-examples-*.jar terasort in-dir out-dir</b>
    */

public class TeraSort extends Configured implements Tool {
    private static final Log LOG = LogFactory.getLog(TeraSort.class);

    /**
    * A partitioner that splits text keys into roughly equal
    * partitions in a global sorted order.
    */

    static class TotalOrderPartitioner implements Partitioner<Text,Text>{
    private TrieNode trie;
    private Text[] splitPoints;

    /**
        * A generic trie node
        */
    static abstract class TrieNode {
        private int level;
        TrieNode(int level) {
        this.level = level;
        }
        abstract int findPartition(Text key);
        abstract void print(PrintStream strm) throws IOException;
        int getLevel() {
        return level;
        }
    }

    /**
        * An inner trie node that contains 256 children based on the next
        * character.
        */
    static class InnerTrieNode extends TrieNode {
        private TrieNode[] child = new TrieNode[256];

        InnerTrieNode(int level) {
        super(level);
        }
        int findPartition(Text key) {
        int level = getLevel();
        if (key.getLength() <= level) {
            return child[0].findPartition(key);
        }
        return child[key.getBytes()[level]].findPartition(key);
        }
        void setChild(int idx, TrieNode child) {
        this.child[idx] = child;
        }
        void print(PrintStream strm) throws IOException {
        for(int ch=0; ch < 255; ++ch) {
            for(int i = 0; i < 2*getLevel(); ++i) {
            strm.print(' ');
            }
            strm.print(ch);
            strm.println(" ->");
            if (child[ch] != null) {
            child[ch].print(strm);
            }
        }
        }
    }

    /**
        * A leaf trie node that does string compares to figure out where the given
        * key belongs between lower..upper.
        */
    static class LeafTrieNode extends TrieNode {
        int lower;
        int upper;
        Text[] splitPoints;
        LeafTrieNode(int level, Text[] splitPoints, int lower, int upper) {
        super(level);
        this.splitPoints = splitPoints;
        this.lower = lower;
        this.upper = upper;
        }
        int findPartition(Text key) {
        for(int i=lower; i<upper; ++i) {
            if (splitPoints[i].compareTo(key) >= 0) {
            return i;
            }
        }
        return upper;
        }
        void print(PrintStream strm) throws IOException {
        for(int i = 0; i < 2*getLevel(); ++i) {
            strm.print(' ');
        }
        strm.print(lower);
        strm.print(", ");
        strm.println(upper);
        }
    }

    /**
        * Read the cut points from the given sequence file.
        * @param fs the file system
        * @param p the path to read
        * @param job the job config
        * @return the strings to split the partitions on
        * @throws IOException
        */
    private static Text[] readPartitions(FileSystem fs, Path p,
                                            JobConf job) throws IOException {
        SequenceFile.Reader reader = new SequenceFile.Reader(fs, p, job);
        List<Text> parts = new ArrayList<Text>();
        Text key = new Text();
        NullWritable value = NullWritable.get();
        while (reader.next(key, value)) {
        parts.add(key);
        key = new Text();
        }
        reader.close();
        return parts.toArray(new Text[parts.size()]);
    }

    /**
        * Given a sorted set of cut points, build a trie that will find the correct
        * partition quickly.
        * @param splits the list of cut points
        * @param lower the lower bound of partitions 0..numPartitions-1
        * @param upper the upper bound of partitions 0..numPartitions-1
        * @param prefix the prefix that we have already checked against
        * @param maxDepth the maximum depth we will build a trie for
        * @return the trie node that will divide the splits correctly
        */
    private static TrieNode buildTrie(Text[] splits, int lower, int upper,
                                        Text prefix, int maxDepth) {
        int depth = prefix.getLength();
        if (depth >= maxDepth || lower == upper) {
        return new LeafTrieNode(depth, splits, lower, upper);
        }
        InnerTrieNode result = new InnerTrieNode(depth);
        Text trial = new Text(prefix);
        // append an extra byte on to the prefix
        trial.append(new byte[1], 0, 1);
        int currentBound = lower;
        for(int ch = 0; ch < 255; ++ch) {
        trial.getBytes()[depth] = (byte) (ch + 1);
        lower = currentBound;
        while (currentBound < upper) {
            if (splits[currentBound].compareTo(trial) >= 0) {
            break;
            }
            currentBound += 1;
        }
        trial.getBytes()[depth] = (byte) ch;
        result.child[ch] = buildTrie(splits, lower, currentBound, trial,
                                        maxDepth);
        }
        // pick up the rest
        trial.getBytes()[depth] = 127;
        result.child[255] = buildTrie(splits, currentBound, upper, trial,
                                    maxDepth);
        return result;
    }

    public void configure(JobConf job) {
        try {
        FileSystem fs = FileSystem.getLocal(job);
        Path partFile = new Path(TeraInputFormat.PARTITION_FILENAME);
        splitPoints = readPartitions(fs, partFile, job);
        trie = buildTrie(splitPoints, 0, splitPoints.length, new Text(), 2);
        } catch (IOException ie) {
        throw new IllegalArgumentException("can't read paritions file", ie);
        }
    }

    public TotalOrderPartitioner() {
    }

    public int getPartition(Text key, Text value, int numPartitions) {
        return trie.findPartition(key);
    }

    }

    public int run(String[] args) throws Exception {
    LOG.info("starting");
    JobConf job = (JobConf) getConf();
    Path inputDir = new Path(args[0]);
    inputDir = inputDir.makeQualified(inputDir.getFileSystem(job));
    Path partitionFile = new Path(inputDir, TeraInputFormat.PARTITION_FILENAME);
    URI partitionUri = new URI(partitionFile.toString() +
                                "#" + TeraInputFormat.PARTITION_FILENAME);
    TeraInputFormat.setInputPaths(job, new Path(args[0]));
    FileOutputFormat.setOutputPath(job, new Path(args[1]));
    job.setJobName("TeraSort");
    job.setJarByClass(TeraSort.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(Text.class);
    job.setInputFormat(TeraInputFormat.class);
    job.setOutputFormat(TeraOutputFormat.class);
    job.setPartitionerClass(TotalOrderPartitioner.class);
    TeraInputFormat.writePartitionFile(job, partitionFile);
    DistributedCache.addCacheFile(partitionUri, job);
    DistributedCache.createSymlink(job);
    job.setInt("dfs.replication", 1);
    TeraOutputFormat.setFinalSync(job, true);
    JobClient.runJob(job);
    LOG.info("done");
    return 0;
    }

    /**
    * @param args
    */

    public static void main(String[] args) throws Exception {
    int res = ToolRunner.run(new JobConf(), new TeraSort(), args);
    System.exit(res);
    }
}
```

[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hadoop/submit-apache-hadoop-jobs-programmatically.md
[hdinsight-introduction]:hadoop/apache-hadoop-introduction.md

[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md

[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-sample-10gb-graysort]: #hdinsight-sample-10gb-graysort
[hdinsight-sample-csharp-streaming]: #hdinsight-sample-csharp-streaming
[hdinsight-sample-pi-estimator]: #hdinsight-sample-pi-estimator
[hdinsight-sample-wordcount]: #hdinsight-sample-wordcount

[hdinsight-use-hive]: hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]: hadoop/hdinsight-use-pig.md

[streamreader]: http://msdn.microsoft.com/library/system.io.streamreader.aspx
[console-writeline]: http://msdn.microsoft.com/library/system.console.writeline
[stdin-stdout-stderr]: https://msdn.microsoft.com/library/3x292kth.aspx
