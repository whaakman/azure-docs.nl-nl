---
title: GeoJSON geometrie Azure Maps uitbreiden | Microsoft Docs
description: Meer informatie over het GeoJSON geometrie Azure Maps uitbreiden
author: sataneja
ms.author: sataneja
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 2cc0e29615ad4fc19040055d847435a9dffa9c95
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654920"
---
# <a name="extending-geojson-geometries"></a>GeoJSON geometrie uitbreiden

Azure Maps bevat een lijst van krachtige API's om te zoeken binnen/langs geografische functies.
Deze API's kunnen worden gestandaardiseerd op [GeoJSON spec] [ 1] voor het voorstellen van de geografische functies (bijvoorbeeld: status grenzen, routes).  

De [GeoJSON spec] [ 1] biedt alleen ondersteuning voor de volgende geometrie:

* GeometryCollection
* LineString
* MultiLineString
* MultiPoint
* MultiPolygon
* beheerpunt
* veelhoek

Sommige Azure kaarten-API's (bijvoorbeeld: [zoeken binnen de geometrie](https://docs.microsoft.com/en-us/rest/api/maps/search/postsearchinsidegeometry)) accepteren geometrie zoals 'Cirkel', die geen deel uit van de [GeoJSON spec][1].

In dit artikel wordt een gedetailleerde beschrijving van hoe Azure Maps breidt de [GeoJSON spec] [ 1] bepaalde geometrie vertegenwoordigt.

### <a name="circle"></a>cirkel

De `Circle` geometrie wordt niet ondersteund door de [GeoJSON spec][1]. We gebruiken de `GeoJSON Feature` object vertegenwoordigt een cirkel.

Een `Circle` geometrie weergegeven met behulp van de `GeoJSON Feature` object __moet__ bevatten het volgende:

1. Centreren
   >Midden van de cirkel wordt weergegeven met een `GeoJSON Point` type.

2. Radius
   >De cirkel `radius` wordt vertegenwoordigd met `GeoJSON Feature`van eigenschappen. De radiuswaarde is in _meters_ en moet van het type `double`.

3. SubType
   >Moet ook de geometrie cirkel bevatten de `subType` eigenschap. Deze eigenschap moet een deel van de `GeoJSON Feature`van eigenschappen en de waarde moet _cirkel_


#### <a name="example"></a>Voorbeeld

Hier ziet u hoe u een cirkel gecentreerd op je vertegenwoordigen (breedtegraad: 47.639754, lengtegraad:-122.126986) met een radius-gelijk zijn aan 100 meter, met behulp van een `GeoJSON Feature` object:

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
