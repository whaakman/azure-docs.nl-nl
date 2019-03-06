---
title: Metrische gegevens met de REST-API ophalen
titlesuffix: Azure Load Balancer
description: Gebruik de Azure REST API's voor het verzamelen van metrische gegevens over status en gebruik voor Load Balancer voor een bepaald bereik van de tijd en datums.
services: sql-database
author: KumudD
ms.reviewer: routlaw
manager: jeconnoc
ms.service: load-balancer
ms.custom: REST, seodec18
ms.topic: article
ms.date: 06/06/2017
ms.author: KumudD
ms.openlocfilehash: 0d8ac22679bc4e789c22396c21c51dacee201302
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57433838"
---
# <a name="get-load-balancer-utilization-metrics-using-the-rest-api"></a>Load Balancer-gebruik metrische gegevens met behulp van de REST-API ophalen

Hoe u deze instructies voor het verzamelen van het aantal bytes dat is verwerkt door een [Standard Load Balancer](/azure/load-balancer/load-balancer-standard-overview) voor een interval van het gebruik van de tijd de [REST API van Azure](/rest/api/azure/).

Volledige documentatie en meer voorbeelden voor de REST-API zijn beschikbaar in de [naslaginformatie over de Azure Monitor REST](/rest/api/monitor). 

## <a name="build-the-request"></a>De aanvraag maken

Gebruik de volgende GET-aanvraag voor het verzamelen van de [ByteCount metriek](/azure/load-balancer/load-balancer-standard-diagnostics#a-name--multidimensionalmetricsamulti-dimensional-metrics) van een Standard Load Balancer. 

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=ByteCount&timespan=2018-06-05T03:00:00Z/2018-06-07T03:00:00Z
```

### <a name="request-headers"></a>Aanvraagheaders

De volgende headers zijn vereist: 

|Aanvraagheader|Description|  
|--------------------|-----------------|  
|*Content-Type:*|Vereist. Ingesteld op `application/json`.|  
|*Autorisatie:*|Vereist. Ingesteld op een geldige `Bearer` [toegangstoken](/rest/api/azure/#authorization-code-grant-interactive-clients). |  

### <a name="uri-parameters"></a>URI-parameters

| Name | Description |
| :--- | :---------- |
| subscriptionId | De abonnements-ID waarmee een Azure-abonnement. Als u meerdere abonnementen hebt, raadpleegt u [werken met meerdere abonnementen](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest). |
| resourceGroupName | De naam van de resourcegroep waarin de resource. U kunt deze waarde niet ophalen van de Azure Resource Manager-API, CLI of de portal. |
| loadBalancerName | De naam van de Azure Load Balancer. |
| metricnames | Door komma's gescheiden lijst van geldige [metrische gegevens over Load Balancer](/azure/load-balancer/load-balancer-standard-diagnostics). |
| API-versie | De API-versie moet worden gebruikt voor de aanvraag.<br /><br /> In dit document bevat informatie over api-versie `2018-01-01`opgenomen in de bovenstaande URL.  |
| timespan | De duur van de query. Het is een tekenreeks met de volgende indeling `startDateTime_ISO/endDateTime_ISO`. Dit is een optionele parameter is ingesteld om te retourneren van een dag aan gegevens in het voorbeeld. |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>Aanvraagbody

Er is geen aanvraagtekst is nodig voor deze bewerking.

## <a name="handle-the-response"></a>Het antwoord verwerken

Statuscode 200 wordt geretourneerd wanneer de lijst met metrische waarden wordt geretourneerd. Een volledige lijst met foutcodes is beschikbaar in de [referentiedocumentatie](/rest/api/monitor/metrics/list#errorresponse).

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
