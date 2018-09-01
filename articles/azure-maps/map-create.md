---
title: Een kaart maken met Azure Maps | Microsoft Docs
description: Over het maken van een Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: c5d48e2e7316f33a565fc61a769a29c00834eed5
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43343366"
---
# <a name="create-a-map"></a>Een kaart maken

In dit artikel wordt beschreven hoe u een kaart te maken.  

## <a name="understand-the-code"></a>De code begrijpen

Er zijn twee manieren kunt u een kaart maken. U kunt het instellen van de camera van de kaart door op te geven het middelpunt en zoomniveau of instellen van de camera grenzen van de kaart door het selectiekader punt en de, noordoosten begrenzingsvak punt zuidwesten op te geven.

<a id="setCameraOptions"></a>

### <a name="setting-the-camera"></a>Instellen van de camera

<iframe height='310' scrolling='no' title='Een kaart via CameraOptions maken' src='//codepen.io/azuremaps/embed/qxKBMN/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>maken van een kaart via CameraOptions</a> door Azure LBS (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code een [toewijzingsobject](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) wordt gemaakt `new atlas.Map()`. Eigenschappen van de kaart zoals centreren en zoomen niveau maken deel uit van [CameraOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraoptions?view=azure-iot-typescript-latest). CameraOptions kunnen worden gedefinieerd in de constructor van de kaart of via [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera) functie van de kaart-klasse.

<a id="setCameraBoundsOptions"></a>

### <a name="setting-the-camera-bounds"></a>Instellen van de grenzen van de camera

<iframe height='310' scrolling='no' title='Een kaart via CameraBoundsOptions maken' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'>maken van een kaart via CameraBoundsOptions</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code een [toewijzingsobject](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) is samengesteld `new atlas.Map()`. Eigenschappen van de kaart zoals begrenzingsvak maken deel uit van [CameraBoundsOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest). CameraBoundsOptions kunnen worden gedefinieerd [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) functie van de kaart-klasse.

## <a name="try-out-the-code"></a>De code uitproberen 

Bekijk de bovenstaande voorbeeldcode. U kunt de JavaScript-code op het tabblad JS aan de linkerkant bewerken en de wijzigingen van de kaart weergeven op het tabblad resultaat aan de rechterkant ziet. U kunt ook klikt u op de knop 'Bewerken op CodePen' en de code in CodePen bewerken. 

<a id="relatedReference"></a>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt: 
* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera)
    * [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds)
    
Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten: 
* [Kies een stijl kaart](choose-map-style.md)
* [Kaart-besturingselementen toevoegen](map-add-controls.md)
    

