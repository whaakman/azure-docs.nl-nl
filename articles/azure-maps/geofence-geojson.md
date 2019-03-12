---
title: Indeling van de Geofence GeoJSON-gegevens in Azure Maps | Microsoft Docs
description: Meer informatie over de indeling van de Geofence GeoJSON-gegevens in Azure-kaarten
author: walsehgal
ms.author: v-musehg
ms.date: 02/14/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 6f76eea365311cd6c10dd39054187bf3a07bd595
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57568988"
---
# <a name="geofencing-geojson-data"></a>Met Geofencing GeoJSON-gegevens

De Azure-kaarten [ophalen Geofence](https://docs.microsoft.com/rest/api/maps/spatial/getgeofencepreview) en [POST Geofence](https://docs.microsoft.com/rest/api/maps/spatial/postgeofencepreview) API kunt u de nabijheid van een coördinaat ten opzichte van een opgegeven geofence of set fences ophalen. In dit artikel wordt uitgelegd hoe het voorbereiden van de gegevens van de geofence die kunnen worden gebruikt in de Azure Maps ophalen en de POST-API.

De gegevens voor de geofence bevindt of een set van geofences wordt vertegenwoordigd door `Feature` Object en `FeatureCollection` van het Object in `GeoJSON` -indeling, die is gedefinieerd in [rfc7946](https://tools.ietf.org/html/rfc7946). Naast het volgende:

* De GeoJSON-objecttype mag een `Feature` Object of een `FeatureCollection` Object.
* Het type Geometry Object kan worden een `Point`, `MultiPoint`, `LineString`, `MultiLineString`, `Polygon`, `MultiPolygon`, en `GeometryCollection`.
* Alle eigenschappen van de functie moeten bevatten een `geometryId`, die wordt gebruikt voor het identificeren van de geofence bevindt.
* Met functie `Point`, `MultiPoint`, `LineString`, `MultiLineString` moet bevatten `radius` in de eigenschappen. `radius` waarde wordt gemeten in meters, de `radius` varieert van 1 tot 10000-waarde.
* Met functie `polygon` en `multipolygon` geometrie-type heeft geen een radius-eigenschap.
* `validityTime` een optionele eigenschap waarmee de gebruiker is ingesteld verlopen tijd en de geldigheid periode voor de gegevens van de geofence bevindt. Indien niet opgegeven, worden de gegevens nooit verloopt en is altijd geldig.
* De `expiredTime` wordt de vervaldatum en -tijd van geofencing gegevens. Als de waarde van `userTime` in de aanvraag is hoger dan deze waarde, de bijbehorende geofence gegevens wordt beschouwd als verlopen gegevens en is geen query wordt uitgevoerd. Waarop, de geometryId van deze gegevens worden opgenomen in geofence `expiredGeofenceGeometryId` matrix in het antwoord van de geofence bevindt.
* De `validityPeriod` is een lijst van de geldigheidsduur tijdsduur van de geofence bevindt. Als de waarde van `userTime` in de aanvraag valt buiten de geldigheidsperiode, de bijbehorende gegevens van de geofence wordt beschouwd als ongeldig en wordt niet worden opgevraagd. De geometryId van deze gegevens van de geofence is opgenomen in `invalidPeriodGeofenceGeometryId` matrix in de reactie van de geofence bevindt. De volgende tabel ziet u de eigenschappen van validityPeriod element.

| Name | Type | Vereist  | Description |
| :------------ |:------------: |:---------------:| :-----|
| startTime | Datum en tijd  | true | Het begin van datum-tijd van de geldigheid periode. |
| endTime   | Datum en tijd  | true |  De einddatum /-tijd van de geldigheid periode. |
| recurrenceType | string | false |   Het type terugkeerpatroon van de periode. De waarde kan zijn `Daily`, `Weekly`, `Monthly`, of `Yearly`. Standaardwaarde is `Daily`.|
| businessDayOnly | Booleaans | false |  Geeft aan of de gegevens alleen geldig tijdens de werkdagen is. Standaardwaarde is `false`.|


* Alle coördinaat waarden worden weergegeven als [breedtegraad, lengtegraad] gedefinieerd in `WGS84`.
* Voor elke functie die bevat `MultiPoint`, `MultiLineString`, `MultiPolygon` , of `GeometryCollection`, de eigenschappen worden toegepast op alle elementen. Bijvoorbeeld: Alle punten in `MultiPoint` dezelfde radius gebruikt om te vormen een meerdere cirkel geofence bevindt.
* In-punt cirkel scenario wordt een cirkel geometry kan worden gebruikt, met behulp van een `Point` geometrieobject met eigenschappen die zijn opgesteld [GeoJSON uitbreiden geometrie](https://docs.microsoft.com/azure/azure-maps/extend-geojson).      

Hieronder volgt een voorbeeld-aanvraagtekst voor een geofence wordt weergegeven als een cirkel geofence geometrie in `GeoJSON` met behulp van een centraal punt en een radius. De geldige periode van de gegevens van de geofence wordt gestart van 2018-10-22, 9: 00 op 17: 00 uur, dagelijks, met uitzondering van het weekend herhaald. `expiredTime` Geeft aan dat deze gegevens van de geofence wordt beschouwd als verlopen, als `userTime` in de aanvraag wordt later zijn dan `2019-01-01`.  

```json
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "geometryId" : "1",
        "subType": "Circle",
        "radius": 500,
        "validityTime": 
        {
            "expiredTime": "2019-01-01T00:00:00",
            "validityPeriod": [
                {
                    "startTime": "2018-10-22T09:00:00",
                    "endTime": "2018-10-22T17:00:00",
                    "recurrenceType": "Daily",
                    "recurrenceFrequency": 1,
                    "businessDayOnly": true
                }
            ]
        }
    }
}
```