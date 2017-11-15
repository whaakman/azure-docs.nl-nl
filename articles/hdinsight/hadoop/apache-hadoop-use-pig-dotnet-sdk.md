---
title: Apache Pig-taken uitvoeren met .NET SDK voor Hadoop - Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van de .NET SDK voor Hadoop voor het verzenden van Pig-taken naar Hadoop op HDInsight.
services: hdinsight
documentationcenter: .net
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: fa11d49a-328c-47e7-b16d-e7ed2a453195
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/08/2017
ms.author: larryfr
ms.openlocfilehash: c828a7b63e70669ed38ecea898442a3978e67ba7
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>Pig-taken met de .NET SDK voor Hadoop in HDInsight uitvoeren

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Informatie over het gebruik van de .NET SDK voor Hadoop voor het verzenden van Apache Pig-taken naar Hadoop op Azure HDInsight.

De HDInsight-SDK voor .NET biedt .NET-clientbibliotheken waarmee eenvoudiger werken met HDInsight-clusters in .NET. Pig kunt u MapReduce-bewerkingen door het modelleren van een reeks gegevenstransformaties maakt. In dit document leert u hoe u een eenvoudige C#-toepassing gebruikt voor het verzenden van een Pig-taak voor een HDInsight-cluster.

## <a name="prerequisites"></a>Vereisten

Als u de stappen in dit artikel hebt uitgevoerd, wilt nodig u het volgende.

* Een Azure HDInsight (Hadoop in HDInsight)-cluster (Windows of Linux-).

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Visual Studio 2012, 2013 of 2015 van 2017.

## <a name="create-the-application"></a>De toepassing maken

De HDInsight .NET SDK biedt clientbibliotheken .NET, waardoor het makkelijker wordt om te werken met HDInsight-clusters in .NET.

1. Van de **bestand** menu in Visual Studio, selecteer **nieuw** en selecteer vervolgens **Project**.

2. Typ of Selecteer de volgende waarden voor het nieuwe project:

   | Eigenschap | Waarde |
   | ------ | ------ |
   | Category | Templates/Visual C#/Windows |
   | Template | Console Application |
   | Naam | SubmitPigJob |

3. Klik op **OK** om het project te maken.

4. Van de **extra** selecteert u **Library Package Manager** of **NuGet Package Manager**, en selecteer vervolgens **Package Manager Console**.

5. Gebruik de volgende opdracht voor het installeren van de .NET SDK-pakketten:

        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. Dubbelklik in Solution Explorer op **Program.cs** om dit te openen. Vervang de bestaande code met de volgende opties.

    ```csharp
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

            static void Main(string[] args)
            {
                System.Console.WriteLine("The application is running ...");

                var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                SubmitPigJob();

                System.Console.WriteLine("Press ENTER to continue ...");
                System.Console.ReadLine();
            }

            private static void SubmitPigJob()
            {
                var parameters = new PigJobSubmissionParameters
                {
                    Query = @"LOGS = LOAD '/example/data/sample.log';
                                LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                RESULT = order FREQUENCIES by COUNT desc;
                                DUMP RESULT;"
                };

                System.Console.WriteLine("Submitting the Pig job to the cluster...");
                var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                System.Console.WriteLine("Validating that the response is as expected...");
                System.Console.WriteLine("Response status code is " + response.StatusCode);
                System.Console.WriteLine("Validating the response object...");
                System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
            }
        }
    }
    ```

7. Als u wilt de toepassing niet starten, drukt u op **F5**.

8. Om af te sluiten van de toepassing, drukt u op **ENTER**.

## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over Pig in HDInsight, [Pig gebruiken met Hadoop op HDInsight](hdinsight-use-pig.md).

Zie de volgende documenten voor meer informatie over het gebruik van Hadoop in HDInsight:

* [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/
