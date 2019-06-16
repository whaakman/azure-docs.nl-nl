---
title: Gebeurtenisschema in het Azure Event Grid Azure App-configuratie
description: Beschrijft de eigenschappen die beschikbaar zijn voor Azure App-configuratie-gebeurtenissen met Azure Event Grid
services: event-grid
author: jimmyca
ms.service: event-grid
ms.topic: reference
ms.date: 05/30/2019
ms.author: jimmyca
ms.openlocfilehash: fe0274f723692eea3cfd25cc0e9e146b35dce2ae
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66735779"
---
# <a name="azure-event-grid-event-schema-for-azure-app-configuration"></a>Azure Event Grid-gebeurtenisschema voor de configuratie van Azure-App

Dit artikel bevat de eigenschappen en het schema voor Azure App-Configuratiegebeurtenissen. Zie voor een inleiding tot gebeurtenisschema's, [Azure Event Grid-gebeurtenisschema](event-schema.md).

Zie voor een lijst met zelfstudies en voorbeelden van scripts, [Azure App-configuratie gebeurtenisbron](event-sources.md#app-configuration).

## <a name="available-event-types"></a>Typen van de gebeurtenis berichten beschikbaar

Azure App-configuratie verzendt de volgende typen gebeurtenissen:

| Gebeurtenistype | Description |
| ---------- | ----------- |
| Microsoft.AppConfiguration.KeyValueModified | Treedt op wanneer een sleutel / waarde wordt gemaakt of vervangen. |
| Microsoft.AppConfiguration.KeyValueDeleted | Treedt op wanneer een sleutel / waarde-wordt verwijderd. |

## <a name="example-event"></a>Voorbeeld van de gebeurtenis

Het volgende voorbeeld ziet u het schema van een sleutel / waarde-gewijzigde gebeurtenis: 

```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueModified",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

Het schema voor een sleutel / waarde-verwijderde gebeurtenis is vergelijkbaar: 

```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueDeleted",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```
 
## <a name="event-properties"></a>Eigenschappen van gebeurtenis

Een gebeurtenis heeft de volgende gegevens op het hoogste niveau:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| topic | string | Volledige resource-pad naar de bron van de gebeurtenis. Dit veld is niet beschrijfbaar. Event Grid biedt deze waarde. |
| subject | string | Uitgever gedefinieerde pad naar het onderwerp van de gebeurtenis. |
| eventType | string | Een van de geregistreerde gebeurtenis-typen voor de bron van deze gebeurtenis. |
| eventTime | string | Het moment waarop dat de gebeurtenis is gegenereerd, is afhankelijk van de UTC-tijd van de provider. |
| id | string | De unieke id voor de gebeurtenis. |
| data | object | Gebeurtenisgegevens voor App-configuratie. |
| dataVersion | string | De schemaversie van het gegevensobject. De uitgever definieert de schemaversie. |
| metadataVersion | string | De schemaversie van de metagegevens van de gebeurtenis. Event Grid definieert het schema van de eigenschappen op het hoogste niveau. Event Grid biedt deze waarde. |

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| key | string | De sleutel van de sleutel-waarde die is gewijzigd of verwijderd. |
| label | string | Het label, indien van toepassing, van de sleutel-waarde die is gewijzigd of verwijderd. |
| etag | string | Voor `KeyValueModified` de etag van de nieuwe sleutel-waarde. Voor `KeyValueDeleted` de etag van de sleutel-waarde die is verwijderd. |
 
## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot Azure Event Grid, [wat is Event Grid?](overview.md)
* Zie voor meer informatie over het maken van een Azure Event Grid-abonnement [Event Grid-abonnementsschema](subscription-creation-schema.md).
* Zie voor een inleiding tot het werken met Azure App-Configuratiegebeurtenissen, [Route Azure App-configuratie gebeurtenissen - Azure CLI](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json). 