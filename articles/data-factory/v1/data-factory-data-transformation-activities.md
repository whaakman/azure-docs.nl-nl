---
title: 'Gegevenstransformatie: De transformatie & verwerken gegevens | Microsoft Docs'
description: Informatie over het voor het transformeren van gegevens of procesgegevens in Azure Data Factory met Hadoop, Machine Learning of Azure Data Lake Analytics.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 39786731-1e4b-40a4-81b7-d06e127427aa
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 4dd393f1f808e5cfc804bdf90ee31f69f15eed7c
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="transform-data-in-azure-data-factory"></a>Transformeer gegevens in Azure Data Factory
> [!div class="op_single_selector"]
> * [Hive](data-factory-hive-activity.md)  
> * [Pig](data-factory-pig-activity.md)  
> * [MapReduce](data-factory-map-reduce.md)  
> * [Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
> * [Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
> * [Opgeslagen procedure](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL](data-factory-usql-activity.md)
> * [.NET custom](data-factory-use-custom-activities.md)

## <a name="overview"></a>Overzicht
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [activiteiten voor gegevenstransformatie in Gegevensfactory versie 2](../transform-data.md).

Dit artikel wordt uitgelegd activiteiten voor gegevenstransformatie in Azure Data Factory kunt gebruiken voor het transformeren van uit te voeren en de onbewerkte gegevens worden verwerkt in de voorspellingen en inzichten. Een transformatieactiviteit wordt uitgevoerd in een computeromgeving zoals Azure HDInsight-cluster of een Azure-Batch. Deze koppelingen naar artikelen met gedetailleerde informatie over elke transformatieactiviteit.

Data Factory ondersteunt de volgende activiteiten voor gegevenstransformatie die kunnen worden toegevoegd aan [pijplijnen](data-factory-create-pipelines.md) ofwel afzonderlijk of gekoppeld met een andere activiteit.

> [!NOTE]
> Zie voor een overzicht met stapsgewijze instructies [een pijplijn maken met Hive-transformatie](data-factory-build-your-first-pipeline.md) artikel.  
> 
> 

## <a name="hdinsight-hive-activity"></a>HDInsight Hive-activiteit
De HDInsight Hive-activiteit in een Data Factory-pijplijn voert Hive-query's op uw eigen of op aanvraag Windows, Linux-gebaseerde HDInsight-cluster. Zie [Hive-activiteit](data-factory-hive-activity.md) voor meer informatie over deze activiteit. 

## <a name="hdinsight-pig-activity"></a>HDInsight Pig-activiteit
De HDInsight Pig-activiteit in een Data Factory-pijplijn voert Pig-query's in uw eigen of op aanvraag Windows, Linux-gebaseerde HDInsight-cluster. Zie [Pig activiteit](data-factory-pig-activity.md) voor meer informatie over deze activiteit. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce-activiteit
De HDInsight MapReduce-activiteit in een Data Factory-pijplijn voert MapReduce-programma's op uw eigen of op aanvraag Windows, Linux-gebaseerde HDInsight-cluster. Zie [MapReduce-activiteit](data-factory-map-reduce.md) voor meer informatie over deze activiteit.

## <a name="hdinsight-streaming-activity"></a>HDInsight-streamingactiviteit
Streamingactiviteit in een Data Factory-pijplijn HDInsight Hadoop-Streaming-programma's op uw eigen of Windows, Linux-gebaseerde HDInsight-cluster op aanvraag worden uitgevoerd. Zie [HDInsight streamingactiviteit](data-factory-hadoop-streaming-activity.md) voor meer informatie over deze activiteit.

## <a name="hdinsight-spark-activity"></a>HDInsight Spark-activiteit
De HDInsight Spark-activiteit in een Data Factory-pijplijn Spark-programma's op uw eigen HDInsight-cluster wordt uitgevoerd. Zie voor meer informatie [aanroepen Spark-programma's van Azure Data Factory](data-factory-spark.md). 

## <a name="machine-learning-activities"></a>Machine Learning-activiteiten
Azure Data Factory kunt u gemakkelijk maken pijplijnen die gebruikmaken van een gepubliceerde Azure Machine Learning-webservice voor predictive analytics. Met behulp van de [Batchuitvoeringsactiviteit](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) in een Azure Data Factory-pijplijn, kunt u een Machine Learning-webservice om te maken van voorspellingen op de gegevens in batch aanroept.

Na verloop van tijd moeten de voorspellende modellen in de Machine Learning-score experimenten worden retrained met nieuwe invoergegevenssets. Nadat u klaar bent met de retraining, wilt u de score webservice bijwerken met de retrained Machine Learning-model. U kunt de [Resourceactiviteit bijwerken](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) de webservice bijwerken met het nieuwe getrainde model.  

Zie [gebruiken Machine Learning-activiteiten](data-factory-azure-ml-batch-execution-activity.md) voor meer informatie over deze Machine Learning-activiteiten. 

## <a name="stored-procedure-activity"></a>Opgeslagen procedureactiviteit
U kunt de activiteit opgeslagen Procedure van SQL Server in een Data Factory-pijplijn gebruiken om aan te roepen, een opgeslagen procedure in een van de volgende gegevens worden opgeslagen: Azure SQL Database, Azure SQL Data Warehouse, SQL Server-Database in uw onderneming of een Azure VM. Zie [activiteit opgeslagen Procedure](data-factory-stored-proc-activity.md) artikel voor meer informatie.  

## <a name="data-lake-analytics-u-sql-activity"></a>U-SQL-activiteit van Data Lake Analytics
Data Lake Analytics U-SQL-activiteit wordt een U-SQL-script uitgevoerd op een Azure Data Lake Analytics-cluster. Zie [Analytics U-SQL-activiteit gegevens](data-factory-usql-activity.md) artikel voor meer informatie. 

## <a name="net-custom-activity"></a>Aangepaste .NET-activiteit
Als u nodig hebt om gegevens op een manier die niet wordt ondersteund door Data Factory te transformeren, kunt u een aangepaste activiteit maken met uw eigen logica gegevensverwerking en gebruikt u de activiteit in de pijplijn. U kunt de aangepaste .NET-activiteit moet worden uitgevoerd met behulp van een Azure Batch-service of een Azure HDInsight-cluster configureren. Zie [aangepaste activiteiten gebruiken](data-factory-use-custom-activities.md) artikel voor meer informatie. 

U kunt een aangepaste activiteit maken om R-scripts uit te voeren op uw HDInsight-cluster waarop R is geïnstalleerd. Zie [R-script uitvoeren met behulp van Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>COMPUTE-omgevingen
U maakt een gekoppelde service voor de compute-omgeving en gebruik vervolgens de gekoppelde service bij het definiëren van een transformatieactiviteit. Er zijn twee soorten berekeningsomgevingen die door Data Factory worden ondersteund. 

1. **On-Demand**: In dit geval de computeromgeving volledig wordt beheerd door de Data Factory. Deze is automatisch gemaakt door de Data Factory-service voordat een taak wordt verzonden om gegevens te verwerken en verwijderd wanneer de taak is voltooid. U kunt configureren en beheren van gedetailleerde instellingen van de omgeving compute op aanvraag voor het uitvoeren van taak Clusterbeheer en acties uitvoeren van de bootstrap. 
2. **Bring Your Own**: In dit geval kunt u uw eigen computeromgeving (bijvoorbeeld een HDInsight-cluster) registreren als een gekoppelde service in de Data Factory. De computeromgeving wordt beheerd door uzelf en de Data Factory-service gebruikt deze om de activiteiten worden uitgevoerd. 

Zie [gekoppelde Services berekenen](data-factory-compute-linked-services.md) artikel voor meer informatie over de compute-services die door Data Factory worden ondersteund. 

## <a name="summary"></a>Samenvatting
Azure Data Factory ondersteunt de volgende activiteiten voor gegevenstransformatie en de compute-omgevingen voor de activiteiten. De activiteiten voor gegevenstransformatie worden toegevoegd aan pijplijnen ofwel afzonderlijk of gekoppeld met een andere activiteit.

| Activiteiten voor gegevenstransformatie | Compute-omgeving |
|:--- |:--- |
| [Hive](data-factory-hive-activity.md) |HDInsight [Hadoop] |
| [Pig](data-factory-pig-activity.md) |HDInsight [Hadoop] |
| [MapReduce](data-factory-map-reduce.md) |HDInsight [Hadoop] |
| [Hadoop Streaming](data-factory-hadoop-streaming-activity.md) |HDInsight [Hadoop] |
| [Machine Learning-activiteiten: batchuitvoering en resources bijwerken](data-factory-azure-ml-batch-execution-activity.md) |Azure VM |
| [Opgeslagen procedure](data-factory-stored-proc-activity.md) |Azure SQL, Azure SQL Data Warehouse of SQL Server |
| [Data Lake Analytics U-SQL](data-factory-usql-activity.md) |Azure Data Lake Analytics |
| [DotNet](data-factory-use-custom-activities.md) |HDInsight [Hadoop] of Azure Batch |

