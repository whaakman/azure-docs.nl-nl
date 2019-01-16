---
title: Gegevens transformeren met behulp van Pig-activiteit in Azure Data Factory | Microsoft Docs
description: Lees hoe u de Pig-activiteit in een Azure data factory kunt gebruiken om uit te voeren van Pig-scripts op een on-demand/uw eigen HDInsight-cluster.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 5af07a1a-2087-455e-a67b-a79841b4ada5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: fe1ef8c141c4a4daa443f800181f8e6e3199d0cc
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54331296"
---
# <a name="transform-data-using-pig-activity-in-azure-data-factory"></a>Gegevens transformeren met behulp van Pig-activiteit in Azure Data Factory
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
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [gegevens transformeren met behulp van Pig-activiteit in Data Factory](../transform-data-using-hadoop-pig.md).


De HDInsight Pig-activiteit in een Data Factory [pijplijn](data-factory-create-pipelines.md) Pig-query's uitvoert op [uw eigen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of [op aanvraag](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux gebaseerd HDInsight-cluster. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) artikel een algemeen overzicht van de gegevenstransformatie van en de ondersteunde transformatieactiviteiten geeft.

> [!NOTE] 
> Als u niet bekend bent met Azure Data Factory, Lees [Inleiding tot Azure Data Factory](data-factory-introduction.md) en de zelfstudie: [Uw eerste pijplijn bouwen](data-factory-build-your-first-pipeline.md) voordat het lezen van dit artikel. 

## <a name="syntax"></a>Syntaxis

```JSON
{
  "name": "HiveActivitySamplePipeline",
  "properties": {
    "activities": [
      {
        "name": "Pig Activity",
        "description": "description",
        "type": "HDInsightPig",
        "inputs": [
          {
            "name": "input tables"
          }
        ],
        "outputs": [
          {
            "name": "output tables"
          }
        ],
        "linkedServiceName": "MyHDInsightLinkedService",
        "typeProperties": {
          "script": "Pig script",
          "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
          "defines": {
            "param1": "param1Value"
          }
        },
        "scheduler": {
          "frequency": "Day",
          "interval": 1
        }
      }
    ]
  }
}
```

## <a name="syntax-details"></a>Syntaxis van de details
| Eigenschap | Description | Vereist |
| --- | --- | --- |
| naam |Naam van de activiteit |Ja |
| description |Beschrijving van het doel waarvoor de activiteit wordt gebruikt |Nee |
| type |HDinsightPig |Ja |
| invoer |Een of meer invoerwaarden die worden gebruikt door de Pig-activiteit |Nee |
| uitvoer |Een of meer uitvoer geproduceerd door de Pig-activiteit |Ja |
| linkedServiceName |Verwijzing naar het HDInsight-cluster dat is geregistreerd als een gekoppelde service in Data Factory |Ja |
| script |Geef de inline Pig-script |Nee |
| pad naar script |Store de Pig-script in een Azure blob-opslag en het pad naar het bestand opgeven. Gebruik de eigenschap 'script' of 'scriptPath'. Beide kunnen niet samen worden gebruikt. De bestandsnaam is hoofdlettergevoelig. |Nee |
| Hiermee worden gedefinieerd |Geef parameters op als sleutel/waarde-paren voor verwijzende binnen de Pig-script |Nee |

## <a name="example"></a>Voorbeeld
Laten we eens een voorbeeld van game-logboeken analytics waar u de tijd besteed aan het spelen van games die zijn gestart door uw bedrijf spelers aangegeven.

Het volgende voorbeeld game-logboek is een bestand gescheiden door een komma (,). Het bevat de volgende velden: ProfileID, SessionStart, duur, SrcIPAddress en GameType.

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

De **Pig-script** voor het verwerken van deze gegevens:

```
PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);

GroupProfile = Group PigSampleIn all;

PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);

Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');
```

Als u wilt deze Pig-script worden uitgevoerd in een Data Factory-pijplijn, voer de volgende stappen uit:

1. Een gekoppelde service te registreren [uw eigen HDInsight-berekeningscluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of Configureer [on-demand HDInsight-rekencluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Noemen we deze gekoppelde service **HDInsightLinkedService**.
2. Maak een [gekoppelde service](data-factory-azure-blob-connector.md) om de verbinding naar Azure Blob storage die als host fungeert voor de gegevens te configureren. Noemen we deze gekoppelde service **StorageLinkedService**.
3. Maak [gegevenssets](data-factory-create-datasets.md) die verwijst naar de invoer en de uitvoergegevens. Noemen we de invoergegevensset **PigSampleIn** en de uitvoergegevensset **PigSampleOut**.
4. Kopieer de Pig-query in een bestand met de Azure Blob-opslag geconfigureerd in stap #2. Als de Azure-opslag die als host fungeert voor de gegevens van de naam die als host fungeert voor de query-bestand verschilt, maakt u een afzonderlijke gekoppelde Azure Storage-service. Verwijzen naar de gekoppelde service in de activiteitsconfiguratie van de. Gebruik ** scriptPath ** om op te geven van het pad naar scriptbestand pig en **scriptLinkedService**. 
   
   > [!NOTE]
   > U kunt ook de inline Pig-script in het definitie van de activiteit opgeven met behulp van de **script** eigenschap. Echter, we raden niet aan deze methode als alle speciale tekens in het script moet worden weergegeven en foutopsporing problemen kunnen veroorzaken. De aanbevolen procedure is stap #4.
   >
   >
5. De pijplijn maken met de activiteit HDInsightPig. Deze activiteit wordt de invoergegevens verwerkt door het uitvoeren van Pig-script op HDInsight-cluster.

    ```JSON
    {
      "name": "PigActivitySamplePipeline",
      "properties": {
        "activities": [
          {
            "name": "PigActivitySample",
            "type": "HDInsightPig",
            "inputs": [
              {
                "name": "PigSampleIn"
              }
            ],
            "outputs": [
              {
                "name": "PigSampleOut"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "typeproperties": {
              "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
              "scriptLinkedService": "StorageLinkedService"
            },
            "scheduler": {
              "frequency": "Day",
              "interval": 1
            }
          }
        ]
      }
    }
    ```
6. Implementeer de pijplijn. Zie [het maken van pijplijnen](data-factory-create-pipelines.md) artikel voor meer informatie. 
7. Bewaak de pijplijn met behulp van de data factory bewakings- en weergaven. Zie [controleren en beheren van Data Factory-pijplijnen](data-factory-monitor-manage-pipelines.md) artikel voor meer informatie.

## <a name="specifying-parameters-for-a-pig-script"></a>Parameters voor een Pig-script op te geven
Houd rekening met het volgende voorbeeld: game-logboeken die in Azure Blob-opslag dagelijks wordt verwerkt en opgeslagen in een map gepartitioneerd op basis van datum en tijd. U wilt voorzien van de Pig-script en de locatie van de invoer dynamisch doorgeven tijdens runtime en ook resulteren in de uitvoer die is gepartitioneerd met de datum en tijd.

Als u wilt gebruiken met parameters Pig-script, het volgende doen:

* Definieer de parameters in **definieert**.

    ```JSON
    {
      "name": "PigActivitySamplePipeline",
      "properties": {
        "activities": [
          {
            "name": "PigActivitySample",
            "type": "HDInsightPig",
            "inputs": [
              {
                "name": "PigSampleIn"
              }
            ],
            "outputs": [
              {
                "name": "PigSampleOut"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "typeproperties": {
              "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
              "scriptLinkedService": "StorageLinkedService",
              "defines": {
                "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0:MM}/dayno={0: dd}/',SliceStart)",
                "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
              }
            },
            "scheduler": {
              "frequency": "Day",
              "interval": 1
            }
          }
        ]
      }
    }
    ```
* In de Pig-Script, verwijzen naar de parameters met '**$parameterName**' zoals wordt weergegeven in het volgende voorbeeld:

    ```
    PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);
    GroupProfile = Group PigSampleIn all;
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);
    Store PigSampleOut into '$Output' USING PigStorage (','); 
    ```

## <a name="see-also"></a>Zie ook
* [Hive-activiteit](data-factory-hive-activity.md)
* [MapReduce-activiteit](data-factory-map-reduce.md)
* [Hadoop-Streamingactiviteit](data-factory-hadoop-streaming-activity.md)
* [Spark-programma's aanroepen](data-factory-spark.md)
* [R-scripts aanroepen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
