---
title: Toevoegen van een pincode met Azure Maps | Microsoft Docs
description: Het toevoegen van een pincode naar een Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 094abe08c0c88c7561185675ceb8529be2c87a0a
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294645"
---
# <a name="add-pins-to-the-map"></a>Pincodes toevoegen aan de kaart

In dit artikel laat zien hoe een pincode toevoegen aan een kaart.  

## <a name="understand-the-code"></a>De code begrijpen

<iframe height='500' scrolling='no' title='Een pincode toevoegen aan een kaart' src='//codepen.io/azuremaps/embed/ZrVpEa/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ZrVpEa/'>een pincode toevoegen aan een kaart</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De eerste blok van code wordt in de code wordt een toewijzingsobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code is een pincode gemaakt en toegevoegd aan de kaart. Een pincode is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-javascript/feature?view=azure-iot-typescript-latest) van [punt](https://docs.microsoft.com/javascript/api/azure-maps-javascript/point?view=azure-iot-typescript-latest) met [PinProperties](https://docs.microsoft.com/javascript/api/azure-maps-javascript/pinproperties?view=azure-iot-typescript-latest) als de functie-eigenschap. Gebruik `new atlas.data.Feature(new atlas.data.Point())` maken van een pincode en bijbehorende eigenschappen definiëren. Een laag van de pincode is een matrix van pincodes. Gebruik [addPins](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpins) functie van de klasse kaart een pincode-laag toevoegen aan de kaart en definiëren van de eigenschappen van de pincode-laag. Controleer de eigenschappen van een laag van de pincode op [PinLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/pinlayeroptions?view=azure-iot-typescript-latest). 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die worden gebruikt in dit artikel: 
* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpins)
    
Zie de volgende artikelen voor meer voorbeelden van programmacode toevoegen aan uw maps: 
* [Een pop-up toevoegen](./map-add-popup.md)
* [Een vorm toevoegen](./map-add-shape.md)

