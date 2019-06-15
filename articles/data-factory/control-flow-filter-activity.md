---
title: Filteren van activiteit in Azure Data Factory | Microsoft Docs
description: De activiteit Filter filtert de invoer.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/04/2018
ms.author: shlo
ms.openlocfilehash: 787c9393e2700bd7ed349b501e70abc4a0687b9c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60554844"
---
# <a name="filter-activity-in-azure-data-factory"></a>Filteren van activiteit in Azure Data Factory
U kunt een filteractiviteit in een pijplijn gebruiken een filterexpressie toepassen op een invoermatrix. 

## <a name="syntax"></a>Syntaxis

```json
{
    "name": "MyFilterActivity",
    "type": "filter",
    "typeProperties": {
        "condition": "<condition>",
        "items": "<input array>"
    }
}
```

## <a name="type-properties"></a>Type-eigenschappen

Eigenschap | Description | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
name | Naam van de `Filter` activiteit. | String | Ja
type | Moet worden ingesteld op **filter**. | String | Ja
condition | Voorwaarde moet worden gebruikt voor het filteren van de invoer. | expressie | Ja
items | De invoermatrix waarop filter moet worden toegepast. | expressie | Ja

## <a name="example"></a>Voorbeeld

In dit voorbeeld heeft de pijplijn twee activiteiten: **Filter** en **ForEach**. De Filter-activiteit is geconfigureerd voor het filteren van de invoermatrix voor artikelen met een waarde die groter is dan 3. De ForEach-activiteit doorloopt over de gefilterde waarden vervolgens en wacht tot het aantal seconden dat is opgegeven door de huidige waarde.

```json
{
    "name": "PipelineName",
    "properties": {
        "activities": [{
                "name": "MyFilterActivity",
                "type": "filter",
                "typeProperties": {
                    "condition": "@greater(item(),3)",
                    "items": "@pipeline().parameters.inputs"
                }
            },
            {
                "name": "MyForEach",
                "type": "ForEach",
                "typeProperties": {
                    "isSequential": "false",
                    "batchCount": 1,
                    "items": "@activity('MyFilterActivity').output.value",
                    "activities": [{
                        "type": "Wait",
                        "typeProperties": {
                            "waitTimeInSeconds": "@item()"
                        },
                        "name": "MyWaitActivity"
                    }]
                },
                "dependsOn": [{
                    "activity": "MyFilterActivity",
                    "dependencyConditions": ["Succeeded"]
                }]
            }
        ],
        "parameters": {
            "inputs": {
                "type": "Array",
                "defaultValue": [1, 2, 3, 4, 5, 6]
            }
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen
Zie andere controlestroomactiviteiten die door Data Factory worden ondersteund: 

- [If Condition Activity](control-flow-if-condition-activity.md)
- [Execute Pipeline Activity](control-flow-execute-pipeline-activity.md)
- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Get Metadata Activity](control-flow-get-metadata-activity.md)
- [Lookup Activity](control-flow-lookup-activity.md)
- [Webactiviteit](control-flow-web-activity.md)
- [Until Activity](control-flow-until-activity.md)
