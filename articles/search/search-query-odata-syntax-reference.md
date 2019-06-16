---
title: OData naslaginformatie over expressiesyntaxis - Azure Search
description: Formele grammatica en de syntaxis van de specificatie voor OData-expressies in Azure Search-query's.
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
ms.openlocfilehash: cccfb749af07d1deeeda6e94de9c2cd5ce5396f3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079663"
---
# <a name="odata-expression-syntax-reference-for-azure-search"></a>Naslaginformatie over de syntaxis van de OData-expressie voor Azure Search

Maakt gebruik van Azure Search [OData-expressies](http://docs.oasis-open.org/odata/odata/v4.01/odata-v4.01-part2-url-conventions.html) als parameters in de API. Meestal OData-expressies worden gebruikt voor de `$orderby` en `$filter` parameters. Deze expressies kunnen lastig zijn, met meerdere componenten, functies en operatoren. Echter, zelfs voor eenvoudige OData-expressies zoals eigenschap paden worden gebruikt in een groot gedeelte van de Azure Search REST-API. Bijvoorbeeld: padexpressies worden gebruikt om te verwijzen naar de onderliggende velden van complexe velden overal in de API, zoals wanneer aanbieding subplan velden in een [suggestie](index-add-suggesters.md), een [scoring-functie](index-add-scoring-profiles.md), wordt de `$select` parameter , of zelfs [fielded zoeken in query's Lucene](query-lucene-syntax.md).

Dit artikel beschrijft alle volgende vormen van OData-expressies met behulp van een formele grammatica. Er is ook een [interactieve diagram](#syntax-diagram) om u te helpen visueel verkennen van de grammatica.

## <a name="formal-grammar"></a>Formele grammatica

Er wordt beschreven en de subset met de OData-taal die wordt ondersteund door Azure Search met behulp van een EBNF ([uitgebreid Backus Naur formulier](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) grammatica. Regels worden "boven naar beneden", beginnend met de meest complexe expressies en afbreken meer primitieve expressies weergegeven. Aan de bovenkant zijn de grammaticaregels die met bepaalde parameters van de Azure Search REST-API overeenkomen:

- [`$filter`](search-query-odata-filter.md), gedefinieerd door de `filter_expression` regel.
- [`$orderby`](search-query-odata-orderby.md), gedefinieerd door de `order_by_expression` regel.
- [`$select`](search-query-odata-select.md), gedefinieerd door de `select_expression` regel.
- Veld paden, die zijn gedefinieerd door de `field_path` regel. Veld paden worden in de API gebruikt. Ze kunnen verwijzen naar een van beide velden op het hoogste niveau van een index of in onderliggende velden met een of meer [complexe veld](search-howto-complex-data-types.md) bovenliggende elementen.

Nadat de EBNF een doorzoekbaar is [syntaxisschema](https://en.wikipedia.org/wiki/Syntax_diagram) waarmee u interactief verkennen van de grammatica en de relaties tussen de regels.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
/* Top-level rules */

filter_expression ::= boolean_expression

order_by_expression ::= order_by_clause(',' order_by_clause)*

select_expression ::= '*' | field_path(',' field_path)*

field_path ::= identifier('/'identifier)*


/* Shared base rules */

identifier ::= [a-zA-Z_][a-zA-Z_0-9]*


/* Rules for $orderby */

order_by_clause ::= (field_path | sortable_function) ('asc' | 'desc')?

sortable_function ::= geo_distance_call | 'search.score()'


/* Rules for $filter */

boolean_expression ::=
    collection_filter_expression
    | logical_expression
    | comparison_expression
    | boolean_literal
    | boolean_function_call
    | '(' boolean_expression ')'
    | variable

/* This can be a range variable in the case of a lambda, or a field path. */
variable ::= identifier | field_path

collection_filter_expression ::=
    field_path'/all(' lambda_expression ')'
    | field_path'/any(' lambda_expression ')'
    | field_path'/any()'

lambda_expression ::= identifier ':' boolean_expression

logical_expression ::=
    boolean_expression ('and' | 'or') boolean_expression
    | 'not' boolean_expression

comparison_expression ::= 
    variable_or_function comparison_operator constant | 
    constant comparison_operator variable_or_function

variable_or_function ::= variable | function_call

comparison_operator ::= 'gt' | 'lt' | 'ge' | 'le' | 'eq' | 'ne'


/* Rules for constants and literals */

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


/* Rules for functions */

function_call ::=
    geo_distance_call |
    boolean_function_call

geo_distance_call ::=
    'geo.distance(' variable ',' geo_point ')'
    | 'geo.distance(' geo_point ',' variable ')'

geo_point ::= "geography'POINT(" lon_lat ")'"

lon_lat ::= float_literal ' ' float_literal

boolean_function_call ::=
    geo_intersects_call |
    search_in_call |
    search_is_match_call

geo_intersects_call ::=
    'geo.intersects(' variable ',' geo_polygon ')'

/* You need at least four points to form a polygon, where the first and
last points are the same. */
geo_polygon ::=
    "geography'POLYGON((" lon_lat ',' lon_lat ',' lon_lat ',' lon_lat_list "))'"

lon_lat_list ::= lon_lat(',' lon_lat)*

search_in_call ::=
    'search.in(' variable ',' string_literal(',' string_literal)? ')'

/* Note that it is illegal to call search.ismatch or search.ismatchscoring
from inside a lambda expression. */
search_is_match_call ::=
    'search.ismatch'('scoring')?'(' search_is_match_parameters ')'

search_is_match_parameters ::=
    string_literal(',' string_literal(',' query_type ',' search_mode)?)?

query_type ::= "'full'" | "'simple'"

search_mode ::= "'any'" | "'all'"
```

## <a name="syntax-diagram"></a>Syntaxisschema

Als u wilt visueel verkennen van de OData-taal-grammatica wordt ondersteund door Azure Search, kunt u proberen het interactieve syntaxisschema:

> [!div class="nextstepaction"]
> [Diagram van de OData-syntaxis voor Azure Search](https://azuresearch.github.io/odata-syntax-diagram/)

## <a name="see-also"></a>Zie ook  

- [Filters in Azure Search](search-filters.md)
- [Documenten zoeken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
- [Lucene-querysyntaxis](query-lucene-syntax.md)
- [Vereenvoudigde querysyntaxis in Azure Search](query-simple-syntax.md)
