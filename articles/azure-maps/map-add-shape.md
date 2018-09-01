---
title: Een vorm met Azure-kaarten toevoegen | Microsoft Docs
description: Een vorm toevoegen aan een Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 5315e7d45ef3af838f26422655cf6971af6f903e
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/01/2018
ms.locfileid: "43382547"
---
# <a name="add-a-shape-to-a-map"></a>Een vorm toevoegen aan een kaart

In dit artikel wordt beschreven hoe u een lijn-, een cirkel- en een veelhoek toevoegen aan de kaart. 

<a id="addALine"></a>

## <a name="add-a-line"></a>Een regel toevoegen

<iframe height='500' scrolling='no' title='Een regel toegevoegd aan een kaart' src='//codepen.io/azuremaps/embed/qomaKv/?height=534&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/qomaKv/'>een regel toegevoegd aan een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, is een regel gemaakt. Een regel is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van LineString met LineStringProperties als de functie-eigenschap. Gebruik `new atlas.data.Feature(new atlas.data.LineString())` maakt u een regel en de eigenschappen definiëren. 

Een lijnlaag is een matrix van regels. Maakt gebruik van het laatste blok van code [addLineStrings](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings) functie van de klasse kaart de lijnlaag toevoegen aan de kaart en definiëren van de eigenschappen van de regel-laag. Controleer de eigenschappen van de laag van een regel op [LinestringLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.linestringlayeroptions?view=azure-iot-typescript-latest).

<a id="addACircle"></a>

## <a name="add-a-circle"></a>Voeg een cirkel toe

<iframe height='500' scrolling='no' title='Voeg een cirkel toe aan een kaart' src='//codepen.io/azuremaps/embed/PRmzJX/?height=538&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>een cirkel toevoegen aan een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, wordt een cirkel gemaakt. Een cirkel wordt een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van [punt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) met [CircleProperties](https://docs.microsoft.com/javascript/api/azure-maps-control/models.circleproperties?view=azure-iot-typescript-latest) als de functie-eigenschap. Gebruik `new atlas.data.Feature(new atlas.data.Point())` maken van een cirkel en bijbehorende eigenschappen definiëren.

Een cirkel-laag is een matrix van cirkels. Maakt gebruik van het laatste blok van code [addCircle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcircles) functie van de klasse kaart in de laag van de cirkel toevoegen aan de kaart en het definiëren van de eigenschappen van de cirkel-laag. Controleer de eigenschappen van een laag van de cirkel op [CircleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.circlelayeroptions?view=azure-iot-typescript-latest).

<a id="addAPolygon"></a>

## <a name="add-a-polygon"></a>Toevoegen van een veelhoek
<iframe height='500' scrolling='no' title='Een veelhoek toevoegen aan een kaart ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>een veelhoek toevoegen aan een kaart </a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, is een veelhoek gemaakt. Een veelhoek is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van [veelhoek](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) met [PolygonProperties](https://docs.microsoft.com/javascript/api/azure-maps-control/models.polygonproperties?view=azure-iot-typescript-latest) als de functie-eigenschap. Gebruik `new atlas.data.Feature(new atlas.data.Polygon())` te maken van een polygoon definieert de eigenschappen ervan. Geordende coördinaten van het pad van de veelhoek in de constructor veelhoek bevatten.

Een polygoonlaag is een matrix van veelhoeken. Maakt gebruik van het laatste blok van code [addPolygons](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpolygons) functie van de klasse kaart de polygoonlaag toevoegen aan de kaart en de eigenschappen definiëren. Controleer de eigenschappen van een polygoonlaag op [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.polygonlayeroptions?view=azure-iot-typescript-latest). 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten:
* [Een aangepaste HTML toevoegen](./map-add-custom-html.md)
* [Zoekresultaten tonen](./map-search-location.md)


