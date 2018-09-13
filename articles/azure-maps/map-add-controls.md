---
title: Kaart-besturingselementen toevoegen in Azure Maps | Microsoft Docs
description: Het zoombesturingselement, presentatie-besturingselement, draaien control en een stijl datumkiezer toevoegen aan een kaart in de Azure-kaarten.
author: walsehgal
ms.author: v-musehg
ms.date: 08/29/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 0bd7e5bd3fc4c4ce3508ca7d3f877fbdf00d5375
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44717632"
---
# <a name="add-map-controls-to-azure-maps"></a>Kaart-besturingselementen toevoegen aan Azure Maps

Dit artikel leest u hoe kaart-besturingselementen toevoegen aan een kaart. Ook leert u hoe u een kaart maken met alle besturingselementen en een [stijl datumkiezer](https://docs.microsoft.com/azure/azure-maps/choose-map-style#adding-the-style-picker).

## <a name="add-zoom-control"></a>Zoombesturingselement toevoegen

<iframe height='500' scrolling='no' title='Het toevoegen van een zoombesturingselement' src='//codepen.io/azuremaps/embed/WKOQyN/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/WKOQyN/'>toe te voegen een zoombesturingselement</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste codeblok in de bovenstaande code wordt een kaartobject gemaakt. Zie [maken van een kaart](./map-create.md) voor instructies over het maken van een kaart.

Het tweede codeblok maakt een zoomen control-object met behulp van de atlas [ZoomControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.zoomcontrol?view=azure-iot-typescript-latest).

Zoombesturingselement voegt de mogelijkheid om in te zoomen en afmelden bij de kaart. Het derde blok zoombesturingselement toegevoegd aan de kaart met behulp van de kaart [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) methode.

## <a name="add-pitch-control"></a>Tekenbreedte besturingselement toevoegen

<iframe height='500' scrolling='no' title='Het toevoegen van een besturingselement verkopen' src='//codepen.io/azuremaps/embed/xJrwaP/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/xJrwaP/'>toevoegen van een besturingselement inspiratie</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste vereiste blok van de bovenstaande code wordt een kaartobject gemaakt met de stijl van ingesteld op weg in grijswaarden. Zie [maken van een kaart](./map-create.md) voor instructies over het maken van een kaart.

Het tweede codeblok wordt gemaakt van een presentatie control-object met behulp van de atlas [PitchControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.pitchcontrol?view=azure-iot-typescript-latest).

Tekenbreedte besturingselement voegt de mogelijkheid om te wijzigen van de breedte van de kaart. Het derde blok inspiratie besturingselement toegevoegd aan de kaart met behulp van de kaart [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) methode.

## <a name="add-compass-control"></a>Kompas besturingselement toevoegen

<iframe height='500' scrolling='no' title='Het toevoegen van een besturingselement draaien' src='//codepen.io/azuremaps/embed/GBEoRb/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/GBEoRb/'>toevoegen van een besturingselement draaien</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste codeblok in de bovenstaande code wordt een kaartobject gemaakt. Zie [maken van een kaart](./map-create.md) voor instructies over het maken van een kaart.

Het tweede blok van code maakt een kompas control-object met behulp van de atlas [kompas besturingselement](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.compasscontrol?view=azure-iot-typescript-latest#compasscontrol). Ook voegt het kompas besturingselement toe aan de kaart met behulp van de kaart [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) methode.

## <a name="a-map-with-all-controls"></a>Een kaart met alle besturingselementen

<iframe height='500' scrolling='no' title='Een kaart met alle besturingselementen' src='//codepen.io/azuremaps/embed/qyjbOM/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/qyjbOM/'>een kaart met alle besturingselementen</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste codeblok in de bovenstaande code wordt een kaartobject gemaakt. Zie [maken van een kaart](./map-create.md) voor instructies over het maken van een kaart.

Het tweede codeblok maakt een kompas control-object met behulp van de atlas [CompassControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.compasscontrol?view=azure-iot-typescript-latest#compasscontrol) en voegt deze toe aan de kaart met behulp van de kaart [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) methode.

Het derde blok van code maakt een zoomen control-object met behulp van de atlas [ZoomControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.zoomcontrol?view=azure-iot-typescript-latest) en voegt deze toe aan de kaart met behulp van de kaart [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) methode.

De vierde codeblok wordt gemaakt van een presentatie control-object met behulp van de atlas [PitchControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.pitchcontrol?view=azure-iot-typescript-latest) en voegt deze toe aan de kaart met behulp van de kaart [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) methode.

Het laatste blok van code voegt een stijl kiezer-object toe aan de kaart met behulp van de atlas [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol) en voegt deze toe aan de kaart met behulp van de kaart [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) methode.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt: 
* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol)

* [Atlas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest)
    * [CompassControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.compasscontrol?view=azure-iot-typescript-latest#compasscontrol)
    * [ZoomControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.zoomcontrol?view=azure-iot-typescript-latest)
    * [PitchControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.pitchcontrol?view=azure-iot-typescript-latest)
    * [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol)
    
Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten: 
* [Toevoegen van een pincode](./map-add-pin.md)
* [Een pop-up toevoegen](./map-add-popup.md)