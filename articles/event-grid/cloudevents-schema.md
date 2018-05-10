---
title: Gebruik van Azure Event raster met gebeurtenissen in CloudEvents schema
description: Hierin wordt beschreven hoe het schema CloudEvents voor gebeurtenissen in Azure gebeurtenis raster.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 04/30/2018
ms.author: babanisa
ms.openlocfilehash: a882073fce28be1b93a6c9118c40398062f61bc5
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="use-cloudevents-schema-with-event-grid"></a>Gebruik CloudEvents schema met gebeurtenis raster

Naast de [gebeurtenis standaardschema](event-schema.md), Azure gebeurtenis raster ondersteunt gebeurtenissen in de [CloudEvents JSON-schema](https://github.com/cloudevents/spec/blob/master/json-format.md). [CloudEvents](http://cloudevents.io/) is een [open standaard specificatie](https://github.com/cloudevents/spec/blob/master/spec.md) voor het beschrijven van gegevens van gebeurtenissen in een veelgebruikte manier.

CloudEvents interoperabiliteit vereenvoudigt door te geven van een algemene gebeurtenis-schema voor publicatie en gebruiken van de cloud op basis van gebeurtenissen. Dit schema staat uniform tooling, standaard manieren van Routering en verwerking van gebeurtenissen en universele manieren om bij het deserialiseren van het buitenste gebeurtenis schema. Met een gemeenschappelijk schema, kunt u gemakkelijker werk integreren in verschillende platforms.

CloudEvents wordt build door verschillende [deelnemers](https://github.com/cloudevents/spec/blob/master/community/contributors.md), met inbegrip van Microsoft, via de [Cloud systeemeigen Compute Foundation](https://www.cncf.io/). Het is momenteel beschikbaar als versie 0,1.

Dit artikel wordt beschreven hoe u het schema CloudEvents met gebeurtenis raster.

## <a name="cloudevent-schema"></a>CloudEvent schema

Hier volgt een voorbeeld van een Azure Blob Storage-gebeurtenis in de indeling CloudEvents:

``` JSON
{
    "cloudEventsVersion" : "0.1",
    "eventType" : "Microsoft.Storage.BlobCreated",
    "eventTypeVersion" : "",
    "source" : "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-account}#blobServices/default/containers/{storage-container}/blobs/{new-file}",
    "eventID" : "173d9985-401e-0075-2497-de268c06ff25",
    "eventTime" : "2018-04-28T02:18:47.1281675Z",
    "data" : {
      "api": "PutBlockList",
      "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
      "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
      "eTag": "0x8D4BCC2E4835CD0",
      "contentType": "application/octet-stream",
      "contentLength": 524288,
      "blobType": "BlockBlob",
      "url": "https://oc2d2817345i60006.blob.core.windows.net/oc2d2817345i200097container/oc2d2817345i20002296blob",
      "sequencer": "00000000000004420000000000028963",
      "storageDiagnostics": {
        "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
      }
    }
}
```

CloudEvents v0.1 heeft de volgende eigenschappen beschikbaar:

| CloudEvents        | Type     | Voorbeeld van de JSON-waarde             | Beschrijving                                                        | Gebeurtenis raster toewijzing
|--------------------|----------|--------------------------------|--------------------------------------------------------------------|-------------------------
| EventType          | Tekenreeks   | 'com.example.someevent'          | Type gebeurtenis die hebben plaatsgevonden                                   | EventType
| eventTypeVersion   | Tekenreeks   | "1.0"                            | De versie van de eventType (optioneel)                            | dataVersion
| cloudEventsVersion | Tekenreeks   | '0,1'                            | De versie van de CloudEvents-specificatie die gebruikmaakt van de gebeurtenis        | *doorgegeven*
| source             | URI      | ' / mycontext '                     | Beschrijft de producent gebeurtenis                                       | onderwerp #subject
| Gebeurtenis-id            | Tekenreeks   | '1234-1234-1234'                 | ID van de gebeurtenis                                                    | id
| eventTime          | Timestamp| ' 2018-04-05T17:31:00Z '           | Tijdstempel van wanneer de gebeurtenis heeft plaatsgevonden (optioneel)                    | eventTime
| schemaURL          | URI      | "https://myschema.com"           | Een koppeling naar het schema dat het kenmerk overeenstemming is met (optioneel) | *niet gebruikt*
| ContentType        | Tekenreeks   | 'application/json'               | Beschrijf de coderingsindeling van de gegevens (optioneel)                       | *niet gebruikt*
| Uitbreidingen         | Kaart      | {{'extA': 'vA', 'extB', "vB"}  | Alle aanvullende metagegevens (optioneel)                                 | *niet gebruikt*
| gegevens               | Object   | {{'objA': 'vA', 'objB', "vB"}  | De nettolading (optioneel)                                       | gegevens

Zie voor meer informatie de [CloudEvents spec](https://github.com/cloudevents/spec/blob/master/spec.md#context-attributes).

## <a name="configure-event-grid-for-cloudevents"></a>Gebeurtenis raster voor CloudEvents configureren

Op dit moment Azure gebeurtenis raster preview ondersteuning voor CloudEvents JSON-indeling-invoer en uitvoer in heeft **West-Centraal VS**, **VS-midden**, en **Noord-Europa**.

Als u wilt gebruiken CloudEvent, moet u een uitbreiding inschakelen voor Azure CLI:

```azurecli
az extension add –-name eventgrid
```

U kunt gebeurtenis raster gebruiken voor zowel invoer en uitvoer van gebeurtenissen in CloudEvents schema. U kunt CloudEvents gebruiken voor systeemgebeurtenissen, zoals gebeurtenissen voor Blob Storage en IoT Hub en aangepaste gebeurtenissen. Het kan ook gebeurtenissen op de kabel heen en weer worden getransformeerd.


| Invoer schema       | Uitvoerschema
|--------------------|---------------------
| CloudEvents-indeling | CloudEvents-indeling
| Gebeurtenis raster  | CloudEvents-indeling
| CloudEvents-indeling | Gebeurtenis raster
| Gebeurtenis raster  | Gebeurtenis raster

Voor alle gebeurtenis schema's gebeurtenis raster validatie is vereist bij het publiceren van een gebeurtenis raster onderwerp en bij het maken van een gebeurtenisabonnement. Zie voor meer informatie [gebeurtenis raster beveiligings- en verificatie](security-authentication.md).

### <a name="input-schema"></a>Invoer schema

Gebruik de volgende parameter om het invoerschema ingesteld op een aangepaste onderwerp aan CloudEvents, in de Azure CLI bij het maken van uw onderwerp `--input-schema cloudeventv01schema`. Het aangepaste onderwerp verwacht nu binnenkomende gebeurtenissen in CloudEvents v0.1 indeling.

Gebruik het volgende voor het maken van een gebeurtenis raster onderwerp:

```azurecli
az eventgrid topic create \
  --name <topic_name> \
  -l westcentralus \
  -g gridResourceGroup \
  --input-schema cloudeventv01schema
```

De huidige versie van CloudEvents biedt geen ondersteuning voor batchverwerking van gebeurtenissen. Voor het publiceren van gebeurtenissen met CloudEvent schema naar een onderwerp, elke gebeurtenis afzonderlijk te publiceren.

### <a name="output-schema"></a>Uitvoerschema

Gebruik de volgende parameter om in te stellen het uitvoerschema op een gebeurtenisabonnement op CloudEvents, in Azure CLI bij het maken van uw abonnement gebeurtenis `--event-delivery-schema cloudeventv01schema`. Gebeurtenissen voor dit abonnement op gebeurtenissen worden nu CloudEvents v0.1 indeling worden geleverd.

Voor een gebeurtenisabonnement gebruiken:

```azurecli
az eventgrid event-subscription create \
  --name <event_subscription_name> \  
  --topic-name <topic_name> \
  -g gridResourceGroup \
  --endpoint <endpoint_URL> \
  --event-delivery-schema cloudeventv01schema
```

De huidige versie van de CloudEvents biedt geen ondersteuning voor batchverwerking van gebeurtenissen. Een abonnement op gebeurtenissen die geconfigureerd voor CloudEvent schema ontvangt afzonderlijk elke gebeurtenis.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het controleren van de gebeurtenis leveringen [Monitor gebeurtenis raster berichtbezorging](monitor-event-delivery.md).
* We raden u aan het testen, opmerkingen, en [bijdragen](https://github.com/cloudevents/spec/blob/master/CONTRIBUTING.md) naar CloudEvents.
* Zie voor meer informatie over het maken van een abonnement op Azure gebeurtenis raster [gebeurtenis raster abonnement schema](subscription-creation-schema.md).
