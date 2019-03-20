---
title: Gegevens transformeren met behulp van Hive-activiteit - Azure | Microsoft Docs
description: Lees hoe u de Hive-activiteit in een Azure data factory kunt gebruiken om uit te voeren van Hive-query's op een on-demand/uw eigen HDInsight-cluster.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 80083218-743e-4da8-bdd2-60d1c77b1227
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 4b622a5925aebd140fed2ac74eaf7cc186803b90
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58113753"
---
# <a name="transform-data-using-hive-activity-in-azure-data-factory"></a>Gegevens transformeren met behulp van Hive-activiteit in Azure Data Factory 
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
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [gegevens transformeren met behulp van Hive-activiteit in Data Factory](../transform-data-using-hadoop-hive.md).

De HDInsight Hive-activiteit in een Data Factory [pijplijn](data-factory-create-pipelines.md) Hive-query's uitvoert op [uw eigen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of [op aanvraag](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux gebaseerd HDInsight-cluster. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) artikel een algemeen overzicht van de gegevenstransformatie van en de ondersteunde transformatieactiviteiten geeft.

> [!NOTE] 
> Als u niet bekend bent met Azure Data Factory, Lees [Inleiding tot Azure Data Factory](data-factory-introduction.md) en de zelfstudie: [Uw eerste pijplijn bouwen](data-factory-build-your-first-pipeline.md) voordat het lezen van dit artikel. 

## <a name="syntax"></a>Syntaxis

```JSON
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
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
      "script": "Hive script",
      "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
      "defines": {
        "param1": "param1Value"
      }
    },
   "scheduler": {
      "frequency": "Day",
      "interval": 1
    }
}
```
## <a name="syntax-details"></a>Syntaxis van de details
| Eigenschap | Description | Vereist |
| --- | --- | --- |
| naam |Naam van de activiteit |Ja |
| description |Beschrijving van het doel waarvoor de activiteit wordt gebruikt |Nee |
| type |HDinsightHive |Ja |
| invoer |Invoer gebruikt door de Hive-activiteit |Nee |
| uitvoer |Uitvoer geproduceerd door de Hive-activiteit |Ja |
| linkedServiceName |Verwijzing naar het HDInsight-cluster dat is geregistreerd als een gekoppelde service in Data Factory |Ja |
| Script |Geef de inline Hive-script |Nee |
| pad naar script |Het Hive-script Store in een Azure blob-opslag en het pad naar het bestand opgeven. Gebruik de eigenschap 'script' of 'scriptPath'. Beide kunnen niet samen worden gebruikt. De bestandsnaam is hoofdlettergevoelig. |Nee |
| Hiermee worden gedefinieerd |Geef parameters op als sleutel/waarde-paren voor verwijzende binnen het Hive-script met behulp van 'hiveconf' |Nee |

## <a name="example"></a>Voorbeeld
Laten we eens een voorbeeld van game-logboeken analytics waar u de tijd besteed aan het spelen van games die zijn gestart door uw bedrijf gebruikers identificeren. 

Het volgende logboek is een game voorbeeldlogboek, die door komma's (`,`) gescheiden en bevat de volgende velden: ProfileID, SessionStart, duur, SrcIPAddress en GameType.

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

De **Hive-script** voor het verwerken van deze gegevens:

```
DROP TABLE IF EXISTS HiveSampleIn; 
CREATE EXTERNAL TABLE HiveSampleIn 
(
    ProfileID        string, 
    SessionStart     string, 
    Duration         int, 
    SrcIPAddress     string, 
    GameType         string
) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 

DROP TABLE IF EXISTS HiveSampleOut; 
CREATE EXTERNAL TABLE HiveSampleOut 
(    
    ProfileID     string, 
    Duration     int
) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';

INSERT OVERWRITE TABLE HiveSampleOut
Select 
    ProfileID,
    SUM(Duration)
FROM HiveSampleIn Group by ProfileID
```

Als u wilt deze Hive-script worden uitgevoerd in een Data Factory-pijplijn, moet u het volgende doen

1. Een gekoppelde service te registreren [uw eigen HDInsight-berekeningscluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of Configureer [on-demand HDInsight-rekencluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Noemen we deze gekoppelde service 'HDInsightLinkedService'.
2. Maak een [gekoppelde service](data-factory-azure-blob-connector.md) om de verbinding naar Azure Blob storage die als host fungeert voor de gegevens te configureren. Noemen we deze gekoppelde service "StorageLinkedService"
3. Maak [gegevenssets](data-factory-create-datasets.md) die verwijst naar de invoer en de uitvoergegevens. Noemen we de invoergegevensset 'HiveSampleIn' en de uitvoergegevensset "HiveSampleOut"
4. Kopieer de Hive-query als een bestand naar Azure Blob-opslag geconfigureerd in stap #2. Als de opslag voor het hosten van de gegevens af van de versie die als host fungeert voor dit querybestand, een afzonderlijke gekoppelde Azure Storage-service maken en hiernaar wordt verwezen in de activiteit. Gebruik **scriptPath** om op te geven van het pad voor het hive-query-bestand en **scriptLinkedService** om op te geven van de Azure-opslag die het scriptbestand bevat. 
   
   > [!NOTE]
   > U kunt ook de inline Hive-script in het definitie van de activiteit opgeven met behulp van de **script** eigenschap. We raden niet aan deze methode als alle speciale tekens in het script in de JSON-document moet worden voorafgegaan en foutopsporing problemen kunnen veroorzaken. De aanbevolen procedure is stap #4.
   > 
   > 
5. Een pijplijn maken met de HDInsightHive-activiteit. De activiteit processen/transformaties de gegevens.

    ```JSON   
    {   
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                {
                    "name": "HiveSampleIn"
                }
                ],
                "outputs": [
                {
                    "name": "HiveSampleOut"
                }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                    "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                    "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
            ]
        }
    }
    ```
6. Implementeer de pijplijn. Zie [het maken van pijplijnen](data-factory-create-pipelines.md) artikel voor meer informatie. 
7. Bewaak de pijplijn met behulp van de data factory bewakings- en weergaven. Zie [controleren en beheren van Data Factory-pijplijnen](data-factory-monitor-manage-pipelines.md) artikel voor meer informatie. 

## <a name="specifying-parameters-for-a-hive-script"></a>Parameters voor een Hive-script op te geven
In dit voorbeeld, game-Logboeken in Azure Blob-opslag dagelijks worden opgenomen en worden opgeslagen in een map die is gepartitioneerd met de datum en tijd. U wilt voorzien van het Hive-script en de locatie van de invoer dynamisch doorgeven tijdens runtime en ook resulteren in de uitvoer die is gepartitioneerd met de datum en tijd.

Als u wilt gebruiken met parameters Hive-script, doet u het volgende

* Definieer de parameters in **definieert**.

    ```JSON  
    {
        "name": "HiveActivitySamplePipeline",
          "properties": {
        "activities": [
             {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                      {
                        "name": "HiveSampleIn"
                      }
                ],
                "outputs": [
                      {
                        "name": "HiveSampleOut"
                    }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                      "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                      "scriptLinkedService": "StorageLinkedService",
                      "defines": {
                        "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)",
                        "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
                      },
                       "scheduler": {
                          "frequency": "Hour",
                          "interval": 1
                    }
                }
              }
        ]
      }
    }
    ```
* In de Hive-Script, verwijzen naar de parameter met behulp van **${hiveconf:parameterName}**. 
  
    ```
    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID     string, 
        SessionStart     string, 
        Duration     int, 
        SrcIPAddress     string, 
        GameType     string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 

    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (
        ProfileID     string, 
        Duration     int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';

    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID
    ```
  ## <a name="see-also"></a>Zie ook
* [Pig-activiteit](data-factory-pig-activity.md)
* [MapReduce-activiteit](data-factory-map-reduce.md)
* [Hadoop-Streamingactiviteit](data-factory-hadoop-streaming-activity.md)
* [Spark-programma's aanroepen](data-factory-spark.md)
* [R-scripts aanroepen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

