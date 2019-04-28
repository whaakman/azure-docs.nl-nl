---
title: Apache Hive gebruiken met PowerShell in HDInsight - Azure
description: PowerShell gebruiken voor het uitvoeren van Hive-query's in Apache Hadoop op HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 04/23/2018
ms.date: 04/15/2019
ms.author: v-yiso
ms.openlocfilehash: 108a3e7d899eef4ca78ae7507bf4852b861e74d5
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62095517"
---
# <a name="run-apache-hive-queries-using-powershell"></a>Apache Hive-query's uitvoeren met behulp van PowerShell
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Dit document bevat een voorbeeld van het gebruik van Azure PowerShell in de modus Azure Resource Group Hive-query's uitvoeren in een Apache Hadoop op HDInsight-cluster.

> [!NOTE]  
> Dit document biedt geen een gedetailleerde beschrijving van wat de HiveQL-instructies die worden gebruikt in de voorbeelden doen. Zie voor informatie over de HiveQL die wordt gebruikt in dit voorbeeld, [Apache Hive gebruiken met Apache Hadoop op HDInsight](hdinsight-use-hive.md).

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

* Een Linux-gebaseerde Apache Hadoop op HDInsight-clusterversie 3.4 of hoger.

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Een client met Azure PowerShell.

[!INCLUDE [upgrade-powershell](../../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-a-hive-query"></a>Een Hive-query uitvoeren

Azure PowerShell biedt *cmdlets* waarmee u op afstand uitvoeren van Hive-query's op HDInsight. Intern, de REST-aanroepen naar voor het maken van de cmdlets [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) op het HDInsight-cluster.

De volgende cmdlets worden gebruikt bij het uitvoeren van Hive-query's in een extern HDInsight-cluster:

* `Connect-AzAccount`: Azure PowerShell geverifieerd bij uw Azure-abonnement.
* `New-AzHDInsightHiveJobDefinition`: Hiermee maakt u een *taak definitie* met behulp van de opgegeven HiveQL-instructies.
* `Start-AzHDInsightJob`: De taakdefinitie verzendt naar HDInsight en wordt de taak wordt gestart. Een *taak* object wordt geretourneerd.
* `Wait-AzHDInsightJob`: Maakt gebruik van het taakobject om de status van de taak te controleren. Wacht totdat de taak is voltooid of de wachttijd is overschreden.
* `Get-AzHDInsightJobOutput`: Gebruikt voor het ophalen van de uitvoer van de taak.
* `Invoke-AzHDInsightHiveJob`: Gebruikt voor het uitvoeren van HiveQL-instructies. Deze cmdlet-blokken de query is voltooid en vervolgens worden de resultaten geretourneerd.
* `Use-AzHDInsightCluster`: Hiermee stelt u het huidige cluster moet worden gebruikt voor de `Invoke-AzHDInsightHiveJob` opdracht.

De volgende stappen laten zien hoe u deze cmdlets gebruiken om een taak uitvoeren in uw HDInsight-cluster:

1. Met behulp van een editor, slaat u de volgende code als `hivejob.ps1`.

    ```powershell
    # Login to your Azure subscription
    # Is there an active Azure subscription?
    $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
    if(-not($sub))
    {
        Add-AzureRmAccount -EnvironmentName AzureChinaCloud
    }

    #Get cluster info
    $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
    $creds=Get-Credential -Message "Enter the login for the cluster"

    #HiveQL
    #Note: set hive.execution.engine=tez; is not required for
    #      Linux-based HDInsight
    $queryString = "set hive.execution.engine=tez;" +
                "DROP TABLE log4jLogs;" +
                "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"

    #Create an HDInsight Hive job definition
    $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString 

    #Submit the job to the cluster
    Write-Host "Start the Hive job..." -ForegroundColor Green

    $hiveJob = Start-AzureRmHDInsightJob -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $creds

    #Wait for the Hive job to complete
    Write-Host "Wait for the job to complete..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $creds

    # Print the output
    Write-Host "Display the standard output..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $hiveJob.JobId `
        -HttpCredential $creds
    ```

2. Open een nieuw **Azure PowerShell** opdrachtprompt. Wijzig de mappen in de locatie van de `hivejob.ps1` bestand en gebruik vervolgens de volgende opdracht uit het script uit te voeren:

        .\hivejob.ps1

    Wanneer het script wordt uitgevoerd, wordt u gevraagd de naam van het cluster en de referenties van het HTTPS-/ Cluster-Admin-account in te voeren. U kunt ook gevraagd zich aanmelden bij uw Azure-abonnement.

3. Wanneer de taak is voltooid, retourneert deze informatie die vergelijkbaar is met de volgende tekst:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Zoals eerder aangegeven, `Invoke-Hive` kan worden gebruikt om een query uitvoeren en wacht tot het antwoord. Het volgende script gebruiken om te zien hoe Invoke-Hive werkt:

    ```powershell
    # Login to your Azure subscription
    # Is there an active Azure subscription?
    $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
    if(-not($sub))
    {
        Add-AzureRmAccount -EnvironmentName AzureChinaCloud
    }

    #Get cluster info
    $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
    $creds=Get-Credential -Message "Enter the login for the cluster"

    # Set the cluster to use
    Use-AzureRmHDInsightCluster -ClusterName $clusterName -HttpCredential $creds

    $queryString = "set hive.execution.engine=tez;" +
                "DROP TABLE log4jLogs;" +
                "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '/example/data/';" +
                "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"
    Invoke-AzureRmHDInsightHiveJob `
        -StatusFolder "statusout" `
        -Query $queryString
    ```

    De uitvoer lijkt op de volgende tekst:

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]
   > Voor meer HiveQL-query's, kunt u de Azure PowerShell **hier tekenreeksen** cmdlet of HiveQL scriptbestanden. Het volgende fragment ziet u hoe u de `Invoke-Hive` cmdlet een bestand HiveQL-script uit te voeren. Het bestand HiveQL-script moet worden geüpload naar wasb: / /.
   >
   > `Invoke-AzHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > Voor meer informatie over **hier tekenreeksen**, Zie <a href="https://technet.microsoft.com/library/ee692792.aspx" target="_blank">met behulp van Windows PowerShell hier-tekenreeksen</a>.

## <a name="troubleshooting"></a>Problemen oplossen

Als er geen gegevens worden geretourneerd als de taak is voltooid, moet u de foutenlogboeken weergeven. Als u wilt bekijken foutgegevens voor deze taak, Voeg het volgende toe aan het einde van de `hivejob.ps1` bestand, opslaan en voer het vervolgens opnieuw uit.

```powershell
# Print the output of the Hive job.
Get-AzHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Deze cmdlet retourneert de gegevens die worden geschreven naar STDERR tijdens de taakverwerking van de.

## <a name="summary"></a>Samenvatting

Zoals u ziet, biedt Azure PowerShell een eenvoudige manier om Hive-query's uitvoeren in een HDInsight-cluster, de taakstatus controleren en ophalen van de uitvoer.

## <a name="next-steps"></a>Volgende stappen

Voor algemene informatie over Hive in HDInsight:

* [Apache Hive gebruiken met Apache Hadoop op HDInsight](hdinsight-use-hive.md)

Voor meer informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Apache Pig gebruiken met Apache Hadoop op HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met Apache Hadoop op HDInsight](hdinsight-use-mapreduce.md)
