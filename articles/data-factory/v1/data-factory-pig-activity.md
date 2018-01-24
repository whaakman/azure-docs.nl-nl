---
title: Transformeer gegevens met behulp van Pig-activiteit in Azure Data Factory | Microsoft Docs
description: Meer informatie over hoe u kunt de Pig-activiteit in een Azure data factory Pig-scripts uitvoeren op een op-verzoek/uw eigen HDInsight-cluster.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 5af07a1a-2087-455e-a67b-a79841b4ada5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: c309debf53cc22d102de740ffd2907257b9821d2
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="transform-data-using-pig-activity-in-azure-data-factory"></a>Transformeer gegevens met behulp van Pig-activiteit in Azure Data Factory
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
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [transformatie van gegevens met behulp van Pig-activiteit in de Data Factory versie 2](../transform-data-using-hadoop-pig.md).


De HDInsight Pig-activiteit in een Data Factory [pijplijn](data-factory-create-pipelines.md) Pig-query's uitvoert op [uw eigen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of [op aanvraag](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows, Linux-gebaseerde HDInsight-cluster. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) artikel, hetgeen een algemeen overzicht van gegevenstransformatie en de ondersteunde transformatieactiviteiten toont.

> [!NOTE] 
> Als u niet bekend met Azure Data Factory bent, Lees [Inleiding tot Azure Data Factory](data-factory-introduction.md) en voer de zelfstudie: [bouwen van uw eerste pijplijn voor gegevens](data-factory-build-your-first-pipeline.md) voordat u dit artikel leest. 

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
## <a name="syntax-details"></a>Details van de syntaxis
| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| naam |Naam van de activiteit |Ja |
| description |Beschrijving van wat de activiteit wordt gebruikt |Nee |
| type |HDinsightPig |Ja |
| Invoer |Een of meer invoerwaarden verbruikt door de activiteit Pig |Nee |
| uitvoer |Een of meer uitvoer geproduceerd door de Pig-activiteit |Ja |
| linkedServiceName |Verwijzing naar het HDInsight-cluster dat is geregistreerd als een gekoppelde service in de Data Factory |Ja |
| Script |Geef de inline Pig-script |Nee |
| scriptpad |De Pig-script opslaat in Azure blob storage en geef het pad naar het bestand. Gebruik de eigenschap 'script' of 'scriptPath'. Beide kunnen niet samen worden gebruikt. De bestandsnaam is hoofdlettergevoelig. |Nee |
| Hiermee worden gedefinieerd |Geef parameters op als sleutel-waardeparen voor verwijzende binnen de Pig-script |Nee |

## <a name="example"></a>Voorbeeld
Laten we eens een voorbeeld van een game logboeken analytics waar u de tijd dat door spelen van games gestart door uw bedrijf spelers besteed identificeren.

De volgende game voorbeeldlogboek is een bestand gescheiden door een komma (,). Het bevat de volgende velden: ProfileID, SessionStart, duur, SrcIPAddress en GameType.

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

De **varkens script** voor het verwerken van deze gegevens:

```
PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);

GroupProfile = Group PigSampleIn all;

PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);

Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');
```

Voor het uitvoeren van deze Pig-script in een Data Factory-pijplijn, kunt u de volgende stappen uitvoeren:

1. Maken van een gekoppelde service te registreren [uw eigen HDInsight berekeningscluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of Configureer [op aanvraag HDInsight berekeningscluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Laten we deze gekoppelde service aanroepen **HDInsightLinkedService**.
2. Maak een [gekoppelde service](data-factory-azure-blob-connector.md) voor het configureren van de verbinding met Azure-blobopslag die als host fungeert voor de gegevens. Laten we deze gekoppelde service aanroepen **StorageLinkedService**.
3. Maak [gegevenssets](data-factory-create-datasets.md) die verwijst naar de invoer- en uitvoergegevens. We bellen invoergegevensset **PigSampleIn** en de uitvoergegevensset **PigSampleOut**.
4. Kopieer de Pig-query in een bestand met de Azure Blob Storage in stap #2 hebt geconfigureerd. Als de Azure-opslag die als host fungeert voor de gegevens van de naam die als host fungeert voor het querybestand verschilt, maakt u een afzonderlijke gekoppelde Azure Storage-service. Raadpleeg de gekoppelde service in de configuratie van de activiteit. Gebruik ** scriptPath ** om het pad naar scriptbestand pig en **scriptLinkedService**. 
   
   > [!NOTE]
   > U kunt ook de Pig-script inline in de definitie van de activiteit opgeven met behulp van de **script** eigenschap. Maar we raden niet aan deze methode als alle speciale tekens in de script moet worden voorafgegaan en foutopsporing problemen kan veroorzaken. De aanbevolen procedure is stap #4.
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
6. Implementeer de pijplijn. Zie [pijplijnen maken](data-factory-create-pipelines.md) artikel voor meer informatie. 
7. De pijplijn met de data factory bewaking en beheer weergaven bewaken. Zie [controleren en beheren van de Data Factory-pijplijnen](data-factory-monitor-manage-pipelines.md) artikel voor meer informatie.

## <a name="specifying-parameters-for-a-pig-script"></a>Parameters voor een Pig-script opgeven
Bekijk het volgende voorbeeld: game logboeken zijn geconsumeerd dagelijks in Azure Blob Storage en opgeslagen in een map gepartitioneerde op basis van datum en tijd. U wilt voorzien van de Pig-script en de locatie van de invoer dynamisch doorgeven tijdens runtime en ook produceren de uitvoer die is gepartitioneerd met de datum en tijd.

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
* In de Pig-Script verwijzen naar de parameters met '**$parameterName**, zoals wordt weergegeven in het volgende voorbeeld:

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

