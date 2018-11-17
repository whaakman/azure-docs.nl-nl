---
title: Zoekresultaten met Azure-kaarten weergeven | Microsoft Docs
description: Hoe u een zoekopdracht uitvoert met Azure Maps aanvragen en vervolgens de resultaten weergeven op een Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 11/15/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 031085b3048d0ffc92dd5a35b4054903088b4858
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51824333"
---
# <a name="show-search-results-on-the-map"></a>Zoekresultaten weergeven op de kaart

In dit artikel wordt beschreven hoe u zoeken naar de locatie van belang en de lijst met zoekresultaten weergeven op de kaart.

Er zijn twee manieren om te zoeken naar een locatie van belang zijn. Een manier is een servicemodule gebruiken om te maken van een zoekaanvraag. De andere manier is om te zoeken vragen via een [XMLHttpRequest](https://xhr.spec.whatwg.org/) naar [Azure Maps Fuzzy zoeken-API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy). Beide methoden worden hieronder besproken.

## <a name="make-a-search-request-via-service-module"></a>Een zoekopdracht indienen via de servicemodule

<iframe height='500' scrolling='no' title='Zoekresultaten weergeven op een kaart (Service-Module)' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>zoekresultaten weergeven op een kaart (Service-Module)</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code wordt een kaartobject en initialiseert de client-service. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede blok van code gebruikt de [getSearchFuzzy](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.search?view=azure-iot-typescript-latest#getsearchfuzzy-string--searchgetsearchfuzzyoptionalparams-) methode in de [servicemodule](https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=1). Hiermee kunt u een vrije indeling tekst te zoeken via de [Fuzzy zoekopdrachten rest-API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) om te zoeken naar nuttige plaats. Fuzzy zoeken-API kan elke combinatie van fuzzy invoer verwerken. Het antwoord van de service fuzzy zoeken wordt vervolgens geparseerd in de GeoJSON-notatie gebruiken de [getGeoJsonSearchResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonsearchresponse?view=azure-iot-typescript-latest#geojsonsearchresponse) methode. 

Het derde codeblok maakt u een gegevensbron object via de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse en zoekresultaten aan toe te voegen. Een [symbool laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) maakt gebruik van tekst of pictogrammen weergegeven op basis van een punt-gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als symbolen op de kaart.  Een symbool-laag wordt vervolgens gemaakt en de gegevensbron wordt toegevoegd aan het symbool-laag, die wordt vervolgens toegevoegd aan de kaart.

Het laatste blok van code Hiermee past u de grenzen van de camera voor de kaart met behulp van de kaart [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) eigenschap.

De zoekaanvraag, de gegevensbron en symbool laag en de camera-grenzen worden gemaakt en het instellen van de kaart [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen dat de resultaten worden weergegeven nadat de kaart volledig is geladen.


## <a name="make-a-search-request-via-xmlhttprequest"></a>Een aanvraag zoeken via XMLHttpRequest

<iframe height='500' scrolling='no' title='Zoekresultaten op een kaart weergeven' src='//codepen.io/azuremaps/embed/KQbaeM/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>zoekresultaten weergeven op een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede codeblok verzendt een [XMLHttpRequest](https://xhr.spec.whatwg.org/) naar [Azure Maps Fuzzy zoeken-API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) om te zoeken naar het punt van belang zijn. Een combinatie van fuzzy invoer kan worden verwerkt door de Fuzzy zoeken-API. 

Het derde codeblok antwoord van de zoekactie geparseerd en worden de resultaten opgeslagen in een matrix voor het berekenen van de grenzen. Deze retourneert vervolgens de lijst met zoekresultaten.

De vierde blok van code maakt een gegevensbron object via de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse en zoekresultaten aan toe te voegen. Een [symbool laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) maakt gebruik van tekst of pictogrammen weergegeven op basis van een punt-gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als symbolen op de kaart. Een symbool-laag wordt vervolgens gemaakt en de gegevensbron wordt toegevoegd aan het symbool-laag, die wordt vervolgens toegevoegd aan de kaart.

Het laatste blok van code maakt een [BoundingBox](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.boundingbox?view=azure-iot-typescript-latest) object met behulp van de matrix van de resultaten en klikt u vervolgens past u de grenzen van de camera voor de kaart met behulp van de kaart [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-). Deze vervolgens de pincodes resultaat weergegeven.

De zoekaanvraag, de gegevensbron en symbool laag en de grenzen van de camera, stelt u in van de kaart [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen dat de resultaten worden weergegeven nadat de kaart volledig is geladen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over **Fuzzy zoekopdrachten**:

> [!div class="nextstepaction"]
> [Azure kaarten-Fuzzy zoeken-API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Zie de volgende artikelen voor volledige codevoorbeelden:

> [!div class="nextstepaction"]
> [Gegevens ophalen uit een coördinaat](./map-get-information-from-coordinate.md)
<!-- Comment added to suppress false positive warning -->
> [!div class="nextstepaction"]
> [Routebeschrijving van A naar B](./map-route.md)
