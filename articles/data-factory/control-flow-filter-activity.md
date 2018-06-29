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
ms.devlang: na
ms.topic: conceptual
ms.date: 05/04/2018
ms.author: shlo
ms.openlocfilehash: b3b26869a84b8519ced19a4c93a6d39d6ed20f9b
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37050349"
---
# <a name="filter-activity-in-azure-data-factory"></a>Filteren van activiteit in Azure Data Factory
U kunt een activiteit Filter gebruiken in een pijplijn een filterexpressie toepassen op een invoermatrix. 

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

Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
naam | Naam van de `Filter` activiteit. | Reeks | Ja
type | Moet worden ingesteld op **filter**. | Reeks | Ja
voorwaarde | De voorwaarde moet worden gebruikt voor het filteren van de invoer. | Expressie | Ja
items | De invoermatrix waarop filter moet worden toegepast. | Expressie | Ja

## <a name="example"></a>Voorbeeld

In dit voorbeeld wordt de pijplijn heeft twee activiteiten: **Filter** en **ForEach**. De activiteit van het Filter is geconfigureerd voor het filteren van de invoermatrix voor items met een waarde groter is dan 3. De ForEach-activiteit vervolgens doorloopt over de gefilterde waarden en wordt gewacht op het aantal seconden dat is opgegeven door de huidige waarde.

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
