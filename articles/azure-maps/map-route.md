---
title: Routebeschrijving met Azure Maps | Microsoft Docs
description: Hoe richtingen tussen twee locaties op een Javascript-kaart weergeven
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: 52462c1c5a2a1a9698a2b51708e63b1bb1664f6e
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/23/2018
ms.locfileid: "42745533"
---
# <a name="show-directions-from-a-to-b"></a>Routebeschrijving van A naar B 

Dit artikel ziet u hoe u een route indienen en de route weergeven op de kaart. 

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

* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds)
    * [addLinestrings](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)

Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten: 
* [Verkeer op de kaart weergeven](./map-show-traffic.md)
* [Interactie met de kaart - muisgebeurtenissen](./map-events.md)
