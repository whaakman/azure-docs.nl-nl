---
title: Zoekresultaten met Azure-kaarten weergeven | Microsoft Docs
description: Hoe u een zoekopdracht uitvoert met Azure Maps aanvragen en vervolgens de resultaten weergeven op een Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 3/7/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 8ae6c8a20a05df723d3f6b394e0639f218896a85
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57845134"
---
# <a name="show-search-results-on-the-map"></a>Zoekresultaten weergeven op de kaart

In dit artikel wordt beschreven hoe u zoeken naar de locatie van belang en de lijst met zoekresultaten weergeven op de kaart.

Er zijn twee manieren om te zoeken naar een locatie van belang zijn. Een manier is een servicemodule gebruiken om te maken van een zoekaanvraag. De andere manier is om te maken van een zoekopdracht naar [Azure Maps Fuzzy zoeken-API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) via de [API ophalen](https://fetch.spec.whatwg.org/). Beide methoden worden hieronder besproken.

## <a name="make-a-search-request-via-service-module"></a>Een zoekopdracht indienen via de servicemodule

<iframe height='500' scrolling='no' title='Zoekresultaten weergeven op een kaart (Service-Module)' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>zoekresultaten weergeven op een kaart (Service-Module)</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code vormt een kaartobject en het verificatiemechanisme voor de sleutel voor het abonnement wordt ingesteld. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede blok van code maakt een **SubscriptionKeyCredentialPolicy** om HTTP-aanvragen naar Azure kaarten met de abonnementssleutel te verifiëren. Vervolgens wordt de **atlas.service.MapsURL.newPipeline()** wordt in de **SubscriptionKeyCredential** beleid en maakt een [pijplijn](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-iot-typescript-latest) exemplaar. De **searchURL** vertegenwoordigt een URL naar Azure Maps [zoeken](https://docs.microsoft.com/rest/api/maps/search) bewerkingen.

Het derde codeblok maakt u een gegevensbron object via de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse en zoekresultaten aan toe te voegen. Een [symbool laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) maakt gebruik van tekst of pictogrammen weergegeven op basis van een punt-gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als symbolen op de kaart.  Een symbool-laag wordt vervolgens gemaakt en de gegevensbron wordt toegevoegd aan het symbool-laag, die wordt vervolgens toegevoegd aan de kaart.

Maakt gebruik van de vierde codeblok de [SearchFuzzy](/javascript/api/azure-maps-rest/atlas.service.models.searchgetsearchfuzzyoptionalparams) methode in de [servicemodule](https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=2). Hiermee kunt u een vrije indeling tekst te zoeken via de [zoeken bij benadering ophalen rest-API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) om te zoeken naar nuttige plaats. Fuzzy zoeken-API Get kunnen een combinatie van fuzzy invoer worden verwerkt. Een verzameling van de functie GeoJSON uit het antwoord wordt vervolgens opgehaald met behulp van de **geojson.getFeatures()** methode en toegevoegd aan de gegevensbron, worden automatisch de gegevens die wordt weergegeven op de kaart via het symbool-laag.

Het laatste blok van code Hiermee past u de grenzen van de camera voor de kaart met behulp van de kaart [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) eigenschap.

De zoekopdracht aanvragen, de gegevensbron en symbool laag en de camera-grenzen worden gemaakt en set binnen de kaart van gereed [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen dat de resultaten worden weergegeven nadat de kaart volledig is geladen.


## <a name="make-a-search-request-via-fetch-api"></a>Een zoekopdracht indienen via de API ophalen

<iframe height='500' scrolling='no' title='Zoekresultaten op een kaart weergeven' src='//codepen.io/azuremaps/embed/KQbaeM/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>zoekresultaten weergeven op een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code vormt een kaartobject en het verificatiemechanisme voor de sleutel voor het abonnement wordt ingesteld. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede codeblok maakt u een URL als u wilt een zoekaanvraag te maken. Het maakt ook twee matrices voor het opslaan van grenzen en pincodes voor zoekresultaten.

Maakt gebruik van de derde blok van code de [API ophalen](https://fetch.spec.whatwg.org/) om te vragen naar [Azure Maps Fuzzy zoeken-API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) om te zoeken naar de punten van belang zijn. Een combinatie van fuzzy invoer kan worden verwerkt door de Fuzzy zoeken-API. Vervolgens worden verwerkt en parseert de antwoorden zoeken en voegt de resultaat-pins toe aan de matrix searchPins.

De vierde blok van code maakt een gegevensbron object via de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse en zoekresultaten aan toe te voegen. Een [symbool laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) maakt gebruik van tekst of pictogrammen weergegeven op basis van een punt-gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als symbolen op de kaart. Een symbool-laag wordt vervolgens gemaakt en de gegevensbron wordt toegevoegd aan het symbool-laag, die wordt vervolgens toegevoegd aan de kaart.

Het laatste blok van code maakt een [BoundingBox](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.boundingbox?view=azure-iot-typescript-latest) object met behulp van de matrix van de resultaten en klikt u vervolgens past u de grenzen van de camera voor de kaart met behulp van de kaart [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-). Deze wordt vervolgens de pincodes resultaat weergegeven.

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
