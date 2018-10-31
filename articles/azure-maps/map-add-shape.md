---
title: Een vorm met Azure-kaarten toevoegen | Microsoft Docs
description: Een vorm toevoegen aan een Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 10/30/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 801cbd6367e0843e43121b3582971a437984e863
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50247705"
---
# <a name="add-a-shape-to-a-map"></a>Een vorm toevoegen aan een kaart

Dit artikel laat u het toevoegen van een [vorm](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) aan de kaart en update-eigenschappen van een bestaande vorm op de kaart.

<a id="addALine"></a>

## <a name="add-a-line"></a>Een regel toevoegen

<iframe height='500' scrolling='no' title='Een regel toegevoegd aan een kaart' src='//codepen.io/azuremaps/embed/qomaKv/?height=534&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/qomaKv/'>een regel toegevoegd aan een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste vereiste blok van code in de bovenstaande code wordt een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, object voor een gegevensbron wordt gemaakt met behulp van de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse. Een regel is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van LineString. Vervolgens wordt gemaakt en loopt van een regel in de [vorm](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) object met behulp van klasse `new atlas.Shape(new atlas.data.Feature((new atlas.data.LineString()))` en voegt deze toe aan de gegevensbron.

Een [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) renders regel objecten die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Het laatste blok van code wordt gemaakt en wordt de laag van een regel toegevoegd aan de kaart. Controleer de eigenschappen van de laag van een regel op [LinestringLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.linestringlayeroptions?view=azure-iot-typescript-latest). De gegevensbron en de regel-laag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) functie om ervoor te zorgen dat de regel wordt weergegeven nadat de kaart volledig is geladen.

<a id="addACircle"></a>

## <a name="add-a-circle"></a>Voeg een cirkel toe

<iframe height='500' scrolling='no' title='Voeg een cirkel toe aan een kaart' src='//codepen.io/azuremaps/embed/PRmzJX/?height=538&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>een cirkel toevoegen aan een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste vereiste blok van code in de bovenstaande code wordt een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, object voor een gegevensbron wordt gemaakt met behulp van de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse. Een cirkel wordt een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van [punt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest). Vervolgens wordt gemaakt en loopt van een cirkel in de [vorm](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) object met behulp van klasse `new atlas.Shape(new atlas.data.Feature(new atlas.data.Point()))` en voegt deze toe aan de gegevensbron.

Een [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) weergegeven van de gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) op de kaart. Het laatste blok van code wordt gemaakt en wordt een polygoonlaag toegevoegd aan de kaart. Controleer de eigenschappen van een polygoonlaag op [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.polygonlayeroptions?view=azure-iot-typescript-latest). De gegevensbron en de polygoonlaag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) functie om ervoor te zorgen dat de cirkel wordt weergegeven nadat de kaart volledig is geladen.

<a id="addAPolygon"></a>

## <a name="add-a-polygon"></a>Toevoegen van een veelhoek

Er zijn twee verschillende manieren kunt u een veelhoek toevoegen aan de kaart. Beide worden beschreven in de volgende voorbeelden.

### <a name="use-polygon-layer"></a>Polygoonlaag gebruiken 

<iframe height='500' scrolling='no' title='Een veelhoek toevoegen aan een kaart ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>een veelhoek toevoegen aan een kaart </a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, object voor een gegevensbron wordt gemaakt met behulp van de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse. Een [veelhoek](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van de [veelhoek](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) klasse. `new atlas.Shape(new atlas.data.Feature(new atlas.data.Polygon()))` wordt gebruikt om u te maken van een veelhoek met geordende coördinaten verpakt in een [vorm](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) klasse-object. Het object shape wordt vervolgens toegevoegd aan de gegevensbron.

Een [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) weergegeven van de gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) op de kaart. Het laatste blok van code wordt gemaakt en wordt een polygoonlaag toegevoegd aan de kaart. Controleer de eigenschappen van een polygoonlaag op [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.polygonlayeroptions?view=azure-iot-typescript-latest). De gegevensbron en de polygoonlaag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) functie om ervoor te zorgen dat de veelhoek wordt weergegeven nadat de kaart volledig is geladen.

### <a name="use-polygon-and-line-layer"></a>Polygoon- en laag gebruiken

<iframe height='500' scrolling='no' title='Polygoon- en laag toe te voegen veelhoek' src='//codepen.io/azuremaps/embed/aRyEPy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/aRyEPy/'>Polygoon- en laag toe te voegen veelhoek</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, object voor een gegevensbron wordt gemaakt met behulp van de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse. Een [veelhoek](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van [veelhoek](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest). `new atlas.Shape(new atlas.data.Feature(new atlas.data.Polygon()))` wordt gebruikt om u te maken van een veelhoek met geordende coördinaten verpakt in een [vorm](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) klasse-object. Het object shape wordt vervolgens toegevoegd aan de gegevensbron.

Een [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) weergegeven van de gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) op de kaart. Controleer de eigenschappen van een polygoonlaag op [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.polygonlayeroptions?view=azure-iot-typescript-latest). Een [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) is een matrix van regels. Controleer de eigenschappen van de laag van een regel op [LinestringLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.linestringlayeroptions?view=azure-iot-typescript-latest). Het derde codeblok maakt polygoon- en lijnlagen.

Het laatste blok van code wordt de polygoon- en lijnlagen toegevoegd aan de kaart. De gegevensbron en de lagen worden gemaakt en toegevoegd aan de kaart in de [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) functie om ervoor te zorgen dat de veelhoek wordt weergegeven nadat de kaart volledig is geladen.

## <a name="update-a-shape"></a>Bijwerken van een vorm

Een Shapeklasse verpakt een [geometrie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.geometry?view=azure-iot-typescript-latest) of [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) en maakt het gemakkelijk om te werken en ze zelf onderhouden.
`new Shape(data: Feature<data.Geometry, any>)` vormt een shape-object en initialiseert ze met de opgegeven functie.

<br>

<iframe height='500' scrolling='no' title='Eigenschappen van de vorm bijwerken' src='//codepen.io/azuremaps/embed/ZqMeQY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ZqMeQY/'>bijwerken van eigenschappen van de shape</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste vereiste blok van de bovenstaande code wordt een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Een punt is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van [wijst](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) de klasse. Het tweede codeblok initialiseert de radiuswaarde voor de schuifregelaar HTML-element en vervolgens en loopt van een point-object in een [vorm](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) klasse-object.

Het derde codeblok maakt u een functie die wordt de waarde van het HTML-bereik schuifregelaar-element en wijzigt de radiuswaarde met behulp van de Shapeklasse [addProperty](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest#addproperty) methode.

In het vierde blok van code, object voor een gegevensbron wordt gemaakt met behulp van de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse. Het punt wordt vervolgens toegevoegd aan de gegevensbron.

Een [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) weergegeven van de gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) op de kaart. Het derde blok van code maakt een polygoonlaag. Controleer de eigenschappen van een polygoonlaag op [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.polygonlayeroptions?view=azure-iot-typescript-latest). De gegevensbron, de gebeurtenislistener klikken en de polygoonlaag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) functie om ervoor te zorgen dat het punt wordt weergegeven nadat de kaart volledig is geladen.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten:

> [!div class="nextstepaction"]
> [Een aangepaste HTML toevoegen](./map-add-custom-html.md)

> [!div class="nextstepaction"]
> [Zoekresultaten tonen](./map-search-location.md)
