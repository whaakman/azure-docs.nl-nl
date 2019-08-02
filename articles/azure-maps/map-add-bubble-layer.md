---
title: Een tekenlaag toevoegen aan Azure Maps | Microsoft Docs
description: Een Bubble laag toevoegen aan de Java script-kaart
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 516e4f35c88ae9c0e2d63e8a4ee40eb57c05ac29
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639037"
---
# <a name="add-a-bubble-layer-to-a-map"></a>Een tekenlaag aan een kaart toevoegen

Dit artikel laat u zien hoe u punt gegevens van een gegevens bron kunt weer geven als een Bubble laag op een kaart. Met bellen lagen worden punten weer gegeven als cirkels op de kaart met een harde pixel RADIUS. 

> [!TIP]
> Met bellen lagen worden standaard de coördinaten van alle geometrieën in een gegevens bron weer gegeven. Als u de laag zo wilt beperken dat alleen de functies van de punt geometrie `filter` worden weer gegeven, stelt `['==', ['geometry-type'], 'Point']` u `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` de eigenschap van de laag in of als u ook multi point-functies wilt toevoegen.

## <a name="add-a-bubble-layer"></a>Een bubbellaag toevoegen

<iframe height='500' scrolling='no' title='BubbleLayer gegevens bron' src='//codepen.io/azuremaps/embed/mzqaKB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/mzqaKB/'>BubbleLayer data source</a> by Azure Maps<a href='https://codepen.io/azuremaps'>@azuremaps</a>() op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code maakt het eerste code blok een kaart object. U kunt [een overzicht maken](./map-create.md) voor instructies.

In het tweede code blok wordt een matrix van [punt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) objecten gedefinieerd en toegevoegd aan het [Data Source](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) -object.

Met een [bellen slaag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) worden gegevens op basis van punten weer gegeven in de [gegevens bron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als cirkels op de kaart. Met het laatste code blok wordt een Bubble laag gemaakt en toegevoegd aan de kaart. Zie Eigenschappen van een Bubble Layer op [BubbleLayerOptions](/javascript/api/azure-maps-control/atlas.bubblelayeroptions).

De matrix van punt objecten, de gegevens bron en de laag Bubble worden gemaakt en toegevoegd aan de kaart in de [event listener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) -functie om ervoor te zorgen dat de cirkel wordt weer gegeven nadat de kaart volledig is geladen.

## <a name="show-labels-with-a-bubble-layer"></a>Labels weer geven met een Bubble Layer

<iframe height='500' scrolling='no' title='Gegevens bron met meerdere lagen' src='//codepen.io/azuremaps/embed/rqbQXy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de <a href='https://codepen.io/azuremaps/pen/rqbQXy/'>gegevens bron</a> met meerdere lagen pen door<a href='https://codepen.io/azuremaps'>@azuremaps</a>Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De bovenstaande code laat zien hoe u gegevens op de kaart kunt visualiseren en labelen. Het eerste code blok hierboven maakt een kaart object. U kunt [een overzicht maken](./map-create.md) voor instructies.

Het tweede code blok maakt een object [Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) . Vervolgens wordt een gegevens bron object gemaakt met behulp van de [gegevens bron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse en wordt het punt aan de gegevens bron toegevoegd.

Met een [bellen slaag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) worden gegevens op basis van punten weer gegeven in de [gegevens bron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als cirkels op de kaart. Het derde code blok maakt een Bubble laag en voegt deze toe aan de kaart. Zie Eigenschappen van een Bubble Layer op [BubbleLayerOptions](/javascript/api/azure-maps-control/atlas.bubblelayeroptions).

Een [symbool laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) maakt gebruik van tekst of pictogrammen voor het weer geven van op punten gebaseerde gegevens die in de [gegevens bron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) zijn verpakt als symbolen op de kaart. Met het laatste code blok wordt een symbool laag gemaakt en toegevoegd aan de kaart die het tekst label voor de bellen rendert. Zie Eigenschappen van een Symbol-laag op [SymbolLayerOptions](/javascript/api/azure-maps-control/atlas.symbollayeroptions).

De gegevens bron en de lagen worden gemaakt en toegevoegd aan de kaart in de [event listener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) -functie om ervoor te zorgen dat gegevens worden weer gegeven nadat de kaart volledig is geladen.

## <a name="customize-a-bubble-layer"></a>Een Bubble laag aanpassen

De Bubble laag heeft slechts enkele opmaak opties. Hier volgt een hulp programma om het uit te proberen.

<br/>

<iframe height='700' scrolling='no' title='Opties voor Bubble Layers' src='//codepen.io/azuremaps/embed/eQxbGm/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de <a href='https://codepen.io/azuremaps/pen/eQxbGm/'>laag opties</a> van de pen op Azure Maps<a href='https://codepen.io/azuremaps'>@azuremaps</a>() op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [BubbleLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [BubbleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.bubblelayeroptions?view=azure-iot-typescript-latest)

Raadpleeg de volgende artikelen voor meer code voorbeelden om toe te voegen aan uw kaarten:

> [!div class="nextstepaction"]
> [Een symbool laag toevoegen](map-add-pin.md)

> [!div class="nextstepaction"]
> [Op gegevens gebaseerde stijl expressies gebruiken](data-driven-style-expressions-web-sdk.md)