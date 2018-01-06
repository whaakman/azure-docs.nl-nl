---
title: Azure gebeurtenisabonnement raster met sjabloon
description: Een abonnement van de rasterlijnen gebeurtenis met Resource Manager-sjabloon maken.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/05/2018
ms.author: tomfitz
ms.openlocfilehash: 2b9f55f8e944d688b622e30a773e1a34698f22ec
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="use-resource-manager-template-for-event-grid-subscription"></a>Gebruik Resource Manager-sjabloon voor gebeurtenis raster abonnement

In dit artikel laat zien hoe een Azure Resource Manager-sjabloon gebruiken om gebeurtenis raster abonnementen te maken. De indeling die u gebruikt, verschilt op basis van of u zich op resource group-gebeurtenissen of gebeurtenissen voor een bepaald resourcetype abonneert. Beide indelingen worden weergegeven in dit artikel.

## <a name="subscribe-to-resource-group-events"></a>Abonneren op resource group-gebeurtenissen

Als u zich abonneert op resource group gebeurtenissen, gebruik `Microsoft.EventGrid/eventSubscriptions` voor het brontype. Voor de gebeurtenis wijst type, een `WebHook` of `EventHub`.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "type": "Microsoft.EventGrid/eventSubscriptions",
            "name": "mySubscription",
            "apiVersion": "2017-09-15-preview",
            "properties": {
                "destination": {
                    "endpointType": "WebHook",
                    "properties": {
                        "endpointUrl": "https://requestb.in/ynboxiyn"
                    }
                },
                "filter": {
                    "subjectBeginsWith": "",
                    "subjectEndsWith": "",
                    "isSubjectCaseSensitive": false,
                    "includedEventTypes": ["All"]
                }
            }
        }
    ]
}
```

Wanneer u deze sjabloon aan een resourcegroep implementeert, wordt u zich abonneert op gebeurtenissen voor die resourcegroep.

## <a name="subscribe-to-resource-events"></a>Abonneren op de resource-gebeurtenissen

Als u zich abonneert op resource gebeurtenissen, kunt u het abonnement op de juiste resource koppelen door het brontype en de naam in de definitie van het abonnement. Gebruik voor het brontype `<provider-namespace>/<resource-type>/providers/eventSubscriptions`. Gebruik voor de naam van de `<resource-name>/Microsoft.EventGrid/<subscription-name>`. Voor de gebeurtenis wijst type, een `WebHook` of `EventHub`.

Het volgende voorbeeld laat zien hoe om u te abonneren op gebeurtenissen voor Blob-opslag.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageName": {
            "type": "string"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts/providers/eventSubscriptions",
            "name": "[concat(parameters('storageName'), '/Microsoft.EventGrid/myStorageSubscription')]",
            "apiVersion": "2017-09-15-preview",
            "properties": {
                "destination": {
                    "endpointType": "WebHook",
                    "properties": {
                        "endpointUrl": "https://requestb.in/ynboxiyn"
                    }
                },
                "filter": {
                    "subjectBeginsWith": "",
                    "subjectEndsWith": "",
                    "isSubjectCaseSensitive": false
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot gebeurtenis raster, [over gebeurtenis raster](overview.md).
* Zie voor een inleiding in Resource Manager [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
* Als u wilt beginnen met de gebeurtenis raster, Zie [maken en route aangepaste gebeurtenissen met Azure Event raster](custom-event-quickstart.md).