---
title: Zoekresultaten met Azure-kaarten weergeven | Microsoft Docs
description: Hoe u een zoekopdracht uitvoert met Azure Maps aanvragen en vervolgens de resultaten op een kaart Javascrip weergeven
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: b0ab271eab45a6f4b05d01713e2e2ddd22a22ea3
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746600"
---
# <a name="show-search-results-on-the-map"></a>Zoekresultaten weergeven op de kaart

Dit artikel ziet u hoe u een aanvraag zoeken en weergeven van de lijst met zoekresultaten op de kaart. 

## <a name="understand-the-code"></a>De code begrijpen

<iframe height='500' scrolling='no' title='Zoekresultaten op een kaart weergeven' src='//codepen.io/azuremaps/embed/KQbaeM/?height=519&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>zoekresultaten weergeven op een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede codeblok maakt en voegt een laag van search kledingwinkelketen op de kaart. U kunt zien [toevoegen van een pincode op de kaart](./map-add-pin.md) voor instructies.

Het derde blok van code verzendt een [XMLHttpRequest](https://xhr.spec.whatwg.org/) naar [Azure Maps Fuzzy zoeken-API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy).

Het laatste blok van code parseert de binnenkomende respons. Voor een geslaagde respons verzamelt het de gegevens voor breedtegraad en lengtegraad voor elke locatie die is geretourneerd. Alle locatiepunten toegevoegd aan de kaart als pins en past u de grenzen van de kaart om weer te geven van alle pincodes.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt: 

* [Azure kaarten-Fuzzy zoeken-API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)
* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)
    
Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten: 
* [Gegevens ophalen uit een coördinaat](./map-get-information-from-coordinate.md)
* [Routebeschrijving van A naar B](./map-route.md)
