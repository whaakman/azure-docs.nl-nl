---
title: Genereren van aanbevelingen met Mahout HDInsight vanuit PowerShell - Azure
description: Informatie over het gebruik van de Apache Mahout-machine learning-bibliotheek Genereer filmaanbevelingen met HDInsight (Hadoop) vanuit een PowerShell-script uitgevoerd op de client.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: jasonh
ms.openlocfilehash: 587ea8d9082a696853d8e25a36d9536c762d0582
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39599986"
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight-powershell"></a>Filmaanbevelingen genereren met behulp van Apache Mahout met Hadoop in HDInsight (PowerShell)

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Meer informatie over het gebruik van de [Apache Mahout](http://mahout.apache.org) machine learning-bibliotheek met Azure HDInsight filmaanbevelingen genereren. Het voorbeeld in dit document wordt Azure PowerShell Mahout taken uit te voeren.

## <a name="prerequisites"></a>Vereisten

* Een Linux gebaseerde HDInsight-cluster. Zie voor meer informatie over het maken van een [aan de slag met Hadoop op basis van Linux in HDInsight][getstarted].

    > [!IMPORTANT]
    > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* [Azure PowerShell](/powershell/azure/overview)

## <a name="recommendations"></a>Aanbevelingen genereren met behulp van Azure PowerShell

> [!WARNING]
> De taak in deze sectie werkt met behulp van Azure PowerShell. Veel van de klassen die zijn opgegeven met Mahout werkt momenteel niet met Azure PowerShell. Zie voor een lijst met klassen die niet met Azure PowerShell werken, de [probleemoplossing](#troubleshooting) sectie.
>
> Zie voor een voorbeeld van het gebruik van SSH verbinding maken met HDInsight en voer Mahout voorbeelden rechtstreeks op het cluster, [filmaanbevelingen genereren met Mahout en HDInsight (SSH)](hadoop/apache-hadoop-mahout-linux-mac.md).

Een van de functies die wordt geleverd door Mahout is een engine voor aanbevelingen. Deze engine accepteert gegevens in de indeling van `userID`, `itemId`, en `prefValue` (gebruikers van de voorkeur voor het item). Mahout gebruikt de gegevens om te bepalen van gebruikers met een dergelijke-item voorkeuren, die kunnen worden gebruikt om aanbevelingen te doen.

Het volgende voorbeeld wordt een eenvoudig overzicht van de werking van het proces van de aanbevelingen:

* **CO exemplaar**: Jaap Els en Bob alle beviel *Star Wars*, *terug ramp in de Empire*, en *rendement van de Jedi*. Mahout bepaalt dat gebruikers die ook een van deze films, zoals de andere twee.

* **CO exemplaar**: Bob en Els ook beviel *de Phantom Menace*, *een aanval van het klonen*, en *Revenge van de Sith*. Mahout bepaalt dat gebruikers die de vorige drie films ook leuk vinden, zoals deze films.

* **Gelijkenis aanbeveling**: omdat Jaap leuk vinden van de eerste drie films, Mahout kijkt naar films die anderen met vergelijkbare voorkeuren leuk vinden, maar Jaap niet heeft bekeken (leuk vinden/geclassificeerd). In dit geval Mahout raadt *de Phantom Menace*, *een aanval van het klonen*, en *Revenge van de Sith*.

### <a name="understanding-the-data"></a>Wat zijn de gegevens?

[GroupLens onderzoek] [ movielens] classificatie gegevens biedt voor films in een indeling die compatibel is met Mahout. Deze gegevens zijn beschikbaar op de standaardopslag voor uw cluster op `/HdiSamples/HdiSamples/MahoutMovieData`.

Er zijn twee bestanden `moviedb.txt` (informatie over de films) en `user-ratings.txt`. De `user-ratings.txt` -bestand wordt gebruikt tijdens de analyse. De `moviedb.txt` bestand wordt gebruikt voor beschrijvende tekst bij het weergeven van de resultaten van de analyse.

De gegevens in gebruiker ratings.txt heeft een structuur van `userID`, `movieID`, `userRating`, en `timestamp`, waarin staat hoe maximaal elke gebruiker een film beoordeeld. Hier volgt een voorbeeld van de gegevens:

    196    242    3    881250949
    186    302    3    891717742
    22     377    1    878887116
    244    51     2    880606923
    166    346    1    886397596

### <a name="run-the-job"></a>De taak uitvoeren

Gebruik de volgende Windows PowerShell-script om uit te voeren van een taak die de Mahout-aanbevelingsengine met de filmgegevens gebruikt:

> [!NOTE]
> Dit bestand vraagt u om de informatie die wordt gebruikt voor verbinding maken met uw HDInsight-cluster en taken uitvoeren. Het kan enkele minuten voor de taken te voltooien en downloadt u het bestand uitvoer.txt duren.

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=5-98)]

> [!NOTE]
> Mahout taken verwijderen tijdelijke gegevens die zijn gemaakt tijdens het verwerken van de taak niet. De `--tempDir` parameter is opgegeven in het voorbeeld van de taak voor het isoleren van de tijdelijke bestanden naar een specifieke map.

De Mahout-taak terug niet de uitvoer naar de STDOUT. In plaats daarvan wordt deze opgeslagen in de map met de opgegeven uitvoer als **onderdeel-r-00000**. Het script downloaden van dit bestand **uitvoer.txt** in de huidige map op uw werkstation.

De volgende tekst is een voorbeeld van de inhoud van dit bestand:

    1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

De eerste kolom is de `userID`. De waarden die zijn opgenomen in ' [' en ']' zijn `movieId`:`recommendationScore`.

Het script kan ook worden gedownload de `moviedb.txt` en `user-ratings.txt` bestanden, die nodig zijn om de uitvoer zodat deze beter leesbaar te delen.

### <a name="view-the-output"></a>De uitvoer weergeven

Hoewel de gegenereerde uitvoer mogelijk OK voor gebruik in een toepassing, is het niet gebruiksvriendelijk. De `moviedb.txt` van de server kan worden gebruikt om op te lossen de `movieId` naar een film-naam. Gebruik de volgende PowerShell-script om aanbevelingen met de namen van de film weer te geven:

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=106-180)]

Gebruik de volgende opdracht om weer te geven van de aanbevelingen in een gebruiksvriendelijke indeling: 

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

Mahout taken verwijder dan niet tijdelijke bestanden die zijn gemaakt tijdens de verwerking. Bovendien overschrijven de taken niet bestaande uitvoerbestand.

Om fouten te voorkomen bij het uitvoeren van taken Mahout, tijdelijke en uitvoer bestanden tussen elke uitvoering te verwijderen. Als u wilt verwijderen van de bestanden die door de eerdere scripts in dit document is gemaakt, gebruikt u de volgende PowerShell-script:

```powershell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzureRmAccount
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

Mahout-taken die gebruikmaken van de volgende klassen retourneren verschillende foutberichten bij gebruik van Windows PowerShell:

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

Taken uit te voeren die gebruikmaken van deze klassen, verbinding maken met het HDInsight-cluster via SSH en de taken worden uitgevoerd vanaf de opdrachtregel. Zie voor een voorbeeld van het gebruik van SSH Mahout taken uit te voeren, [filmaanbevelingen genereren met Mahout en HDInsight (SSH)](hadoop/apache-hadoop-mahout-linux-mac.md).

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u een Mahout, Ontdek andere manieren van het werken met gegevens in HDInsight:

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
