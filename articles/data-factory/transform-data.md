---
title: Gegevens transformeren met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het transformeren van gegevens of gegevens verwerken in Azure Data Factory met behulp van Hadoop-, Machine Learning- of Azure Data Lake Analytics.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/31/2018
ms.author: douglasl
ms.openlocfilehash: 09b5f387b0c7f765ea0835dcef79932203d68338
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54014349"
---
# <a name="transform-data-in-azure-data-factory"></a>Transformeer gegevens in Azure Data Factory
> [!div class="op_single_selector"]
> * [Hive](transform-data-using-hadoop-hive.md)  
> * [Pig](transform-data-using-hadoop-pig.md)  
> * [MapReduce](transform-data-using-hadoop-map-reduce.md)  
> * [HDInsight Streaming](transform-data-using-hadoop-streaming.md)
> * [HDInsight Spark](transform-data-using-spark.md)
> * [Machine Learning](transform-data-using-machine-learning.md) 
> * [Opgeslagen procedure](transform-data-using-stored-procedure.md)
> * [Data Lake Analytics U-SQL](transform-data-using-data-lake-analytics.md)
> * [Databricks-notebook](transform-data-databricks-notebook.md)
> * [Databricks-Jar](transform-data-databricks-jar.md)
> * [Databricks-Python](transform-data-databricks-python.md)
> * [Aangepaste .NET](transform-data-using-dotnet-custom-activity.md)

## <a name="overview"></a>Overzicht
In dit artikel wordt uitgelegd activiteiten voor gegevenstransformatie in Azure Data Factory, dat u gebruiken kunt om te zetten en de onbewerkte gegevens in voorspellingen en inzichten verwerkt. Een transformatieactiviteit wordt uitgevoerd in een computeromgeving, zoals Azure HDInsight-cluster of een Azure Batch. Het bevat koppelingen naar artikelen met gedetailleerde informatie over elke transformatieactiviteit.

Data Factory ondersteunt de volgende activiteiten voor gegevenstransformatie die kunnen worden toegevoegd aan [pijplijnen](concepts-pipelines-activities.md) ofwel afzonderlijk of in een keten met een andere activiteit.

## <a name="hdinsight-hive-activity"></a>HDInsight Hive-activiteit
De HDInsight Hive-activiteit in een Data Factory-pijplijn voert een Hive-query's op uw eigen of Windows/Linux gebaseerd HDInsight-cluster op aanvraag. Zie [Hive-activiteit](transform-data-using-hadoop-hive.md) artikel voor meer informatie over deze activiteit. 

## <a name="hdinsight-pig-activity"></a>HDInsight Pig-activiteit
De HDInsight Pig-activiteit in een Data Factory-pijplijn voert Pig-query's op uw eigen of Windows/Linux gebaseerd HDInsight-cluster op aanvraag. Zie [Pig-activiteit](transform-data-using-hadoop-pig.md) artikel voor meer informatie over deze activiteit. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce-activiteit
De HDInsight MapReduce-activiteit in een Data Factory-pijplijn voert MapReduce-programma's op uw eigen of Windows/Linux gebaseerd HDInsight-cluster op aanvraag. Zie [MapReduce-activiteit](transform-data-using-hadoop-map-reduce.md) artikel voor meer informatie over deze activiteit.

## <a name="hdinsight-streaming-activity"></a>HDInsight Streaming-activiteit
De HDInsight Streaming-activiteit in een Data Factory-pijplijn voert Hadoop-Streaming-programma's op uw eigen of Windows/Linux gebaseerd HDInsight-cluster op aanvraag. Zie [HDInsight Streaming-activiteit](transform-data-using-hadoop-streaming.md) voor meer informatie over deze activiteit.

## <a name="hdinsight-spark-activity"></a>HDInsight Spark-activiteit
De HDInsight Spark-activiteit in een Data Factory-pijplijn Spark-programma's op uw eigen HDInsight-cluster wordt uitgevoerd. Zie voor meer informatie, [aanroepen Spark-programma's van Azure Data Factory](transform-data-using-spark.md). 

## <a name="machine-learning-activities"></a>Machine Learning-activiteiten
Azure Data Factory kunt u eenvoudig pijplijnen die gebruikmaken van een gepubliceerde Azure Machine Learning-webservice voor voorspellende analyses te maken. Met behulp van de [Batch Execution-activiteit](transform-data-using-machine-learning.md) in een Azure Data Factory-pijplijn, kunt u een Machine Learning-webservice om voorspellingen te maken van de gegevens in batch aanroepen.

De voorspellende modellen in de Machine Learning experimenten scoren moeten opnieuw worden getraind met behulp van nieuwe invoergegevenssets na verloop van tijd. Als u klaar bent met het opnieuw trainen, die u wilt de scoringwebservice bijwerken met de retrained Machine Learning-model. U kunt de [activiteit resources bijwerken](update-machine-learning-models.md) het bijwerken van de webservice met het zojuist getrainde model.  

Zie [gebruik Machine Learning-activiteiten](transform-data-using-machine-learning.md) voor meer informatie over deze Machine Learning-activiteiten. 

## <a name="stored-procedure-activity"></a>Opgeslagen procedureactiviteit
U kunt de SQL Server opgeslagen Procedure-activiteit in een Data Factory-pijplijn gebruiken om aan te roepen een opgeslagen procedure in een van de volgende gegevensarchieven: Azure SQL-Database, Azure SQL Data Warehouse, SQL Server-Database in uw onderneming of een Azure-VM. Zie [Stored Procedure-activiteit](transform-data-using-stored-procedure.md) artikel voor meer informatie.  

## <a name="data-lake-analytics-u-sql-activity"></a>U-SQL-activiteit van Data Lake Analytics
Data Lake Analytics U-SQL-activiteit wordt een U-SQL-script uitgevoerd op een Azure Data Lake Analytics-cluster. Zie [Data Analytics U-SQL-activiteit](transform-data-using-data-lake-analytics.md) artikel voor meer informatie. 

## <a name="databricks-notebook-activity"></a>Databricks Notebook-activiteit

De Azure Databricks Notebook-activiteit in een Data Factory-pijplijn voert een Databricks-notebook in uw Azure Databricks-werkruimte. Azure Databricks is een beheerd platform voor het uitvoeren van Apache Spark. Zie [transformeren van gegevens door te voeren een Databricks-notebook](transform-data-databricks-notebook.md).

## <a name="databricks-jar-activity"></a>Databricks-Jar-activiteit

De Azure Databricks Jar-activiteit in een Data Factory-pijplijn voert een Spark-Jar in uw Azure Databricks-cluster. Azure Databricks is een beheerd platform voor het uitvoeren van Apache Spark. Zie [gegevens transformeren met behulp van een Jar-activiteit in Azure Databricks](transform-data-databricks-jar.md).

## <a name="databricks-python-activity"></a>Databricks-Python-activiteit

De Azure Databricks Python-activiteit in een Data Factory-pijplijn voert een Python-bestand in uw Azure Databricks-cluster. Azure Databricks is een beheerd platform voor het uitvoeren van Apache Spark. Zie [gegevens transformeren met behulp van een Python-activiteit in Azure Databricks](transform-data-databricks-python.md).

## <a name="custom-activity"></a>Aangepaste activiteit
Als u nodig hebt voor het transformeren van gegevens op een manier die niet wordt ondersteund door Data Factory, kunt u een aangepaste activiteit maken met uw eigen logica gegevensverwerking en het gebruik van de activiteit in de pijplijn. U kunt de aangepaste .NET-activiteit om uit te voeren met behulp van een Azure Batch-service of een Azure HDInsight-cluster configureren. Zie [aangepaste activiteiten gebruiken](transform-data-using-dotnet-custom-activity.md) artikel voor meer informatie. 

U kunt een aangepaste activiteit maken om R-scripts uit te voeren op uw HDInsight-cluster waarop R is geïnstalleerd. Zie [R-script uitvoeren met behulp van Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>COMPUTE-omgevingen
U maakt een gekoppelde service voor de compute-omgeving en de gekoppelde service vervolgens gebruiken bij het definiëren van een transformatieactiviteit. Er zijn twee soorten rekenomgevingen ondersteund door Data Factory. 

- **On-Demand**:  In dit geval wordt de computeromgeving volledig beheerd door Data Factory. Er wordt automatisch gemaakt door de Data Factory-service voordat een taak wordt verzonden om gegevens te verwerken en verwijderd wanneer de taak is voltooid. U kunt configureren en beheren van gedetailleerde instellingen van de on-demand compute-omgeving voor het uitvoeren van taak Clusterbeheer en acties uitvoeren van de bootstrap. 
- **Voeg uw eigen**: In dit geval kunt u uw eigen IT-omgeving (bijvoorbeeld HDInsight-cluster) registreren als een gekoppelde service in Data Factory. De computeromgeving wordt beheerd door uzelf en de Data Factory-service gebruikt voor het uitvoeren van de activiteiten. 

Zie [gekoppelde Services berekenen](compute-linked-services.md) artikel voor meer informatie over de compute-services die worden ondersteund door Data Factory. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende zelfstudie voor een voorbeeld van het gebruik van een transformatieactiviteit: [Zelfstudie: gegevens transformeren met Spark](tutorial-transform-data-spark-powershell.md)
