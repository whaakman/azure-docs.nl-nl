---
title: Gegevens transformeren met behulp van Hadoop-Streamingactiviteit - Azure | Microsoft Docs
description: Lees hoe u de Hadoop-Streamingactiviteit in een Azure data factory kunt gebruiken om gegevens te transformeren door het uitvoeren van Hadoop-Streaming-programma's op een on-demand/uw eigen HDInsight-cluster.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 4c3ff8f2-2c00-434e-a416-06dfca2c41ec
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: f4bdeee08e81c16dfdd03620eb1fc61251f90400
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54025170"
---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a>Gegevens transformeren met behulp van Hadoop-Streamingactiviteit in Azure Data Factory
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-activiteit](data-factory-hive-activity.md) 
> * [Pig-activiteit](data-factory-pig-activity.md)
> * [MapReduce-activiteit](data-factory-map-reduce.md)
> * [Hadoop-Streamingactiviteit](data-factory-hadoop-streaming-activity.md)
> * [Spark-activiteit](data-factory-spark.md)
> * [Machine Learning-batchuitvoeringsactiviteit](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning-activiteit resources bijwerken](data-factory-azure-ml-update-resource-activity.md)
> * [Opgeslagen procedureactiviteit](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL-activiteit](data-factory-usql-activity.md)
> * [Aangepaste .NET-activiteit](data-factory-use-custom-activities.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [gegevens transformeren met behulp van Hadoop-streaming-activiteit in Data Factory](../transform-data-using-hadoop-streaming.md).


U kunt de activiteit HDInsightStreamingActivity aanroepen van een Hadoop-Streaming-taak van een Azure Data Factory-pijplijn. De volgende JSON-fragment ziet u de syntaxis voor het gebruik van de HDInsightStreamingActivity in een pijplijn-JSON-bestand. 

De HDInsight Streaming-activiteit in een Data Factory [pijplijn](data-factory-create-pipelines.md) uitvoeren van Hadoop-Streaming-programma's op [uw eigen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of [op aanvraag](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux gebaseerd HDInsight-cluster. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) artikel een algemeen overzicht van de gegevenstransformatie van en de ondersteunde transformatieactiviteiten geeft.

> [!NOTE] 
> Als u niet bekend bent met Azure Data Factory, Lees [Inleiding tot Azure Data Factory](data-factory-introduction.md) en de zelfstudie: [Uw eerste pijplijn bouwen](data-factory-build-your-first-pipeline.md) voordat het lezen van dit artikel. 

## <a name="json-sample"></a>JSON-voorbeeld
Het HDInsight-cluster wordt automatisch gevuld met voorbeeld van de programma's (wc.exe en cat.exe) en gegevens (davinci.txt). Naam van de container die wordt gebruikt door het HDInsight-cluster is standaard de naam van het cluster zelf. Bijvoorbeeld als de clusternaam van uw myhdicluster is, is de naam van de blob-container die zijn gekoppeld myhdicluster. 

```JSON
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": [
                        "<nameofthecluster>/example/apps/wc.exe",
                        "<nameofthecluster>/example/apps/cat.exe"
                    ],
                    "fileLinkedService": "AzureStorageLinkedService",
                    "getDebugInfo": "Failure"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-04T00:00:00Z",
        "end": "2014-01-05T00:00:00Z"
    }
}
```

Houd rekening met de volgende punten:

1. Stel de **linkedServiceName** op de naam van de gekoppelde service die naar uw HDInsight verwijst-cluster op waarop de streaming mapreduce-taak wordt uitgevoerd.
2. Instellen van het type van de activiteit in op **HDInsightStreaming**.
3. Voor de **mapper** eigenschap, geef de naam van uitvoerbaar bestand toewijzen. In het voorbeeld is cat.exe het toewijzen van de uitvoerbare.
4. Voor de **reducer** eigenschap, de naam van uitvoerbaar reducer opgeven. In het voorbeeld is wc.exe de uitvoerbare reducer.
5. Voor de **invoer** type-eigenschap, geeft u het invoerbestand (met inbegrip van de locatie) voor het toewijzen van de. In het voorbeeld: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample is de blob-container, gegevens-voorbeeld/Gutenberg is de map en davinci.txt wordt de blob.
6. Voor de **uitvoer** type-eigenschap, geeft u het uitvoerbestand (met inbegrip van de locatie) voor de reducer. De uitvoer van de Hadoop-Streaming-taak is geschreven naar de locatie die is opgegeven voor deze eigenschap.
7. In de **filePaths** sectie, geeft u de paden voor de toewijzing en reducer uitvoerbare bestanden. In het voorbeeld: "adfsample/example/apps/wc.exe" adfsample wordt de blob-container, voorbeeld-apps is de map en wc.exe is het uitvoerbare bestand.
8. Voor de **fileLinkedService** eigenschap opgeven van de gekoppelde Azure Storage-service die de Azure-opslag met de bestanden die zijn opgegeven in de sectie filePaths vertegenwoordigt.
9. Voor de **argumenten** eigenschap, geef de argumenten voor de streaming-taak.
10. De **getDebugInfo** eigenschap is een optioneel element. Wanneer deze is ingesteld op mislukt, worden de logboeken alleen bij fout gedownload. Wanneer deze is ingesteld op altijd, worden de logboeken altijd, ongeacht de uitvoeringsstatus gedownload.

> [!NOTE]
> Zoals u in het voorbeeld, u een uitvoergegevensset opgeven voor de Hadoop-Streamingactiviteit voor de **levert** eigenschap. Deze gegevensset is alleen een dummy gegevensset die is vereist om de planning van de pijplijn te stimuleren. U hoeft niet om op te geven van een invoergegevensset van de activiteit voor de **invoer** eigenschap.  
> 
> 

## <a name="example"></a>Voorbeeld
De pijplijn in dit scenario voert het aantal woorden-programma voor streaming toewijzen/verminderen in uw Azure HDInsight-cluster. 

### <a name="linked-services"></a>Gekoppelde services
#### <a name="azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service
U maakt eerst een gekoppelde service om te koppelen van de Azure-opslag die wordt gebruikt door de Azure HDInsight-cluster aan de Azure data factory. Vergeet niet accountnaam en accountsleutel vervangen door de naam en sleutel van uw Azure Storage als kopiëren/plakken van de volgende code. 

```JSON
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
        }
    }
}
```

#### <a name="azure-hdinsight-linked-service"></a>Azure HDInsight gekoppelde service
Vervolgens, maakt u een gekoppelde service om te koppelen van uw Azure HDInsight-cluster aan de Azure data factory. Als kopiëren/plakken van de volgende code vervangen door de naam van de HDInsight-cluster met de naam van uw HDInsight-cluster en waarden van gebruikersnaam en wachtwoord te wijzigen. 

```JSON
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
            "userName": "admin",
            "password": "**********",
            "linkedServiceName": "StorageLinkedService"
        }
    }
}
```

### <a name="datasets"></a>Gegevenssets
#### <a name="output-dataset"></a>Uitvoergegevensset
De pijplijn in dit voorbeeld neemt geen invoeren. U opgeven een uitvoergegevensset voor de HDInsight Streaming-activiteit. Deze gegevensset is alleen een dummy gegevensset die is vereist om de planning van de pijplijn te stimuleren. 

```JSON
{
    "name": "StreamingOutputDataset",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "adftutorial/streamingdata/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            },
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="pipeline"></a>Pijplijn
De pijplijn in dit voorbeeld heeft slechts één activiteit die is van het type: **HDInsightStreaming**. 

Het HDInsight-cluster wordt automatisch gevuld met voorbeeld van de programma's (wc.exe en cat.exe) en gegevens (davinci.txt). Naam van de container die wordt gebruikt door het HDInsight-cluster is standaard de naam van het cluster zelf. Bijvoorbeeld als de clusternaam van uw myhdicluster is, is de naam van de blob-container die zijn gekoppeld myhdicluster.  

```JSON
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": [
                        "<blobcontainer>/example/apps/wc.exe",
                        "<blobcontainer>/example/apps/cat.exe"
                    ],
                    "fileLinkedService": "StorageLinkedService"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-01-03T00:00:00Z",
        "end": "2017-01-04T00:00:00Z"
    }
}
```
## <a name="see-also"></a>Zie ook
* [Hive-activiteit](data-factory-hive-activity.md)
* [Pig-activiteit](data-factory-pig-activity.md)
* [MapReduce-activiteit](data-factory-map-reduce.md)
* [Spark-programma's aanroepen](data-factory-spark.md)
* [R-scripts aanroepen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

