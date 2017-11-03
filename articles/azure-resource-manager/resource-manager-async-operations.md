---
title: Azure asynchrone bewerkingen | Microsoft Docs
description: Beschrijving van het bijhouden van asynchrone bewerkingen in Azure.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/11/2017
ms.author: tomfitz
ms.openlocfilehash: 9fe3d98cd345aae45722295b6c1b7fc3e9036e95
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="track-asynchronous-azure-operations"></a>Asynchrone bewerkingen voor Azure bijhouden
Sommige Azure REST-bewerkingen asynchroon uitgevoerd omdat de bewerking kan niet snel worden voltooid. In dit onderwerp wordt beschreven hoe de status van asynchrone bewerkingen via waarden in het antwoord geretourneerd.  

## <a name="status-codes-for-asynchronous-operations"></a>Statuscodes voor asynchrone bewerkingen
Een asynchrone bewerking retourneert in eerste instantie een HTTP-statuscode van een van beide:

* 201 (gemaakt)
* 202 (geaccepteerde) 

Wanneer de bewerking is voltooid, wordt een:

* 200 (OK)
* 204 (geen inhoud) 

Raadpleeg de [REST API-documentatie](/rest/api/) om te zien van de antwoorden voor de bewerking die u uitvoert. 

## <a name="monitor-status-of-operation"></a>Monitor status van bewerking
De asynchrone REST-bewerkingen retourwaarden header die u gebruikt om de status van de bewerking te bepalen. Er zijn mogelijk drie headerwaarden om te controleren:

* `Azure-AsyncOperation`-URL voor het controleren van de actieve status van de bewerking. Als de bewerking voor deze waarde retourneert, gebruik altijd het (in plaats van locatie) de status van de bewerking.
* `Location`-URL om te bepalen wanneer een bewerking is voltooid. Gebruik deze waarde alleen als Azure-asynchrone bewerking wordt niet geretourneerd.
* `Retry-After`-Het aantal seconden moet worden gewacht voordat de status van de asynchrone bewerking.

Niet elke asynchrone bewerking retourneert echter al deze waarden. U wilt bijvoorbeeld de waarde van de Azure-asynchrone bewerking header voor één bewerking en de waarde van de locatie-header voor een andere bewerking evalueren. 

U haalt u de waarden van de koptekst als u een headerwaarde voor een aanvraag wilt ophalen. In C#, ophalen u bijvoorbeeld de waarde van de header van een `HttpWebResponse` object met de naam `response` met de volgende code:

```cs
response.Headers.GetValues("Azure-AsyncOperation").GetValue(0)
```

## <a name="azure-asyncoperation-request-and-response"></a>Azure-asynchrone bewerking aanvraag en -antwoord

Als u de status van de asynchrone bewerking, een GET-aanvraag te verzenden naar de URL in de kopwaarde Azure-asynchrone bewerking.

De hoofdtekst van het antwoord van deze bewerking bevat informatie over het opnieuw. Het volgende voorbeeld ziet u de mogelijke waarden geretourneerd van de bewerking:

```json
{
    "id": "{resource path from GET operation}",
    "name": "{operation-id}", 
    "status" : "Succeeded | Failed | Canceled | {resource provider values}", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
    "percentComplete": {double between 0 and 100 },
    "properties": {
        /* Specific resource provider values for successful operations */
    },
    "error" : { 
        "code": "{error code}",  
        "message": "{error description}" 
    }
}
```

Alleen `status` wordt geretourneerd voor alle antwoorden op. Het foutobject wordt geretourneerd wanneer de status mislukt of geannuleerd is. Alle andere waarden zijn optioneel. daarom het antwoord dat u ontvangt zien er mogelijk anders dan in het voorbeeld.

## <a name="provisioningstate-values"></a>provisioningState waarden

Bewerkingen die maken, bijwerken of verwijderen (PUT, PATCH, verwijderen) een resource doorgaans retourneren een `provisioningState` waarde. Wanneer een bewerking is voltooid, wordt de volgende drie waarden geretourneerd: 

* Geslaagd
* Is mislukt
* Geannuleerd

Alle andere waarden geven dat de bewerking wordt nog steeds uitgevoerd. De resourceprovider kan een aangepaste waarde die aangeeft van de status retourneren. Bijvoorbeeld, krijgt u **geaccepteerde** wanneer de aanvraag is ontvangen en wordt uitgevoerd.

## <a name="example-requests-and-responses"></a>Voorbeeld aanvragen en antwoorden

### <a name="start-virtual-machine-202-with-azure-asyncoperation"></a>Start de virtuele machine (202 met Azure-asynchrone bewerking)
Dit voorbeeld ziet u hoe u bepaalt de status van **start** bewerking voor virtuele machines. De eerste aanvraag is in de volgende indeling:

```HTTP
POST 
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Compute/virtualMachines/{vm-name}/start?api-version=2016-03-30
```

Deze retourneert statuscode 202. U ziet tussen de headerwaarden:

```HTTP
Azure-AsyncOperation : https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

Controleer de status van de asynchrone bewerking, een andere aanvraag verzenden naar deze URL.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

Hoofdtekst van de reactie bevat de status van de bewerking:

```json
{
  "startTime": "2017-01-06T18:58:24.7596323+00:00",
  "status": "InProgress",
  "name": "9a062a88-e463-4697-bef2-fe039df73a02"
}
```

### <a name="deploy-resources-201-with-azure-asyncoperation"></a>Resources (201 met Azure-asynchrone bewerking) implementeren

Dit voorbeeld ziet u hoe u bepaalt de status van **implementaties** bewerking voor het implementeren van resources in Azure. De eerste aanvraag is in de volgende indeling:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/microsoft.resources/deployments/{deployment-name}?api-version=2016-09-01
```

Deze retourneert statuscode 201. De hoofdtekst van het antwoord bevat:

```json
"provisioningState":"Accepted",
```

U ziet tussen de headerwaarden:

```HTTP
Azure-AsyncOperation: https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

Controleer de status van de asynchrone bewerking, een andere aanvraag verzenden naar deze URL.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

Hoofdtekst van de reactie bevat de status van de bewerking:

```json
{"status":"Running"}
```

Wanneer de implementatie is voltooid, wordt het antwoord bevat:

```json
{"status":"Succeeded"}
```

### <a name="create-storage-account-202-with-location-and-retry-after"></a>Storage-account (202 met locatie en probeer het opnieuw na) maken

Dit voorbeeld ziet u hoe u bepaalt de status van de **maken** bewerking voor storage-accounts. De eerste aanvraag is in de volgende indeling:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2016-01-01
```

En de hoofdtekst van de aanvraag bevat de eigenschappen voor het opslagaccount:

```json
{ "location": "South Central US", "properties": {}, "sku": { "name": "Standard_LRS" }, "kind": "Storage" }
```

Deze retourneert statuscode 202. Tussen de headerwaarden ziet u de volgende twee waarden:

```HTTP
Location: https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
Retry-After: 17
```

Nadat het wachten op aantal seconden zijn opgegeven in de nieuwe poging na, Controleer de status van de asynchrone bewerking door een andere aanvraag verzenden naar deze URL.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
```

Als de aanvraag wordt nog steeds uitgevoerd, ontvangt u een statuscode 202. Als de aanvraag is voltooid, de statuscode 200 ontvangt en de hoofdtekst van het antwoord bevat de eigenschappen van het opslagaccount dat is gemaakt.

## <a name="next-steps"></a>Volgende stappen

* Zie voor documentatie over elke REST-bewerking [REST API-documentatie](/rest/api/).
* Zie voor meer informatie over het beheren van resources via de REST-API van Resource Manager [met de REST-API van Resource Manager](resource-manager-rest-api.md).
* Zie voor meer informatie over het implementeren van sjablonen met de REST-API van Resource Manager [resources met Resource Manager-sjablonen en REST-API van Resource Manager implementeren](resource-group-template-deploy-rest.md).