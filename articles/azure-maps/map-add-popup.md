---
title: Een pop-up met Azure-kaarten toevoegen | Microsoft Docs
description: Een pop-up toevoegen aan Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 0f86578e33e5c6a2d6528e2deb1c8068a0c94d01
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/23/2018
ms.locfileid: "42747102"
---
# <a name="add-a-popup-to-the-map"></a>Een pop-up toevoegen aan de kaart

In dit artikel wordt beschreven hoe u een pop-upvenster toevoegen aan een kaart.  

## <a name="understand-the-code"></a>De code begrijpen

<a id="addAPopup"></a>

<iframe height='500' scrolling='no' title='Een pop-up toevoegen aan een kaart' src='//codepen.io/azuremaps/embed/zRyKxj/?height=545&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/zRyKxj/'>een pop-up toevoegen aan een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede codeblok wordt gemaakt van een pincode en toe te voegen aan de kaart. U kunt zien [een speld toevoegen aan de kaart](./map-add-pin.md) voor instructies.

Het derde codeblok wordt gemaakt van inhoud die moet worden weergegeven in een pop-upvenster. Pop-upvenster inhoud is HTML-element. 

De vierde blok van code maakt een [pop-upvenster object](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) via `new atlas.Popup()`. Pop-upmenu eigenschappen zoals inhoud en positie maken deel uit van [PopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.popupoptions?view=azure-iot-typescript-latest). PopupOptions kunnen worden gedefinieerd in het pop-upvenster constructor of via [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) functie van de klasse pop-upvenster.

Maakt gebruik van het laatste blok van code [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addeventlistener) functie van de klasse kaart om te luisteren naar mouseover gebeurtenis op de pincodes en maakt gebruik [open](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) functie van de klasse pop-upvenster het pop-upvenster geopend, als de gebeurtenis zich voordoet.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt: 

* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)
    * [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addeventlistener)
* [Pop-upvenster](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)
    * [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions)
    * [openen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open)
    * [Sluiten](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#close)
    
Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten: 
* [Een vorm toevoegen](./map-add-shape.md)
* [Een aangepaste HTML toevoegen](./map-add-custom-html.md)
