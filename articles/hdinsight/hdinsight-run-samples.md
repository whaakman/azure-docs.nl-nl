---
title: De voorbeelden van Hadoop in HDInsight - Azure uitvoeren
description: Aan de slag met de Azure HDInsight-service met de opgegeven voorbeelden. Gebruik PowerShell-scripts die MapReduce-programma's op gegevens clusters worden uitgevoerd.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 0a174c3de33b01f936eec599c1de68e2cebbf9c5
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55820416"
---
# <a name="run-mapreduce-samples-in-windows-based-hdinsight"></a>Voorbeelden van MapReduce in HDInsight op basis van Windows worden uitgevoerd
[!INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Een reeks voorbeelden wordt waarmee u kunt halen gestart actieve MapReduce-taken op Apache Hadoop-clusters met behulp van Azure HDInsight geleverd. Deze voorbeelden zijn beschikbaar op elk van de beheerde HDInsight-clusters die u maakt. Deze voorbeelden wordt uitgevoerd, raakt u vertrouwd met het gebruik van Azure PowerShell-cmdlets uitvoeren op Hadoop-clusters.

* [**Aantal woorden**][hdinsight-sample-wordcount]: Woorden worden geteld in een tekstbestand.
* [**C#aantal woorden streaming**][hdinsight-sample-csharp-streaming]: Woorden worden geteld in een tekstbestand met de Hadoop-streaming-interface.
* [**PI estimator**][hdinsight-sample-pi-estimator]: Maakt gebruik van een statistische (quasi Monte Carlo) methode voor het schatten van de waarde van pi.
* [**10 GB Graysort**][hdinsight-sample-10gb-graysort]: Een algemene GraySort uitvoert op een 10 GB-bestand met behulp van HDInsight. Er zijn drie taken om uit te voeren: Teragen voor het genereren van de gegevens, Terasort om de gegevens te sorteren, en Teravalidate om te bevestigen dat de gegevens correct zijn gesorteerd.

> [!NOTE]  
> De broncode kan worden gevonden in de bijlage.

Veel aanvullende documentatie bestaat op het web voor Hadoop-gerelateerde technologieën, zoals op basis van Java MapReduce-programma's en streaming en documentatie over de cmdlets die worden gebruikt in Windows PowerShell-scripts. Zie voor meer informatie over deze resources:

* [Java MapReduce-programma's ontwikkelen voor Apache Hadoop in HDInsight](hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md)
* [Apache Hadoop-taken in HDInsight](hadoop/submit-apache-hadoop-jobs-programmatically.md)
* [Inleiding tot Azure HDInsight][hdinsight-introduction]

Veel mensen kiezen tegenwoordig, Apache Hive en Apache Pig via MapReduce.  Zie voor meer informatie:

* [Apache Hive in HDInsight gebruiken](hadoop/hdinsight-use-hive.md)
* [Apache Pig in HDInsight gebruiken](hadoop/hdinsight-use-pig.md)

**Vereisten**:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Een HDInsight-cluster**. Zie voor meer informatie over de verschillende manieren waarop deze clusters kunnen worden gemaakt, [Apache Hadoop-clusters maken in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
* **Een werkstation met Azure PowerShell**.

    > [!IMPORTANT]  
    > Azure PowerShell-ondersteuning voor het beheer van HDInsight-resources met behulp van Azure Service Manager is **afgeschaft** en verdwijnt per 1 januari 2017. In de stappen in dit document worden de nieuwe HDInsight-cmdlets gebruikt die met Azure Resource Manager werken.
    >
    > Volg de stappen in [Azure PowerShell installeren en configureren](/powershell/azureps-cmdlets-docs) voor het installeren van de meest recente versie van Azure PowerShell. Als u scripts hebt die worden gewijzigd moeten voor het gebruik van de nieuwe cmdlets die met werken Azure Resource Manager, Zie [migreren naar Azure Resource Manager gebaseerde ontwikkelingsprogramma's voor HDInsight-clusters](hdinsight-hadoop-development-using-azure-resource-manager.md).

## <a name="hdinsight-sample-wordcount"></a>Word count - Java
Als u wilt een MapReduce-project verzenden, moet u eerst de definitie van een MapReduce-taak maken. In de taakdefinitie, geeft u het jar-bestand van de MapReduce-programma en de locatie van de jar-bestand is **wasb:///example/jars/hadoop-mapreduce-examples.jar**, naam van de klasse en de argumenten.  Het wordcount MapReduce-programma neemt twee argumenten: de bronbestand dat wordt gebruikt voor het tellen van woorden en de locatie voor de uitvoer.

De broncode kan worden gevonden in de bijlage A.

Voor de procedure van het ontwikkelen van een Java MapReduce-programma, Zie - [ontwikkel Java MapReduce-programma's voor Apache Hadoop in HDInsight](hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md)

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

    De MapReduce-taak produceert een bestand met de naam *onderdeel-r-00000*, die het aantal fouten en woorden bevat. Het script gebruikt de **findstr** opdracht om een lijst van alle woorden met *"er"*.
3. De eerste drie variabelen worden ingesteld en voer het script.

## <a name="hdinsight-sample-csharp-streaming"></a>Word count - C# streaming
Hadoop biedt een streaming-API voor MapReduce, waarmee u kaart schrijven en functies in een andere taal dan Java verlagen.

> [!NOTE]  
> De stappen in deze zelfstudie alleen van toepassing op Windows gebaseerde HDInsight-clusters. Zie voor een voorbeeld van streaming voor Linux gebaseerde HDInsight-clusters, [ontwikkelen streaming Python-programma's voor HDInsight](hadoop/apache-hadoop-streaming-python.md).

In het voorbeeld wordt het toewijzen van de en de reducer zijn uitvoerbare bestanden die de invoer van lezen [stdin] [ stdin-stdout-stderr] (regel voor regel) en hoe de uitvoer naar [stdout] [ stdin-stdout-stderr]. Het programma telt de woorden in de tekst.

Als een uitvoerbaar bestand is opgegeven voor **mappers**, elke taak toewijzen worden het uitvoerbare bestand als een afzonderlijk proces gestart wanneer de toewijzing is geïnitialiseerd. Terwijl de mapper-taak wordt uitgevoerd, zet u de invoer in regels en -feeds van de regels die moeten worden de [stdin] [ stdin-stdout-stderr] van het proces.

Het toewijzen van de verzamelt in de tussentijd de regel-georiënteerde uitvoer van de stdout van het proces. Elke regel wordt geconverteerd naar een sleutel/waarde-paar, die worden verzameld als de uitvoer van het toewijzen van de. Standaard is de sleutel van het voorvoegsel van een regel tot aan het eerste tabblad teken en is de waarde van de rest van de regel (met uitzondering van het tabblad-teken). Als er geen tabteken in de regel, volledige regel wordt beschouwd als de sleutel en de waarde null is.

Als een uitvoerbaar bestand is opgegeven voor **reducers tegelijkertijd**, elke taak reducer wordt het uitvoerbare bestand als een afzonderlijk proces gestart wanneer de reducer is geïnitialiseerd. Als de reducer-taak wordt uitgevoerd, wordt de sleutel/invoerwaarden paren in regels omgezet, en deze feeds, de regels die moeten worden de [stdin] [ stdin-stdout-stderr] van het proces.

In de tussentijd de reducer verzameld voor de regel-georiënteerde uitvoer van de [stdout] [ stdin-stdout-stderr] van het proces. Elke regel wordt geconverteerd naar een sleutel/waarde-paar, die worden verzameld als de uitvoer van de reducer. Standaard is de sleutel van het voorvoegsel van een regel tot aan het eerste tabblad teken en is de waarde van de rest van de regel (met uitzondering van het tabblad-teken).

**Om in te dienen een word-count streamingtaak C#**

* Volg de procedure in Word-count - Java, en de taakdefinitie vervangen door de volgende regel:

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
Het pi estimator maakt gebruik van een statistische (quasi Monte Carlo) methode voor het schatten van de waarde van pi. Punten geplaatst in willekeurige volgorde binnen een eenheid vierkante ook binnen een cirkel aangebracht in een vierkant met een kans die gelijk is aan het gebied van de cirkel, vallen pi/4. De waarde van pi kan worden geschat van de waarde van 4R, waarop R is de verhouding van het aantal punten die in de cirkel in op het totale aantal punten die zich binnen het vierkant. Hoe groter het voorbeeld van de punten die wordt gebruikt, hoe beter de schatting wordt weergegeven.

Het script dat is opgegeven voor dit voorbeeld verzendt een Hadoop-jar-taak en is ingesteld maximaal 16 kaarten, die is vereist voor het berekenen van 10 miljoen voorbeeldpunten door de parameterwaarden met een waarde worden uitgevoerd. De parameterwaarden van deze kunnen worden gewijzigd voor het verbeteren van de geschatte waarde van pi. Ter referentie zijn de eerste 10 decimalen van pi 3.1415926535.

**Een pi estimator-taak verzenden**

* Volg de procedure in Word-count - Java, en de taakdefinitie vervangen door de volgende regel:

    ```powershell
    $mrJobJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "pi" `
                                -Arguments "16", "10000000"
    ```

## <a name="hdinsight-sample-10gb-graysort"></a>10 GB Graysort
In dit voorbeeld maakt gebruik van een gemiddelde hoeveelheden 10 GB aan gegevens zodat deze kan relatief snel kan worden uitgevoerd. Maakt gebruik van de MapReduce-toepassingen die zijn ontwikkeld door Owen O'Malley en Arun Murthy die de jaarlijkse voor algemeen gebruik ("daytona") terabyte sorteren benchmark in 2009 met een snelheid van 0.578 TB/min (100 TB in 173 minuten gewonnen). Zie voor meer informatie over deze en andere sorteren benchmarks de [Sortbenchmark](https://sortbenchmark.org/) site.

In dit voorbeeld maakt gebruik van drie sets MapReduce-programma's:

1. **TeraGen** is een MapReduce-programma dat u gebruiken kunt voor het genereren van de rijen met gegevens om te sorteren.
2. **TeraSort** -voorbeelden voor de invoergegevens en MapReduce gebruikt voor het sorteren van de gegevens in een totale volgorde. TeraSort is een standaard sorteren van MapReduce-functies, met uitzondering van een aangepaste partitioner die gebruikmaakt van een gesorteerde lijst met sleutels voor N-1 steekproef die het belangrijkste bereik voor elke verminderen definiëren. In het bijzonder, alle sleutels dergelijke die voorbeeld [i-1] < sleutel = < voorbeeld [i] om te beperken i worden verzonden. Dit garandeert dat de uitvoer van i beperken zijn alle kleiner is dan de uitvoer van verminderen i + 1.
3. **TeraValidate** is een MapReduce-programma die valideert dat de uitvoer wereldwijd is gesorteerd. Een kaart per bestand in de uitvoermap wordt gemaakt en elke kaart zorgt ervoor dat elke sleutel kleiner dan of gelijk zijn aan het vorige voorbeeld is. De functie van de kaart ook records van de eerste en laatste sleutels van elk bestand gegenereerd en de functie verminderen zorgt ervoor dat de eerste sleutel van i-bestand groter dan de laatste sleutel van het bestand i-1 is. Geen problemen worden gerapporteerd als een uitvoer van de verminderen met de sleutels die niet de juiste volgorde zijn.

De invoer- en -indeling, die worden gebruikt door alle drie de toepassingen, leest en schrijft de tekstbestanden in de juiste indeling. De uitvoer van de verminderen heeft replicatie ingesteld op 1, in plaats van de standaard 3, omdat de benchmark-wedstrijd niet vereist dat de uitvoergegevens worden gerepliceerd op meerdere knooppunten.

Drie taken die vereist zijn voor het voorbeeld, elke overeenkomt met een van de MapReduce-programma's die worden beschreven in de inleiding:

1. Genereren van de gegevens voor het sorteren door te voeren de **TeraGen** MapReduce-taak.
2. Sorteer de gegevens door het uitvoeren van de **TeraSort** MapReduce-taak.
3. Bevestig dat de gegevens correct is gesorteerd door het uitvoeren van de **TeraValidate** MapReduce-taak.

**De taken indienen**

* Volg de procedure in Word-count - Java, en gebruik de volgende taakdefinities:

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
In dit artikel en de artikelen in elk van de voorbeelden, hebt u geleerd hoe u de opgenomen met de HDInsight-clusters met behulp van Azure PowerShell-voorbeelden uitvoeren. Zie de volgende onderwerpen voor zelfstudies over Pig, Hive en MapReduce gebruiken met HDInsight:

* [Aan de slag met behulp van Apache Hadoop voor het analyseren van de mobiele telefoon gebruiken met Apache Hive in HDInsight][hdinsight-get-started]
* [Apache Pig gebruiken met Apache Hadoop op HDInsight][hdinsight-use-pig]
* [Apache Hive gebruiken met Apache Hadoop op HDInsight][hdinsight-use-hive]
* [Apache Hadoop-taken in HDInsight][hdinsight-submit-jobs]

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

## <a name="appendix-b---the-word-count-streaming-source-code"></a>Bijlage B – het aantal woorden broncode streaming
De MapReduce-programma maakt gebruik van de toepassing cat.exe als een toewijzing-interface het streamen van de tekst in de console en de toepassing wc.exe als de interface Verminder het aantal woorden die worden gestreamd vanaf een document telt. De mapper- en reducer tekens, regel voor regel lezen van de standaard (stdin) in de invoerstroom en schrijven naar de standaard uitvoerstroom (stdout).

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

De mapper-code in het bestand cat.cs gebruikt een [StreamReader] [ streamreader] object om de tekens van de inkomende gegevensstroom naar de console, die vervolgens de stroom naar de standaard uitvoerstroom met de statische schrijfttelezen[ Console.Writeline] [ console-writeline] methode.

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

De code reducer in het bestand wc.cs gebruikt een [StreamReader] [ streamreader] object tekens uit de standard invoerstroom die zijn uitgevoerd door het toewijzen van de cat.exe lezen. Als het ingesteld op de tekens met de [Console.Writeline] [ console-writeline] methode, wordt de woorden geteld door spaties en einde van regel tekens aan het einde van elk woord tellen. Vervolgens worden het totale aantal geschreven naar de standaard uitvoerstroom met de [Console.Writeline] [ console-writeline] methode.

## <a name="appendix-c---the-pi-estimator-source-code"></a>Bijlage C - het Pi estimator-broncode
Het pi estimator Java-code met de functies toewijzen en reducer is beschikbaar voor inspectie hieronder. De mapper-programma genereert een opgegeven aantal punten in willekeurige volgorde binnen een vierkant eenheid geplaatst en vervolgens telt het aantal deze punten die in de cirkel. Het programma reducer stelt punten geteld op basis van de mappers samen en maakt vervolgens een schatting van de waarde van pi van de formule 4R, waarop R is de verhouding van het aantal punten in de cirkel op het totale aantal punten die zich binnen het vierkant wordt geteld.

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
* https://www.apache.org/licenses/LICENSE-2.0
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
//@param out output {true->numInside, false->numOutside}
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
De code voor de TeraSort MapReduce-programma wordt voor inspectie in deze sectie weergegeven.

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
    *     https://www.apache.org/licenses/LICENSE-2.0
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
        throw new IllegalArgumentException("can't read partitions file", ie);
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

[streamreader]: https://msdn.microsoft.com/library/system.io.streamreader.aspx
[console-writeline]: https://msdn.microsoft.com/library/system.console.writeline
[stdin-stdout-stderr]: https://msdn.microsoft.com/library/3x292kth.aspx
