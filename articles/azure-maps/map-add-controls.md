---
title: Kaart-besturingselementen toevoegen in Azure Maps | Microsoft Docs
description: Het zoombesturingselement, presentatie-besturingselement, draaien control en een stijl datumkiezer toevoegen aan een kaart in de Azure-kaarten.
author: walsehgal
ms.author: v-musehg
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 781e5d71637a1e86a56dee0aad3c1a5e00f1807a
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48885784"
---
# <a name="add-map-controls-to-azure-maps"></a>Kaart-besturingselementen toevoegen aan Azure Maps

Dit artikel leest u hoe kaart-besturingselementen toevoegen aan een kaart. Ook leert u hoe u een kaart maken met alle besturingselementen en een [stijl datumkiezer](https://docs.microsoft.com/azure/azure-maps/choose-map-style#adding-the-style-picker).

## <a name="add-zoom-control"></a>Zoombesturingselement toevoegen

<iframe height='500' scrolling='no' title='Het toevoegen van een zoombesturingselement' src='//codepen.io/azuremaps/embed/WKOQyN/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/WKOQyN/'>toe te voegen een zoombesturingselement</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste codeblok Hiermee stelt u de abonnementssleutel en maakt u een kaartobject zonder vooraf instellen van de stijl. Zie [maken van een kaart](./map-create.md) voor instructies over het maken van een kaart.

Zoombesturingselement voegt de mogelijkheid om in te zoomen en afmelden bij de kaart. Het tweede codeblok maakt een uitzoomen-object met behulp van de atlas [ZoomControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.zoomcontrol?view=azure-iot-typescript-latest) en voegt deze toe aan de kaart met behulp van de kaart [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) methode. De zoomknop valt binnen de kaart **gebeurtenislistener** om te controleren of het nadat de kaart wordt geladen volledig is geladen.

## <a name="add-pitch-control"></a>Tekenbreedte besturingselement toevoegen

<iframe height='500' scrolling='no' title='Het toevoegen van een besturingselement verkopen' src='//codepen.io/azuremaps/embed/xJrwaP/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/xJrwaP/'>toevoegen van een besturingselement inspiratie</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste codeblok Hiermee stelt u de abonnementssleutel en maakt u een kaartobject zonder vooraf instellen van de stijl. Zie [maken van een kaart](./map-create.md) voor instructies over het maken van een kaart.

Tekenbreedte besturingselement voegt de mogelijkheid om te wijzigen van de breedte van de kaart. Het tweede blok van code maakt een presentatie-Control-object met behulp van de atlas [PitchControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.pitchcontrol?view=azure-iot-typescript-latest) en voegt deze toe aan de kaart met behulp van de kaart [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) methode. Het besturingselement inspiratie valt binnen de kaart **gebeurtenislistener** om te controleren of het nadat de kaart wordt geladen volledig is geladen.

## <a name="add-compass-control"></a>Kompas besturingselement toevoegen

<iframe height='500' scrolling='no' title='Het toevoegen van een besturingselement draaien' src='//codepen.io/azuremaps/embed/GBEoRb/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/GBEoRb/'>toevoegen van een besturingselement draaien</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste codeblok Hiermee stelt u de abonnementssleutel en maakt u een kaartobject zonder vooraf instellen van de stijl. Zie [maken van een kaart](./map-create.md) voor instructies over het maken van een kaart.

Het tweede blok van code maakt een kompas-Control-object met behulp van de atlas [kompas besturingselement](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.compasscontrol?view=azure-iot-typescript-latest#compasscontrol). Ook voegt het kompas besturingselement toe aan de kaart met behulp van de kaart [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) methode. Het besturingselement kompas valt binnen de kaart **gebeurtenislistener** om te controleren of het nadat de kaart wordt geladen volledig is geladen.

## <a name="a-map-with-all-controls"></a>Een kaart met alle besturingselementen

<iframe height='500' scrolling='no' title='Een kaart met alle besturingselementen' src='//codepen.io/azuremaps/embed/qyjbOM/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/qyjbOM/'>een kaart met alle besturingselementen</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste codeblok Hiermee stelt u de abonnementssleutel en maakt u een kaartobject zonder vooraf instellen van de stijl. Zie [maken van een kaart](./map-create.md) voor instructies over het maken van een kaart.

Het tweede codeblok maakt een kompas-Control-object met behulp van de atlas [CompassControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.compasscontrol?view=azure-iot-typescript-latest#compasscontrol) en voegt deze toe aan de kaart met behulp van de kaart [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) methode.

Het derde blok van code maakt een uitzoomen-object met behulp van de atlas [ZoomControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.zoomcontrol?view=azure-iot-typescript-latest) en voegt deze toe aan de kaart met behulp van de kaart [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) methode.

De vierde codeblok wordt gemaakt van een presentatie-Control-object met behulp van de atlas [PitchControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.pitchcontrol?view=azure-iot-typescript-latest) en voegt deze toe aan de kaart met behulp van de kaart [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) methode.

Het laatste blok van code wordt een Stijlkiezer-object gemaakt met behulp van de atlas [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol) en voegt deze toe aan de kaart met behulp van de kaart [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) methode. Alle controleobjecten worden toegevoegd binnen de kaart **gebeurtenislistener** om ervoor te zorgen dat deze worden geladen nadat de kaart volledig is geladen.

De volgorde van de objecten van het besturingselement in het script bepaalt de volgorde waarin ze worden weergegeven op de kaart. U wijzigt de volgorde van de besturingselementen op de kaart, kunt u de volgorde in het script wijzigen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Atlas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest)

Zie de volgende artikelen voor de volledige code:

> [!div class="nextstepaction"]
> [Toevoegen van een pincode](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Een pop-up toevoegen](./map-add-popup.md)