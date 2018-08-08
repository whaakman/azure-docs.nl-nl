---
title: Uitvoeren van Hive-query's met behulp van HDInsight .NET SDK - Azure
description: Leer hoe u Hadoop-taken op Azure HDInsight Hadoop met HDInsight .NET SDK.
editor: jasonwhowell
services: hdinsight
author: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: jasonh
ms.openlocfilehash: cc7eb82e36ecc03e17b7abcf38f6bbe5c754ee5b
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39599432"
---
# <a name="run-hive-queries-using-hdinsight-net-sdk"></a>Uitvoeren van Hive-query's met behulp van HDInsight .NET SDK
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Informatie over het indienen van Hive-query's met behulp van HDInsight .NET SDK. U schrijft een C#-programma om in te dienen een Hive-query voor het aanbieden van Hive-tabellen en de resultaten weer te geven.

> [!NOTE]
> De stappen in dit artikel moeten worden uitgevoerd vanaf een Windows-client. Gebruik de tabselector weergegeven bovenaan het artikel voor meer informatie over het gebruik van een Linux-, OS X- of Unix-client om te werken met Hive.

## <a name="prerequisites"></a>Vereisten
Voordat u dit artikel, hebt u de volgende items:

* **Een Hadoop-cluster in HDInsight**. Zie [aan de slag met Hadoop op basis van Linux in HDInsight](apache-hadoop-linux-tutorial-get-started.md).

    > [!WARNING]
    > Vanaf 15 September 2017 ondersteunt de HDInsight .NET SDK alleen terugkerende Hive-query-resultaten van Azure Storage-accounts. Als u dit voorbeeld met een HDInsight-cluster dat gebruik maakt van Azure Data Lake Store als primaire opslag gebruikt, kunt u met de .NET SDK zoekresultaten niet ophalen.

* **Visual Studio 2013/2015/2017**.

## <a name="run-a-hive-query"></a>Een Hive-Query uitvoeren
De HDInsight .NET SDK bevat clientbibliotheken voor .NET, waardoor het gemakkelijker om te werken met HDInsight-clusters van .NET. 

**Voor het verzenden van taken**

1. Maak een C#-consoletoepassing in Visual Studio.
2. Voer de volgende opdracht uit vanuit de Nuget Package Manager-Console:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. Gebruik de volgende code:

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
In dit artikel hebt u geleerd verschillende manieren om een HDInsight-cluster te maken. Zie de volgende artikelen voor meer informatie:

* [Aan de slag met Azure HDInsight](apache-hadoop-linux-tutorial-get-started.md)
* [Hadoop-clusters maken in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md)
* [Hadoop-clusters in HDInsight beheren met behulp van de Azure-portal](../hdinsight-administer-use-management-portal.md)
* [Naslaginformatie over de HDInsight .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight)
* [Pig gebruiken met HDInsight](hdinsight-use-pig.md)
* [Sqoop gebruiken met HDInsight](apache-hadoop-use-sqoop-mac-linux.md)
* [Toepassingen zonder interactieve verificatie voor .NET HDInsight maken](../hdinsight-create-non-interactive-authentication-dotnet-applications.md)
 


