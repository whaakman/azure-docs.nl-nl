---
title: De afbeeldingslaag van een toevoegen aan Azure-kaarten | Microsoft Docs
description: De laag van een afbeelding toevoegen aan de Javascript-kaart
author: rbrundritt
ms.author: richbrun
ms.date: 12/3/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 8d0ad34496963c32c842033e81bdffb375a27412
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54214671"
---
# <a name="add-an-image-layer-to-a-map"></a>De afbeeldingslaag van een toevoegen aan een kaart

Dit artikel leest u hoe u een installatiekopie naar een vaste set coördinaten op de kaart kunt overlay. Er zijn veel scenario's waarin een installatiekopie van een bedrijfscontext op de kaart wordt uitgevoerd. Hier volgen enkele voorbeelden van het type afbeeldingen vaak overlay bekijken op kaarten;

* Installatiekopieën van drones.
* Building floorplans.
* Historische of andere speciale Kaartafbeeldingen.
* Blauwdrukken van taak sites.
* Installatiekopieën van weer werkt.

> [!TIP]
> Een [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) is een snelle een eenvoudige manier om een installatiekopie op een kaart bedekt. Echter, als de afbeelding te groot is, de browser mogelijk is het moeilijk om het te laden. In dit geval kunt u uw installatiekopie opgesplitst in tegels en deze worden geladen op de kaart als een [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest).

## <a name="add-an-image-layer"></a>Een afbeeldingslaag toevoegen

In dit voorbeeld laat zien hoe u een afbeelding van overlay een [toewijzing van één in New Jersey van 1922](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) op de kaart.

<br/>

<iframe height='500' scrolling='no' title='Eenvoudige installatiekopie laag' src='//codepen.io/azuremaps/embed/eQodRo/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/eQodRo/'>eenvoudige installatiekopie laag</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, een [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) wordt gemaakt door een URL wordt doorgegeven aan een afbeelding en coördinaten voor de hoeken in de indeling `[Top Left Corner, Top Right Corner, Bottom Right Conter, Bottom Left Corner]`.

## <a name="import-a-kml-ground-overlay"></a>Importeren van een KML grond overlay

In dit voorbeeld laat zien hoe overlay KML grond Overlay informatie als de laag van een installatiekopie op de kaart. KML grond overlays bieden Noord, Zuid, Oost, en west-coördinaten en een linksom draaien, waar als de afbeelding laag wordt verwacht coördinaten voor elke hoek van de installatiekopie dat. De overlay KML volledig is ontwikkeld in dit voorbeeld is van de Chartres cathedral en Source van [Wikimedia](https://commons.wikimedia.org/wiki/File:Chartres.svg/overlay.kml).

<br/>

<iframe height='500' scrolling='no' title='KML grond Overlay als afbeeldingslaag' src='//codepen.io/azuremaps/embed/EOJgpj/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/EOJgpj/'>KML gemalen Overlay als afbeeldingslaag</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De bovenstaande code wordt de statische `getCoordinatesFromEdges` functie van de [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) klasse voor het berekenen van de hoeken van de installatiekopie van het noorden, Zuid, Oost-west en rotatie gegevens uit de KML gemalen overlay.


## <a name="customize-an-image-layer"></a>De laag van een installatiekopie aanpassen

De installatiekopie-laag heeft veel opties voor stijl. Hier is een hulpprogramma voor ze kunt uitproberen.

<br/>

<iframe height='700' scrolling='no' title='Opties voor de laag afbeelding' src='//codepen.io/azuremaps/embed/RqOGzx/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/RqOGzx/'>Image opties voor Tegellaag</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [ImageLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.imagelayeroptions?view=azure-iot-typescript-latest)

Zie de volgende artikelen voor meer voorbeelden van code toevoegen aan uw kaarten:

> [!div class="nextstepaction"]
> [Een tegellaag toevoegen](./map-add-tile-layer.md)