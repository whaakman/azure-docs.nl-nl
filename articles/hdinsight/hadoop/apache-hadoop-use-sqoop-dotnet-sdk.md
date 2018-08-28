---
title: Sqoop taken uitvoeren met behulp van .NET en HDInsight - Azure
description: Informatie over het gebruik van de HDInsight .NET SDK Sqoop-Importeer uitvoeren en exporteren tussen een Hadoop-cluster en een Azure SQL database.
keywords: sqoop taak
ms.reviewer: jasonh
services: hdinsight
author: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: jasonh
ms.openlocfilehash: ffcc2bff17fbe07c58df032dbb8994edb04ae5d0
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43046774"
---
# <a name="run-sqoop-jobs-by-using-net-sdk-for-hadoop-in-hdinsight"></a>Sqoop taken uitvoeren met behulp van .NET-SDK voor Hadoop in HDInsight
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Informatie over het gebruik van de Azure HDInsight .NET SDK uitvoeren Sqoop taken in HDInsight voor het importeren en exporteren tussen een HDInsight-cluster en een Azure SQL-database of SQL Server-database.

> [!NOTE]
> Maar u de procedures in dit artikel met ofwel een Windows- of Linux gebaseerde HDInsight-cluster gebruiken kunt, werken ze alleen vanaf een Windows-client. Als u andere methoden, gebruikt u de tabselector boven aan dit artikel.
> 

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint, moet u het volgende item hebben:

* Een Hadoop-cluster in HDInsight. Zie voor meer informatie, [maken van een cluster en een SQL-database](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="use-sqoop-on-hdinsight-clusters-with-the-net-sdk"></a>Sqoop gebruiken op HDInsight-clusters met de .NET SDK
De HDInsight .NET-SDK bevat clientbibliotheken voor .NET, zodat het eenvoudiger om te werken met HDInsight-clusters van .NET. In deze sectie maakt u een C#-consoletoepassing de hivesampletable exporteren naar de Azure SQL Database-tabel die u eerder in deze zelfstudie hebt gemaakt.

## <a name="submit-a-sqoop-job"></a>Een taak Sqoop verzenden

1. Maak een C#-consoletoepassing in Visual Studio.

2. Importeren vanuit de Visual Studio Package Manager-console het pakket met de volgende NuGet-opdracht:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job

3. Gebruik de volgende code in het bestand Program.cs:
   
        using System.Collections.Generic;
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
   
                    SubmitSqoopJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
   
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
   
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
   
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
   
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }

4. Als u wilt het programma uitvoert, selecteert u de **F5** sleutel. 

## <a name="limitations"></a>Beperkingen
HDInsight op basis van Linux biedt de volgende beperkingen:

* Bulk-export: de Sqoop-connector die wordt gebruikt voor het exporteren van gegevens naar Microsoft SQL Server of Azure SQL Database biedt momenteel geen ondersteuning voor bulksgewijs invoegen.

* Batchverwerking: met behulp van de `-batch` overschakelen wanneer voegt worden uitgevoerd, Sqoop meerdere invoegingen in plaats van de bewerkingen insert batchverwerking wordt uitgevoerd.

## <a name="next-steps"></a>Volgende stappen
U hebt nu geleerd hoe u Sqoop gebruiken. Voor meer informatie zie:

* [Oozie gebruiken met HDInsight](../hdinsight-use-oozie.md): gebruik Sqoop actie in een Oozie-workflow.
* [Analyseren van gegevens van vertragingen van vluchten met behulp van HDInsight](../hdinsight-analyze-flight-delay-data.md): gebruik van Hive voor het analyseren van vertragingen van vluchten vertraging van gegevens en Sqoop kunt gebruiken om gegevens te exporteren naar een Azure SQL-database.
* [Gegevens uploaden naar HDInsight](../hdinsight-upload-data.md): vinden van andere methoden voor het uploaden van gegevens naar HDInsight of Azure Blob storage.

