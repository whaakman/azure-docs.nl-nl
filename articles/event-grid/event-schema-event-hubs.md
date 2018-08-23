---
title: Azure Event Grid event hubs-gebeurtenisschema
description: Beschrijft de eigenschappen die beschikbaar zijn voor gebeurtenissen van event hubs met Azure Event Grid
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: reference
ms.date: 08/17/2018
ms.author: tomfitz
ms.openlocfilehash: e301f3895126ed52b8d4c1f046f69dfcedb3563c
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "42055313"
---
# <a name="azure-event-grid-event-schema-for-event-hubs"></a>Azure Event Grid-gebeurtenisschema voor eventhubs

Dit artikel bevat de eigenschappen en het schema voor event hubs-gebeurtenissen. Zie voor een inleiding tot gebeurtenisschema's, [Azure Event Grid-gebeurtenisschema](event-schema.md).

Zie voor een lijst met zelfstudies en voorbeelden van scripts, [Event Hubs-gebeurtenisbron](event-sources.md#event-hubs).

### <a name="available-event-types"></a>Typen van de gebeurtenis berichten beschikbaar

Eventhubs verzendt de **Microsoft.EventHub.CaptureFileCreated** gebeurtenistype wanneer een bestand vastleggen wordt gemaakt.

## <a name="example-event"></a>Voorbeeld van de gebeurtenis

Deze voorbeeldgebeurtenis ziet u het schema van een event hubs-gebeurtenis treedt op wanneer de capture-functie wordt een bestand opgeslagen: 

```json
[
    {
        "topic": "/subscriptions/<guid>/resourcegroups/rgDataMigrationSample/providers/Microsoft.EventHub/namespaces/tfdatamigratens",
        "subject": "eventhubs/hubdatamigration",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-08-31T19:12:46.0498024Z",
        "id": "14e87d03-6fbf-4bb2-9a21-92bd1281f247",
        "data": {
            "fileUrl": "https://tf0831datamigrate.blob.core.windows.net/windturbinecapture/tfdatamigratens/hubdatamigration/1/2017/08/31/19/11/45.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 249168,
            "eventCount": 1500,
            "firstSequenceNumber": 2400,
            "lastSequenceNumber": 3899,
            "firstEnqueueTime": "2017-08-31T19:12:14.674Z",
            "lastEnqueueTime": "2017-08-31T19:12:44.309Z"
        },
        "dataVersion": "",
        "metadataVersion": "1"
    }
]
```

## <a name="event-properties"></a>Eigenschappen van gebeurtenis

Een gebeurtenis heeft de volgende gegevens op het hoogste niveau:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| onderwerp | tekenreeks | Volledige resource-pad naar de bron van de gebeurtenis. Dit veld is niet beschrijfbaar. Event Grid biedt deze waarde. |
| Onderwerp | tekenreeks | Uitgever gedefinieerde pad naar het onderwerp van de gebeurtenis. |
| type gebeurtenis | tekenreeks | Een van de geregistreerde gebeurtenis-typen voor de bron van deze gebeurtenis. |
| eventTime | tekenreeks | Het moment waarop dat de gebeurtenis is gegenereerd, is afhankelijk van de UTC-tijd van de provider. |
| id | tekenreeks | De unieke id voor de gebeurtenis. |
| gegevens | object | Gebeurtenisgegevens van de Event hub. |
| dataVersion | tekenreeks | De schemaversie van het gegevensobject. De uitgever definieert de schemaversie. |
| metadataVersion | tekenreeks | De schemaversie van de metagegevens van de gebeurtenis. Event Grid definieert het schema van de eigenschappen op het hoogste niveau. Event Grid biedt deze waarde. |

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| fileUrl | tekenreeks | Het pad naar het bestand vastleggen. |
| Bestandstype | tekenreeks | Het bestandstype van het bestand vastleggen. |
| PartitionId | tekenreeks | De shard-ID. |
| sizeInBytes | geheel getal | De bestandsgrootte. |
| eventCount | geheel getal | Het aantal gebeurtenissen in het bestand. |
| firstSequenceNumber | geheel getal | Het kleinste volgnummer uit de wachtrij. |
| lastSequenceNumber | geheel getal | Het laatste volgnummer uit de wachtrij. |
| firstEnqueueTime | tekenreeks | De eerste keer uit de wachtrij. |
| lastEnqueueTime | tekenreeks | De laatste keer uit de wachtrij. |

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot Azure Event Grid, [wat is Event Grid?](overview.md)
* Zie voor meer informatie over het maken van een Azure Event Grid-abonnement [Event Grid-abonnementsschema](subscription-creation-schema.md).
* Zie voor meer informatie over het verwerken van gebeurtenissen van event hubs [Stream van big data in een datawarehouse](event-grid-event-hubs-integration.md).