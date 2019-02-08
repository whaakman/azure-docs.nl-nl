---
title: Ophalen van Azure resource health-gebeurtenissen met de REST API | Microsoft Docs
description: Gebruik de Azure REST API's om de health-gebeurtenissen voor uw Azure-resources.
author: stephbaron
ms.author: stbaron
ms.service: service-health
ms.custom: REST
ms.topic: article
ms.date: 06/06/2017
ms.openlocfilehash: 6d83aed6910127ceb34b9a694f48ca9c19ab6d18
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878388"
---
# <a name="get-resource-health-using-the-rest-api"></a>Ophalen Resource Health met behulp van de REST-API 

In dit artikel voorbeeld laat zien hoe ophalen van een lijst van health-gebeurtenissen voor de Azure-resources in uw abonnement met de [REST API van Azure](/rest/api/azure/).

Volledige documentatie en meer voorbeelden voor de REST-API zijn beschikbaar in de [naslaginformatie over de Azure Monitor REST](/rest/api/monitor). 

## <a name="build-the-request"></a>De aanvraag maken

Gebruik de volgende `GET` HTTP-aanvraag om de health-gebeurtenissen voor uw abonnement voor het bereik van de tijd tussen `2018-05-16` en `2018-06-20`.

```http
https://management.azure.com/subscriptions/{subscription-id}/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&%24filter=eventTimestamp%20ge%20'2018-05-16T04%3A36%3A37.6407898Z'%20and%20eventTimestamp%20le%20'2018-06-20T04%3A36%3A37.6407898Z'
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
| API-versie | De API-versie moet worden gebruikt voor de aanvraag.<br /><br /> In dit document bevat informatie over api-versie `2015-04-01`opgenomen in de bovenstaande URL.  |
| $filter | De filteroptie te verminderen van het aantal geretourneerde resultaten. De toegestane patronen voor deze parameter zijn beschikbaar [in de referentie voor de bewerking activiteitenlogboeken](/rest/api/monitor/activitylogs/list#uri-parameters). Het voorbeeld bevat alle gebeurtenissen in een periode tussen 2018-05-16 en 2018-06-20 |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>Aanvraagbody

Er is geen aanvraagtekst is nodig voor deze bewerking.

## <a name="handle-the-response"></a>Het antwoord verwerken

Statuscode 200 wordt geretourneerd met een lijst met waarden van de gezondheid van gebeurtenis overeenkomt met de filterparameter, samen met een `nextlink` URI om op te halen van de volgende pagina van de resultaten.

## <a name="example-response"></a>Voorbeeld van een antwoord 

```json
{
  "value": [
    {
      "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
      "eventName": {
        "value": "EndRequest",
        "localizedValue": "End request"
      },
      "id": "/subscriptions/{subscription-id}/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
      "resourceGroupName": "MSSupportGroup",
      "resourceProviderName": {
        "value": "microsoft.support",
        "localizedValue": "microsoft.support"
      },
      "operationName": {
        "value": "microsoft.support/supporttickets/write",
        "localizedValue": "microsoft.support/supporttickets/write"
      },
      "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
      },
      "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
      "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
      "level": "Informational"
    }
  ],
  "nextLink": "https://management.azure.com/########-####-####-####-############$skiptoken=######"
}
```
