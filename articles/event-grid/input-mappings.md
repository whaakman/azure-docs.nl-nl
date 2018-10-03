---
title: Aangepast veld toegewezen aan Azure Event Grid-schema
description: Beschrijft hoe u uw aangepaste schema converteren naar de Azure Event Grid-schema.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: tomfitz
ms.openlocfilehash: f79fa096484edc34294ea0a69584e12788dba647
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48043389"
---
# <a name="map-custom-fields-to-event-grid-schema"></a>Aangepaste velden toewijzen aan Event Grid-schema

Als uw gebeurtenisgegevens komt niet overeen met de verwachte [Event Grid schema](event-schema.md), u kunt nog steeds Event Grid op route-gebeurtenis voor abonnees. In dit artikel wordt beschreven hoe u uw schema toegewezen aan het Event Grid-schema.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="original-event-schema"></a>Gebeurtenisschema in het oorspronkelijke

Stel dat u hebt een toepassing die gebeurtenissen in de volgende indeling verzendt:

```json
[
  {
    "myEventTypeField":"Created",
    "resource":"Users/example/Messages/1000",
    "resourceData":{"someDataField1":"SomeDataFieldValue"}
  }
]
```

Hoewel deze indeling komt niet overeen met de vereiste schema, wordt er Event Grid kunt u uw velden toewijzen aan het schema. Of u kunt de waarden in het oorspronkelijke schema ontvangen.

## <a name="create-custom-topic-with-mapped-fields"></a>Aangepast onderwerp maken met toegewezen velden

Bij het maken van een aangepast onderwerp, Geef op hoe velden van uw oorspronkelijke gebeurtenis naar het event grid-schema toe te wijzen. Er zijn drie eigenschappen die u gebruiken om aan te passen van de toewijzing:

* De `--input-schema` parameter geeft u het type van het schema. De beschikbare opties zijn *cloudeventv01schema*, *customeventschema*, en *eventgridschema*. De standaardwaarde is eventgridschema. Gebruik customeventschema bij het maken van aangepaste toewijzing tussen het schema en het event grid-schema. Wanneer gebeurtenissen zich in een CloudEvents-schema, gebruikt u cloudeventv01schema.

* De `--input-mapping-default-values` parameter bepaalt de standaardwaarden voor de velden in het schema voor Event Grid. U kunt standaardwaarden instellen voor `subject`, `eventtype`, en `dataversion`. Meestal gebruikt u deze parameter wanneer uw aangepaste schema bevat geen een veld dat overeenkomt met een van deze drie velden. U kunt bijvoorbeeld opgeven die gegevensversie is altijd ingesteld op **1.0**.

* De `--input-mapping-fields` parameter toegewezen velden uit uw schema naar het event grid-schema. Geeft u waarden in een door spaties gescheiden sleutel/waarde-paren. Gebruik de naam van het event grid-veld voor de naam van de sleutel. Gebruik de naam van het veld voor de waarde. U kunt de namen van de sleutels voor `id`, `topic`, `eventtime`, `subject`, `eventtype`, en `dataversion`.

Het volgende voorbeeld maakt u een aangepast onderwerp met een aantal toegewezen en standaardvelden:

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  -n demotopic \
  -l eastus2 \
  -g myResourceGroup \
  --input-schema customeventschema \
  --input-mapping-fields eventType=myEventTypeField \
  --input-mapping-default-values subject=DefaultSubject dataVersion=1.0
```

## <a name="subscribe-to-event-grid-topic"></a>Abonneren op event grid-onderwerp

Wanneer u zich abonneert op het aangepaste onderwerp, geeft u het schema dat u gebruiken wilt voor het ontvangen van de gebeurtenissen. U gebruikt de `--event-delivery-schema` parameter instelt op *cloudeventv01schema*, *eventgridschema*, of *inputeventschema*. De standaardwaarde is eventgridschema.

De voorbeelden in deze sectie gebruiken een Queue storage voor de gebeurtenis-handler. Zie voor meer informatie, [aangepaste gebeurtenissen routeren naar Azure Queue storage](custom-event-to-queue-storage.md).

Het volgende voorbeeld zich abonneert op een event grid-onderwerp en de event grid-schema wordt gebruikt:

```azurecli-interactive
az eventgrid event-subscription create \
  --topic-name demotopic \
  -g myResourceGroup \
  --name eventsub1 \
  --event-delivery-schema eventgridschema \
  --endpoint-type storagequeue \
  --endpoint <storage-queue-url>
```

Het volgende voorbeeld wordt de invoer-schema van de gebeurtenis:

```azurecli-interactive
az eventgrid event-subscription create \
  --topic-name demotopic \
  -g myResourceGroup \
  --name eventsub2 \
  --event-delivery-schema inputeventschema \
  --endpoint-type storagequeue \
  --endpoint <storage-queue-url>
```

## <a name="publish-event-to-topic"></a>Gebeurtenis publiceren naar onderwerp

U kunt nu een gebeurtenis verzenden naar het aangepaste onderwerp en het resultaat van de toewijzing te bekijken. Het volgende script voor het plaatsen van een gebeurtenis in de [voorbeeldschema](#original-event-schema):

```azurecli-interactive
endpoint=$(az eventgrid topic show --name demotopic -g myResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name demotopic -g myResourceGroup --query "key1" --output tsv)

event='[ { "myEventTypeField":"Created", "resource":"Users/example/Messages/1000", "resourceData":{"someDataField1":"SomeDataFieldValue"} } ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $endpoint
```

Bekijk nu uw Queue storage. De twee abonnementen gebeurtenissen in verschillende schema's geleverd.

Het eerste abonnement gebruikt gebeurtenisschema in het raster. De indeling van de geleverde gebeurtenis is:

```json
{
  "Id": "016b3d68-881f-4ea3-8a9c-ed9246582abe",
  "EventTime": "2018-05-01T20:00:25.2606434Z",
  "EventType": "Created",
  "DataVersion": "1.0",
  "MetadataVersion": "1",
  "Topic": "/subscriptions/<subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.EventGrid/topics/demotopic",
  "Subject": "DefaultSubject",
  "Data": {
    "myEventTypeField": "Created",
    "resource": "Users/example/Messages/1000",
    "resourceData": { "someDataField1": "SomeDataFieldValue" } 
  }
}
```

Deze velden bevatten de toewijzingen van het aangepaste onderwerp. **myEventTypeField** is toegewezen aan **type gebeurtenis**. De standaardwaarden voor **DataVersion** en **onderwerp** worden gebruikt. De **gegevens** object bevat de oorspronkelijke gebeurtenis schemavelden.

Het tweede abonnement gebruikt de invoer-schema. De indeling van de geleverde gebeurtenis is:

```json
{
  "myEventTypeField": "Created",
  "resource": "Users/example/Messages/1000",
  "resourceData": { "someDataField1": "SomeDataFieldValue" }
}
```

U ziet dat de oorspronkelijke velden zijn geleverd.

## <a name="next-steps"></a>Volgende stappen

* Voor informatie over de bezorging van gebeurtenissen en nieuwe pogingen, [bezorging van berichten van Event Grid en probeer het opnieuw](delivery-and-retry.md).
* Zie [Een inleiding tot Event Grid](overview.md) voor een inleiding tot Event Grid.
* Als u wilt snel aan de slag met Event Grid, Zie [aangepaste gebeurtenissen maken en routeren met Azure Event Grid](custom-event-quickstart.md).
