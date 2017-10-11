---
title: Azure Event raster abonnement schema
description: Beschrijft de eigenschappen voor het abonneren op een gebeurtenis met gebeurtenis raster van Azure.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/17/2017
ms.author: babanisa
ms.openlocfilehash: eff2352066a76010d6d882a7b7e1961870cd2d46
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="event-grid-subscription-schema"></a>Gebeurtenis raster abonnement schema

Voor het maken van een gebeurtenis raster-abonnement, kunt u een aanvraag verzendt naar het maken van de gebeurtenis abonnement opnieuw. Gebruik de volgende notatie:

```
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

Bijvoorbeeld, een gebeurtenisabonnement voor een opslagaccount maken met de naam `examplestorage` in een resourcegroep met de naam `examplegroup`, gebruik de volgende notatie:

```
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

Het artikel beschrijft de eigenschappen en het schema voor de hoofdtekst van de aanvraag.
 
## <a name="event-subscription-properties"></a>Eigenschappen van gebeurtenis abonnement

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| Bestemming | object | Het object dat het eindpunt definieert. |
| Filter | object | Een optioneel veld voor het filteren van de typen gebeurtenissen. |

### <a name="destination-object"></a>doelobject

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| endpointType | Tekenreeks | Het type van het eindpunt voor het abonnement (webhook/HTTP, Event Hub of wachtrij). | 
| bij voor endpointUrl | Tekenreeks |  | 

### <a name="filter-object"></a>Filter-object

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| includedEventTypes | matrix | Treffer wanneer het gebeurtenistype bericht in de gebeurtenis is een exacte overeenkomst voor een van deze gebeurtenis type-namen. Er wordt een fout bij het gebeurtenisnaam komt niet overeen met de namen van het type geregistreerde gebeurtenis voor de gebeurtenisbron. Standaard komt overeen met alle types van gebeurtenissen. |
| subjectBeginsWith | Tekenreeks | Een voorvoegsel-overeenkomst filteren op het onderwerpveld in de gebeurtenisstroom bericht. De standaard- of lege tekenreeks komt overeen met alle. | 
| subjectEndsWith | Tekenreeks | Een achtervoegsel-overeenkomst filteren op het onderwerpveld in de gebeurtenisstroom bericht. De standaard- of lege tekenreeks komt overeen met alle. |
| subjectIsCaseSensitive | Tekenreeks | Hoofdlettergevoelige die overeenkomt met filters voor besturingselementen. |


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
      "includedEventTypes": [ "blobCreated", "blobDeleted" ],
      "subjectBeginsWith": "blobServices/default/containers/mycontainer/log",
      "subjectEndsWith": ".jpg",
      "subjectIsCaseSensitive": "true"
    }
  }
}
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot gebeurtenis raster, [wat gebeurtenis raster is?](overview.md)