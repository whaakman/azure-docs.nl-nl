---
title: Een vorm met Azure-kaarten toevoegen | Microsoft Docs
description: Een vorm toevoegen aan een Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 10/30/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: f61c7a939902ee5d02b2e9ba896c7555968f9d0d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60769512"
---
# <a name="add-a-shape-to-a-map"></a>Een vorm toevoegen aan een kaart

In dit artikel leest u hoe geometrie op de kaart met behulp van lagen van regel en Veelhoek weergeven. Azure Maps Web SDK ondersteunt ook het maken van de cirkel geometrie zoals gedefinieerd in de [uitgebreid GeoJSON-schema](extend-geojson.md#circle). Alle functie geometrie kunnen ook worden eenvoudig bijgewerkt als verpakt met de [vorm](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) klasse.

<a id="addALine"></a>

## <a name="add-lines-to-the-map"></a>Voeg regels toe aan de kaart

`LineString` en `MultiLineString` functies worden gebruikt om aan te duiden paden en die worden beschreven op de kaart.

### <a name="add-a-line"></a>Een regel toevoegen

<iframe height='500' scrolling='no' title='Een regel toegevoegd aan een kaart' src='//codepen.io/azuremaps/embed/qomaKv/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/qomaKv/'>een regel toegevoegd aan een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste vereiste blok van code in de bovenstaande code wordt een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, object voor een gegevensbron wordt gemaakt met behulp van de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse. Een [LineString](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.linestring?view=azure-iot-typescript-latest) object wordt gemaakt en toegevoegd aan de gegevensbron.

Een [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) renders regel objecten die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Het laatste blok van code wordt gemaakt en wordt de laag van een regel toegevoegd aan de kaart. Controleer de eigenschappen van de laag van een regel op [LineLayerOptions](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest). De gegevensbron en de regel-laag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenis-handler](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen dat de regel wordt weergegeven nadat de kaart volledig is geladen.

### <a name="add-symbols-along-a-line"></a>Symbolen langs een lijn toevoegen

In dit voorbeeld laat zien hoe pictogrammen langs een lijn toevoegen op de kaart. Wanneer met behulp van een laag symbool, stelt u de plaatsingsoptie '' naar 'line', wordt dit weergegeven van de symbolen op de lijn en draaien de pictogrammen (0 graden = rechts).

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Pijl langs lijn weergeven" src="//codepen.io/azuremaps/embed/drBJwX/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie de Pen <a href='https://codepen.io/azuremaps/pen/drBJwX/'>Show pijl langs lijn</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="line-stroke-gradient"></a> Een lijn-verloop toevoegen aan een regel

U kunt ook een regel met een kleurovergang van de kleuren om weer te geven van de overgang van één regel segment naar de volgende niet alleen de lijnkleur van een enkel van toepassing op een regel invullen. Verlopen van de regel kunnen bijvoorbeeld worden gebruikt om aan te duiden wijzigingen gedurende de tijd en de afstand of de verschillende temperaturen in een verbonden line-of-objecten. Om deze functie op een regel toepast, de gegevensbron moet hebben de `lineMetrics` optie is ingesteld op true, en vervolgens een kleurovergang expressie kan worden doorgegeven aan de `strokeColor` optie van de regel. De lijn kleurovergang expressie heeft tot verwijzing naar de `['line-progress']` gegevens-expressie die wordt aangegeven dat de berekende regel metrische gegevens op de expressie.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Lijn met kleurovergang voor de lijn" src="//codepen.io/azuremaps/embed/wZwWJZ/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie de Pen <a href='https://codepen.io/azuremaps/pen/wZwWJZ/'>lijn met kleurovergang voor de lijn</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="customize-a-line-layer"></a>Een lijnlaag aanpassen

De regel van de verschillende opties voor stijl laag. Hier is een hulpprogramma voor ze kunt uitproberen.

<br/>

<iframe height='700' scrolling='no' title='Lijnopties laag' src='//codepen.io/azuremaps/embed/GwLrgb/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/GwLrgb/'>laag Regelopties</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

<a id="addAPolygon"></a>

## <a name="add-a-polygon-to-the-map"></a>Een veelhoek toevoegen aan de kaart

`Polygon` en `MultiPolygon` functies worden vaak gebruikt om weer te geven van een gebied op een kaart. 

### <a name="use-a-polygon-layer"></a>Gebruik een polygoonlaag 

Een polygoonlaag geeft het gebied van een polygoon bevindt. 

<iframe height='500' scrolling='no' title='Een veelhoek toevoegen aan een kaart ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>een veelhoek toevoegen aan een kaart </a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, object voor een gegevensbron wordt gemaakt met behulp van de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse. Een [veelhoek](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) is gemaakt op basis van een matrix van coördinaten en toegevoegd aan de gegevensbron. 

Een [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) weergegeven van de gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) op de kaart. Het laatste blok van code wordt gemaakt en wordt een polygoonlaag toegevoegd aan de kaart. Controleer de eigenschappen van een polygoonlaag op [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). De gegevensbron en de polygoonlaag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenis-handler](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen dat de veelhoek wordt weergegeven nadat de kaart volledig is geladen.

### <a name="use-a-polygon-and-line-layer-together"></a>Een polygoon- en laag samen gebruiken

Een lijnlaag kan worden gebruikt om de omtrek van een polygoon weer te geven. 

<iframe height='500' scrolling='no' title='Polygoon- en laag toe te voegen veelhoek' src='//codepen.io/azuremaps/embed/aRyEPy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/aRyEPy/'>Polygoon- en laag toe te voegen veelhoek</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, object voor een gegevensbron wordt gemaakt met behulp van de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse. Een [veelhoek](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) is gemaakt op basis van een matrix van coördinaten en toegevoegd aan de gegevensbron. 

Een [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) weergegeven van de gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) op de kaart. Controleer de eigenschappen van een polygoonlaag op [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). Een [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) is een matrix van regels. Controleer de eigenschappen van de laag van een regel op [LineLayerOptions](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest). Het derde codeblok maakt polygoon- en lijnlagen.

Het laatste blok van code wordt de polygoon- en lijnlagen toegevoegd aan de kaart. De gegevensbron en de lagen worden gemaakt en toegevoegd aan de kaart in de [gebeurtenis-handler](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen dat de veelhoek wordt weergegeven nadat de kaart volledig is geladen.

> [!TIP]
> Lagen van de regel standaard worden de coördinaten van polygonen, evenals regels in een gegevensbron weergegeven. Functies instellen om te beperken van de laag zodanig dat deze alleen LineString wordt weergegeven de `filter` eigenschap van de laag `['==', ['geometry-type'], 'LineString']` of `['any', ['==', ['geometry-type'], 'LineString'], ['==', ['geometry-type'], 'MultiLineString']]` als u wilt opnemen en MultiLineString-functies.

### <a name="fill-a-polygon-with-a-pattern"></a>Vul een veelhoek met een patroon

Een installatiekopie-patroon kan ook worden gebruikt naast het invullen van een veelhoek met een kleur. Laden van een installatiekopie-patroon in de maps-installatiekopie sprite resources en deze vervolgens deze installatiekopie met de `fillPattern` eigenschap van de polygoonlaag.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Het patroon opvulling veelhoek" src="//codepen.io/azuremaps/embed/JzQpYX/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie de Pen <a href='https://codepen.io/azuremaps/pen/JzQpYX/'>veelhoek opvulling patroon</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="customize-a-polygon-layer"></a>Een polygoonlaag aanpassen

De polygoonlaag heeft alleen een aantal opties voor stijl. Hier is een hulpprogramma voor ze kunt uitproberen.

<br/>

<iframe height='700' scrolling='no' title='LXvxpg' src='//codepen.io/azuremaps/embed/LXvxpg/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/LXvxpg/'>LXvxpg</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

<a id="addACircle"></a>

## <a name="add-a-circle-to-the-map"></a>Voeg een cirkel toe aan de kaart

Azure kaarten maakt gebruik van een uitgebreide versie van de GeoJSON-schema waarmee een definitie voor cirkels, zoals vermeld [hier](extend-geojson.md#circle). Een cirkel kan worden weergegeven op de kaart door het maken van een `Point` functie waarvoor een `subType` eigenschap met de waarde `"Circle"` en een `radius` eigenschap met een getal dat de radius in meters. Bijvoorbeeld:

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

Azure Maps Web SDK converteert deze `Pooint` functies in `Polygon` functies op de achtergrond en kunnen worden weergegeven op de kaart met behulp van polygoon- en lijnlagen zoals hier wordt weergegeven.

<iframe height='500' scrolling='no' title='Voeg een cirkel toe aan een kaart' src='//codepen.io/azuremaps/embed/PRmzJX/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>een cirkel toevoegen aan een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste vereiste blok van code in de bovenstaande code wordt een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, object voor een gegevensbron wordt gemaakt met behulp van de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse. Een cirkel wordt een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van [punt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) en heeft een `subType` eigenschap ingesteld op `"Circle"` en een `radius` eigenschapswaarde in meters. Wanneer een punt-functie met een `subType` van `"Circle"` wordt toegevoegd aan een gegevensbron, worden deze omgezet in een circulaire veelhoek binnen de kaart.

Een [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) weergegeven van de gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) op de kaart. Het laatste blok van code wordt gemaakt en wordt een polygoonlaag toegevoegd aan de kaart. Controleer de eigenschappen van een polygoonlaag op [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). De gegevensbron en de polygoonlaag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenis-handler](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen dat de cirkel wordt weergegeven nadat de kaart volledig is geladen.

## <a name="make-a-geometry-easy-to-update"></a>Een geometrie gemakkelijk om bij te werken

Een `Shape` klasse wordt een [geometrie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.geometry?view=azure-iot-typescript-latest) of [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) en maakt het gemakkelijk om te werken en ze zelf onderhouden.
`new Shape(data: Feature<data.Geometry, any>)` vormt een shape-object en initialiseert ze met de opgegeven functie.

<br/>

<iframe height='500' scrolling='no' title='Eigenschappen van de vorm bijwerken' src='//codepen.io/azuremaps/embed/ZqMeQY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/ZqMeQY/'>bijwerken van eigenschappen van de shape</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste vereiste blok van de bovenstaande code wordt een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Een punt is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van [wijst](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) de klasse. Het tweede codeblok initialiseert de radiuswaarde voor de schuifregelaar HTML-element en vervolgens en loopt van een point-object in een [vorm](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) klasse-object.

Het derde codeblok maakt u een functie die wordt de waarde van het HTML-bereik schuifregelaar-element en wijzigt de radiuswaarde met behulp van de Shapeklasse [addProperty](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) methode.

In het vierde blok van code, object voor een gegevensbron wordt gemaakt met behulp van de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) klasse. Het punt wordt vervolgens toegevoegd aan de gegevensbron.

Een [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) weergegeven van de gegevens die zijn ingepakt in de [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) op de kaart. Het derde blok van code maakt een polygoonlaag. Controleer de eigenschappen van een polygoonlaag op [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). De gegevensbron, de gebeurtenis-handler klikken en de polygoonlaag worden gemaakt en toegevoegd aan de kaart in de [gebeurtenis-handler](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) om ervoor te zorgen dat het punt wordt weergegeven nadat de kaart volledig is geladen.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten:

> [!div class="nextstepaction"]
> [Gegevensgestuurde stijl expressies gebruiken](data-driven-style-expressions-web-sdk.md)
