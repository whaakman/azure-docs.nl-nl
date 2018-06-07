---
title: Het lezen of schrijven gepartitioneerd gegevens in Azure Data Factory | Microsoft Docs
description: Informatie over het lezen of schrijven gepartitioneerde in Azure Data Factory versie 2.
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
ms.date: 05/15/2018
ms.author: shlo
ms.openlocfilehash: cdf305e3607d7483186185a014883cff5458b89f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34619079"
---
# <a name="how-to-read-or-write-partitioned-data-in-azure-data-factory-version-2"></a>Het lezen of schrijven gepartitioneerd gegevens in Azure Data Factory versie 2
Azure Data Factory ondersteund in versie 1, lezen of schrijven van gepartitioneerde gegevens met behulp van de SliceStart/SliceEnd/WindowStart/WindowEnd systeemvariabelen. In versie 2, kunt u dit gedrag kunt bereiken met behulp van een pipeline-parameter en van de trigger geplande tijd/begintijd als een waarde van de parameter. 

## <a name="use-a-pipeline-parameter"></a>Gebruik de parameter van een pijplijn 
In versie 1, kunt u de eigenschap partitionedBy en SliceStart systeemvariabele, zoals wordt weergegeven in het volgende voorbeeld gebruiken: 

```json
"folderPath": "adfcustomerprofilingsample/logs/marketingcampaigneffectiveness/{Year}/{Month}/{Day}/",
"partitionedBy": [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "%M" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "%d" } }
],
```

Zie voor meer informatie over de eigenschap partitonedBy [versie 1 Azure Blob-connector](v1/data-factory-azure-blob-connector.md#dataset-properties) artikel. 

In versie 2 is een manier om dit gedrag de volgende acties uitvoeren: 

1. Definieer een **pipeline-parameter** van het type tekenreeks. In het volgende voorbeeld is de naam van de pipeline-parameter **windowStartTime**. 
2. Stel **folderPath** in de definitie van de gegevensset om te verwijzen naar de waarde van parameter van de pijplijn. 
3. De werkelijke waarde voor de parameter doorgeven bij het aanroepen van de pijplijn op aanvraag, of geeft u een trigger geplande tijd/begintijd dynamisch tijdens runtime. 

```json
"folderPath": {
      "value": "adfcustomerprofilingsample/logs/marketingcampaigneffectiveness/@{formatDateTime(pipeline().parameters.windowStartTime, 'yyyy/MM/dd')}/",
      "type": "Expression"
},
```

## <a name="pass-in-value-from-a-trigger"></a>In waarde van een trigger doorgeven
In de volgende daling trigger vensterdefinitie, begintijd van de trigger is doorgegeven als een waarde voor de pipeline-parameter **windowStartTime**: 

```json
{
    "name": "MyTrigger",
    "properties": {
        "type": "TumblingWindowTrigger",
        "typeProperties": {
            "frequency": "Hour",
            "interval": "1",
            "startTime": "2018-05-15T00:00:00Z",
            "delay": "00:10:00",
            "maxConcurrency": 10
        },
        "pipeline": {
            "pipelineReference": {
                "type": "PipelineReference",
                "referenceName": "MyPipeline"
            },
            "parameters": {
                "windowStartTime": "@trigger().outputs.windowStartTime"
            }
        }
    }
}
```

## <a name="example"></a>Voorbeeld

Hier volgt een voorbeeld gegevenssetdefinitie:

```json
{
  "name": "SampleBlobDataset",
  "type": "AzureBlob",
  "typeProperties": {
    "folderPath": {
      "value": "adfcustomerprofilingsample/logs/marketingcampaigneffectiveness/@{formatDateTime(pipeline().parameters.windowStartTime, 'yyyy/MM/dd')}/",
      "type": "Expression"
    },
    "format": {
      "type": "TextFormat",
      "columnDelimiter": ","
    }
  },
  "structure": [
    { "name": "ProfileID", "type": "String" },
    { "name": "SessionStart", "type": "String" },
    { "name": "Duration", "type": "Int32" },
    { "name": "State", "type": "String" },
    { "name": "SrcIPAddress", "type": "String" },
    { "name": "GameType", "type": "String" },
    { "name": "Multiplayer", "type": "String" },
    { "name": "EndRank", "type": "String" },
    { "name": "WeaponsUsed", "type": "Int32" },
    { "name": "UsersInteractedWith", "type": "String" },
    { "name": "Impressions", "type": "String" }
  ],
  "linkedServiceName": {
    "referenceName": "churnStorageLinkedService",
    "type": "LinkedServiceReference"
  }
}
```

De definitie van de pijplijn: 

```json
{
    "properties": {
        "activities": [{
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": {
                    "value": "@concat(pipeline().parameters.blobContainer, '/scripts/', pipeline().parameters.partitionHiveScriptFile)",
                    "type": "Expression"
                },
                "scriptLinkedService": {
                    "referenceName": "churnStorageLinkedService",
                    "type": "LinkedServiceReference"
                },
                "defines": {
                    "RAWINPUT": {
                        "value": "@concat('wasb://', pipeline().parameters.blobContainer, '@', pipeline().parameters.blobStorageAccount, '.blob.core.windows.net/logs/', pipeline().parameters.inputRawLogsFolder, '/')",
                        "type": "Expression"
                    },
                    "Year": {
                        "value": "@formatDateTime(pipeline().parameters.windowStartTime, 'yyyy')",
                        "type": "Expression"
                    },
                    "Month": {
                        "value": "@formatDateTime(pipeline().parameters.windowStartTime, 'MM')",
                        "type": "Expression"
                    },
                    "Day": {
                        "value": "@formatDateTime(pipeline().parameters.windowStartTime, 'dd')",
                        "type": "Expression"
                    }
                }
            },
            "linkedServiceName": {
                "referenceName": "HdiLinkedService",
                "type": "LinkedServiceReference"
            },
            "name": "HivePartitionGameLogs"
        }],
        "parameters": {
            "windowStartTime": {
                "type": "String"
            },
            "blobStorageAccount": {
                "type": "String"
            },
            "blobContainer": {
                "type": "String"
            },
            "inputRawLogsFolder": {
                "type": "String"
            }
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen
Zie voor een volledig overzicht van het maken van een gegevensfactory met een pipeline [Snelstartgids: Maak een gegevensfactory](quickstart-create-data-factory-powershell.md). 
