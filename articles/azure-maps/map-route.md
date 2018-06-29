---
title: Richtingen met Azure-kaarten weergeven | Microsoft Docs
description: Het weergeven van de richtingen tussen twee locaties op een Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: 5e9ab73ddc16517e17894cddd9bc102f3941f00c
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "35759797"
---
# <a name="show-directions-from-a-to-b"></a>Routebeschrijving van A naar B 

In dit artikel laat zien hoe een route indienen en de route weergeven op de kaart. 

## <a name="understand-the-code"></a>De code begrijpen

<iframe height='500' scrolling='no' title='Aanwijzingen van A naar B weergeven op een kaart' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>aanwijzingen van A naar B weergeven op een kaart</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De eerste blok van code wordt in de code wordt een toewijzingsobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede codeblok wordt en pincodes worden op de kaart om weer te geven het begin- en eindpunt van de route. U kunt zien [toevoegen van een pincode op de kaart](map-add-pin.md) voor instructies.

De derde codeblok gebruikt [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamerabounds) functie van de klasse kaart instellen van het begrenzingsvak van de kaart op basis van de begin- en eindpunt van de route.

Het vierde blok van code verzendt een [XMLHttpRequest](https://xhr.spec.whatwg.org/) naar [Azure Maps Route API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections).

Het laatste blok van code parseert de binnenkomende reactie. Voor een geslaagd antwoord, worden de breedtegraad en lengtegraad informatie voor elke beginpunt verzameld. Een matrix van regels wordt gemaakt door elke beginpunt verbinding te maken met de volgende beginpunt. Alle regels op de kaart voor het weergeven van de route wordt toegevoegd. U kunt zien [toevoegen van een regel op de kaart](./map-add-shape.md#addALine) voor instructies.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die worden gebruikt in dit artikel: 

* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamerabounds)
    * [addLinestrings](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addlinestrings)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpins)

Zie de volgende artikelen voor meer voorbeelden van programmacode toevoegen aan uw maps: 
* [Verkeer op de kaart weergeven](./map-show-traffic.md)
* [Interactie met de kaart - muisgebeurtenissen](./map-events.md)
