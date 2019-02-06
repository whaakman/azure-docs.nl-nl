---
title: Informatie weergeven over een coördinaat met Azure Maps | Microsoft Docs
description: Informatie over een adres op de kaart weergegeven wanneer een gebruiker een coördinaat selecteert
author: jingjing-z
ms.author: jinzh
ms.date: 11/15/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 20616adf649924a13e80411aa5135889a175f442
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55750185"
---
# <a name="get-information-from-a-coordinate"></a>Informatie ophalen uit een coördinaat

Dit artikel leest hoe u een omgekeerde zoeken waarin het adres van een aantal malen aangeklikt pop-locatie.

Er zijn twee manieren om een zoekopdracht omgekeerde-adres te maken. Één manier is voor de query de [Azure Maps omgekeerde adres Search API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) via een servicemodule. De andere manier is om een [XMLHttpRequest](https://xhr.spec.whatwg.org/) naar de API-adres vinden. Beide richtingen worden bewaakt hieronder.

## <a name="make-a-reverse-search-request-via-service-module"></a>Een omgekeerde zoeken indienen via de servicemodule

<iframe height='500' scrolling='no' title='Gegevens ophalen uit een coördinaat (Service-Module)' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>-gegevens ophalen uit een coördinaat (Service-Module)</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste vereiste blok van code wordt een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

De regel in het tweede codeblok waarmee een client-service wordt gemaakt.

Het derde blok van code-updates de stijl van de muisaanwijzer op een verwijzing en [pop-upvenster](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) object. U kunt zien [een pop-up toevoegen op de kaart](./map-add-popup.md) voor instructies.

De vierde blok van code voegt een [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) voor klikken met de muis. Bij de muis klikt, wordt een zoekquery gemaakt met de coördinaten van het punt waarop is geklikt. Vervolgens wordt van de kaart [getSearchAddressReverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.search?view=azure-iot-typescript-latest) eindpunt query uitvoeren op het adres voor de coördinaten.

Voor een geslaagde respons verzamelt het adres voor de locatie van het aantal malen aangeklikt en definieert de pop-upvenster inhoud en de positie via [setOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) functie van de klasse pop-upvenster.

De wijziging van de cursor, een pop-object en de gebeurtenis click worden gemaakt in van de kaart [load gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen kaart belastingen volledig voordat de coördinaten informatie kan worden opgehaald.

## <a name="make-a-reverse-search-request-via-xmlhttprequest"></a>Een omgekeerde zoeken via XMLHttpRequest indienen

<iframe height='500' scrolling='no' title='Informatie ophalen uit een coördinaat' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>-gegevens ophalen uit een coördinaat</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste vereiste blok van de bovenstaande code wordt een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede codeblok updates de stijl van de muisaanwijzer op een wijzer en [pop-upvenster](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) object. U kunt zien [een pop-up toevoegen op de kaart](./map-add-popup.md) voor instructies.

Het derde blok van code voegt een gebeurtenislistener voor klikken met de muis. Bij de muis klikt, stuurt deze een [XMLHttpRequest](https://xhr.spec.whatwg.org/) naar [Azure Maps omgekeerde adres Search API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) query voor het aantal malen aangeklikt coördinaten-adres. Voor een geslaagde respons verzamelt het adres voor de locatie van het aantal malen aangeklikt en definieert de pop-upvenster inhoud en de positie via [setOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) functie van de klasse pop-upvenster.

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
