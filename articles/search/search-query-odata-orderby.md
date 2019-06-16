---
title: OData-sorteren referentie - Azure Search
description: Naslaggids voor OData-syntaxis, voor order in Azure Search-query's.
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
ms.openlocfilehash: 1ced35dc73e6d596fbeda32590ab0b69df396c5c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079754"
---
# <a name="odata-orderby-syntax-in-azure-search"></a>Syntaxis voor OData $orderby in Azure Search

 U kunt de [OData **$orderby** parameter](query-odata-filter-orderby-syntax.md) om toe te passen van een aangepaste sorteervolgorde voor zoekresultaten in Azure Search. In dit artikel beschrijft de syntaxis van **$orderby** in detail. Voor meer algemene informatie over hoe u **$orderby** wanneer zoekresultaten wordt gepresenteerd, Zie [over het werken met zoeken in Azure Search resultaten](search-pagination-page-layout.md).

## <a name="syntax"></a>Syntaxis

De **$orderby** parameter accepteert een door komma's gescheiden lijst van maximaal 32 **order by-componenten**. De syntaxis van een component order by wordt beschreven door de volgende EBNF ([uitgebreid Backus Naur formulier](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)):

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
order_by_clause ::= (field_path | sortable_function) ('asc' | 'desc')?

sortable_function ::= geo_distance_call | 'search.score()'
```

Een diagram van een interactieve syntaxis is ook beschikbaar:

> [!div class="nextstepaction"]
> [Diagram van de OData-syntaxis voor Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#order_by_clause)

> [!NOTE]
> Zie [naslaginformatie over expressiesyntaxis voor Azure Search OData](search-query-odata-syntax-reference.md) voor de volledige EBNF.

Elke component heeft sorteercriteria, eventueel gevolgd door een sorteerrichting (`asc` in oplopende volgorde of `desc` voor aflopende). Als u de richting niet opgeeft, wordt de standaard oplopende. De sorteercriteria kunnen het pad van een `sortable` veld of een aanroep van hetzij de [ `geo.distance` ](search-query-odata-geo-spatial-functions.md) of de [ `search.score` ](search-query-odata-search-score-function.md) functies.

Als meerdere documenten dezelfde criteria sorteren hebben en de `search.score` functie wordt niet gebruikt (bijvoorbeeld, als u op een numerieke sorteren `Rating` veld en drie documenten hebt u een classificatie van 4), ties zullen niet werken op document score in aflopende volgorde. Wanneer het document scores dezelfde (bijvoorbeeld wanneer er geen query zoeken in volledige tekst is opgegeven in de aanvraag) zijn, klikt u vervolgens is de relatieve positie van de gebonden documenten onbepaald.

U kunt meerdere criteria voor sorteren opgeven. De volgorde van de expressies bepaalt de volgorde van de laatste sorteren. Bijvoorbeeld, de syntaxis van de zou zijn als u wilt sorteren op Aflopend op score, gevolgd door classificatie `$orderby=search.score() desc,Rating desc`.

De syntaxis voor `geo.distance` in **$orderby** is hetzelfde als het zich in **$filter**. Bij het gebruik van `geo.distance` in **$orderby**, het veld waarop deze van toepassing is moet van het type `Edm.GeographyPoint` en moet ook `sortable`.

De syntaxis voor `search.score` in **$orderby** is `search.score()`. De functie `search.score` parameters niet uitvoeren.

## <a name="examples"></a>Voorbeelden

Hotels door basistarief Oplopend sorteren:

    $orderby=BaseRate asc

Sorteren door te beoordelen en vervolgens basistarief oplopende aflopende hotels (Houd er rekening mee dat de standaardinstelling oplopend is):

    $orderby=Rating desc,BaseRate

Hotels Aflopend op classificatie en klik vervolgens op afstand van de opgegeven coördinaten Oplopend sorteren:

    $orderby=Rating desc,geo.distance(Location, geography'POINT(-122.131577 47.678581)') asc

Sorteren hotels in aflopende volgorde door search.score en classificatie en vervolgens van de opgegeven coördinaten in oplopende volgorde op afstand. Tussen twee hotels met identieke relevantie scores en beoordelingen, wordt de dichtstbijzijnde een eerste weergegeven:

    $orderby=search.score() desc,Rating desc,geo.distance(Location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Volgende stappen  

- [Over het werken met zoeken resulteert in Azure Search](search-pagination-page-layout.md)
- [Overzicht van taal van OData-expressie voor Azure Search](query-odata-filter-orderby-syntax.md)
- [Naslaginformatie over de syntaxis van de OData-expressie voor Azure Search](search-query-odata-syntax-reference.md)
- [Documenten zoeken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
