---
title: Transformeer gegevens met Hadoop-Streamingactiviteit - Azure | Microsoft Docs
description: Meer informatie over hoe u de Hadoop-Streamingactiviteit kunt gebruiken in een Azure data factory om gegevens te transformeren met Hadoop-Streaming programma's uitvoeren op een op-verzoek/uw eigen HDInsight-cluster.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 4c3ff8f2-2c00-434e-a416-06dfca2c41ec
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 9022b03af8c87651a552e7fd3f505156daa3924e
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a>Transformeer gegevens met Hadoop-Streamingactiviteit in Azure Data Factory
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
> * [Aangepaste activiteit .NET](data-factory-use-custom-activities.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [transformeer gegevens met Hadoop-streaming van activiteit in de Data Factory versie 2](../transform-data-using-hadoop-streaming.md).


U kunt de activiteit HDInsightStreamingActivity aanroepen van een Hadoop-Streaming-taak van een Azure Data Factory-pijplijn. De volgende JSON-fragment toont de syntaxis voor het gebruik van de HDInsightStreamingActivity in een pijplijn-JSON-bestand. 

De HDInsight Streaming-activiteit in een Data Factory [pijplijn](data-factory-create-pipelines.md) Hadoop-Streaming programma's worden uitgevoerd op [uw eigen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of [op aanvraag](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows, Linux-gebaseerde HDInsight-cluster. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) artikel, hetgeen een algemeen overzicht van gegevenstransformatie en de ondersteunde transformatieactiviteiten toont.

> [!NOTE] 
> Als u niet bekend met Azure Data Factory bent, Lees [Inleiding tot Azure Data Factory](data-factory-introduction.md) en voer de zelfstudie: [bouwen van uw eerste pijplijn voor gegevens](data-factory-build-your-first-pipeline.md) voordat u dit artikel leest. 

## <a name="json-sample"></a>JSON-voorbeeld
Het HDInsight-cluster wordt automatisch gevuld met de voorbeeld-programma's (wc.exe en cat.exe) en gegevens (davinci.txt). Naam van de container die wordt gebruikt door het HDInsight-cluster is standaard de naam van het cluster zelf. Als de clusternaam van uw myhdicluster is, zou de naam van de blob-container die is gekoppeld myhdicluster zijn. 

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

1. Stel de **linkedServiceName** naar de naam van de gekoppelde service die naar uw HDInsight verwijst-cluster op het streaming mapreduce-taak wordt uitgevoerd.
2. Het type van de activiteit instellen **HDInsightStreaming**.
3. Voor de **mapper** eigenschap, geef de naam van de uitvoerbare toewijzen. In het voorbeeld is cat.exe de uitvoerbare toewijzen.
4. Voor de **reducer** eigenschap, de naam van de uitvoerbare reducer opgeven. In het voorbeeld is wc.exe de uitvoerbare reducer.
5. Voor de **invoer** eigenschap type, geeft u het invoerbestand (inclusief de locatie) voor de toewijzing. In het voorbeeld: ' wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt ': adfsample is de blob-container, gegevens-voorbeeld/Gutenberg is de map en davinci.txt is de blob.
6. Voor de **uitvoer** eigenschap type, het uitvoerbestand (inclusief de locatie) voor de reducer opgeven. De uitvoer van de Hadoop-Streaming-taak wordt geschreven naar de locatie die is opgegeven voor deze eigenschap.
7. In de **filePaths** sectie, geeft u de paden voor de toewijzing en reducer uitvoerbare bestanden. In het voorbeeld: 'adfsample/example/apps/wc.exe' adfsample is de blob-container, bijvoorbeeld/apps is de map en wc.exe is het uitvoerbare bestand.
8. Voor de **fileLinkedService** -eigenschap geeft u de gekoppelde Azure Storage-service die de Azure-opslag met de bestanden die zijn opgegeven in de sectie filePaths vertegenwoordigt.
9. Voor de **argumenten** eigenschap, geef de argumenten voor de streaming-taak.
10. De **getDebugInfo** eigenschap is een optioneel element. Wanneer deze is ingesteld op mislukt, worden de logboeken gedownload alleen bij fouten. Wanneer deze is ingesteld op Always, worden altijd logboeken ongeacht de uitvoeringsstatus gedownload.

> [!NOTE]
> In het voorbeeld ziet u een uitvoergegevensset opgeven voor het Hadoop-Streamingactiviteit voor de **levert** eigenschap. Deze gegevensset is slechts een dummy gegevensset die is vereist voor het station van de planning van de pijplijn. U hoeft niet te geven van een invoergegevensset voor de activiteit voor het **invoer** eigenschap.  
> 
> 

## <a name="example"></a>Voorbeeld
De pijplijn in dit scenario voert het programma voor streaming kaart/verminderen van aantal woorden in uw Azure HDInsight-cluster. 

### <a name="linked-services"></a>Gekoppelde services
#### <a name="azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service
U maakt eerst een gekoppelde service voor het koppelen van de Azure-opslag die wordt gebruikt door de Azure HDInsight-cluster aan het Azure data factory. Vergeet niet te vervangen door de naam en sleutel van uw Azure Storage accountnaam en accountsleutel als kopiëren en plakken van de volgende code. 

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

#### <a name="azure-hdinsight-linked-service"></a>Azure gekoppelde HDInsight-service
Maak vervolgens een gekoppelde service voor uw Azure HDInsight-cluster koppelen aan de Azure-gegevensfactory. Als kopiëren en plakken van de volgende code vervangen door de naam van de HDInsight-cluster met de naam van uw HDInsight-cluster en waarden van gebruikersnaam en wachtwoord wijzigen. 

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
De pijplijn in dit voorbeeld vindt niet alle invoer. U kunt een uitvoergegevensset opgeven voor de activiteit voor het streamen van HDInsight. Deze gegevensset is slechts een dummy gegevensset die is vereist voor het station van de planning van de pijplijn. 

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

Het HDInsight-cluster wordt automatisch gevuld met de voorbeeld-programma's (wc.exe en cat.exe) en gegevens (davinci.txt). Naam van de container die wordt gebruikt door het HDInsight-cluster is standaard de naam van het cluster zelf. Als de clusternaam van uw myhdicluster is, zou de naam van de blob-container die is gekoppeld myhdicluster zijn.  

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

