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
ms.openlocfilehash: 0753e4f2bee1259356f1c27c3b9967a914b27798
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2018
ms.locfileid: "52888631"
---
# <a name="add-a-shape-to-a-map"></a>Een vorm toevoegen aan een kaart

Dit artikel laat u het toevoegen van een [vorm](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) aan de kaart en update-eigenschappen van een bestaande vorm op de kaart.

<a id="addALine"></a>

## <a name="add-a-line"></a>Een regel toevoegen

<iframe height='500' scrolling='no' title='Een regel toegevoegd aan een kaart' src='//codepen.io/azuremaps/embed/qomaKv/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/qomaKv/'>een regel toegevoegd aan een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste vereiste blok van code in de bovenstaande code wordt een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, object voor een gegevensbron wordt gemaakt met behulp van de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse. Een [LineString](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.linestring?view=azure-iot-typescript-latest) object wordt gemaakt en toegevoegd aan de gegevensbron.

Een [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) renders regel objecten die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Het laatste blok van code wordt gemaakt en wordt de laag van een regel toegevoegd aan de kaart. Controleer de eigenschappen van de laag van een regel op [LineLayerOptions](/javascript/api/azure-maps-control/atlas.linestringlayeroptions?view=azure-iot-typescript-latest). De gegevensbron en de regel-laag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) functie om ervoor te zorgen dat de regel wordt weergegeven nadat de kaart volledig is geladen.

## <a name="customize-a-line-layer"></a>Een lijnlaag aanpassen

De regel van de verschillende opties voor stijl laag. Hier is een hulpprogramma voor ze kunt uitproberen.

<br/>

<iframe height='700' scrolling='no' title='Lijnopties laag' src='//codepen.io/azuremaps/embed/GwLrgb/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/GwLrgb/'>laag Regelopties</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

<a id="addACircle"></a>

## <a name="add-a-circle"></a>Voeg een cirkel toe

<iframe height='500' scrolling='no' title='Voeg een cirkel toe aan een kaart' src='//codepen.io/azuremaps/embed/PRmzJX/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>een cirkel toevoegen aan een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste vereiste blok van code in de bovenstaande code wordt een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, object voor een gegevensbron wordt gemaakt met behulp van de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse. Een cirkel wordt een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van [punt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) en heeft een `subType` eigenschap ingesteld op "cirkel" en een `radius` eigenschap waarde meters. Wanneer een punt-functie met een subType van cirkel wordt toegevoegd aan een gegevensbron, converteert het naar een circulaire veelhoek binnen de kaart.

Een [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) weergegeven van de gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) op de kaart. Het laatste blok van code wordt gemaakt en wordt een polygoonlaag toegevoegd aan de kaart. Controleer de eigenschappen van een polygoonlaag op [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). De gegevensbron en de polygoonlaag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) functie om ervoor te zorgen dat de cirkel wordt weergegeven nadat de kaart volledig is geladen.

<a id="addAPolygon"></a>

## <a name="add-a-polygon"></a>Toevoegen van een veelhoek

Er zijn twee verschillende manieren kunt u een veelhoek toevoegen aan de kaart. Beide worden beschreven in de volgende voorbeelden.

### <a name="use-polygon-layer"></a>Polygoonlaag gebruiken 

<iframe height='500' scrolling='no' title='Een veelhoek toevoegen aan een kaart ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>een veelhoek toevoegen aan een kaart </a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, object voor een gegevensbron wordt gemaakt met behulp van de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse. Een [veelhoek](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) is gemaakt op basis van een matrix van coördinaten en toegevoegd aan de gegevensbron. 

Een [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) weergegeven van de gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) op de kaart. Het laatste blok van code wordt gemaakt en wordt een polygoonlaag toegevoegd aan de kaart. Controleer de eigenschappen van een polygoonlaag op [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). De gegevensbron en de polygoonlaag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) functie om ervoor te zorgen dat de veelhoek wordt weergegeven nadat de kaart volledig is geladen.

### <a name="use-polygon-and-line-layer"></a>Polygoon- en laag gebruiken

<iframe height='500' scrolling='no' title='Polygoon- en laag toe te voegen veelhoek' src='//codepen.io/azuremaps/embed/aRyEPy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/aRyEPy/'>Polygoon- en laag toe te voegen veelhoek</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, object voor een gegevensbron wordt gemaakt met behulp van de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse. Een [veelhoek](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) is gemaakt op basis van een matrix van coördinaten en toegevoegd aan de gegevensbron. 

Een [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) weergegeven van de gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) op de kaart. Controleer de eigenschappen van een polygoonlaag op [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). Een [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) is een matrix van regels. Controleer de eigenschappen van de laag van een regel op [LineLayerOptions](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest). Het derde codeblok maakt polygoon- en lijnlagen.

Het laatste blok van code wordt de polygoon- en lijnlagen toegevoegd aan de kaart. De gegevensbron en de lagen worden gemaakt en toegevoegd aan de kaart in de [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) functie om ervoor te zorgen dat de veelhoek wordt weergegeven nadat de kaart volledig is geladen.

## <a name="customize-a-polygon-layer"></a>Een polygoonlaag aanpassen

De polygoonlaag heeft alleen een aantal opties voor stijl. Hier is een hulpprogramma voor ze kunt uitproberen.

<br/>

<iframe height='700' scrolling='no' title='LXvxpg' src='//codepen.io/azuremaps/embed/LXvxpg/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/LXvxpg/'>LXvxpg</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="update-a-shape"></a>Bijwerken van een vorm

Een Shapeklasse verpakt een [geometrie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.geometry?view=azure-iot-typescript-latest) of [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) en maakt het gemakkelijk om te werken en ze zelf onderhouden.
`new Shape(data: Feature<data.Geometry, any>)` vormt een shape-object en initialiseert ze met de opgegeven functie.

<br/>

<iframe height='500' scrolling='no' title='Eigenschappen van de vorm bijwerken' src='//codepen.io/azuremaps/embed/ZqMeQY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ZqMeQY/'>bijwerken van eigenschappen van de shape</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste vereiste blok van de bovenstaande code wordt een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Een punt is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van [wijst](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) de klasse. Het tweede codeblok initialiseert de radiuswaarde voor de schuifregelaar HTML-element en vervolgens en loopt van een point-object in een [vorm](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) klasse-object.

Het derde codeblok maakt u een functie die wordt de waarde van het HTML-bereik schuifregelaar-element en wijzigt de radiuswaarde met behulp van de Shapeklasse [addProperty](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest#addproperty) methode.

In het vierde blok van code, object voor een gegevensbron wordt gemaakt met behulp van de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse. Het punt wordt vervolgens toegevoegd aan de gegevensbron.

Een [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) weergegeven van de gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) op de kaart. Het derde blok van code maakt een polygoonlaag. Controleer de eigenschappen van een polygoonlaag op [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). De gegevensbron, de gebeurtenislistener klikken en de polygoonlaag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) functie om ervoor te zorgen dat het punt wordt weergegeven nadat de kaart volledig is geladen.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten:

> [!div class="nextstepaction"]
> [Een aangepaste HTML toevoegen](./map-add-custom-html.md)

> [!div class="nextstepaction"]
> [Zoekresultaten tonen](./map-search-location.md)
