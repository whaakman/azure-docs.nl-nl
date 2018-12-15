---
title: MapReduce-taken opgeven met behulp van HDInsight .NET SDK - Azure
description: Informatie over het verzenden van MapReduce-taken naar Azure HDInsight Apache Hadoop met HDInsight .NET SDK.
ms.reviewer: jasonh
services: hdinsight
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: 67738a810968024e9006c142f5d8b1a975870c54
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53436820"
---
# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>Uitvoeren van de MapReduce-taken met behulp van HDInsight .NET SDK
[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Informatie over het verzenden van MapReduce-taken met behulp van HDInsight .NET SDK. HDInsight clusters worden geleverd met een jar-bestand met enkele voorbeelden van MapReduce. Het jar-bestand is */example/jars/hadoop-mapreduce-examples.jar*.  Een van de voorbeelden is *wordcount*. U ontwikkelt een C#-consoletoepassing om een taak wordcount te verzenden.  De taak leest de */example/data/gutenberg/davinci.txt* bestand, en levert de resultaten */example/data/davinciwordcount*.  Als u wilt dat de toepassing opnieuw uit te voeren, moet u de map voor uitvoer opschonen.

> [!NOTE]  
> De stappen in dit artikel moeten worden uitgevoerd vanaf een Windows-client. Gebruik de tabselector weergegeven bovenaan het artikel voor meer informatie over het gebruik van een Linux-, OS X- of Unix-client om te werken met Hive.
> 
> 

## <a name="prerequisites"></a>Vereisten
Voordat u dit artikel, hebt u de volgende items:

* **Een Hadoop-cluster in HDInsight**. Zie [aan de slag met Apache Hadoop op basis van Linux in HDInsight](apache-hadoop-linux-tutorial-get-started.md).
* **Visual Studio 2013/2015/2017**.

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>MapReduce-taken opgeven met behulp van HDInsight .NET SDK
De HDInsight .NET SDK bevat clientbibliotheken voor .NET, waardoor het gemakkelijker om te werken met HDInsight-clusters van .NET. 

**Voor het verzenden van taken**

1. Maak een C#-consoletoepassing in Visual Studio.
2. Voer de volgende opdracht uit vanuit de NuGet Package Manager-Console:

    ```   
    Install-Package Microsoft.Azure.Management.HDInsight.Job
    ```
3. Gebruik de volgende code:

    ```csharp
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
    ```

4. Druk op **F5** om de toepassing uit te voeren.

Als u wilt de taak opnieuw uitvoeren, moet u de taak uitvoer mapnaam op, in het voorbeeld is '/ voorbeeld/data/davinciwordcount'.

Wanneer de taak voltooid is, wordt de toepassing de inhoud van het uitvoerbestand 'onderdeel-r-00000'.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd verschillende manieren om een HDInsight-cluster te maken. Zie de volgende artikelen voor meer informatie:

* Zie voor het indienen van een Hive-taak, [uitvoeren Apache Hive-query's met behulp van HDInsight .NET SDK](apache-hadoop-use-hive-dotnet-sdk.md).
* Zie voor het maken van HDInsight-clusters, [Linux maken op basis van Apache Hadoop-clusters in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).
* Zie voor het beheren van HDInsight-clusters, [beheren Apache Hadoop-clusters in HDInsight](../hdinsight-administer-use-portal-linux.md).
* Zie voor het leren van de HDInsight .NET SDK, [HDInsight .NET SDK-verwijzing](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight).
* Voor niet-interactieve verificatie op Azure, Zie [niet-interactieve verificatie voor .NET HDInsight-toepassingen maken](../hdinsight-create-non-interactive-authentication-dotnet-applications.md).

