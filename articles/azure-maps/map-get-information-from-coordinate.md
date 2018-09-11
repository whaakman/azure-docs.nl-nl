---
title: Informatie weergeven over een coördinaat met Azure Maps | Microsoft Docs
description: Informatie over een adres op de kaart weergegeven wanneer een gebruiker een coördinaat selecteert
author: jingjing-z
ms.author: jinzh
ms.date: 09/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 993d1da4b2a99ec0f30a5a685835d9f6b6d35a9e
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302891"
---
# <a name="get-information-from-a-coordinate"></a>Informatie ophalen uit een coördinaat

Dit artikel ziet u hoe u omgekeerde-adres zoeken en bij een muisklik geven het adres van het aantal malen aangeklikt locatie in een pop-upvenster.

Er zijn twee manieren om een zoekactie omgekeerde-adres, een door het uitvoeren van query's is de [Azure Maps omgekeerde-adres zoeken-API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) via een service-module en de andere is door een [XMLHttpRequest](https://xhr.spec.whatwg.org/) aan de query-API de adres. We bespreken beide hieronder.

## <a name="making-a-reverse-search-request-via-service-module"></a>Maken van een zoekaanvraag omgekeerde via de servicemodule

### <a name="understand-the-code"></a>De code begrijpen

<iframe height='500' scrolling='no' title='Gegevens ophalen uit een coördinaat (Service-Module)' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>-gegevens ophalen uit een coördinaat (Service-Module)</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

De regel in het tweede codeblok waarmee een serviceclient wordt gemaakt.

Het derde blok van code-updates de stijl van de muisaanwijzer op een verwijzing.

De vierde codeblok Hiermee maakt u een pop-upvenster. U kunt zien [een pop-up toevoegen op de kaart](./map-add-popup.md) voor instructies.

Het laatste blok van code voegt een gebeurtenislistener voor klikken met de muis. Bij de muis klikt, wordt een zoekquery gemaakt met de coördinaten van het punt waarop is geklikt. Vervolgens wordt gebruikt van de kaart [getSearchAddressReverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.search?view=azure-iot-typescript-latest#getsearchaddressreverse) eindpunt query uitvoeren op het adres voor de coördinaten.

Voor een geslaagde respons verzamelt het adres voor de locatie van het aantal malen aangeklikt en definieert de pop-upvenster inhoud en de positie via [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) functie van de klasse pop-upvenster.

## <a name="making-a-reverse-search-request-via-xmlhttprequest"></a>Maken van een zoekaanvraag omgekeerde via XMLHttpRequest

### <a name="understand-the-code"></a>De code begrijpen

<iframe height='500' scrolling='no' title='Informatie ophalen uit een coördinaat' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>-gegevens ophalen uit een coördinaat</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede codeblok werkt de stijl van de muisaanwijzer op een verwijzing.

Het derde blok van code maakt een pop-upvenster. U kunt zien [een pop-up toevoegen op de kaart](./map-add-popup.md) voor instructies.

Het laatste blok van code voegt een gebeurtenislistener voor klikken met de muis. Bij de muis klikt, stuurt deze een [XMLHttpRequest](https://xhr.spec.whatwg.org/) naar [Azure Maps omgekeerde adres Search API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse). Voor een geslaagde respons verzamelt het adres voor de locatie van het aantal malen aangeklikt en definieert de pop-upvenster inhoud en de positie via [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) functie van de klasse pop-upvenster

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt: 
* [Reverse-adres zoeken](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addeventlistener)
* [Pop-upvenster](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)
    * [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions)
    * [openen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open)
    * [Sluiten](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#close)

Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten:
* [Routebeschrijving van A naar B](./map-route.md)
* [Verkeer weergeven](./map-show-traffic.md)
