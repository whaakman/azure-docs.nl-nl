---
title: Azure Event raster event hubs gebeurtenis schema
description: Beschrijft de eigenschappen die beschikbaar zijn voor event hubs gebeurtenissen met Azure Event raster
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: tomfitz
ms.openlocfilehash: 9fdc8816d8db88d4f1fd7b6ce722b7d2763eeaeb
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="azure-event-grid-event-schema-for-event-hubs"></a>Azure Event raster gebeurtenis schema voor event hubs

Dit artikel bevat de eigenschappen en het schema voor gebeurtenissen van event hubs. Zie voor een inleiding tot gebeurtenis schema's, [Azure gebeurtenis raster gebeurtenis schema](event-schema.md).

### <a name="available-event-types"></a>Typen beschikbare gebeurtenissen

Event Hubs verzendt de **Microsoft.EventHub.CaptureFileCreated** gebeurtenistype wanneer een bestand met vastleggen wordt gemaakt.

## <a name="example-event"></a>Voorbeeld van de gebeurtenis

Deze voorbeeldgebeurtenis ziet u het schema van een event hubs-gebeurtenis die optreedt wanneer de functie voor het vastleggen van een bestand wordt opgeslagen: 

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
| Onderwerp | tekenreeks | Volledige resource-pad naar de gegevensbron. Dit veld is niet beschrijfbaar. Gebeurtenis raster bevat deze waarde. |
| Onderwerp | tekenreeks | Publisher gedefinieerde pad naar het onderwerp van de gebeurtenis. |
| eventType | tekenreeks | Een van de typen van de geregistreerde gebeurtenis van de bron van deze gebeurtenis. |
| eventTime | tekenreeks | De tijd dat de gebeurtenis wordt gegenereerd, gebaseerd op de UTC-tijd van de provider. |
| id | tekenreeks | De unieke id voor de gebeurtenis. |
| gegevens | object | Gebeurtenisgegevens van de Event hub. |
| dataVersion | tekenreeks | De versie van het schema van het gegevensobject. De uitgever definieert de schemaversie. |
| metadataVersion | tekenreeks | De versie van het schema van de metagegevens van de gebeurtenis. Gebeurtenis raster definieert het schema van de eigenschappen op het hoogste niveau. Gebeurtenis raster bevat deze waarde. |

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| fileUrl | tekenreeks | Het pad naar het bestand vastleggen. |
| fileType | tekenreeks | Het type van het bestand vastleggen. |
| partitionId | tekenreeks | De shard-ID. |
| sizeInBytes | geheel getal | De bestandsgrootte. |
| eventCount | geheel getal | Het aantal gebeurtenissen in het bestand. |
| firstSequenceNumber | geheel getal | Het kleinste volgnummer uit de wachtrij. |
| lastSequenceNumber | geheel getal | Het laatste volgnummer uit de wachtrij. |
| firstEnqueueTime | tekenreeks | De eerste keer uit de wachtrij. |
| lastEnqueueTime | tekenreeks | De laatste keer uit de wachtrij. |

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot Azure gebeurtenis raster, [wat gebeurtenis raster is?](overview.md)
* Zie voor meer informatie over het maken van een abonnement op Azure gebeurtenis raster [gebeurtenis raster abonnement schema](subscription-creation-schema.md).
* Zie voor meer informatie over het verwerken van gebeurtenissen van event hubs [Stream big data in een datawarehouse](event-grid-event-hubs-integration.md).