---
title: 'Gegevenstransformatie: Gegevens verwerken en transformeren | Microsoft Docs'
description: Meer informatie over het transformeren van gegevens of gegevens verwerken in Azure Data Factory met behulp van Hadoop-, Machine Learning- of Azure Data Lake Analytics.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 39786731-1e4b-40a4-81b7-d06e127427aa
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: b1a99f2872a69e01232c69a73f36319552429ca0
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54022441"
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
> * [Aangepaste .NET](data-factory-use-custom-activities.md)

## <a name="overview"></a>Overzicht
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [activiteiten voor gegevenstransformatie in Data Factory](../transform-data.md).

In dit artikel wordt uitgelegd activiteiten voor gegevenstransformatie in Azure Data Factory, dat u gebruiken kunt om te zetten en de onbewerkte gegevens in voorspellingen en inzichten verwerkt. Een transformatieactiviteit wordt uitgevoerd in een computeromgeving, zoals Azure HDInsight-cluster of een Azure Batch. Het bevat koppelingen naar artikelen met gedetailleerde informatie over elke transformatieactiviteit.

Data Factory ondersteunt de volgende activiteiten voor gegevenstransformatie die kunnen worden toegevoegd aan [pijplijnen](data-factory-create-pipelines.md) ofwel afzonderlijk of in een keten met een andere activiteit.

> [!NOTE]
> Zie voor een overzicht met stapsgewijze instructies [een pijplijn maken met Hive-transformatie](data-factory-build-your-first-pipeline.md) artikel.  
> 
> 

## <a name="hdinsight-hive-activity"></a>HDInsight Hive-activiteit
De HDInsight Hive-activiteit in een Data Factory-pijplijn voert een Hive-query's op uw eigen of Windows/Linux gebaseerd HDInsight-cluster op aanvraag. Zie [Hive-activiteit](data-factory-hive-activity.md) artikel voor meer informatie over deze activiteit. 

## <a name="hdinsight-pig-activity"></a>HDInsight Pig-activiteit
De HDInsight Pig-activiteit in een Data Factory-pijplijn voert Pig-query's op uw eigen of Windows/Linux gebaseerd HDInsight-cluster op aanvraag. Zie [Pig-activiteit](data-factory-pig-activity.md) artikel voor meer informatie over deze activiteit. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce-activiteit
De HDInsight MapReduce-activiteit in een Data Factory-pijplijn voert MapReduce-programma's op uw eigen of Windows/Linux gebaseerd HDInsight-cluster op aanvraag. Zie [MapReduce-activiteit](data-factory-map-reduce.md) artikel voor meer informatie over deze activiteit.

## <a name="hdinsight-streaming-activity"></a>HDInsight Streaming-activiteit
De HDInsight Streaming-activiteit in een Data Factory-pijplijn voert Hadoop-Streaming-programma's op uw eigen of Windows/Linux gebaseerd HDInsight-cluster op aanvraag. Zie [HDInsight Streaming-activiteit](data-factory-hadoop-streaming-activity.md) voor meer informatie over deze activiteit.

## <a name="hdinsight-spark-activity"></a>HDInsight Spark-activiteit
De HDInsight Spark-activiteit in een Data Factory-pijplijn Spark-programma's op uw eigen HDInsight-cluster wordt uitgevoerd. Zie voor meer informatie, [aanroepen Spark-programma's van Azure Data Factory](data-factory-spark.md). 

## <a name="machine-learning-activities"></a>Machine Learning-activiteiten
Azure Data Factory kunt u eenvoudig pijplijnen die gebruikmaken van een gepubliceerde Azure Machine Learning-webservice voor voorspellende analyses te maken. Met behulp van de [Batchuitvoeringsactiviteit](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) in een Azure Data Factory-pijplijn, kunt u een Machine Learning-webservice om voorspellingen te maken van de gegevens in batch aanroepen.

De voorspellende modellen in de Machine Learning experimenten scoren moeten opnieuw worden getraind met behulp van nieuwe invoergegevenssets na verloop van tijd. Als u klaar bent met het opnieuw trainen, die u wilt de scoringwebservice bijwerken met de retrained Machine Learning-model. U kunt de [activiteit resources bijwerken](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) het bijwerken van de webservice met het zojuist getrainde model.  

Zie [gebruik Machine Learning-activiteiten](data-factory-azure-ml-batch-execution-activity.md) voor meer informatie over deze Machine Learning-activiteiten. 

## <a name="stored-procedure-activity"></a>Opgeslagen procedureactiviteit
U kunt de SQL Server opgeslagen Procedure-activiteit in een Data Factory-pijplijn gebruiken om aan te roepen een opgeslagen procedure in een van de volgende gegevensarchieven: Azure SQL-Database, Azure SQL Data Warehouse, SQL Server-Database in uw onderneming of een Azure-VM. Zie [opgeslagen-Procedureactiviteit](data-factory-stored-proc-activity.md) artikel voor meer informatie.  

## <a name="data-lake-analytics-u-sql-activity"></a>U-SQL-activiteit van Data Lake Analytics
Data Lake Analytics U-SQL-activiteit wordt een U-SQL-script uitgevoerd op een Azure Data Lake Analytics-cluster. Zie [Data Analytics U-SQL-activiteit](data-factory-usql-activity.md) artikel voor meer informatie. 

## <a name="net-custom-activity"></a>Aangepaste .NET-activiteit
Als u nodig hebt voor het transformeren van gegevens op een manier die niet wordt ondersteund door Data Factory, kunt u een aangepaste activiteit maken met uw eigen logica gegevensverwerking en het gebruik van de activiteit in de pijplijn. U kunt de aangepaste .NET-activiteit om uit te voeren met behulp van een Azure Batch-service of een Azure HDInsight-cluster configureren. Zie [aangepaste activiteiten gebruiken](data-factory-use-custom-activities.md) artikel voor meer informatie. 

U kunt een aangepaste activiteit maken om R-scripts uit te voeren op uw HDInsight-cluster waarop R is geïnstalleerd. Zie [R-script uitvoeren met behulp van Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>COMPUTE-omgevingen
U maakt een gekoppelde service voor de compute-omgeving en de gekoppelde service vervolgens gebruiken bij het definiëren van een transformatieactiviteit. Er zijn twee soorten rekenomgevingen ondersteund door Data Factory. 

1. **On-Demand**:  In dit geval wordt de computeromgeving volledig beheerd door Data Factory. Er wordt automatisch gemaakt door de Data Factory-service voordat een taak wordt verzonden om gegevens te verwerken en verwijderd wanneer de taak is voltooid. U kunt configureren en beheren van gedetailleerde instellingen van de on-demand compute-omgeving voor het uitvoeren van taak Clusterbeheer en acties uitvoeren van de bootstrap. 
2. **Voeg uw eigen**: In dit geval kunt u uw eigen IT-omgeving (bijvoorbeeld HDInsight-cluster) registreren als een gekoppelde service in Data Factory. De computeromgeving wordt beheerd door uzelf en de Data Factory-service gebruikt voor het uitvoeren van de activiteiten. 

Zie [gekoppelde Services berekenen](data-factory-compute-linked-services.md) artikel voor meer informatie over de compute-services die worden ondersteund door Data Factory. 

## <a name="summary"></a>Samenvatting
Azure Data Factory ondersteunt de volgende activiteiten voor gegevenstransformatie en de rekenomgevingen voor de activiteiten. De activiteiten voor gegevenstransformatie kunnen worden toegevoegd aan pijplijnen afzonderlijk of in een keten met een andere activiteit.

| Activiteiten voor gegevenstransformatie | Compute-omgeving |
|:--- |:--- |
| [Hive](data-factory-hive-activity.md) |HDInsight [Hadoop] |
| [Pig](data-factory-pig-activity.md) |HDInsight [Hadoop] |
| [MapReduce](data-factory-map-reduce.md) |HDInsight [Hadoop] |
| [Hadoop Streaming](data-factory-hadoop-streaming-activity.md) |HDInsight [Hadoop] |
| [Machine Learning-activiteiten: Batchuitvoering en resources bijwerken](data-factory-azure-ml-batch-execution-activity.md) |Azure VM |
| [Opgeslagen procedure](data-factory-stored-proc-activity.md) |Azure SQL, Azure SQL Data Warehouse of SQL Server |
| [Data Lake Analytics U-SQL](data-factory-usql-activity.md) |Azure Data Lake Analytics |
| [DotNet](data-factory-use-custom-activities.md) |HDInsight [Hadoop] of Azure Batch |

