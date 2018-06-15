---
title: Azure Event raster abonnement schema
description: Beschrijft de eigenschappen voor het abonneren op een gebeurtenis met gebeurtenis raster van Azure.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 05/02/2018
ms.author: babanisa
ms.openlocfilehash: cfb4dabea12f2988108d24b025e324cf05afb325
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
ms.locfileid: "34301716"
---
# <a name="event-grid-subscription-schema"></a>Gebeurtenis raster abonnement schema

Voor het maken van een gebeurtenis raster-abonnement, kunt u een aanvraag verzendt naar het maken van de gebeurtenis abonnement opnieuw. Gebruik de volgende notatie:

```HTTP
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2018-01-01
``` 

Bijvoorbeeld, een gebeurtenisabonnement voor een opslagaccount maken met de naam `examplestorage` in een resourcegroep met de naam `examplegroup`, gebruik de volgende notatie:

```HTTP
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2018-01-01
``` 

Het artikel beschrijft de eigenschappen en het schema voor de hoofdtekst van de aanvraag.
 
## <a name="event-subscription-properties"></a>Eigenschappen van gebeurtenis abonnement

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| destination | object | Het object dat het eindpunt definieert. |
| filter | object | Een optioneel veld voor het filteren van de typen gebeurtenissen. |

### <a name="destination-object"></a>doelobject

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| endpointType | tekenreeks | Het type van het eindpunt voor het abonnement (webhook/HTTP, Event Hub of wachtrij). | 
| bij voor endpointUrl | tekenreeks | De doel-URL voor gebeurtenissen in dit gebeurtenisabonnement. | 

### <a name="filter-object"></a>Filter-object

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| includedEventTypes | matrix | Treffer wanneer het gebeurtenistype bericht in de gebeurtenis is een exacte overeenkomst voor een van deze gebeurtenis type-namen. Er wordt een fout bij het gebeurtenisnaam komt niet overeen met de namen van het type geregistreerde gebeurtenis voor de gebeurtenisbron. Standaard komt overeen met alle types van gebeurtenissen. |
| subjectBeginsWith | tekenreeks | Een voorvoegsel-overeenkomst filteren op het onderwerpveld in de gebeurtenisstroom bericht. De standaard- of lege tekenreeks komt overeen met alle. | 
| subjectEndsWith | tekenreeks | Een achtervoegsel-overeenkomst filteren op het onderwerpveld in de gebeurtenisstroom bericht. De standaard- of lege tekenreeks komt overeen met alle. |
| isSubjectCaseSensitive | tekenreeks | Hoofdlettergevoelige die overeenkomt met filters voor besturingselementen. |


## <a name="example-subscription-schema"></a>Voorbeeld abonnement schema

```json
{
  "properties": {
    "destination": {
      "endpointType": "webhook",
      "properties": {
          "endpointUrl": "https://example.azurewebsites.net/api/HttpTriggerCSharp1?code=VXbGWce53l48Mt8wuotr0GPmyJ/nDT4hgdFj9DpBiRt38qqnnm5OFg=="
      }
    },
    "filter": {
      "includedEventTypes": [ "Microsoft.Storage.BlobCreated", "Microsoft.Storage.BlobDeleted" ],
      "subjectBeginsWith": "blobServices/default/containers/mycontainer/log",
      "subjectEndsWith": ".jpg",
      "isSubjectCaseSensitive ": "true"
    }
  }
}
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot gebeurtenis raster, [wat gebeurtenis raster is?](overview.md)