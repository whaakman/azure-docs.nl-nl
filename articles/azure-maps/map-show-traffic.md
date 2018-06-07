---
title: Verkeer met Azure-kaarten weergeven | Microsoft Docs
description: Verkeersgegevens weergeven op een Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 6ff7a0270509c244fc97bd04d8ba648fd262dc58
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34600116"
---
# <a name="show-traffic-on-the-map"></a>Verkeer op de kaart weergeven

In dit artikel laat zien hoe verkeer en incidenten informatie weergeven op de kaart. 

## <a name="understand-the-code"></a>De code begrijpen

<iframe height='456' scrolling='no' title='Verkeer op een kaart weergeven' src='//codepen.io/azuremaps/embed/WMLRPw/?height=456&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>verkeer weergeven op een kaart</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De eerste blok van code wordt in de code wordt een toewijzingsobject. U kunt zien [maken van een kaart](map-create.md) voor instructies.

Maakt gebruik van de tweede codeblok [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#settraffic) functie van de kaart-klasse voor het weergeven van de verkeersstromen en incidenten op de kaart.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die worden gebruikt in dit artikel: 
* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#settraffic)

Zie de volgende artikelen voor meer voorbeelden van programmacode toevoegen aan uw maps: 
* [Interactie met de kaart – mouse-gebeurtenissen](./map-events.md)
* [Het bouwen van een toewijzing toegankelijk](./map-accessibility.md)

Bekijk onze [code voorbeeldpagina](http://aka.ms/AzureMapsSamples) voor scenario's met meer toewijzing.
