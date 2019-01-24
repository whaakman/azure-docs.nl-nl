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
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: shlo
ms.openlocfilehash: 68cdabd8d6e5921eabaa200169c0523352461733
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54856941"
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

Eigenschap | Description | Toegestane waarden | Vereist
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
**Scenario:** Kopiëren van de dezelfde bron-bestand in Azure-Blob naar meerdere doelbestanden in Azure Blob.

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
**Scenario:** Een InnerPipeline binnen een ForEach-activiteit met Execute Pipeline-activiteit herhalen. De binnenste pijplijn kopieert met schemadefinities met parameters.

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

## <a name="aggregating-outputs"></a>Uitvoer verzamelen

Aan de cumulatieve uitvoer van __foreach__ activiteit,. Gebruik _Variable_s en _variabele toevoegen_ activiteit.

Declareer eerst een `array` _variabele_ in de pijplijn. Vervolgens aanroepen _variabele toevoegen_ activiteit binnen elk __foreach__ lus. U kunt vervolgens de aggregatie ophalen uit uw matrix.

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
