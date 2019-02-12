---
title: Reageren op gebeurtenissen van Azure Maps met behulp van Event Grid | Microsoft Docs
description: Informatie over het reageren op gebeurtenissen van Azure Maps met behulp van Event Grid.
author: walsehgal
ms.author: v-musehg
ms.date: 02/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: a70011b934398ac4e7f74bb67013e93bb5e86e4e
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56008508"
---
# <a name="react-to-azure-maps-events-by-using-event-grid"></a>Reageren op gebeurtenissen van Azure Maps met behulp van Event Grid 

Azure Maps kan worden geïntegreerd met Azure Event Grid, zodat u kunt meldingen van gebeurtenissen verzenden naar andere services en -trigger downstream-processen. Het doel van dit artikel is om u te helpen u bij het configureren van uw zakelijke toepassingen om te luisteren naar gebeurtenissen van Azure Maps, zodat u op kritieke gebeurtenissen op een betrouwbare, schaalbare en veilige manier reageren kunt. Bijvoorbeeld, een toepassing bouwt die meerdere bewerkingen zoals het bijwerken van een database, het maken van een ticket en het leveren van een e-mailmelding telkens wanneer een apparaat een geofence voert wordt uitgevoerd.

Azure Event Grid is een volledig beheerde service voor gebeurtenisroutering die gebruikmaakt van een publish-subscribe-model. Event Grid heeft ingebouwde ondersteuning voor Azure-services zoals [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview) en [Azure Logic Apps](https://docs.microsoft.com/azure/azure-functions/functions-overview), en waarschuwingen kunt leveren aan niet-Azure-services met behulp van webhooks. Zie voor een volledige lijst van de gebeurtenis-handlers die ondersteuning biedt voor Event Grid, [een inleiding tot Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview).


![Azure Event Grid functionele model](./media/azure-maps-event-grid-integration/azure-event-grid-functional-model.png)


## <a name="azure-maps-events-types"></a>Azure Maps gebeurtenistypen

Maakt gebruik van Event grid [gebeurtenisabonnementen](https://docs.microsoft.com/azure/event-grid/concepts#event-subscriptions) gebeurtenis om berichten te routeren voor abonnees. Een Azure kaarten-account verzendt de volgende typen gebeurtenissen: 

| Gebeurtenistype | Description |
| ---------- | ----------- |
| Microsoft.Maps.GeofenceEntered | Treedt op wanneer coördinaten ontvangen zijn verplaatst van buiten een opgegeven geofence naar binnen |
| Microsoft.Maps.GeofenceExited | Treedt op wanneer coördinaten ontvangen zijn verplaatst van binnen een bepaalde geofence naar buiten |
| Microsoft.Maps.GeofenceResult | Deze gebeurtenis treedt op wanneer een query met geofencing een resultaat, ongeacht de status retourneert |

## <a name="event-schema"></a>Gebeurtenisschema

Het volgende voorbeeld show-schema voor GeofenceResult

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

## <a name="tips-for-consuming-events"></a>Tips voor het gebruik van gebeurtenissen

Toepassingen die werken met Azure Maps geofence gebeurtenissen moeten volgen enkele aanbevolen procedures:

* Meerdere abonnementen kunnen worden geconfigureerd om gebeurtenissen routeren naar de dezelfde gebeurtenis-handler. Het is belangrijk dat u niet wordt ervan uitgegaan dat er gebeurtenissen van een bepaalde bron zijn. Controleer altijd het onderwerp van het bericht om ervoor te zorgen dat het afkomstig is van de bron die u verwacht.
* Berichten kunnen niet de juiste volgorde of na een vertraging. Gebruik de `X-Correlation-id` veld in de antwoordkop om te begrijpen als uw gegevens over objecten bijgewerkt is.
* Wanneer en POST Geofence-API wordt aangeroepen met de modusparameter ingesteld op `EnterAndExit`, een gebeurtenis Enter of afsluiten is gegenereerd voor elke geometrie in de geofence waarvoor de status van de vorige Geofence API-aanroep is gewijzigd.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het gebruik van geofencing voor beheerbewerkingen op een site bouwen, Zie:

> [!div class="nextstepaction"] 
> [Een geofence instellen met behulp van Azure Maps](tutorial-geofence.md)