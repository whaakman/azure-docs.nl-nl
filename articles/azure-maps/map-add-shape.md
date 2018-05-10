---
title: Een vorm met Azure Maps toevoegen | Microsoft Docs
description: Het toevoegen van een vorm naar een Javascript-kaart
services: azure-maps
keywords: ''
author: jinzh-azureiot
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: codepen
ms.openlocfilehash: fb9ec0713d3db465cf835346465e70c4455b38ff
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="add-a-shape-to-a-map"></a>Een vorm toevoegen aan een kaart

In dit artikel leest u hoe een regel, een cirkel en een polygoon toevoegen aan de kaart. 

<a id="addALine"></a>

## <a name="add-a-line"></a>Een regel toevoegen

<iframe height='500' scrolling='no' title='Een regel toevoegen aan een kaart' src='//codepen.io/azuremaps/embed/qomaKv/?height=534&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/qomaKv/'>een regel toevoegen aan een kaart</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De eerste blok van code wordt in de code wordt een toewijzingsobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code wordt een regel gemaakt. Een regel is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-javascript/feature?view=azure-iot-typescript-latest) van LineString met LineStringProperties als de functie-eigenschap. Gebruik `new atlas.data.Feature(new atlas.data.LineString())` maakt u een regel en bijbehorende eigenschappen definiëren. 

Een regel laag is een matrix van regels. Maakt gebruik van het laatste blok van code [addLineStrings](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addlinestrings) functie van de kaart klasse de laag regel toevoegen aan de kaart en de eigenschappen van de laag regel definiëren. Controleer de eigenschappen van een laag van de regel op [LinestringLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/linestringlayeroptions?view=azure-iot-typescript-latest).

<a id="addACircle"></a>

## <a name="add-a-circle"></a>Toevoegen van een cirkel

<iframe height='500' scrolling='no' title='Een cirkel toevoegen aan een kaart' src='//codepen.io/azuremaps/embed/PRmzJX/?height=538&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>een cirkel toevoegen aan een kaart</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De eerste blok van code wordt in de code wordt een toewijzingsobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede codeblok, wordt een cirkel gemaakt. Een cirkel wordt een [functie](https://docs.microsoft.com/javascript/api/azure-maps-javascript/feature?view=azure-iot-typescript-latest) van [punt](https://docs.microsoft.com/javascript/api/azure-maps-javascript/point?view=azure-iot-typescript-latest) met [CircleProperties](https://docs.microsoft.com/javascript/api/azure-maps-javascript/circleproperties?view=azure-iot-typescript-latest) als de functie-eigenschap. Gebruik `new atlas.data.Feature(new atlas.data.Point())` maken van een cirkel en bijbehorende eigenschappen definiëren.

Een cirkel-laag is een raster vormen van stippen. Maakt gebruik van het laatste blok van code [addCircle](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addcircles) functie van de kaart klasse de cirkel-laag toevoegen aan de kaart en definiëren van de eigenschappen van de cirkel-laag. Controleer de eigenschappen van een cirkel-laag op [CircleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/circlelayeroptions?view=azure-iot-typescript-latest).

<a id="addAPolygon"></a>

## <a name="add-a-polygon"></a>Toevoegen van een polygoon
<iframe height='500' scrolling='no' title='Een polygoon toevoegen aan een kaart ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>een polygoon toevoegen aan een kaart </a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De eerste blok van code wordt in de code wordt een toewijzingsobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede codeblok, is een veelhoek gemaakt. Een veelhoek is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-javascript/feature?view=azure-iot-typescript-latest) van [veelhoek](https://docs.microsoft.com/javascript/api/azure-maps-javascript/polygon?view=azure-iot-typescript-latest) met [PolygonProperties](https://docs.microsoft.com/javascript/api/azure-maps-javascript/polygonproperties?view=azure-iot-typescript-latest) als de functie-eigenschap. Gebruik `new atlas.data.Feature(new atlas.data.Polygon())` voor het maken van een polygoon en bijbehorende eigenschappen definiëren. Geordende coördinaten van het pad van de veelhoek in de constructor veelhoek bieden.

Een laag veelhoek is een matrix van de veelhoek. Maakt gebruik van het laatste blok van code [addPolygons](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpolygons) functie van de kaart klasse de veelhoek laag toevoegen aan de kaart en bijbehorende eigenschappen definiëren. Controleer de eigenschappen van een laag veelhoek op [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/polygonlayeroptions?view=azure-iot-typescript-latest). 
