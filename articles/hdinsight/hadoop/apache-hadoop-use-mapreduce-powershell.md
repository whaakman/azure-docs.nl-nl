---
title: MapReduce en PowerShell gebruiken met Apache Hadoop - Azure HDInsight
description: Informatie over het gebruik van PowerShell op afstand MapReduce-taken uitvoeren met Apache Hadoop op HDInsight.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21b56d32-1785-4d44-8ae8-94467c12cfba
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 05/09/2018
ms.date: 04/15/2019
ms.author: v-yiso
ms.openlocfilehash: 29e23d5919a953566c803f2b7825a75a2993723c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62129022"
---
# <a name="run-mapreduce-jobs-with-apache-hadoop-on-hdinsight-using-powershell"></a>MapReduce-taken uitvoeren met Apache Hadoop op HDInsight met behulp van PowerShell

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]



Dit document bevat een voorbeeld van het gebruik van Azure PowerShell een MapReduce-taak uitvoeren in een Hadoop op HDInsight-cluster.

## <a id="prereq"></a>Vereisten

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

* **Een Azure HDInsight (Hadoop op HDInsight)-cluster**

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* **Een werkstation met Azure PowerShell**.

## <a id="powershell"></a>Voer een MapReduce-taak

Azure PowerShell biedt *cmdlets* waarmee u op afstand MapReduce-taken uitvoeren op HDInsight. Intern, kunt u PowerShell REST-aanroepen naar [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (voorheen Templeton) uitgevoerd op het HDInsight-cluster.

De volgende cmdlets worden gebruikt bij het uitvoeren van MapReduce-taken in een extern HDInsight-cluster.

* **Connect-AzAccount**: Azure PowerShell geverifieerd bij uw Azure-abonnement.

* **New-AzHDInsightMapReduceJobDefinition**: Maakt u een nieuwe *taak definitie* met behulp van de opgegeven MapReduce-informatie.

* **Start-AzHDInsightJob**: De taakdefinitie verzendt naar HDInsight en wordt de taak wordt gestart. Een *taak* object wordt geretourneerd.

* **Wait-AzHDInsightJob**: Maakt gebruik van het taakobject om de status van de taak te controleren. Wacht totdat de taak is voltooid of de wachttijd is overschreden.

* **Get-AzHDInsightJobOutput**: Gebruikt voor het ophalen van de uitvoer van de taak.

De volgende stappen laten zien hoe u deze cmdlets gebruikt voor het uitvoeren van een taak in uw HDInsight-cluster.

1. Met behulp van een editor, slaat u de volgende code als **mapreducejob.ps1**.

    ```powershell
    # Login to your Azure subscription
    # Is there an active Azure subscription?
    $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
    if(-not($sub))
    {
        Add-AzureRmAccount -EnvironmentName AzureChinaCloud
    }

    # Get cluster info
    $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
    $creds=Get-Credential -Message "Enter the login for the cluster"

    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    #NOTE: This assumes that the storage account is in the same resource
    #      group as the cluster. If it is not, change the
    #      --ResourceGroupName parameter to the group that contains storage.
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
    -ResourceGroupName $resourceGroup)[0].Value

    #Create a storage context
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey

    #Define the MapReduce job
    #NOTE: If using an HDInsight 2.0 cluster, use hadoop-examples.jar instead.
    # -JarFile = the JAR containing the MapReduce application
    # -ClassName = the class of the application
    # -Arguments = The input file, and the output directory
    $wordCountJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
        -JarFile "/example/jars/hadoop-mapreduce-examples.jar" `
        -ClassName "wordcount" `
        -Arguments `
            "/example/data/gutenberg/davinci.txt", `
            "/example/data/WordCountOutput"

    #Submit the job to the cluster
    Write-Host "Start the MapReduce job..." -ForegroundColor Green
    $wordCountJob = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $wordCountJobDefinition `
        -HttpCredential $creds

    #Wait for the job to complete
    Write-Host "Wait for the job to complete..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds
    # Download the output
    Get-AzureStorageBlobContent `
        -Blob 'example/data/WordCountOutput/part-r-00000' `
        -Container $container `
        -Destination output.txt `
        -Context $context
    # Print the output of the job.
    Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds
    ```

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
Get-AzHDInsightJobOutput `
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

* [Apache Hive gebruiken met Apache Hadoop op HDInsight](hdinsight-use-hive.md)
* [Apache Pig gebruiken met Apache Hadoop op HDInsight](hdinsight-use-pig.md)
