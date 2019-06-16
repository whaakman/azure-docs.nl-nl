---
title: Naslag OData - zoekopdracht in volledige tekst voor Azure Search
description: OData zoeken in volledige tekst-functies, search.ismatch en search.ismatchscoring in Azure Search-query's.
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
ms.openlocfilehash: 158312a7afe88e7b9885376c5d28b01958acbbfb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079806"
---
# <a name="odata-full-text-search-functions-in-azure-search---searchismatch-and-searchismatchscoring"></a>OData zoeken in volledige tekst functies in Azure Search - `search.ismatch` en `search.ismatchscoring`

Azure Search biedt ondersteuning voor zoeken in volledige tekst in de context van [OData-filterexpressies](query-odata-filter-orderby-syntax.md) via de `search.ismatch` en `search.ismatchscoring` functies. Deze functies kunnen u zoeken in volledige tekst worden gecombineerd met strikte Booleaanse filteren op een manier die niet mogelijk zijn alleen met behulp van het hoogste niveau `search` parameter van de [zoeken-API](https://docs.microsoft.com/rest/api/searchservice/search-documents).

> [!NOTE]
> De `search.ismatch` en `search.ismatchscoring` functies worden alleen ondersteund in de filters in de [zoeken-API](https://docs.microsoft.com/rest/api/searchservice/search-documents). Ze worden niet ondersteund in de [voorstellen](https://docs.microsoft.com/rest/api/searchservice/suggestions) of [automatisch aanvullen](https://docs.microsoft.com/rest/api/searchservice/autocomplete) API's.

## <a name="syntax"></a>Syntaxis

De volgende EBNF ([uitgebreid Backus Naur formulier](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) definieert de grammatica van de `search.ismatch` en `search.ismatchscoring` functies:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
search_is_match_call ::=
    'search.ismatch'('scoring')?'(' search_is_match_parameters ')'

search_is_match_parameters ::=
    string_literal(',' string_literal(',' query_type ',' search_mode)?)?

query_type ::= "'full'" | "'simple'"

search_mode ::= "'any'" | "'all'"
```

Een diagram van een interactieve syntaxis is ook beschikbaar:

> [!div class="nextstepaction"]
> [Diagram van de OData-syntaxis voor Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#search_is_match_call)

> [!NOTE]
> Zie [naslaginformatie over expressiesyntaxis voor Azure Search OData](search-query-odata-syntax-reference.md) voor de volledige EBNF.

### <a name="searchismatch"></a>Search.ismatch

De `search.ismatch` functie evalueert een query zoeken in volledige tekst als onderdeel van een filterexpressie. De documenten die overeenkomen met de zoekopdracht worden geretourneerd in de resultatenset. De volgende overloads van deze functie zijn beschikbaar:

- `search.ismatch(search)`
- `search.ismatch(search, searchFields)`
- `search.ismatch(search, searchFields, queryType, searchMode)`

De parameters zijn gedefinieerd in de volgende tabel:

| Parameternaam | Type | Description |
| --- | --- | --- |
| `search` | `Edm.String` | De zoekquery (in een [eenvoudige](query-simple-syntax.md) of [volledige](query-lucene-syntax.md) Lucene-querysyntaxis). |
| `searchFields` | `Edm.String` | Door komma's gescheiden lijst met doorzoekbare velden om te zoeken. de standaardinstelling alle doorzoekbare velden in de index. Bij het gebruik van [fielded zoeken](query-lucene-syntax.md#bkmk_fields) in de `search` parameter, de specificaties van het veld in de query Lucene overschrijven alle velden die in deze parameter is opgegeven. |
| `queryType` | `Edm.String` | `'simple'` of `'full'`; standaard ingesteld op `'simple'`. Hiermee geeft u op welke querytaal is gebruikt in de `search` parameter. |
| `searchMode` | `Edm.String` | `'any'` of `'all'`, standaard ingesteld op `'any'`. Geeft aan of een of meer van de zoekopdracht voorwaarden de `search` parameter moet overeenkomen om op te tellen van het document als een overeenkomst. Wanneer u de [Lucene Booleaanse operators](query-lucene-syntax.md#bkmk_boolean) in de `search` parameter, ze voorrang krijgt boven deze parameter. |

De bovenstaande parameters gelijk zijn aan de bijbehorende [aanvraagparameters zoeken in de Search-API](https://docs.microsoft.com/rest/api/searchservice/search-documents).

De `search.ismatch` functie retourneert een waarde van het type `Edm.Boolean`, waarmee u voor het opstellen van het met andere onderliggende filterexpressies met behulp van de Booleaanse waarde [logische operators](search-query-odata-logical-operators.md).

> [!NOTE]
> Azure Search biedt geen ondersteuning voor het gebruik `search.ismatch` of `search.ismatchscoring` binnen lambda-expressies. Dit betekent dat het is niet mogelijk is het schrijven van filters op verzamelingen van objecten die van zoeken in volledige tekst overeenkomsten met strikte filterovereenkomsten op hetzelfde object correleren kunnen. Zie voor meer informatie over deze beperking, evenals de voorbeelden [probleemoplossing filters voor bestandsverzameling in Azure Search](search-query-troubleshoot-collection-filters.md). Voor meer gedetailleerde informatie over waarom deze beperking bestaat, Zie [informatie over filters voor bestandsverzameling in Azure Search](search-query-understand-collection-filters.md).


### <a name="searchismatchscoring"></a>Search.ismatchscoring

De `search.ismatchscoring` functioneren, zoals de `search.ismatch` functioneren, retourneert `true` voor documenten die overeenkomen met de query van zoeken in volledige tekst als parameter doorgegeven. Het verschil is dat de relevantie score van documenten die overeenkomt met de `search.ismatchscoring` query dragen bij aan de totale score document, terwijl in het geval van `search.ismatch`, de document-score niet meer gewijzigd. De volgende overloads van deze functie zijn beschikbaar met parameters die identiek zijn aan die van `search.ismatch`:

- `search.ismatchscoring(search)`
- `search.ismatchscoring(search, searchFields)`
- `search.ismatchscoring(search, searchFields, queryType, searchMode)`

Zowel de `search.ismatch` en `search.ismatchscoring` functies in de dezelfde filterexpressie kunnen worden gebruikt.

## <a name="examples"></a>Voorbeelden

Documenten met het woord "afgebakend" zoeken. Dit filterquery is gelijk aan een [zoekaanvraag](https://docs.microsoft.com/rest/api/searchservice/search-documents) met `search=waterfront`.

    search.ismatchscoring('waterfront')

Documenten met het woord 'hostel' en waardering groter of gelijk zijn aan 4 of documenten met het woord "motel" en waardering gelijk is aan 5 zoeken. Opmerking: deze aanvraag kan niet worden weergegeven zonder de `search.ismatchscoring` functie.

    search.ismatchscoring('hostel') and Rating ge 4 or search.ismatchscoring('motel') and Rating eq 5

Documenten zonder het woord "luxe" zoeken.

    not search.ismatch('luxury')

Documenten met de woordgroep in de Indische Oceaan "view" of de classificatie die gelijk is aan 5 zoeken. De `search.ismatchscoring` query wordt uitgevoerd alleen op basis van velden `HotelName` en `Rooms/Description`.

Documenten die overeenkomen met alleen de tweede component van de splitsing wordt geretourneerd te--hotels met `Rating` gelijk is aan 5. Om deze te maken dat deze documenten niet overeenkomt met een van de beoordeelde onderdelen van de expressie, wordt deze geretourneerd met score gelijk zijn aan nul wissen.

    search.ismatchscoring('"ocean view"', 'Rooms/Description,HotelName') or Rating eq 5

Zoeken naar documenten, waarbij de termen "Hotels" en "luchthaven" zijn binnen 5 woorden uit elkaar in de beschrijving van het hotel en waar soorten is niet toegestaan in ten minste enkele van de ruimten. Deze query gebruikt de [volledige Lucene-querytaal](query-lucene-syntax.md).

    search.ismatch('"hotel airport"~5', 'Description', 'full', 'any') and Rooms/any(room: not room/SmokingAllowed)

## <a name="next-steps"></a>Volgende stappen  

- [Filters in Azure Search](search-filters.md)
- [Overzicht van taal van OData-expressie voor Azure Search](query-odata-filter-orderby-syntax.md)
- [Naslaginformatie over de syntaxis van de OData-expressie voor Azure Search](search-query-odata-syntax-reference.md)
- [Documenten zoeken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
