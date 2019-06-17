---
title: Overzicht van OData taal - Azure Search
description: Overzicht van de OData-taal voor filters, selecteer en volgorde door voor Azure Search-query's.
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
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
ms.openlocfilehash: 166c23088fe0388199ca51efde05153bb5697e38
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67063704"
---
# <a name="odata-language-overview-for-filter-orderby-and-select-in-azure-search"></a>Overzicht van de OData-taal voor `$filter`, `$orderby`, en `$select` in Azure Search

Azure Search ondersteunt een subset van de syntaxis van de OData-expressie voor **$filter**, **$orderby**, en **$select** expressies. Filterexpressies worden geëvalueerd tijdens query parseren, zoeken naar specifieke velden beperken of toe te voegen overeenkomen met criteria die zijn gebruikt tijdens scans index. Order by-expressies worden toegepast als een stap na verwerking via een resultatenset voor het sorteren van de documenten die worden geretourneerd. Selecteer expressies te bepalen welke documentvelden u zijn opgenomen in de resultatenset. De syntaxis van deze expressies is hetzelfde als de [eenvoudige](query-simple-syntax.md) of [volledige](query-lucene-syntax.md) querysyntaxis die wordt gebruikt in de **zoeken** parameter, hoewel er een thema's besproken in de syntaxis voor verwijzen naar velden.

Dit artikel bevat een overzicht van de OData-expressietaal die wordt gebruikt in filters, order by en selecteer expressies. De taal die wordt gepresenteerd "beneden", beginnen met de belangrijkste elementen en het bouwen van op deze. De syntaxis van de op het hoogste niveau voor elke parameter wordt in een apart artikel beschreven:

- [$filter syntaxis](search-query-odata-filter.md)
- [$orderby syntaxis](search-query-odata-orderby.md)
- [$select syntaxis](search-query-odata-select.md)

OData-expressies variëren van eenvoudige tot complexe, maar ze alle delen gemeenschappelijke elementen. De belangrijkste onderdelen van een OData-expressie in Azure Search zijn:

- **Veld paden**, die verwijzen naar specifieke velden van uw index.
- **Constanten**, letterlijke waarden van een bepaald gegevenstype zijn.

> [!NOTE]
> Belangrijkste termen in Azure Search wijkt af van de [OData-standaard](https://www.odata.org/documentation/) in een aantal manieren. Noemen we een **veld** in Azure Search heet een **eigenschap** in OData en op dezelfde manier voor **pad naar veld** versus **eigenschapspad**. Een **index** met **documenten** in Azure Search wordt genoemd in het algemeen in OData als een **entiteitsset** met **entiteiten**. De Azure Search-terminologie wordt overal in deze referentie gebruikt.

## <a name="field-paths"></a>Veld paden

De volgende EBNF ([uitgebreid Backus Naur formulier](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) definieert de grammatica van veld paden.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
field_path ::= identifier('/'identifier)*

identifier ::= [a-zA-Z_][a-zA-Z_0-9]*
```

Een diagram van een interactieve syntaxis is ook beschikbaar:

> [!div class="nextstepaction"]
> [Diagram van de OData-syntaxis voor Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#field_path)

> [!NOTE]
> Zie [naslaginformatie over expressiesyntaxis voor Azure Search OData](search-query-odata-syntax-reference.md) voor de volledige EBNF.

Het pad naar een veld bestaat uit een of meer **id's** gescheiden door slashes. Elke-id is een reeks tekens dat moet beginnen met een ASCII-letter of onderstrepingsteken en alleen ASCII-letters, cijfers of onderstrepingstekens bevatten. De letters mag hoofdletters of kleine letters.

Een id kan verwijzen naar de naam van een veld of naar een **bereik variabele** in de context van een [verzameling expressie](search-query-odata-collection-operators.md) (`any` of `all`) in een filter. Een variabele bereik is vergelijkbaar met een lusvariabele die staat voor de huidige element van de verzameling. Voor complexe verzamelingen vertegenwoordigt die variabele een object, dat is de reden waarom u veld paden gebruiken kunt om te verwijzen naar de onderliggende velden van de variabele. Dit is vergelijkbaar met puntnotatie in veel programmeertalen.

Voorbeelden van veld paden worden weergegeven in de volgende tabel:

| Pad naar veld | Description |
| --- | --- |
| `HotelName` | Verwijst naar een veld op het hoogste niveau van de index |
| `Address/City` | Verwijst naar de `City` subplan veld van een complexe veld in de index; `Address` is van het type `Edm.ComplexType` in dit voorbeeld |
| `Rooms/Type` | Verwijst naar de `Type` subplan veld van een verzameling complexe-veld in de index; `Rooms` is van het type `Collection(Edm.ComplexType)` in dit voorbeeld |
| `Stores/Address/Country` | Verwijst naar de `Country` subplan veld van de `Address` subplan veld van een verzameling complexe-veld in de index; `Stores` is van het type `Collection(Edm.ComplexType)` en `Address` is van het type `Edm.ComplexType` in dit voorbeeld |
| `room/Type` | Verwijst naar de `Type` subplan veld van de `room` bereik-variabele, bijvoorbeeld in de filterexpressie `Rooms/any(room: room/Type eq 'deluxe')` |
| `store/Address/Country` | Verwijst naar de `Country` subplan veld van de `Address` subplan veld van de `store` bereik-variabele, bijvoorbeeld in de filterexpressie `Stores/any(store: store/Address/Country eq 'Canada')` |

De betekenis van het pad naar een veld is afhankelijk van de context. In de filters, het pad naar een veld verwijst naar de waarde van een *één exemplaar* van een veld in het huidige document. In een andere context, zoals **$orderby**, **$select**, of in [fielded zoeken in de volledige Lucene-syntaxis](query-lucene-syntax.md#bkmk_fields), een pad naar veld verwijst naar het veld zelf. Dit verschil heeft enkele gevolgen voor hoe u een veld paden in filters gebruikt.

Houd rekening met het pad naar veld `Address/City`. Dit verwijst naar een enkele plaats voor het huidige document, zoals "San Francisco" in een filter. Daarentegen `Rooms/Type` verwijst naar de `Type` lagere veld voor veel ruimten (zoals 'standaard' voor de eerste ruimte 'deluxe' voor de tweede ruimte, enzovoort). Aangezien `Rooms/Type` verwijst niet naar een *één exemplaar* van het onderliggende veld `Type`, deze rechtstreeks in een filter kan niet worden gebruikt. In plaats daarvan om te filteren op type ruimte, gebruikt u een [lambda-expressie](search-query-odata-collection-operators.md) met een bereik-variabele, als volgt:

    Rooms/any(room: room/Type eq 'deluxe')

In dit voorbeeld wordt de variabele bereik `room` wordt weergegeven in de `room/Type` pad naar veld. Op die manier `room/Type` verwijst naar het type van de huidige ruimte in het huidige document. Dit is slechts één exemplaar van de `Type` veld, zodat deze rechtstreeks in het filter kan worden gebruikt.

### <a name="using-field-paths"></a>Met behulp van de paden van veld

Veld paden worden gebruikt in veel parameters van de [Azure Search-API](https://docs.microsoft.com/rest/api/searchservice/). De volgende tabel geeft een lijst van alle locaties waar ze kunnen worden gebruikt, plus eventuele beperkingen van hun gebruik:

| API | Parameternaam | Beperkingen |
| --- | --- | --- |
| [Maak](https://docs.microsoft.com/rest/api/searchservice/create-index) of [Update](https://docs.microsoft.com/rest/api/searchservice/update-index) Index | `suggesters/sourceFields` | Geen |
| [Maak](https://docs.microsoft.com/rest/api/searchservice/create-index) of [Update](https://docs.microsoft.com/rest/api/searchservice/update-index) Index | `scoringProfiles/text/weights` | Kan alleen verwijzen naar **doorzoekbare** velden |
| [Maak](https://docs.microsoft.com/rest/api/searchservice/create-index) of [Update](https://docs.microsoft.com/rest/api/searchservice/update-index) Index | `scoringProfiles/functions/fieldName` | Kan alleen verwijzen naar **Filterbaar** velden |
| [Zoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents) | `search` Wanneer `queryType` is `full` | Kan alleen verwijzen naar **doorzoekbare** velden |
| [Zoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents) | `facet` | Kan alleen verwijzen naar **geschikt voor facetten** velden |
| [Zoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents) | `highlight` | Kan alleen verwijzen naar **doorzoekbare** velden |
| [Zoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents) | `searchFields` | Kan alleen verwijzen naar **doorzoekbare** velden |
| [Voorstellen](https://docs.microsoft.com/rest/api/searchservice/suggestions) en [automatisch aanvullen](https://docs.microsoft.com/rest/api/searchservice/autocomplete) | `searchFields` | Kan alleen verwijzen naar velden die deel van uitmaken een [suggestie](index-add-suggesters.md) |
| [Search](https://docs.microsoft.com/rest/api/searchservice/search-documents), [voorstellen](https://docs.microsoft.com/rest/api/searchservice/suggestions), en [automatisch aanvullen](https://docs.microsoft.com/rest/api/searchservice/autocomplete) | `$filter` | Kan alleen verwijzen naar **Filterbaar** velden |
| [Search](https://docs.microsoft.com/rest/api/searchservice/search-documents) en [voorstellen](https://docs.microsoft.com/rest/api/searchservice/suggestions) | `$orderby` | Kan alleen verwijzen naar **sorteerbaar** velden |
| [Search](https://docs.microsoft.com/rest/api/searchservice/search-documents), [voorstellen](https://docs.microsoft.com/rest/api/searchservice/suggestions), en [opzoeken](https://docs.microsoft.com/rest/api/searchservice/lookup-document) | `$select` | Kan alleen verwijzen naar **ophaalbaar** velden |

## <a name="constants"></a>Constanten

Constanten in OData worden letterlijke waarden van een bepaalde [Entity Data Model](https://docs.microsoft.com/dotnet/framework/data/adonet/entity-data-model) (EDP)-type. Zie [ondersteunde gegevenstypen](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) voor een lijst met ondersteunde typen in Azure Search. Constanten van verzamelingtypen worden niet ondersteund.

De volgende tabel ziet u voorbeelden van constanten voor elk van de gegevens die worden ondersteund door Azure Search:

| Gegevenstype | Voorbeeld van de constanten |
| --- | --- |
| `Edm.Boolean` | `true`, `false` |
| `Edm.DateTimeOffset` | `2019-05-06T12:30:05.451Z` |
| `Edm.Double` | `3.14159`, `-1.2e7`, `NaN`, `INF`, `-INF` |
| `Edm.GeographyPoint` | `geography'POINT(-122.131577 47.678581)'` |
| `Edm.GeographyPolygon` | `geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'` |
| `Edm.Int32` | `123`, `-456` |
| `Edm.Int64` | `283032927235` |
| `Edm.String` | `'hello'` |

De volgende EBNF ([uitgebreid Backus Naur formulier](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) definieert de grammatica voor het merendeel van de constanten die worden weergegeven in de bovenstaande tabel. De grammatica voor geo-ruimtelijk typen kunt u vinden in [OData, georuimtelijke functies in Azure Search](search-query-odata-geo-spatial-functions.md).

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
constant ::=
    string_literal
    | date_time_offset_literal
    | integer_literal
    | float_literal
    | boolean_literal
    | 'null'

string_literal ::= "'"([^'] | "''")*"'"

date_time_offset_literal ::= date_part'T'time_part time_zone

date_part ::= year'-'month'-'day

time_part ::= hour':'minute(':'second('.'fractional_seconds)?)?

zero_to_fifty_nine ::= [0-5]digit

digit ::= [0-9]

year ::= digit digit digit digit

month ::= '0'[1-9] | '1'[0-2]

day ::= '0'[1-9] | [1-2]digit | '3'[0-1]

hour ::= [0-1]digit | '2'[0-3]

minute ::= zero_to_fifty_nine

second ::= zero_to_fifty_nine

fractional_seconds ::= integer_literal

time_zone ::= 'Z' | sign hour':'minute

sign ::= '+' | '-'

/* In practice integer literals are limited in length to the precision of
the corresponding EDM data type. */
integer_literal ::= digit+

float_literal ::=
    sign? whole_part fractional_part? exponent?
    | 'NaN'
    | '-INF'
    | 'INF'

whole_part ::= integer_literal

fractional_part ::= '.'integer_literal

exponent ::= 'e' sign? integer_literal

boolean_literal ::= 'true' | 'false'
```

Een diagram van een interactieve syntaxis is ook beschikbaar:

> [!div class="nextstepaction"]
> [Diagram van de OData-syntaxis voor Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#constant)

> [!NOTE]
> Zie [naslaginformatie over expressiesyntaxis voor Azure Search OData](search-query-odata-syntax-reference.md) voor de volledige EBNF.

## <a name="building-expressions-from-field-paths-and-constants"></a>Building expressies van het veld paden en constanten

Veld paden en constanten zijn het meest eenvoudige deel van een OData-expressie, maar ze al volledige expressies zelf. In feite de **$select** parameter in Azure Search is alleen een door komma's gescheiden lijst met paden veld, en **$orderby** is niet veel ingewikkelder dan **$select**. Als u een veld van het type `Edm.Boolean` in uw index, kunt u ook een filter dat niets, maar het pad van dat veld te schrijven. De constanten `true` en `false` worden eveneens geldig filters.

Echter, de meeste gevallen moet u meer complexe expressies die naar meer dan één veld en constante verwijzen. Deze expressies zijn gebouwd op verschillende manieren, afhankelijk van de parameter.

De volgende EBNF ([uitgebreid Backus Naur formulier](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) definieert de grammatica voor de **$filter**, **$orderby**, en **$select** parameters. Deze zijn gebouwd op basis van eenvoudiger expressies die naar paden veld en constanten verwijzen:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
filter_expression ::= boolean_expression

order_by_expression ::= order_by_clause(',' order_by_clause)*

select_expression ::= '*' | field_path(',' field_path)*
```

Een diagram van een interactieve syntaxis is ook beschikbaar:

> [!div class="nextstepaction"]
> [Diagram van de OData-syntaxis voor Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#filter_expression)

> [!NOTE]
> Zie [naslaginformatie over expressiesyntaxis voor Azure Search OData](search-query-odata-syntax-reference.md) voor de volledige EBNF.

De **$orderby** en **$select** parameters zijn beide met door komma's gescheiden lijsten zijn van eenvoudiger expressies. De **$filter** parameter is een Booleaanse expressie die is samengesteld, van de onderliggende expressies eenvoudiger. Deze onderliggende expressies worden gecombineerd met behulp van de logische operators zoals [ `and`, `or`, en `not` ](search-query-odata-logical-operators.md), vergelijkingsoperators zoals [ `eq`, `lt`, `gt`, enzovoort](search-query-odata-comparison-operators.md), en verzameling operatoren zoals [ `any` en `all` ](search-query-odata-collection-operators.md).

De **$filter**, **$orderby**, en **$select** parameters worden verkend gedetailleerd besproken in de volgende artikelen:

- [Syntaxis voor OData $filter in Azure Search](search-query-odata-filter.md)
- [Syntaxis voor OData $orderby in Azure Search](search-query-odata-orderby.md)
- [Syntaxis voor OData $select in Azure Search](search-query-odata-select.md)

## <a name="see-also"></a>Zie ook  

- [Facetnavigatie in Azure Search](search-faceted-navigation.md)
- [Filters in Azure Search](search-filters.md)
- [Documenten zoeken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
- [Lucene-querysyntaxis](query-lucene-syntax.md)
- [Vereenvoudigde querysyntaxis in Azure Search](query-simple-syntax.md)
