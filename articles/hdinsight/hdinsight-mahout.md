---
title: Genereren met Mahout HDInsight vanuit PowerShell - Azure aanbevelingen | Microsoft Docs
description: Informatie over het gebruik van de Apache Mahout-machine learning-bibliotheek voor het genereren van filmaanbevelingen met HDInsight (Hadoop) vanuit een PowerShell-script uitgevoerd op de client.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 07b57208-32aa-4e59-900a-6c934fa1b7a7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2018
ms.author: larryfr
ms.openlocfilehash: a2477b0a7c9c76f8ce4a183f4d699ddf4c291023
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight-powershell"></a>Filmaanbevelingen genereren met behulp van Apache Mahout met Hadoop in HDInsight (PowerShell)

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Meer informatie over het gebruik van de [Apache Mahout](http://mahout.apache.org) machine learning-bibliotheek met Azure HDInsight filmaanbevelingen genereren. Het voorbeeld in dit document wordt Azure PowerShell Mahout taken uitvoeren.

## <a name="prerequisites"></a>Vereisten

* Een Linux gebaseerde HDInsight-cluster. Zie voor meer informatie over het maken van een [aan de slag met Hadoop op basis van Linux in HDInsight][getstarted].

    > [!IMPORTANT]
    > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* [Azure PowerShell](/powershell/azure/overview)

## <a name="recommendations"></a>Aanbevelingen genereren met behulp van Azure PowerShell

> [!WARNING]
> De taak in deze sectie werkt met behulp van Azure PowerShell. Veel van de klassen die zijn opgegeven met Mahout werkt momenteel niet met Azure PowerShell. Zie voor een lijst met klassen die niet met Azure PowerShell werken, de [probleemoplossing](#troubleshooting) sectie.
>
> Zie voor een voorbeeld van het gebruik van SSH verbinding maken met HDInsight en voer Mahout voorbeelden rechtstreeks op het cluster [filmaanbevelingen genereren met Mahout en HDInsight (SSH)](hadoop/apache-hadoop-mahout-linux-mac.md).

Een van de functies die wordt geleverd door Mahout is een aanbeveling-engine. Deze engine accepteert gegevens in de indeling van `userID`, `itemId`, en `prefValue` (gebruikers van de voorkeur voor het item). Mahout gebruikt de gegevens om te bepalen van gebruikers met dergelijke-item voorkeuren, die kunnen worden gebruikt om aanbevelingen te doen.

Het volgende voorbeeld is een vereenvoudigde procedure van de werking van het proces aanbeveling:

* **mede exemplaar**: Jan Els en Bob alle bevallen *Star Wars*, *terug ontvangen in de Empire*, en *Return van de Jedi*. Mahout bepaalt dat gebruikers die een van deze films ook de andere twee zoals.

* **mede exemplaar**: Bob en Els ook bevallen *de Phantom Menace*, *een aanval van de klonen*, en *Revenge van de Sith*. Mahout bepaalt dat gebruikers die de vorige drie films ook bevallen, zoals deze films.

* **Gelijkenis aanbeveling**: Jan omdat de eerste drie films bevallen, Mahout wordt bekeken films die anderen met vergelijkbare voorkeuren bevallen, maar Joe heeft geen gevolgde (bevallen/geclassificeerd). In dit geval Mahout raadt *de Phantom Menace*, *een aanval van de klonen*, en *Revenge van de Sith*.

### <a name="understanding-the-data"></a>Wat zijn de gegevens?

[GroupLens Research] [ movielens] biedt classificatie gegevens voor films in een indeling die compatibel is met Mahout. Deze gegevens zijn beschikbaar op de standaard-opslag voor uw cluster op `/HdiSamples/HdiSamples/MahoutMovieData`.

Er zijn twee bestanden: `moviedb.txt` (informatie over de films) en `user-ratings.txt`. De `user-ratings.txt` -bestand wordt gebruikt tijdens de analyse. De `moviedb.txt` bestand wordt gebruikt voor de beschrijvende tekst opgeven bij het weergeven van de resultaten van de analyse.

De gegevens in gebruiker ratings.txt heeft een structuur van `userID`, `movieID`, `userRating`, en `timestamp`, waarmee wordt uitgelegd hoe maximaal elke gebruiker een film beoordeeld. Hier volgt een voorbeeld van de gegevens:

    196    242    3    881250949
    186    302    3    891717742
    22     377    1    878887116
    244    51     2    880606923
    166    346    1    886397596

### <a name="run-the-job"></a>Voer de taak

Gebruik de volgende Windows PowerShell-script een taak die de Mahout aanbeveling-engine met de filmgegevens gebruikt uit te voeren:

> [!NOTE]
> Dit bestand wordt u gevraagd om informatie die wordt gebruikt voor verbinding maken met uw HDInsight-cluster en taken uitvoeren. Het kan enkele minuten duren voordat de taken zijn voltooid en download het bestand uitvoer.txt.

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=5-98)]

> [!NOTE]
> Mahout taken verwijderd tijdelijke gegevens die zijn gemaakt tijdens het verwerken van de taak niet. De `--tempDir` parameter wordt opgegeven in de voorbeeld-taak voor het isoleren van de tijdelijke bestanden naar een specifieke map.

De taak Mahout weer niet de uitvoer STDOUT. In plaats daarvan wordt deze opgeslagen in de opgegeven uitvoermap als **onderdeel-r-00000**. Het script downloaden van dit bestand **uitvoer.txt** in de huidige map op uw werkstation.

De volgende tekst is een voorbeeld van de inhoud van dit bestand:

    1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

De eerste kolom is de `userID`. De waarden in ' [' en ']' zijn `movieId`:`recommendationScore`.

Het script ook downloadt de `moviedb.txt` en `user-ratings.txt` bestanden die nodig zijn om de uitvoer zodat deze beter leesbaar te formatteren.

### <a name="view-the-output"></a>De uitvoer weergeven

Hoewel de gegenereerde uitvoer mogelijk OK voor gebruik in een toepassing, is het niet gebruiksvriendelijke. De `moviedb.txt` van de server kan worden gebruikt om op te lossen de `movieId` op de naam van een film. Gebruik de volgende PowerShell-script om aanbevelingen met film namen weer te geven:

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=106-180)]

Gebruik de volgende opdracht om de aanbevelingen in een gebruiksvriendelijke indeling weer te geven: 

```powershell
.\show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt
```

De uitvoer lijkt op het volgende:

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, The (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of the Dove, The (1997)            4.6666665
    People vs. Larry Flynt, The (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="cannot-overwrite-files"></a>Bestanden kan niet worden overschreven.

Mahout taken komen niet opruimen van tijdelijke bestanden die zijn gemaakt tijdens de verwerking. Bovendien overschrijven de taken niet bestaande uitvoerbestand.

Om fouten te voorkomen wanneer Mahout taken wordt uitgevoerd, verwijdert u tijdelijke- en uitvoergegevens bestanden tussen wordt uitgevoerd. Gebruik de volgende PowerShell-script voor het verwijderen van de bestanden die zijn gemaakt door de eerdere scripts in dit document:

```powershell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$creds=Get-Credential -Message "Enter the login for the cluster"

#Get the cluster info so we can get the resource group, storage, etc.
$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
$container = $clusterInfo.DefaultStorageContainer
$storageAccountKey = (Get-AzureRmStorageAccountKey `
    -Name $storageAccountName `
-ResourceGroupName $resourceGroup)[0].Value

#Create a storage context and upload the file
$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey $storageAccountKey

#Azure PowerShell can't delete blobs using wildcard,
#so have to get a list and delete one at a time
# Start with the output
$blobs = Get-AzureStorageBlob -Container $container -Context $context -Prefix "example/out"
foreach($blob in $blobs)
{
    Remove-AzureStorageBlob -Blob $blob.Name -Container $container -context $context
}
# Next the temp files
$blobs = Get-AzureStorageBlob -Container $container -Context $context -Prefix "example/temp"
foreach($blob in $blobs)
{
    Remove-AzureStorageBlob -Blob $blob.Name -Container $container -context $context
}
```

### <a name="nopowershell"></a>Klassen die niet met Azure PowerShell werken

Mahout-functies die gebruikmaken van de volgende klassen retourneren verschillende foutberichten bij gebruik van Windows PowerShell:

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

Als u wilt uitvoeren op functies die gebruikmaken van deze klassen, verbinding met het HDInsight-cluster via SSH en de jobs uitvoeren vanaf de opdrachtregel. Zie voor een voorbeeld van SSH met Mahout taken uitvoert, [filmaanbevelingen genereren met Mahout en HDInsight (SSH)](hadoop/apache-hadoop-mahout-linux-mac.md).

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u Mahout, detectie van andere manieren van het werken met gegevens in HDInsight:

* [Hive met HDInsight](hadoop/hdinsight-use-hive.md)
* [Pig met HDInsight](hadoop/hdinsight-use-pig.md)
* [MapReduce met HDInsight](hadoop/hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: /powershell/azureps-cmdlets-docs
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
