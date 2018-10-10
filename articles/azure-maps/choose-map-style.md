---
title: Style-functies van Azure Maps toewijzen | Microsoft Docs
description: Meer informatie over Azure Maps stijl gerelateerde functies.
author: walsehgal
ms.author: v-musehg
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 32571d88ffc08c19f819335254b4f5dda521bdb0
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48885918"
---
# <a name="choose-a-map-style-in-azure-maps"></a>Kies een stijl kaart op Azure-kaarten

Azure Maps heeft vier verschillende maps stijlen om uit te kiezen. Zie voor meer informatie over de kaartstijlen [kaartstijlen ondersteund in Azure Maps](./supported-map-styles.md). Dit artikel leest hoe u de functies met betrekking tot stijl voor een stijl instellen bij het laden van de kaart, de stijl van een nieuwe instellen en gebruiken van een besturingselement voor het kiezen van de stijl.

## <a name="set-style-on-map-load"></a>Set-stijl bij het laden van de kaart

<iframe height='500' scrolling='no' title='De stijl instellen bij het laden van de kaart' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>de stijl instellen bij het laden van de kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het bovenstaande codeblok Hiermee stelt u de abonnementssleutel en maakt een kaart-object met de stijl van ingesteld op grayscale_dark. Zie [maken van een kaart](./map-create.md) voor instructies over het maken van een kaart.

## <a name="update-the-style"></a>De stijl bijwerken

<iframe height='500' scrolling='no' title='De stijl bijwerken' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>bijwerken van de stijl</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het bovenstaande codeblok Hiermee stelt u de abonnementssleutel en maakt u een kaartobject zonder vooraf instellen van de stijl. Zie [maken van een kaart](./map-create.md) voor instructies over het maken van een kaart.

Het tweede codeblok maakt gebruik van de kaart [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setstyle) methode om in te stellen van de stijl van de kaart naar satelliet.

## <a name="add-the-style-picker"></a>De Stijlkiezer toevoegen

<iframe height='500' scrolling='no' title='De Stijlkiezer toevoegen' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>toe te voegen de Stijlkiezer</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste codeblok in de bovenstaande code stelt de abonnementssleutel en maakt u een kaartobject, de stijl van de kaart is vooraf ingesteld op grayscale_dark. Zie [maken van een kaart](./map-create.md) voor instructies over het maken van een kaart.

Het tweede codeblok vormt een stijl selector met behulp van de atlas [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol) constructor.

Een stijl kiezen kunt stijl selectie voor de kaart. Het derde codeblok wordt de Stijlkiezer toegevoegd aan de kaart met behulp van de kaart [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) methode. De stijl selector is binnen de kaart **gebeurtenislistener** om ervoor te zorgen dat deze wordt geladen nadat de kaart volledig is geladen.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Besturingselement voor toevoegen aan uw kaarten:

> [!div class="nextstepaction"]
> [Kaart-besturingselementen toevoegen](./map-add-controls.md)

Voeg een kaart pincode:

> [!div class="nextstepaction"]
> [Toevoegen van een pincode](./map-add-pin.md)
