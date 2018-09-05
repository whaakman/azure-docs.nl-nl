---
title: Style-functies van Azure Maps toewijzen | Microsoft Docs
description: Meer informatie over Azure Maps stijl gerelateerde functies.
author: walsehgal
ms.author: v-musehg
ms.date: 08/31/2018
ms.topic: how-to-guides
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: b95e3c325160d1c0b775320e25fdd40514b02b9f
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666868"
---
# <a name="choose-a-map-style-in-azure-maps"></a>Kies een stijl kaart op Azure-kaarten
Azure Maps heeft vier verschillende maps stijlen om uit te kiezen. Zie voor meer informatie over de kaartstijlen [kaartstijlen ondersteund in Azure Maps](./supported-map-styles.md). Dit artikel leest hoe u de functies met betrekking tot stijl voor een stijl instellen bij het laden van de kaart, de stijl van een nieuwe instellen en gebruiken van een besturingselement voor het kiezen van de stijl.

## <a name="setting-style-on-map-load"></a>Stijl instellen bij het laden van de kaart

<iframe height='500' scrolling='no' title='De stijl instellen bij het laden van de kaart' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>de stijl instellen bij het laden van de kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De bovenstaande code wordt een kaartobject gemaakt met de stijl van ingesteld op weg in grijswaarden. Zie [maken van een kaart](./map-create.md) voor instructies over het maken van een kaart.

## <a name="updating-the-style"></a>De stijl bijwerken

<iframe height='500' scrolling='no' title='De stijl bijwerken' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>bijwerken van de stijl</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste vereiste blok van code in de bovenstaande code wordt een kaartobject gemaakt zonder vooraf instellen van de stijl. Zie [maken van een kaart](./map-create.md) voor instructies over het maken van een kaart.

De tweede code klok maakt gebruik van de kaart [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setstyle) methode om in te stellen van de stijl van de kaart naar satelliet.

## <a name="adding-the-style-picker"></a>De Stijlkiezer toevoegen

<iframe height='500' scrolling='no' title='De Stijlkiezer toevoegen' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>toe te voegen de Stijlkiezer</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste codeblok in de bovenstaande code wordt een kaartobject gemaakt zonder vooraf instellen van de stijl. Zie [maken van een kaart](./map-create.md) voor instructies over het maken van een kaart.

Het tweede codeblok vormt een stijl selector met behulp van de atlas [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol) constructor.

Een stijl kiezen kunt stijl selectie voor de kaart. Het derde codeblok wordt de Stijlkiezer toegevoegd aan de kaart met behulp van de kaart [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) methode.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt: 
* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setstyle)
    * [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol)

* [Atlas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest)
    * [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol)
    
Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten:
* [Kaart-besturingselementen toevoegen](./map-add-controls.md)
* [Toevoegen van een pincode](./map-add-pin.md)
