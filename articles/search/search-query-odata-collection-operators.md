---
title: Referentie voor OData-operator - Azure Search
description: OData-verzameling operators, alle, en lambda-expressies in Azure Search-query's.
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
ms.openlocfilehash: 7afafe158732b14ebe314eeee5d015acddc55b72
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079936"
---
# <a name="odata-collection-operators-in-azure-search---any-and-all"></a>OData-verzameling operators in Azure Search - `any` en `all`

Bij het schrijven van een [OData-filterexpressie](query-odata-filter-orderby-syntax.md) als u wilt gebruiken met Azure Search, is het vaak nuttig om te filteren in de verzameling velden. U kunt dit doen met behulp van de `any` en `all` operators.

## <a name="syntax"></a>Syntaxis

De volgende EBNF ([uitgebreid Backus Naur formulier](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) definieert de grammatica van een OData-expressie die gebruikmaakt van `any` of `all`.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
collection_filter_expression ::=
    field_path'/all(' lambda_expression ')'
    | field_path'/any(' lambda_expression ')'
    | field_path'/any()'

lambda_expression ::= identifier ':' boolean_expression
```

Een diagram van een interactieve syntaxis is ook beschikbaar:

> [!div class="nextstepaction"]
> [Diagram van de OData-syntaxis voor Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#collection_filter_expression)

> [!NOTE]
> Zie [naslaginformatie over expressiesyntaxis voor Azure Search OData](search-query-odata-syntax-reference.md) voor de volledige EBNF.

Er zijn drie soorten expressie filteren van verzamelingen.

- De eerste twee herhalen van een veld verzameling, het toepassen van een predikaat is opgegeven in de vorm van een lambda-expressie op elk element van de verzameling.
  - Een expressie met `all` retourneert `true` als het predikaat ingesteld op true voor elk element van de verzameling is.
  - Een expressie met `any` retourneert `true` als het predikaat ingesteld op true voor ten minste één element van de verzameling is.
- De derde vorm van verzameling filteren gebruikt `any` zonder een lambda-expressie om te testen of een verzameling veld leeg is. Als de verzameling worden elementen bevat, retourneert deze `true`. Als de verzameling leeg is, retourneert deze `false`.

Een **lambda-expressie** in een verzameling filter is, zoals de hoofdtekst van een lus in een programmeertaal. Een variabele, wordt gedefinieerd met de naam de **bereik variabele**, dat het huidige element van de verzameling tijdens herhaling bevat. Het definieert ook een andere Booleaanse expressie die wordt de filtercriteria om toe te passen aan de variabele bereik voor elk element van de verzameling.

## <a name="examples"></a>Voorbeelden

Overeenkomst documenten waarvan `tags` veld exact de tekenreeks 'Wi-Fi' bevat:

    tags/any(t: t eq 'wifi')

Overeenkomst documenten waar elk element van de `ratings` veld valt tussen 3 en 5 liggen:

    ratings/all(r: r ge 3 and r le 5)

Overeenkomst documenten waarin een van de geo-coördinaten in het `locations` veld binnen de opgegeven veelhoek is:

    locations/any(loc: geo.intersects(loc, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))

Overeenkomst documenten waar de `rooms` veld leeg is:

    not rooms/any()

Overeenkomen met documenten wanneer voor alle ruimten de `rooms/amenities` veld bevat "' tv-samenvatting en `rooms/baseRate` is minder dan 100:

    rooms/all(room: room/amenities/any(a: a eq 'tv') and room/baseRate lt 100.0)

## <a name="limitations"></a>Beperkingen

Niet elke functie van filterexpressies is beschikbaar in de hoofdtekst van een lambda-expressie. De beperkingen verschillen, afhankelijk van het gegevenstype van de verzameling-veld dat u wilt filteren. De volgende tabel geeft een overzicht van de beperkingen.

[!INCLUDE [Limitations on OData lambda expressions in Azure Search](../../includes/search-query-odata-lambda-limitations.md)]

Zie voor meer informatie over deze beperkingen, evenals de voorbeelden [probleemoplossing filters voor bestandsverzameling in Azure Search](search-query-troubleshoot-collection-filters.md). Voor meer gedetailleerde informatie over waarom deze beperkingen bestaat, Zie [informatie over filters voor bestandsverzameling in Azure Search](search-query-understand-collection-filters.md).

## <a name="next-steps"></a>Volgende stappen  

- [Filters in Azure Search](search-filters.md)
- [Overzicht van taal van OData-expressie voor Azure Search](query-odata-filter-orderby-syntax.md)
- [Naslaginformatie over de syntaxis van de OData-expressie voor Azure Search](search-query-odata-syntax-reference.md)
- [Documenten zoeken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
