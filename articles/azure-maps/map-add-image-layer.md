---
title: Een afbeelding slaag toevoegen aan Azure Maps | Microsoft Docs
description: Een afbeelding slaag toevoegen aan de Java script-kaart
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 7ea0f37e307196af4b27fd3f8fb1aa0d42443dfa
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638729"
---
# <a name="add-an-image-layer-to-a-map"></a>Een afbeelding slaag toevoegen aan een kaart

Dit artikel laat u zien hoe u een afbeelding kunt bedekken naar een vaste set coördinaten op de kaart. Er zijn veel scenario's waarin het bedekken van een afbeelding op de kaart is voltooid. Hier volgen enkele voor beelden van het type afbeeldingen dat vaak op kaarten wordt overlapt.

* Installatie kopieën die zijn vastgelegd vanuit drones.
* Floorplans bouwen.
* Historische of andere gespecialiseerde kaart afbeeldingen.
* Blauw drukken van taak sites.
* Weers radar-afbeeldingen.

> [!TIP]
> Een [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) is een snelle manier om een afbeelding op een kaart te bedekken. Als de afbeelding echter groot is, kan het moeilijk worden om deze te laden. In dit geval kunt u overwegen om uw afbeelding in tegels te verdelen en ze te laden in de kaart als een [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest).

## <a name="add-an-image-layer"></a>Een afbeeldingslaag toevoegen

Dit voor beeld laat zien hoe u een afbeelding van een [kaart van Newark New Jersey](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) kunt bedekken vanaf 1922 op de kaart.

<br/>

<iframe height='500' scrolling='no' title='Laag met eenvoudige afbeeldingen' src='//codepen.io/azuremaps/embed/eQodRo/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bekijk de <a href='https://codepen.io/azuremaps/pen/eQodRo/'>eenvoudige afbeelding slaag</a> van de pen door<a href='https://codepen.io/azuremaps'>@azuremaps</a>Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code maakt het eerste code blok een kaart object. U kunt [een overzicht maken](./map-create.md) voor instructies.

In het tweede code blok wordt een [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) gemaakt door een URL door te geven aan een afbeelding en een coördinaten voor de vier hoeken in de `[Top Left Corner, Top Right Corner, Bottom Right Corner, Bottom Left Corner]`notatie.

## <a name="import-a-kml-ground-overlay"></a>Een KML-wegdek-overlay importeren

In dit voor beeld ziet u hoe u de KML-bedekkings gegevens als een afbeelding slaag op de kaart kunt bedekken. KML-bedekkingen bieden Noord-, Zuid-, Oost-en West-coördinaten en een rotatie linksom, waarbij de afbeelding slaag coördinaten voor elke hoek van de afbeelding verwacht. De KML-vlak bedekking in dit voor beeld is van de Chartres-Cathedral en gesourceeerd van [Wikimedia](https://commons.wikimedia.org/wiki/File:Chartres.svg/overlay.kml).

<br/>

<iframe height='500' scrolling='no' title='KML-oppervlak overlay als afbeelding slaag' src='//codepen.io/azuremaps/embed/EOJgpj/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bekijk de KML-bedekking van de pen <a href='https://codepen.io/azuremaps/pen/EOJgpj/'>als afbeelding slaag</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De bovenstaande code maakt gebruik van `getCoordinatesFromEdges` de statische functie van de klasse [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) om de vier hoeken van de afbeelding te berekenen op basis van de Noord-, Zuid-, Oost-, West-en rotatie gegevens van de KML-wegdek bedekking.


## <a name="customize-an-image-layer"></a>Een afbeelding slaag aanpassen

De afbeelding slaag heeft veel stijl opties. Hier volgt een hulp programma om het uit te proberen.

<br/>

<iframe height='700' scrolling='no' title='Opties voor afbeelding slaags' src='//codepen.io/azuremaps/embed/RqOGzx/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de <a href='https://codepen.io/azuremaps/pen/RqOGzx/'>laag opties</a> van de pen-afbeelding<a href='https://codepen.io/azuremaps'>@azuremaps</a>per Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [ImageLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.imagelayeroptions?view=azure-iot-typescript-latest)

Raadpleeg de volgende artikelen voor meer code voorbeelden om toe te voegen aan uw kaarten:

> [!div class="nextstepaction"]
> [Een laag voor een tegel toevoegen](./map-add-tile-layer.md)