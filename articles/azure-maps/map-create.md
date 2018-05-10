---
title: Een map maken met Azure Maps | Microsoft Docs
description: Het maken van een Javascript-kaart
services: azure-maps
keywords: ''
author: jinzh-azureiot
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: codepen
ms.openlocfilehash: 9a7c611860a6d32f82d6714d945c62e6c562f91f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="create-a-map"></a>Een map maken

In dit artikel leest u hoe een kaart te maken.  

## <a name="understand-the-code"></a>De code begrijpen

Er zijn twee manieren waarop u een kaart kunt samenstellen. U kunt de camera van de kaart instellen door te geven van het middelpunt en zoomniveau of de camera grenzen van de kaart instellen door te geven van het begrenzingsvak point- and -noordoosten begrenzingsvak punt linksonder.

<a id="setCameraOptions"></a>

### <a name="setting-the-camera"></a>Instellen van de camera

<iframe height='310' scrolling='no' title='Een kaart via CameraOptions maken' src='//codepen.io/azuremaps/embed/qxKBMN/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>maken van een kaart via CameraOptions</a> door Azure LBS (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code een [toewijzingsobject](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest) wordt gemaakt via `new atlas.Map()`. Eigenschappen Serviceoverzicht zoals center en zoomen niveau deel uitmaken van [CameraOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/cameraoptions?view=azure-iot-typescript-latest). CameraOptions kunnen worden gedefinieerd in kaart constructor of via [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamera) functie van de kaart-klasse.

<a id="setCameraBoundsOptions"></a>

### <a name="setting-the-camera-bounds"></a>De grenzen van de camera instellen

<iframe height='310' scrolling='no' title='Een kaart via CameraBoundsOptions maken' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'>maken van een kaart via CameraBoundsOptions</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code een [toewijzingsobject](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest) is samengesteld `new atlas.Map()`. Eigenschappen Serviceoverzicht zoals begrenzingsvak deel uitmaken van [CameraBoundsOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/cameraboundsoptions?view=azure-iot-typescript-latest). CameraBoundsOptions kunnen worden gedefinieerd [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamerabounds) functie van de kaart-klasse.

## <a name="try-out-the-code"></a>De code uitproberen 

Bekijk de bovenstaande voorbeeldcode. U kunt de JavaScript-code op het tabblad JS aan de linkerkant bewerken en de wijzigingen van de kaart weergeven op het tabblad resultaat aan de rechterkant ziet. U kunt ook op op de knop 'Bewerken op CodePen' en de code in CodePen bewerken. 

<a id="relatedReference"></a>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die worden gebruikt in dit artikel: 
* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamera)
    * [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamerabounds)

