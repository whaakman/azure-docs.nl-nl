---
title: Een tegel laag toevoegen aan Azure Maps | Microsoft Docs
description: Een laag voor een tegel toevoegen aan de Java script-kaart
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: e288e03b9e2c02ba963595f192dea7225c6d5762
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638986"
---
# <a name="add-a-tile-layer-to-a-map"></a>Een tegel laag aan een kaart toevoegen

In dit artikel wordt beschreven hoe u een tegel laag kunt bedekken op de kaart. Met tegel lagen kunt u afbeeldingen boven op Azure Maps basis kaart tegels plaatsen. Meer informatie over Azure Maps tegel systeem vindt u in de documentatie over het [Zoom niveau en het tegel raster](zoom-levels-and-tile-grid.md) .

Een tegel laag die wordt geladen in tegels van een server. Deze installatie kopieën kunnen vooraf worden weer gegeven en opgeslagen, zoals elke andere installatie kopie op een server met behulp van een naamgevings Conventie die de tegel laag begrijpt, of een dynamische service die de afbeeldingen op de vlucht genereert. Er worden drie verschillende naamgevings conventies voor tegel Services ondersteund door Azure Maps klasse [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest) ; 

* X, Y, zoom notatie: gebaseerd op het zoom niveau, x is de kolom en Y de rijpositie van de tegel in het tegel raster.
* Quadkey notatie: combi natie x, y en zoom informatie in een enkele teken reeks waarde die een unieke id voor een tegel is.
* Omsluitende Box-coördinaten kunnen worden gebruikt om een afbeelding op te geven in de indeling `{west},{south},{east},{north}` die wordt gebruikt door [Web Mapping Services (WMS)](https://www.opengeospatial.org/standards/wms).

> [!TIP]
> Een [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest) is een uitstekende manier om grote gegevens sets op de kaart te visualiseren. U kunt niet alleen een tegel laag genereren op basis van een afbeelding, maar u kunt ook vector gegevens weer geven als een tegel laag. Door vector gegevens als een tegel laag te renderen, hoeft het kaart besturings element alleen de tegels te laden die veel kleiner kunnen zijn dan de vector gegevens die ze vertegenwoordigen. Deze techniek wordt gebruikt door veel die miljoenen rijen met gegevens op de kaart moeten weer geven.

De tegel-URL die wordt door gegeven aan een tegel laag moet een HTTP/HTTPS-URL zijn naar een TileJSON-resource of een tegel-URL-sjabloon die gebruikmaakt van de volgende para meters: 

* `{x}`-X-positie van de tegel. Ook nodig `{y}` en `{z}`.
* `{y}`-Y-positie van de tegel. Ook nodig `{x}` en `{z}`.
* `{z}`-Zoom niveau van de tegel. Ook nodig `{x}` en `{y}`.
* `{quadkey}`-Tegel quadkey-id gebaseerd op de naam Conventie voor Bing Maps-tegel systeem.
* `{bbox-epsg-3857}`-Een teken reeks voor selectie kader met de `{west},{south},{east},{north}` indeling in het EPSG 3857 Spatial Reference System.
* `{subdomain}`-Een tijdelijke aanduiding waar de opgegeven subdomein waarden worden toegevoegd.

## <a name="add-a-tile-layer"></a>Een titellaag toevoegen

 Dit voor beeld laat zien hoe u een tegel laag maakt die verwijst naar een set tegels die gebruikmaken van het systeem x, y en zoomen. De bron van deze tegel laag is een weers radar-overlay van de [Iowa Environment Mesonet van de Iowa State University](https://mesonet.agron.iastate.edu/ogc/).

<br/>

<iframe height='500' scrolling='no' title='Tegel laag met X, Y en Z' src='//codepen.io/azuremaps/embed/BGEQjG/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bekijk de laag met de tegels van de pen <a href='https://codepen.io/azuremaps/pen/BGEQjG/'>met X, Y en Z</a> per Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code maakt het eerste code blok een kaart object. U kunt [een overzicht maken](./map-create.md) voor instructies.

In het tweede code blok wordt een [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest) gemaakt door een opgemaakte URL door te geven aan een tegel service, de tegel grootte en een dekking om deze semi-transparant te maken. Wanneer u de laag van de tegel toevoegt aan de kaart, wordt deze toegevoegd onder `labels` de laag zodat de labels nog steeds zichtbaar zijn.

## <a name="customize-a-tile-layer"></a>Een laag voor een tegel aanpassen

De laag van de tegel heeft slechts een aantal opties voor opmaak. Hier volgt een hulp programma om het uit te proberen.

<br/>

<iframe height='700' scrolling='no' title='Opties voor de laag van de tegel' src='//codepen.io/azuremaps/embed/xQeRWX/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de <a href='https://codepen.io/azuremaps/pen/xQeRWX/'>laag opties</a> voor pen-tegels<a href='https://codepen.io/azuremaps'>@azuremaps</a>per Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [TileLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.tilelayeroptions?view=azure-iot-typescript-latest)

Raadpleeg de volgende artikelen voor meer code voorbeelden om toe te voegen aan uw kaarten:

> [!div class="nextstepaction"]
> [Een afbeelding slaag toevoegen](./map-add-image-layer.md)
