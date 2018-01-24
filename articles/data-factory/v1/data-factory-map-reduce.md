---
title: MapReduce-programma uit Azure Data Factory aanroepen
description: Informatie over het verwerken van gegevens met MapReduce-programma's uitvoeren op een Azure HDInsight-cluster uit een Azure data factory.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: c34db93f-570a-44f1-a7d6-00390f4dc0fa
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 5961395f5ca4e9a6efb019a77dbdf5db5ff1ee38
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="invoke-mapreduce-programs-from-data-factory"></a>Aanroepen van MapReduce-programma's uit Data Factory
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
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [transformatie van gegevens met behulp van MapReduce-activiteit in de Data Factory versie 2](../transform-data-using-hadoop-map-reduce.md).


De HDInsight MapReduce-activiteit in een Data Factory [pijplijn](data-factory-create-pipelines.md) MapReduce-programma's worden uitgevoerd op [uw eigen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of [op aanvraag](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows, Linux-gebaseerde HDInsight-cluster. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) artikel, hetgeen een algemeen overzicht van gegevenstransformatie en de ondersteunde transformatieactiviteiten toont.

> [!NOTE] 
> Als u niet bekend met Azure Data Factory bent, Lees [Inleiding tot Azure Data Factory](data-factory-introduction.md) en voer de zelfstudie: [bouwen van uw eerste pijplijn voor gegevens](data-factory-build-your-first-pipeline.md) voordat u dit artikel leest.  

## <a name="introduction"></a>Inleiding
Een pijplijn in een Azure data factory verwerkt gegevens in gekoppelde storage-services met behulp van gekoppelde compute services. Het bevat een reeks activiteiten, waarbij elke activiteit uitvoert voor een specifieke verwerking. In dit artikel wordt beschreven hoe de HDInsight-MapReduce-activiteit.

Zie [Pig](data-factory-pig-activity.md) en [Hive](data-factory-hive-activity.md) voor meer informatie over het uitvoeren van Pig/Hive scripts op een Windows, Linux-gebaseerde HDInsight-cluster van een pijplijn met behulp van HDInsight Pig en Hive-activiteiten. 

## <a name="json-for-hdinsight-mapreduce-activity"></a>JSON voor HDInsight MapReduce-activiteit
In de JSON-definitie voor de HDInsight-activiteit: 

1. Stel de **type** van de **activiteit** naar **HDInsight**.
2. Geef de naam van de klasse voor **className** eigenschap.
3. Geef het pad naar het JAR-bestand dat is inclusief de bestandsnaam voor **jarFilePath** eigenschap.
4. Geef de gekoppelde service die verwijst naar de Azure Blob Storage met het JAR-bestand voor **jarLinkedService** eigenschap.   
5. Geef geen argumenten voor de MapReduce-programma in de **argumenten** sectie. Tijdens runtime, ziet u enkele extra argumenten (bijvoorbeeld: mapreduce.job.tags) van het MapReduce-framework. Overweeg het gebruik van zowel de optie als de waarde als argumenten zoals weergegeven in het volgende voorbeeld om te onderscheiden van de argumenten met de argumenten MapReduce, (- s,--invoer,--uitvoer enz., zijn opties onmiddellijk wordt gevolgd door hun waarden).

    ```JSON   
    {
        "name": "MahoutMapReduceSamplePipeline",
        "properties": {
            "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix to determine the similarity between 2 items",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                        "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "-s",
                            "SIMILARITY_LOGLIKELIHOOD",
                            "--input",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                            "--output",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                            "--maxSimilaritiesPerItem",
                            "500",
                            "--tempDir",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                        ]
                    },
                    "inputs": [
                        {
                            "name": "MahoutInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "MahoutOutput"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "MahoutActivity",
                    "description": "Custom Map Reduce to generate Mahout result",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-01-03T00:00:00Z",
            "end": "2017-01-04T00:00:00Z"
        }
    }
    ```
U kunt de HDInsight-MapReduce-activiteit gebruiken een MapReduce jar-bestand op een HDInsight-cluster uit te voeren. In het volgende voorbeeld JSON-definitie van een pijplijn, worden de HDInsight-activiteit is geconfigureerd voor het uitvoeren van een Mahout JAR-bestand.

## <a name="sample-on-github"></a>Voorbeeld op GitHub
U kunt een voorbeeld voor het gebruik van de HDInsight-MapReduce-activiteit uit downloaden: [Data Factory-voorbeelden op GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).  

## <a name="running-the-word-count-program"></a>Het aantal woorden programma uitvoeren
De pijplijn in dit voorbeeld voert het programma Word-Count kaart/verkleinen op Azure HDInsight-cluster.   

### <a name="linked-services"></a>Gekoppelde services
U maakt eerst een gekoppelde service voor het koppelen van de Azure-opslag die wordt gebruikt door de Azure HDInsight-cluster aan het Azure data factory. Als kopiëren en plakken van de volgende code vergeet niet om te vervangen **accountnaam** en **accountsleutel** met de naam en sleutel van uw Azure-opslag. 

#### <a name="azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service

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
Maak vervolgens een gekoppelde service voor uw Azure HDInsight-cluster koppelen aan de Azure-gegevensfactory. Als kopiëren en plakken van de volgende code vervangt **de naam van de HDInsight-cluster** met de naam van uw HDInsight-cluster en de wijziging waarden gebruikersnaam en wachtwoord.   

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
De pijplijn in dit voorbeeld vindt niet alle invoer. U kunt een uitvoergegevensset opgeven voor het HDInsight-MapReduce-activiteit. Deze gegevensset is slechts een dummy gegevensset die is vereist voor het station van de planning van de pijplijn.  

```JSON
{
    "name": "MROutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "WordCountOutput1.txt",
            "folderPath": "example/data/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="pipeline"></a>Pijplijn
De pijplijn in dit voorbeeld heeft slechts één activiteit die is van het type: HDInsightMapReduce. Enkele van de belangrijke eigenschappen in de JSON zijn: 

| Eigenschap | Opmerkingen |
|:--- |:--- |
| type |Het type moet worden ingesteld op **HDInsightMapReduce**. |
| className |Naam van de klasse is: **wordcount** |
| jarFilePath |Pad naar het jar-bestand met de klasse. Vergeet niet de naam van het cluster wijzigen als kopiëren en plakken van de volgende code. |
| jarLinkedService |Gekoppelde Azure Storage-service die het jar-bestand bevat. Deze gekoppelde service verwijst naar de opslag die is gekoppeld aan het HDInsight-cluster. |
| Argumenten |Het programma wordcount heeft twee argumenten, invoer en uitvoer. Het invoerbestand is het bestand davinci.txt. |
| frequency/interval |De waarden voor deze eigenschappen overeen met de uitvoergegevensset. |
| linkedServiceName |verwijst naar de gekoppelde HDInsight-service die u eerder hebt gemaakt. |

```JSON
{
    "name": "MRSamplePipeline",
    "properties": {
        "description": "Sample Pipeline to Run the Word Count Program",
        "activities": [
            {
                "type": "HDInsightMapReduce",
                "typeProperties": {
                    "className": "wordcount",
                    "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                    "jarLinkedService": "StorageLinkedService",
                    "arguments": [
                        "/example/data/gutenberg/davinci.txt",
                        "/example/data/WordCountOutput1"
                    ]
                },
                "outputs": [
                    {
                        "name": "MROutput"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "MRActivity",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-03T00:00:00Z",
        "end": "2014-01-04T00:00:00Z"
    }
}
```

## <a name="run-spark-programs"></a>Spark-programma's uitvoeren
U kunt de MapReduce-activiteit gebruiken om Spark-programma's uit te voeren op uw HDInsight Spark-cluster. Zie [Spark-programma's aanroepen vanuit Azure Data Factory](data-factory-spark.md) voor meer informatie.  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com

## <a name="see-also"></a>Zie ook
* [Hive-activiteit](data-factory-hive-activity.md)
* [Pig-activiteit](data-factory-pig-activity.md)
* [Hadoop-Streamingactiviteit](data-factory-hadoop-streaming-activity.md)
* [Spark-programma's aanroepen](data-factory-spark.md)
* [R-scripts aanroepen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

