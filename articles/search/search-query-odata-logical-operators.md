---
title: OData-referentie voor logische operator - Azure Search
description: OData logische operators, en, of, en niet in Azure Search-query's.
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
ms.openlocfilehash: de93765117b4cafe70e5ad277e32ca0a1fa8d90a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079767"
---
# <a name="odata-logical-operators-in-azure-search---and-or-not"></a>OData logische operators in Azure Search - `and`, `or`, `not`

[OData-filterexpressies](query-odata-filter-orderby-syntax.md) in Azure Search worden Booleaanse expressies die worden geëvalueerd tot `true` of `false`. U kunt een complexe filter schrijven met het schrijven van een reeks [eenvoudiger filters](search-query-odata-comparison-operators.md) en samenstellen met behulp van de logische operators van [Booleaanse waarde wiskundige](https://en.wikipedia.org/wiki/Boolean_algebra):

- `and`: Een binaire operator die wordt geëvalueerd als `true` als zowel de linker- en onderliggende expressies `true`.
- `or`: Een binaire operator die wordt geëvalueerd als `true` als een van de onderliggende expressies naar links of rechts resulteert in `true`.
- `not`: Een unaire operator die wordt geëvalueerd als `true` als de subquery expressie wordt geëvalueerd als `false`, en vice versa.

Deze samen met de [verzameling operators `any` en `all` ](search-query-odata-collection-operators.md), kunt u filters die kunnen zeer complex zoekcriteria express maken.

## <a name="syntax"></a>Syntaxis

De volgende EBNF ([uitgebreid Backus Naur formulier](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) definieert de grammatica van een OData-expressie die gebruikmaakt van de logische operators.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
logical_expression ::=
    boolean_expression ('and' | 'or') boolean_expression
    | 'not' boolean_expression
```

Een diagram van een interactieve syntaxis is ook beschikbaar:

> [!div class="nextstepaction"]
> [Diagram van de OData-syntaxis voor Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#logical_expression)

> [!NOTE]
> Zie [naslaginformatie over expressiesyntaxis voor Azure Search OData](search-query-odata-syntax-reference.md) voor de volledige EBNF.

Er zijn twee soorten logische expressies: binair (`and`/`or`), waarbij er twee onderliggende expressies en unaire zijn (`not`), waarbij er slechts één. De onderliggende expressies kunnen worden Booleaanse expressies van welke aard dan ook:

- Variabelen voor de velden of het bereik van het type `Edm.Boolean`
- Functies die resulteren in waarden van het type `Edm.Boolean`, zoals `geo.intersects` of `search.ismatch`
- [Vergelijkingsexpressies](search-query-odata-comparison-operators.md), zoals `rating gt 4`
- [Verzameling expressies](search-query-odata-collection-operators.md), zoals `Rooms/any(room: room/Type eq 'Deluxe Room')`
- De letterlijke Booleaanse `true` of `false`.
- Andere logische expressies die zijn gebouwd met behulp van `and`, `or`, en `not`.

> [!IMPORTANT]
> Er zijn bepaalde situaties waarbij niet alle soorten subexpressie kunnen worden gebruikt met `and` / `or`, met name in lambda-expressies. Zie [operators voor OData-verzameling in Azure Search](search-query-odata-collection-operators.md#limitations) voor meer informatie.

### <a name="logical-operators-and-null"></a>Logische operators en `null`

De meeste Booleaanse expressies, zoals functies en vergelijkingen niet produceren `null` waarden en de logische operators kunnen niet worden toegepast op de `null` rechtstreeks letterlijke waarde (bijvoorbeeld `x and null` is niet toegestaan). Booleaanse waarden kunnen echter zijn `null`, dus moet u rekening houden met hoe de `and`, `or`, en `not` operators zich gedragen bij null. Deze worden samengevat in de volgende tabel, waarbij `b` is een veld van het type `Edm.Boolean`:

| expressie | Resultaat wanneer `b` is `null` |
| --- | --- |
| `b` | `false` |
| `not b` | `true` |
| `b eq true` | `false` |
| `b eq false` | `false` |
| `b eq null` | `true` |
| `b ne true` | `true` |
| `b ne false` | `true` |
| `b ne null` | `false` |
| `b and true` | `false` |
| `b and false` | `false` |
| `b or true` | `true` |
| `b or false` | `false` |

Wanneer een Boole-veld `b` wordt weergegeven door zelf in een filterexpressie gedraagt alsof ze waren is geschreven `b eq true`, dus als `b` is `null`, de expressie wordt geëvalueerd als `false`. Op deze manier `not b` gedraagt zich als `not (b eq true)`, zodat dit resulteert in `true`. Op deze manier `null` velden werken op dezelfde manier als `false`. Dit komt overeen met hoe ze zich gedragen in combinatie met andere expressies met `and` en `or`, zoals wordt weergegeven in de bovenstaande tabel. Ondanks dat dit een directe vergelijking `false` (`b eq false`) wordt nog steeds worden geëvalueerd tot `false`. Met andere woorden, `null` is niet gelijk aan `false`, zelfs als deze zich als deze in Booleaanse expressies gedraagt.

## <a name="examples"></a>Voorbeelden

Overeenkomst documenten waar de `rating` veld is tussen de 3 en 5 liggen:

    rating ge 3 and rating le 5

Overeenkomst documenten waar alle elementen van de `ratings` veld zijn minder dan 3 of groter is dan 5:

    ratings/all(r: r lt 3 or r gt 5)

Overeenkomst documenten waar de `location` veld binnen de opgegeven veelhoek is en het document bevat de term 'openbaar'.

    geo.intersects(location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))') and not search.ismatch('public')

Documenten voor hotels in Vancouver, Canada, waarbij er een deluxe ruimte met een base minder dan 160 beoordelen:

    Address/City eq 'Vancouver' and Address/Country eq 'Canada' and Rooms/any(room: room/Type eq 'Deluxe Room' and room/BaseRate lt 160)

## <a name="next-steps"></a>Volgende stappen  

- [Filters in Azure Search](search-filters.md)
- [Overzicht van taal van OData-expressie voor Azure Search](query-odata-filter-orderby-syntax.md)
- [Naslaginformatie over de syntaxis van de OData-expressie voor Azure Search](search-query-odata-syntax-reference.md)
- [Documenten zoeken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
