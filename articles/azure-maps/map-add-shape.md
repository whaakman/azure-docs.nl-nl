---
title: Een vorm met Azure Maps toevoegen | Microsoft Docs
description: Een vorm toevoegen aan een Java script-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 0696eba4f3cca7beedc2efcda0182ab82b3d69d9
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638708"
---
# <a name="add-a-shape-to-a-map"></a>Een vorm aan een kaart toevoegen

In dit artikel wordt beschreven hoe u geometrieën op de kaart kunt weer geven met behulp van lijn-en veelhoek lagen. De websdk van Azure Maps biedt ook ondersteuning voor het maken van cirkel geometrie zoals gedefinieerd in het [uitgebreide GEOjson-schema](extend-geojson.md#circle). Alle functie-geometrieën kunnen ook eenvoudig worden bijgewerkt als deze wordt ingepakt met de klasse [shape](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) .

<a id="addALine"></a>

## <a name="add-lines-to-the-map"></a>Regels toevoegen aan de kaart

`LineString`en `MultiLineString` -onderdelen worden gebruikt om paden en overzichten op de kaart weer te geven.

### <a name="add-a-line"></a>Een regel toevoegen

<iframe height='500' scrolling='no' title='Een regel aan een kaart toevoegen' src='//codepen.io/azuremaps/embed/qomaKv/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bekijk de pen <a href='https://codepen.io/azuremaps/pen/qomaKv/'>een regel toevoegen aan een kaart</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste code blok in de bovenstaande code maakt een kaart object. U kunt [een overzicht maken](./map-create.md) voor instructies.

In het tweede code blok wordt een gegevens bron object gemaakt met behulp van de klasse [Data Source](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) . Er wordt een [Lines Tring](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.linestring?view=azure-iot-typescript-latest) -object gemaakt en toegevoegd aan de gegevens bron.

Een [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) rendert regel objecten die in de [gegevens bron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest)zijn verpakt. Met het laatste code blok wordt een laag gemaakt en toegevoegd aan de kaart. Zie Eigenschappen van een laag op [LineLayerOptions](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest). De gegevens bron en de laag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenis-handler](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen dat de regel wordt weer gegeven nadat de kaart volledig is geladen.

### <a name="add-symbols-along-a-line"></a>Symbolen toevoegen langs een regel

In dit voor beeld ziet u hoe u pijl pictogrammen kunt toevoegen langs een regel op de kaart. Wanneer u een symbool-laag gebruikt, stelt u de optie plaatsing in op regel, waarna de symbolen langs de lijn worden weer gegeven en de pictogrammen worden geroteerd (0 graden = rechts).

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Pijl langs lijn weer geven" src="//codepen.io/azuremaps/embed/drBJwX/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie de <a href='https://codepen.io/azuremaps/pen/drBJwX/'>pijl weer geven naast regel</a> voor Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="line-stroke-gradient"></a>Een lijn kleur overgang toevoegen aan een lijn

Naast het Toep assen van een enkele lijn kleur op een lijn kunt u ook een regel met een kleur overgang vullen om de overgang van het ene lijn segment naar het volgende weer te geven. Lijn kleur overgangen kunnen bijvoorbeeld worden gebruikt om wijzigingen in de loop van de tijd en afstand te vertegenwoordigen, of verschillende Tempe raturen over een verbonden object regel. Als u deze functie op een regel wilt Toep assen, moet de `lineMetrics` optie zijn ingesteld op True voor de gegevens bron. vervolgens kan een kleur verloop expressie worden door gegeven aan de `strokeColor` optie van de regel. De expressie voor de kleur overgang van de `['line-progress']` lijn moet verwijzen naar de gegevens expressie die de berekende regel metrieken voor de expressie beschrijft.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Lijn met lijn kleur overgang" src="//codepen.io/azuremaps/embed/wZwWJZ/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Bekijk de pen <a href='https://codepen.io/azuremaps/pen/wZwWJZ/'>met de lijn kleur overgang</a> op Azure Maps<a href='https://codepen.io/azuremaps'>@azuremaps</a>() op <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="customize-a-line-layer"></a>Een lijn laag aanpassen

De lijn laag bevat verschillende opties voor opmaak. Hier volgt een hulp programma om het uit te proberen.

<br/>

<iframe height='700' scrolling='no' title='Opties voor lijn lagen' src='//codepen.io/azuremaps/embed/GwLrgb/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de <a href='https://codepen.io/azuremaps/pen/GwLrgb/'>laag opties</a> van de pen op Azure Maps<a href='https://codepen.io/azuremaps'>@azuremaps</a>() op <a href='https://codepen.io'>CodePen</a>.
</iframe>

<a id="addAPolygon"></a>

## <a name="add-a-polygon-to-the-map"></a>Een veelhoek toevoegen aan de kaart

`Polygon`en `MultiPolygon` -functies worden vaak gebruikt om een gebied op een kaart aan te duiden. 

### <a name="use-a-polygon-layer"></a>Een polygoon laag gebruiken 

Met een veelhoek laag wordt het gebied van een veelhoek weer gegeven. 

<iframe height='500' scrolling='no' title='Een veelhoek aan een kaart toevoegen ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bekijk de pen <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>een veelhoek aan een kaart toevoegen</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code maakt het eerste code blok een kaart object. U kunt [een overzicht maken](./map-create.md) voor instructies.

In het tweede code blok wordt een gegevens bron object gemaakt met behulp van de klasse [Data Source](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) . Er wordt een [veelhoek](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) gemaakt op basis van een matrix met coördinaten en toegevoegd aan de gegevens bron. 

In een [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) worden gegevens weer gegeven die in de gegevens [bron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) op de kaart zijn verpakt. Met het laatste code blok wordt een polygoon laag gemaakt en toegevoegd aan de kaart. Zie Eigenschappen van een polygoon laag op [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). De gegevens bron en de polygoon laag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenis-handler](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen dat de veelhoek wordt weer gegeven nadat de kaart volledig is geladen.

### <a name="use-a-polygon-and-line-layer-together"></a>Een veelhoek en een lijn-laag samen gebruiken

Een line-laag kan worden gebruikt om het overzicht van een veelhoek weer te geven. 

<iframe height='500' scrolling='no' title='Veelhoek en lijn-laag om veelhoek toe te voegen' src='//codepen.io/azuremaps/embed/aRyEPy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de <a href='https://codepen.io/azuremaps/pen/aRyEPy/'>veelhoek en de lijn-laag om veelhoek toe te voegen</a> met Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code maakt het eerste code blok een kaart object. U kunt [een overzicht maken](./map-create.md) voor instructies.

In het tweede code blok wordt een gegevens bron object gemaakt met behulp van de klasse [Data Source](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) . Er wordt een [veelhoek](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) gemaakt op basis van een matrix met coördinaten en toegevoegd aan de gegevens bron. 

In een [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) worden gegevens weer gegeven die in de gegevens [bron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) op de kaart zijn verpakt. Zie Eigenschappen van een polygoon laag op [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). Een [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) is een matrix met lijnen. Zie Eigenschappen van een laag op [LineLayerOptions](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest). Het derde code blok maakt veelhoek-en lijn lagen.

Het laatste code blok voegt de polygoon-en lijn lagen toe aan de kaart. De gegevens bron en de lagen worden gemaakt en toegevoegd aan de kaart in de [gebeurtenis-handler](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen dat de veelhoek wordt weer gegeven nadat de kaart volledig is geladen.

> [!TIP]
> Met lijn lagen worden standaard de coördinaten van veelhoeken en lijnen in een gegevens bron weer gegeven. Als u de laag zodanig wilt beperken dat de Lines Tring-functies alleen worden `filter` weer gegeven, stelt u `['==', ['geometry-type'], 'LineString']` de `['any', ['==', ['geometry-type'], 'LineString'], ['==', ['geometry-type'], 'MultiLineString']]` eigenschap van de laag in of als u ook multi line string-functies wilt toevoegen.

### <a name="fill-a-polygon-with-a-pattern"></a>Een veelhoek met een patroon vullen

Naast het vullen van een veelhoek met een kleur, kan ook een afbeeldings patroon worden gebruikt. Laad een afbeeldings patroon in de Maps-afbeelding sprite-resources en verwijs vervolgens `fillPattern` naar deze installatie kopie met de eigenschap van de polygoon laag.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Opvul patroon veelhoek" src="//codepen.io/azuremaps/embed/JzQpYX/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie het <a href='https://codepen.io/azuremaps/pen/JzQpYX/'>opvul patroon</a> voor de pen-veelhoek<a href='https://codepen.io/azuremaps'>@azuremaps</a>door Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="customize-a-polygon-layer"></a>Een polygoon laag aanpassen

De polygoon laag heeft slechts enkele opmaak opties. Hier volgt een hulp programma om het uit te proberen.

<br/>

<iframe height='700' scrolling='no' title='LXvxpg' src='//codepen.io/azuremaps/embed/LXvxpg/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/LXvxpg/'>LXvxpg</a> by Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

<a id="addACircle"></a>

## <a name="add-a-circle-to-the-map"></a>Een cirkel aan de kaart toevoegen

Azure Maps maakt gebruik van een uitgebreide versie van het geojson-schema met een definitie voor cirkels zoals [hier](extend-geojson.md#circle)wordt beschreven. Een cirkel kan worden weer gegeven op de `Point` kaart door een functie te maken die een `subType` eigenschap heeft met een waarde van `"Circle"` en `radius` een eigenschap die een getal heeft dat de RADIUS in meters vertegenwoordigt. Bijvoorbeeld:

```javascript
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "subType": "Circle",
        "radius": 100
    }
}  
```

Met de Azure Maps Web SDK worden `Point` deze functies `Polygon` geconverteerd naar functies onder de kaften en kunnen ze worden weer gegeven op de kaart met behulp van veelhoek-en lijn lagen, zoals hier wordt weer gegeven.

<iframe height='500' scrolling='no' title='Een cirkel aan een kaart toevoegen' src='//codepen.io/azuremaps/embed/PRmzJX/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bekijk de pen <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>een cirkel aan een kaart toevoegen</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste code blok in de bovenstaande code maakt een kaart object. U kunt [een overzicht maken](./map-create.md) voor instructies.

In het tweede code blok wordt een gegevens bron object gemaakt met behulp van de klasse [Data Source](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) . Een cirkel is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van [punt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) en heeft een `subType` eigenschap ingesteld op `"Circle"` en een `radius` eigenschaps waarde in meters. Wanneer een punt functie met een `subType` van `"Circle"` wordt toegevoegd aan een gegevens bron, wordt deze omgezet in een cirkel vormige veelhoek binnen de kaart.

In een [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) worden gegevens weer gegeven die in de gegevens [bron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) op de kaart zijn verpakt. Met het laatste code blok wordt een polygoon laag gemaakt en toegevoegd aan de kaart. Zie Eigenschappen van een polygoon laag op [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). De gegevens bron en de polygoon laag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenis-handler](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen dat de cirkel wordt weer gegeven nadat de kaart volledig is geladen.

## <a name="make-a-geometry-easy-to-update"></a>Een geometrie gemakkelijk te updaten maken

Een `Shape` klasse verpakt een [geometrie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.geometry?view=azure-iot-typescript-latest) of [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) en maakt deze eenvoudig bij en houdt deze bij.
`new Shape(data: Feature<data.Geometry, any>)`Hiermee wordt een vorm object gemaakt en geïnitialiseerd met de opgegeven functie.

<br/>

<iframe height='500' scrolling='no' title='Eigenschappen van shape bijwerken' src='//codepen.io/azuremaps/embed/ZqMeQY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de eigenschappen van de <a href='https://codepen.io/azuremaps/pen/ZqMeQY/'>shape pen bijwerken</a> per<a href='https://codepen.io/azuremaps'>@azuremaps</a>Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste code blok hierboven maakt een kaart object. U kunt [een overzicht maken](./map-create.md) voor instructies.

Een punt is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van een [punt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) voor de klasse. Het tweede code blok initialiseert de RADIUS-waarde voor het element HTML-schuif regelaar en maakt vervolgens een punt object in een object [vorm](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) klasse.

Het derde code blok maakt een functie die de waarde van het element schuif regelaar HTML-bereik gebruikt en wijzigt de RADIUS-waarde met behulp van de methode [addProperty](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) van de klasse.

In het vierde code blok wordt een gegevens bron object gemaakt met behulp van de klasse [Data Source](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) . Het punt wordt vervolgens toegevoegd aan de gegevens bron.

In een [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) worden gegevens weer gegeven die in de gegevens [bron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) op de kaart zijn verpakt. Het derde code blok maakt een polygoon laag. Zie Eigenschappen van een polygoon laag op [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). De gegevens bron, de gebeurtenis-handler voor klikken en de polygoon laag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenis-handler](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen dat het punt wordt weer gegeven nadat de kaart volledig is geladen.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer code voorbeelden om toe te voegen aan uw Maps:

> [!div class="nextstepaction"]
> [Op gegevens gebaseerde stijl expressies gebruiken](data-driven-style-expressions-web-sdk.md)
