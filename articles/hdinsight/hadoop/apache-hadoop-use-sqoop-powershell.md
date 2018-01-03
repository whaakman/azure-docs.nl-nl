---
title: Sqoop taken uitvoeren met behulp van PowerShell en Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van Azure PowerShell vanaf een werkstation voor het uitvoeren van Sqoop importeren en exporteren tussen een Hadoop-cluster en een Azure SQL database.
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: bbb6f53a-e019-4d01-92bd-92c208c760b6
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2017
ms.author: jgao
ms.openlocfilehash: 1eaf55ea8e46598e261be43c646b5dbd48ac730b
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="run-sqoop-jobs-by-using-azure-powershell-for-hadoop-in-hdinsight"></a>Sqoop taken uitvoeren met behulp van Azure PowerShell voor Hadoop in HDInsight
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Informatie over het gebruik van Azure PowerShell Sqoop taken uitvoeren in Azure HDInsight importeren en exporteren tussen een HDInsight-cluster en een Azure SQL-database of SQL Server-database.

> [!NOTE]
> Hoewel u de procedures in dit artikel met ofwel een Windows- of Linux gebaseerde HDInsight-cluster gebruiken kunt, werken ze alleen vanuit een Windows-client. Als u andere methoden, gebruikt u de tabselector boven aan dit artikel. 
> 
> 

### <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u beschikken over de volgende items:

* Een werkstation met Azure PowerShell.
* Een Hadoop-cluster in HDInsight. Zie voor meer informatie [cluster en SQL-database maken](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="run-sqoop-by-using-powershell"></a>Sqoop uitvoeren met behulp van PowerShell
De volgende PowerShell-script vooraf verwerkt het bronbestand en exporteert het naar een Azure SQL database:

    $resourceGroupName = "<AzureResourceGroupName>"
    $hdinsightClusterName = "<HDInsightClusterName>"

    $httpUserName = "admin"
    $httpPassword = "<Password>"

    $defaultStorageAccountName = $hdinsightClusterName + "store"
    $defaultBlobContainerName = $hdinsightClusterName


    $sqlDatabaseServerName = $hdinsightClusterName + "dbserver"
    $sqlDatabaseName = $hdinsightClusterName + "db"
    $sqlDatabaseLogin = "sqluser"
    $sqlDatabasePassword = "<Password>"

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    #region - pre-process the source file

    Write-Host "`nPreprocessing the source file ..." -ForegroundColor Green

    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"

    # Define the connection string
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    # Create block blob objects referencing the source and destination blob.
    $storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $defaultStorageAccountName
    $storageContainer = ($storageAccount |Get-AzureStorageContainer -Name $defaultBlobContainerName).CloudBlobContainer
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)

    # Define a MemoryStream and a StreamReader for reading from the source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)

    # Define a MemoryStream and a StreamWriter for writing into the destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    # Pre-process the source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")

        # remove the "java.lang.Exception" from the first element of the array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList to remove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)

            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }

        # remove the lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }

    # Write to the destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    #endregion

    #region - export the log file from the cluster to the SQL database

    Write-Host "Exporting the log file ..." -ForegroundColor Green

    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"

    $tableName_log4j = "log4jlogs"
    $exportDir_log4j = "/tutorials/usesqoop/data"
    $sqljdbcdriver = "/user/oozie/share/lib/sqoop/sqljdbc41.jar"

    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1" `
        -Files $sqljdbcdriver

    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput
    #endregion

## <a name="limitations"></a>Beperkingen
Linux gebaseerde HDInsight biedt de volgende beperkingen:

* Bulk-export: de Sqoop-connector die wordt gebruikt voor het exporteren van gegevens naar Microsoft SQL Server of Azure SQL Database biedt momenteel geen ondersteuning voor bulksgewijs invoegen.

* Batchverwerking: met behulp van de `-batch` overschakelen wanneer worden ingevoegd, Sqoop meerdere invoegen in plaats van de bewerkingen insert batchverwerking wordt uitgevoerd. 

## <a name="next-steps"></a>Volgende stappen
Nu hebt u geleerd hoe Sqoop gebruiken. Voor meer informatie zie:

* [Oozie gebruiken met HDInsight](../hdinsight-use-oozie.md): Sqoop gebruiken in een werkstroom Oozie in te grijpen.
* [Vertraging vluchtgegevens analyseren met behulp van HDInsight](../hdinsight-analyze-flight-delay-data.md): Hive gebruiken voor het analyseren van vlucht gegevens uit te stellen en vervolgens Sqoop gebruiken om gegevens te exporteren naar een Azure SQL database.
* [Gegevens uploaden naar HDInsight](../hdinsight-upload-data.md): vinden van andere methoden voor het uploaden van gegevens naar HDInsight of Azure Blob storage.

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
