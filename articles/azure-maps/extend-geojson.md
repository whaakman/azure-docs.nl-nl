---
title: Uitgebreide geojson-geometrie in Azure Maps | Microsoft Docs
description: Meer informatie over het uitbreiden van geojson-geometrie in Azure Maps
author: sataneja
ms.author: sataneja
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 52325248d21a5d5112c9a7f9497c3e03fdf102a4
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68881988"
---
# <a name="extended-geojson-geometries"></a>Uitgebreide geojson-geometrie

Azure Maps biedt een lijst met krachtige Api's om in/naast geografische functies te zoeken.
Deze Api's worden gestandaardiseerd op de [geojson spec][1] voor de geografische functies (bijvoorbeeld: status grenzen, routes).  

De [geojson spec][1] ondersteunt alleen de volgende geometrieën:

* GeometryCollection
* Lines Tring
* Multi line String
* MultiPoint
* MultiPolygon
* Spreek
* Polygoon

Sommige Azure Maps-Api's (bijvoorbeeld: [Zoeken binnen geometrie](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)) Accepteer geometrie als ' cirkel ' die geen deel uitmaken van de geojson- [specificatie][1].

Dit artikel bevat een gedetailleerde uitleg over hoe Azure Maps de geojson- [specificatie][1] uitbreidt om bepaalde geometrieën weer te geven.

## <a name="circle"></a>Middencirkel

De `Circle` geometrie wordt niet ondersteund door de [geojson-specificatie][1]. We gebruiken een `GeoJSON Point Feature` object om een cirkel aan te duiden.

Een `Circle` geometrie die wordt weer `GeoJSON Feature` gegeven met behulp van het object __moet__ het volgende bevatten:

- Centreren

    Het midden van de cirkel wordt weer gegeven `GeoJSON Point` met behulp van een-object.

- Radius

    De cirkel `radius` wordt weer gegeven met `GeoJSON Feature`behulp van de eigenschappen. De RADIUS-waarde is in meters en moet van het `double`type zijn.

- SubType

    De geometrie van de cirkel moet ook `subType` de eigenschap bevatten. Deze eigenschap moet een deel van de `GeoJSON Feature`eigenschappen zijn en de waarde moet een _cirkel_ zijn

#### <a name="example"></a>Voorbeeld

Hier ziet u hoe een cirkel gecentreerd op (Latitude: 47,639754, lengte graad:-122,126986) met een straal gelijk aan 100 meters, `GeoJSON Feature` met behulp van een object:

```json            
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "subType": "Circle",
        "radius": 100
    }
}          
```

## <a name="rectangle"></a>Rechthoek

De `Rectangle` geometrie wordt niet ondersteund door de [geojson-specificatie][1]. We gebruiken een `GeoJSON Polygon Feature` object om een rechthoek weer te geven. De uitbrei ding van de rechthoek wordt voornamelijk gebruikt door de module teken hulpprogramma's van de Web-SDK.

Een `Rectangle` geometrie die wordt weer `GeoJSON Polygon Feature` gegeven met behulp van het object __moet__ het volgende bevatten:

- Hoeken

    De hoeken van de rechthoek worden weer gegeven met behulp `GeoJSON Polygon` van de coördinaten van een object. Er moeten vijf coördinaten zijn: één voor elke hoek en een vijfde coördinaat die hetzelfde is als de 1e om de polygoon ring te sluiten. Deze coördinaten worden geacht te zijn uitgelijnd en gedraaid, zoals gewenst door de ontwikkelaar.

- SubType

    De geometrie van de rechthoek moet ook `subType` de eigenschap bevatten. Deze eigenschap moet een deel van de `GeoJSON Feature`eigenschappen zijn en de waarde moet een _rechthoek_ zijn

### <a name="example"></a>Voorbeeld

```json
{
    "type": "Feature",
    "geometry": {
        "type": "Polygon",
        "coordinates": [[[5,25],[14,25],[14,29],[5,29],[5,25]]]
    },
    "properties": {
        "subType": "Rectangle"
    }
}

```
## <a name="next-steps"></a>Volgende stappen

Meer informatie over geojson-gegevens in Azure Maps:

> [!div class="nextstepaction"]
> [Geojson-indeling geofence](geofence-geojson.md)

Bekijk de woorden lijst met algemene technische termen die zijn gekoppeld aan Azure Maps en locatie intelligence-toepassingen:

> [!div class="nextstepaction"]
> [Azure Maps woordenlijst](glossary.md)

[1]: https://tools.ietf.org/html/rfc7946
