---
title: Routebeschrijving met Azure Maps | Microsoft Docs
description: Hoe richtingen tussen twee locaties op een Javascript-kaart weergeven
author: jingjing-z
ms.author: jinzh
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: cb0d5d7239095b67235cc68233d9492377178362
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44295530"
---
# <a name="show-directions-from-a-to-b"></a>Routebeschrijving van A naar B 

Dit artikel ziet u hoe u een route indienen en de route weergeven op de kaart.

Er zijn twee manieren om dit te doen, één manier is door het opvragen van de [Azure kaarten-Route API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) via een service-module en de andere is door een [XMLHttpRequest](https://xhr.spec.whatwg.org/) naar de API. Beide worden hieronder besproken.

## <a name="querying-the-route-via-service-module"></a>Uitvoeren van query's de route via service-module

## <a name="understand-the-code"></a>De code begrijpen

<iframe height='500' scrolling='no' title='Routebeschrijving van A naar B op een kaart (Service-Module)' src='//codepen.io/azuremaps/embed/RBZbep/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/RBZbep/'>routebeschrijving van A naar B op een kaart (Service-Module)</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

De regel in het tweede codeblok waarmee een serviceclient wordt gemaakt.

Het derde codeblok initialiseert de [regel tekenreeks laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings) op de kaart.

De vierde blok van code maakt en voegt pincodes op de kaart om de begin- en eindpunt van de route vertegenwoordigen. U kunt zien [toevoegen van een pincode op de kaart](map-add-pin.md) voor instructies.

Maakt gebruik van het volgende codeblok [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) functie van de klasse kaart om in te stellen het selectiekader van de kaart op basis van de begin- en eindpunt van de route.

Het zesde codeblok wordt een route-query.

Het laatste blok van code de Azure Maps-service voor gebeurtenisroutering via query's de [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) methode voor het ophalen van een route tussen de begin- en doel het toegangspunt. Het antwoord wordt vervolgens geparseerd in de GeoJSON-notatie gebruiken de [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest#getgeojsonroutes) methode. Alle regels op de kaart om weer te geven van de route wordt toegevoegd. U kunt zien [toevoegen van een regel op de kaart](./map-add-shape.md#addALine) voor meer informatie.

## <a name="querying-the-route-via-xmlhttprequest"></a>De route via XMLHttpRequest uitvoeren van query 's

## <a name="understand-the-code"></a>De code begrijpen

<iframe height='500' scrolling='no' title='Routebeschrijving van A naar B op een kaart' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>routebeschrijving van A naar B op een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede codeblok maakt en voegt pincodes op de kaart om de begin- en eindpunt van de route vertegenwoordigen. U kunt zien [toevoegen van een pincode op de kaart](map-add-pin.md) voor instructies.

Maakt gebruik van de derde codeblok [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) functie van de klasse kaart om in te stellen het selectiekader van de kaart op basis van de begin- en eindpunt van de route.

De vierde blok van code verzendt een [XMLHttpRequest](https://xhr.spec.whatwg.org/) naar [Azure kaarten-Route API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections).

Het laatste blok van code parseert de binnenkomende respons. Voor een geslaagde respons verzamelt het de gegevens voor breedtegraad en lengtegraad voor elke beginpunt. Een matrix van regels wordt gemaakt door elke beginpunt verbinding te maken met de volgende beginpunt. Alle regels op de kaart om weer te geven van de route wordt toegevoegd. U kunt zien [toevoegen van een regel op de kaart](./map-add-shape.md#addALine) voor instructies.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt: 

* [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections)
* [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest#getgeojsonroutes)
* [Lijnlaag tekenreeks](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings)
* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds)
    * [addLinestrings](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)

Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten: 
* [Verkeer op de kaart weergeven](./map-show-traffic.md)
* [Interactie met de kaart - muisgebeurtenissen](./map-events.md)
