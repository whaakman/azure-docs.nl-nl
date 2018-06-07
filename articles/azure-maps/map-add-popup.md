---
title: Toevoegen van een pop-upvenster met Azure Maps | Microsoft Docs
description: Het toevoegen van een pop-up aan Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: d9eeac28b204af0bb7d8e204762aae854055b33a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34599300"
---
# <a name="add-a-popup-to-the-map"></a>Een pop-up toevoegen aan de kaart

In dit artikel laat zien hoe een pop-up toevoegen aan een kaart.  

## <a name="understand-the-code"></a>De code begrijpen

<a id="addAPopup"></a>

<iframe height='500' scrolling='no' title='Een pop-up toevoegen aan een kaart' src='//codepen.io/azuremaps/embed/zRyKxj/?height=545&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/zRyKxj/'>een pop-up toevoegen aan een kaart</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De eerste blok van code wordt in de code wordt een toewijzingsobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede codeblok wordt gemaakt van een pincode en toe te voegen aan de kaart. U kunt zien [een pincode toevoegen aan de kaart](./map-add-pin.md) voor instructies.

Het derde codeblok maakt inhoud binnen een pop-up wordt weergegeven. Pop-inhoud is HTML-element. 

Het vierde blok van code maakt een [popup-object](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest) via `new atlas.Popup()`. Pop-upmenu eigenschappen zoals inhoud en positie deel uitmaken van [PopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popupoptions?view=azure-iot-typescript-latest). PopupOptions kunnen worden gedefinieerd in het pop-constructor of via [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions) functie van de pop-klasse.

Maakt gebruik van het laatste blok van code [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addeventlistener) functie van de kaart-klasse om te luisteren naar mouseover gebeurtenis op de pincodes en maakt gebruik van [openen](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open) functie van de klasse pop-upvenster het pop-upvenster openen als de gebeurtenis zich voordoet.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die worden gebruikt in dit artikel: 

* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpins)
    * [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addeventlistener)
* [Pop-upvenster](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest)
    * [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions)
    * [openen](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open)
    * [Sluiten](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#close)
    
Zie de volgende artikelen voor meer voorbeelden van programmacode toevoegen aan uw maps: 
* [Een vorm toevoegen](./map-add-shape.md)
* [Toevoegen van aangepaste HTML](./map-add-custom-html.md)
