---
title: Uitvoeren van Hive-query's met behulp van HDInsight-SDK voor .NET - Azure | Microsoft Docs
description: Informatie over het verzenden van Hadoop-taken naar Azure HDInsight Hadoop met HDInsight .NET SDK.
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 4e291890-f8b4-426c-b5e8-d4fd512ff042
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2017
ms.author: jgao
ms.openlocfilehash: 642b02b06caaa7f2c5df71227d75281c0778a483
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="run-hive-queries-using-hdinsight-net-sdk"></a>Uitvoeren van Hive-query's met HDInsight .NET SDK
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Informatie over het indienen van Hive-query's met HDInsight .NET SDK. U schrijft een C#-programma voor het verzenden van een Hive-query voor het aanbieden van Hive-tabellen en de resultaten weer te geven.

> [!NOTE]
> De stappen in dit artikel moeten worden uitgevoerd vanuit een Windows-client. Gebruik de tabselector weergegeven boven aan het artikel voor meer informatie over het gebruik van een Linux-, OS X- of Unix-client werkt met Hive.

## <a name="prerequisites"></a>Vereisten
Voordat u dit artikel, hebt u de volgende items:

* **Een Hadoop-cluster in HDInsight**. Zie [aan de slag met Hadoop op basis van Linux in HDInsight](apache-hadoop-linux-tutorial-get-started.md).

    > [!WARNING]
    > De HDInsight-SDK voor .NET ondersteunt alleen terugkerende Hive-queryresultaten uit Azure Storage-accounts vanaf 15 September 2017. Als u dit voorbeeld met een HDInsight-cluster dat gebruik maakt van Azure Data Lake Store als primaire opslag gebruikt, kunt u met de .NET SDK zoekresultaten niet ophalen.

* **Visual Studio 2013/2015/2017**.

## <a name="submit-hive-queries-using-hdinsight-net-sdk"></a>Indienen van Hive-query's met HDInsight .NET SDK
De HDInsight .NET SDK biedt clientbibliotheken .NET, waardoor het makkelijker wordt om te werken met HDInsight-clusters in .NET. 

**Om taken te verzenden**

1. Maak een C#-consoletoepassing in Visual Studio.
2. Voer de volgende opdracht vanaf de Nuget Package Manager-Console:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. De volgende code gebruiken:

    ```csharp
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
   
        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
   
                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";
                
                // Only Azure Storage accounts are supported by the SDK
                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitHiveJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "tez" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };
   
                    System.Console.WriteLine("Submitting the Hive job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
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
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure
   
                    System.Console.WriteLine("Job output is: ");
   
                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }
    ```
4. Druk op **F5** om de toepassing uit te voeren.

De uitvoer van de toepassing zijn vergelijkbaar met:

![Uitvoer van HDInsight Hadoop Hive-taak](./media/apache-hadoop-use-hive-dotnet-sdk/hdinsight-hadoop-use-hive-net-sdk-output.png)

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd op verschillende manieren om een HDInsight-cluster te maken. Zie voor meer informatie de volgende artikelen:

* [Aan de slag met Azure HDInsight](apache-hadoop-linux-tutorial-get-started.md)
* [Hadoop-clusters maken in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md)
* [Hadoop-clusters in HDInsight beheren met behulp van de Azure-portal](../hdinsight-administer-use-management-portal.md)
* [HDInsight-SDK voor .NET-verwijzing](https://msdn.microsoft.com/library/mt271028.aspx)
* [Pig gebruiken met HDInsight](hdinsight-use-pig.md)
* [Sqoop gebruiken met HDInsight](apache-hadoop-use-sqoop-mac-linux.md)
* [Toepassingen zonder interactieve verificatie voor .NET HDInsight maken](../hdinsight-create-non-interactive-authentication-dotnet-applications.md)
 


