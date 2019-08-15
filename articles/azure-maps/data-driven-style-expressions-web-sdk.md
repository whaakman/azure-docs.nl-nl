---
title: Gegevensgestuurde stijl expressies in de Azure Maps Web-SDK | Microsoft Docs
description: Het gebruik van gegevensgestuurde stijl expressies in de Azure Maps Web-SDK.
author: rbrundritt
ms.author: richbrun
ms.date: 4/4/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.custom: codepen
ms.openlocfilehash: 507af54b8b4c2e7c67538a1a25a040c7ee5fdfd5
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976322"
---
# <a name="data-driven-style-expressions-web-sdk"></a>Gegevensgestuurde stijl expressies (Web SDK)

Met expressies kunt u bedrijfs logica Toep assen op Opmaak opties die de eigenschappen zien die in elke vorm in een gegevens bron zijn gedefinieerd. Expressies kunnen ook worden gebruikt voor het filteren van gegevens in een gegevens bron of-laag. Expressies kunnen bestaan uit voorwaardelijke logica, zoals if-instructies, en kunnen ook worden gebruikt om gegevens te bewerken met; teken reeks, logische en wiskundige Opera tors. 

Gegevensgestuurde stijlen kunnen de hoeveelheid code verminderen die nodig is voor het implementeren van bedrijfs logica rond stijlen. Bij gebruik met lagen worden expressies geëvalueerd op basis van de weergave tijd op een afzonderlijke thread die betere prestaties levert in vergelijking met het evalueren van bedrijfs logica op de UI-thread.

De volgende video biedt een overzicht van gegevensgestuurde stijlen in de Azure Maps Web-SDK.

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Data-Driven-Styling-with-Azure-Maps/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

Expressies worden weer gegeven als JSON-matrices. Het eerste element van een expressie in de matrix is een teken reeks die de naam van de expressie operator opgeeft. Bijvoorbeeld, "+" of "case". De volgende elementen (indien aanwezig) zijn de argumenten voor de expressie. Elk argument is een letterlijke waarde (een teken reeks, getal, Booleaans of `null`) of een andere expressie matrix. De volgende pseudocode definieert de basis structuur van een expressie. 

```javascript
[ 
    expression_operator, 
    argument0, 
    argument1, 
    …
] 
```

De Azure Maps Web-SDK ondersteunt veel typen die kunnen worden gebruikt in hun eigen of in combi natie met andere expressies.

| Type expressies | Description |
|---------------------|-------------|
| [Statistische expressie](#aggregate-expression) | Een expressie die een berekening definieert die wordt verwerkt via een set gegevens en kan worden gebruikt met de `clusterProperties` optie a. `DataSource` |
| [Booleaanse expressies](#boolean-expressions) | Boole-expressies bieden een set Booleaanse Opera tors voor het evalueren van Boole-vergelijkingen. |
| [Kleur expressies](#color-expressions) | Kleur expressies maken het gemakkelijker om kleur waarden te maken en te bewerken. |
| [Voorwaardelijke expressies](#conditional-expressions) | Voorwaardelijke expressies bieden logische bewerkingen, zoals if-instructies. |
| [Gegevens expressies](#data-expressions) | Biedt toegang tot de eigenschaps gegevens in een functie. |
| [Interpoleer-en Step-expressies](#interpolate-and-step-expressions) | Interpolatie-en stap expressies kunnen worden gebruikt voor het berekenen van waarden in een geïnterpoleerde curve of stap functie. |
| [Laag-specifieke expressies](#layer-specific-expressions) | Speciale expressies die alleen van toepassing zijn op één laag. |
| [Wiskundige expressies](#math-expressions) | Biedt wiskundige Opera tors voor het uitvoeren van gegevensgestuurde berekeningen in het Framework van de expressie. |
| [Teken reeks operator expressies](#string-operator-expressions) | Teken reeks operator expressies voeren conversie bewerkingen uit op teken reeksen, zoals samen voegen en het converteren van de aanvraag. |
| [Type-expressies](#type-expressions) | Type-expressies bieden hulpprogram ma's voor het testen en omzetten van verschillende gegevens typen, zoals teken reeksen, getallen en Booleaanse waarden. |
| [Variabele bindings expressies](#variable-binding-expressions) | Met variabelen bindings expressies kunnen de resultaten van een berekening worden opgeslagen in een variabele en ergens anders in een expressie worden verwezen zonder dat de opgeslagen waarde opnieuw moet worden berekend. |
| [Expressie voor in-/uitzoomen](#zoom-expression) | Hiermee wordt het huidige zoom niveau van de kaart op de weergave tijd opgehaald. |

In alle voor beelden in dit document wordt de volgende functie gebruikt om verschillende manieren te tonen waarop de verschillende typen expressies kunnen worden gebruikt. 

```javascript
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.13284, 47.63699]
    },
    "properties": { 
        "id": 123,
        "entityType": "restaurant",
        "revenue": 12345,
        "subTitle": "Building 40", 
        "temperature": 72,
        "title": "Cafeteria", 
        "zoneColor": "red"
    }
}
```

## <a name="data-expressions"></a>Gegevens expressies

Gegevens expressies bieden toegang tot de eigenschaps gegevens in een functie. 

| Expressie | Retour type | Description |
|------------|-------------|-------------|
| `['at', number, array]` | object | Hiermee wordt een item uit een matrix opgehaald. |
| `['geometry-type']` | string | Hiermee wordt het type geometrie van de functie opgehaald: Point, multi point, Lines Tring, multi line String, veelhoek, multiveelhoek. |
| `['get', string]` | value | Hiermee wordt de eigenschaps waarde opgehaald uit de eigenschappen van de huidige functie. Retourneert null als de aangevraagde eigenschap ontbreekt. |
| `['get', string, object]` | value | Hiermee wordt de eigenschaps waarde opgehaald uit de eigenschappen van het gegeven object. Retourneert null als de aangevraagde eigenschap ontbreekt. |
| `['has', string]` | boolean | Hiermee wordt bepaald of de eigenschappen van een functie de opgegeven eigenschap hebben. |
| `['has', string, object]` | boolean | Hiermee wordt bepaald of de eigenschappen van het object de opgegeven eigenschap hebben. |
| `['id']` | value | Hiermee haalt u de ID van de functie op als deze een bevat. |
| `['length', string | array]` | nummer | Hiermee wordt de lengte van een teken reeks of matrix opgehaald. |

**Voorbeelden**

Eigenschappen van een functie kunnen rechtstreeks in een expressie worden geopend met behulp `get` van een expressie. In het volgende voor beeld wordt de waarde ' zoneColor ' van de functie gebruikt om de eigenschap Color van een Bubble Layer op te geven. 

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: ['get', 'zoneColor'] //Get the zoneColor value.
});
```

Het bovenstaande voor beeld werkt prima als alle punt functies de `zoneColor` eigenschap hebben, maar als dat niet het geval is, zal de kleur waarschijnlijk terugvallen op Black. Als u de terugval kleur wilt wijzigen `case` , kunt u een expressie gebruiken in combi `has` natie met de expressie om te controleren of de eigenschap bestaat en of het niet mogelijk is om in plaats daarvan een terugval kleur te retour neren.

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

Met bellen-en symbool lagen worden standaard de coördinaten van alle shapes in een gegevens bron weer gegeven. Dit kan worden gedaan om de hoek punten van een veelhoek of lijn te markeren. De `filter` optie van de laag kan worden gebruikt om het type geometrie te beperken van de functies die worden weer gegeven met `['geometry-type']` behulp van een expressie binnen een Boole-expressie. In het volgende voor beeld wordt een Bubble laag beperkt `Point` zodat alleen de functies worden weer gegeven.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    filter: ['==', ['geometry-type'], 'Point']
});
```

In het volgende voor beeld kunnen `Point` beide `MultiPoint` en-functies worden weer gegeven. 

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    filter: ['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]
});
```

Op dezelfde manier wordt het overzicht van veelhoeken in lijn lagen weer gegeven. Als u dit gedrag wilt uitschakelen in een line-laag, voegt u een `LineString` filter `MultiLineString` toe dat alleen toestaat en functies bevat.  

## <a name="math-expressions"></a>Wiskundige expressies

Wiskundige expressies bieden wiskundige Opera tors voor het uitvoeren van gegevensgestuurde berekeningen in het Framework van de expressie.

| Expressie | Retour type | Description |
|------------|-------------|-------------|
| `['+', number, number, …]` | nummer | Berekent de som van de opgegeven getallen. |
| `['-', number]` | nummer | Trekt 0 af op het opgegeven aantal. |
| `['-', number, number]` | nummer | De eerste getallen worden afgetrokken van het tweede getal. |
| `['*', number, number, …]` | nummer | Vermenigvuldigt de opgegeven getallen met elkaar. |
| `['/', number, number]` | nummer | Deelt het eerste getal door het tweede getal. |
| `['%', number, number]` | nummer | Berekent het resterende aantal bij het delen van het eerste getal door het tweede getal. |
| `['^', number, number]` | nummer | Berekent de waarde van de eerste waarde verheven tot de macht van het tweede getal. |
| `['abs', number]` | nummer | Hiermee wordt de absolute waarde van het opgegeven getal berekend. |
| `['acos', number]` | nummer | Berekent de arccosinus van het opgegeven getal. |
| `['asin', number]` | nummer | Berekent de boog sinus van het opgegeven getal. |
| `['atan', number]` | nummer | Berekent de arctangens van het opgegeven getal. |
| `['ceil', number]` | nummer | Rondt het getal af naar het volgende geheel getal. |
| `['cos', number]` | nummer | Berekent de co's van het opgegeven getal. |
| `['e']` | nummer | Retourneert de wiskundige constante `e`. |
| `['floor', number]` | nummer | Rondt het getal af naar het vorige gehele gehele getal. |
| `['ln', number]` | nummer | Berekent de natuurlijke logaritme van het opgegeven getal. |
| `['ln2']` | nummer | Retourneert de wiskundige constante `ln(2)`. |
| `['log10', number]` | nummer | Berekent de logaritme met grondtal 10 van het opgegeven getal. |
| `['log2', number]` | nummer | Berekent de logaritme met grondtal 2 van het opgegeven getal. |
| `['max', number, number, …]` | nummer | Berekent het maximum aantal in de opgegeven reeks getallen. |
| `['min', number, number, …]` | nummer | Berekent het minimale getal in de opgegeven reeks getallen. |
| `['pi']` | nummer | Retourneert de wiskundige constante `PI`. |
| `['round', number]` | nummer | Rondt het getal af op het dichtstbijzijnde gehele getal. De helft waarden worden afgerond van nul. Bijvoorbeeld, `['round', -1.5]` evalueert naar-2. |
| `['sin', number]` | nummer | Berekent de sinus van het opgegeven getal. |
| `['sqrt', number]` | nummer | Hiermee wordt de vierkantswortel van het opgegeven getal berekend. |
| `['tan', number]` | nummer | Berekent de tangens van het opgegeven getal. |

## <a name="aggregate-expression"></a>Statistische expressie

Een statistische expressie definieert een berekening die wordt verwerkt via een set gegevens en kan worden gebruikt met de `clusterProperties` optie a. `DataSource` De uitvoer van deze expressies moet een getal of een Booleaanse waarde zijn. 

Een statistische expressie neemt drie waarden in beslag; een operator waarde, en begin waarde, en een expressie voor het ophalen van een eigenschap van elke functie in een gegevens om de cumulatieve bewerking op toe te passen. Deze expressie heeft de volgende indeling:

```javascript
[operator: string, initialValue: boolean | number, mapExpression: Expression]
```

- and Een expressie functie die vervolgens wordt toegepast op alle waarden die worden berekend op `mapExpression` basis van de voor elk punt in het cluster. Ondersteunde Opera tors; 
    - Voor getallen: `+`, `*`, `max`,`min`
    - Voor Booleaanse waarden: `all`,`any`
- initialValue: Een begin waarde waarin de eerste berekende waarde wordt geaggregeerd.
- mapExpression: Een expressie die wordt toegepast op elk punt in de gegevensset.

**Voorbeelden**

Als alle functies in een gegevensset een `revenue` eigenschap hebben die een getal is. De totale omzet van alle punten in een cluster dat is gemaakt op basis van de gegevensset kan worden berekend met behulp van de volgende statistische expressie:`['+', 0, ['get', 'revenue']]`

## <a name="boolean-expressions"></a>Booleaanse expressies

Boole-expressies bieden een set Booleaanse Opera tors voor het evalueren van Boole-vergelijkingen.

Bij het vergelijken van waarden is de vergelijking strikt getypt. Waarden van verschillende typen worden altijd als ongelijk beschouwd. Gevallen waarin de typen bekend zijn om te worden geparseerd, worden beschouwd als ongeldig en er wordt een parseringsfout gegenereerd. 

| Expressie | Retour type | Description |
|------------|-------------|-------------|
| `['! ', boolean]` | boolean | Logische negatie. Retourneert `true` of de invoer is `false` `false`enof de invoer is. `true` |
| `['!= ', value, value]` | boolean | Retourneert `true` of de invoer waarden niet gelijk zijn, `false` anders. |
| `['<', value, value]` | boolean | Retourneert `true` of de eerste invoer strikt kleiner is dan de seconde, `false` anders. De argumenten moeten beide ofwel teken reeksen of beide getallen zijn. |
| `['<=', value, value]` | boolean | Retourneert `true` of de eerste invoer kleiner dan of gelijk aan de tweede is, `false` anders. De argumenten moeten beide ofwel teken reeksen of beide getallen zijn. |
| `['==', value, value]` | boolean | Retourneert `true` of de invoer waarden gelijk zijn, `false` anders. De argumenten moeten beide ofwel teken reeksen of beide getallen zijn. |
| `['>', value, value]` | boolean | Retourneert `true` of de eerste invoer absoluut groter is dan de tweede, `false` anders. De argumenten moeten beide ofwel teken reeksen of beide getallen zijn. |
| `['>=' value, value]` | boolean | Retourneert `true` of de eerste invoer groter is dan of gelijk is aan de tweede `false` , anders. De argumenten moeten beide ofwel teken reeksen of beide getallen zijn. |
| `['all', boolean, boolean, …]` | boolean | Retourneert `true` of alle `true`invoer waarden, `false` anders. |
| `['any', boolean, boolean, …]` | boolean | Retourneert `true` of een van de `true`invoer waarden `false` is. |

## <a name="conditional-expressions"></a>Voorwaardelijke expressies

Voorwaardelijke expressies bieden logische bewerkingen, zoals if-instructies.

Met de volgende expressies worden voorwaardelijke logica bewerkingen uitgevoerd op de invoer gegevens. De expressie biedt bijvoorbeeld `case` de logica ' If/then/else ' terwijl de `match` expressie lijkt op een ' switch-instructie '. 

### <a name="case-expression"></a>Case-expressie

Een `case` expressie is een type voorwaardelijke expressie die wordt geboden als-instructie zoals Logic (if/then/else). Dit type expressie wordt stapsgewijs door een lijst met Boole-voor waarden en retourneert de uitvoer waarde van de eerste Booleaanse voor waarde die waar is.

De volgende pseudocode definieert de structuur van de `case` expressie. 

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

In het volgende voor beeld worden de verschillende Booleaanse voor waarden door lopen totdat er een `true`wordt gevonden die hiermee wordt geëvalueerd. vervolgens wordt de bijbehorende waarde geretourneerd. Als er geen Booleaanse voor waarde wordt `true`geëvalueerd, wordt een terugval waarde geretourneerd. 

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

### <a name="match-expression"></a>Overeenkomende expressie

Een `match` expressie is een type voorwaardelijke expressie die een switch-instructie als logica biedt. De invoer kan een wille keurige `['get', 'entityType']` expressie zijn, zoals het retour neren van een teken reeks of een getal. Elk label moet één letterlijke waarde of een matrix van letterlijke waarden zijn, waarvan de waarden alle teken reeksen of alle getallen moeten zijn. De invoer komt overeen als een van de waarden in de matrix overeenkomt. Elk label moet uniek zijn. Als het invoer type niet overeenkomt met het type van de labels, wordt het resultaat de terugval waarde.

De volgende pseudocode definieert de structuur van de `match` expressie. 

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

In het volgende voor beeld wordt `entityType` met de eigenschap van een punt functie in een Bubble Layer gezocht naar een overeenkomst. Als er een overeenkomst wordt gevonden, wordt deze opgegeven waarde geretourneerd of wordt de terugval waarde geretourneerd.

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

In het volgende voor beeld wordt een matrix gebruikt om een set labels weer te geven die allemaal dezelfde waarde moeten retour neren. Dit is veel efficiënter dan elk label afzonderlijk weer te geven. Als in dit geval de `entityType` eigenschap "restaurant" of "grocery_store" is, wordt de kleur "Red" geretourneerd.

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

In het volgende voor beeld wordt een match-expressie gebruikt voor het uitvoeren van een ' in matrix ' of ' matrix bevat ' type filter. in dit geval worden gegevens met een ID-waarde in een lijst met toegestane Id's gefilterd. Wanneer u expressies met filters gebruikt, moet het resultaat een Booleaanse waarde zijn.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    filter: [
        'match',  

        //Get the property to match.
        ['get', 'id'],  

         //List of values to match.
        [24, 53, 98], 

        //If there is a match, return true.
        true,
    
        //Otherwise return false.
        false
    ]
});
```

### <a name="coalesce-expression"></a>Coalesce-expressie

Een `coalesce` expressie wordt stapsgewijs door een reeks expressies uitgevoerd, totdat de eerste niet-null-waarde wordt opgehaald en deze waarde wordt geretourneerd. 

De volgende pseudocode definieert de structuur van de `coalesce` expressie. 

```javascript
[
    'coalesce', 
    value1, 
    value2, 
    …
]
```

**Voorbeeld**

In het volgende voor beeld `coalesce` wordt een expressie gebruikt `textField` om de optie van een symbool laag in te stellen. Als de `title` eigenschap ontbreekt in de-functie of is ingesteld `null`op, zoekt de expressie vervolgens naar de `subtitle` eigenschap, als deze ontbreekt of `null`, wordt deze terugvallen op een lege teken reeks. 

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

## <a name="type-expressions"></a>Type-expressies

Type-expressies bieden hulpprogram ma's voor het testen en omzetten van verschillende gegevens typen, zoals teken reeksen, getallen en Booleaanse waarden.

| Expressie | Retour type | Description |
|------------|-------------|-------------|
| `['literal', array]`<br/><br/>`['literal', object]` | matrix \| object | Retourneert een letterlijke matrix of object waarde. Gebruik deze expressie om te voor komen dat een matrix of object als een expressie wordt geëvalueerd. Dit is nodig wanneer een matrix of object moet worden geretourneerd door een expressie. |
| `['to-boolean', value]` | boolean | Hiermee wordt de invoer waarde geconverteerd naar een Boolean. Het resultaat is `false` wanneer de invoer een lege teken reeks `false`, `0` `null`,, of `NaN`; anders `true`is. |
| `['to-color', value]`<br/><br/>`['to-color', value1, value2…]` | kleur | Converteert de invoer waarde naar een kleur. Als er meerdere waarden worden gegeven, wordt elke waarde in de aangegeven volg orde geëvalueerd totdat de eerste geslaagde conversie is verkregen. Als geen van de invoer kan worden geconverteerd, is de expressie een fout. |
| `['to-number', value]`<br/><br/>`['to-number', value1, value2, …]` | nummer | Converteert de invoer waarde indien mogelijk naar een getal. Als de invoer is `null` of `false`, is het resultaat 0. Als de invoer is `true`, is het resultaat 1. Als de invoer een teken reeks is, wordt deze geconverteerd naar een getal met behulp van de [ToNumber](https://tc39.github.io/ecma262/#sec-tonumber-applied-to-the-string-type) -teken reeks functie van de ECMAScript Language Specification. Als er meerdere waarden worden gegeven, wordt elke waarde in de aangegeven volg orde geëvalueerd totdat de eerste geslaagde conversie is verkregen. Als geen van de invoer kan worden geconverteerd, is de expressie een fout. |
| `['to-string', value]` | string | Converteert de invoer waarde naar een teken reeks. Als de invoer is `null`, is `""`het resultaat. Als de invoer een Booleaanse waarde is, is `"true"` het resultaat of. `"false"` Als de invoer een getal is, wordt dit geconverteerd naar een teken reeks met behulp van de functie [toString](https://tc39.github.io/ecma262/#sec-tostring-applied-to-the-number-type) -nummer van de ECMAScript Language Specification. Als de invoer een kleur is, wordt deze geconverteerd naar een CSS-kleur `"rgba(r,g,b,a)"`teken reeks. Anders wordt de invoer geconverteerd naar een teken reeks met behulp van de [JSON. stringify](https://tc39.github.io/ecma262/#sec-json.stringify) -functie van de ECMAScript Language Specification. |
| `['typeof', value]` | string | Retourneert een teken reeks die het type van de opgegeven waarde beschrijft. |

> [!TIP]
> Als er een fout bericht `Expression name must be a string, but found number instead. If you wanted a literal array, use ["literal", [...]].` wordt weer gegeven dat lijkt op de browser console, betekent dit dat er een expressie ergens in uw code staat die een matrix heeft die geen teken reeks voor de eerste waarde heeft. Als u wilt dat de expressie een matrix retourneert, laat u de matrix teruglopen `literal` met de expressie. In het volgende voor beeld wordt `offset` de pictogram optie van een Symbol-laag ingesteld. deze moet een matrix zijn met twee cijfers, `match` door gebruik te maken van een expressie om te kiezen tussen twee offset `entityType` waarden op basis van de waarde van de eigenschap van het punt hulp.
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

## <a name="color-expressions"></a>Kleur expressies

Kleur expressies maken het gemakkelijker om kleur waarden te maken en te bewerken.

| Expressie | Retour type | Description |
|------------|-------------|-------------|
| `['rgb', number, number, number]` | kleur | Hiermee maakt u een kleur waarde van de onderdelen *rood*, *groen*en *blauw* die tussen `0` en `255`moeten variëren, en een alpha `1`-onderdeel van. Als een onderdeel zich buiten het bereik bevindt, is er een fout opgetreden in de expressie. |
| `['rgba', number, number, number, number]` | kleur | Hiermee maakt u een kleur waarde van *rode*, *groene*en *blauwe* onderdelen die tussen `0` en `255`moeten liggen en een alpha-component binnen een `0` bereik `1`van en. Als een onderdeel zich buiten het bereik bevindt, is er een fout opgetreden in de expressie. |
| `['to-rgba']` | \[getal, getal, getal, getal\] | Retourneert een matrix met vier elementen met de *rode*, *groene*, *blauwe*en *alpha* -onderdelen van de invoer kleur, in die volg orde. |

**Voorbeeld**

In het volgende voor beeld wordt een RGB-kleur waarde gemaakt met een `255`rode waarde van en waarden voor *groen* en *blauw* die worden berekend `2.5` door te vermenigvuldigen met `temperature` de waarde van de eigenschap. Wanneer de Tempe ratuur wijzigt, verandert de kleur in verschillende tinten *rood*.

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

## <a name="string-operator-expressions"></a>Teken reeks operator expressies

Teken reeks operator expressies voeren conversie bewerkingen uit op teken reeksen, zoals samen voegen en het converteren van de aanvraag. 

| Expressie | Retour type | Description |
|------------|-------------|-------------|
| `['concat', string, string, …]` | string | Meerdere teken reeksen samen voegen. Elke waarde moet een teken reeks zijn. Gebruik de `to-string` type-expressie om andere waardetypen zo nodig te converteren naar een teken reeks. |
| `['downcase', string]` | string | Hiermee wordt de opgegeven teken reeks geconverteerd naar kleine letters. |
| `['upcase', string]` | string | Converteert de opgegeven teken reeks naar hoofd letters. |

**Voorbeeld**

In het volgende voor beeld `temperature` wordt de eigenschap van de punt functie geconverteerd naar een teken reeks en wordt vervolgens ' °F ' aan het einde van het object aaneengeschakeld.

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

De bovenstaande expressie geeft een pincode op de kaart weer met de tekst ' 64 °F ', zoals in de onderstaande afbeelding wordt weer gegeven.

<center>

![Voor beeld van een](media/how-to-expressions/string-operator-expression.png) expressie voor een teken reeks operator</center>

## <a name="interpolate-and-step-expressions"></a>Interpoleer-en Step-expressies

Interpolatie-en stap expressies kunnen worden gebruikt voor het berekenen van waarden in een geïnterpoleerde curve of stap functie. Deze expressies nemen in een expressie op die bijvoorbeeld `['get',  'temperature']`een numerieke waarde retourneert als invoer. De invoer waarde wordt geëvalueerd ten opzichte van paren van invoer-en uitvoer waarden, met de naam ' stops ', om de waarde te bepalen die het beste past bij de geïnterpoleerde curve of stap functie. De invoer waarden voor elke Stop moeten een getal zijn en moeten in oplopende volg orde staan. De uitvoer waarden moeten een getal, een matrix van getallen of een kleur zijn.

### <a name="interpolate-expression"></a>Interpole-expressie

Een `interpolate` expressie kan worden gebruikt om een continue, soepele set waarden te berekenen door te interpolatie tussen de stop waarden. Een `interpolate` expressie die kleur waarden retourneert, produceert een kleur overgang waarbij resultaat waarden worden geselecteerd uit.

Er zijn drie typen interpolatie methoden die kunnen worden gebruikt in een `interpolate` expressie:
 
* `['linear']`-Interpoleeert lineair tussen het paar stops.
* `['exponential', base]`-Interpoleert exponentieel tussen de stops. De `base` waarde bepaalt de snelheid waarmee de uitvoer toeneemt. Hoe hoger de waarde, hoe groter de uitvoer meer naar het hoge einde van het bereik. Een `base` waarde dicht bij 1 resulteert in een uitvoer die meer lineair toeneemt.
* `['cubic-bezier', x1, y1, x2, y2]`-Interpoles met behulp van een [kubieke Bézier-curve](https://developer.mozilla.org/docs/Web/CSS/timing-function) die is gedefinieerd door de gegeven besturings punten.

Hier volgt een voor beeld van hoe de verschillende typen interpolatie eruitzien. 

| Lineair  | Exponentieel | Kubieke Bézier |
|---------|-------------|--------------|
| ![Grafiek met lineaire interpolatie](media/how-to-expressions/linear-interpolation.png) | ![Exponentiële interpolatie grafiek](media/how-to-expressions/exponential-interpolation.png) | ![Grafiek met kubieke Bezier-interpolatie](media/how-to-expressions/bezier-curve-interpolation.png) |

De volgende pseudocode definieert de structuur van de `interpolate` expressie. 

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

In het volgende voor beeld `linear interpolate` wordt een expressie gebruikt `color` om de eigenschap van een tekenlaag in te `temperature` stellen op basis van de eigenschap van de functie Point. Als de `temperature` waarde lager is dan 60, wordt er "Blue" geretourneerd, als tussen 60 en minder dan 70, geel wordt geretourneerd, indien tussen 70 en minder dan 80, "oranje" wordt geretourneerd als het 80 of groter is, "rood" wordt geretourneerd.

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

In de volgende afbeelding ziet u hoe de kleuren voor de bovenstaande expressie worden gekozen.
 
<center>

![Voor beeld](media/how-to-expressions/interpolate-expression-example.png) van interpolatie-expressie</center>

### <a name="step-expression"></a>Expressie voor stap

Een `step` expressie kan worden gebruikt om discrete, getrapte resultaat waarden te berekenen door te evalueren van een [piecewise-constante functie](http://mathworld.wolfram.com/PiecewiseConstantFunction.html) die door stops wordt gedefinieerd. 

De volgende pseudocode definieert de structuur van de `step` expressie. 

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

Met stap expressies wordt de uitvoer waarde van de stop net vóór de invoer waarde geretourneerd of de eerste invoer waarde als de invoer kleiner is dan de eerste stop. 

**Voorbeeld**

In het volgende voor beeld `step` wordt een expressie gebruikt `color` om de eigenschap van een tekenlaag in te `temperature` stellen op basis van de eigenschap van de functie Point. Als de `temperature` waarde lager is dan 60, wordt ' Blue ' geretourneerd, als tussen 60 en minder dan 70, ' geel ' wordt geretourneerd, indien tussen 70 en minder dan 80, ' oranje ' wordt geretourneerd als het 80 of groter is, ' rood ' wordt geretourneerd.

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

In de volgende afbeelding ziet u hoe de kleuren voor de bovenstaande expressie worden gekozen.
 
<center>

![Voor beeld van een stap-expressie](media/how-to-expressions/step-expression-example.png)
</center>

## <a name="layer-specific-expressions"></a>Laag-specifieke expressies

Speciale expressies die alleen van toepassing zijn op specifieke lagen.

### <a name="heat-map-density-expression"></a>Dichtheids expressie voor heatmap

Met een dichtheids expressie voor heatmap wordt de waarde van de heatmap voor de kaart voor elke pixel in een heatmap van een `['heatmap-density']`hitte opgehaald en gedefinieerd als. Deze waarde is een getal tussen `0` en `1` en wordt gebruikt in combi natie met `interpolation` een `step` or-expressie om de kleur overgang te definiëren die wordt gebruikt voor het inkleuren van de heatmap. Deze expressie kan alleen worden gebruikt in de [kleur optie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest#color) van de laag van de heatmap.

> [!TIP]
> De kleur bij index 0 in een interpolatie-expressie of de standaard kleur van een stap kleur, definieert de kleur van het gebied waar er geen gegevens zijn en kan worden gebruikt voor het definiëren van een achtergrond kleur. Veel gewenst om deze waarde in te stellen op transparant of semi-transparant zwart. 

**Voorbeeld**

In dit voor beeld wordt een lijn-interpolatie-expressie gebruikt voor het maken van een vloeiend kleur verloop voor het renderen van de heatmap. 

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

Naast het gebruik van een glad verloop om een heatmap in te vullen, kunnen kleuren worden opgegeven in een set bereiken met behulp van `step` een expressie. Het gebruik `step` van een expressie voor het inkleuren van de heatmap onthoudt de densiteit visueel in bereiken die meer lijkt op een contour-of radar stijl kaart.  

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

Zie de documentatie [een hitte kaart toevoegen](map-add-heat-map-layer.md) voor meer informatie.

### <a name="line-progress-expression"></a>Expressie voor voortgangs lijn

Met een expressie voor de voortgangs lijn wordt de voortgang van een verloop lijn in een line-laag `['line-progress']`opgehaald en gedefinieerd als. Deze waarde is een getal tussen 0 en 1 en wordt gebruikt in combi natie met `interpolation` een `step` or-expressie. Deze expressie kan alleen worden gebruikt met de [optie strokeGradient]( https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest#strokegradient) van de laag. 

> [!NOTE]
> Voor de `lineMetrics` optievan`true`de laag moet de optie van de gegevens bron worden ingesteld op. `strokeGradient`

**Voorbeeld**

In het volgende voor beeld `['line-progress']` wordt de expressie gebruikt om een kleur overgang toe te passen op de lijn van een regel.

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

[Zie Live voor beeld](map-add-line-layer.md#line-stroke-gradient)

### <a name="text-field-format-expression"></a>Expressie voor tekst veld notatie

De expressie voor de tekst veld notatie kan worden gebruikt `textField` met de optie van de `textOptions` eigenschap Symbol Layers om gemengde tekst opmaak te bieden. Met deze expressie kan een set invoer teken reeksen en opmaak opties worden opgegeven. De volgende opties kunnen worden opgegeven voor elke invoer reeks in deze expressie.

 * `'font-scale'`-Hiermee geeft u de schaal factor voor de teken grootte op. Als deze waarde wordt opgegeven, wordt de `size` eigenschap van de `textOptions` afzonderlijke teken reeks overschreven.
 * `'text-font'`-Hiermee geeft u een of meer lettertype families op die moeten worden gebruikt voor deze teken reeks. Als deze waarde wordt opgegeven, wordt de `font` eigenschap van de `textOptions` afzonderlijke teken reeks overschreven.

De volgende pseudocode definieert de structuur van de tekst veld indelings expressie. 

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

In het volgende voor beeld wordt het tekst veld opgemaakt door een vet letter type toe te voegen en de `title` teken grootte van de eigenschap van de functie omhoog te schalen. In dit voor beeld wordt `subtitle` ook de eigenschap van de functie op een nieuwe regel toegevoegd, met een geschaalde teken grootte.

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

Met deze laag wordt de punt functie weer gegeven zoals in de onderstaande afbeelding:
 
<center>

![Afbeelding van punt functie met opgemaakte](media/how-to-expressions/text-field-format-expression.png) tekst veld</center>

### <a name="number-format-expression"></a>De expressie getalnotatie

De `number-format` expressie kan alleen worden gebruikt met de `textField` optie van een Symbol-laag. Met deze expressie wordt het gegeven getal geconverteerd naar een ingedeelde teken reeks. Met deze expressie wordt de functie [Number. toLocalString](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString) van Java script ingepakt en wordt de volgende set opties ondersteund.

 * `locale`-Geef deze optie op voor het converteren van getallen naar teken reeksen op een manier die wordt uitgelijnd met de opgegeven taal. Geef een [BCP 47-taal label](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Intl#Locale_identification_and_negotiation) door in deze optie.
 * `currency`-Om het getal te converteren naar een teken reeks die een valuta vertegenwoordigt. Mogelijke waarden zijn de [ISO 4217-valuta codes](https://en.wikipedia.org/wiki/ISO_4217), zoals "USD" voor de Amerikaanse dollar, "EUR" voor de euro of "CNY" voor de Chinese RMB.
 * `'min-fraction-digits'`-Geeft het minimale aantal decimalen aan dat moet worden meegenomen in de teken reeks versie van het getal.
 * `'max-fraction-digits'`-Hiermee geeft u het maximum aantal decimalen op dat moet worden meegenomen in de teken reeks versie van het getal.

De volgende pseudocode definieert de structuur van de tekst veld indelings expressie. 

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

In het volgende voor beeld `number-format` wordt een expressie gebruikt om `revenue` te wijzigen hoe de eigenschap van de punt functie wordt `textField` weer gegeven in de optie van een Symbol-laag, zodat deze een Amerikaanse dollar waarde wordt weer gegeven.

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

Met deze laag wordt de punt functie weer gegeven zoals in de onderstaande afbeelding:

<center>

![Voor beeld van een](media/how-to-expressions/number-format-expression.png) expressie voor getalnotatie</center>

## <a name="zoom-expression"></a>Expressie voor in-/uitzoomen

Een `zoom` expressie wordt gebruikt om het huidige zoom niveau van de kaart op te halen op weergave tijd en wordt `['zoom']`gedefinieerd als. Deze expressie retourneert een getal tussen het minimale en maximale zoom niveau bereik van de kaart. Met deze expressie kunnen stijlen dynamisch worden gewijzigd naarmate het zoom niveau van de kaart wordt gewijzigd. De `zoom` expressie kan alleen worden gebruikt met `interpolate` and `step` -expressies.

**Voorbeeld**

De RADIUS van gegevens punten die worden weer gegeven in de laag van de heatmap hebben standaard een straal van vaste pixels voor alle zoom niveaus. Als de kaart is ingezoomd, worden de gegevens samen geaggregeerd en de heatmap van de kaart laag. Een `zoom` expressie kan worden gebruikt om de RADIUS voor elk zoom niveau zodanig te schalen dat elk gegevens punt overeenkomt met hetzelfde fysieke gebied van de kaart. Hierdoor is de heatmap van de hitte statisch en consistent. Elk zoom niveau van de kaart heeft twee keer zoveel pixels verticaal en horizon taal als het vorige zoom niveau. Door de RADIUS zodanig te schalen dat deze met elk zoom niveau verdubbelt, wordt een heatmap gemaakt die consistent is op alle zoom niveaus. Dit kunt u doen met behulp `zoom` van de expressie `base 2 exponential interpolation` met een expressie zoals hieronder wordt weer gegeven. 

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

[Zie Live voor beeld](map-add-heat-map-layer.md#consistent-zoomable-heat-map)

## <a name="variable-binding-expressions"></a>Variabele bindings expressies

Met variabelen bindings expressies worden de resultaten van een berekening in een variabele opgeslagen, zodat er meermaals naar een andere locatie in een expressie kan worden verwezen zonder dat deze opnieuw moet worden berekend. Dit is een handige optimalisatie voor expressies waarbij veel berekeningen worden gebruikt

| Expressie | Retour type | Description |
|--------------|---------------|--------------|
| \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;Let,<br/>&nbsp;&nbsp;&nbsp;&nbsp;NAME1: teken reeks,<br/>&nbsp;&nbsp;&nbsp;&nbsp;waarde1: any,<br/>&nbsp;&nbsp;&nbsp;&nbsp;naam2: teken reeks,<br/>&nbsp;&nbsp;&nbsp;&nbsp;waarde2: any,<br/>&nbsp;&nbsp;&nbsp;&nbsp;…<br/>&nbsp;&nbsp;&nbsp;&nbsp;childExpression<br/>\] | | Slaat een of meer waarden op als variabelen voor gebruik door `var` de expressie in de onderliggende expressie waarmee het resultaat wordt geretourneerd. |
| `['var', name: string]` | willekeurig | Verwijst naar een variabele die is gemaakt met `let` behulp van de expressie. |

**Voorbeeld**

In dit voor beeld wordt een expressie gebruikt waarmee de opbrengst wordt berekend ten opzichte `case` van de Tempe ratuur en vervolgens een expressie wordt gebruikt om verschillende Boole-bewerkingen op deze waarde te evalueren. De `let` expressie wordt gebruikt voor het opslaan van de opbrengst ten opzichte van de temperatuur verhouding zodat deze slechts één keer `var` hoeft te worden berekend en de expressie naar deze variabele zo vaak als nodig is, zonder dat deze opnieuw moet worden berekend.

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

Raadpleeg de volgende artikelen voor meer code voorbeelden voor het implementeren van expressies:

> [!div class="nextstepaction"] 
> [Een symbool laag toevoegen](map-add-pin.md)

> [!div class="nextstepaction"] 
> [Een Bubble laag toevoegen](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Een line laag toevoegen](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Een polygoon laag toevoegen](map-add-shape.md)

> [!div class="nextstepaction"] 
> [Een hitte kaart laag toevoegen](map-add-heat-map-layer.md)

Meer informatie over de laag opties die expressies ondersteunen:

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
