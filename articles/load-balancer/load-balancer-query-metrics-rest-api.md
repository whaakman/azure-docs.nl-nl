---
title: Metrische gegevens ophalen met de REST API
titlesuffix: Azure Load Balancer
description: Gebruik de Azure REST Api's voor het verzamelen van metrische gegevens over de status en het gebruik van Load Balancer voor een bepaald tijd bereik en datums.
services: sql-database
author: asudbring
ms.reviewer: routlaw
manager: jeconnoc
ms.service: load-balancer
ms.custom: REST, seodec18
ms.topic: article
ms.date: 06/06/2017
ms.author: allensu
ms.openlocfilehash: 0d12dc04aff58dd6273d8d29d422bdbd9e7c886b
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68274532"
---
# <a name="get-load-balancer-utilization-metrics-using-the-rest-api"></a>Load Balancer metrische gegevens over het gebruik ophalen met behulp van de REST API

In deze procedure ziet u hoe u het aantal bytes dat wordt verwerkt door een [Standard Load Balancer](/azure/load-balancer/load-balancer-standard-overview) , kunt verzamelen gedurende een tijds interval met behulp van de [Azure rest API](/rest/api/azure/).

Volledige referentie documentatie en aanvullende voor beelden voor de REST API zijn beschikbaar in de [Azure monitor rest-referentie](/rest/api/monitor). 

## <a name="build-the-request"></a>De aanvraag voor het samenstellen

Gebruik de volgende GET-aanvraag om de [ByteCount-metriek](/azure/load-balancer/load-balancer-standard-diagnostics#multi-dimensional-metrics) van een Standard Load Balancer te verzamelen. 

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=ByteCount&timespan=2018-06-05T03:00:00Z/2018-06-07T03:00:00Z
```

### <a name="request-headers"></a>Aanvraagheaders

De volgende headers zijn vereist: 

|Aanvraag header|Description|  
|--------------------|-----------------|  
|*Inhouds type:*|Vereist. Ingesteld op `application/json`.|  
|*Authorization:*|Vereist. Ingesteld op een geldig `Bearer` [toegangs token](/rest/api/azure/#authorization-code-grant-interactive-clients). |  

### <a name="uri-parameters"></a>URI-parameters

| Name | Description |
| :--- | :---------- |
| subscriptionId | De abonnements-ID waarmee een Azure-abonnement wordt geïdentificeerd. Als u meerdere abonnementen hebt, raadpleegt u [werken met meerdere abonnementen](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest). |
| resourceGroupName | De naam van de resource groep die de resource bevat. U kunt deze waarde verkrijgen via de Azure Resource Manager-API, CLI of de portal. |
| loadBalancerName | De naam van de Azure Load Balancer. |
| metricnames | Een door komma's gescheiden lijst met geldige [Load Balancer metrische gegevens](/azure/load-balancer/load-balancer-standard-diagnostics). |
| api-version | De API-versie die voor de aanvraag moet worden gebruikt.<br /><br /> In dit document wordt de API `2018-01-01`-versie beschreven die is opgenomen in de bovenstaande URL.  |
| duur | De time span van de query. Het is een teken reeks met de volgende `startDateTime_ISO/endDateTime_ISO`indeling. Deze optionele para meter is zo ingesteld dat de gegevens van een dag in het voor beeld worden geretourneerd. |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>Aanvraagbody

Er is geen aanvraag tekst nodig voor deze bewerking.

## <a name="handle-the-response"></a>Het antwoord verwerken

De status code 200 wordt geretourneerd wanneer de lijst met metrische waarden is geretourneerd. In de [referentie documentatie](/rest/api/monitor/metrics/list#errorresponse)vindt u een volledige lijst met fout codes.

## <a name="example-response"></a>Voorbeeld van een antwoord 

```json
{
    "cost": 0,
    "timespan": "2018-06-05T03:00:00Z/2018-06-07T03:00:00Z",
    "interval": "PT1M",
    "value": [
        {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}/providers/Microsoft.Insights/metrics/ByteCount",
            "type": "Microsoft.Insights/metrics",
            "name": {
                "value": "ByteCount",
                "localizedValue": "Byte Count"
            },
            "unit": "Count",
            "timeseries": [
                {
                    "metadatavalues": [],
                    "data": [
                        {
                            "timeStamp": "2018-06-06T17:24:00Z",
                            "total": 1067921034.0
                        },
                        {
                            "timeStamp": "2018-06-06T17:25:00Z",
                            "total": 0.0
                        },
                        {
                            "timeStamp": "2018-06-06T17:26:00Z",
                            "total": 3781344.0
                        },
                    ]
                }
            ]
        }
    ],
    "namespace": "Microsoft.Network/loadBalancers",
    "resourceregion": "eastus"
}
```
