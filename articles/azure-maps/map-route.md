---
title: Routebeschrijving met Azure Maps | Microsoft Docs
description: Hoe richtingen tussen twee locaties op een Javascript-kaart weergeven
author: jingjing-z
ms.author: jinzh
ms.date: 3/7/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: 786880c5fa919fce5ed60d011211e6d7348f7260
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57570059"
---
# <a name="show-directions-from-a-to-b"></a>Routebeschrijving van A naar B

Dit artikel ziet u hoe u een route indienen en de route weergeven op de kaart.

Er zijn twee manieren om dit te doen. De eerste manier is het query de [Azure kaarten-Route API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) via een servicemodule. De tweede manier is het gebruikmaken van de [API ophalen](https://fetch.spec.whatwg.org/) om een zoekactie die naar de [Azure kaarten-Route API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). Beide methoden worden hieronder besproken.

## <a name="query-the-route-via-service-module"></a>Query uitvoeren op de route via service-module

<iframe height='500' scrolling='no' title='Routebeschrijving van A naar B op een kaart (Service-Module)' src='//codepen.io/azuremaps/embed/RBZbep/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/RBZbep/'>routebeschrijving van A naar B op een kaart (Service-Module)</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code vormt een kaartobject en het verificatiemechanisme voor de sleutel voor het abonnement wordt ingesteld. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede blok van code maakt een **SubscriptionKeyCredentialPolicy** om HTTP-aanvragen naar Azure kaarten met de abonnementssleutel te verifiëren. De **atlas.service.MapsURL.newPipeline()** wordt in de **SubscriptionKeyCredential** beleid en maakt een [pijplijn](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-iot-typescript-latest) exemplaar. De **routeURL** vertegenwoordigt een URL naar Azure Maps [Route](https://docs.microsoft.com/rest/api/maps/route) bewerkingen.

Het derde codeblok maakt en voegt een [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) object aan de kaart.

De vierde blok van code maakt begin- en [punten](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) objecten en voegt deze toe aan het dataSource-object.

Een regel is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van LineString. Een [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) renders regel objecten die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als regels op de kaart. De vierde blok van code maakt en een lijnlaag toevoegen aan de kaart. Controleer de eigenschappen van de laag van een regel op [LinestringLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest).

Een [symbool laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) maakt gebruik van tekst of pictogrammen weergegeven op basis van een punt-gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als symbolen op de kaart. Het vijfde codeblok wordt gemaakt en een laag symbool toevoegen aan de kaart.

Het zesde codeblok query naar de service voor gebeurtenisroutering op Azure-kaarten, die deel uitmaakt van de [servicemodule](https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=2). De [calculateRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl?view=azure-iot-typescript-latest#methods) -methode van de RouteURL wordt gebruikt om op te halen van een route tussen de begin- en eindpunten. Een verzameling van de functie GeoJSON uit het antwoord wordt vervolgens opgehaald met behulp van de **geojson.getFeatures()** methode en is toegevoegd aan de gegevensbron. Deze wordt vervolgens het antwoord weergegeven als een route op de kaart. Zie voor meer informatie over het toevoegen van een regel op de kaart [toevoegen van een regel op de kaart](./map-add-shape.md#addALine).

Het laatste blok van code stelt de grenzen van de kaart met behulp van de kaart [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) eigenschap.

De route-query, gegevensbron, symbool en lagen van de regel en de camera-grenzen worden gemaakt en het instellen van de kaart [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen dat de resultaten worden weergegeven nadat de kaart volledig is geladen.

## <a name="query-the-route-via-fetch-api"></a>Query uitvoeren op de route via API ophalen

<iframe height='500' scrolling='no' title='Routebeschrijving van A naar B op een kaart' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>routebeschrijving van A naar B op een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code vormt een kaartobject en het verificatiemechanisme voor de sleutel voor het abonnement wordt ingesteld. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede codeblok maakt en voegt een [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) object aan de kaart.

Het derde codeblok wordt gemaakt van de begin- en de herstelpunten voor de route en voegt deze toe aan de gegevensbron. U kunt zien [toevoegen van een pincode op de kaart](map-add-pin.md) voor instructies over het gebruik van [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest).

Een [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) renders regel objecten die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als regels op de kaart. Het vierde codeblok wordt gemaakt en wordt de laag van een regel toegevoegd aan de kaart. Controleer de eigenschappen van de laag van een regel op [LineLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest).

Een [symbool laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) maakt gebruik van tekst of pictogrammen weergegeven op basis van een punt-gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als symbolen op de kaart. Het vijfde codeblok wordt gemaakt en een laag symbool toevoegen aan de kaart. Controleer de eigenschappen van een laag symbool op [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest).

Hiermee maakt u het volgende codeblok `SouthWest` en `NorthEast` punten van de begin- en doel-punten en stelt de grenzen van de kaart met behulp van de kaart [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) eigenschap.

Maakt gebruik van het laatste blok van code de [API ophalen](https://fetch.spec.whatwg.org/) om een zoekactie die naar de [Azure kaarten-Route API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). Vervolgens de binnenkomende respons worden geparseerd. En voor een geslaagde respons verzamelt u de gegevens voor breedtegraad en lengtegraad voor elke route-updatepunt, en wordt een matrix van regels wordt gemaakt door verbinding te maken die punten. Vervolgens wordt toegevoegd deze regels op de gegevensbron om de route op de kaart weer te geven. U kunt zien [toevoegen van een regel op de kaart](./map-add-shape.md#addALine) voor instructies.

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
