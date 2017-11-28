---
title: Hadoop Pig gebruiken met PowerShell in HDInsight - Azure | Microsoft Docs
description: Informatie over het verzenden van Pig-taken naar een Hadoop-cluster in HDInsight met Azure PowerShell.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 737089c1-b494-4387-9def-7b4dac3be532
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/27/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: b883a3d9559c2f11742cd54716d8220b2034470d
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="use-azure-powershell-to-run-pig-jobs-with-hdinsight"></a>Gebruik Azure PowerShell Pig-taken uitvoeren met HDInsight

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Dit document bevat een voorbeeld van het gebruik van Azure PowerShell voor het verzenden van Pig-taken naar een Hadoop op HDInsight-cluster. Pig kunt u MapReduce-taken schrijven met behulp van een andere taal (Pig Latin) die gegevenstransformaties modellen in plaats van toewijzen en functies te beperken.

> [!NOTE]
> Dit document biedt geen een gedetailleerde beschrijving van wat de Pig Latin-instructies gebruikt in de voorbeelden doen. Zie voor meer informatie over de Pig Latin in dit voorbeeld gebruikt [Pig gebruiken met Hadoop op HDInsight](hdinsight-use-pig.md).

## <a id="prereq"></a>Vereisten

* **Een Azure HDInsight-cluster**

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* **Een werkstation met Azure PowerShell**.

[!INCLUDE [upgrade-powershell](../../../includes/hdinsight-use-latest-powershell.md)]

## <a id="powershell"></a>Pig-taken met behulp van PowerShell uitgevoerd

Azure PowerShell biedt *cmdlets* waarmee u kunt op afstand Pig-taken uitvoeren op HDInsight. PowerShell gebruikt intern, REST-aanroepen naar [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) uitgevoerd op het HDInsight-cluster.

De volgende cmdlets worden gebruikt bij het uitvoeren van Pig-taken op een externe HDInsight-cluster:

* **Login-AzureRmAccount**: Azure PowerShell gebruiken om uw Azure-abonnement te worden geverifieerd.
* **Nieuwe AzureRmHDInsightPigJobDefinition**: maakt een *taak definitie* met behulp van de opgegeven Pig Latin-instructies.
* **Start AzureRmHDInsightJob**: de taakdefinitie verzendt naar HDInsight en de taak wordt gestart. Een *taak* object wordt geretourneerd.
* **Wacht AzureRmHDInsightJob**: het taakobject gebruikt om te controleren van de status van de taak. Wacht totdat de taak is voltooid of de wachttijd is overschreden.
* **Get-AzureRmHDInsightJobOutput**: gebruikt voor het ophalen van de uitvoer van de taak.

De volgende stappen laten zien hoe u deze cmdlets gebruiken om een taak uitvoeren op uw HDInsight-cluster.

1. De volgende code als met een editor opslaan **pigjob.ps1**.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]

1. Open een nieuw Windows PowerShell-opdrachtprompt. Wijzig de mappen naar de locatie van de **pigjob.ps1** bestand en vervolgens voert u het script met de volgende opdracht:

        .\pigjob.ps1

    U wordt gevraagd of u zich aanmelden bij uw Azure-abonnement. Vervolgens wordt u gevraagd om de naam HTTPs/Admin en het wachtwoord voor het HDInsight-cluster.

2. Wanneer de taak is voltooid, wordt de geretourneerde informatie moet vergelijkbaar met de volgende tekst:

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="troubleshooting"></a>Problemen oplossen

Als er geen gegevens worden geretourneerd als de taak is voltooid, kunt u de foutenlogboeken weergeven. Om weer te geven informatie over de fout voor deze taak, kunt u de volgende opdracht toevoegen aan het einde van de **pigjob.ps1** bestand, opslaan en vervolgens opnieuw uit te voeren.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Deze cmdlet retourneert de gegevens die in STDERR is gemaakt tijdens de verwerking van een taak.

## <a id="summary"></a>Samenvatting
Zoals u zien kunt, biedt Azure PowerShell een eenvoudige manier om Pig-taken uitvoeren op een HDInsight-cluster, de taakstatus te controleren en ophalen van de uitvoer.

## <a id="nextsteps"></a>Volgende stappen
Voor algemene informatie over Pig in HDInsight:

* [Pig gebruiken met Hadoop in HDInsight](hdinsight-use-pig.md)

Voor informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)
