---
title: Een kaart maken met Azure Maps | Microsoft Docs
description: Over het maken van een Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 10/30/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: d9ba121beb7d97c90fcad5ed22f49dc9940d525a
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/19/2018
ms.locfileid: "51975414"
---
# <a name="create-a-map"></a>Een kaart maken

Dit artikel laat u manieren voor het maken van een kaart en laten bewegen van een kaart.  

## <a name="understand-the-code"></a>De code begrijpen

Er zijn twee manieren kunt u een kaart maken. U kunt de camera van de kaart door op te geven het middelpunt instellen en zoomniveau of als u de grenzen van de camera van de kaart en in te stellen op te geven het zuidwesten noordoosten begrenzingsvak punten.

<a id="setCameraOptions"></a>

### <a name="set-the-camera"></a>Stel de camera

<iframe height='500' scrolling='no' title='Een kaart via CameraOptions maken' src='//codepen.io/azuremaps/embed/qxKBMN/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>maken van een kaart via `CameraOptions` </a>door Azure Location Based Services (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code een [toewijzingsobject](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) wordt gemaakt `new atlas.Map()` en centreren en zoomen zijn ingesteld. Eigenschappen van de kaart zoals centreren en zoomen niveau maken deel uit van [CameraOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraoptions?view=azure-iot-typescript-latest).

<a id="setCameraBoundsOptions"></a>

### <a name="set-the-camera-bounds"></a>Stel de grenzen van de camera

<iframe height='500' scrolling='no' title='Een kaart via CameraBoundsOptions maken' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'>maken van een kaart via `CameraBoundsOptions` </a>Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code een [toewijzingsobject](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) is samengesteld `new atlas.Map()`. Eigenschappen van kaart zoals `CameraBoundsOptions` kunnen worden gedefinieerd [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera) functie van de kaart-klasse. Grenzen en opvulling eigenschappen zijn ingesteld met behulp van `setCamera`.

### <a name="animate-map-view"></a>Elk afzonderlijk Animate kaartweergave

<iframe height='500' scrolling='no' title='Elk afzonderlijk Animate kaartweergave' src='//codepen.io/azuremaps/embed/WayvbO/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/WayvbO/'>laten bewegen kaartweergave</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste codeblok maakt een [toewijzingsobject](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) via `new atlas.Map()`. Eigenschappen van de kaart zoals centreren en zoomen niveau maken deel uit van [CameraOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraoptions?view=azure-iot-typescript-latest). `CameraOptions` kunnen worden gedefinieerd in de constructor kaart of via [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera) functie van de kaart-klasse. De [toewijzen stijl](https://docs.microsoft.com/azure/azure-maps/supported-map-styles) is ingesteld op `road`.

Het tweede codeblok maakt u een kaart voor elk afzonderlijk animate-functie, die wijziging in de overzichtsweergave van animatie door te definiëren voorzien [AnimateOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.animationoptions?view=azure-iot-typescript-latest) via [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera) functie. De functie wordt geactiveerd door de knop 'Laten bewegen kaart' voor het genereren van een willekeurige zoomniveau van telkens wanneer u klikt.

## <a name="try-out-the-code"></a>De code uitproberen

Bekijk de bovenstaande voorbeeldcode. U kunt de JavaScript-code bewerken op de **JS-tabblad** aan de linkerkant en Zie de kaartweergave wijzigingen op de **resultaat tabblad** aan de rechterkant. U kunt ook klikken op de **bewerken op CodePen** knop en de code in CodePen bewerken.

<a id="relatedReference"></a>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Zie de codevoorbeelden functionaliteit toevoegen aan uw app:

> [!div class="nextstepaction"]
> [Kies een stijl kaart](choose-map-style.md)

> [!div class="nextstepaction"]
> [Kaart-besturingselementen toevoegen](map-add-controls.md)
