---
title: Een symbool laag toevoegen aan Azure-kaarten | Microsoft Docs
description: Symbolen toevoegen aan de Javascript-kaart
author: rbrundritt
ms.author: richbrun
ms.date: 12/2/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 2580f1177bf9e6e3a92934f88a5d8ab51894e8d9
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59269483"
---
# <a name="add-a-symbol-layer-to-a-map"></a>Een symbool laag toevoegen aan een kaart

Dit artikel leest u hoe u gegevens uit een gegevensbron als een laag symbool op een kaart kan weergeven. Symbool lagen worden samengesteld met WebGL en ondersteuning van veel grotere sets van punten dan HTML-markeringen, maar bieden geen ondersteuning voor traditionele CSS en HTML-elementen voor stijl.  

> [!TIP]
> Symbool lagen standaard worden de coördinaten van alle geometrie in een gegevensbron weergegeven. Functies instellen om te beperken van de laag zodanig dat deze alleen punt geometrie wordt weergegeven de `filter` eigenschap van de laag `['==', ['geometry-type'], 'Point']` of `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` als u wilt opnemen en MultiPoint functies.

## <a name="add-a-symbol-layer"></a>Een symboollaag toevoegen

<iframe height='500' scrolling='no' title='Switch-pincode-locatie' src='//codepen.io/azuremaps/embed/ZqJjRP/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ZqJjRP/'>Switch pincode locatie</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste vereiste blok van de bovenstaande code wordt een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, object voor een gegevensbron wordt gemaakt met behulp van de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse. Een [functie] met een [punt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) geometry is verpakt door de [vorm](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) klasse zodat het eenvoudiger om bij te werken, en vervolgens gemaakt en toegevoegd aan de gegevensbron.

Het derde blok van code maakt een [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) en updates van het punt coördinaten van de muis klikt u op met behulp van de Shapeklasse [setCoordinates](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) methode.

Een [symbool laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) maakt gebruik van tekst of pictogrammen weergegeven op basis van een punt-gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als symbolen op de kaart.  De gegevensbron, de gebeurtenislistener klikken en het symbool-laag worden gemaakt en toegevoegd aan de kaart in de `ready` [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) functie om te controleren of het punt wordt weergegeven na de kaart geladen en klaar om te worden geopend.

> [!TIP]
> Standaard voor de prestaties optimaliseren symbool lagen de rendering van symbolen door te verbergen symbolen die elkaar overlappen. Als u de verborgen symbolen in te zoomen zichtbaar worden. Voor deze functie uitschakelen en weer van alle symbolen te allen tijde, stel de `allowOverlap` eigenschap van de `iconOptions` -opties op `true`.

## <a name="add-a-custom-icon-to-a-symbol-layer"></a>Een aangepast pictogram toevoegen aan een symbool-laag

Symbool lagen worden weergegeven met behulp van WebGL. Als alle resources, zoals pictogramafbeeldingen, moeten worden geladen in de context WebGL. Dit voorbeeld laat zien hoe u een aangepast pictogram toevoegen aan het toewijzen van resources en vervolgens worden gebruikt om gegevens met een aangepaste symbool op de kaart weer te geven. De `textField` eigenschap van het symbool laag vereist een expressie die moet worden opgegeven. In dit geval willen we de temperatuur-eigenschap weergegeven, maar omdat dit een getal is, moet worden geconverteerd naar een tekenreeks. Daarnaast willen we de "° F' toegevoegd aan. Een expressie kan worden gebruikt om te doen. `['concat', ['to-string', ['get', 'temperature']], '°F']`. 

<br/>

<iframe height='500' scrolling='no' title='Pictogram van een afbeelding aangepaste symbool' src='//codepen.io/azuremaps/embed/WYWRWZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/WYWRWZ/'>pictogram van een afbeelding aangepaste symbool</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="customize-a-symbol-layer"></a>Een symbool laag aanpassen 

Het symbool-laag heeft veel opmaakopties beschikbaar. Hier is een hulpprogramma voor het testen van deze verschillende opmaakopties.

<br/>

<iframe height='700' scrolling='no' title='Opties voor Tegellaag symbool' src='//codepen.io/azuremaps/embed/PxVXje/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/PxVXje/'>symbool opties voor Tegellaag</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [SymbolLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [IconOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [TexTOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)

Zie de volgende artikelen voor meer voorbeelden van code toevoegen aan uw kaarten:

> [!div class="nextstepaction"]
> [Een pop-up toevoegen](map-add-popup.md)

> [!div class="nextstepaction"]
> [Gegevensgestuurde stijl expressies gebruiken](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [Een vorm toevoegen](map-add-shape.md)

> [!div class="nextstepaction"]
> [Een bellendiagram laag toevoegen](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [HTML-Makers toevoegen](map-add-bubble-layer.md)
