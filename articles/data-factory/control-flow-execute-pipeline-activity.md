---
title: Activiteit van de pijplijn niet uitvoeren in Azure Data Factory | Microsoft Docs
description: Meer informatie over hoe u de pijplijn uitvoeren activiteit kunt gebruiken om aan te roepen een Data Factory-pijplijn van een andere Data Factory-pijplijn.
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
ms.openlocfilehash: 2aa25004fb9c2e914cd8c669095953e174686197
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37051760"
---
# <a name="execute-pipeline-activity-in-azure-data-factory"></a>Activiteit van de pijplijn niet uitvoeren in Azure Data Factory
De pijplijn niet uitvoeren-activiteit kunt een Data Factory-pijplijn om aan te roepen, een andere pijplijn.

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
naam | De naam van de activiteit van de pijplijn uitvoeren. | Reeks | Ja
type | Moet worden ingesteld op: **ExecutePipeline**. | Reeks | Ja
pijplijn | Pipeline-verwijzing naar de afhankelijke pijplijn die deze pipeline wordt aangeroepen. Een pijplijn reference-object heeft twee eigenschappen: **naamverwijzing** en **type**. De eigenschap naamverwijzing geeft de naam van de referentie-pijplijn. De eigenschap type moet worden ingesteld op PipelineReference. | PipelineReference | Ja
parameters | Parameters moeten worden doorgegeven aan de pijplijn aangeroepen | Een JSON-object dat is toegewezen parameternamen aan argumentwaarden | Nee
waitOnCompletion | Bepaalt of de uitvoering van de activiteit wordt gewacht op de uitvoering van de afhankelijke pipeline te voltooien. | De standaardinstelling is onwaar. | Boole-waarde | Nee

## <a name="sample"></a>Voorbeeld
Dit scenario heeft twee pijplijnen:

- **Master pijplijn** -deze pipeline heeft één pijplijn niet uitvoeren-activiteit die de aangeroepen pijplijn aanroept. De pijplijn master heeft twee parameters: `masterSourceBlobContainer`, `masterSinkBlobContainer`.
- **Aangeroepen pijplijn** -deze pijplijn heeft een kopieeractiviteit waarmee gegevens worden gekopieerd van de bron van een Azure-Blob naar Azure Blob-sink. De aangeroepen pijplijn heeft twee parameters: `sourceBlobContainer`, `sinkBlobContainer`.

### <a name="master-pipeline-definition"></a>De definitie van de master-pipeline

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
            "sinkBlobCountainer": {
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

### <a name="invoked-pipeline-definition"></a>Aangeroepen pipeline-definitie

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

**Bron-gegevensset**
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

### <a name="running-the-pipeline"></a>De pijplijn wordt uitgevoerd

Als u wilt de master pijplijn in dit voorbeeld uitvoert, worden de volgende waarden voor de parameters masterSourceBlobContainer en masterSinkBlobContainer doorgegeven: 

```json
{
  "masterSourceBlobContainer": "executetest",
  "masterSinkBlobContainer": "executesink"
}
```

De master pijplijn stuurt deze waarden voor de pijplijn aangeroepen, zoals wordt weergegeven in het volgende voorbeeld: 

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
        "sinkBlobCountainer": {
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
