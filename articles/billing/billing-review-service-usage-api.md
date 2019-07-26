---
title: Azure Service Resource Usage controleren met REST API | Microsoft Docs
description: Meer informatie over het gebruik van Azure REST Api's voor het controleren van het gebruik van Azure-service resources.
services: billing
documentationcenter: na
author: lleonard-msft
manager: ''
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2018
ms.author: banders
ms.openlocfilehash: 47e19fae26d6e3bc465799980c587d7bb7ed5e92
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443069"
---
# <a name="review-azure-resource-usage-using-the-rest-api"></a>Het gebruik van Azure-resources controleren met behulp van de REST API

Met Azure Cost Management Api's kunt u het verbruik van uw Azure-resources controleren en beheren.

In dit artikel leert u hoe u een dagelijks rapport maakt waarmee een door komma's gescheiden waarde wordt gegenereerd met uw uurverbruik en hoe filters kunnen worden gebruikt om het rapport aan te passen zodat u het gebruik van virtuele machines, data bases en gecodeerde gegevens kunt opvragen. resources in een Azure-resource groep.

>[!NOTE]
> De Cost Management-API bevindt zich momenteel in een persoonlijke preview.

## <a name="create-a-basic-cost-management-report"></a>Een eenvoudig kosten beheer rapport maken

Gebruik de `reports` bewerking in de Cost Management-API om te definiëren hoe de kosten rapportage wordt gegenereerd en waar de rapporten worden gepubliceerd.

```http
https://management.azure.com/subscriptions/{subscriptionGuid}/providers/Microsoft.CostManagement/reports/{reportName}?api-version=2018-09-01-preview
Content-Type: application/json   
Authorization: Bearer
```

De `{subscriptionGuid}` para meter is vereist en moet een abonnements-id bevatten die kan worden gelezen met de referenties die zijn opgegeven in het API-token. Dit`{reportName}`

De volgende headers zijn vereist: 

|Aanvraag header|Description|  
|--------------------|-----------------|  
|*Inhouds type:*| Vereist. Ingesteld op `application/json`. |  
|*Authorization:*| Vereist. Stel in op een `Bearer` geldig token. |

Configureer de para meters van het rapport in de hoofd tekst van de HTTP-aanvraag. In het onderstaande voor beeld is het rapport ingesteld op het genereren van elke dag wanneer actief, een CSV-bestand dat is geschreven naar een Azure Storage BLOB-container en bevat informatie over uurkosten voor alle resources `westus`in de resource groep.

```json
{
    "properties": {
        "schedule": {
            "status": "Inactive",
            "recurrence": "Daily",
            "recurrencePeriod": {
                "from": "2018-08-21",
                "to": "2019-10-31"
            }
        },
        "deliveryInfo": {
            "destination": {
                "resourceId": "/subscriptions/{subscriptionGuid}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}",
                "container": "MyReportContainer",
                "rootFolderPath": "MyScheduleTest"
            }
        },
        "format": "Csv",
        "definition": {
            "type": "Usage",
            "timeframe": "MonthToDate",
            "dataSet": {
                "granularity": "Hourly",
                "filter": {
                    "dimensions": {
                        "name": "ResourceLocation",
                        "operator": "In",
                        "values": [
                            "westus"
                        ]
                    }
                }
            }
        }
    }
}
```

Kan

## <a name="filtering-reports"></a>Rapporten filteren

In `filter` het `dimensions` gedeelte en van de hoofd tekst van de aanvraag bij het maken van een rapport kunt u zich richten op de kosten voor specifieke resource typen. In de vorige aanvraag tekst ziet u hoe u kunt filteren op alle resources in een regio. 

### <a name="get-all-compute-usage"></a>Alle reken gebruik ophalen

Gebruik de `ResourceType` dimensie voor het rapporteren van de kosten van virtuele Azure-machines in uw abonnement op alle regio's.

```json
"filter": {
    "dimensions": {
        "name": "ResourceType",
        "operator": "In",
        "values": [
                "Microsoft.ClassicCompute/virtualMachines", 
                "Microsoft.Compute/virtualMachines"
        ] 
    }
}
```

### <a name="get-all-database-usage"></a>Alle database gebruik ophalen

Gebruik de `ResourceType` dimensie om Azure SQL database kosten in uw abonnement te rapporteren over alle regio's.

```json
"filter": {
    "dimensions": {
        "name": "ResourceType",
        "operator": "In",
        "values": [
                "Microsoft.Sql/servers"
        ] 
    }
}
```

### <a name="report-on-specific-instances"></a>Rapport over specifieke instanties

Met `Resource` de dimensie kunt u kosten voor specifieke resources rapporteren.

```json
"filter": {
    "dimensions": {
        "name": "Resource",
        "operator": "In",
        "values": [
            "subscriptions/{subscriptionGuid}/resourceGroups/{resourceGroup}/providers/Microsoft.ClassicCompute/virtualMachines/{ResourceName}"
        ]
    }
}
```

### <a name="changing-timeframes"></a>Tijds perioden wijzigen

Stel de `timeframe` definitie in `Custom` op om een tijds bestek in te stellen buiten de week tot de ingebouwde opties datum en maand.

```json
"timeframe": "Custom",
"timePeriod": {
    "from": "2017-12-31T00:00:00.000Z",
    "to": "2018-12-30T00:00:00.000Z"
}
```

## <a name="next-steps"></a>Volgende stappen
- [Aan de slag met Azure REST API](https://docs.microsoft.com/rest/api/azure/)   
