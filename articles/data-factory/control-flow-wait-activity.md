---
title: Wachtactiviteit in Azure Data Factory | Microsoft Docs
description: De activiteit wachten onderbreekt de uitvoering van de pijplijn voor de opgegeven periode.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: 731df55a11f4671670a65dac8a83927d81da454c
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54015794"
---
# <a name="wait-activity-in-azure-data-factory"></a>Wachtactiviteit in Azure Data Factory
Als u een Wait Activity in een pijplijn gebruikt, wacht de pijplijn tot de opgegeven periode voorbij is voordat de volgende activiteiten worden uitgevoerd. 

## <a name="syntax"></a>Syntaxis

```json
{
    "name": "MyWaitActivity",
    "type": "Wait",
    "typeProperties": {
        "waitTimeInSeconds": 1
    }
}

```

## <a name="type-properties"></a>Type-eigenschappen

Eigenschap | Description | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
naam | Naam van de `Wait` activiteit. | Reeks | Ja
type | Moet worden ingesteld op **wacht**. | Reeks | Ja
waitTimeInSeconds | Het aantal seconden dat de pijplijn moet wachten voordat u doorgaat met de verwerking. | Geheel getal | Ja

## <a name="example"></a>Voorbeeld

> [!NOTE]
> Deze sectie bevat JSON-definities en voorbeeld van PowerShell-opdrachten om uit te voeren van de pijplijn. Zie voor een overzicht met stapsgewijze instructies voor het maken van een Data Factory-pijplijn met behulp van Azure PowerShell en JSON-definities [zelfstudie: een data factory maken met behulp van Azure PowerShell](quickstart-create-data-factory-powershell.md).

### <a name="pipeline-with-wait-activity"></a>Pijplijn met wachtactiviteit
In dit voorbeeld heeft de pijplijn twee activiteiten: **Pas** en **wacht**. De activiteit wachten is geconfigureerd om te wachten tot één seconde. De pijplijn wordt de Web-activiteit in een lus uitgevoerd met één seconde wachttijd tussen elke uitvoering. 

```json
{
    "name": "DoUntilPipeline",
    "properties": {
        "activities": [
            {
                "type": "Until",
                "typeProperties": {
                    "expression": {
                        "value": "@equals('Failed', coalesce(body('MyUnauthenticatedActivity')?.status, actions('MyUnauthenticatedActivity')?.status, 'null'))",
                        "type": "Expression"
                    },
                    "timeout": "00:00:01",
                    "activities": [
                        {
                            "name": "MyUnauthenticatedActivity",
                            "type": "WebActivity",
                            "typeProperties": {
                                "method": "get",
                                "url": "https://www.fake.com/",
                                "headers": {
                                    "Content-Type": "application/json"
                                }
                            },
                            "dependsOn": [
                                {
                                    "activity": "MyWaitActivity",
                                    "dependencyConditions": [ "Succeeded" ]
                                }
                            ]
                        },
                        {
                            "type": "Wait",
                            "typeProperties": {
                                "waitTimeInSeconds": 1
                            },
                            "name": "MyWaitActivity"
                        }
                    ]
                },
                "name": "MyUntilActivity"
            }
        ]
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
