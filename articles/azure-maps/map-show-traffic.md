---
title: Verkeer met Azure-kaarten weergeven | Microsoft Docs
description: Hoe verkeersgegevens weergeven op een Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 09/14/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 532001a0cda22903d0bdf807ee868aef211336e0
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51240083"
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

> [!div class="nextstepaction"]
> [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Zie de volgende artikelen voor volledige codevoorbeelden:

> [!div class="nextstepaction"]
> [Voorbeeld-codetabel](https://aka.ms/AzureMapsSamples)

Uw gebruikerservaringen verbeteren:

> [!div class="nextstepaction"]
> [Interactie met muisgebeurtenissen toewijzen](./map-events.md)

> [!div class="nextstepaction"]
> [Het bouwen van een toegankelijke-kaart](./map-accessibility.md)
