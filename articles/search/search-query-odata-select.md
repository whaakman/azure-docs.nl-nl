---
title: Selecteer referentie OData - Azure Search
description: Syntaxis van de OData-Naslaggids voor selecteren in Azure Search-query's.
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
ms.openlocfilehash: 9383ae725fffac55854488ffbc6aeb161ae7e0c2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079676"
---
# <a name="odata-select-syntax-in-azure-search"></a>Syntaxis voor OData $select in Azure Search

 U kunt de [OData **$select** parameter](query-odata-filter-orderby-syntax.md) om te kiezen welke velden u wilt opnemen in zoekresultaten van Azure Search. In dit artikel beschrijft de syntaxis van **$select** in detail. Voor meer algemene informatie over hoe u **$select** wanneer zoekresultaten wordt gepresenteerd, Zie [over het werken met zoeken in Azure Search resultaten](search-pagination-page-layout.md).

## <a name="syntax"></a>Syntaxis

De **$select** parameter bepaalt welke velden voor elk document worden in de query-resultatenset geretourneerd. De volgende EBNF ([uitgebreid Backus Naur formulier](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) definieert de grammatica voor de **$select** parameter:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
select_expression ::= '*' | field_path(',' field_path)*

field_path ::= identifier('/'identifier)*
```

Een diagram van een interactieve syntaxis is ook beschikbaar:

> [!div class="nextstepaction"]
> [Diagram van de OData-syntaxis voor Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#select_expression)

> [!NOTE]
> Zie [naslaginformatie over expressiesyntaxis voor Azure Search OData](search-query-odata-syntax-reference.md) voor de volledige EBNF.

De **$select** parameter is beschikbaar in twee vormen:

1. Een enkele ster (`*`), waarmee wordt aangegeven dat alle Ophaalbare velden moeten worden geretourneerd, of
1. Een door komma's gescheiden lijst van veld paden, identificeren welke velden moet worden heeft geretourneerd.

Wanneer u het tweede formulier, kunt u alleen Ophaalbare velden opgeven in de lijst.

Als u een complexe veld zonder expliciet op te geven de onderliggende velden, worden alle ophaalbaar onderliggende velden worden opgenomen in de queryresultaatset. Bijvoorbeeld, wordt ervan uitgegaan dat uw index heeft een `Address` veld `Street`, `City`, en `Country` onderliggende velden die allemaal worden opgehaald. Als u opgeeft `Address` in **$select**, resultaten van de query bevat alle drie onderliggende velden.

## <a name="examples"></a>Voorbeelden

Bevatten de `HotelId`, `HotelName`, en `Rating` op het hoogste niveau van de velden in de resultaten, evenals de `City` subplan veld `Address`:

    $select=HotelId, HotelName, Rating, Address/City

Een voorbeeld van resultaat kan er als volgt uitzien:

```json
{
  "HotelId": "1",
  "HotelName": "Secret Point Motel",
  "Rating": 4,
  "Address": {
    "City": "New York"
  }
}
```

Bevatten de `HotelName` op het hoogste niveau veld in de resultaten, evenals alle onderliggende velden van `Address`, en de `Type` en `BaseRate` onderliggende velden van elk object in de `Rooms` verzameling:

    $select=HotelName, Address, Rooms/Type, Rooms/BaseRate

Een voorbeeld van resultaat kan er als volgt uitzien:

```json
{
  "HotelName": "Secret Point Motel",
  "Rating": 4,
  "Address": {
    "StreetAddress": "677 5th Ave",
    "City": "New York",
    "StateProvince": "NY",
    "Country": "USA",
    "PostalCode": "10022"
  },
  "Rooms": [
    {
      "Type": "Budget Room",
      "BaseRate": 9.69
    },
    {
      "Type": "Budget Room",
      "BaseRate": 8.09
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen  

- [Over het werken met zoeken resulteert in Azure Search](search-pagination-page-layout.md)
- [Overzicht van taal van OData-expressie voor Azure Search](query-odata-filter-orderby-syntax.md)
- [Naslaginformatie over de syntaxis van de OData-expressie voor Azure Search](search-query-odata-syntax-reference.md)
- [Documenten zoeken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
