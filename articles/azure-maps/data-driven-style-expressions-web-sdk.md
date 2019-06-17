---
title: Gegevensgestuurde stijl expressies in de Azure Maps Web SDK | Microsoft Docs
description: Het gebruik van gegevensgestuurde stijl expressies in de Azure Maps Web SDK.
author: rbrundritt
ms.author: richbrun
ms.date: 4/4/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.custom: codepen
ms.openlocfilehash: 3b234ca37783fe557baf307f198de9636b06a382
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60904916"
---
# <a name="data-driven-style-expressions-web-sdk"></a>Data-driven Style Expressions (Web SDK)

Expressies kunnen u bedrijfslogica toepassen met opties voor stijl waar u de eigenschappen die zijn gedefinieerd in de vorm van een gegevensbron. Expressies kunnen ook worden gebruikt voor het filteren van gegevens in een gegevensbron of laag. Expressies kunnen bestaan uit voorwaardelijke logica, zoals de if-instructies, en kunnen ook worden gebruikt voor het bewerken van gegevens. tekenreeks, logische en rekenkundige operators. 

Gegevensgestuurde stijlen kunnen verminderen de hoeveelheid code die nodig zijn om bedrijfslogica rond stijl te implementeren. Gebruikt in combinatie met lagen, worden expressies op weergavetijd op een afzonderlijke thread die betere prestaties in vergelijking biedt met het evalueren van bedrijfslogica op de UI-thread geëvalueerd.

De volgende video biedt een overzicht van gegevensgestuurde stijl in de Azure Maps Web SDK.

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Data-Driven-Styling-with-Azure-Maps/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

Expressies worden weergegeven als JSON-matrices. Het eerste element van een expressie in de matrix is een tekenreeks met de naam van de expressie-operator. Bijvoorbeeld, '+' of 'geval'. De volgende elementen (indien aanwezig) zijn de argumenten voor de expressie. Elk argument een letterlijke waarde is (een tekenreeks, getal, Booleaanse waarde, of `null`), of een andere expressie matrix. De volgende pseudocode definieert de basisstructuur van een expressie. 

```javascript
[ 
    expression_operator, 
    argument0, 
    argument1, 
    …
] 
```

Azure Maps Web SDK ondersteunt veel typen expressies die kunnen worden gebruikt op hun eigen of in combinatie met andere expressies.

| Type van expressies | Description |
|---------------------|-------------|
| [Booleaanse expressies](#boolean-expressions) | Booleaanse expressies bevatten een set met Booleaanse operators expressies voor het evalueren van Booleaanse vergelijkingen. |
| [Kleur-expressies](#color-expressions) | Kleur expressies wordt het eenvoudiger maken en manipuleren van kleurwaarden. |
| [Voorwaardelijke expressies](#conditional-expressions) | Voorwaardelijke expressies bevatten logic-bewerkingen, zoals de if-instructies. |
| [Gegevensexpressies](#data-expressions) | Biedt toegang tot de gegevens van de eigenschap een functie. |
| [Interpoleren en stap expressies](#interpolate-and-step-expressions) | Interpoleren en stap expressies kunnen worden gebruikt voor het berekenen van de waarden langs een geïnterpoleerde kromme of stap-functie. |
| [Laag specifieke expressies](#layer-specific-expressions) | Speciale expressies die alleen van toepassing op één laag. |
| [Wiskundige expressies](#math-expressions) | Rekenkundige operatoren voor het uitvoeren van gegevensgestuurde berekeningen in het kader van de expressie bevat. |
| [Expressies voor verbindingsreeksen operator](#string-operator-expressions) | Expressies voor verbindingsreeksen operator uitvoeren conversiebewerkingen op tekenreeksen, zoals samenvoegen en omzetten van de aanvraag. |
| [Type expressies](#type-expressions) | Type expressies over hulpprogramma's voor het testen en te converteren van verschillende gegevenstypen, zoals tekenreeksen, cijfers en Booleaanse waarden. |
| [Expressies voor variabele netwerkverbinding](#variable-binding-expressions) | Variabele netwerkverbinding expressies kunnen de resultaten van een berekening worden opgeslagen in een variabele en waarnaar wordt verwezen elders in een expressie meerdere keren zonder dat de opgeslagen waarde opnieuw berekenen. |
| [De expressie zoomniveau](#zoom-expression) | Hiermee haalt u het huidige zoomniveau van de kaart op weergavetijd. |

Alle voorbeelden in dit document wordt de volgende functie gebruiken om aan te tonen verschillende manieren in die de verschillende typen expressies kunnen worden gebruikt. 

```javascript
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.13284, 47.63699]
    },
    "properties": {     
        "entityType": "restaurant",
        "revenue": 12345,
        "subTitle": "Building 40", 
        "temperature": 72,
        "title": "Cafeteria", 
        "zoneColor": "red"
    }
}
```

## <a name="data-expressions"></a>Gegevensexpressies

Gegevensexpressies bieden toegang tot de gegevens van de eigenschap een functie. 

| expressie | Retourtype | Description |
|------------|-------------|-------------|
| `['at', number, array]` | object | Een item wordt opgehaald uit een matrix. |
| `['geometry-type']` | string | Hiermee wordt opgehaald van de functie geometrie-type: Point, MultiPoint, LineString, MultiLineString, Polygon, MultiPolygon. |
| `['get', string]` | value | Hiermee haalt u de waarde van eigenschap van eigenschappen van de huidige functie. Retourneert null als de aangevraagde eigenschap ontbreekt. |
| `['get', string, object]` | value | Hiermee haalt u de waarde van eigenschap van de eigenschappen van het opgegeven object. Retourneert null als de aangevraagde eigenschap ontbreekt. |
| `['has', string]` | booleaans | Hiermee bepaalt u als de eigenschappen van een functie voor de opgegeven eigenschap. |
| `['has', string, object]` | booleaans | Hiermee bepaalt u als de eigenschappen van het object dat de opgegeven eigenschap. |
| `['id']` | value | Hiermee haalt u de ID van de functie als er een. |
| `['length', string | array]` | getal | Hiermee haalt u de lengte van een tekenreeks of een matrix. |

**Voorbeelden**

Eigenschappen van een functie toegankelijk rechtstreeks in een expressie met behulp van een `get` expressie. Het volgende voorbeeld wordt de waarde 'zoneColor' van de functie om op te geven van de kleureigenschap van de laag van een Bel. 

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: ['get', 'zoneColor'] //Get the zoneColor value.
});
```

Het bovenstaande voorbeeld ook prima als alle functies van het punt de `zoneColor` eigenschap, maar als zij dat niet doet, de kleur wordt waarschijnlijk terugvallen op "black". Wijzig de kleur van de alternatieve een `case` expressie kan worden gebruikt in combinatie met de `has` expressie om te controleren als de eigenschap bestaat, en als er een alternatieve kleur in plaats daarvan wordt niet geretourneerd.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'case', //Use a conditional case expression.

        ['has', 'zoneColor'],   //Check to see if feature has a "zoneColor" property
        ['get', 'zoneColor'],   //If it does, use it.

        'blue'  //If it doesn't, default to blue.
    ]
});
```

Bel en het symbool lagen worden de coördinaten van alle shapes in een gegevensbron wordt standaard weergegeven. Dit kan worden gedaan om te markeren de hoekpunten van een polygoon of de regel. De `filter` optie van de laag kan worden gebruikt om te beperken van het type geometry van de functies voor deze wordt weergegeven met behulp van een `['geometry-type']` expressie in een Booleaanse expressie. In het volgende voorbeeld wordt een bel laag zodat alleen beperkt `Point` functies worden weergegeven.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    filter: ['==', ['geometry-type'], 'Point']
});
```

Het volgende voorbeeld kunt beide `Point` en `MultiPoint` functies moet worden gerenderd. 

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    filter: ['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]
});
```

Op dezelfde manier worden de omtrek van veelhoeken weergegeven in de lagen van de regel. Als u wilt dit gedrag in de laag van een regel uitschakelen, een filter waarmee alleen toevoegen `LineString` en `MultiLineString` functies.  

## <a name="math-expressions"></a>Wiskundige expressies

Wiskundige expressies bevatten rekenkundige operatoren voor het uitvoeren van gegevensgestuurde berekeningen in het kader van de expressie.

| expressie | Retourtype | Description |
|------------|-------------|-------------|
| `['+', number, number, …]` | getal | Berekent de som van de opgegeven getallen. |
| `['-', number]` | getal | Trekt 0 door het opgegeven getal. |
| `['-', number, number]` | getal | Hiermee verwijdert u de eerste getallen door het tweede getal. |
| `['*', number, number, …]` | getal | De opgegeven getallen vermenigvuldigen samen. |
| `['/', number, number]` | getal | Het eerste getal door het tweede getal deelt. |
| `['%', number, number]` | getal | Restwaarde berekenen voor het bij het delen van het eerste getal door het tweede getal. |
| `['^', number, number]` | getal | Berekent de waarde van de eerste waarde verheven tot de macht van het tweede getal. |
| `['abs', number]` | getal | Berekent de absolute waarde van het opgegeven getal. |
| `['acos', number]` | getal | Berekent de arccosinus van het opgegeven getal. |
| `['asin', number]` | getal | Berekent de arcsinus van het opgegeven getal. |
| `['atan', number]` | getal | Berekent de arctangens van het opgegeven getal. |
| `['ceil', number]` | getal | Het getal tot de volgende gehele getal afgerond. |
| `['cos', number]` | getal | Berekent de cosinus van het opgegeven getal. |
| `['e']` | getal | Retourneert de rekenkundige constante `e`. |
| `['floor', number]` | getal | Het getal af naar het vorige gehele getal afgerond. |
| `['ln', number]` | getal | Berekent de natuurlijke logaritme van het opgegeven getal. |
| `['ln2']` | getal | Retourneert de rekenkundige constante `ln(2)`. |
| `['log10', number]` | getal | Berekent de base-10-logaritme van het opgegeven getal. |
| `['log2', number]` | getal | Berekent de base-twee logaritme van het opgegeven getal. |
| `['max', number, number, …]` | getal | Berekent het maximum aantal in de opgegeven groep getallen. |
| `['min', number, number, …]` | getal | Berekent het minimale aantal in de opgegeven groep getallen. |
| `['pi']` | getal | Retourneert de rekenkundige constante `PI`. |
| `['round', number]` | getal | Het nummer op het dichtstbijzijnde gehele getal afgerond. Halverwege waarden zijn afgerond weg van nul. Bijvoorbeeld, `['round', -1.5]` -2 wordt geëvalueerd. |
| `['sin', number]` | getal | Berekent de sinus van het opgegeven getal. |
| `['sqrt', number]` | getal | Berekent de vierkantswortel van het opgegeven getal. |
| `['tan', number]` | getal | Berekent de tangens van het opgegeven getal. |
## <a name="boolean-expressions"></a>Booleaanse expressies

Booleaanse expressies bevatten een set met Booleaanse operators expressies voor het evalueren van Booleaanse vergelijkingen.

Bij het vergelijken van waarden, wordt de vergelijking strikt hebt getypt. Waarden van verschillende typen worden altijd als ongelijk beschouwd. Gevallen waar de typen bekend is dat deze verschillende tijdens het parseren ongeldig worden beschouwd en treedt er een parseringsfout. 

| expressie | Retourtype | Description |
|------------|-------------|-------------|
| `['! ', boolean]` | booleaans | Logische negatie. Retourneert `true` als de invoer `false`, en `false` als de invoer `true`. |
| `['!= ', value, value]` | booleaans | Retourneert `true` als de ingevoerde waarden niet gelijk aan, `false` anders. |
| `['<', value, value]` | booleaans | Retourneert `true` als de eerste invoer strikt kleiner is dan het tweede `false` anders. De argumenten zijn vereist voor zowel tekenreeksen of beide nummers. |
| `['<=', value, value]` | booleaans | Retourneert `true` als de eerste invoer kleiner dan of gelijk zijn aan het tweede is `false` anders. De argumenten zijn vereist voor zowel tekenreeksen of beide nummers. |
| `['==', value, value]` | booleaans | Retourneert `true` als de ingevoerde waarden zijn gelijk aan, `false` anders. De argumenten zijn vereist voor zowel tekenreeksen of beide nummers. |
| `['>', value, value]` | booleaans | Retourneert `true` als de eerste invoer strikt groter is dan het tweede `false` anders. De argumenten zijn vereist voor zowel tekenreeksen of beide nummers. |
| `['>=' value, value]` | booleaans | Retourneert `true` als de eerste invoer groter dan of gelijk aan het tweede is `false` anders. De argumenten zijn vereist voor zowel tekenreeksen of beide nummers. |
| `['all', boolean, boolean, …]` | booleaans | Retourneert `true` als alle invoer `true`, `false` anders. |
| `['any', boolean, boolean, …]` | booleaans | Retourneert `true` als een van de invoer `true`, `false` anders. |

## <a name="conditional-expressions"></a>Voorwaardelijke expressies

Voorwaardelijke expressies bevatten logic-bewerkingen, zoals de if-instructies.

De volgende expressies voorwaardelijke logica bewerkingen uitvoeren op de ingevoerde gegevens. Bijvoorbeeld, de `case` expressie biedt ' als/dan/anders' logische tijdens de `match` expressie is vergelijkbaar met een 'switch-instructie'. 

### <a name="case-expression"></a>Case-expressie

Een `case` expressie is een voorwaardelijke expressie waarmee de if-instructie, zoals logica (als/dan/anders). Dit type expressie doorloopt een lijst met Booleaanse voorwaarden en retourneert de uitvoerwaarde van de eerste Boole-voorwaarde die is ingesteld op true.

De volgende pseudocode bepalen de structuur van de `case` expressie. 

```javascript
[
    'case',
    condition1: boolean, 
    output1: value,
    condition2: boolean, 
    output2: value,
    ...,
    fallback: value
]
```

**Voorbeeld**

Het volgende voorbeeld wordt stapsgewijs uitgelegd verschillende Booleaanse voorwaarden totdat er een wordt gevonden die wordt geëvalueerd als `true`, en vervolgens retourneert die waarde die is gekoppeld. Als er geen Boole-voorwaarde wordt geëvalueerd als `true`, een alternatieve waarde wordt geretourneerd. 

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'case',

        //Check to see if the first boolean expression is true, and if it is, return its assigned result.
        ['has', 'zoneColor'],
        ['get', 'zoneColor'],

        //Check to see if the second boolean expression is true, and if it is, return its assigned result.
        ['all', ['has', ' temperature '], ['>', ['get', 'temperature'], 100]],
        'red',

        //Specify a default value to return.
        'green'
    ]
});
```

### <a name="match-expression"></a>Match-expressie

Een `match` expressie is een voorwaardelijke expressie waarmee de switch-instructie, zoals logica. De invoer kan bestaan uit een expressie zoals `['get', 'entityType']` die retourneert een tekenreeks of een getal. Elk label moet één letterlijke waarde of een matrix van letterlijke waarden, waarvan de waarden alle tekenreeksen of alle getallen moet. De invoer komt overeen met als een van de waarden in de overeenkomst met de matrix. Elk label moet uniek zijn. Als het invoertype komt niet overeen met het type van de labels, wordt het resultaat de alternatieve waarde zijn.

De volgende pseudocode bepalen de structuur van de `match` expressie. 

```javascript
[
    'match',
    input: number | string,
    label1: number | string | (number | string)[], 
    output1: value,
    label2: number | string | (number | string)[], 
    output2: value,
    ...,
    fallback: value
]
```

**Voorbeelden**

Het volgende voorbeeld wordt gekeken de `entityType` eigenschap van een punt-functie in een laag bel wordt gezocht naar een overeenkomst. Als er een overeenkomst wordt gevonden, die de opgegeven waarde wordt geretourneerd of wordt de alternatieve waarde geretourneerd.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'match',

        //Get the property to match.
        ['get', 'entityType'],

        //List the values to match and the result to return for each match.
        'restaurant', 'red',
        'park', 'green',

        //Specify a default value to return if no match is found.
        'black'
    ]
});
```

Het volgende voorbeeld wordt een matrix om een reeks labels die moeten allemaal hetzelfde resultaat weer te geven. Dit is veel efficiënter dan afzonderlijk lijst van elk label. In dit geval als de `entityType` eigenschap "restaurant" of "grocery_store" is, worden de kleur "rood" geretourneerd.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'match',

        //Get the property to match.
        ['get', 'entityType'],

        //List the values to match and the result to return for each match.
        ['restaurant', 'grocery_store'], 'red',

        'park', 'green',

        //Specify a default value to return if no match is found.
        'black'
    ]
});
```

### <a name="coalesce-expression"></a>Expressie samenvoegen

Een `coalesce` expressie stappen via een set expressies totdat de eerste niet-null-waarde wordt verkregen en die waarde retourneert. 

De volgende pseudocode bepalen de structuur van de `coalesce` expressie. 

```javascript
[
    'coalesce', 
    value1, 
    value2, 
    …
]
```

**Voorbeeld**

Het volgende voorbeeld wordt een `coalesce` expressie om in te stellen de `textField` optie van een laag symbool. Als de `title` eigenschap ontbreekt in de functie of is ingesteld op `null`, de expressie probeert vervolgens zoekt de `subtitle` eigenschap als de ontbrekende of `null`, het wordt terugvallen op een lege tekenreeks. 

```javascript
var layer = new atlas.layer.SymbolLayer(datasource, null, {
    textOptions: {
        textField: [
            'coalesce',

            //Try getting the title property.
            ['get', 'title'],

            //If there is no title, try getting the subtitle. 
            ['get', 'subtitle'],

            //Default to an empty string.
            ''
        ]
    }
});
```

## <a name="type-expressions"></a>Type expressies

Type expressies over hulpprogramma's voor het testen en te converteren van verschillende gegevenstypen, zoals tekenreeksen, cijfers en Booleaanse waarden.

| expressie | Retourtype | Description |
|------------|-------------|-------------|
| `['literal', array]`<br/><br/>`['literal', object]` | matrix \| object | Retourneert een letterlijke waarde van de matrix of één object. Deze expressie gebruiken om te voorkomen dat een matrix of een object wordt geëvalueerd als een expressie. Dit is nodig als een matrix of een object moet worden geretourneerd door een expressie. |
| `['to-boolean', value]` | booleaans | De invoerwaarde converteert naar een Booleaanse waarde. Het resultaat is `false` wanneer de invoer een lege tekenreeks is `0`, `false`, `null`, of `NaN`; anders de `true`. |
| `['to-color', value]`<br/><br/>`['to-color', value1, value2…]` | color | De invoerwaarde converteert naar een kleur. Als er meerdere waarden worden opgegeven, wordt elk criterium op volgorde geëvalueerd totdat de eerste geslaagde conversie wordt verkregen. Als geen van de invoer kan worden geconverteerd, wordt in de expressie een fout is. |
| `['to-number', value]`<br/><br/>`['to-number', value1, value2, …]` | getal | Converteert de invoerwaarde naar een getal, indien mogelijk. Als de invoer `null` of `false`, het resultaat is 0. Als de invoer `true`, het resultaat is 1. Als de invoer een tekenreeks is, wordt deze geconverteerd naar een getal met de [ToNumber](https://tc39.github.io/ecma262/#sec-tonumber-applied-to-the-string-type) functie van de ECMAScript Language Specification. Als er meerdere waarden worden opgegeven, wordt elk criterium op volgorde geëvalueerd totdat de eerste geslaagde conversie wordt verkregen. Als geen van de invoer kan worden geconverteerd, wordt in de expressie een fout is. |
| `['to-string', value]` | string | De invoerwaarde converteert naar een tekenreeks. Als de invoer `null`, het resultaat is `""`. Als de invoer een Booleaanse waarde is, is het resultaat `"true"` of `"false"`. Als de invoer een getal is, wordt deze geconverteerd naar een tekenreeks met de [ToString](https://tc39.github.io/ecma262/#sec-tostring-applied-to-the-number-type) functie van de ECMAScript Language Specification getal. Als de invoer een kleur is, wordt deze geconverteerd naar RGBA CSS-kleurenreeks `"rgba(r,g,b,a)"`. Anders wordt de invoer wordt geconverteerd naar een tekenreeks met de [JSON.stringify](https://tc39.github.io/ecma262/#sec-json.stringify) functie van de ECMAScript Language Specification. |
| `['typeof', value]` | string | Retourneert een tekenreeks met een beschrijving van het type van de opgegeven waarde. |

> [!TIP]
> Als een foutbericht weergegeven die vergelijkbaar is met `Expression name must be a string, but found number instead. If you wanted a literal array, use ["literal", [...]].` wordt weergegeven in de browserconsole dit betekent dat er een expressie ergens in uw code die een matrix die geen een tekenreeks op voor de eerste waarde heeft. Als u de expressie wilt retourneert een matrix, verpakken de matrix met de `literal` expressie. Het volgende voorbeeld wordt het pictogram `offset` optie van een symbool-laag, die worden van een matrix met twee getallen moet met behulp van een `match` expressie om te kiezen tussen twee offsetwaarden op basis van de waarde van de `entityType` eigenschap van het punt functie.
>
> ```javascript
> var layer = new atlas.layer.SymbolLayer(datasource, null, {
>     iconOptions: {
>         offset: [
>             'match',
>
>             //Get the entityType value.
>             ['get', 'entityType'],
>
>             //If there is no title, try getting the subtitle. 
>             'restaurant', ['literal', [0, -10]],
>
>             //Default to value.
>             ['literal', [0, 0]]
>         ]
>     }
> });
> ```

## <a name="color-expressions"></a>Kleur-expressies

Kleur expressies wordt het eenvoudiger maken en manipuleren van kleurwaarden.

| expressie | Retourtype | Description |
|------------|-------------|-------------|
| `['rgb', number, number, number]` | color | Hiermee maakt u een kleurwaarde van *rode*, *groen*, en *blauwe* onderdelen die tussen liggen moeten `0` en `255`, en een alpha-component van `1`. Als een onderdeel buiten het bereik is, is de expressie een fout. |
| `['rgba', number, number, number, number]` | color | Hiermee maakt u een kleurwaarde van *rode*, *groen*, *blauw* onderdelen die tussen liggen moeten `0` en `255`, en binnen een bereik van een alpha-component `0` en `1`. Als een onderdeel buiten het bereik is, is de expressie een fout. |
| `['to-rgba']` | \[aantal, aantal, aantal, aantal\] | Retourneert een matrix van vier-elementen met de invoerkleur *rode*, *groen*, *blauwe*, en *alpha* onderdelen, in die volgorde. |

**Voorbeeld**

Het volgende voorbeeld wordt gemaakt en de RGB-waarde die is een *rode* waarde van `255`, en *groen* en *blauwe* waarden die worden berekend door het vermenigvuldigen`2.5` door de waarde van de `temperature` eigenschap. Als de temperatuurwijzigingen de kleur wordt gewijzigd in verschillende tinten van *rode*.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'rgb', //Create a RGB color value.

        255,    //Set red value to 255.

        ['*', 2.5, ['get', 'temperature']], //Multiple the temperature by 2.5 and set the green value.

        ['*', 2.5, ['get', 'temperature']]  //Multiple the temperature by 2.5 and set the blue value.
    ]
});
```

## <a name="string-operator-expressions"></a>Expressies voor verbindingsreeksen operator

Expressies voor verbindingsreeksen operator uitvoeren conversiebewerkingen op tekenreeksen, zoals samenvoegen en omzetten van de aanvraag. 

| expressie | Retourtype | Description |
|------------|-------------|-------------|
| `['concat', string, string, …]` | string | Meerdere tekenreeksen samen worden samengevoegd. Elke waarde moet een tekenreeks zijn. Gebruik de `to-string` expressie andere waardetypen converteren naar tekenreeks, indien nodig. |
| `['downcase', string]` | string | De opgegeven tekenreeks converteren naar kleine letters. |
| `['upcase', string]` | string | Converteert de opgegeven tekenreeks naar hoofdletters. |

**Voorbeeld**

Het volgende voorbeeld wordt geconverteerd naar de `temperature` eigenschap van het punt presenteren in een tekenreeks en vervolgens worden samengevoegd in '° F' aan het einde van deze.

```javascript
var layer = new atlas.layer.SymbolLayer(datasource, null, {
    textOptions: {
        textField: ['concat', ['to-string', ['get', 'temperature']], '°F'],

        //Some additional style options.
        offset: [0, -1.5],
        size: 12,
        color: 'white'
    }
});
```

De bovenstaande expressie een pincode op de kaart met de tekst "64° F' overlapt daarboven zoals wordt weergegeven in de onderstaande afbeelding wordt weergegeven.

<center>

![Voorbeeld van een tekenreeks operator expressie](media/how-to-expressions/string-operator-expression.png) </center>

## <a name="interpolate-and-step-expressions"></a>Interpoleren en stap expressies

Interpoleren en stap expressies kunnen worden gebruikt voor het berekenen van de waarden langs een geïnterpoleerde kromme of stap-functie. Deze expressies worden in een expressie die een numerieke waarde bijvoorbeeld als de invoer retourneert `['get',  'temperature']`. De invoerwaarde is geëvalueerd op basis van combinaties van waarden voor invoer en uitvoer, met de naam 'gestopt', om te bepalen van de waarde die het beste past bij de geïnterpoleerde kromme of stap-functie. De ingevoerde waarden voor elke stop moeten een getal zijn en moet in oplopende volgorde. De uitvoerwaarden moet een getal en matrix met getallen of een kleur.

### <a name="interpolate-expression"></a>Expressie interpoleren

Een `interpolate` expressie kan worden gebruikt voor het berekenen van een continue, goede set waarden door de interpolatie tussen stop-waarden. Een `interpolate` expressie die waarden retourneert produceert een kleurverloop in dat er waarden worden geselecteerd uit.

Er zijn drie soorten interpolatiemethoden die kunnen worden gebruikt in een `interpolate` expressie:
 
* `['linear']` -Geïnterpoleerd lineair toe tussen de twee stopt.
* `['exponential', base]` -Geïnterpoleerd exponentieel tussen de stopt. De `base` waarde bepaalt de snelheid waarmee de uitvoer wordt verhoogd. Hogere waarden moeten u de uitvoer meer verhogen naar het hoge einde van het bereik. Een `base` waarde dicht bij 1 produceert een uitvoer die meer lineair worden vergroot.
* `['cubic-bezier', x1, y1, x2, y2]` -Geïnterpoleerd met behulp van een [kubieke Bézier-curve](https://developer.mozilla.org/docs/Web/CSS/timing-function) gedefinieerd door de opgegeven controlepunten.

Hier volgt een voorbeeld van hoe deze verschillende typen interpolations eruit zien. 

| Lineair  | Exponentieel | Kubieke Bézier |
|---------|-------------|--------------|
| ![Lineaire interpolatie graph](media/how-to-expressions/linear-interpolation.png) | ![Exponentiële interpolatie graph](media/how-to-expressions/exponential-interpolation.png) | ![Kubieke Bézier interpolatie graph](media/how-to-expressions/bezier-curve-interpolation.png) |

De volgende pseudocode bepalen de structuur van de `interpolate` expressie. 

```javascript
[
    'interpolate',
    interpolation: ['linear'] | ['exponential', base] | ['cubic-bezier', x1, y1, x2, y2],
    input: number,
    stopInput1: number, 
    stopOutput1: value1,
    stopInput2: number, 
    stopOutput2: value2, 
    ...
]
```

**Voorbeeld**

Het volgende voorbeeld wordt een `linear interpolate` expressie om in te stellen de `color` eigenschap van een laag Bel op basis van de `temperature` eigenschap van de functie punt. Als de `temperature` is minder dan 60, 'blauw', worden geretourneerd als tussen 60 en minder dan 70, geel wordt geretourneerd, als tussen 70 en minder dan 80, "oranje" wordt geretourneerd, als 80 of hoger, "rood" wordt geretourneerd.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'interpolate',
        ['linear'],
        ['get', 'temperature'],
        50,        
        'blue',
        60,
        'yellow',
        70,
        'orange',
        80,
        'red'
    ]
});
```

De volgende afbeelding ziet u hoe de kleuren voor de bovenstaande expressie worden gekozen.
 
<center>

![Voorbeeld van een expressie interpoleren](media/how-to-expressions/interpolate-expression-example.png) </center>

### <a name="step-expression"></a>Step-expressie

Een `step` expressie kan worden gebruikt voor het berekenen van discrete, getrapte resultaatwaarden door het evalueren van een [piecewise constante functie](http://mathworld.wolfram.com/PiecewiseConstantFunction.html) gedefinieerd door stopt. 

De volgende pseudocode bepalen de structuur van de `step` expressie. 

```javascript
[
    'step',
    input: number,
    output0: value0,
    stop1: number, 
    output1: value1,
    stop2: number, 
    output2: value2, 
    ...
]
```

Stap expressies retourneert de uitvoerwaarde van de stoppen voordat de invoerwaarde of de eerste invoerwaarde als de invoer is kleiner dan de eerste stop. 

**Voorbeeld**

Het volgende voorbeeld wordt een `step` expressie om in te stellen de `color` eigenschap van een laag Bel op basis van de `temperature` eigenschap van de functie punt. Als de `temperature` is minder dan 60, 'blauw', worden geretourneerd als tussen 60 en minder dan 70, "yellow" wordt geretourneerd, als tussen 70 en minder dan 80, "oranje" wordt geretourneerd, als 80 of hoger, "rood" wordt geretourneerd.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'step',
        ['get', 'temperature'],
        'blue',
        60,
        'yellow',
        70,
        'orange',
        80,
        'red'
    ]
});
```

De volgende afbeelding ziet u hoe de kleuren voor de bovenstaande expressie worden gekozen.
 
<center>

![Voorbeeld van een expressie stap](media/how-to-expressions/step-expression-example.png)
</center>

## <a name="layer-specific-expressions"></a>Laag specifieke expressies

Speciale expressies die alleen van toepassing op bepaalde lagen.

### <a name="heat-map-density-expression"></a>Warmte-expressie voor documentstructuur dichtheid

Een expressie heat map dichtheid heatmap kaart dichtheid waarde voor elke pixel in een kaartLaag heatmap opgehaald en wordt gedefinieerd als `['heatmap-density']`. Deze waarde is een getal tussen `0` en `1` en wordt gebruikt in combinatie met een `interpolation` of `step` expressie voor het definiëren van de kleurovergang voor de kleur waarmee inkleuren van de heatmap. Deze expressie kan alleen worden gebruikt de [optie kleur](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest#color) van de kaartLaag heatmap.

> [!TIP]
> De kleur bij index 0 in een expressie interpolatie of de standaardkleur van een kleur stap definieert u de kleur van het gebied waar er geen gegevens zijn en kan worden gebruikt voor het definiëren van een achtergrondkleur. Veel liever deze waarde transparant of een semi-transparant zwart in te stellen. 

**Voorbeeld**

Dit voorbeeld wordt een voering interpolatie-expressie om te maken van een soepele kleurverloop voor het renderen van de heatmap. 

```javascript 
var layer = new atlas.layer.HeatMapLayer(datasource, null, {
    color: [
        'interpolate',
        ['linear'],
        ['heatmap-density'],
        0, 'transparent',
        0.01, 'purple',
        0.5, '#fb00fb',
        1, '#00c3ff'
    ]
});
```

Naast het gebruik van een soepel verloop naar een heatmap inkleuren, kleuren kunnen worden opgegeven in een set bereiken met behulp van een `step` expressie. Met behulp van een `step` -expressie voor het inkleuren van de heatmap breekt de dichtheid visueel in bereiken die meer lijkt op een kaart contour of radardiagram stijl.  

```javascript 
var layer = new atlas.layer.HeatMapLayer(datasource, null, {
    color: [
        'step',
        ['heatmap-density'],
        'transparent',
        0.01, 'navy',
        0.25, 'navy',
        0.5, 'green',
        0.75, 'yellow',
        1, 'red'
    ]
});
```

Zie voor meer informatie de [toevoegen van een kaartLaag heatmap](map-add-heat-map-layer.md) documentatie.

### <a name="line-progress-expression"></a>Expressie voor regel wordt uitgevoerd

Een expressie voor een regel wordt uitgevoerd de voortgang langs een lijn met kleurovergang in een lijnlaag opgehaald en wordt gedefinieerd als `['line-progress']`. Deze waarde een getal tussen 0 en 1 is en wordt gebruikt in combinatie met een `interpolation` of `step` expressie. Deze expressie kan alleen worden gebruikt met de [strokeGradient optie]( https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest#strokegradient) van de regel-laag. 

> [!NOTE]
> De `strokeGradient` optie van de regel-laag moet de `lineMetrics` optie van de gegevensbron moet worden ingesteld op `true`.

**Voorbeeld**

Het volgende voorbeeld wordt de `['line-progress']` expressie voor een kleurovergang voor de kleur van toepassing op de lijn van een regel.

```javascript
var layer = new atlas.layer.LineLayer(datasource, null, {
    strokeGradient: [
        'interpolate',
        ['linear'],
        ['line-progress'],
        0, "blue",
        0.1, "royalblue",
        0.3, "cyan",
        0.5, "lime",
        0.7, "yellow",
        1, "red"
    ]
});
```

[Bekijk live voorbeeld](map-add-shape.md#line-stroke-gradient)

### <a name="text-field-format-expression"></a>Expressie voor opmaak van tekst-veld

De expressie voor veld indeling kan worden gebruikt met de `textField` optie van de lagen symbool `textOptions` eigenschap voor gemengde tekst met opmaak. Deze expressie kan een set van invoer tekenreeksen en opmaakopties worden opgegeven. De volgende opties kunnen worden opgegeven voor elke invoerreeks in deze expressie.

 * `'font-scale'` -Hiermee geeft u de schaalfactor voor de tekengrootte. Indien opgegeven, wordt deze waarde overschrijft de `size` eigenschap van de `textOptions` voor de afzonderlijke tekenreeks.
 * `'text-font'` -Hiermee geeft u een of meer lettertypefamilies die moeten worden gebruikt voor deze tekenreeks. Indien opgegeven, wordt deze waarde overschrijft de `font` eigenschap van de `textOptions` voor de afzonderlijke tekenreeks.

De volgende pseudocode bepalen de structuur van de expressie voor veld-indeling. 

```javascript
[
    'format', 
    input1: string, 
    options1: { 
        'font-scale': number, 
        'text-font': string[] 
    },
    input2: string, 
    options2: { 
        'font-scale': number, 
        'text-font': string[] 
    },
    …
]
```

**Voorbeeld**

Het volgende voorbeeld wordt het tekstveld door het toevoegen van een vet lettertype en schalen van de tekengrootte van de `title` eigenschap van de functie. In dit voorbeeld wordt ook de `subtitle` eigenschap van de functie op een nieuwe regel, met een schaal tekengrootte verkleinen.

```javascript
var layer = new atlas.layer.SymbolLayer(datasource, null, {
    textOptions: {
        textField: [
            'format',

            //Bold the title property and scale its font size up.
            ['get', 'title'],
            {
                'text-font': ['literal', ['StandardFont-Bold']],
                'font-scale': 1.25
            },

            '\n', {},   //Add a new line without any formatting.

            //Scale the font size down of the subtitle property. 
            ['get', 'subtitle'],
            { 'font-scale': 0.75 }
        ]
    }
});
```

Deze laag worden de functie punt zoals wordt weergegeven in de onderstaande afbeelding weergegeven:
 
<center>

![Afbeelding van punt-functie met opgemaakte tekstveld](media/how-to-expressions/text-field-format-expression.png) </center>

### <a name="number-format-expression"></a>Numerieke expressie voor opmaak

De `number-format` expressie kan alleen worden gebruikt met de `textField` optie van een laag symbool. Het opgegeven aantal converteert deze expressie naar een opgemaakte tekenreeks. Deze expressie loopt van JavaScript [Number.toLocalString](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString) functioneren en ondersteunt de volgende opties.

 * `locale` -Geef deze optie voor het omzetten van getallen in tekenreeksen op een manier die met de opgegeven taal overeenstemt. Geeft een [BCP-47-taalcode](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Intl#Locale_identification_and_negotiation) in deze optie.
 * `currency` -Voor het aantal converteren naar een tekenreeks voor een valuta. Mogelijke waarden zijn de [ISO 4217 valuatacodes](https://en.wikipedia.org/wiki/ISO_4217), zoals 'USD' voor de Amerikaanse dollar, "EUR' voor de euro, of 'CNY' voor de RMB voor Chinese.
 * `'min-fraction-digits'` -Hiermee geeft u het minimale aantal aan te nemen in de tekenreeksversie van het aantal decimalen.
 * `'max-fraction-digits'` -Hiermee geeft u het maximum aantal te nemen in de tekenreeksversie van het aantal decimalen.

De volgende pseudocode bepalen de structuur van de expressie voor veld-indeling. 

```javascript
[
    'number-format', 
    input: number, 
    options: {
        locale: string, 
        currency: string, 
        'min-fraction-digits': number, 
        'max-fraction-digits': number
    }
]
```

**Voorbeeld**

Het volgende voorbeeld wordt een `number-format` expressie om te wijzigen hoe de `revenue` eigenschap van de functie punt wordt weergegeven in de `textField` optie van een symbool laag zodanig dat deze een Amerikaanse dollar-waarde wordt weergegeven.

```javascript
var layer = new atlas.layer.SymbolLayer(datasource, null, {
    textOptions: {
        textField: [
            'number-format', 
            ['get', 'revenue'], 
            { ‘currency’: 'USD' }
        ],

        offset: [0, 0.75]
    }
});
```

Deze laag worden de functie punt zoals wordt weergegeven in de onderstaande afbeelding weergegeven:

<center>

![Aantal voorbeeld van een expressie indeling](media/how-to-expressions/number-format-expression.png) </center>

## <a name="zoom-expression"></a>De expressie zoomniveau

Een `zoom` expressie wordt gebruikt om op te halen van het huidige zoomniveau van de kaart op rendertijd en wordt gedefinieerd als `['zoom']`. Deze expressie retourneert een getal tussen de minimum- en uitzoomen op bereik van de kaart. Deze expressie kunt stijlen dynamisch worden gewijzigd omdat het zoomniveau van de kaart is gewijzigd. De `zoom` expressie kan alleen worden gebruikt met `interpolate` en `step` expressies.

**Voorbeeld**

Het bepalen van gegevenspunten weergegeven in de heatmap kaartLaag hebben standaard een vaste pixelradius voor alle zoomniveaus. Als het zoomniveau van de kaart is de statistische gegevens bij elkaar en de kaartLaag heatmap er anders uitziet. Een `zoom` expressie kan worden gebruikt voor het schalen van de radius voor elke zoomniveau zodat elk gegevenspunt bevat informatie over hetzelfde fysieke gebied van de kaart. Dit maakt de heatmap kaartLaag zoeken naar meer statische en consistent. Elke zoomniveau van de kaart heeft twee keer zoveel pixels horizontaal en verticaal als het vorige zoomniveau. De radius schalen zodat deze verdubbeld met elke zoomniveau maakt een heatmap die consistent op alle zoomniveaus. Dit kan worden bewerkstelligd met behulp van de `zoom` expressie met een `base 2 exponential interpolation` expressie zoals hieronder wordt weergegeven. 

```javascript 
var layer = new atlas.layer.HeatMapLayer(datasource, null, {
    radius: [
        'interpolate',
        ['exponential', 2],
        ['zoom'],
        
        //For zoom level 1 set the radius to 2 pixels.
        10, 2,

        //Between zoom level 1 and 19, exponentially scale the radius from 2 pixels to 10,000 pixels.
        19, 10000
    ]
};
```

[Bekijk live voorbeeld](map-add-heat-map-layer.md#consistent-zoomable-heat-map)

## <a name="variable-binding-expressions"></a>Expressies voor variabele netwerkverbinding

Expressies voor variabele netwerkverbinding opslaan de resultaten van een berekening in een variabele, zodat deze kan worden verwezen elders in een expressie meerdere keren zonder deze opnieuw te berekenen. Dit is een nuttig optimalisatie voor expressies die betrekking hebben op veel berekeningen

| expressie | Retourtype | Description |
|--------------|---------------|--------------|
| \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'laat'<br/>&nbsp;&nbsp;&nbsp;&nbsp;Name1: tekenreeks<br/>&nbsp;&nbsp;&nbsp;&nbsp;Value1: alle,<br/>&nbsp;&nbsp;&nbsp;&nbsp;Name2: tekenreeks<br/>&nbsp;&nbsp;&nbsp;&nbsp;Value2: alle,<br/>&nbsp;&nbsp;&nbsp;&nbsp;…<br/>&nbsp;&nbsp;&nbsp;&nbsp;childExpression<br/>\] | | Een of meer waarden worden opgeslagen als variabelen voor gebruik door de `var` expressie in de onderliggende-expressie die het resultaat wordt geretourneerd. |
| `['var', name: string]` | Alle | Verwijst naar een variabele die is gemaakt met de `let` expressie. |

**Voorbeeld**

In dit voorbeeld wordt een expressie die wordt berekend van de omzet ten opzichte van de verhouding tussen de temperatuur en wordt vervolgens een `case` expressie om te evalueren verschillende Booleaanse bewerkingen op deze waarde. De `let` expressie wordt gebruikt voor het opslaan van de opbrengst ten opzichte van de temperatuur verhouding zodat deze hoeft slechts één keer worden berekend en de `var` expressie verwijst naar deze variabele zo vaak als nodig is zonder deze opnieuw te berekenen.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        //Divide the point features `revenue` property by the `temperature` property and store it in a variable called `ratio`.
        'let', 'ratio', ['/', ['get', 'revenue'], ['get', 'temperature']],
        //Evaluate the child expression in which the stored variable will be used.
        [
            'case',

            //Check to see if the ratio is less than 100, return 'red'.
            ['<', ['var', 'ratio'], 100],
            'red',

            //Check to see if the ratio is less than 200, return 'green'.
            ['<', ['var', 'ratio'], 200],
            'green',

            //Return `blue` for values greater or equal to 200.
            'blue'
        ]
    ]
});
```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer voorbeelden van code die expressies implementeren:

> [!div class="nextstepaction"] 
> [Een symbool laag toevoegen](map-add-pin.md)

> [!div class="nextstepaction"] 
> [Een bellendiagram laag toevoegen](map-add-bubble-layer.md)

> [!div class="nextstepaction"] 
> [Vormen toevoegen](map-add-shape.md)

> [!div class="nextstepaction"] 
> [Een kaartLaag heatmap toevoegen](map-add-heat-map-layer.md)

Meer informatie over de opties voor tegellaag die ondersteuning bieden voor expressies:

> [!div class="nextstepaction"] 
> [BubbleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.bubblelayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [HeatMapLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [LineLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest) 
