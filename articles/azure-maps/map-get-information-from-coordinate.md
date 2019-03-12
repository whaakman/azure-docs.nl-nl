---
title: Informatie weergeven over een coördinaat met Azure Maps | Microsoft Docs
description: Informatie over een adres op de kaart weergegeven wanneer een gebruiker een coördinaat selecteert
author: jingjing-z
ms.author: jinzh
ms.date: 3/7/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 9ab6d9708843cda492795b92b6fb5f58bd7a9154
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57571419"
---
# <a name="get-information-from-a-coordinate"></a>Informatie ophalen uit een coördinaat

Dit artikel leest hoe u een omgekeerde zoeken waarin het adres van een aantal malen aangeklikt pop-locatie.

Er zijn twee manieren om een zoekopdracht omgekeerde-adres te maken. Één manier is voor de query de [Azure Maps omgekeerde adres Search API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) via een servicemodule. De andere manier is om te gebruiken de [API ophalen](https://fetch.spec.whatwg.org/) om te vragen naar de [Azure Maps omgekeerde adres Search API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) -adres vinden. Beide richtingen worden bewaakt hieronder.

## <a name="make-a-reverse-search-request-via-service-module"></a>Een omgekeerde zoeken indienen via de servicemodule

<iframe height='500' scrolling='no' title='Gegevens ophalen uit een coördinaat (Service-Module)' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>-gegevens ophalen uit een coördinaat (Service-Module)</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code vormt een kaartobject en het verificatiemechanisme voor de sleutel voor het abonnement wordt ingesteld. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede blok van code maakt een **SubscriptionKeyCredentialPolicy** om HTTP-aanvragen naar Azure kaarten met de abonnementssleutel te verifiëren. Vervolgens wordt de **atlas.service.MapsURL.newPipeline()** wordt in de **SubscriptionKeyCredential** beleid en maakt een [pijplijn](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-iot-typescript-latest) exemplaar. De **searchURL** vertegenwoordigt een URL naar Azure Maps [zoeken](https://docs.microsoft.com/rest/api/maps/search) bewerkingen.

Het derde codeblok updates van de stijl van de muisaanwijzer op een wijzer en maakt een [pop-upvenster](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) object. U kunt zien [een pop-up toevoegen op de kaart](./map-add-popup.md) voor instructies.

De vierde blok van code voegt een muisklik [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events). Wanneer ze worden geactiveerd, wordt een zoekquery gemaakt met de coördinaten van het punt waarop is geklikt. Vervolgens wordt de servicemodule [getSearchAddressReverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest#searchaddressreverse-aborter--geojson-position--searchaddressreverseoptions-) methode query de [ophalen adres Reverse-API voor Search](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) voor het adres van de coördinaten. Een verzameling van de functie GeoJSON uit het antwoord wordt vervolgens opgehaald met behulp van de **geojson.getFeatures()** methode.

Het vijfde blok van code stelt u de pop-upvenster HTML-inhoud om de antwoordadres voor het aantal malen aangeklikt coördinaat positie weer te geven.

De wijziging van de cursor, een pop-object en de gebeurtenis click worden gemaakt in van de kaart [load gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen kaart belastingen volledig voordat de coördinaten informatie kan worden opgehaald.

## <a name="make-a-reverse-search-request-via-fetch-api"></a>Een omgekeerde zoeken indienen via de API ophalen

Klik op de kaart om een omgekeerde geocode indienen voor die locatie met behulp van ophalen.

<iframe height='500' scrolling='no' title='Informatie ophalen uit een coördinaat' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>-gegevens ophalen uit een coördinaat</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code vormt een kaartobject en het verificatiemechanisme voor de sleutel voor het abonnement wordt ingesteld. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede codeblok updates de stijl van de muisaanwijzer op een wijzer en [pop-upvenster](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) object. U kunt zien [een pop-up toevoegen op de kaart](./map-add-popup.md) voor instructies.

Het derde blok van code voegt een gebeurtenislistener voor klikken met de muis. Bij een muisklik hierbij wordt gebruikgemaakt van de [API ophalen](https://fetch.spec.whatwg.org/) aan query de [Azure Maps omgekeerde adres Search API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) voor het aantal malen aangeklikt coördinaten-adres. Voor een geslaagde respons verzamelt het adres voor de locatie van het aantal malen aangeklikt en definieert de pop-upvenster inhoud en de positie via [setOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) functie van de klasse pop-upvenster.

De wijziging van de cursor, een pop-object en de gebeurtenis click worden gemaakt in van de kaart [load gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen kaart belastingen volledig voordat de coördinaten informatie kan worden opgehaald.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Zie de volgende artikelen voor volledige codevoorbeelden:

> [!div class="nextstepaction"]
> [Routebeschrijving van A naar B](./map-route.md)

> [!div class="nextstepaction"]
> [Verkeer weergeven](./map-show-traffic.md)
