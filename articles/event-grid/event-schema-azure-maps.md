---
title: Gebeurtenisschema in het Azure Event Grid Azure Maps
description: Beschrijft de eigenschappen en het schema is opgegeven voor de Azure-kaarten gebeurtenissen met Azure Event Grid
services: event-grid
author: walsehgal
ms.service: event-grid
ms.topic: reference
ms.date: 02/08/2019
ms.author: v-musehg
ms.openlocfilehash: 74a3674e632f8dc3f0755bc2ad48376708c7966f
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56008382"
---
# <a name="azure-event-grid-event-schema-for-azure-maps"></a>Azure Event Grid-gebeurtenisschema voor Azure-kaarten

Dit artikel bevat de eigenschappen en het schema voor gebeurtenissen van Azure Maps. Zie voor een inleiding tot gebeurtenisschema's, [Azure Event Grid-gebeurtenisschema](https://docs.microsoft.com/azure/event-grid/event-schema).

## <a name="available-event-types"></a>Typen van de gebeurtenis berichten beschikbaar

Een Azure kaarten-account verzendt de volgende typen gebeurtenissen:

| Gebeurtenistype | Description |
| ---------- | ----------- |
| Microsoft.Maps.GeofenceEntered | Treedt op wanneer coördinaten ontvangen zijn verplaatst van buiten een opgegeven geofence naar binnen |
| Microsoft.Maps.GeofenceExited | Treedt op wanneer coördinaten ontvangen zijn verplaatst van binnen een bepaalde geofence naar buiten |
| Microsoft.Maps.GeofenceResult | Deze gebeurtenis treedt op wanneer een query met geofencing een resultaat, ongeacht de status retourneert |

## <a name="event-examples"></a>Voorbeelden van gebeurtenis

Het volgende voorbeeld ziet u het schema van een **GeofenceEntered** gebeurtenis

```JSON
{   
   "id":"7f8446e2-1ac7-4234-8425-303726ea3981", 
   "topic":"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Maps/accounts/{accountName}", 
   "subject":"/spatial/geofence/udid/{udid}/id/{eventId}", 
   "data":{   
      "geometries":[   
         {   
            "deviceId":"device_1", 
            "udId":"1a13b444-4acf-32ab-ce4e-9ca4af20b169", 
            "geometryId":"2", 
            "distance":-999.0, 
            "nearestLat":47.618786, 
            "nearestLon":-122.132151 
         } 
      ], 
      "expiredGeofenceGeometryId":[   
      ], 
      "invalidPeriodGeofenceGeometryId":[   
      ] 
   }, 
   "eventType":"Microsoft.Maps.GeofenceEntered", 
   "eventTime":"2018-11-08T00:54:17.6408601Z", 
   "metadataVersion":"1", 
   "dataVersion":"1.0" 
}
```

Het volgende voorbeeld weergeven-schema voor **GeofenceResult** 

```JSON
{   
   "id":"451675de-a67d-4929-876c-5c2bf0b2c000", 
   "topic":"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Maps/accounts/{accountName}", 
   "subject":"/spatial/geofence/udid/{udid}/id/{eventId}", 
   "data":{   
      "geometries":[   
         {   
            "deviceId":"device_1", 
            "udId":"1a13b444-4acf-32ab-ce4e-9ca4af20b169", 
            "geometryId":"1", 
            "distance":999.0, 
            "nearestLat":47.609833, 
            "nearestLon":-122.148274 
         }, 
         {   
            "deviceId":"device_1", 
            "udId":"1a13b444-4acf-32ab-ce4e-9ca4af20b169", 
            "geometryId":"2", 
            "distance":999.0, 
            "nearestLat":47.621954, 
            "nearestLon":-122.131841 
         } 
      ], 
      "expiredGeofenceGeometryId":[   
      ], 
      "invalidPeriodGeofenceGeometryId":[   
      ] 
   }, 
   "eventType":"Microsoft.Maps.GeofenceResult", 
   "eventTime":"2018-11-08T00:52:08.0954283Z", 
   "metadataVersion":"1", 
   "dataVersion":"1.0" 
}
```

## <a name="event-properties"></a>Eigenschappen van gebeurtenis

Een gebeurtenis heeft de volgende gegevens op het hoogste niveau:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| onderwerp | string | Volledige resource-pad naar de bron van de gebeurtenis. Dit veld is niet beschrijfbaar. Event Grid biedt deze waarde. |
| onderwerp | string | Uitgever gedefinieerde pad naar het onderwerp van de gebeurtenis. |
| eventType | string | Een van de geregistreerde gebeurtenis-typen voor de bron van deze gebeurtenis. |
| eventTime | string | Het moment waarop dat de gebeurtenis is gegenereerd, is afhankelijk van de UTC-tijd van de provider. |
| id | string | De unieke id voor de gebeurtenis. |
| gegevens | object | Met Geofencing gebeurtenisgegevens. |
| dataVersion | string | De schemaversie van het gegevensobject. De uitgever definieert de schemaversie. |
| metadataVersion | string | De schemaversie van de metagegevens van de gebeurtenis. Event Grid definieert het schema van de eigenschappen op het hoogste niveau. Event Grid biedt deze waarde. |

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| apiCategory | string | De categorie van de API van de gebeurtenis. |
| apiName | string | API-naam van de gebeurtenis. |
| Problemen met | object | Geeft een lijst van fouten die optreden tijdens de verwerking. Als er problemen zijn geretourneerd, wordt er geen geometrie geretourneerd met het antwoord. |
| responseCode | getal | HTTP-responscode |
| geometrie | object | Een lijst met de omheining geometrie die de coördinaat bevatten positie of overlappen de searchBuffer rond de positie. |

Het foutobject wordt geretourneerd wanneer een fout in de kaarten-API optreedt. Het foutobject heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| error | ErrorDetails |Dit object wordt geretourneerd wanneer er een fout optreedt in de kaarten-API  |

Het object ErrorDetails wordt geretourneerd wanneer een fout in de kaarten-API optreedt. De ErrorDetails of het object heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| code | string | De HTTP-statuscode. |
| message | string | Indien beschikbaar, een mens leesbaar beschrijving van de fout. |
| innererror | InnerError | Indien beschikbaar, een object met service-specifieke informatie over de fout. |

De InnerError is een object met service-specifieke informatie over de fout. Het object InnerError heeft de volgende eigenschappen: 

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| code | string | Het foutbericht. |

Het object geometrie, geeft een lijst van geometrie-id's van de geofences die ten opzichte van de tijd in gebruikersmodus in de aanvraag is verlopen. De geometrie-object bevat de geometrie-items met de volgende eigenschappen: 

| Eigenschap | Type | Description |
|:-------- |:---- |:----------- |
| apparaat-id | string | ID van het apparaat. |
| afstand | string | <p>De afstand tussen de coördinaat naar de dichtstbijzijnde rand van de geofence bevindt. Positief betekent dat de coördinaat is buiten de geofence bevindt. Als de coördinaat buiten de geofence bevindt, maar hoger dan de waarde van searchBuffer weg van de dichtstbijzijnde rand van de geofence bevindt is, is de waarde 999. Negatieve betekent de coördinaat binnen de geofence bevindt. Als de coördinaat binnen de veelhoek, maar hoger dan de waarde van searchBuffer weg van de dichtstbijzijnde geofencing rand is, is de waarde-999. Een waarde van 999 betekent dat er meer vertrouwen is de coördinaat is ook buiten de geofence bevindt. Een waarde van-999 betekent dat er meer vertrouwen is de coördinaat is ook binnen de geofence bevindt.<p> |
| geometryid |string | De unieke id identificeert de geometrie geofence bevindt. |
| nearestlat | getal | De breedtegraad van het dichtstbijzijnde punt van de geometrie. |
| nearestlon | getal | De lengtegraad van het dichtstbijzijnde punt van de geometrie. |
| udId | string | De unieke id van gebruiker uploaden service geretourneerd tijdens het uploaden van een geofence bevindt. Wordt niet worden opgenomen in geofencing post-API. |

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| expiredGeofenceGeometryId | string[] | Lijsten van de geometrie-ID van de geofence bevindt ten opzichte van de tijd in gebruikersmodus in de aanvraag is verlopen. |
| geometrie | geometrie] |Een lijst met de omheining geometrie die de coördinaat bevatten positie of overlappen de searchBuffer rond de positie. |
| invalidPeriodGeofenceGeometryId | string[]  | Lijsten van de geometrie-ID van de geofence die zich in een ongeldige periode ten opzichte van de tijd in gebruikersmodus in de aanvraag. |
| isEventPublished | booleaans | Waar wordt of ten minste één gebeurtenis wordt gepubliceerd op de abonnee Azure Maps gebeurtenis, de waarde false als er geen gebeurtenis gepubliceerd naar de gebeurtenisabonnee Azure Maps. |

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot Azure Event Grid, [wat is Event Grid?](overview.md)
* Zie voor meer informatie over het maken van een Azure Event Grid-abonnement [Event Grid-abonnementsschema](subscription-creation-schema.md).