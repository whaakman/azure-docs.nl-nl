---
title: OData georuimtelijke naslag - Azure Search
description: OData georuimtelijke functies, geo.distance en geo.intersects in Azure Search-query's.
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 0ce63ab1143c784eb3e10f47c20ef2b5034d63a7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079793"
---
# <a name="odata-geo-spatial-functions-in-azure-search---geodistance-and-geointersects"></a>OData, georuimtelijke functies in Azure Search - `geo.distance` en `geo.intersects`

Azure Search biedt ondersteuning voor georuimtelijke query's in [OData-filterexpressies](query-odata-filter-orderby-syntax.md) via de `geo.distance` en `geo.intersects` functies. De `geo.distance` functie wordt de afstand in kilometer zijn verwijderd tussen de twee punten, één wordt een veld of de bereik-variabele en een wordt een constante doorgegeven als onderdeel van het filter. De `geo.intersects` functie retourneert `true` als een opgegeven punt zich binnen een bepaalde veelhoek, waarbij het punt een veld of bereik-variabele en de veelhoek is opgegeven als een constante doorgegeven als onderdeel van het filter.

De `geo.distance` functie kan ook worden gebruikt de [ **$orderby** parameter](search-query-odata-orderby.md) zoekresultaten op afstand van een bepaald moment sorteren. De syntaxis voor `geo.distance` in **$orderby** is hetzelfde als het zich in **$filter**. Bij het gebruik van `geo.distance` in **$orderby**, het veld waarop deze van toepassing is moet van het type `Edm.GeographyPoint` en moet ook **sorteerbaar**.

## <a name="syntax"></a>Syntaxis

De volgende EBNF ([uitgebreid Backus Naur formulier](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) definieert de grammatica van de `geo.distance` en `geo.intersects` functies, evenals de georuimtelijke waarden waarop zij werken:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
geo_distance_call ::=
    'geo.distance(' variable ',' geo_point ')'
    | 'geo.distance(' geo_point ',' variable ')'

geo_point ::= "geography'POINT(" lon_lat ")'"

lon_lat ::= float_literal ' ' float_literal

geo_intersects_call ::=
    'geo.intersects(' variable ',' geo_polygon ')'

/* You need at least four points to form a polygon, where the first and
last points are the same. */
geo_polygon ::=
    "geography'POLYGON((" lon_lat ',' lon_lat ',' lon_lat ',' lon_lat_list "))'"

lon_lat_list ::= lon_lat(',' lon_lat)*
```

Een diagram van een interactieve syntaxis is ook beschikbaar:

> [!div class="nextstepaction"]
> [Diagram van de OData-syntaxis voor Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#geo_distance_call)

> [!NOTE]
> Zie [naslaginformatie over expressiesyntaxis voor Azure Search OData](search-query-odata-syntax-reference.md) voor de volledige EBNF.

### <a name="geodistance"></a>geo.distance

De `geo.distance` functie heeft twee parameters van het type `Edm.GeographyPoint` en retourneert een `Edm.Double` waarde die de afstand tussen hen in kilometer zijn verwijderd. Dit wijkt af van andere services die ondersteuning bieden voor OData georuimtelijke bewerkingen, die doorgaans afstanden in meters te retourneren.

Een van de parameters voor `geo.distance` moet een constante Geografie-punt en de andere moet een pad naar veld (of een bereik-variabele in het geval van een filter iteratie van een veld van het type `Collection(Edm.GeographyPoint)`). De volgorde van deze parameters maakt niet uit.

De Geografie point-constante is van het formulier `geography'POINT(<longitude> <latitude>)'`, waarbij de lengtegraad en breedtegraad numerieke constanten zijn.

> [!NOTE]
> Bij het gebruik van `geo.distance` in een filter, moet u de afstand die zijn geretourneerd door de functie met een constante met vergelijken `lt`, `le`, `gt`, of `ge`. De operators `eq` en `ne` worden niet ondersteund bij het vergelijken van afstanden. Dit is bijvoorbeeld een correct gebruik van `geo.distance`: `$filter=geo.distance(location, geography'POINT(-122.131577 47.678581)') le 5`.

### <a name="geointersects"></a>geo.intersects

De `geo.intersects` functie omvat een variabele van het type `Edm.GeographyPoint` en een constante `Edm.GeographyPolygon` en retourneert een `Edm.Boolean`  --  `true` als het punt zich binnen de grenzen van de polygoon `false` anders.

De veelhoek is een tweedimensionale oppervlak opgeslagen als een reeks punten definiëren van een selectiekader ring (Zie de [voorbeelden](#examples) hieronder). De veelhoek moet worden gesloten, wat betekent dat de eerste en laatste punt sets moeten hetzelfde zijn. [Punten in een polygoon moeten zich in tegen de klok in volgorde](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

### <a name="geo-spatial-queries-and-polygons-spanning-the-180th-meridian"></a>Georuimtelijke query's en de 180th kaartmeridiaan spanning veelhoeken

Voor veel georuimtelijke query bibliotheken formuleren van een query uitvoert met de 180th kaartmeridiaan (in de buurt van de datumgrens) is een off-limits of een tijdelijke oplossing, zoals het splitsen van de veelhoek in twee, een aan beide zijden van de kaartmeridiaan vereist.

In Azure Search, georuimtelijke query's met 180 graden lengtegraad werkt zoals verwacht als de query-shape rechthoekige en uw coördinaten uitgelijnd met een rasterindeling langs de lengtegraad en breedtegraad (bijvoorbeeld `geo.intersects(location, geography'POLYGON((179 65, 179 66, -179 66, -179 65, 179 65))'`). Anders voor niet-rechthoekige of niet-uitgelijnd vormen, kunt u de benadering van de polygoon splitsen.  

### <a name="geo-spatial-functions-and-null"></a>Georuimtelijke functies en `null`

Als alle andere velden die niet-verzameling in Azure Search, velden van het type `Edm.GeographyPoint` mag `null` waarden. Wanneer Azure Search wordt geëvalueerd `geo.intersects` voor een veld dat is `null`, het resultaat is altijd `false`. Het gedrag van `geo.distance` in dit geval is afhankelijk van de context:

- In de filters, `geo.distance` van een `null` resulteert in een veld `null`. Dit betekent dat het document komt niet overeen met omdat `null` in vergelijking met een niet-null-waarde resulteert in `false`.
- Bij het sorteren van resultaten met behulp van **$orderby**, `geo.distance` van een `null` veld resultaten in de maximale afstand mogelijk. Documenten met zo'n veld sorteren lager is dan alle andere wanneer de sorteerrichting `asc` wordt gebruikt (de standaardinstelling) en hoger is dan alle andere wanneer de richting is `desc`.

## <a name="examples"></a>Voorbeelden

### <a name="filter-examples"></a>Voorbeelden van filters

Alle hotels binnen 10 kilometer van de opgegeven referentie vinden (locatie is waar een veld van het type `Edm.GeographyPoint`):

    geo.distance(location, geography'POINT(-122.131577 47.678581)') le 10

Alle hotels binnen een bepaalde viewport beschreven als een veelhoek vinden (locatie is waar een veld van het type `Edm.GeographyPoint`). Houd er rekening mee dat de veelhoek is gesloten (de eerste en laatste punt sets moet hetzelfde) en [de punten moeten worden weergegeven in tegen de klok in volgorde](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

    geo.intersects(location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))')

### <a name="order-by-examples"></a>Volgorde van voorbeelden

Hotels door Aflopend sorteren `rating`, klikt u vervolgens oplopende op afstand van de opgegeven coördinaten:

    rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

Hotels in aflopende volgorde sorteren `search.score` en `rating`, en klik vervolgens in oplopende volgorde op afstand van de opgegeven coördinaten zodat tussen twee hotels met identieke beoordeling, het beste een eerste wordt weergegeven:

    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Volgende stappen  

- [Filters in Azure Search](search-filters.md)
- [Overzicht van taal van OData-expressie voor Azure Search](query-odata-filter-orderby-syntax.md)
- [Naslaginformatie over de syntaxis van de OData-expressie voor Azure Search](search-query-odata-syntax-reference.md)
- [Documenten zoeken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
