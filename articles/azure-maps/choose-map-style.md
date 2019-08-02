---
title: Stijl functionaliteiten toewijzen in Azure Maps | Microsoft Docs
description: Meer informatie over de functionaliteit van Azure Maps stijl.
author: walsehgal
ms.author: v-musehg
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 52936b14264bd4fe1846ae365e1de447d594b612
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639057"
---
# <a name="choose-a-map-style-in-azure-maps"></a>Kies een kaart stijl in Azure Maps

Azure Maps heeft vier verschillende stijlen voor kaarten waaruit u kunt kiezen. Zie [ondersteunde kaart stijlen in azure Maps](./supported-map-styles.md)voor meer informatie over het toewijzen van stijlen. In dit artikel wordt beschreven hoe u de stijl gerelateerde functies gebruikt om een stijl in te stellen voor het laden van kaarten, een nieuwe stijl instellen en het besturings element stijl kiezer te gebruiken.

## <a name="set-style-on-map-load"></a>Stijl instellen voor toewijzings belasting

<iframe height='500' scrolling='no' title='Stijl instellen voor toewijzings belasting' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>instellen van de stijl van de kaart belasting</a> door<a href='https://codepen.io/azuremaps'>@azuremaps</a>Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Met het bovenstaande code blok wordt de abonnements sleutel ingesteld en een kaart object gemaakt, waarbij de stijl is ingesteld op grayscale_dark. Zie [een overzicht maken](./map-create.md) voor instructies over het maken van een kaart.

## <a name="update-the-style"></a>De stijl bijwerken

<iframe height='500' scrolling='no' title='De stijl bijwerken' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>het profiel bijwerken</a> met Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In het bovenstaande code blok wordt de abonnements sleutel ingesteld en er wordt een kaart object gemaakt zonder de stijl vooraf in te stellen. Zie [een overzicht maken](./map-create.md) voor instructies over het maken van een kaart.

Het tweede code blok maakt gebruik van de methode [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) van de kaart om de kaart stijl in te stellen op satelliet.

## <a name="add-the-style-picker"></a>De stijl kiezer toevoegen

<iframe height='500' scrolling='no' title='De stijl kiezer toevoegen' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>de stijl kiezer toevoegen</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Met het eerste code blok in de bovenstaande code wordt de abonnements sleutel ingesteld en een kaart object gemaakt, de kaart stijl is vooraf ingesteld op grayscale_dark. Zie [een overzicht maken](./map-create.md) voor instructies over het maken van een kaart.

Met het tweede code blok wordt een stijl kiezer gemaakt met behulp van de Atlas [StyleControl](/javascript/api/azure-maps-control/atlas.control.stylecontrol) -constructor.

Een stijl kiezer maakt stijl selectie voor de kaart mogelijk. Het derde code blok voegt de stijl kiezer aan de kaart toe met behulp van de [besturings elementen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) van de kaart. de methode Add. De stijl kiezer bevindt zich in de map **event listener** om er zeker van te zijn dat deze wordt geladen nadat de kaart volledig is geladen.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Diagram](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Besturings element toevoegen aan uw kaarten:

> [!div class="nextstepaction"]
> [Kaart besturings elementen toevoegen](./map-add-controls.md)

Een toewijzings pincode toevoegen:

> [!div class="nextstepaction"]
> [Een pincode toevoegen](./map-add-pin.md)
