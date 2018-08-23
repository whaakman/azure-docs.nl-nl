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
ms.author: alleonar
ms.openlocfilehash: 2af87c87916dd272026a3bd7e1438507c655053b
ms.sourcegitcommit: a62cbb539c056fe9fcd5108d0b63487bd149d5c3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/22/2018
ms.locfileid: "42616990"
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

De `{subscriptionGuid}` -parameter is vereist en moet een abonnements-ID die kan worden gelezen met behulp van de provieed referenties in de API-token bevatten. de `{reportName}`

De volgende headers zijn vereist: 

|Aanvraagheader|Beschrijving|  
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

Stel de `timeframe` de definitie `Custom` in te stellen van een bepaald tijdsbestek buiten de week op datum en maand tot heden van ingebouwde opties.

```json
"timeframe": "Custom",
"timePeriod": {
    "from": "2017-12-31T00:00:00.000Z",
    "to": "2018-12-30T00:00:00.000Z"
}
```

## <a name="next-steps"></a>Volgende stappen
- [Aan de slag met REST API van Azure](https://docs.microsoft.com/rest/api/azure/)   
