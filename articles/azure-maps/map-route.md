---
title: Routebeschrijving met Azure Maps | Microsoft Docs
description: Hoe richtingen tussen twee locaties op een Javascript-kaart weergeven
author: jingjing-z
ms.author: jinzh
ms.date: 11/15/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: 0fd8acf00f2afd0690fa84172c5dd2e242554b84
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54212662"
---
# <a name="show-directions-from-a-to-b"></a>Routebeschrijving van A naar B

Dit artikel ziet u hoe u een route indienen en de route weergeven op de kaart.

Er zijn twee manieren om dit te doen. De eerste manier is het query de [Azure kaarten-Route API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) via een servicemodule. De tweede manier is om een [XMLHttpRequest](https://xhr.spec.whatwg.org/) naar de API. Beide methoden worden hieronder besproken.

## <a name="query-the-route-via-service-module"></a>Query uitvoeren op de route via service-module

<iframe height='500' scrolling='no' title='Routebeschrijving van A naar B op een kaart (Service-Module)' src='//codepen.io/azuremaps/embed/RBZbep/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/RBZbep/'>routebeschrijving van A naar B op een kaart (Service-Module)</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

De regel in het tweede codeblok waarmee een client-service wordt gemaakt.

De derde maakt en voegt een [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) object aan de kaart.

 Een regel is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van LineString. Een [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) renders regel objecten die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als regels op de kaart. De vierde blok van code maakt en een lijnlaag toevoegen aan de kaart. Controleer de eigenschappen van de laag van een regel op [LinestringLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.deprecated.linestringlayeroptions?view=azure-iot-typescript-latest).

Een [symbool laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) maakt gebruik van tekst of pictogrammen weergegeven op basis van een punt-gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als symbolen op de kaart. Het vijfde codeblok wordt gemaakt en een laag symbool toevoegen aan de kaart.

Het zesde codeblok maakt begin- en [punten](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) object en voegt deze toe aan het dataSource-object.

Het zevende codeblok Hiermee stelt u de grenzen van de kaart met behulp van de kaart [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) eigenschap.

Het laatste blok van code query naar de service voor gebeurtenisroutering op Azure-kaarten, die deel uitmaakt van de [servicemodule](https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=1). De [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) methode wordt gebruikt om op te halen van een route tussen de begin- en eindpunten. Het antwoord wordt vervolgens geparseerd in de GeoJSON-notatie gebruiken de [getGeoJsonRouteDirectionsResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest#getgeojsonroutes) methode. Deze wordt vervolgens het antwoord weergegeven als een route op de kaart. Zie voor meer informatie over het toevoegen van een regel op de kaart [toevoegen van een regel op de kaart](./map-add-shape.md#addALine).

De route-query, gegevensbron, symbool en lagen van de regel en de camera-grenzen worden gemaakt en het instellen van de kaart [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen dat de resultaten worden weergegeven nadat de kaart volledig is geladen.

## <a name="query-the-route-via-xmlhttprequest"></a>De route via XMLHttpRequest query

<iframe height='500' scrolling='no' title='Routebeschrijving van A naar B op een kaart' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>routebeschrijving van A naar B op een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste vereiste blok van code wordt een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede codeblok maakt en voegt een [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) object aan de kaart.

Het derde codeblok wordt gemaakt van de begin- en de herstelpunten voor de route en voegt deze toe aan de gegevensbron. U kunt zien [toevoegen van een pincode op de kaart](map-add-pin.md) voor instructies over het gebruik van [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins).

 Een [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) renders regel objecten die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als regels op de kaart. Het vierde codeblok wordt gemaakt en wordt de laag van een regel toegevoegd aan de kaart. Controleer de eigenschappen van de laag van een regel op [LineLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest).

Een [symbool laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) maakt gebruik van tekst of pictogrammen weergegeven op basis van een punt-gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als symbolen op de kaart. Het vijfde codeblok wordt gemaakt en een laag symbool toevoegen aan de kaart. Controleer de eigenschappen van een laag symbool op [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest).

Hiermee maakt u het volgende codeblok `SouthWest` en `NorthEast` punten van de begin- en doel-punten en stelt de grenzen van de kaart met behulp van de kaart [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) eigenschap.

Het laatste blok van code verzendt een [XMLHttpRequest](https://xhr.spec.whatwg.org/) naar [Azure kaarten-Route API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). Vervolgens de binnenkomende respons worden geparseerd. En voor een geslaagde respons verzamelt u de gegevens voor breedtegraad en lengtegraad voor elke route-updatepunt, en wordt een matrix van regels wordt gemaakt door verbinding te maken die punten. Vervolgens wordt toegevoegd deze regels op de gegevensbron om de route op de kaart weer te geven. U kunt zien [toevoegen van een regel op de kaart](./map-add-shape.md#addALine) voor instructies.

De route-query, gegevensbron, symbool en lagen van de regel en de camera-grenzen worden gemaakt en het instellen van de kaart [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen dat de resultaten worden weergegeven nadat de kaart volledig is geladen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Zie de volgende artikelen voor volledige codevoorbeelden:

> [!div class="nextstepaction"]
> [Verkeer op de kaart weergeven](./map-show-traffic.md)

> [!div class="nextstepaction"]
> [Interactie met de kaart - muisgebeurtenissen](./map-events.md)
