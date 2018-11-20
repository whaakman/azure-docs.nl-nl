---
title: Execute Pipeline-activiteit in Azure Data Factory | Microsoft Docs
description: Lees hoe u de Execute Pipeline-activiteit kunt gebruiken om aan te roepen een Data Factory-pijplijn vanaf een andere Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: aace01fedd0c2ab538d4e11b418907f962128d0e
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52163115"
---
# <a name="execute-pipeline-activity-in-azure-data-factory"></a>Execute Pipeline-activiteit in Azure Data Factory
De Execute Pipeline-activiteit kunt een Data Factory-pijplijn om aan te roepen een andere pijplijn.

## <a name="syntax"></a>Syntaxis

```json
{
    "name": "MyPipeline",
    "properties": {
        "activities": [
            {
                "name": "ExecutePipelineActivity",
                "type": "ExecutePipeline",
                "typeProperties": {
                    "parameters": {                        
                        "mySourceDatasetFolderPath": {
                            "value": "@pipeline().parameters.mySourceDatasetFolderPath",
                            "type": "Expression"
                        }
                    },
                    "pipeline": {
                        "referenceName": "<InvokedPipelineName>",
                        "type": "PipelineReference"
                    },
                    "waitOnCompletion": true
                 }
            }
        ],
        "parameters": [
            {
                "mySourceDatasetFolderPath": {
                    "type": "String"
                }
            }
        ]
    }
}
```

## <a name="type-properties"></a>Type-eigenschappen
Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
naam | De naam van de execute pipeline-activiteit. | Reeks | Ja
type | Moet worden ingesteld op: **ExecutePipeline**. | Reeks | Ja
pijplijn | Pijplijn verwijzing naar de afhankelijke pijplijn die deze pijplijn aanroept. Een pijplijn reference-object heeft twee eigenschappen: **verwijzing** en **type**. De eigenschap verwijzing geeft de naam van de referentie-pijplijn. De eigenschap type moet worden ingesteld op PipelineReference. | PipelineReference | Ja
parameters | Parameters worden doorgegeven aan de pijplijn aangeroepen | Een JSON-object dat de namen van parameters wordt toegewezen aan de waarden van het argument | Nee
waitOnCompletion | Bepaalt of de uitvoeringsomgeving van de activiteit moet wachten voor de uitvoering van de afhankelijke pipeline om te voltooien. | De standaardinstelling is onwaar. | Booleaans | Nee

## <a name="sample"></a>Voorbeeld
In dit scenario heeft twee pijplijnen:

- **Master pijplijn** -deze pijplijn heeft één Execute Pipeline-activiteit die de aangeroepen pijplijn aanroept. De master pijplijn heeft twee parameters: `masterSourceBlobContainer`, `masterSinkBlobContainer`.
- **Aangeroepen pijplijn** -deze pijplijn heeft één kopieeractiviteit waarmee gegevens worden gekopieerd van de bron van een Azure-Blob naar Azure Blob-sink. De aangeroepen pijplijn heeft twee parameters: `sourceBlobContainer`, `sinkBlobContainer`.

### <a name="master-pipeline-definition"></a>Master pijplijndefinitie

```json
{
  "name": "masterPipeline",
  "properties": {
    "activities": [
      {
        "type": "ExecutePipeline",
        "typeProperties": {
          "pipeline": {
            "referenceName": "invokedPipeline",
            "type": "PipelineReference"
          },
          "parameters": {
            "sourceBlobContainer": {
              "value": "@pipeline().parameters.masterSourceBlobContainer",
              "type": "Expression"
            },
            "sinkBlobContainer": {
              "value": "@pipeline().parameters.masterSinkBlobContainer",
              "type": "Expression"
            }
          },
          "waitOnCompletion": true
        },
        "name": "MyExecutePipelineActivity"
      }
    ],
    "parameters": {
      "masterSourceBlobContainer": {
        "type": "String"
      },
      "masterSinkBlobContainer": {
        "type": "String"
      }
    }
  }
}

```

### <a name="invoked-pipeline-definition"></a>Aangeroepen pijplijndefinitie

```json
{
  "name": "invokedPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
        "name": "CopyBlobtoBlob",
        "inputs": [
          {
            "referenceName": "SourceBlobDataset",
            "type": "DatasetReference"
          }
        ],
        "outputs": [
          {
            "referenceName": "sinkBlobDataset",
            "type": "DatasetReference"
          }
        ]
      }
    ],
    "parameters": {
      "sourceBlobContainer": {
        "type": "String"
      },
      "sinkBlobContainer": {
        "type": "String"
      }
    }
  }
}

```

**Gekoppelde service**

```json
{
    "name": "BlobStorageLinkedService",
    "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": {
        "value": "DefaultEndpointsProtocol=https;AccountName=*****",
        "type": "SecureString"
      }
    }
  }
}
```

**Brongegevensset**
```json
{
    "name": "SourceBlobDataset",
    "properties": {
    "type": "AzureBlob",
    "typeProperties": {
      "folderPath": {
        "value": "@pipeline().parameters.sourceBlobContainer",
        "type": "Expression"
      },
      "fileName": "salesforce.txt"
    },
    "linkedServiceName": {
      "referenceName": "BlobStorageLinkedService",
      "type": "LinkedServiceReference"
    }
  }
}
```

**Sink-gegevensset**
```json
{
    "name": "sinkBlobDataset",
    "properties": {
    "type": "AzureBlob",
    "typeProperties": {
      "folderPath": {
        "value": "@pipeline().parameters.sinkBlobContainer",
        "type": "Expression"
      }
    },
    "linkedServiceName": {
      "referenceName": "BlobStorageLinkedService",
      "type": "LinkedServiceReference"
    }
  }
}
```

### <a name="running-the-pipeline"></a>De pijplijn

Als u wilt de master pijplijn in dit voorbeeld uitvoert, worden de volgende waarden voor de parameters masterSourceBlobContainer en masterSinkBlobContainer doorgegeven: 

```json
{
  "masterSourceBlobContainer": "executetest",
  "masterSinkBlobContainer": "executesink"
}
```

De master pijplijn verzendt deze waarden aan de pijplijn aangeroepen, zoals wordt weergegeven in het volgende voorbeeld: 

```json
{
    "type": "ExecutePipeline",
    "typeProperties": {
      "pipeline": {
        "referenceName": "invokedPipeline",
        "type": "PipelineReference"
      },
      "parameters": {
        "sourceBlobContainer": {
          "value": "@pipeline().parameters.masterSourceBlobContainer",
          "type": "Expression"
        },
        "sinkBlobContainer": {
          "value": "@pipeline().parameters.masterSinkBlobContainer",
          "type": "Expression"
        }
      },

      ....
}

```
## <a name="next-steps"></a>Volgende stappen
Zie andere controlestroomactiviteiten die door Data Factory worden ondersteund: 

- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Get Metadata Activity](control-flow-get-metadata-activity.md)
- [Lookup Activity](control-flow-lookup-activity.md)
- [Webactiviteit](control-flow-web-activity.md)
