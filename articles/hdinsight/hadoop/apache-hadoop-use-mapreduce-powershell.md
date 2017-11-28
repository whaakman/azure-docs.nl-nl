---
title: MapReduce en PowerShell gebruiken met Hadoop - Azure HDInsight | Microsoft Docs
description: Informatie over het gebruiken van PowerShell op afstand MapReduce-taken uitvoeren met Hadoop op HDInsight.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21b56d32-1785-4d44-8ae8-94467c12cfba
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/27/2017
ms.author: larryfr
ms.openlocfilehash: cfed3617f20074f361629c65f14ce38c1012c702
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>MapReduce-taken uitvoeren met Hadoop in HDInsight met behulp van PowerShell

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Dit document bevat een voorbeeld van een MapReduce-taak uitgevoerd in een Hadoop op HDInsight-cluster met behulp van Azure PowerShell.

## <a id="prereq"></a>Vereisten

* **Een Azure HDInsight (Hadoop in HDInsight)-cluster**

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* **Een werkstation met Azure PowerShell**.

## <a id="powershell"></a>Voer een MapReduce-taak met Azure PowerShell

Azure PowerShell biedt *cmdlets* waarmee u kunt op afstand MapReduce-taken uitvoeren op HDInsight. Intern PowerShell opgeroepen REST [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (voorheen Templeton) uitgevoerd op het HDInsight-cluster.

De volgende cmdlets worden gebruikt bij het uitvoeren van MapReduce-taken in een externe HDInsight-cluster.

* **Login-AzureRmAccount**: Azure PowerShell verifieert met uw Azure-abonnement.

* **Nieuwe AzureRmHDInsightMapReduceJobDefinition**: maakt een nieuw *taak definitie* met behulp van de opgegeven MapReduce-informatie.

* **Start AzureRmHDInsightJob**: de taakdefinitie verzendt naar HDInsight en de taak wordt gestart. Een *taak* object wordt geretourneerd.

* **Wacht AzureRmHDInsightJob**: het taakobject gebruikt om te controleren van de status van de taak. Wacht totdat de taak is voltooid of de wachttijd is overschreden.

* **Get-AzureRmHDInsightJobOutput**: gebruikt voor het ophalen van de uitvoer van de taak.

De volgende stappen laten zien hoe u deze cmdlets gebruiken om een taak uitvoert in uw HDInsight-cluster.

1. De volgende code als met een editor opslaan **mapreducejob.ps1**.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-mapreduce/use-mapreduce.ps1?range=5-69)]

2. Open een nieuw **Azure PowerShell** opdrachtprompt. Wijzig de mappen naar de locatie van de **mapreducejob.ps1** bestand en vervolgens voert u het script met de volgende opdracht:

        .\mapreducejob.ps1

    Wanneer u het script uitvoert, wordt u gevraagd de naam van het HDInsight-cluster en de cluster-aanmelding op te geven. U mogelijk ook gevraagd om uw Azure-abonnement te verifiëren.

3. Wanneer de taak is voltooid, wordt de uitvoer is vergelijkbaar met de volgende tekst:

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

    In dit voorbeeld worden ook opgeslagen de gedownloade bestanden naar een **uitvoer.txt** bestand in de map met het script uit.

### <a name="view-output"></a>Uitvoer weergeven

Overzicht van de woorden en aantallen geproduceerd door de taak, opent u de **uitvoer.txt** bestand in een teksteditor.

> [!NOTE]
> De uitvoerbestanden van een MapReduce-taak zijn niet-wijzigbaar. Dus als u dit voorbeeld opnieuw uitvoeren, moet u de naam van het uitvoerbestand wijzigen.

## <a id="troubleshooting"></a>Problemen oplossen

Als er geen gegevens worden geretourneerd als de taak is voltooid, kunt u fouten voor de taak weergeven. Om weer te geven informatie over de fout voor deze taak, kunt u de volgende opdracht toevoegen aan het einde van de **mapreducejob.ps1** bestand, opslaan en vervolgens opnieuw uit te voeren.

```powershell
# Print the output of the WordCount job.
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Deze cmdlet retourneert de gegevens die in STDERR is geschreven als de taak wordt uitgevoerd.

## <a id="summary"></a>Samenvatting

Zoals u zien kunt, biedt Azure PowerShell een eenvoudige manier om MapReduce-taken uitvoeren op een HDInsight-cluster, de taakstatus te controleren en ophalen van de uitvoer.

## <a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over MapReduce-taken in HDInsight:

* [Gebruik MapReduce op HDInsight Hadoop](hdinsight-use-mapreduce.md)

Voor informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md)
* [Pig gebruiken met Hadoop in HDInsight](hdinsight-use-pig.md)
