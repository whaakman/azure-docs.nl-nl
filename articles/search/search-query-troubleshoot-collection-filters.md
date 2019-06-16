---
title: Het oplossen van de filters voor bestandsverzameling OData - Azure Search
description: Het oplossen van fouten van OData-verzameling filteren in Azure Search-query's.
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
ms.openlocfilehash: c7fa00c82eea03a50bae22fcb1ad16e230aa5bcb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079624"
---
# <a name="troubleshooting-odata-collection-filters-in-azure-search"></a>Het oplossen van OData-verzameling filters in Azure Search

Naar [filter](query-odata-filter-orderby-syntax.md) op verzameling velden in Azure Search, kunt u de [ `any` en `all` operators](search-query-odata-collection-operators.md) samen met **lambda-expressies**. Een lambda-expressie is een subquery filter dat wordt toegepast op elk element van een verzameling.

Niet elke functie van filterexpressies is beschikbaar in een lambda-expressie. Welke functies beschikbaar zijn, is afhankelijk van het gegevenstype van de verzameling-veld dat u wilt filteren. Dit kan resulteren in een fout als u probeert te gebruiken van een functie in een lambda-expressie die niet wordt ondersteund in deze context. Als u dergelijke fouten ondervindt tijdens het schrijven van een complexe filter over de verzameling velden, krijgt in dit artikel u het probleem op te lossen.

## <a name="common-collection-filter-errors"></a>Veelvoorkomende fouten van de verzameling filter

De volgende tabel bevat fouten die optreden mogelijk wanneer bij het uitvoeren van een verzamelingsfilter. Deze fouten zich voordoen wanneer u een functie van filterexpressies die niet wordt ondersteund in een lambda-expressie gebruikt. Elke fout biedt enkele richtlijnen voor hoe u uw filter om te voorkomen dat de fout kan herschrijven. De tabel bevat ook een koppeling naar de betreffende sectie van dit artikel vindt u meer informatie over hoe u om te voorkomen dat deze fout.

| Foutbericht | Situatie | Zie voor meer informatie |
| --- | --- | --- |
| De functie 'ismatch' heeft geen parameters die zijn gekoppeld aan het bereik van de variabele '. Alleen gebonden veld verwijzingen worden ondersteund in de lambda-expressies ('any' of 'all'). Uw filter wijzigen zodat de functie 'ismatch' buiten de lambda-expressie valt en probeer het opnieuw. | Met behulp van `search.ismatch` of `search.ismatchscoring` binnen een lambda-expressie | [Regels voor het filteren van complexe verzamelingen](#bkmk_complex) |
| Ongeldige lambda-expressie. Een test gevonden voor gelijkheid of ongelijkheid waar het tegenovergestelde werd verwacht in een lambda-expressie die een veld van het type Collection(Edm.String) te herhalen. Gebruik expressies van het formulier 'y' x eq' of 'search.in(...)' voor 'alle'. Voor 'alle', de expressies van het formulier 'y' x ne', 'niet (x eq y)' of 'niet search.in(...)' gebruik. | Filteren op een veld van het type `Collection(Edm.String)` | [Regels voor het filteren van tekenreeks verzamelingen](#bkmk_strings) |
| Ongeldige lambda-expressie. Een niet-ondersteunde vorm van complexe Boole-expressie gevonden. Gebruik expressies die 'Or-items van and', ook wel bekend als scheidende normaal formulier voor 'alle'. Bijvoorbeeld: '(a and b) of (c en d)' waarbij a, b, c en d vergelijking of gelijkheid onderliggende expressies zijn. Voor 'alle', gebruik expressies die 'And van or-items', ook wel bekend als Conjunctive normaal formulier. Bijvoorbeeld: '(a or b) en (c of d)' waarbij a, b, c en d vergelijking of ongelijkheid onderliggende expressies zijn. Voorbeelden van vergelijkingsexpressies: ' x gt 5', ' x-le 2'. Voorbeeld van een expressie gelijkheid: ' x eq 5'. Voorbeeld van een expressie ongelijkheid: ' x ne 5'. | Filteren op velden van het type `Collection(Edm.DateTimeOffset)`, `Collection(Edm.Double)`, `Collection(Edm.Int32)`, of `Collection(Edm.Int64)` | [Regels voor het filteren van vergelijkbare verzamelingen](#bkmk_comparables) |
| Ongeldige lambda-expressie. Een niet-ondersteund gebruik van geo.distance() of geo.intersects() gevonden in een lambda-expressie die een veld van het type Collection(Edm.GeographyPoint) te herhalen. Voor 'alle', zorg ervoor dat u met behulp van de operators 'lt' of 'RP' geo.distance() vergelijken en zorg ervoor dat er niet voor het gebruik van geo.intersects() is genegeerde. Voor 'alle', zorg dat u met behulp van de operators 'gt' of 'ge' geo.distance() vergelijken en zorg ervoor dat voor het gebruik van geo.intersects() genegeerde is. | Filteren op een veld van het type `Collection(Edm.GeographyPoint)` | [Regels voor het filteren van GeographyPoint-verzamelingen](#bkmk_geopoints) |
| Ongeldige lambda-expressie. Complexe Booleaanse expressies worden niet ondersteund in de lambda-expressies die velden van het type Collection(Edm.GeographyPoint) herhalen. Deelnemen aan onderliggende expressies met voor 'alle', 'of'; 'en' wordt niet ondersteund. Voor 'alle', doe subexpressies met 'en'; 'of' wordt niet ondersteund. | Filteren op velden van het type `Collection(Edm.String)` of `Collection(Edm.GeographyPoint)` | [Regels voor het filteren van tekenreeks verzamelingen](#bkmk_strings) <br/><br/> [Regels voor het filteren van GeographyPoint-verzamelingen](#bkmk_geopoints) |
| Ongeldige lambda-expressie. Een vergelijkingsoperator (een van de 'lt', 'RP', 'gt' of 'ge') gevonden. Alleen gelijkheid operators zijn toegestaan in de lambda-expressies die velden van het type Collection(Edm.String) herhalen. Gebruik expressies van het formulier 'y' x eq' voor 'alle'. Voor 'alle', gebruik expressies van het formulier x ne 'y' of 'niet (x eq y)'. | Filteren op een veld van het type `Collection(Edm.String)` | [Regels voor het filteren van tekenreeks verzamelingen](#bkmk_strings) |

<a name="bkmk_examples"></a>

## <a name="how-to-write-valid-collection-filters"></a>Over het schrijven van geldige verzameling filters

De regels voor het schrijven van geldige verzameling filters zijn verschillend voor elk gegevenstype. De volgende secties worden de regels door voorbeelden van waaruit de filterwaarden functies worden ondersteund en welke niet weer te geven:

- [Regels voor het filteren van tekenreeks verzamelingen](#bkmk_strings)
- [Regels voor het filteren van Booleaanse verzamelingen](#bkmk_bools)
- [Regels voor het filteren van GeographyPoint-verzamelingen](#bkmk_geopoints)
- [Regels voor het filteren van vergelijkbare verzamelingen](#bkmk_comparables)
- [Regels voor het filteren van complexe verzamelingen](#bkmk_complex)

<a name="bkmk_strings"></a>

## <a name="rules-for-filtering-string-collections"></a>Regels voor het filteren van tekenreeks verzamelingen

In de lambda-expressies voor tekenreeks verzamelingen, de enige vergelijkingsoperators op die kunnen worden gebruikt zijn `eq` en `ne`.

> [!NOTE]
> Azure Search biedt geen ondersteuning voor de `lt` / `le` / `gt` / `ge` operators voor tekenreeksen, of binnen of buiten een lambda-expressie.

De hoofdtekst van een `any` kunt alleen een failovertest voor gelijkheid tijdens de hoofdtekst van een `all` alleen voor ongelijkheid kunt testen.

Het is ook mogelijk om te combineren van meerdere expressies via `or` in de hoofdtekst van een `any`, en via `and` in de hoofdtekst van een `all`. Omdat de `search.in` functie is gelijk aan het combineren van de gelijkheid controleert `or`, het ook toegestaan in de hoofdtekst van een `any`. Daarentegen `not search.in` is toegestaan in de hoofdtekst van een `all`.

Bijvoorbeeld, zijn deze expressies toegestaan:

- `tags/any(t: t eq 'books')`
- `tags/any(t: search.in(t, 'books, games, toys'))`
- `tags/all(t: t ne 'books')`
- `tags/all(t: not (t eq 'books'))`
- `tags/all(t: not search.in(t, 'books, games, toys'))`
- `tags/any(t: t eq 'books' or t eq 'games')`
- `tags/all(t: t ne 'books' and not (t eq 'games'))`

terwijl deze expressies zijn niet toegestaan:

- `tags/any(t: t ne 'books')`
- `tags/any(t: not search.in(t, 'books, games, toys'))`
- `tags/all(t: t eq 'books')`
- `tags/all(t: search.in(t, 'books, games, toys'))`
- `tags/any(t: t eq 'books' and t ne 'games')`
- `tags/all(t: t ne 'books' or not (t eq 'games'))`

<a name="bkmk_bools"></a>

## <a name="rules-for-filtering-boolean-collections"></a>Regels voor het filteren van Booleaanse verzamelingen

Het type `Edm.Boolean` ondersteunt alleen de `eq` en `ne` operators. Als zodanig het werkt niet echt waarmee deze componenten controleren van de variabele in hetzelfde bereik met combineren `and` / `or` omdat dat altijd tot tautologies of strijdigheden leiden zou.

Hier volgen enkele voorbeelden van filters op Booleaanse verzamelingen die zijn toegestaan:

- `flags/any(f: f)`
- `flags/all(f: f)`
- `flags/any(f: f eq true)`
- `flags/any(f: f ne true)`
- `flags/all(f: not f)`
- `flags/all(f: not (f eq true))`

In tegenstelling tot tekenreeks verzamelingen hebben Booleaanse verzamelingen geen beperkingen op die de operator kan worden gebruikt in welk type lambda-expressie Beide `eq` en `ne` kan worden gebruikt in de hoofdtekst van `any` of `all`.

Expressies, zoals de volgende zijn niet toegestaan voor Booleaanse verzamelingen:

- `flags/any(f: f or not f)`
- `flags/any(f: f or f)`
- `flags/all(f: f and not f)`
- `flags/all(f: f and f eq true)`

<a name="bkmk_geopoints"></a>

## <a name="rules-for-filtering-geographypoint-collections"></a>Regels voor het filteren van GeographyPoint-verzamelingen

Waarden van het type `Edm.GeographyPoint` in een verzameling kan niet rechtstreeks met elkaar worden vergeleken. In plaats daarvan, ze moeten worden gebruikt als parameters voor de `geo.distance` en `geo.intersects` functies. De `geo.distance` functie op zijn beurt moet worden vergeleken met een waarde voor scheidingsafstand met behulp van een van de vergelijkingsoperators `lt`, `le`, `gt`, of `ge`. Deze regels zijn ook van toepassing op niet-verzameling Edm.GeographyPoint velden.

Tekenreeks-verzamelingen, zoals `Edm.GeographyPoint` verzamelingen hebben sommige regels voor hoe de georuimtelijke functies kunnen worden gebruikt en op de verschillende typen lambda-expressies gecombineerd:

- Welke vergelijkingsoperators kunt u met de `geo.distance` functie afhankelijk is van het type van de lambda-expressie. Voor `any`, kunt u alleen `lt` of `le`. Voor `all`, kunt u alleen `gt` of `ge`. U kunt negatief moet worden gemaakt met betrekking tot expressies `geo.distance`, maar u hebt de vergelijkingsoperator wijzigen (`geo.distance(...) lt x` wordt `not (geo.distance(...) ge x)` en `geo.distance(...) le x` wordt `not (geo.distance(...) gt x)`).
- In de hoofdtekst van een `all`, wordt de `geo.intersects` functie moet worden gecompenseerd. Daarentegen in de hoofdtekst van een `any`, wordt de `geo.intersects` functie niet moet worden gecompenseerd.
- In de hoofdtekst van een `any`, georuimtelijke expressies kunnen worden gecombineerd met `or`. In de hoofdtekst van een `all`, dergelijke expressies kunnen worden gecombineerd met `and`.

De bovenstaande beperkingen bestaan voor dezelfde redenen als gelijkheid/ongelijkheid beperking van de tekenreeks verzamelingen. Zie [Understanding OData verzameling filters in Azure Search](search-query-understand-collection-filters.md) voor stil bij de volgende redenen.

Hier volgen enkele voorbeelden van filters op `Edm.GeographyPoint` verzamelingen die zijn toegestaan:

- `locations/any(l: geo.distance(l, geography'POINT(-122 49)') lt 10)`
- `locations/any(l: not (geo.distance(l, geography'POINT(-122 49)') ge 10) or geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/all(l: geo.distance(l, geography'POINT(-122 49)') ge 10 and not geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`

De volgende tabel bevat voorbeelden van expressies zijn niet toegestaan voor `Edm.GeographyPoint` verzamelingen:

- `locations/any(l: l eq geography'POINT(-122 49)')`
- `locations/any(l: not geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/all(l: geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/any(l: geo.distance(l, geography'POINT(-122 49)') gt 10)`
- `locations/all(l: geo.distance(l, geography'POINT(-122 49)') lt 10)`
- `locations/any(l: geo.distance(l, geography'POINT(-122 49)') lt 10 and geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/all(l: geo.distance(l, geography'POINT(-122 49)') le 10 or not geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`

<a name="bkmk_comparables"></a>

## <a name="rules-for-filtering-comparable-collections"></a>Regels voor het filteren van vergelijkbare verzamelingen

In deze sectie is van toepassing op de volgende gegevenstypen:

- `Collection(Edm.DateTimeOffset)`
- `Collection(Edm.Double)`
- `Collection(Edm.Int32)`
- `Collection(Edm.Int64)`

Typen zoals `Edm.Int32` en `Edm.DateTimeOffset` ondersteuning voor alle zes van de vergelijkingsoperators: `eq`, `ne`, `lt`, `le`, `gt`, en `ge`. Lambda-expressies op verzamelingen van de volgende typen kunnen eenvoudige expressies met behulp van een van deze operators bevatten. Dit geldt voor zowel `any` en `all`. Bijvoorbeeld, zijn deze filters toegestaan:

- `ratings/any(r: r ne 5)`
- `dates/any(d: d gt 2017-08-24T00:00:00Z)`
- `not margins/all(m: m eq 3.5)`

Er zijn echter beperkingen met betrekking tot hoe dergelijke vergelijkingsexpressies kunnen worden gecombineerd tot complexe expressies in een lambda-expressie:

- Regels voor `any`:
  - Eenvoudige ongelijkheid expressies kunnen niet nuttige worden gecombineerd met andere expressies. Bijvoorbeeld, mag deze expressie:
    - `ratings/any(r: r ne 5)`

    maar deze expressie niet:
    - `ratings/any(r: r ne 5 and r gt 2)`

    en hoewel deze expressie is toegestaan, is het niet handig omdat de voorwaarden overlappen:
    - `ratings/any(r: r ne 5 or r gt 7)`
  - Eenvoudige vergelijkingsexpressies met betrekking tot `eq`, `lt`, `le`, `gt`, of `ge` kan worden gecombineerd met `and` / `or`. Bijvoorbeeld:
    - `ratings/any(r: r gt 2 and r le 5)`
    - `ratings/any(r: r le 5 or r gt 7)`
  - Van de vergelijkingsexpressies in combinatie met `and` (voegwoorden) verder kunnen worden gecombineerd met `or`. Dit formulier is bekend in Booleaanse logica als '[scheidende normaal formulier](https://en.wikipedia.org/wiki/Disjunctive_normal_form)' (DNF). Bijvoorbeeld:
    - `ratings/any(r: (r gt 2 and r le 5) or (r gt 7 and r lt 10))`
- Regels voor `all`:
  - Eenvoudige gelijkheid expressies kunnen niet nuttige worden gecombineerd met andere expressies. Bijvoorbeeld, mag deze expressie:
    - `ratings/all(r: r eq 5)`

    maar deze expressie niet:
    - `ratings/all(r: r eq 5 or r le 2)`

    en hoewel deze expressie is toegestaan, is het niet handig omdat de voorwaarden overlappen:
    - `ratings/all(r: r eq 5 and r le 7)`
  - Eenvoudige vergelijkingsexpressies met betrekking tot `ne`, `lt`, `le`, `gt`, of `ge` kan worden gecombineerd met `and` / `or`. Bijvoorbeeld:
    - `ratings/all(r: r gt 2 and r le 5)`
    - `ratings/all(r: r le 5 or r gt 7)`
  - Van de vergelijkingsexpressies in combinatie met `or` (disjunctions) verder kunnen worden gecombineerd met `and`. Dit formulier is bekend in Booleaanse logica als '[Conjunctive normaal formulier](https://en.wikipedia.org/wiki/Conjunctive_normal_form)' (CNF). Bijvoorbeeld:
    - `ratings/all(r: (r le 2 or gt 5) and (r lt 7 or r ge 10))`

<a name="bkmk_complex"></a>

## <a name="rules-for-filtering-complex-collections"></a>Regels voor het filteren van complexe verzamelingen

Lambda-expressies via complexe verzamelingen ondersteunt de syntaxis van een veel meer flexibiliteit dan lambda-expressies in verzamelingen van primitieve typen. U kunt een filter om voor te bereiden in dergelijke een lambda-expressie die u buiten een, met slechts twee uitzonderingen kunt gebruiken.

De functies First, `search.ismatch` en `search.ismatchscoring` binnen lambda-expressies worden niet ondersteund. Zie voor meer informatie, [Understanding OData verzameling filters in Azure Search](search-query-understand-collection-filters.md).

Ten tweede verwijzen naar velden die niet *gebonden* aan de variabele bereik (zogeheten *gratis variabelen*) is niet toegestaan. Bijvoorbeeld, houd rekening met de volgende twee gelijkwaardige OData-filterexpressies:

1. `stores/any(s: s/amenities/any(a: a eq 'parking')) and details/margin gt 0.5`
1. `stores/any(s: s/amenities/any(a: a eq 'parking' and details/margin gt 0.5))`

De eerste expressie die actief mag zijn, terwijl het tweede formulier wordt geweigerd omdat `details/margin` is niet gebonden aan de variabele bereik `s`.

Deze regel wordt ook vergroot voor expressies die in een buitenste bereik gebonden variabelen. Deze variabelen zijn gratis met betrekking tot het bereik waarin ze worden weergegeven. Bijvoorbeeld, de eerste expressie die is toegestaan, terwijl de tweede equivalente expressie is niet toegestaan omdat `s/name` is gratis met betrekking tot het bereik van de variabele bereik `a`:

1. `stores/any(s: s/amenities/any(a: a eq 'parking') and s/name ne 'Flagship')`
1. `stores/any(s: s/amenities/any(a: a eq 'parking' and s/name ne 'Flagship'))`

Deze beperking mag niet een probleem in de praktijk zijn omdat het is altijd mogelijk te maken van filters zodat lambda-expressies alleen gebonden variabelen bevatten.

## <a name="cheat-sheet-for-collection-filter-rules"></a>Overzichtskaart voor regels voor het verzamelen

De volgende tabel geeft een overzicht van de regels voor het maken van geldige filters voor het gegevenstype van elke verzameling.

[!INCLUDE [Limitations on OData lambda expressions in Azure Search](../../includes/search-query-odata-lambda-limitations.md)]

Zie voor meer voorbeelden van hoe u een van de geldige filters voor elk geval [over het schrijven van geldige verzameling filters](#bkmk_examples).

Als u schrijffilters vaak en inzicht krijgen in de regels van de eerste beginselen kunt u meer dan alleen onthouden, Zie [Understanding OData verzameling filters in Azure Search](search-query-understand-collection-filters.md).

## <a name="next-steps"></a>Volgende stappen  

- [Informatie over filters voor OData-verzameling in Azure Search](search-query-understand-collection-filters.md)
- [Filters in Azure Search](search-filters.md)
- [Overzicht van taal van OData-expressie voor Azure Search](query-odata-filter-orderby-syntax.md)
- [Naslaginformatie over de syntaxis van de OData-expressie voor Azure Search](search-query-odata-syntax-reference.md)
- [Documenten zoeken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
