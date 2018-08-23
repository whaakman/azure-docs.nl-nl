---
title: Informatie weergeven over een coördinaat met Azure Maps | Microsoft Docs
description: Informatie over een adres op de kaart weergegeven wanneer een gebruiker een coördinaat selecteert
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: b640346b0d6f490457e1e82a65c0d3f373d658d3
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/11/2018
ms.locfileid: "42061323"
---
# <a name="get-information-from-a-coordinate"></a>Informatie ophalen uit een coördinaat

Dit artikel ziet u hoe u omgekeerde-adres zoeken en bij een muisklik geven het adres van het aantal malen aangeklikt locatie in een pop-upvenster. 

## <a name="understand-the-code"></a>De code begrijpen

<iframe height='500' scrolling='no' title='Informatie ophalen uit een coördinaat' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>-gegevens ophalen uit een coördinaat</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede codeblok werkt de stijl van de muisaanwijzer op een verwijzing.

Het derde blok van code maakt een pop-upvenster. U kunt zien [een pop-up toevoegen op de kaart](./map-add-popup.md) voor instructies.

Het laatste blok van code voegt een gebeurtenislistener voor klikken met de muis. Bij de muis klikt, stuurt deze een [XMLHttpRequest](https://xhr.spec.whatwg.org/) naar [Azure Maps omgekeerde adres Search API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse). Voor een geslaagde respons verzamelt het adres voor de locatie van het aantal malen aangeklikt en definieert de pop-upvenster inhoud en de positie via [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions) functie van de klasse pop-upvenster

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt: 
* [Reverse-adres zoeken](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addeventlistener)
* [Pop-upvenster](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest)
    * [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions)
    * [openen](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open)
    * [Sluiten](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#close)

Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten: 
* [Routebeschrijving van A naar B](./map-route.md)
* [Verkeer weergeven](./map-show-traffic.md)
