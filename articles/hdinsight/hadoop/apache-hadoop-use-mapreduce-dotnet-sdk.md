---
title: Met behulp van HDInsight-SDK voor .NET - Azure MapReduce-taken verzenden | Microsoft Docs
description: Informatie over het verzenden van MapReduce-taken naar Azure HDInsight Hadoop met HDInsight .NET SDK.
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: c85e44b0-85fd-4185-ad1c-c34a9fe5ef44
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/06/2017
ms.author: jgao
ms.openlocfilehash: 3028edde4c2c4f3bc268ad9103e3a600379ec295
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>Met HDInsight .NET SDK MapReduce-taken uitvoeren
[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Informatie over het verzenden van MapReduce-taken met HDInsight .NET SDK. HDInsight clusters worden geleverd met een jar-bestand met voorbeelden van MapReduce. Het jar-bestand is */example/jars/hadoop-mapreduce-examples.jar*.  Een van de voorbeelden is *wordcount*. U een C#-consoletoepassing te verzenden van een taak wordcount ontwikkelen.  De taak leest de */example/data/gutenberg/davinci.txt* bestand en worden de resultaten uitgevoerd naar */example/data/davinciwordcount*.  Als u wilt opnieuw uit te voeren van de toepassing, moet u de map voor uitvoer opschonen.

> [!NOTE]
> De stappen in dit artikel moeten worden uitgevoerd vanuit een Windows-client. Gebruik de tabselector weergegeven boven aan het artikel voor meer informatie over het gebruik van een Linux-, OS X- of Unix-client werkt met Hive.
> 
> 

## <a name="prerequisites"></a>Vereisten
Voordat u dit artikel, hebt u de volgende items:

* **Een Hadoop-cluster in HDInsight**. Zie [aan de slag met Hadoop op basis van Linux in HDInsight](apache-hadoop-linux-tutorial-get-started.md).
* **Visual Studio 2013/2015/2017**.

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>Verzenden van MapReduce-taken met HDInsight .NET SDK
De HDInsight .NET SDK biedt clientbibliotheken .NET, waardoor het makkelijker wordt om te werken met HDInsight-clusters in .NET. 

**Om taken te verzenden**

1. Maak een C#-consoletoepassing in Visual Studio.
2. Voer de volgende opdracht vanaf de Nuget Package Manager-Console:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. De volgende code gebruiken:
   
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;

        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
   
                private const string existingClusterName = "<Your HDInsight Cluster Name>";
                private const string existingClusterUri = existingClusterName + ".azurehdinsight.net";
                private const string existingClusterUsername = "<Cluster Username>";
                private const string existingClusterPassword = "<Cluster User Password>";
   
                private const string defaultStorageAccountName = "<Default Storage Account Name>"; //<StorageAccountName>.blob.core.windows.net
                private const string defaultStorageAccountKey = "<Default Storage Account Key>";
                private const string defaultStorageContainerName = "<Default Blob Container Name>";

                private const string sourceFile = "/example/data/gutenberg/davinci.txt";  
                private const string outputFolder = "/example/data/davinciwordcount";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = existingClusterUsername, Password = existingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(existingClusterUri, clusterCredentials);
   
                    SubmitMRJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitMRJob()
                {
                    List<string> args = new List<string> { { "/example/data/gutenberg/davinci.txt" }, { "/example/data/davinciwordcount" } };
   
                    var paras = new MapReduceJobSubmissionParameters
                    {
                        JarFile = @"/example/jars/hadoop-mapreduce-examples.jar",
                        JarClass = "wordcount",
                        Arguments = args
                    };
   
                    System.Console.WriteLine("Submitting the MR job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitMapReduceJob(paras);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);
   
                    System.Console.WriteLine("Waiting for the job completion ...");
   
                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }
   
                    // Get job output
                    System.Console.WriteLine("Job output is: ");
                    var storageAccess = new AzureStorageAccess(defaultStorageAccountName, defaultStorageAccountKey,
                        defaultStorageContainerName);
        
                    if (jobDetail.ExitValue == 0)
                    {
                        // Create the storage account object
                        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=" + 
                            defaultStorageAccountName + 
                            ";AccountKey=" + defaultStorageAccountKey);
        
                        // Create the blob client.
                        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
                        // Retrieve reference to a previously created container.
                        CloudBlobContainer container = blobClient.GetContainerReference(defaultStorageContainerName);
        
                        CloudBlockBlob blockBlob = container.GetBlockBlobReference(outputFolder.Substring(1) + "/part-r-00000");
        
                        using (var stream = blockBlob.OpenRead())
                        {
                            using (StreamReader reader = new StreamReader(stream))
                            {
                                while (!reader.EndOfStream)
                                {
                                    System.Console.WriteLine(reader.ReadLine());
                                }
                            }
                        }
                    }
                    else
                    {
                        // fetch stderr output in case of failure
                        var output = _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); 
        
                        using (var reader = new StreamReader(output, Encoding.UTF8))
                        {
                            string value = reader.ReadToEnd();
                            System.Console.WriteLine(value);
                        }
        
                    }
                }
            }
        }
4. Druk op **F5** om de toepassing uit te voeren.

Als u wilt de taak opnieuw uitvoeren, moet u de taak uitvoer mapnaam op, in het voorbeeld is '/ data-voorbeeld/davinciwordcount'.

Wanneer de taak voltooid is, wordt de inhoud van het uitvoerbestand 'onderdeel-r-00000' in de toepassing afgedrukt.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd op verschillende manieren om een HDInsight-cluster te maken. Zie voor meer informatie de volgende artikelen:

* Zie voor het indienen van een Hive-taak [uitvoeren Hive-query's met HDInsight .NET SDK](apache-hadoop-use-hive-dotnet-sdk.md).
* Zie voor het maken van HDInsight-clusters [maken Linux gebaseerde Hadoop-clusters in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).
* Zie voor het beheer van HDInsight-clusters, [beheren Hadoop-clusters in HDInsight](../hdinsight-administer-use-portal-linux.md).
* Zie voor het leren van de HDInsight .NET SDK, [HDInsight .NET SDK-naslaginformatie](https://msdn.microsoft.com/library/mt271028.aspx).
* Voor niet-interactieve verificatie op Azure, Zie [niet-interactieve verificatie .NET HDInsight-toepassingen maken](../hdinsight-create-non-interactive-authentication-dotnet-applications.md).

