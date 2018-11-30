---
title: ForEach-activiteit in Azure Data Factory | Microsoft Docs
description: De voor elke activiteit voor definieert een herhalende controlestroom in de pijplijn. Het wordt gebruikt voor iteratie van een verzameling en opgegeven activiteiten uitvoeren.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: shlo
ms.openlocfilehash: e38a0ec39227b0064175c3c39d32bf87970ef9f5
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52423725"
---
# <a name="foreach-activity-in-azure-data-factory"></a>ForEach-activiteit in Azure Data Factory
De ForEach-activiteit definieert een herhalende controlestroom in de pijplijn. Deze activiteit wordt gebruikt om een verzameling te herhalen en voert opgegeven activiteiten uit in een lus. De lusimplementatie van deze activiteit is vergelijkbaar met Foreach-lusstructuur in computertalen.

## <a name="syntax"></a>Syntaxis
De eigenschappen worden verderop in dit artikel beschreven. De eigenschap items is een verzameling en elk item in de verzameling wordt aangeduid met behulp van de `@item()` zoals wordt weergegeven in de volgende syntaxis:  

```json
{  
   "name":"MyForEachActivityName",
   "type":"ForEach",
   "typeProperties":{  
      "isSequential":"true",
        "items": {
            "value": "@pipeline().parameters.mySinkDatasetFolderPathCollection",
            "type": "Expression"
        },
      "activities":[  
         {  
            "name":"MyCopyActivity",
            "type":"Copy",
            "typeProperties":{  
               ...
            },
            "inputs":[  
               {  
                  "referenceName":"MyDataset",
                  "type":"DatasetReference",
                  "parameters":{  
                     "MyFolderPath":"@pipeline().parameters.mySourceDatasetFolderPath"
                  }
               }
            ],
            "outputs":[  
               {  
                  "referenceName":"MyDataset",
                  "type":"DatasetReference",
                  "parameters":{  
                     "MyFolderPath":"@item()"
                  }
               }
            ]
         }
      ]
   }
}

```

## <a name="type-properties"></a>Type-eigenschappen

Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
naam | Naam van de voor elke activiteit. | Reeks | Ja
type | Moet worden ingesteld op **ForEach** | Reeks | Ja
isSequential | Hiermee geeft u op of de lus sequentieel of parallel moet worden uitgevoerd.  Maximaal 20 lus iteraties kan worden uitgevoerd in één keer parallel). Bijvoorbeeld, als u hebt een ForEach activiteit met iteratie van een kopieeractiviteit met 10 verschillende gegevensbronnen en sinks gegevenssets met **isSequential** ingesteld op False, alle exemplaren worden uitgevoerd in één keer. Standaardinstelling is False. <br/><br/> Als 'isSequential' is ingesteld op False, zorg ervoor dat er een juiste configuratie om uit te voeren van meerdere uitvoerbare bestanden. Deze eigenschap moet anders voorzichtig worden gebruikt om te vermijden kunt oplossen. Zie voor meer informatie, [parallelle uitvoering](#parallel-execution) sectie. | Booleaans | Nee. Standaardinstelling is False.
batchCount | Batch-aantal moet worden gebruikt voor het beheren van het aantal parallelle uitvoering (als isSequential is ingesteld op false). | Geheel getal (maximum 50) | Nee. Standaardwaarde is 20.
Items | Een expressie die als een JSON-matrix resultaat te worden herhaald. | Expressie (die resulteert in een JSON-matrix) | Ja
Activiteiten | De activiteiten worden uitgevoerd. | Lijst van activiteiten | Ja

## <a name="parallel-execution"></a>Parallelle uitvoering
Als **isSequential** is ingesteld op false, de activiteit gegevensbrontabellen loopt, in combinatie met een maximum van 20 gelijktijdige iteraties. Deze instelling moet voorzichtig worden gebruikt. Als de gelijktijdige iteraties naar dezelfde map, maar naar verschillende bestanden schrijft, is deze aanpak is niets mis mee. Als de gelijktijdige iteraties gelijktijdig naar de exacte hetzelfde bestand schrijft, wordt deze benadering waarschijnlijk veroorzaakt een fout. 

## <a name="iteration-expression-language"></a>Iteratie expressietaal
Geef in de ForEach-activiteit, een matrix te worden herhaald voor de eigenschap **items**. " Gebruik `@item()` om te herhalen met een één-opsomming in ForEach-activiteit. Bijvoorbeeld, als **items** is een matrix: [1, 2, 3], `@item()` retourneert 1 in de eerste versie 2 in de tweede iteratie en 3 in de derde iteratie.

## <a name="iterating-over-a-single-activity"></a>Iteratie van een enkele activiteit
**Scenario:** kopiëren uit het hetzelfde bronbestand in Azure-Blob naar meerdere doelbestanden in Azure Blob.

### <a name="pipeline-definition"></a>De pijplijndefinitie van de

```json
{
    "name": "<MyForEachPipeline>",
    "properties": {
        "activities": [
            {
                "name": "<MyForEachActivity>",
                "type": "ForEach",
                "typeProperties": {
                    "isSequential": "true",
                    "items": {
                        "value": "@pipeline().parameters.mySinkDatasetFolderPath",
                        "type": "Expression"
                    },
                    "activities": [
                        {
                            "name": "MyCopyActivity",
                            "type": "Copy",
                            "typeProperties": {
                                "source": {
                                    "type": "BlobSource",
                                    "recursive": "false"
                                },
                                "sink": {
                                    "type": "BlobSink",
                                    "copyBehavior": "PreserveHierarchy"
                                }
                            },
                            "inputs": [
                                {
                                    "referenceName": "<MyDataset>",
                                    "type": "DatasetReference",
                                    "parameters": {
                                        "MyFolderPath": "@pipeline().parameters.mySourceDatasetFolderPath"
                                    }
                                }
                            ],
                            "outputs": [
                                {
                                    "referenceName": "MyDataset",
                                    "type": "DatasetReference",
                                    "parameters": {
                                        "MyFolderPath": "@item()"
                                    }
                                }
                            ]
                        }
                    ]
                }
            }
        ],
        "parameters": {
            "mySourceDatasetFolderPath": {
                "type": "String"
            },
            "mySinkDatasetFolderPath": {
                "type": "String"
            }
        }
    }
}

```

### <a name="blob-dataset-definition"></a>De definitie van de BLOB-gegevensset

```json
{  
   "name":"<MyDataset>",
   "properties":{  
      "type":"AzureBlob",
      "typeProperties":{  
         "folderPath":{  
            "value":"@dataset().MyFolderPath",
            "type":"Expression"
         }
      },
      "linkedServiceName":{  
         "referenceName":"StorageLinkedService",
         "type":"LinkedServiceReference"
      },
      "parameters":{  
         "MyFolderPath":{  
            "type":"String"
         }
      }
   }
}

```

### <a name="run-parameter-values"></a>Parameterwaarden uitvoeren

```json
{
    "mySourceDatasetFolderPath": "input/",
    "mySinkDatasetFolderPath": [ "outputs/file1", "outputs/file2" ]
}

```

## <a name="iterate-over-multiple-activities"></a>Meerdere activiteiten herhalen
Het is mogelijk om te herhalen meerdere activiteiten (bijvoorbeeld: kopiëren en web activiteiten) in een ForEach-activiteit. In dit scenario is het raadzaam dat u uit meerdere activiteiten in afzonderlijke pijplijn abstracte. Vervolgens kunt u de [activiteit ExecutePipeline](control-flow-execute-pipeline-activity.md) in de pijplijn met ForEach-activiteit om aan te roepen van de afzonderlijke pijplijn met meerdere activiteiten. 


### <a name="syntax"></a>Syntaxis

```json
{
  "name": "masterPipeline",
  "properties": {
    "activities": [
      {
        "type": "ForEach",
        "name": "<MyForEachMultipleActivities>"
        "typeProperties": {
          "isSequential": true,
          "items": {
            ...
          },
          "activities": [
            {
              "type": "ExecutePipeline",
              "name": "<MyInnerPipeline>"
              "typeProperties": {
                "pipeline": {
                  "referenceName": "<copyHttpPipeline>",
                  "type": "PipelineReference"
                },
                "parameters": {
                  ...
                },
                "waitOnCompletion": true
              }
            }
          ]
        }
      }
    ],
    "parameters": {
      ...
    }
  }
}

```
### <a name="example"></a>Voorbeeld
**Scenario:** Iterate via een InnerPipeline binnen een ForEach-activiteit met Execute Pipeline-activiteit. De binnenste pijplijn kopieert met schemadefinities met parameters.

#### <a name="master-pipeline-definition"></a>De definitie van de master-pijplijn

```json
{
  "name": "masterPipeline",
  "properties": {
    "activities": [
      {
        "type": "ForEach",
        "name": "MyForEachActivity",
        "typeProperties": {
          "isSequential": true,
          "items": {
            "value": "@pipeline().parameters.inputtables",
            "type": "Expression"
          },
          "activities": [
            {
              "type": "ExecutePipeline",
              "typeProperties": {
                "pipeline": {
                  "referenceName": "InnerCopyPipeline",
                  "type": "PipelineReference"
                },
                "parameters": {
                  "sourceTableName": {
                    "value": "@item().SourceTable",
                    "type": "Expression"
                  },
                  "sourceTableStructure": {
                    "value": "@item().SourceTableStructure",
                    "type": "Expression"
                  },
                  "sinkTableName": {
                    "value": "@item().DestTable",
                    "type": "Expression"
                  },
                  "sinkTableStructure": {
                    "value": "@item().DestTableStructure",
                    "type": "Expression"
                  }
                },
                "waitOnCompletion": true
              },
              "name": "ExecuteCopyPipeline"
            }
          ]
        }
      }
    ],
    "parameters": {
      "inputtables": {
        "type": "Array"
      }
    }
  }
}

```

#### <a name="inner-pipeline-definition"></a>Binnenste pijplijndefinitie

```json
{
  "name": "InnerCopyPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            }
          },
          "sink": {
            "type": "SqlSink"
          }
        },
        "name": "CopyActivity",
        "inputs": [
          {
            "referenceName": "sqlSourceDataset",
            "parameters": {
              "SqlTableName": {
                "value": "@pipeline().parameters.sourceTableName",
                "type": "Expression"
              },
              "SqlTableStructure": {
                "value": "@pipeline().parameters.sourceTableStructure",
                "type": "Expression"
              }
            },
            "type": "DatasetReference"
          }
        ],
        "outputs": [
          {
            "referenceName": "sqlSinkDataset",
            "parameters": {
              "SqlTableName": {
                "value": "@pipeline().parameters.sinkTableName",
                "type": "Expression"
              },
              "SqlTableStructure": {
                "value": "@pipeline().parameters.sinkTableStructure",
                "type": "Expression"
              }
            },
            "type": "DatasetReference"
          }
        ]
      }
    ],
    "parameters": {
      "sourceTableName": {
        "type": "String"
      },
      "sourceTableStructure": {
        "type": "String"
      },
      "sinkTableName": {
        "type": "String"
      },
      "sinkTableStructure": {
        "type": "String"
      }
    }
  }
}

```

#### <a name="source-dataset-definition"></a>De definitie van de bron-gegevensset

```json
{
  "name": "sqlSourceDataset",
  "properties": {
    "type": "SqlServerTable",
    "typeProperties": {
      "tableName": {
        "value": "@dataset().SqlTableName",
        "type": "Expression"
      }
    },
    "structure": {
      "value": "@dataset().SqlTableStructure",
      "type": "Expression"
    },
    "linkedServiceName": {
      "referenceName": "sqlserverLS",
      "type": "LinkedServiceReference"
    },
    "parameters": {
      "SqlTableName": {
        "type": "String"
      },
      "SqlTableStructure": {
        "type": "String"
      }
    }
  }
}

```

#### <a name="sink-dataset-definition"></a>Sink gegevenssetdefinitie

```json
{
  "name": "sqlSinkDataSet",
  "properties": {
    "type": "AzureSqlTable",
    "typeProperties": {
      "tableName": {
        "value": "@dataset().SqlTableName",
        "type": "Expression"
      }
    },
    "structure": {
      "value": "@dataset().SqlTableStructure",
      "type": "Expression"
    },
    "linkedServiceName": {
      "referenceName": "azureSqlLS",
      "type": "LinkedServiceReference"
    },
    "parameters": {
      "SqlTableName": {
        "type": "String"
      },
      "SqlTableStructure": {
        "type": "String"
      }
    }
  }
}

```

#### <a name="master-pipeline-parameters"></a>Master pijplijnparameters
```json
{
    "inputtables": [
        {
            "SourceTable": "department",
            "SourceTableStructure": [
              {
                "name": "departmentid",
                "type": "int"
              },
              {
                "name": "departmentname",
                "type": "string"
              }
            ],
            "DestTable": "department2",
            "DestTableStructure": [
              {
                "name": "departmentid",
                "type": "int"
              },
              {
                "name": "departmentname",
                "type": "string"
              }
            ]
        }
    ]
    
}

```
## <a name="aggregating-metric-output"></a>Metrische uitvoer verzamelen
Expressie voor het verzamelen van de uitvoer van alle herhalingen van een ForEach `@activity('NameofInnerActivity')`. Bijvoorbeeld, als een ForEach-activiteit iteratie van een 'MyCopyActivity', de syntaxis van de zou zijn: `@activity('MyCopyActivity')`. De uitvoer is een matrix met elk artikel met gedetailleerde informatie over een specifieke versie.

> [!NOTE]
> Als u meer informatie over een specifieke versie wilt, de syntaxis is: `@activity('NameofInnerActivity')[0]` voor de laatste iteratie. Het nummer in de vierkante haken gebruiken voor toegang tot de specifieke herhaling van de matrix. Voor toegang tot een bepaalde eigenschap van een bepaalde iteratie, zou u: `@activity('NameofInnerActivity')[0].output` of `@activity('NameofInnerActivity')[0].pipelineName`.

**Details van de matrix uitvoer van alle herhalingen:**
```json
[    
    {      
        "pipelineName": "db1f7d2b-dbbd-4ea8-964e-0d9b2d3fe676",      
        "jobId": "a43766cb-ba13-4c68-923a-8349af9a76a3",      
        "activityRunId": "217526fa-0218-42f1-b85c-e0b4f7b170ce",      
        "linkedServiceName": "ADFService",      
        "status": "Succeeded",      
        "statusCode": null,      
        "output": 
            {        
                "progress": 100,        
                "loguri": null,        
                "dataRead": "6.00 Bytes",        
                "dataWritten": "6.00 Bytes",        
                "regionOrGateway": "West US",        
                "details": "Data Read: 6.00 Bytes, Written: 6.00 Bytes",        
                "copyDuration": "00:00:05",        
                "dataVolume": "6.00 Bytes",        
                "throughput": "1.16 Bytes/s",       
                 "totalDuration": "00:00:10"      
            },      
        "resumptionToken": 
            {       
                "ExecutionId": "217526fa-0218-42f1-b85c-e0b4f7b170ce",        
                "ResumptionToken": 
                    {          
                        "in progress": "217526fa-0218-42f1-b85c-e0b4f7b170ce/wu/cloud/"       
                    },        
                "ExtendedProperties": 
                    {          
                        "dataRead": "6.00 Bytes",          
                        "dataWritten": "6.00 Bytes",          
                        "regionOrGateway": "West US",          
                        "details": "Data Read: 6.00 Bytes, Written: 6.00 Bytes",          
                        "copyDuration": "00:00:05",          
                        "dataVolume": "6.00 Bytes",          
                        "throughput": "1.16 Bytes/s",          
                        "totalDuration": "00:00:10"        
                    }      
            },      
        "error": null,      
        "executionStartTime": "2017-08-01T04:17:27.5747275Z",      
        "executionEndTime": "2017-08-01T04:17:46.4224091Z",     
        "duration": "00:00:18.8476816"    
    },
    {      
        "pipelineName": "db1f7d2b-dbbd-4ea8-964e-0d9b2d3fe676",      
        "jobId": "54232-ba13-4c68-923a-8349af9a76a3",      
        "activityRunId": "217526fa-0218-42f1-b85c-e0b4f7b170ce",      
        "linkedServiceName": "ADFService",      
        "status": "Succeeded",      
        "statusCode": null,      
        "output": 
            {        
                "progress": 100,        
                "loguri": null,        
                "dataRead": "6.00 Bytes",        
                "dataWritten": "6.00 Bytes",        
                "regionOrGateway": "West US",        
                "details": "Data Read: 6.00 Bytes, Written: 6.00 Bytes",        
                "copyDuration": "00:00:05",        
                "dataVolume": "6.00 Bytes",        
                "throughput": "1.16 Bytes/s",       
                 "totalDuration": "00:00:10"      
            },      
        "resumptionToken": 
            {       
                "ExecutionId": "217526fa-0218-42f1-b85c-e0b4f7b170ce",        
                "ResumptionToken": 
                    {          
                        "in progress": "217526fa-0218-42f1-b85c-e0b4f7b170ce/wu/cloud/"       
                    },        
                "ExtendedProperties": 
                    {          
                        "dataRead": "6.00 Bytes",          
                        "dataWritten": "6.00 Bytes",          
                        "regionOrGateway": "West US",          
                        "details": "Data Read: 6.00 Bytes, Written: 6.00 Bytes",          
                        "copyDuration": "00:00:05",          
                        "dataVolume": "6.00 Bytes",          
                        "throughput": "1.16 Bytes/s",          
                        "totalDuration": "00:00:10"        
                    }      
            },      
        "error": null,      
        "executionStartTime": "2017-08-01T04:18:27.5747275Z",      
        "executionEndTime": "2017-08-01T04:18:46.4224091Z",     
        "duration": "00:00:18.8476816"    
    }
]

```

## <a name="limitations-and-workarounds"></a>Beperkingen en tijdelijke oplossingen

Hier zijn enkele beperkingen van de ForEach-activiteit en voorgestelde oplossingen.

| Beperking | Tijdelijke oplossing |
|---|---|
| Een ForEach-lus in een andere ForEach-lus (of een Until-lus) kunnen niet worden genest. | Ontwerp een twee niveaus pijplijn waar de buitenste pijplijn met de buitenste ForEach-lus over een binnenste pijplijn met de geneste lus doorloopt. |
| De ForEach-activiteit heeft een maximale `batchCount` van 50 voor parallelle verwerking en maximaal 100.000 items. | Ontwerp een twee niveaus pijplijn waar de buitenste pijplijn met de ForEach-activiteit over een binnenste-pijplijn doorloopt. |
| | |

## <a name="next-steps"></a>Volgende stappen
Zie andere controlestroomactiviteiten die door Data Factory worden ondersteund: 

- [Execute Pipeline Activity](control-flow-execute-pipeline-activity.md)
- [Get Metadata Activity](control-flow-get-metadata-activity.md)
- [Lookup Activity](control-flow-lookup-activity.md)
- [Webactiviteit](control-flow-web-activity.md)
