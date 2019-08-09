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
ms.openlocfilehash: eb667c398be0bd51e05a6b65d416d5bce54e4386
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68881967"
---
# <a name="choose-a-map-style-in-azure-maps"></a>Kies een kaart stijl in Azure Maps

Veel van de [ondersteunde kaart stijlen in azure Maps](./supported-map-styles.md) zijn beschikbaar in de Web-SDK. In dit artikel wordt beschreven hoe u de stijl gerelateerde functies gebruikt om een stijl in te stellen voor het laden van kaarten, een nieuwe stijl instellen en het besturings element stijl kiezer te gebruiken.

## <a name="set-style-on-map-load"></a>Stijl instellen voor toewijzings belasting

In de volgende code is de `style` optie van de kaart `grayscale_dark` ingesteld op bij initialisatie.

<br/>

<iframe height='500' scrolling='no' title='Stijl instellen voor toewijzings belasting' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>instellen van de stijl van de kaart belasting</a> door<a href='https://codepen.io/azuremaps'>@azuremaps</a>Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="update-the-style"></a>De stijl bijwerken

In de volgende code, nadat een kaart exemplaar is geladen, wordt de kaart stijl bijgewerkt van `road` naar `satellite` met behulp van de functie [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) van de kaart.

<br/>

<iframe height='500' scrolling='no' title='De stijl bijwerken' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>het profiel bijwerken</a> met Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="add-the-style-picker"></a>De stijl kiezer toevoegen

Met de volgende code wordt een [StyleControl](/javascript/api/azure-maps-control/atlas.control.stylecontrol) toegevoegd aan de kaart, zodat de gebruiker eenvoudig tussen de verschillende kaart stijlen kan scha kelen. 

<br/>

<iframe height='500' scrolling='no' title='De stijl kiezer toevoegen' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>de stijl kiezer toevoegen</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Standaard wordt in het besturings element stijl kiezer een lijst weer gegeven met alle beschik bare stijlen voor het gebruik van de prijs categorie S0 van Azure Maps standaard. Als u het aantal stijlen in deze lijst wilt beperken, geeft u een matrix van de stijlen die u wilt weer geven in de lijst `mapStyle` in de optie van de stijl kiezer. Als u S1 gebruikt en alle beschik bare stijlen wilt weer geven, stelt u `mapStyles` de optie van de stijl kiezer `"all"`in op.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Diagram](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Besturings elementen toevoegen aan uw kaarten:

> [!div class="nextstepaction"]
> [Kaart besturings elementen toevoegen](map-add-controls.md)

> [!div class="nextstepaction"]
> [Een pincode toevoegen](map-add-pin.md)
