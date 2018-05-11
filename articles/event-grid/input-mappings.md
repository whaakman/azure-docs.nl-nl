---
title: Aangepaste veldnamen worden toegewezen aan Azure gebeurtenis raster schema
description: Beschrijft hoe u uw aangepaste schema converteren naar het schema Azure gebeurtenis raster.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 05/09/2018
ms.author: tomfitz
ms.openlocfilehash: 1e809f83b43c32031b66c8f470575da6e9fcdc56
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="map-custom-fields-to-event-grid-schema"></a>Aangepaste velden worden toegewezen aan de gebeurtenis raster schema

Als uw gebeurtenisgegevens niet overeenkomt met de verwachte [gebeurtenis raster schema](event-schema.md), kunt u de gebeurtenis raster route gebeurtenis naar abonnees. Dit artikel wordt beschreven hoe u uw schema toegewezen aan het raster voor gebeurtenis-schema.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="original-event-schema"></a>Oorspronkelijke gebeurtenis schema

Stel dat u een toepassing die u gebeurtenissen in de volgende indeling verzendt hebben:

```json
[
  {
    "myEventTypeField":"Created",
    "resource":"Users/example/Messages/1000",
    "resourceData":{"someDataField1":"SomeDataFieldValue"}
  }
]
```

Hoewel deze indeling komt niet overeen met het vereiste schema, wordt gebeurtenis raster kunt u de velden toegewezen aan het schema. Of u kunt de waarden in het oorspronkelijke schema ontvangen.

## <a name="create-custom-topic-with-mapped-fields"></a>Maken van aangepaste onderwerp met toegewezen velden

Wanneer u een aangepaste onderwerp maakt, Geef op hoe de velden van uw oorspronkelijke gebeurtenis aan het raster gebeurtenis schema toe te wijzen. Er zijn drie eigenschappen die u gebruikt voor het aanpassen van de toewijzing:

* De `--input-schema` parameter geeft het type van het schema. De beschikbare opties zijn *cloudeventv01schema*, *customeventschema*, en *eventgridschema*. De standaardwaarde is eventgridschema. Gebruik customeventschema bij het maken van aangepaste toewijzing tussen uw schema en de gebeurtenis raster schema. Wanneer gebeurtenissen in het schema CloudEvents, gebruik cloudeventv01schema.

* De `--input-mapping-default-values` parameter geeft de standaardwaarden voor velden in het schema van de gebeurtenis raster. U kunt standaardwaarden instellen voor *onderwerp*, *eventtype*, en *dataversion*. Normaal gesproken gebruik deze parameter wanneer uw aangepaste schema geen een veld dat overeenkomt met een van deze drie velden. U kunt bijvoorbeeld opgeven dat dataversion is altijd ingesteld op **1.0**.

* De `--input-mapping-fields` parameter velden uit uw schema aan het raster gebeurtenis schema wordt toegewezen. U kunt waarden opgeven in een door spaties gescheiden sleutel/waarde-paren. Gebruik de naam van de gebeurtenis raster veld voor de naam van de sleutel. Gebruik de naam van het veld voor de waarde. U kunt de namen van de sleutels voor *id*, *onderwerp*, *eventtime*, *onderwerp*, *eventtype*, en *dataversion*.

Het volgende voorbeeld maakt een aangepaste onderwerp met enkele toegewezen en de standaardinstellingen van de velden:

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  -n demotopic \
  -l eastus2 \
  -g myResourceGroup \
  --input-schema customeventschema
  --input-mapping-fields eventType=myEventTypeField \
  --input-mapping-default-values subject=DefaultSubject dataVersion=1.0
```

## <a name="subscribe-to-event-grid-topic"></a>Abonneren op het raster onderwerp over de gebeurtenis

Wanneer u zich abonneert op het aangepaste onderwerp, geeft u het schema dat u gebruiken wilt voor het ontvangen van de gebeurtenissen. U gebruikt de `--event-delivery-schema` parameter en wordt ingesteld op *cloudeventv01schema*, *eventgridschema*, of *inputeventschema*. De standaardwaarde is eventgridschema.

De voorbeelden in deze sectie wordt een Queue storage gebruiken voor de gebeurtenis-handler. Zie voor meer informatie [aangepaste gebeurtenissen doorsturen naar Azure Queue storage](custom-event-to-queue-storage.md).

Het volgende voorbeeld is lid van een gebeurtenis raster onderwerp en gebruikt het standaardschema voor het raster van gebeurtenis:

```azurecli-interactive
az eventgrid event-subscription create \
  --topic-name demotopic \
  -g myResourceGroup \
  --name eventsub1 \
  --endpoint-type storagequeue \
  --endpoint <storage-queue-url>
```

Het volgende voorbeeld wordt de invoer schema van de gebeurtenis:

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

U bent nu klaar om te een gebeurtenis verzenden naar het aangepaste onderwerp en het resultaat van de toewijzing te bekijken. Het volgende script voor het plaatsen van een gebeurtenis in de [voorbeeldschema](#original-event-schema):

```azurecli-interactive
endpoint=$(az eventgrid topic show --name demotopic -g myResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name demotopic -g myResourceGroup --query "key1" --output tsv)

body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/mapeventfields.json)'")

curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

Bekijk nu uw Queue storage. De twee abonnementen bezorgd gebeurtenissen in verschillende schema's.

Het eerste abonnement gebeurtenis raster schema gebruikt. De indeling van de geleverde gebeurtenis is:

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

Deze velden bevatten de toewijzingen van het aangepaste onderwerp. **myEventTypeField** is toegewezen aan **EventType**. De standaardwaarden voor **DataVersion** en **onderwerp** worden gebruikt. De **gegevens** object bevat de oorspronkelijke gebeurtenis schemavelden.

Het tweede abonnement gebruikt de invoergebeurtenis-schema. De indeling van de geleverde gebeurtenis is:

```json
{
  "myEventTypeField": "Created",
  "resource": "Users/example/Messages/1000",
  "resourceData": { "someDataField1": "SomeDataFieldValue" }
}
```

U ziet dat de oorspronkelijke velden zijn geleverd.

## <a name="next-steps"></a>Volgende stappen

* Voor informatie over de levering van de gebeurtenis en nieuwe pogingen, [gebeurtenis raster berichtbezorging en probeer het opnieuw](delivery-and-retry.md).
* Zie voor een inleiding tot gebeurtenis raster, [over gebeurtenis raster](overview.md).
* Zie om snel aan de slag met Event raster [maken en route aangepaste gebeurtenissen met Azure Event raster](custom-event-quickstart.md).
