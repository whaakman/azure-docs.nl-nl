---
title: Toevoegen van een pincode voor TPM met Azure Maps | Microsoft Docs
description: Een pincode toevoegen aan een Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: be3c9960961b5c4800faf476bd6a21f7395f057d
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746375"
---
# <a name="add-pins-to-the-map"></a>Pincodes toevoegen aan de kaart

In dit artikel wordt beschreven hoe u een pincode toevoegen aan een kaart.  

## <a name="understand-the-code"></a>De code begrijpen

<iframe height='500' scrolling='no' title='Een pincode toevoegen aan een kaart' src='//codepen.io/azuremaps/embed/ZrVpEa/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ZrVpEa/'>een pincode toevoegen aan een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Een pincode is gemaakt en toegevoegd aan de kaart in het tweede blok van code. Een pincode is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.atlas.data.feature?view=azure-iot-typescript-latest) van [punt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.atlas.data.point?view=azure-iot-typescript-latest) met [PinProperties](https://docs.microsoft.com/javascript/api/azure-maps-control/models.pinproperties?view=azure-iot-typescript-latest) als de functie-eigenschap. Gebruik `new atlas.data.Feature(new atlas.data.Point())` maken van een pincode en de eigenschappen definiëren. Een pincode-laag is een matrix van pincodes. Gebruik [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins) functie van de klasse toewijzen aan een laag pins toevoegen aan de kaart en het definiëren van de eigenschappen van de pincode-laag. Controleer de eigenschappen van een laag pins op [PinLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.pinlayeroptions?view=azure-iot-typescript-latest). 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt: 
* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)
    
Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten: 
* [Een pop-up toevoegen](./map-add-popup.md)
* [Een vorm toevoegen](./map-add-shape.md)

