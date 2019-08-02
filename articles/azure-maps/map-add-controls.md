---
title: Besturings elementen voor kaarten toevoegen in Azure Maps | Microsoft Docs
description: Het toevoegen van een zoom besturings element, besturings element pitch, besturings element draaien en een stijl kiezer aan een kaart in Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 7a504b8df199a3a461d5eb4e5b7238462b4c438f
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638768"
---
# <a name="add-map-controls-to-azure-maps"></a>Kaart besturings elementen toevoegen aan Azure Maps

Dit artikel laat u zien hoe u kaart besturings elementen kunt toevoegen aan een kaart. U leert ook hoe u een kaart maakt met alle besturings elementen en een [stijl kiezer](https://docs.microsoft.com/azure/azure-maps/choose-map-style).

## <a name="add-zoom-control"></a>Zoom besturings element toevoegen

<iframe height='500' scrolling='no' title='Een zoom besturings element toevoegen' src='//codepen.io/azuremaps/embed/WKOQyN/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/WKOQyN/'>een zoom besturings element toevoegen</a> door Azure Maps<a href='https://codepen.io/azuremaps'>@azuremaps</a>() op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In het eerste code blok wordt een kaart object gemaakt met behulp van het anonieme verificatie mechanisme. Zie [een overzicht maken](./map-create.md) voor instructies over het maken van een kaart.

Met het besturings element zoomen kunt u in-en uitzoomen op de kaart. Met het tweede code blok wordt een zoom besturings object gemaakt met behulp van de Atlas [ZoomControl](/javascript/api/azure-maps-control/atlas.control.zoomcontrol) en aan de kaart toegevoegd met behulp van de [besturings elementen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) van de kaart. methode Add. Het besturings element zoomen bevindt zich in de map **event listener** om ervoor te zorgen dat het wordt geladen nadat de kaart volledig is geladen.

## <a name="add-pitch-control"></a>Besturings element voor Pitch toevoegen

<iframe height='500' scrolling='no' title='Een besturings element voor Pitch toevoegen' src='//codepen.io/azuremaps/embed/xJrwaP/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/xJrwaP/'>een besturings element voor Pitch toevoegen</a> door<a href='https://codepen.io/azuremaps'>@azuremaps</a>Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In het eerste code blok wordt een kaart object gemaakt met behulp van het anonieme verificatie mechanisme. Zie [een overzicht maken](./map-create.md) voor instructies over het maken van een kaart.

Met de optie toont u de mogelijkheid om de Toon hoogte van de kaart te wijzigen. Met het tweede code blok wordt een besturings element voor Pitch gemaakt met behulp van de Atlas [PitchControl](/javascript/api/azure-maps-control/atlas.control.pitchcontrol) en aan de kaart toegevoegd met behulp van de [Controls](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) -methode van de kaart. Het besturings element pitch bevindt zich in de map **event listener** om ervoor te zorgen dat het wordt geladen nadat de kaart volledig is geladen.

## <a name="add-compass-control"></a>Besturings element kompas toevoegen

<iframe height='500' scrolling='no' title='Een besturings element draaien toevoegen' src='//codepen.io/azuremaps/embed/GBEoRb/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/GBEoRb/'>een rotatie besturings element toevoegen</a> door Azure Maps<a href='https://codepen.io/azuremaps'>@azuremaps</a>() op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In het eerste code blok wordt een kaart object gemaakt met behulp van het anonieme verificatie mechanisme. Zie [een overzicht maken](./map-create.md) voor instructies over het maken van een kaart.

Met het tweede code blok wordt een kompas Control-object gemaakt met behulp van het [besturings element Atlas kompas](/javascript/api/azure-maps-control/atlas.control.compasscontrol). Ook wordt het besturings element kompas aan de kaart toegevoegd met behulp van de methode Controls van de kaart. [add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) . Het besturings element kompas bevindt zich in de map **event listener** om er zeker van te zijn dat het wordt geladen nadat de kaart volledig is geladen.

## <a name="a-map-with-all-controls"></a>Een kaart met alle besturings elementen

<iframe height='500' scrolling='no' title='Een kaart met alle besturings elementen' src='//codepen.io/azuremaps/embed/qyjbOM/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/qyjbOM/'>A map met alle besturings elementen</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In het eerste code blok wordt een kaart object gemaakt met behulp van het anonieme verificatie mechanisme. Zie [een overzicht maken](./map-create.md) voor instructies over het maken van een kaart.

Met het tweede code blok wordt een kompas Control-object gemaakt met behulp van de Atlas [CompassControl](/javascript/api/azure-maps-control/atlas.control.compasscontrol) en aan de kaart toegevoegd met behulp van de [Controls](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) -methode van de kaart.

Met het derde code blok wordt een zoom besturings object gemaakt met behulp van de Atlas [ZoomControl](/javascript/api/azure-maps-control/atlas.control.zoomcontrol) en aan de kaart toegevoegd met behulp van de [besturings elementen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) van de kaart. methode Add.

Met het vierde code blok wordt een besturings element voor verkopen gemaakt met behulp van de Atlas [PitchControl](/javascript/api/azure-maps-control/atlas.control.pitchcontrol) en aan de kaart toegevoegd met behulp van de [Controls](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) -methode van de kaart.

Met het laatste code blok maakt u een stijl kiezer-object met de Atlas [StyleControl](/javascript/api/azure-maps-control/atlas.control.stylecontrol) en voegt u het toe aan de kaart met behulp van de [besturings elementen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) van de kaart. methode Add. Alle besturings objecten worden toegevoegd in de map **event listener** om ervoor te zorgen dat ze worden geladen nadat de kaart volledig is geladen.

De volg orde van de besturings objecten in het script bepaalt de volg orde waarin ze op de kaart worden weer gegeven. Als u de volg orde van de besturings elementen op de kaart wilt wijzigen, kunt u de volg orde wijzigen in het script.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Diagram](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Atlas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest)

Raadpleeg de volgende artikelen voor volledige code:

> [!div class="nextstepaction"]
> [Een pincode toevoegen](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Een pop-upvenster toevoegen](./map-add-popup.md)
