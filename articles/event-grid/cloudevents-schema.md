---
title: Gebruik Azure Event Grid met gebeurtenissen in een CloudEvents-schema
description: Beschrijft hoe u het instellen van een CloudEvents-schema voor gebeurtenissen in Azure Event Grid.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 07/13/2018
ms.author: babanisa
ms.openlocfilehash: 41e7be80eb67deaf7a8189aa0d9f62c48b88799a
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39036234"
---
# <a name="use-cloudevents-schema-with-event-grid"></a>Gebruik een CloudEvents-schema met Event Grid

Naast de [gebeurtenisschema in het standaard](event-schema.md), gebeurtenissen in systeemeigen ondersteuning biedt voor Azure Event Grid de [CloudEvents JSON-schema](https://github.com/cloudevents/spec/blob/master/json-format.md). [Een CloudEvents](http://cloudevents.io/) is een [open standaard-specificatie](https://github.com/cloudevents/spec/blob/master/spec.md) voor het beschrijven van gebeurtenisgegevens in een veelgebruikte manier.

Een CloudEvents vereenvoudigt interoperabiliteit door te geven van een gemeenschappelijk gebeurtenisschema voor het publiceren en gebruiken van cloud op basis van gebeurtenissen. Dit schema kunt u uniform tooling standard manieren van Routering en verwerken van gebeurtenissen en universele manieren om bij het deserialiseren van de buitenste gebeurtenisschema. U kunt werken eenvoudiger verschillende platforms integreren met een gemeenschappelijk schema.

Een CloudEvents wordt build door verschillende [samenwerkers](https://github.com/cloudevents/spec/blob/master/community/contributors.md), waaronder Microsoft, via de [Cloud Native Compute Foundation](https://www.cncf.io/). Het is momenteel beschikbaar als versie 0.1.

In dit artikel wordt beschreven hoe u een CloudEvents-schema met Event Grid gebruiken.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="cloudevent-schema"></a>CloudEvent schema

Hier volgt een voorbeeld van een Azure Blob Storage-gebeurtenis in een CloudEvents-indeling:

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

Een CloudEvents v0.1 heeft de volgende eigenschappen beschikbaar:

| Een CloudEvents        | Type     | Voorbeeld van JSON-waarde             | Beschrijving                                                        | Event Grid-toewijzing
|--------------------|----------|--------------------------------|--------------------------------------------------------------------|-------------------------
| type gebeurtenis          | Reeks   | "com.example.someevent"          | Type gebeurtenis die heeft plaatsgevonden                                   | type gebeurtenis
| eventTypeVersion   | Reeks   | "1.0"                            | De versie van het type gebeurtenis (optioneel)                            | dataVersion
| cloudEventsVersion | Reeks   | "0.1"                            | De versie van de specificatie van een CloudEvents die maakt gebruik van de gebeurtenis        | *doorgegeven*
| source             | URI      | "/ mycontext"                     | Beschrijving van de gebeurtenisproducent                                       | onderwerp #subject
| gebeurtenis-id            | Reeks   | '1234-1234-1234'                 | ID van de gebeurtenis                                                    | id
| eventTime          | Timestamp| "2018-04-05T17:31:00Z"           | Timestamp van wanneer de gebeurtenis heeft plaatsgevonden (optioneel)                    | eventTime
| schemaURL          | URI      | "https://myschema.com"           | Een koppeling naar het schema dat het kenmerk heeft (optioneel) | *niet gebruikt*
| contentType        | Reeks   | 'application/json'               | Beschrijf de coderingsindeling van de gegevens (optioneel)                       | *niet gebruikt*
| Extensies         | Kaart      | {"extra": "vA", "extB", "vB"}  | Alle aanvullende metagegevens (optioneel)                                 | *niet gebruikt*
| gegevens               | Object   | {"objA": "vA", "objB", "vB"}  | De nettolading van de gebeurtenis (optioneel)                                       | gegevens

Zie voor meer informatie de [een CloudEvents-specificatie](https://github.com/cloudevents/spec/blob/master/spec.md#context-attributes).

## <a name="configure-event-grid-for-cloudevents"></a>Event Grid voor een CloudEvents configureren

U kunt Event Grid gebruiken voor zowel invoer en uitvoer van gebeurtenissen in een CloudEvents-schema. U kunt een CloudEvents gebruiken voor systeemgebeurtenissen, zoals gebeurtenissen van Blob Storage en IoT Hub-gebeurtenissen en aangepaste gebeurtenissen. Het kunt ook gebeurtenissen op de kabel heen en weer transformeren.


| Invoer schema       | Uitvoerschema
|--------------------|---------------------
| Een CloudEvents-indeling | Een CloudEvents-indeling
| Event Grid-indeling  | Een CloudEvents-indeling
| Een CloudEvents-indeling | Event Grid-indeling
| Event Grid-indeling  | Event Grid-indeling

Voor alle gebeurtenis schema's, wordt in Event Grid validatie vereist bij het publiceren naar een event grid-onderwerp en bij het maken van een gebeurtenisabonnement. Zie voor meer informatie, [Event Grid-beveiliging en verificatie](security-authentication.md).

### <a name="input-schema"></a>Invoer schema

Om het invoerschema op een aangepast onderwerp op een CloudEvents, gebruikt u de volgende parameter in de Azure CLI bij het maken van uw onderwerp `--input-schema cloudeventv01schema`. Het aangepaste onderwerp dat nu wordt verwacht dat binnenkomende gebeurtenissen in een CloudEvents v0.1-indeling.

Gebruik het volgende voor het maken van een event grid-onderwerp:

```azurecli
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  --name <topic_name> \
  -l westcentralus \
  -g gridResourceGroup \
  --input-schema cloudeventv01schema
```

De huidige versie van een CloudEvents biedt geen ondersteuning voor batchverwerking van gebeurtenissen. Voor het publiceren van gebeurtenissen met CloudEvent schema naar een onderwerp, elke gebeurtenis afzonderlijk te publiceren.

### <a name="output-schema"></a>Uitvoerschema

Om in te stellen het uitvoerschema op een gebeurtenisabonnement op een CloudEvents, gebruikt u de volgende parameter in de Azure CLI bij het maken van het gebeurtenisabonnement `--event-delivery-schema cloudeventv01schema`. Gebeurtenissen voor dit gebeurtenisabonnement worden nu in een CloudEvents v0.1 indeling worden geleverd.

Gebruik het volgende voor het maken van een gebeurtenisabonnement:

```azurecli
az eventgrid event-subscription create \
  --name <event_subscription_name> \
  --topic-name <topic_name> \
  -g gridResourceGroup \
  --endpoint <endpoint_URL> \
  --event-delivery-schema cloudeventv01schema
```

De huidige versie van de een CloudEvents biedt geen ondersteuning voor batchverwerking van gebeurtenissen. Een gebeurtenisabonnement die geconfigureerd voor het schema CloudEvent ontvangt afzonderlijk elke gebeurtenis. Op dit moment kunt u een Event Grid-trigger voor een Azure Functions-app wanneer de gebeurtenis wordt geleverd in de een CloudEvents-schema. U moet een HTTP-trigger. Zie voor meer voorbeelden van de implementatie van een HTTP-trigger die ontvangt gebeurtenissen in het schema een CloudEvents [een HTTP-trigger gebruiken als een trigger Gebeurtenisraster](../azure-functions/functions-bindings-event-grid.md#use-an-http-trigger-as-an-event-grid-trigger).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het controleren van de gebeurtenis leveringen [Monitor Event Grid berichtbezorging](monitor-event-delivery.md).
* We raden u aan te testen, opmerking, en [bijdragen](https://github.com/cloudevents/spec/blob/master/CONTRIBUTING.md) naar een CloudEvents.
* Zie voor meer informatie over het maken van een Azure Event Grid-abonnement [Event Grid-abonnementsschema](subscription-creation-schema.md).
