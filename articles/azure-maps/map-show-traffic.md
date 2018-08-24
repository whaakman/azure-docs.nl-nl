---
title: Verkeer met Azure-kaarten weergeven | Microsoft Docs
description: Hoe verkeersgegevens weergeven op een Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 2499822db587dbf47dccedf6039d0fb5823c58b5
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746075"
---
# <a name="show-traffic-on-the-map"></a>Verkeer op de kaart weergeven

Dit artikel ziet u hoe u informatie over verkeer en incidenten weergeven op de kaart. 

## <a name="understand-the-code"></a>De code begrijpen

<iframe height='456' scrolling='no' title='Verkeer op een kaart weergeven' src='//codepen.io/azuremaps/embed/WMLRPw/?height=456&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>verkeer weergeven op een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](map-create.md) voor instructies.

Maakt gebruik van het tweede codeblok [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#settraffic) functie van de kaart-klasse om de verkeersstromen en incidenten op de kaart weer te geven.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt: 
* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#settraffic)

Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten: 
* [Interactie met de kaart – muisgebeurtenissen](./map-events.md)
* [Het bouwen van een toegankelijke-kaart](./map-accessibility.md)

Bekijk onze [voorbeeld codetabel](http://aka.ms/AzureMapsSamples) voor meer scenario's voor toewijzing.
