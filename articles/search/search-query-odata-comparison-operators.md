---
title: OData-vergelijking van de operator referentie - Azure Search
description: OData-vergelijkingsoperators eq, ne, gt, lt, ge en le in Azure Search-query's.
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
ms.openlocfilehash: b51bf3d77283ae828f47fdb0355d2deb43f071a1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079923"
---
# <a name="odata-comparison-operators-in-azure-search---eq-ne-gt-lt-ge-and-le"></a>OData-vergelijkingsoperators in Azure Search - `eq`, `ne`, `gt`, `lt`, `ge`, en `le`

De meest eenvoudige bewerking in een [OData-filterexpressie](query-odata-filter-orderby-syntax.md) in Azure Search is een veld in een bepaalde waarde wilt vergelijken. Er zijn twee soorten vergelijking mogelijk--gelijkheidsvergelijking en bereik vergelijking. U kunt de volgende operatoren gebruiken om te vergelijken van een veld met een constante waarde:

Gelijkheid operators:

- `eq`: Test of een veld is **gelijk zijn aan** een constante waarde
- `ne`: Test of een veld is **niet gelijk zijn aan** een constante waarde

Bereik-operators:

- `gt`: Test of een veld is **groter is dan** een constante waarde
- `lt`: Test of een veld is **minder dan** een constante waarde
- `ge`: Test of een veld is **groter is dan of gelijk zijn aan** een constante waarde
- `le`: Test of een veld is **kleiner dan of gelijk zijn aan** een constante waarde

U kunt de bereik-operators gebruiken in combinatie met de [logische operators](search-query-odata-logical-operators.md) om te testen of een veld binnen een bepaald bereik van waarden is. Zie de [voorbeelden](#examples) verderop in dit artikel.

> [!NOTE]
> Als u liever, kunt u de constante waarde aan de linkerkant van de operator en de naam van het veld aan de rechterkant plaatsen. Voor operators van bereik, is de betekenis van de vergelijking omgekeerd. Bijvoorbeeld, als de constante waarde aan de linkerkant `gt` wilt testen of de constante waarde groter dan het veld is. U kunt ook de vergelijkingsoperators gebruiken om te vergelijken van het resultaat van een functie, zoals `geo.distance`, met een waarde. Voor Boolean functies, zoals `search.ismatch`, vergelijken van het resultaat dat `true` of `false` is optioneel.

## <a name="syntax"></a>Syntaxis

De volgende EBNF ([uitgebreid Backus Naur formulier](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) definieert de grammatica van een OData-expressie die gebruikmaakt van de vergelijkingsoperators op.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
comparison_expression ::=
    variable_or_function comparison_operator constant |
    constant comparison_operator variable_or_function

variable_or_function ::= variable | function_call

comparison_operator ::= 'gt' | 'lt' | 'ge' | 'le' | 'eq' | 'ne'
```

Een diagram van een interactieve syntaxis is ook beschikbaar:

> [!div class="nextstepaction"]
> [Diagram van de OData-syntaxis voor Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#comparison_expression)

> [!NOTE]
> Zie [naslaginformatie over expressiesyntaxis voor Azure Search OData](search-query-odata-syntax-reference.md) voor de volledige EBNF.

Er zijn twee soorten vergelijkingsexpressies. Het enige verschil tussen deze is of de constante wordt weergegeven op de of rechts-rechter-aan de linkerkant van de operator. De expressie aan de andere kant van de operator moet een **variabele** of een aanroep van de functie. Een variabele mag bestaan uit de naam van een veld of een bereik-variabele in het geval van een [lambda-expressie](search-query-odata-collection-operators.md).

## <a name="data-types-for-comparisons"></a>Gegevenstypen voor vergelijkingen

De gegevenstypen aan beide zijden van een vergelijkingsoperator moeten compatibel zijn. Bijvoorbeeld, als aan de linkerkant is een veld van het type `Edm.DateTimeOffset`, en vervolgens aan de rechterkant een datum / tijd-constante zijn moet. Numerieke gegevenstypen zijn flexibeler. U kunt variabelen en functies van een numeriek type met constanten van andere numerieke typen, met enkele beperkingen, vergelijken, zoals beschreven in de volgende tabel.

| Type variabele of functie | Constante waardetype | Beperkingen |
| --- | --- | --- |
| `Edm.Double` | `Edm.Double` | Vergelijking is onderhevig aan [speciale regels voor `NaN`](#special-case-nan) |
| `Edm.Double` | `Edm.Int64` | Constante wordt geconverteerd naar `Edm.Double`, wat resulteert in een verlies van precisie voor de waarden van grote omvang |
| `Edm.Double` | `Edm.Int32` | N.v.t. |
| `Edm.Int64` | `Edm.Double` | Vergelijkingen tussen `NaN`, `-INF`, of `INF` zijn niet toegestaan |
| `Edm.Int64` | `Edm.Int64` | N.v.t. |
| `Edm.Int64` | `Edm.Int32` | Constante wordt geconverteerd naar `Edm.Int64` voor vergelijking |
| `Edm.Int32` | `Edm.Double` | Vergelijkingen tussen `NaN`, `-INF`, of `INF` zijn niet toegestaan |
| `Edm.Int32` | `Edm.Int64` | N.v.t. |
| `Edm.Int32` | `Edm.Int32` | N.v.t. |

Voor de vergelijkingen die niet zijn toegestaan, bijvoorbeeld het vergelijken van een veld van het type `Edm.Int64` naar `NaN`, de Azure Search REST-API retourneert een "HTTP 400: Ongeldige aanvraag"fout.

> [!IMPORTANT]
> Hoewel numeriek type vergelijkingen flexibel zijn, is het raadzaam vergelijkingen schrijven in de filters, zodat de constante waarde is van hetzelfde gegevenstype als de variabele of functie waaraan het wordt vergeleken. Dit is vooral belangrijk wanneer combineren met drijvende komma en gehele getallen, waar de impliciete conversies die minder nauwkeurig mogelijk zijn.

<a name="special-case-nan"></a>

### <a name="special-cases-for-null-and-nan"></a>Speciale gevallen voor `null` en `NaN`

Wanneer u vergelijkingsoperators, is het belangrijk om te onthouden dat alle niet-verzameling velden in Azure Search mogelijk kunnen worden `null`. De volgende tabel toont de mogelijke resultaten voor een van de vergelijkingsexpressie waar beide kanten kan worden `null`:

| Operator | Resultaat wanneer alleen het veld of de variabele is `null` | Resultaat wanneer alleen de constante is `null` | Resultaat als het veld of de variabele en de constante zijn `null` |
| --- | --- | --- | --- |
| `gt` | `false` | HTTP 400: Ongeldige aanvraag-fout | HTTP 400: Ongeldige aanvraag-fout |
| `lt` | `false` | HTTP 400: Ongeldige aanvraag-fout | HTTP 400: Ongeldige aanvraag-fout |
| `ge` | `false` | HTTP 400: Ongeldige aanvraag-fout | HTTP 400: Ongeldige aanvraag-fout |
| `le` | `false` | HTTP 400: Ongeldige aanvraag-fout | HTTP 400: Ongeldige aanvraag-fout |
| `eq` | `false` | `false` | `true` |
| `ne` | `true` | `true` | `false` |

Kortom, `null` gelijk is aan zichzelf en is niet kleiner of groter is dan een andere waarde.

Als uw index velden van het type heeft `Edm.Double` en uploadt u `NaN` waarden aan deze velden, moet u rekening voor die bij het schrijven van filters. Azure Search implementeert de IEEE 754-standaard voor het verwerken van `NaN` waarden en vergelijkingen met dergelijke waarden niet duidelijk resultaten opleveren zoals wordt weergegeven in de volgende tabel.

| Operator | Resultaat wanneer ten minste één operand `NaN` |
| --- | --- |
| `gt` | `false` |
| `lt` | `false` |
| `ge` | `false` |
| `le` | `false` |
| `eq` | `false` |
| `ne` | `true` |

Kortom, `NaN` is niet gelijk aan een willekeurige waarde, waaronder zelf.

### <a name="comparing-geo-spatial-data"></a>Georuimtelijke gegevens vergelijken

U kunt geen rechtstreeks vergelijken met een veld van het type `Edm.GeographyPoint` met een constante waarde, maar u kunt de `geo.distance` functie. Deze functie retourneert een waarde van het type `Edm.Double`, zodat u deze met een numerieke vergelijken kunt constante om te filteren op basis van de afstand van constante georuimtelijke coördinaten. Zie de [voorbeelden](#examples) hieronder.

### <a name="comparing-string-data"></a>Tekenreeksgegevens vergelijken

Tekenreeksen kunnen worden vergeleken in de filters voor exacte overeenkomsten met behulp van de `eq` en `ne` operators. Deze vergelijkingen zijn hoofdlettergevoelig.

## <a name="examples"></a>Voorbeelden

Overeenkomst documenten waar de `Rating` veld is tussen de 3 en 5 liggen:

    Rating ge 3 and Rating le 5

Overeenkomst documenten waar de `Location` veld is minder dan 2 kilometer zijn verwijderd van de opgegeven lengtegraad en breedtegraad:

    geo.distance(Location, geography'POINT(-122.031577 47.578581)') lt 2.0

Overeenkomst documenten waar de `LastRenovationDate` veld is groter dan of gelijk zijn aan 1 januari 2015, middernacht UTC:

    LastRenovationDate ge 2015-01-01T00:00:00.000Z

Overeenkomst documenten waar de `Details/Sku` veld is niet `null`:

    Details/Sku ne null

Documenten voor hotels waar ten minste één ruimte type 'Deluxe ruimte', waarbij heeft de tekenreeks van de `Rooms/Type` veld precies overeenkomen met het filter:

    Rooms/any(room: room/Type eq 'Deluxe Room')

## <a name="next-steps"></a>Volgende stappen  

- [Filters in Azure Search](search-filters.md)
- [Overzicht van taal van OData-expressie voor Azure Search](query-odata-filter-orderby-syntax.md)
- [Naslaginformatie over de syntaxis van de OData-expressie voor Azure Search](search-query-odata-syntax-reference.md)
- [Documenten zoeken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
