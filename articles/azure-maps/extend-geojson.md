---
title: GeoJSON geometrie van Azure Maps uitbreiden | Microsoft Docs
description: Meer informatie over het uitbreiden van GeoJSON geometrie in Azure-kaarten
author: sataneja
ms.author: sataneja
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: be3c31951c4721a861f9239c5220419dec11b6bf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60799149"
---
# <a name="extending-geojson-geometries"></a>GeoJSON geometrie uitbreiden

Azure Maps biedt een lijst met krachtige API's om te zoeken binnen/langs geografische functies.
Deze API's te standaardiseren op [GeoJSON-specificatie] [ 1] voor weergave van de geografische functies (bijvoorbeeld: status grenzen, routes).  

De [GeoJSON-specificatie] [ 1] biedt alleen ondersteuning voor de volgende geometrie:

* GeometryCollection
* LineString
* MultiLineString
* MultiPoint
* MultiPolygon
* Punt
* Polygon

Sommige Azure kaarten-API's (bijvoorbeeld: [Zoeken binnen geometrie](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)) accepteren geometrie, zoals "Cirkel", die geen deel uitmaakt van de [GeoJSON-specificatie][1].

In dit artikel biedt een gedetailleerde beschrijving van hoe Azure Maps breidt de [GeoJSON-specificatie] [ 1] bepaalde geometrie vertegenwoordigt.

### <a name="circle"></a>Cirkel

De `Circle` geometrie wordt niet ondersteund door de [GeoJSON-specificatie][1]. We gebruiken de `GeoJSON Feature` object om weer te geven van een cirkel.

Een `Circle` geometry weergegeven met behulp van de `GeoJSON Feature` object __moet__ bevatten het volgende:

1. Centreren
   >Midden van de cirkel wordt weergegeven met behulp van een `GeoJSON Point` type.

2. Radius
   >Van de cirkel `radius` wordt weergegeven met behulp van `GeoJSON Feature`van eigenschappen. De radiuswaarde heeft _meters_ en moet van het type `double`.

3. SubType
   >Moet ook de cirkel geometry bevatten het `subType` eigenschap. Deze eigenschap moet een deel van de `GeoJSON Feature`van eigenschappen en de waarde moet _Circle_


#### <a name="example"></a>Voorbeeld

Hier ziet u hoe u weergeven in een cirkel gecentreerd op (breedtegraad: 47.639754, lengtegraad:-122.126986) met een radius gelijk is aan 100 meters, met behulp van een `GeoJSON Feature` object:

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

[1]: https://tools.ietf.org/html/rfc7946
