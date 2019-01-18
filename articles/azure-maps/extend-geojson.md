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
ms.openlocfilehash: 319f9cba23d088553f361b6a0d648bbde94e0743
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38968558"
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
* Veelhoek

Sommige Azure kaarten-API's (bijvoorbeeld: [zoeken binnen geometrie](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)) accepteren geometrie, zoals "Cirkel", die niet deel uitmaakt van de [GeoJSON-specificatie][1].

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
