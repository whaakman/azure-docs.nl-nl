---
title: Naslag voor OData search.in - Azure Search
description: OData search.in-functie in Azure Search-query's.
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
ms.openlocfilehash: f72a59aac448796cf15220e15a3c8a4f12803bb5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079728"
---
# <a name="odata-searchin-function-in-azure-search"></a>OData `search.in` functie in Azure Search

Een veelvoorkomend scenario in [OData-filterexpressies](query-odata-filter-orderby-syntax.md) is om te controleren of één veld in elk document dat gelijk aan een van de vele mogelijke waarden is. Dit is bijvoorbeeld hoe sommige toepassingen implementeren [security trimming wordt geregeld](search-security-trimming-for-azure-search.md) --door het controleren van een veld met een of meer principal-id's op basis van een lijst van de principal-id's die het verzenden van de query. Één manier is het schrijven van een query zoals dit is het gebruik van de [ `eq` ](search-query-odata-comparison-operators.md) en [ `or` ](search-query-odata-logical-operators.md) operators:

    group_ids/any(g: g eq '123' or g eq '456' or g eq '789')

Er is echter een kortere manier om te schrijven, met behulp van de `search.in` functie:

    group_ids/any(g: search.in(g, '123, 456, 789'))

> [!IMPORTANT]
> Naast het korter en gemakkelijker te lezen, met behulp van `search.in` biedt ook [prestatievoordelen](#bkmk_performance) en voorkomt u bepaalde [grootte van de beperkingen van filters](search-query-odata-filter.md#bkmk_limits) wanneer er honderden of zelfs duizenden waarden om op te nemen in het filter. Voor deze reden wordt ten zeerste aangeraden `search.in` in plaats van een meer complexe scheiding van gelijkheid expressies.

> [!NOTE]
> Versie 4.01 van de OData-standaard is onlangs geïntroduceerd de [ `in` operator](http://docs.oasis-open.org/odata/odata/v4.01/cs01/part2-url-conventions/odata-v4.01-cs01-part2-url-conventions.html#_Toc505773230), die is soortgelijk gedrag als de `search.in` functie in Azure Search. Echter, Azure Search biedt geen ondersteuning deze operator, dus moet u de `search.in` werken in plaats daarvan.

## <a name="syntax"></a>Syntaxis

De volgende EBNF ([uitgebreid Backus Naur formulier](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) definieert de grammatica van de `search.in` functie:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
search_in_call ::=
    'search.in(' variable ',' string_literal(',' string_literal)? ')'
```

Een diagram van een interactieve syntaxis is ook beschikbaar:

> [!div class="nextstepaction"]
> [Diagram van de OData-syntaxis voor Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#search_in_call)

> [!NOTE]
> Zie [naslaginformatie over expressiesyntaxis voor Azure Search OData](search-query-odata-syntax-reference.md) voor de volledige EBNF.

De `search.in` functie test of een veld van de opgegeven tekenreeks of variabele bereik is gelijk aan een van een opgegeven lijst met waarden. Gelijkheid van de variabele en van elke waarde in de lijst wordt bepaald op een hoofdlettergevoelige manier, dezelfde manier als voor de `eq` operator. Daarom een expressie, zoals `search.in(myfield, 'a, b, c')` is gelijk aan `myfield eq 'a' or myfield eq 'b' or myfield eq 'c'`, behalve dat `search.in` leidt tot veel betere prestaties.

Er zijn twee overloads van de `search.in` functie:

- `search.in(variable, valueList)`
- `search.in(variable, valueList, delimiters)`

De parameters zijn gedefinieerd in de volgende tabel:

| Parameternaam | Type | Description |
| --- | --- | --- |
| `variable` | `Edm.String` | De verwijzing naar een tekenreeks-veld (of een variabele bereik via een tekenreeksveld verzameling in het geval waarbij `search.in` wordt gebruikt binnen een `any` of `all` expressie). |
| `valueList` | `Edm.String` | Een tekenreeks met een door tekens gescheiden lijst met waarden te vergelijken met de `variable` parameter. Als de `delimiters` parameter niet wordt opgegeven, de standaardscheidingstekens ruimte en door komma's. |
| `delimiters` | `Edm.String` | Een tekenreeks waarin elk teken wordt behandeld als scheidingsteken bij het parseren van de `valueList` parameter. De standaardwaarde van deze parameter is `' ,'` wat betekent dat alle waarden met spaties en/of komma's worden gescheiden. Als u gebruiken, scheidingstekens dan spaties en komma's wilt omdat de waarden die tekens bevatten, kunt u alternatieve scheidingstekens, zoals `'|'` in deze parameter. |

<a name="bkmk_performance"></a>

### <a name="performance-of-searchin"></a>Prestaties van `search.in`

Als u `search.in`, u krijgt dan een seconde reactietijd wanneer de tweede parameter een lijst met honderden of duizenden waarden bevat. Er is geen expliciete limiet voor het aantal items dat u kunt doorgeven aan `search.in`, hoewel u nog steeds worden beperkt door de grootte van de maximale aanvraag. Echter, de latentie zal toenemen naarmate het aantal waarden groeit.

## <a name="examples"></a>Voorbeelden

Zoek alle hotels met de naam gelijk is aan 'Zee weergave motel' of 'Budget hotel'. Zinnen bevatten spaties, dit is een standaardscheidingsteken. U kunt een alternatieve scheidingsteken tussen enkele aanhalingstekens als de derde tekenreeksparameter opgeven:  

    search.in(HotelName, 'Sea View motel,Budget hotel', ',')

Zoek alle hotels met de naam gelijk is aan 'Zee weergave motel' of 'Budget hotel' gescheiden door ' |'):

    search.in(HotelName, 'Sea View motel|Budget hotel', '|')

Zoek alle hotels met ruimten met de tag 'Wi-Fi' of 'l' als volgt:

    Rooms/any(room: room/Tags/any(tag: search.in(tag, 'wifi, tub')))

Een overeenkomst op zinnen binnen een verzameling, zoals 'hete handdoeken rekken' of 'hairdryer opgenomen' niet vinden in tags.

    Rooms/any(room: room/Tags/any(tag: search.in(tag, 'heated towel racks,hairdryer included', ','))

Zoek alle hotels zonder de tag 'motel' of 'handbagage':

    Tags/all(tag: not search.in(tag, 'motel, cabin'))

## <a name="next-steps"></a>Volgende stappen  

- [Filters in Azure Search](search-filters.md)
- [Overzicht van taal van OData-expressie voor Azure Search](query-odata-filter-orderby-syntax.md)
- [Naslaginformatie over de syntaxis van de OData-expressie voor Azure Search](search-query-odata-syntax-reference.md)
- [Documenten zoeken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
