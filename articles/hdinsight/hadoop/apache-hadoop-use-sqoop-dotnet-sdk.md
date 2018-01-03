---
title: Sqoop taken uitvoeren met behulp van .NET- en HDInsight - Azure | Microsoft Docs
description: Informatie over het gebruik van de HDInsight-SDK voor .NET voor het uitvoeren van Sqoop importeren en exporteren tussen een Hadoop-cluster en een Azure SQL database.
keywords: sqoop taak
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 87bacd13-7775-4b71-91da-161cb6224a96
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2017
ms.author: jgao
ms.openlocfilehash: 41a1c7c0b7c0ad00746508bbf341eff448690725
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="run-sqoop-jobs-by-using-net-sdk-for-hadoop-in-hdinsight"></a>Sqoop taken uitvoeren met behulp van de .NET SDK voor Hadoop in HDInsight
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Informatie over het gebruik van de Azure HDInsight .NET SDK Sqoop taken uitvoeren in HDInsight om te importeren en exporteren tussen een HDInsight-cluster en een Azure SQL-database of SQL Server-database.

> [!NOTE]
> Hoewel u de procedures in dit artikel met ofwel een Windows- of Linux gebaseerde HDInsight-cluster gebruiken kunt, werken ze alleen vanuit een Windows-client. Als u andere methoden, gebruikt u de tabselector boven aan dit artikel.
> 

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint, hebt u het volgende item:

* Een Hadoop-cluster in HDInsight. Zie voor meer informatie [maken van een cluster en een SQL-database](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="use-sqoop-on-hdinsight-clusters-with-the-net-sdk"></a>Sqoop gebruiken op HDInsight-clusters met de .NET SDK
De HDInsight .NET SDK biedt clientbibliotheken .NET, zodat het eenvoudiger om te werken met HDInsight-clusters in .NET. In deze sectie maakt u een C#-consoletoepassing de hivesampletable exporteren naar de Azure SQL Database-tabel die u eerder in deze zelfstudie hebt gemaakt.

## <a name="submit-a-sqoop-job"></a>Verzenden van een taak Sqoop

1. Maak een C#-consoletoepassing in Visual Studio.

2. Importeer het pakket met de volgende NuGet-opdracht uit de Visual Studio Package Manager-console:
   
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

4. Om het programma uitvoert, selecteert u de **F5** sleutel. 

## <a name="limitations"></a>Beperkingen
Linux gebaseerde HDInsight biedt de volgende beperkingen:

* Bulk-export: de Sqoop-connector die wordt gebruikt voor het exporteren van gegevens naar Microsoft SQL Server of Azure SQL Database biedt momenteel geen ondersteuning voor bulksgewijs invoegen.

* Batchverwerking: met behulp van de `-batch` overschakelen wanneer worden ingevoegd, Sqoop meerdere invoegen in plaats van de bewerkingen insert batchverwerking wordt uitgevoerd.

## <a name="next-steps"></a>Volgende stappen
Nu hebt u geleerd hoe Sqoop gebruiken. Voor meer informatie zie:

* [Oozie gebruiken met HDInsight](../hdinsight-use-oozie.md): Sqoop gebruiken in een werkstroom Oozie in te grijpen.
* [Vertraging vluchtgegevens met HDInsight analyseren](../hdinsight-analyze-flight-delay-data.md): Hive gebruiken voor het analyseren van vlucht gegevens uit te stellen en vervolgens Sqoop gebruiken om gegevens te exporteren naar een Azure SQL database.
* [Gegevens uploaden naar HDInsight](../hdinsight-upload-data.md): vinden van andere methoden voor het uploaden van gegevens naar HDInsight of Azure Blob storage.

