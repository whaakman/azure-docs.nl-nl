---
title: Bekijk het gebruik van Azure-service-resource met REST-API | Microsoft Docs
description: Leer hoe u Azure REST API's gebruiken om te controleren van Resourcegebruik van de Azure-service.
services: billing
documentationcenter: na
author: lleonard-msft
manager: MBaldwin
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2018
ms.author: erikre
ms.openlocfilehash: 297b1d5e683a8ad505a37c43ab74c8b888ec87f2
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54265750"
---
# <a name="review-azure-resource-usage-using-the-rest-api"></a>Bekijk het gebruik van Azure-resource met behulp van de REST-API

Azure Cost Management-API's helpt u bij gebruik van uw Azure-resources beheren en controleren.

In dit artikel leert u hoe u een dagelijkse rapport maken dat een document met door komma's gescheiden waarden met uw per uur informatie over het gebruik en hoe u filters worden gebruikt om het rapport aanpassen, zodat u kunt het gebruik van virtuele machines, databases, een query wordt gegenereerd en getagd resources in een Azure-resourcegroep.

>[!NOTE]
> De kosten-API is momenteel in private preview.

## <a name="create-a-basic-cost-management-report"></a>Een eenvoudige kosten management-rapport maken

Gebruik de `reports` bewerking in de kosten-API om te definiëren hoe de kosten voor rapportage wordt gegenereerd en waar de rapporten naar worden gepubliceerd.

```http
https://management.azure.com/subscriptions/{subscriptionGuid}/providers/Microsoft.CostManagement/reports/{reportName}?api-version=2018-09-01-preview
Content-Type: application/json   
Authorization: Bearer
```

De `{subscriptionGuid}` -parameter is vereist en moet een abonnements-ID die kan worden gelezen met de referenties die zijn opgegeven in de API-token bevatten. de `{reportName}`

De volgende headers zijn vereist: 

|Aanvraagheader|Description|  
|--------------------|-----------------|  
|*Content-Type:*| Vereist. Ingesteld op `application/json`. |  
|*Autorisatie:*| Vereist. Ingesteld op een geldige `Bearer` token. |

Configureer de parameters van het rapport in de hoofdtekst van de HTTP-aanvraag. In het volgende voorbeeld wordt het rapport is ingesteld voor het genereren van elke dag als actief, een CSV-bestand dat is geschreven naar een Azure Storage blob-container en per uur gegevens over kosten voor alle resources in de resourcegroep bevat `westus`.

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

de

## <a name="filtering-reports"></a>Filteren van rapporten

De `filter` en `dimensions` sectie van de hoofdtekst van de aanvraag bij het maken van een rapport kunt u focus in op de kosten voor specifieke brontypen. De hoofdtekst van de vorige aanvraag laat zien hoe om te filteren op alle resources in een regio. 

### <a name="get-all-compute-usage"></a>Ophalen van alle compute-gebruik

Gebruik de `ResourceType` dimensie voor het rapporteren van de virtuele machine van Azure-kosten in uw abonnement in alle regio's.

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

### <a name="get-all-database-usage"></a>Alle Databasegebruik ophalen

Gebruik de `ResourceType` dimensie rapport van Azure SQL Database kosten in uw abonnement in alle regio's.

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

### <a name="report-on-specific-instances"></a>Een rapport over specifieke instanties

De `Resource` dimensie kunt u kosten voor specifieke resources rapporteren.

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

### <a name="changing-timeframes"></a>Tijdsbestek wijzigen

Stel de `timeframe` definitie `Custom` in te stellen van een bepaald tijdsbestek buiten de week op datum en maand tot datum ingebouwde opties.

```json
"timeframe": "Custom",
"timePeriod": {
    "from": "2017-12-31T00:00:00.000Z",
    "to": "2018-12-30T00:00:00.000Z"
}
```

## <a name="next-steps"></a>Volgende stappen
- [Aan de slag met REST API van Azure](https://docs.microsoft.com/rest/api/azure/)   
