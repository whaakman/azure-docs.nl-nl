---
title: Een aangepaste html toevoegen in Azure Maps | Microsoft Docs
description: Een aangepaste html toevoegen aan een Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: e5cfbc7ddc10edf9b21afce73e3b7f8795fcdac9
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46121954"
---
# <a name="add-custom-html-to-the-map"></a>Een aangepaste HTML toevoegen aan de kaart

In dit artikel wordt beschreven hoe u een aangepaste HTML, zoals een afbeeldingsbestand toevoegen aan de kaart.

## <a name="understand-the-code"></a>De code begrijpen

<iframe height='466' scrolling='no' title='Een aangepaste html toevoegen aan een kaart - png' src='//codepen.io/azuremaps/embed/MVoeVw/?height=466&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/MVoeVw/'>aangepaste html toevoegen aan een kaart - png</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede blok van code maakt een HTML-element van een installatiekopie.

Maakt gebruik van het laatste blok van code [addHtml](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addhtml) functie van de klasse toewijzen aan de installatiekopie toevoegen aan de opgegeven positie van de kaart.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten:

> [!div class="nextstepaction"]
> [Zoekresultaten tonen](./map-search-location.md)

> [!div class="nextstepaction"]
> [Gegevens ophalen uit een coördinaat](./map-get-information-from-coordinate.md)