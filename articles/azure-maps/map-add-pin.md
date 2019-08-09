---
title: Een symbool laag toevoegen aan Azure Maps | Microsoft Docs
description: Symbolen toevoegen aan de Java script-kaart
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 3bce4922a33648f5d7c0d211dba126f35603239b
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68849285"
---
# <a name="add-a-symbol-layer-to-a-map"></a>Een symbool laag aan een kaart toevoegen

Dit artikel laat u zien hoe u punt gegevens van een gegevens bron kunt weer geven als een symbool laag op een kaart. Symbool lagen worden gerenderd met behulp van WebGL en bieden veel grotere sets punten dan HTML-markeringen, maar ondersteunen geen traditionele CSS-en HTML-elementen voor opmaak.  

> [!TIP]
> Met symbool lagen worden standaard de coördinaten van alle geometrieën in een gegevens bron weer gegeven. Als u de laag zo wilt beperken dat alleen de functies van de punt geometrie `filter` worden weer gegeven, stelt `['==', ['geometry-type'], 'Point']` u `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` de eigenschap van de laag in of als u ook multi point-functies wilt toevoegen.

## <a name="add-a-symbol-layer"></a>Een symboollaag toevoegen

<iframe height='500' scrolling='no' title='Switch pincode locatie' src='//codepen.io/azuremaps/embed/ZqJjRP/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de <a href='https://codepen.io/azuremaps/pen/ZqJjRP/'>pincode locatie</a> van de pen van Azure Maps<a href='https://codepen.io/azuremaps'>@azuremaps</a>() op <a href='https://codepen.io'>CodePen</a>().
</iframe>

Het eerste code blok hierboven maakt een kaart object. U kunt [een overzicht maken](./map-create.md) voor instructies.

In het tweede code blok wordt een gegevens bron object gemaakt met behulp van de klasse [Data Source](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) . Een geojson- [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature) met een [punt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) geometrie wordt verpakt door de klasse [shape](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) , zodat deze eenvoudiger kan worden bijgewerkt en vervolgens wordt gemaakt en toegevoegd aan de gegevens bron.

Het derde code blok maakt een gebeurtenislistener [](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) en werkt de coördinaten van het punt bij met de muis klik met behulp van de klasse [setCoordinates](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) methode van de vorm.

Een [symbool laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) maakt gebruik van tekst of pictogrammen voor het weer geven van op punten gebaseerde gegevens die in de [gegevens bron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) zijn verpakt als symbolen op de kaart.  De gegevens bron, de Click-gebeurtenislistener en de symbool laag worden gemaakt en toegevoegd aan de kaart in de `ready` [event listener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) -functie om ervoor te zorgen dat het punt wordt weer gegeven nadat de kaart is geladen en gereed is om te worden geopend.

> [!TIP]
> Bij prestaties kunnen symbool lagen standaard de rendering van symbolen optimaliseren door symbolen te verbergen die elkaar overlappen. Wanneer u inzoomt, worden de verborgen symbolen zichtbaar. Als u deze functie wilt uitschakelen en alle symbolen wilt renderen, stelt `allowOverlap` u de eigenschap `iconOptions` van de `true`opties in op.

## <a name="add-a-custom-icon-to-a-symbol-layer"></a>Een aangepast pictogram toevoegen aan een symbool-laag

Symbool lagen worden gerenderd met behulp van WebGL. Alle resources, zoals pictogram afbeeldingen, moeten worden geladen in de WebGL-context. In dit voor beeld ziet u hoe u een aangepast pictogram kunt toevoegen aan de kaart resources en hoe u dit vervolgens kunt gebruiken om punt gegevens weer te geven met een aangepast symbool op de kaart. Voor `textField` de eigenschap van de Symbol-laag moet een expressie worden opgegeven. In dit geval willen we de eigenschap Tempe ratuur weer geven, maar omdat het een getal is, moet dit worden geconverteerd naar een teken reeks. Daarnaast willen we het ' °F ' aan het bestand toevoegen. Een expressie kan worden gebruikt om dit te doen; `['concat', ['to-string', ['get', 'temperature']], '°F']`. 

<br/>

<iframe height='500' scrolling='no' title='Pictogram voor aangepaste symbool afbeelding' src='//codepen.io/azuremaps/embed/WYWRWZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie het <a href='https://codepen.io/azuremaps/pen/WYWRWZ/'>pictogram afbeelding voor aangepast symbool</a> voor pen door<a href='https://codepen.io/azuremaps'>@azuremaps</a>Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> De Azure Maps Web-SDK biedt verschillende aanpas bare afbeeldings sjablonen die u kunt gebruiken met de Symbol-laag. Zie het document [over het gebruik van afbeeldings sjablonen](how-to-use-image-templates-web-sdk.md) voor meer informatie.

## <a name="customize-a-symbol-layer"></a>Een symbool-laag aanpassen 

De Symbol-laag heeft veel stijl opties beschikbaar. Hier volgt een hulp programma om deze verschillende opmaak opties te testen.

<br/>

<iframe height='700' scrolling='no' title='Opties voor symbool lagen' src='//codepen.io/azuremaps/embed/PxVXje/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de <a href='https://codepen.io/azuremaps/pen/PxVXje/'>laag opties</a> van het pen-symbool<a href='https://codepen.io/azuremaps'>@azuremaps</a>per Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
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

Raadpleeg de volgende artikelen voor meer code voorbeelden om toe te voegen aan uw kaarten:

> [!div class="nextstepaction"]
> [Een pop-upvenster toevoegen](map-add-popup.md)

> [!div class="nextstepaction"]
> [Op gegevens gebaseerde stijl expressies gebruiken](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [Afbeeldings sjablonen gebruiken](how-to-use-image-templates-web-sdk.md)

> [!div class="nextstepaction"]
> [Een vorm toevoegen](map-add-shape.md)

> [!div class="nextstepaction"]
> [Een Bubble laag toevoegen](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [HTML-makers toevoegen](map-add-bubble-layer.md)
