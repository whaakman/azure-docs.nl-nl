---
title: MapReduce en PowerShell gebruiken met Apache Hadoop - Azure HDInsight
description: Informatie over het gebruik van PowerShell op afstand MapReduce-taken uitvoeren met Apache Hadoop op HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: hrasheed
ms.openlocfilehash: 753a0ad72e1d4b60a93daa570ceecc25d21bb228
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634138"
---
# <a name="run-mapreduce-jobs-with-apache-hadoop-on-hdinsight-using-powershell"></a>MapReduce-taken uitvoeren met Apache Hadoop op HDInsight met behulp van PowerShell

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Dit document bevat een voorbeeld van het gebruik van Azure PowerShell een MapReduce-taak uitvoeren in een Hadoop op HDInsight-cluster.

## <a id="prereq"></a>Vereisten

* **Een Azure HDInsight (Hadoop op HDInsight)-cluster**

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* **Een werkstation met Azure PowerShell**.

## <a id="powershell"></a>Voer een MapReduce-taak

Azure PowerShell biedt *cmdlets* waarmee u op afstand MapReduce-taken uitvoeren op HDInsight. Intern, kunt u PowerShell REST-aanroepen naar [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (voorheen Templeton) uitgevoerd op het HDInsight-cluster.

De volgende cmdlets worden gebruikt bij het uitvoeren van MapReduce-taken in een extern HDInsight-cluster.

* **Connect-AzureRmAccount**: Azure PowerShell wordt geverifieerd met uw Azure-abonnement.

* **Nieuwe AzureRmHDInsightMapReduceJobDefinition**: maakt u een nieuwe *taak definitie* met behulp van de opgegeven MapReduce-informatie.

* **Start-AzureRmHDInsightJob**: de taakdefinitie verzendt naar HDInsight en wordt de taak wordt gestart. Een *taak* object wordt geretourneerd.

* **Wacht AzureRmHDInsightJob**: maakt gebruik van het taakobject om de status van de taak te controleren. Wacht totdat de taak is voltooid of de wachttijd is overschreden.

* **Get-AzureRmHDInsightJobOutput**: gebruikt voor het ophalen van de uitvoer van de taak.

De volgende stappen laten zien hoe u deze cmdlets gebruikt voor het uitvoeren van een taak in uw HDInsight-cluster.

1. Met behulp van een editor, slaat u de volgende code als **mapreducejob.ps1**.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-mapreduce/use-mapreduce.ps1?range=5-69)]

2. Open een nieuw **Azure PowerShell** opdrachtprompt. Wijzig de mappen in de locatie van de **mapreducejob.ps1** bestand en gebruik vervolgens de volgende opdracht uit het script uit te voeren:

        .\mapreducejob.ps1

    Wanneer u het script uitvoert, wordt u gevraagd om de naam van het HDInsight-cluster en de aanmelding bij cluster. U kunt ook gevraagd om te verifiëren bij uw Azure-abonnement.

3. Wanneer de taak is voltooid, ontvangt u uitvoer die vergelijkbaar is met de volgende tekst:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    Deze uitvoer geeft aan dat de taak is voltooid.

    > [!NOTE]
    > Als de **ExitCode** is een waarde dan 0, Zie [probleemoplossing](#troubleshooting).

    In dit voorbeeld slaat ook de gedownloade bestanden naar een **uitvoer.txt** bestand in de map met het script uit.

### <a name="view-output"></a>Uitvoer weergeven

Als u wilt zien van de woorden en aantallen die worden geproduceerd door de taak, opent u de **uitvoer.txt** bestand in een teksteditor.

> [!NOTE]
> De uitvoerbestanden van een MapReduce-taak zijn onveranderd. Dus als u dit voorbeeld opnieuw uitvoeren, moet u de naam van het uitvoerbestand te wijzigen.

## <a id="troubleshooting"></a>Problemen oplossen

Als er geen gegevens worden geretourneerd als de taak is voltooid, kunt u fouten voor de taak weergeven. Toevoegen als foutinformatie voor deze taak, de volgende opdracht aan het einde van de **mapreducejob.ps1** bestand, opslaan en voer het vervolgens opnieuw uit.

```powershell
# Print the output of the WordCount job.
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Deze cmdlet retourneert de informatie die is geschreven naar STDERR terwijl de taak wordt uitgevoerd.

## <a id="summary"></a>Samenvatting

Zoals u ziet, biedt Azure PowerShell een eenvoudige manier MapReduce-taken uitvoeren op een HDInsight-cluster, het bewaken van de taak de status en het ophalen van de uitvoer.

## <a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over MapReduce-taken in HDInsight:

* [MapReduce gebruiken met HDInsight Hadoop](hdinsight-use-mapreduce.md)

Voor meer informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Hive gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)
* [Pig gebruiken met Hadoop op HDInsight](hdinsight-use-pig.md)
