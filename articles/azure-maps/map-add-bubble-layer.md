---
title: Een laag bel toevoegen aan Azure-kaarten | Microsoft Docs
description: Een laag bel toevoegen aan de Javascript-kaart
author: rbrundritt
ms.author: richbrun
ms.date: 10/30/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 895f6ce728ce608184bf6f68be3b73d5dc384d79
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2018
ms.locfileid: "52892229"
---
# <a name="add-a-bubble-layer-to-a-map"></a>Een laag bel toevoegen aan een kaart

Dit artikel leest u hoe u gegevens uit een gegevensbron als een laag Bel op een kaart kan weergeven. Bel lagen weergegeven punten als cirkels op de kaart met vaste pixelradius. 

> [!TIP]
> Bel lagen standaard worden de coördinaten van alle geometrie in een gegevensbron weergegeven. Functies instellen om te beperken van de laag zodanig dat deze alleen punt geometrie wordt weergegeven de `filter` eigenschap van de laag `['==', '$type', 'Point']`

## <a name="add-a-bubble-layer"></a>Een bellendiagram laag toevoegen

<iframe height='500' scrolling='no' title='BubbleLayer DataSource' src='//codepen.io/azuremaps/embed/mzqaKB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/mzqaKB/'>BubbleLayer DataSource</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, een matrix met [punt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) objecten wordt gedefinieerd en toegevoegd aan de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) object.

Een [Bel laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) wordt weergegeven op basis van een punt-gegevens die zijn ingepakt in de [gegevensbron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als cirkels op de kaart. Het laatste blok van code maakt een bel-laag en voegt deze toe aan de kaart. Controleer de eigenschappen van een laag Bel op [BubbleLayerOptions](/javascript/api/azure-maps-control/atlas.bubblelayeroptions).

De matrix van Point-objecten, de gegevensbron en de bel-laag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) functie om ervoor te zorgen dat de cirkel wordt weergegeven nadat de kaart volledig is geladen.

## <a name="show-labels-with-a-bubble-layer"></a>Labels met een laag bel weergeven

<iframe height='500' scrolling='no' title='MultiLayer DataSource' src='//codepen.io/azuremaps/embed/rqbQXy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/rqbQXy/'>MultiLayer DataSource</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

De bovenstaande code ziet u hoe om te visualiseren en labelgegevens op de kaart. Het eerste vereiste blok van de bovenstaande code wordt een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede blok van code, maakt een [punt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) object. Het maakt vervolgens een gegevensbron object via de [gegevensbron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse en voegt u het punt toe aan de gegevensbron.

Een [Bel laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) wordt weergegeven op basis van een punt-gegevens die zijn ingepakt in de [gegevensbron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als cirkels op de kaart. Het derde blok van code maakt een bel-laag en voegt deze toe aan de kaart. Controleer de eigenschappen van een laag Bel op [BubbleLayerOptions](/javascript/api/azure-maps-control/atlas.bubblelayeroptions).

Een [symbool laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) maakt gebruik van tekst of pictogrammen weergegeven op basis van een punt-gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als symbolen op de kaart. Het laatste blok van code maakt en voegt een laag symbool aan de kaart die het tekstlabel voor de belgrootte wordt weergegeven. Controleer de eigenschappen van een laag symbool op [SymbolLayerOptions](/javascript/api/azure-maps-control/atlas.symbollayeroptions).

De gegevensbron en de lagen worden gemaakt en toegevoegd aan de kaart in de [gebeurtenislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) functie om ervoor te zorgen dat gegevens worden weergegeven nadat de kaart volledig is geladen.

## <a name="customize-a-bubble-layer"></a>Een laag Bel aanpassen

De laag Bel heeft alleen een aantal opties voor stijl. Hier is een hulpprogramma voor ze kunt uitproberen.

<br/>

<iframe height='700' scrolling='no' title='Opties voor Tegellaag Bel' src='//codepen.io/azuremaps/embed/eQxbGm/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/eQxbGm/'>Bel opties voor Tegellaag</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [BubbleLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [BubbleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.bubblelayeroptions?view=azure-iot-typescript-latest)

Zie de volgende artikelen voor meer voorbeelden van code toevoegen aan uw kaarten:

> [!div class="nextstepaction"]
> [Een symbool laag toevoegen](./map-add-pin.md)