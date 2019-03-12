---
title: Azure Media Services diagnostische logboeken schema's - Azure
description: Dit artikel laat de Azure Media Services diagnostische logboeken schema's.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/20/2019
ms.author: juliako
ms.openlocfilehash: 225bc4368d2a87fa5fef3f74624fd10f42dc7c97
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554994"
---
# <a name="diagnostic-logs-schemas"></a>Schema's voor diagnostische logboeken

[Azure Monitor](../../azure-monitor/overview.md) kunt u metrische gegevens controleren en tot diagnostische logboeken waarmee u inzicht in hoe uw toepassingen worden uitgevoerd. U kunt diagnostische logboeken van Media Services controleren en maken van waarschuwingen en meldingen voor de verzamelde metrische gegevens en Logboeken. U kunt Logboeken verzenden [Azure Storage](https://azure.microsoft.com/services/storage/), ze te streamen [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), en ze te exporteren [Log Analytics](https://azure.microsoft.com/services/log-analytics/), of 3rd party services gebruiken.

Zie voor gedetailleerde informatie [Azure Monitor Metrics](../../azure-monitor/platform/data-collection.md) en [Azure Monitor diagnostische logboeken](../../azure-monitor/platform/diagnostic-logs-overview.md).

Dit artikel wordt beschreven Media Services-schema's voor diagnostische logboeken.

## <a name="top-level-diagnostic-logs-schema"></a>Op het hoogste niveau diagnostische logboeken schema

Zie voor gedetailleerde beschrijving van het schema op het hoogste niveau diagnostische logboeken [ondersteunde services, schema's en categorieën voor diagnostische logboeken van Azure](../../azure-monitor/platform/tutorial-dashboards.md).

## <a name="key-delivery-log-schema"></a>Schema voor sleutellevering-logboek

### <a name="properties"></a>Properties

Deze eigenschappen zijn specifiek voor het logboek sleutellevering schema.

|Name|Description|
|---|---|
|keyId|De ID van de aangevraagde sleutel.|
|keyType|Een van de volgende waarden kan zijn: "Wissen" (geen versleuteling), "FairPlay", 'PlayReady' of 'Widevine'.|
|policyName|De Azure Resource Manager-naam van het beleid.|
|tokenType|Het type token.|
|statusMessage|Het statusbericht.|

### <a name="examples"></a>Voorbeelden

De eigenschappen van het schema van de aanvragen sleutellevering.

```json
{
    "time": "2019-01-11T17:59:10.4908614Z",
    "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000/RESOURCEGROUPS/SBKEY/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/SBDNSTEST",
    "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
    "operationVersion": "1.0",
    "category": "KeyDeliveryRequests",
    "resultType": "Succeeded",
    "resultSignature": "OK",
    "durationMs": 315,
    "identity": {
        "authorization": {
            "issuer": "http://testacs",
            "audience": "urn:test"
        },
        "claims": {
            "urn:microsoft:azure:mediaservices:contentkeyidentifier": "3321e646-78d0-4896-84ec-c7b98eddfca5",
            "iss": "http://testacs",
            "aud": "urn:test",
            "exp": "1547233138"
        }
    },
    "level": "Informational",
    "location": "uswestcentral",
    "properties": {
        "requestId": "b0243468-d8e5-4edf-a48b-d408e1661050",
        "keyType": "Clear",
        "keyId": "3321e646-78d0-4896-84ec-c7b98eddfca5",
        "policyName": "56a70229-82d0-4174-82bc-e9d3b14e5dbf",
        "tokenType": "JWT",
        "statusMessage": "OK"
    }
} 
```

```json
 {
    "time": "2019-01-11T17:59:33.4676382Z",
    "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000/RESOURCEGROUPS/SBKEY/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/SBDNSTEST",
    "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
    "operationVersion": "1.0",
    "category": "KeyDeliveryRequests",
    "resultType": "Failed",
    "resultSignature": "Unauthorized",
    "durationMs": 2,
    "level": "Error",
    "location": "uswestcentral",
    "properties": {
        "requestId": "875af030-b77c-416b-b7e1-58f23ebec182",
        "keyType": "Clear",
        "keyId": "3321e646-78d0-4896-84ec-c7b98eddfca5",
        "policyName": "56a70229-82d0-4174-82bc-e9d3b14e5dbf",
        "tokenType": "None",
        "statusMessage": "No token present in authorization header or URL."
    }
} 
```

## <a name="next-steps"></a>Volgende stappen

[Diagnostische logboeken en metrische gegevens over Media Services controleren](media-services-metrics-diagnostic-logs.md)
