---
title: Verwerken van muisgebeurtenissen met Azure Maps | Microsoft Docs
description: Hoe u een interactieve Javascript-kaart met kaart gebeurtenissen
author: jingjing-z
ms.author: jinzh
ms.date: 11/29/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 4fce8eae25942d098bb3f3277938bfaa3dafa00b
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499184"
---
# <a name="interact-with-the-map---mouse-events"></a>In de kaart - muisgebeurtenissen werken

In dit artikel leest u hoe u [klasse toewijzen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) [gebeurtenissen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) eigenschap markeren van gebeurtenissen op de kaart en op verschillende lagen van de kaart. Deze ook wordt beschreven hoe u met behulp van de kaart klasse gebeurtenissen eigenschap gebeurtenissen wanneer u met een HTML-markering communiceert markeren.

## <a name="interact-with-the-map"></a>In de kaart werken

<iframe height='600' scrolling='no' title='Interactie met de kaart – muisgebeurtenissen' src='//codepen.io/azuremaps/embed/bLZEWd/?height=600&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/bLZEWd/'>interactie met de kaart – muisgebeurtenissen</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Experimenteer met de bovenstaande kaart en de bijbehorende muisgebeurtenissen gemarkeerd aan de rechterkant ziet. U kunt klikken op de **JS-tabblad** bekijken en bewerken van de JavaScript-code. U kunt ook klikken op de **bewerken op CodePen** knop en de code op CodePen bewerken.

## <a name="interact-with-map-layers"></a>Interactie met kaartlagen

<iframe height='600' scrolling='no' title='Interactie met de kaart: laag gebeurtenissen' src='//codepen.io/azuremaps/embed/bQRRPE/?height=600&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/bQRRPE/'>interactie met de kaart: laag gebeurtenissen</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De bovenstaande code ziet u de naam van de gebeurtenissen die van gestart u tijdens het werken met de laag symbool. Het symbool, bel, regel en Veelhoek laag die alle ondersteuning voor dezelfde set gebeurtenissen. De tegel-laag biedt geen ondersteuning van deze gebeurtenissen.

## <a name="interact-with-html-marker"></a>Interactie met HTML-markering

<iframe height='500' scrolling='no' title='Interactie met de kaart - markering voor HTML-gebeurtenissen' src='//codepen.io/azuremaps/embed/VVzKJY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/VVzKJY/'>interactie met de kaart - HTML-markering gebeurtenissen</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De bovenstaande code voegt Javascript map gebeurtenissen naar een HTML-markering. Deze handleiding komen ook de naam van de gebeurtenissen die van u geactiveerd tijdens het werken met de HTML-markering.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Zie de volgende artikelen voor volledige codevoorbeelden:

> [!div class="nextstepaction"]
> [Met behulp van de module Azure Maps Services](./how-to-use-services-module.md)

> [!div class="nextstepaction"]
> [Voorbeeld-codetabel](https://aka.ms/AzureMapsSamples)
