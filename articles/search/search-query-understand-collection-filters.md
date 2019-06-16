---
title: Informatie over filters voor bestandsverzameling OData - Azure Search
description: Informatie over de werking van OData-verzameling filters in Azure Search-query's.
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
ms.openlocfilehash: 7af1b0ab95d04249d6d74e3324dbeb30eda6e1de
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079598"
---
# <a name="understanding-odata-collection-filters-in-azure-search"></a>Informatie over filters voor OData-verzameling in Azure Search

Naar [filter](query-odata-filter-orderby-syntax.md) op verzameling velden in Azure Search, kunt u de [ `any` en `all` operators](search-query-odata-collection-operators.md) samen met **lambda-expressies**. Lambda-expressies zijn Booleaanse expressies die naar verwijzen een **bereik variabele**. De `any` en `all` operators zijn vergelijkbaar met een `for` lus in de meeste moderne programmeertalen, met de bereik-variabele houdend met de rol van de lus, en de lambda-expressie als hoofdtekst van de lus. De variabele bereik neemt de "huidige" waarde van de verzameling tijdens de herhaling van de lus.

Ten minste dat is hoe het werkt conceptueel gezien. Azure Search worden in werkelijkheid filters geïmplementeerd op een heel andere manier hoe `for` werk wordt uitgevoerd. In het ideale geval dit verschil zou onzichtbaar voor u zijn, maar in bepaalde situaties is het niet. Het eindresultaat is dat er regels die u volgen zijn moet bij het schrijven van lambda-expressies.

In dit artikel wordt uitgelegd waarom de regels voor filters voor bestandsverzameling bestaat door te verkennen hoe deze filters in Azure Search wordt uitgevoerd. Als u geavanceerde filters met complexe lambda-expressies schrijft, u mogelijk nuttig in dit artikel bij het bouwen van uw inzicht in wat er mogelijk is in de filters en waarom.

Voor meer informatie over wat de regels voor filters voor bestandsverzameling zijn, inclusief voorbeelden, Zie [probleemoplossing OData verzameling filters in Azure Search](search-query-troubleshoot-collection-filters.md).

## <a name="why-collection-filters-are-limited"></a>Waarom de filters voor bestandsverzameling zijn beperkt

Er zijn drie onderliggende redenen waarom niet alle filter-functies worden ondersteund voor alle typen verzamelingen:

1. Alleen bepaalde operators worden ondersteund voor bepaalde gegevenstypen. Bijvoorbeeld, het geen zin om de Booleaanse waarden te vergelijken `true` en `false` met behulp van `lt`, `gt`, enzovoort.
1. Azure Search biedt geen ondersteuning voor **gecorreleerde zoeken** op velden van het type `Collection(Edm.ComplexType)`.
1. Azure Search gebruikt omgekeerd indexen voor het uitvoeren van filters in op alle soorten gegevens, met inbegrip van verzamelingen.

De eerste reden is slechts een gevolg van hoe de OData-taal en het EDM-typesysteem zijn gedefinieerd. De laatste twee worden gedetailleerd besproken in de rest van dit artikel beschreven.

## <a name="correlated-versus-uncorrelated-search"></a>Gecorreleerde ten opzichte van niet-gerelateerde zoeken

Wanneer u meerdere criteria voor het filter toepast op een verzameling complexe objecten, de criteria die zijn **gecorreleerde** omdat ze van toepassing op *elk object in de verzameling*. Het volgende filter retourneert bijvoorbeeld hotels waarvoor ten minste één deluxe ruimte met een tarief van minder dan 100:

    Rooms/any(room: room/Type eq 'Deluxe Room' and room/BaseRate lt 100)

Als het filteren is *niet-gerelateerde*, het bovenstaande filter retourneert mogelijk hotels waarbij een ruimte deluxe en een andere kamer heeft een base beoordelen kleiner zijn dan 100. Die heeft geen zin, aangezien beide componenten van de lambda-expressie van toepassing op de variabele met dezelfde bereik, namelijk `room`. Dit is de reden waarom deze filters worden gecorreleerd.

Voor zoeken in volledige tekst is er echter geen manier om te verwijzen naar een specifiek bereik-variabele. Als u fielded zoeken om uit te geven een [volledige Lucene-query](query-lucene-syntax.md) zoals deze:

    Rooms/Type:deluxe AND Rooms/Description:"city view"

krijgt u mogelijk terug hotels waarbij een ruimte deluxe, en een andere kamer noemt 'view city' in de beschrijving. Bijvoorbeeld, het document hieronder met `Id` van `1` zou komen overeen met de query:

```json
{
  "value": [
    {
      "Id": "1",
      "Rooms": [
        { "Type": "deluxe", "Description": "Large garden view suite" },
        { "Type": "standard", "Description": "Standard city view room" }
      ]
    },
    {
      "Id": "2",
      "Rooms": [
        { "Type": "deluxe", "Description": "Courtyard motel room" }
      ]
    }
  ]
}
```

De reden hiervoor is dat `Rooms/Type` verwijst naar de geanalyseerde voorwaarden van de `Rooms/Type` veld in het hele document en op dezelfde manier voor `Rooms/Description`, zoals wordt weergegeven in de onderstaande tabellen.

Hoe `Rooms/Type` voor zoeken in volledige tekst wordt opgeslagen:

| Term in `Rooms/Type` | Document-id 's |
| --- | --- |
| Deluxe | 1, 2 |
| standaard | 1 |

Hoe `Rooms/Description` voor zoeken in volledige tekst wordt opgeslagen:

| Term in `Rooms/Description` | Document-id 's |
| --- | --- |
| binnenplaats | 2 |
| city | 1 |
| garden | 1 |
| Grote | 1 |
| motel | 2 |
| Ruimte | 1, 2 |
| standaard | 1 |
| Suite | 1 |
| weergeven | 1 |

Dus in tegenstelling tot het filter, die in feite vertelt "documenten waarin een kamer heeft `Type` gelijk zijn aan 'Deluxe Room' en **die dezelfde ruimte** heeft `BaseRate` kleiner zijn dan 100", zegt de zoekquery ' overeenkomst documenten waar `Rooms/Type`heeft de term 'deluxe' en `Rooms/Description` heeft de zin 'plaats weergeven'. Er is geen concept van afzonderlijke ruimten waarvan de velden in het laatste geval kunnen worden gecorreleerd.

> [!NOTE]
> Als u graag zou willen zien ondersteuning voor gecorreleerde zoeken die zijn toegevoegd aan Azure Search, kunt stemmen voor [dit Uservoice-item](https://feedback.azure.com/forums/263029-azure-search/suggestions/37735060-support-correlated-search-on-complex-collections).

## <a name="inverted-indexes-and-collections"></a>Omgekeerde indexen en verzamelingen

U hebt wellicht opgemerkt dat er veel minder beperkingen op lambda-expressies via complexe verzamelingen zijn dan er zijn voor eenvoudige verzamelingen, zoals `Collection(Edm.Int32)`, `Collection(Edm.GeographyPoint)`, enzovoort. Dit komt doordat Azure Search slaat complexe verzamelingen op als werkelijke verzamelingen van onderliggende documenten, terwijl eenvoudige verzamelingen worden niet als een verzameling helemaal opgeslagen.

Neem bijvoorbeeld een veld van de verzameling Filterbaar tekenreeks, zoals `seasons` in een index voor een online winkel. Aantal documenten dat is geüpload naar deze index uitzien als volgt:

```json
{
  "value": [
    {
      "id": "1",
      "name": "Hiking boots",
      "seasons": ["spring", "summer", "fall"]
    },
    {
      "id": "2",
      "name": "Rain jacket",
      "seasons": ["spring", "fall", "winter"]
    },
    {
      "id": "3",
      "name": "Parka",
      "seasons": ["winter"]
    }
  ]
}
```

De waarden van de `seasons` veld worden opgeslagen in een structuur genaamd een **omgekeerde index**, die ziet er ongeveer als volgt uit:

| Termijn | Document-id 's |
| --- | --- |
| Spring | 1, 2 |
| zomer | 1 |
| vallen | 1, 2 |
| winter | 2, 3 |

Deze gegevensstructuur is ontworpen om een vraag met uiterst snel te beantwoorden: In welke documenten wordt een opgegeven term weergegeven? Deze vraag beantwoorden werkt meer, zoals een gewone gelijkheidscontrole dan een lus via een verzameling. In feite, dit is waarom voor tekenreeks verzamelingen, Azure Search kunt alleen `eq` als een vergelijkingsoperator binnen een lambda-expressie voor `any`.

Het opbouwen van van gelijkheid, we gaan nu kijken hoe belangrijk het is mogelijk om te combineren van meerdere gelijkheid controles op de variabele in hetzelfde bereik met `or`. Het werkt dankzij wiskundige en [de distributieve eigenschap van kwantoren](https://en.wikipedia.org/wiki/Existential_quantification#Negation). Deze expressie:

    seasons/any(s: s eq 'winter' or s eq 'fall')

is gelijk aan:

    seasons/any(s: s eq 'winter') or seasons/any(s: s eq 'fall')

en elk van de twee `any` subexpressies efficiënt kunnen worden uitgevoerd met behulp van de omgekeerde index. Bovendien dank aan [het recht ontkenning van kwantoren](https://en.wikipedia.org/wiki/Existential_quantification#Negation), deze expressie:

    seasons/all(s: s ne 'winter' and s ne 'fall')

is gelijk aan:

    not seasons/any(s: s eq 'winter' or s eq 'fall')

Daarom is het mogelijk om te gebruiken `all` met `ne` en `and`.

> [!NOTE]
> Hoewel de details buiten het bereik van dit document zijn, wordt deze dezelfde principes uitbreiden naar [afstand en snijpunt tests voor verzamelingen van georuimtelijke punten](search-query-odata-geo-spatial-functions.md) ook. Dit is waarom `any`:
>
> - `geo.intersects` kan niet worden genegeerde
> - `geo.distance` moet worden vergeleken met behulp van `lt` of `le`
> - expressies moeten worden gecombineerd met `or`, niet `and`
>
> De omgekeerde-regels van toepassing voor `all`.

Een groter aantal expressies zijn toegestaan bij het filteren op verzamelingen van gegevens van het type die ondersteuning bieden voor de `lt`, `gt`, `le`, en `ge` operators, zoals `Collection(Edm.Int32)` bijvoorbeeld. Specifiek, kunt u `and` , evenals `or` in `any`, zolang de onderliggende vergelijkingsexpressies worden gecombineerd tot **bereik vergelijkingen** met behulp van `and`, die vervolgens meer zijn gecombineerd met behulp van `or`. Deze structuur van Booleaanse expressies heet [scheidende normaal formulier (DNF)](https://en.wikipedia.org/wiki/Disjunctive_normal_form), ook wel 'Or-items van and' genoemd. Daarentegen lambda-expressies voor `all` voor deze gegevens typen moeten zich in [Conjunctive normaal formulier (CNF)](https://en.wikipedia.org/wiki/Conjunctive_normal_form), ook bekend als 'and van or-items. Met Azure Search kunt dergelijke vergelijkingen bereik omdat ze kunnen worden uitgevoerd met behulp van omgekeerde indexen efficiënt, net zoals dit wordt mogelijk snel term lookup voor tekenreeksen.

Kortom, moet u dit gelden de volgende vuistregels voor wat toegestaan in een lambda-expressie zijn:

- Binnen `any`, *positieve controles* zijn altijd toegestaan, zoals gelijkheid, bereik vergelijkingen, `geo.intersects`, of `geo.distance` vergeleken met `lt` of `le` (Zie "mate' als zijnde zoals gelijkheid als het gaat om te controleren op afstand).
- Binnen `any`, `or` is altijd toegestaan. U kunt `and` alleen voor de gegevenstypen die u kunnen snelle controleren en alleen als u de or-items van and (DNF).
- Binnen `all`, de regels worden omgekeerd--alleen *negatieve controles* zijn toegestaan, kunt u `and` altijd, en kunt u `or` uitsluitend voor bereik uitgedrukt als and controleert van or-items (CNF).

Dit zijn de typen filters die u waarschijnlijk toch in de praktijk. Is het nog steeds nuttig om te begrijpen van de grenzen van wat er echter mogelijk is.

Zie voor specifieke voorbeelden van welke soorten filters zijn toegestaan en welke niet [over het schrijven van geldige verzameling filters](search-query-troubleshoot-collection-filters.md#bkmk_examples).

## <a name="next-steps"></a>Volgende stappen  

- [Het oplossen van OData-verzameling filters in Azure Search](search-query-troubleshoot-collection-filters.md)
- [Filters in Azure Search](search-filters.md)
- [Overzicht van taal van OData-expressie voor Azure Search](query-odata-filter-orderby-syntax.md)
- [Naslaginformatie over de syntaxis van de OData-expressie voor Azure Search](search-query-odata-syntax-reference.md)
- [Documenten zoeken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
