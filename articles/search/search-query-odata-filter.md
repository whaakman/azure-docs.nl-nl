---
title: OData-filter referentie - Azure Search
description: OData-Naslaggids voor filtersyntaxis in Azure Search-query's.
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
ms.openlocfilehash: c44645ebcbf8808cc929bfaa0c3cb0d3ee9c90cd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079910"
---
# <a name="odata-filter-syntax-in-azure-search"></a>Syntaxis voor OData $filter in Azure Search

Maakt gebruik van Azure Search [OData-filterexpressies](query-odata-filter-orderby-syntax.md) aanvullende criteria toepassen op een zoekquery naast voorwaarden zoeken in volledige tekst. Dit artikel wordt de syntaxis van de filters in detail beschreven. Zie voor meer algemene informatie over filters en hoe ze gebruikt om te profiteren van scenario's voor specifieke query [Filters in Azure Search](search-filters.md).

## <a name="syntax"></a>Syntaxis

Een filter in de OData-taal is een Booleaanse expressie, dit op zijn beurt kan een aantal verschillende soorten expressie, zoals aangegeven in de volgende EBNF ([uitgebreid Backus Naur formulier](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)):

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
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
```

Een diagram van een interactieve syntaxis is ook beschikbaar:

> [!div class="nextstepaction"]
> [Diagram van de OData-syntaxis voor Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#boolean_expression)

> [!NOTE]
> Zie [naslaginformatie over expressiesyntaxis voor Azure Search OData](search-query-odata-syntax-reference.md) voor de volledige EBNF.

De typen Booleaanse expressies zijn onder andere:

- Verzameling filterexpressies met behulp van `any` of `all`. Deze filtercriteria die van toepassing op de verzameling velden. Zie voor meer informatie, [operators voor OData-verzameling in Azure Search](search-query-odata-collection-operators.md).
- Logische expressies met een combinatie van andere Booleaanse expressies met behulp van de operators `and`, `or`, en `not`. Zie voor meer informatie, [OData logische operators in Azure Search](search-query-odata-logical-operators.md).
- Van de vergelijkingsexpressies die velden vergelijken of het bereik van variabelen moeten constante waarden met behulp van de operators `eq`, `ne`, `gt`, `lt`, `ge`, en `le`. Zie voor meer informatie, [OData-vergelijkingsoperators in Azure Search](search-query-odata-comparison-operators.md). Vergelijkingsexpressies worden ook gebruikt om te vergelijken van de afstand tussen georuimtelijke coördinaten met behulp van de `geo.distance` functie. Zie voor meer informatie, [OData, georuimtelijke functies in Azure Search](search-query-odata-geo-spatial-functions.md).
- De letterlijke Booleaanse `true` en `false`. Deze constanten kunnen nuttig zijn soms bij het genereren van programmatisch filters, maar anders niet vaak worden gebruikt in de praktijk.
- Aanroepen naar Boole-functies, met inbegrip van:
  - `geo.intersects`, waarmee wordt gecontroleerd of een opgegeven punt zich binnen een bepaalde veelhoek. Zie voor meer informatie, [OData, georuimtelijke functies in Azure Search](search-query-odata-geo-spatial-functions.md).
  - `search.in`, waarmee een veld of bereik variabele met elke waarde in een lijst met waarden worden vergeleken. Zie voor meer informatie, [OData `search.in` functie in Azure Search](search-query-odata-search-in-function.md).
  - `search.ismatch` en `search.ismatchscoring`, die bij het uitvoeren van bewerkingen voor zoeken in volledige tekst in een filtercontext. Zie voor meer informatie, [OData zoeken in volledige tekst functies in Azure Search](search-query-odata-full-text-search-functions.md).
- Veld paden of het bereik van variabelen van het type `Edm.Boolean`. Bijvoorbeeld, als uw index heeft een Boole-veld met de naam `IsEnabled` en u wilt dat alle documenten retourneren wanneer dit veld is `true`, uw filterexpressie mag alleen de naam van de `IsEnabled`.
- Booleaanse expressies tussen haakjes. Met behulp van haakjes kan helpen om expliciet te bepalen de volgorde van bewerkingen in een filter. Zie voor meer informatie over de standaard-prioriteit van de OData-operators, de volgende sectie.

### <a name="operator-precedence-in-filters"></a>Operatoren in de filters

Als u een filterexpressie met geen bijbehorende onderliggende expressies tussen haakjes schrijft, wordt Azure Search geëvalueerd op basis van een set regels voor opdrachtprioriteit operator. Deze regels zijn afhankelijk van welke operators worden gebruikt voor het combineren van de onderliggende expressies. De volgende tabel geeft een lijst van groepen van operators in volgorde van hoogste naar laagste prioriteit:

| Groep | Operator(s) |
| --- | --- |
| Logische operators | `not` |
| Vergelijkingsoperators | `eq`, `ne`, `gt`, `lt`, `ge`, `le` |
| Logische operators | `and` |
| Logische operators | `or` |

Een operator op die hoger is in de bovenstaande tabel wordt '-binding dichter naar de operands dan andere operatoren. Bijvoorbeeld, `and` is met een hogere prioriteit dan `or`, en vergelijkingsoperators zijn met een hogere prioriteit dan een van beide, dus de volgende twee expressies gelijk zijn:

    Rating gt 0 and Rating lt 3 or Rating gt 7 and Rating lt 10
    ((Rating gt 0) and (Rating lt 3)) or ((Rating gt 7) and (Rating lt 10))

De `not` operator is de hoogste prioriteit van alle--zelfs hoger dan de vergelijkingsoperators op. Daarom is als u wilt een schrijffilter is als volgt:

    not Rating gt 5

U ontvangt deze foutmelding:

    Invalid expression: A unary operator with an incompatible type was detected. Found operand type 'Edm.Int32' for operator kind 'Not'.

Deze fout treedt op omdat de operator is gekoppeld aan alleen de `Rating` veld, die van het type is `Edm.Int32`, en niet met de van de hele vergelijkingsexpressie. De oplossing is om de operand van `not` tussen haakjes:

    not (Rating gt 5)

<a name="bkmk_limits"></a>

### <a name="filter-size-limitations"></a>Beperkingen van de bestandsgrootte filteren

Er gelden beperkingen voor de grootte en complexiteit van filterexpressies die u naar Azure Search verzenden kunt. De limieten zijn ongeveer gebaseerd op het aantal in de filterexpressie van de EU. Een goede richtlijn is dat hebt u honderden van de EU, u het risico van meer dan de limiet. Het is raadzaam om het ontwerpen van uw toepassing zodanig dat deze filters van niet-gebonden grootte niet genereren.

> [!TIP]
> Met behulp van [de `search.in` functie](search-query-odata-search-in-function.md) in plaats van lange disjunctions van gelijke vergelijkingen kunnen helpen te voorkomen dat de limiet van filter-component, omdat een functieoproep als één component telt.

## <a name="examples"></a>Voorbeelden

Zoek alle hotels met ten minste één ruimte met een basistarief kleiner is dan $200 die worden beoordeeld op of boven de 4:

    $filter=Rooms/any(room: room/BaseRate lt 200.0) and Rating ge 4

Zoek alle hotels dan "Zee weergave Motel" hebben is renovated sinds 2010:

    $filter=HotelName ne 'Sea View Motel' and LastRenovationDate ge 2010-01-01T00:00:00Z

Zoek alle hotels die zijn renovated in 2010 of hoger. De letterlijke datetime-waarde bevat informatie over de tijdzone voor Pacific (standaardtijd):  

    $filter=LastRenovationDate ge 2010-01-01T00:00:00-08:00

Zoek alle hotels waarvoor de vervangende domeinpagina opgenomen en waar alle ruimten zijn niet-soorten:

    $filter=ParkingIncluded and Rooms/all(room: not room/SmokingAllowed)

 \- OR-  

    $filter=ParkingIncluded eq true and Rooms/all(room: room/SmokingAllowed eq false)

Alle hotels die luxe of vervangende domeinpagina bevatten en hebben een classificatie van 5 zoeken:  

    $filter=(Category eq 'Luxury' or ParkingIncluded eq true) and Rating eq 5

Alle hotels met het label 'Wi-Fi' niet vinden in ten minste één ruimte (waarbij elke ruimte heeft voor labels die zijn opgeslagen in een `Collection(Edm.String)` veld):  

    $filter=Rooms/any(room: room/Tags/any(tag: tag eq 'wifi'))

Alle hotels met eventuele ruimten zoeken:  

    $filter=Rooms/any()

Alle hotels waarvoor geen ruimten zoeken:

    $filter=not Rooms/any()

Alle hotels binnen 10 kilometer van de opgegeven referentie vinden (waarbij `Location` is een veld van het type `Edm.GeographyPoint`):

    $filter=geo.distance(Location, geography'POINT(-122.131577 47.678581)') le 10

Alle hotels binnen een bepaalde viewport beschreven als een veelhoek vinden (waarbij `Location` is een veld van het type Edm.GeographyPoint). De veelhoek moet worden gesloten, wat betekent dat de eerste en laatste punt sets moeten hetzelfde zijn. Bovendien [de punten moeten worden weergegeven in tegen de klok in volgorde](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

    $filter=geo.intersects(Location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))')

Zoek alle hotels waar het veld 'Description' null is. Het veld niet null zijn als deze niet is ingesteld, of als het expliciet is ingesteld op null:  

    $filter=Description eq null

Zoek alle hotels met de naam gelijk is aan 'Zee weergave motel' of 'Budget hotel'). Deze zinnen spaties bevatten en ruimte wordt gebruikt als standaardscheidingsteken. U kunt een alternatieve scheidingsteken tussen enkele aanhalingstekens als de derde tekenreeksparameter opgeven:  

    $filter=search.in(HotelName, 'Sea View motel,Budget hotel', ',')

Zoek alle hotels met de naam gelijk is aan 'Zee weergave motel' of 'Budget hotel' gescheiden door ' |'):  

    $filter=search.in(HotelName, 'Sea View motel|Budget hotel', '|')

Zoek alle hotels waarin alle ruimten de tag 'Wi-Fi' of 'l hebben' als volgt:

    $filter=Rooms/any(room: room/Tags/any(tag: search.in(tag, 'wifi, tub'))

Een overeenkomst op zinnen binnen een verzameling, zoals 'hete handdoeken rekken' of 'hairdryer opgenomen' niet vinden in tags.

    $filter=Rooms/any(room: room/Tags/any(tag: search.in(tag, 'heated towel racks,hairdryer included', ','))

Documenten met het woord "afgebakend" zoeken. Dit filterquery is gelijk aan een [zoekaanvraag](https://docs.microsoft.com/rest/api/searchservice/search-documents) met `search=waterfront`.

    $filter=search.ismatchscoring('waterfront')

Documenten met het woord 'hostel' en waardering groter of gelijk zijn aan 4 of documenten met het woord "motel" en waardering gelijk is aan 5 zoeken. Deze aanvraag kan niet worden weergegeven zonder de `search.ismatchscoring` functie omdat het zoeken in volledige tekst combineert met filter bewerkingen met behulp van `or`.

    $filter=search.ismatchscoring('hostel') and rating ge 4 or search.ismatchscoring('motel') and rating eq 5

Documenten zonder het woord "luxe" zoeken.

    $filter=not search.ismatch('luxury')

Documenten met de woordgroep in de Indische Oceaan "view" of de classificatie die gelijk is aan 5 zoeken. De `search.ismatchscoring` query wordt uitgevoerd alleen op basis van velden `HotelName` en `Description`. Documenten die overeenkomen met alleen de tweede component van de splitsing wordt geretourneerd te--hotels met `Rating` gelijk is aan 5. Deze documenten worden geretourneerd met score gelijk zijn aan nul om het te maken dat ze niet met de beoordeelde onderdelen van de expressie overeenkomen wissen.

    $filter=search.ismatchscoring('"ocean view"', 'Description,HotelName') or Rating eq 5

Zoeken naar hotels waar de termen "Hotels" en 'luchthaven' zijn niet meer dan vijf woorden uit elkaar in de beschrijving en waar alle ruimten zijn niet-soorten. Deze query gebruikt de [volledige Lucene-querytaal](query-lucene-syntax.md).

    $filter=search.ismatch('"hotel airport"~5', 'Description', 'full', 'any') and not Rooms/any(room: room/SmokingAllowed)

## <a name="next-steps"></a>Volgende stappen  

- [Filters in Azure Search](search-filters.md)
- [Overzicht van taal van OData-expressie voor Azure Search](query-odata-filter-orderby-syntax.md)
- [Naslaginformatie over de syntaxis van de OData-expressie voor Azure Search](search-query-odata-syntax-reference.md)
- [Documenten zoeken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
