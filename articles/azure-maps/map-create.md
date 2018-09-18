---
title: Een kaart maken met Azure Maps | Microsoft Docs
description: Over het maken van een Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 09/14/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 9759c4149c6b026837e550dcf3ab0a0156bbb736
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45730006"
---
# <a name="create-a-map"></a>Een kaart maken

In dit artikel wordt beschreven hoe u een kaart te maken.  

## <a name="understand-the-code"></a>De code begrijpen

Er zijn twee manieren kunt u een kaart maken. U kunt de camera van de kaart door op te geven het middelpunt en zoomniveau. Stel de grenzen van de camera van de kaart door op te geven het zuidwesten omsluitende punt en noordoosten begrenzingsvak punt.

<a id="setCameraOptions"></a>

### <a name="set-the-camera"></a>Stel de camera

<iframe height='310' scrolling='no' title='Een kaart via CameraOptions maken' src='//codepen.io/azuremaps/embed/qxKBMN/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>maken van een kaart via `CameraOptions` </a>door Azure Location Based Services (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code een [toewijzingsobject](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) wordt gemaakt `new atlas.Map()`. Eigenschappen van de kaart zoals centreren en zoomen niveau maken deel uit van [CameraOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraoptions?view=azure-iot-typescript-latest). `CameraOptions` kunnen worden gedefinieerd in de constructor kaart of via [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera) functie van de kaart-klasse.

<a id="setCameraBoundsOptions"></a>

### <a name="set-the-camera-bounds"></a>Stel de grenzen van de camera

<iframe height='310' scrolling='no' title='Een kaart via CameraBoundsOptions maken' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'>maken van een kaart via `CameraBoundsOptions` </a>Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code een [toewijzingsobject](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) is samengesteld `new atlas.Map()`. Eigenschappen van de kaart zoals begrenzingsvak maken deel uit van [CameraBoundsOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest). `CameraBoundsOptions` kunnen worden gedefinieerd [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) functie van de kaart-klasse.

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
