---
title: Een symbolen toevoegen en markeringen met Azure Maps | Microsoft Docs
description: Symbolen en markeringen toevoegen aan een Javascript-kaart
author: walsehgal
ms.author: v-musehg
ms.date: 10/30/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 92d80d9a4d39a3f21539e5e6e498fc52df213a19
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50248809"
---
# <a name="add-symbols-and-markers-to-a-map"></a>Symbolen en markeringen toevoegen aan een kaart

Dit artikel leest u hoe symbolen en markeringen kunt toevoegen aan een kaart met behulp van een gegevensbron.

## <a name="add-a-symbol-marker"></a>Een symbool markering toevoegen

<iframe height='500' scrolling='no' title='Switch-pincode-locatie' src='//codepen.io/azuremaps/embed/ZqJjRP/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ZqJjRP/'>Switch pincode locatie</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste vereiste blok van de bovenstaande code wordt een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, object voor een gegevensbron wordt gemaakt met behulp van de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse. Een punt wordt vervolgens gemaakt en toegevoegd aan de gegevensbron. Een punt is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van [wijst](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest).

Het derde blok van code maakt een [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) en updates van het punt coördinaten van de muis klikt u op met behulp van de Shapeklasse [setCoordinates](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest#setcoordinates) methode.

Een [symbool laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) maakt gebruik van tekst of pictogrammen weergegeven op basis van een punt-gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als symbolen op de kaart.  De gegevensbron, de gebeurtenislistener klikken en het symbool-laag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) functie om ervoor te zorgen dat het punt wordt weergegeven nadat de kaart volledig is geladen.

## <a name="add-a-custom-symbol"></a>Een aangepaste symbool toevoegen

<iframe height='500' scrolling='no' title='HTML-gegevensbron' src='//codepen.io/azuremaps/embed/qJVgMx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/qJVgMx/'>HTML DataSource</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede codeblok wordt toegevoegd een [HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest) aan de map met de [markeringen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#markers) eigenschap van de [kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) klasse. De HtmlMarker wordt toegevoegd aan de kaart in de [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) functie om ervoor te zorgen dat deze wordt weergegeven nadat de kaart volledig is geladen.

## <a name="add-bubble-markers"></a>Bellendiagrammen toevoegen

<iframe height='500' scrolling='no' title='BubbleLayer DataSource' src='//codepen.io/azuremaps/embed/mzqaKB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/mzqaKB/'>BubbleLayer DataSource</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, een matrix van functies is gedefinieerd en een [MultiPoint](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.multipoint?view=azure-iot-typescript-latest) object wordt gemaakt. Object voor een gegevensbron wordt vervolgens gemaakt met behulp van de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse en de MultiPoint-object wordt toegevoegd aan de gegevensbron.

Een [Bel laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) wordt weergegeven op basis van een punt-gegevens die zijn ingepakt in de [gegevensbron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als cirkels op de kaart. Het laatste blok van code maakt een bel-laag en voegt deze toe aan de kaart. Controleer de eigenschappen van een laag Bel op [BubblerLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.bubblelayeroptions?view=azure-iot-typescript-latest).

De MultiPoint-object, de gegevensbron en de bel-laag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) functie om ervoor te zorgen dat de cirkel wordt weergegeven nadat de kaart volledig is geladen.

## <a name="add-bubble-markers-with-label"></a>Bellendiagrammen met het label toevoegen

<iframe height='500' scrolling='no' title='MultiLayer DataSource' src='//codepen.io/azuremaps/embed/rqbQXy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/rqbQXy/'>MultiLayer DataSource</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De bovenstaande code ziet u hoe om te visualiseren en labelgegevens op de kaart. Het eerste vereiste blok van de bovenstaande code wordt een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede blok van code, maakt een [wijst](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) object. Het maakt vervolgens een gegevensbron object via de [gegevensbron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse en voegt u het punt toe aan de gegevensbron.

Een [Bel laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) wordt weergegeven op basis van een punt-gegevens die zijn ingepakt in de [gegevensbron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als cirkels op de kaart. Het derde blok van code maakt een bel-laag en voegt deze toe aan de kaart. Controleer de eigenschappen van een laag Bel op [BubblerLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.bubblelayeroptions?view=azure-iot-typescript-latest).

Een [symbool laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) maakt gebruik van tekst of pictogrammen weergegeven op basis van een punt-gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als symbolen op de kaart. Het laatste blok van code maakt en voegt een laag symbool aan de kaart die het tekstlabel voor de belgrootte wordt weergegeven. Controleer de eigenschappen van een laag symbool op [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.symbollayeroptions?view=azure-iot-typescript-latest).

De gegevensbron en de lagen worden gemaakt en toegevoegd aan de kaart in de [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) functie om ervoor te zorgen dat gegevens worden weergegeven nadat de kaart volledig is geladen.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Zie de volgende artikelen voor meer voorbeelden van code toevoegen aan uw kaarten:

> [!div class="nextstepaction"]
> [Een pop-up toevoegen](./map-add-popup.md)

> [!div class="nextstepaction"]
> [Een vorm toevoegen](./map-add-shape.md)