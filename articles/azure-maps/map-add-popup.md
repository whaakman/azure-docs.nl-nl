---
title: Een pop-up met Azure-kaarten toevoegen | Microsoft Docs
description: Een pop-up toevoegen aan Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 11/09/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: a6c8a8aa954379036ce566a205b8cb4e97952727
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2018
ms.locfileid: "52887832"
---
# <a name="add-a-popup-to-the-map"></a>Een pop-up toevoegen aan de kaart

In dit artikel wordt beschreven hoe u een pop-upvenster toevoegen aan een punt op een kaart.

## <a name="understand-the-code"></a>De code begrijpen

<a id="addAPopup"></a>

<iframe height='500' scrolling='no' title='Een pop up toevoegen met behulp van Azure Maps' src='//codepen.io/azuremaps/embed/MPRPvz/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/MPRPvz/'>oplopen een pop-server met behulp van Azure Maps</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies. U maakt ook HTML-inhoud moet worden weergegeven in het pop-upvenster is.

Het tweede codeblok maakt u een gegevensbron object via de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse. Een punt is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van de [wijst](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) klasse. Een point-object met een naam en beschrijving van eigenschappen wordt vervolgens gemaakt en toegevoegd aan de gegevensbron.

Een [symbool laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) maakt gebruik van tekst of pictogrammen weergegeven op basis van een punt-gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als symbolen op de kaart.  Een symbool-laag wordt gemaakt in het derde blok van code. De gegevensbron wordt toegevoegd aan het symbool-laag, die wordt vervolgens toegevoegd aan de kaart.

De vierde blok van code maakt een [pop-upvenster object](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) via `new atlas.Popup()`. Pop-upmenu eigenschappen zoals de positie en pixelOffset maken deel uit van [PopupOptions](/javascript/api/azure-maps-control/atlas.popupoptions). PopupOptions kunnen worden gedefinieerd in het pop-upvenster constructor of via [setOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) functie van de klasse pop-upvenster. Een `mouseover` gebeurtenislistener voor de laag symbool wordt gemaakt.

Het laatste blok van code maakt een functie die wordt geactiveerd door de `mouseover` gebeurtenislistener. Het instellen van de inhoud en eigenschappen van het pop-upvenster en het pop-object toegevoegd aan de kaart.

## <a name="reusing-a-popup-with-multiple-points"></a>Hergebruik van een pop-upvenster met meerdere punten

Wanneer u veel gegevenspunten hebt en alleen wilt weergeven van een pop-upvenster op een tijdstip, wordt de aanbevolen aanpak is het een pop-upvenster maken en deze in plaats van het maken van een pop-upvenster voor elke functie punt gebruiken. Op deze manier kunt het aantal DOM-elementen die zijn gemaakt door de toepassing aanzienlijk verminderd die voor betere prestaties zorgen. Hiermee maakt u eigenschappen van 3. Als u op een van deze klikt, wordt een pop-upvenster weergegeven met de inhoud voor deze functie punt.

<br/>

<iframe height='500' scrolling='no' title='Hergebruik van pop-upvenster met meerdere pincodes' src='//codepen.io/azuremaps/embed/rQbjvK/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/rQbjvK/'>hergebruik van pop-upvenster met meerdere pincodes</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Pop-upvenster](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [PopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popupoptions?view=azure-iot-typescript-latest)

Zie de volgende fantastische artikelen voor volledige codevoorbeelden:

> [!div class="nextstepaction"]
> [Een symbool laag toevoegen](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Een HTML-markering toevoegen](./map-add-custom-html.md)

> [!div class="nextstepaction"]
> [Een vorm toevoegen](./map-add-shape.md)