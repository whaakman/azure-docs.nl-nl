---
title: Apache Pig-taken uitvoeren met .NET SDK voor Hadoop - Azure HDInsight
description: Informatie over het gebruik van de .NET-SDK voor Hadoop voor het verzenden van Pig-taken naar Hadoop op HDInsight.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: jasonh
ms.openlocfilehash: e781d5d6cf9b1e2ce3c79e138c7a82c780ce8df4
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39594811"
---
# <a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>Met de .NET-SDK voor Hadoop in HDInsight Pig-taken uitvoeren

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Informatie over het gebruik van de .NET-SDK voor Hadoop voor het verzenden van Apache Pig-taken naar Hadoop op Azure HDInsight.

De HDInsight .NET SDK bevat .NET-clientbibliotheken die het gemakkelijker om te werken met HDInsight-clusters van .NET. Pig kunt u MapReduce-bewerkingen door het modelleren van een reeks gegevenstransformaties maken. In dit document leert u hoe u een basic C#-toepassing gebruiken om een Pig-taak met een HDInsight-cluster te verzenden.

## <a name="prerequisites"></a>Vereisten

Als u wilt de stappen in dit artikel hebt voltooid, moet u het volgende.

* Een Azure HDInsight (Hadoop op HDInsight)-cluster (ofwel Windows of Linux-).

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Visual Studio 2012, 2013, 2015 of 2017.

## <a name="create-the-application"></a>De toepassing maken

De HDInsight .NET SDK bevat clientbibliotheken voor .NET, waardoor het gemakkelijker om te werken met HDInsight-clusters van .NET.

1. Uit de **bestand** menu van Visual Studio, selecteer **nieuw** en selecteer vervolgens **Project**.

2. Voor het nieuwe project, typt of selecteert u de volgende waarden:

   | Eigenschap | Waarde |
   | ------ | ------ |
   | Categorie | Templates/Visual C#/Windows |
   | Template | Console Application |
   | Naam | SubmitPigJob |

3. Klik op **OK** om het project aan te maken.

4. Uit de **extra** in het menu **Library Package Manager** of **NuGet Package Manager**, en selecteer vervolgens **Package Manager Console**.

5. De SDK voor .NET om pakketten te installeren, gebruik de volgende opdracht:

        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. Dubbelklik in Solution Explorer op **Program.cs** om dit te openen. Vervang de bestaande code door de volgende.

    ```csharp
    using Microsoft.Azure.Management.HDInsight.Job;
    using Microsoft.Azure.Management.HDInsight.Job.Models;
    using Hyak.Common;

    namespace SubmitPigJob
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

7. Als u wilt de toepassing wordt gestart, drukt u op **F5**.

8. Als u wilt de toepassing afsluiten, drukt u op **ENTER**.

## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over Pig in HDInsight, [Pig gebruiken met Hadoop op HDInsight](hdinsight-use-pig.md).

Zie de volgende documenten voor meer informatie over het gebruik van Hadoop op HDInsight:

* [Hive gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/
