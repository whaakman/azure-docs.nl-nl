---
title: Een HTML-markering toevoegen aan Azure Maps | Microsoft Docs
description: Een HTML-markering toevoegen aan de Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 1c812a77429e13ea39b2f4946043c13e10aaf097
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55993739"
---
# <a name="add-html-markers-to-the-map"></a>HTML-markeringen toevoegen aan de kaart

In dit artikel wordt beschreven hoe u een aangepaste HTML, zoals een afbeeldingsbestand toevoegen aan de kaart als een HTML-markering.

> [!NOTE]
> HTML-markeringen kunnen geen verbinding maken met gegevensbronnen. In plaats daarvan positie informatie rechtstreeks naar de markering is toegevoegd en de markering wordt toegevoegd aan de kaarten `markers` eigenschap waarvoor een [HtmlMarkerManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkermanager?view=azure-iot-typescript-latest).

> [!IMPORTANT]
> In tegenstelling tot de meeste lagen in het webbesturingselement voor Azure-kaarten die WebGL voor rendering gebruiken HTML-markeringen traditionele DOM-elementen voor rendering. Als zodanig het meer HTML-markeringen een pagina, de meer DOM-elementen die er zijn toegevoegd. Prestaties kunnen echter afnemen na het toevoegen van een paar honderd HTML-markeringen. Voor grotere gegevenssets kunt u de clustering van uw gegevens of met behulp van een laag symbool of bel op te geven.

## <a name="add-an-html-marker"></a>Een HTML-markering toevoegen

De klasse HtmlMarker heeft een standaardstijl. U kunt de markering aanpassen door het instellen van de opties voor tekst en kleur van de markering. De standaardstijl van de klasse HtmlMarker is een SVG-sjabloon met een tijdelijke aanduiding voor tekst en kleur. Stel de eigenschappen kleur en tekst in de opties HtmlMarker voor een snel kunt aanpassen. 

<br/>

<iframe height='500' scrolling='no' title='Een HTML-markering toevoegen aan een kaart' src='//codepen.io/azuremaps/embed/MVoeVw/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/MVoeVw/'>een HTML-markering toevoegen aan een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede codeblok wordt toegevoegd een [HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest) aan de map met de [markeringen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#markers) eigenschap van de [kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) klasse. De HtmlMarker wordt toegevoegd aan de kaart in de [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) functie om ervoor te zorgen dat deze wordt weergegeven nadat de kaart volledig is geladen.

## <a name="create-svg-templated-html-marker"></a>SVG-sjablonen HTML-markering maken

De standaardwaarde `htmlContent` van een HTML-markering is een SVG-sjabloon met plaats mappen `{color}` en `{text}` erin. U kunt maken van aangepaste SVG-tekenreeksen en deze dezelfde tijdelijke aanduidingen toevoegen aan uw SVG instellen dat de `color` en `text` opties van de markering deze tijdelijke aanduidingen in uw SVG bijwerken.

<br/>

<iframe height='500' scrolling='no' title='HTML-markering met aangepaste SVG-sjabloon' src='//codepen.io/azuremaps/embed/LXqMWx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/LXqMWx/'>HTML-markering met aangepaste SVG-sjabloon</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="add-a-css-styled-html-marker"></a>Een CSS-stijl HTML-markering toevoegen

Een van de voordelen van HTML-markeringen is dat er veel grote aanpassingen die kunnen worden bereikt met behulp van CSS. In dit voorbeeld wordt de inhoud van de HtmlMarker bestaat uit HTML en CSS die een animatie pincode die in de plaats en uitzendt komt maken.

<br/>

<iframe height='500' scrolling='no' title='HTML-gegevensbron' src='//codepen.io/azuremaps/embed/qJVgMx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/qJVgMx/'>HTML DataSource</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="draggable-html-markers"></a>Versleepbare HTML-markeringen

Dit voorbeeld laat zien hoe u een HTML-markering versleepbare. Ondersteuning voor HTML-markeringen `drag`, `dragstart` en `dragend` gebeurtenissen.

<br/>

<iframe height='500' scrolling='no' title='Versleepbare HTML-markering' src='//codepen.io/azuremaps/embed/wQZoEV/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/wQZoEV/'>versleepbare HTML-markering</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="add-mouse-events-to-html-markers"></a>Muisgebeurtenissen toevoegen aan HTML-markeringen

Deze voorbeelden laten zien hoe muis toevoegen en gebeurtenissen naar een HTML-markering.

<br/>

<iframe height='500' scrolling='no' title='Muisgebeurtenissen toe te voegen aan HTML-markeringen' src='//codepen.io/azuremaps/embed/RqOKRz/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/RqOKRz/'>muisgebeurtenissen toe te voegen aan HTML-markeringen</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HtmlMarkerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HtmlMarkerManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkermanager?view=azure-iot-typescript-latest)

Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten:

> [!div class="nextstepaction"]
> [Een symbool laag toevoegen](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Een bellendiagram laag toevoegen](./map-add-bubble-layer.md)