---
title: Ophalen van gebruiksgegevens voor virtuele Azure-Machine met behulp van de REST-API | Microsoft Docs
description: Gebruik de Azure REST API's voor het verzamelen van metrische gegevens over Resourcegebruik voor een virtuele Machine.
services: virtual-machines
author: rloutlaw
ms.reviewer: routlaw
manager: jeconnoc
ms.service: load-balancer
ms.custom: REST
ms.topic: article
ms.date: 06/13/2018
ms.author: routlaw
ms.openlocfilehash: 924154a64673b4ff646f3b6ece373b278ee37181
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55754843"
---
# <a name="get-virtual-machine-usage-metrics-using-the-rest-api"></a>Virtuele Machine gebruik metrische gegevens met behulp van de REST-API ophalen

In dit voorbeeld laat zien hoe om op te halen van het CPU-gebruik voor een [virtuele Linux-Machine](https://docs.microsoft.com/azure/virtual-machines/linux/monitor) met behulp van de [REST API van Azure](/rest/api/azure/).

Volledige documentatie en meer voorbeelden voor de REST-API zijn beschikbaar in de [naslaginformatie over de Azure Monitor REST](/rest/api/monitor). 

## <a name="build-the-request"></a>De aanvraag maken

Gebruik de volgende GET-aanvraag voor het verzamelen van de [Percentage CPU-metriek](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftcomputevirtualmachines) van een virtuele Machine

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmname}/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=Percentage%20CPU&timespan=2018-06-05T03:00:00Z/2018-06-07T03:00:00Z
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
| resourceGroupName | De naam van de Azure-resourcegroep die is gekoppeld aan de resource. U kunt deze waarde ophalen vanuit de Azure Resource Manager-API, CLI of de portal. |
| vmname | De naam van de Azure-Machine. |
| metricnames | Door komma's gescheiden lijst van geldige [metrische gegevens over Load Balancer](/azure/load-balancer/load-balancer-standard-diagnostics). |
| API-versie | De API-versie moet worden gebruikt voor de aanvraag.<br /><br /> In dit document bevat informatie over api-versie `2018-01-01`opgenomen in de bovenstaande URL.  |
| timespan | De tekenreeks met de volgende indeling `startDateTime_ISO/endDateTime_ISO` waarmee het tijdsbereik van de geretourneerde metrische gegevens worden gedefinieerd. Dit is een optionele parameter is ingesteld om te retourneren van een dag aan gegevens in het voorbeeld. |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>Aanvraagbody

Er is geen aanvraagtekst is nodig voor deze bewerking.

## <a name="handle-the-response"></a>Het antwoord verwerken

Statuscode 200 wordt geretourneerd wanneer de lijst met metrische waarden wordt geretourneerd. Een volledige lijst met foutcodes is beschikbaar in de [referentiedocumentatie](/rest/api/monitor/metrics/list#errorresponse).

## <a name="example-response"></a>Voorbeeld van een antwoord 

```json
{
    "cost": 0,
    "timespan": "2018-06-08T23:48:10Z/2018-06-09T00:48:10Z",
    "interval": "PT1M",
    "value": [
        {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmname}/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=Percentage%20CPU",
            "type": "Microsoft.Insights/metrics",
            "name": {
                "value": "Percentage CPU",
                "localizedValue": "Percentage CPU"
            },
            "unit": "Percent",
            "timeseries": [
                {
                    "metadatavalues": [],
                    "data": [
                        {
                            "timeStamp": "2018-06-08T23:48:00Z",
                            "average": 0.44
                        },
                        {
                            "timeStamp": "2018-06-08T23:49:00Z",
                            "average": 0.31
                        },
                        {
                            "timeStamp": "2018-06-08T23:50:00Z",
                            "average": 0.29
                        },
                        {
                            "timeStamp": "2018-06-08T23:51:00Z",
                            "average": 0.29
                        },
                        {
                            "timeStamp": "2018-06-08T23:52:00Z",
                            "average": 0.285
                        } ]
                } ]
        } ]
}
```
