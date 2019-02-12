---
title: Syntaxis voor OData-expressie voor filters en volgorde van de EU - Azure Search
description: Filteren en sorteren expressie OData-syntaxis voor Azure Search-query's.
ms.date: 01/31/2019
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
ms.openlocfilehash: c6dbd95cfd17f5ce49245fcadea299c5a0dfd582
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56008445"
---
# <a name="odata-expression-syntax-for-filters-and-order-by-clauses-in-azure-search"></a>Syntaxis voor OData-expressie voor filters en order by-componenten in Azure Search

Azure Search ondersteunt een subset van de syntaxis van de OData-expressie voor **$filter** en **$orderby** expressies. Filterexpressies worden geëvalueerd tijdens query parseren, zoeken naar specifieke velden beperken of toe te voegen overeenkomen met criteria die zijn gebruikt tijdens scans index. Order by-expressies worden toegepast als een stap na verwerking via een resultatenset. Zowel filters en order by-expressies zijn opgenomen in een queryaanvraag aan een OData-syntaxis, onafhankelijk van de [eenvoudige](query-simple-syntax.md) of [volledige](query-lucene-syntax.md) querysyntaxis gebruikt in een **zoeken** de parameter. Dit artikel bevat de referentiedocumentatie voor OData-expressies die worden gebruikt in filters en sorteerexpressies.

## <a name="filter-syntax"></a>Filtersyntaxis

Een **$filter** expressie zelfstandige als een volledig uitgedrukt query uitvoeren, of een query met extra parameters verfijnen. De volgende voorbeelden ziet u enkele belangrijke scenario's. Het filter is in het eerste voorbeeld wordt de inhoud van de query.


```POST
POST /indexes/hotels/docs/search?api-version=2017-11-11  
    {  
      "filter": "(baseRate ge 60 and baseRate lt 300) or hotelName eq 'Fancy Stay'"  
    }  
```

Een ander gebruikelijk is filters gecombineerd facetten, waarbij het filter vermindert het query-gebied op basis van een selectie van de navigatie door gebruikers geïnitieerde facet:

```POST
POST /indexes/hotels/docs/search?api-version=2017-11-11  
    {  
      "search": "test",  
      "facets": [ "tags", "baseRate,values:80|150|220" ],  
      "filter": "rating eq 3 and category eq 'Motel'"  
    }  
```

### <a name="filter-operators"></a>Voor de filteroperators  

-   Logische operators (en, of niet).  

-   Vergelijkingsexpressies (`eq, ne, gt, lt, ge, le`). Tekenreeksvergelijkingen zijn hoofdlettergevoelig.  

-   Constanten van de ondersteunde [Entiteitgegevensmodel](https://docs.microsoft.com/dotnet/framework/data/adonet/entity-data-model) (EDP)-typen (Zie [ondersteunde gegevenstypen &#40;Azure Search&#41; ](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) voor een lijst met ondersteunde typen). Constanten van verzamelingtypen worden niet ondersteund.  

-   Verwijzingen naar de veldnamen. Alleen `filterable` velden kunnen worden gebruikt in filterexpressies.  

-   `any` zonder parameters. Deze test of een veld van het type `Collection(Edm.String)` worden elementen bevat.  

-   `any` en `all` met ondersteuning voor beperkte lambda-expressie. 
    
    -   `any/all` worden ondersteund in de velden van het type `Collection(Edm.String)`. 
    
    -   `any` kan alleen worden gebruikt met eenvoudige gelijkheid expressies of een `search.in` functie. Eenvoudige expressies bestaan uit een vergelijking tussen één veld en een letterlijke waarde, bijvoorbeeld `Title eq 'Magna Carta'`.
    
    -   `all` kan alleen worden gebruikt met eenvoudige ongelijkheid expressies of een `not search.in`.   

-   Georuimtelijke functies `geo.distance` en `geo.intersects`. De `geo.distance` functie wordt de afstand in kilometer zijn verwijderd tussen de twee punten, één wordt een veld en één een constante wordt doorgegeven als onderdeel van het filter. De `geo.intersects` functie retourneert ' True ' als een opgegeven punt zich binnen een bepaalde veelhoek, waar het punt een veld is en de veelhoek is opgegeven als een constante doorgegeven als onderdeel van het filter.  

    De veelhoek is een tweedimensionale oppervlak opgeslagen als een reeks punten definiëren van een selectiekader ring (Zie het onderstaande voorbeeld). De veelhoek moet worden gesloten, wat betekent dat de eerste en laatste punt sets moeten hetzelfde zijn. [Punten in een polygoon moeten zich in tegen de klok in volgorde](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

    `geo.distance` retourneert de afstand in kilometer zijn verwijderd in Azure Search. Dit wijkt af van andere services die ondersteuning bieden voor OData georuimtelijke bewerkingen, die doorgaans afstanden in meters te retourneren.  

    > [!NOTE]  
    >  Wanneer u geo.distance in een filter, moet u de afstand die zijn geretourneerd door de functie met een constante met vergelijken `lt`, `le`, `gt`, of `ge`. De operators `eq` en `ne` worden niet ondersteund bij het vergelijken van afstanden. Dit is bijvoorbeeld een correct gebruik van geo.distance: `$filter=geo.distance(location, geography'POINT(-122.131577 47.678581)') le 5`.  

-   De `search.in` functie test of een opgegeven tekenreeks-veld gelijk aan een van een opgegeven lijst met waarden is. Het kan ook worden gebruikt in een of meer aan een enkele waarde van een verzameling tekenreeksveld met een opgegeven lijst met waarden te vergelijken. Gelijkheid van het veld en van elke waarde in de lijst wordt bepaald op een hoofdlettergevoelige manier, dezelfde manier als voor de `eq` operator. Daarom een expressie, zoals `search.in(myfield, 'a, b, c')` is gelijk aan `myfield eq 'a' or myfield eq 'b' or myfield eq 'c'`, behalve dat `search.in` leidt tot veel betere prestaties. 

    De eerste parameter voor de `search.in` functie is de tekenreeks veldverwijzing (of een variabele bereik via een tekenreeksveld verzameling in het geval waarbij `search.in` wordt gebruikt binnen een `any` of `all` expressie). De tweede parameter is een tekenreeks met de lijst met waarden, gescheiden door spaties en/of komma's. Als u gebruiken, scheidingstekens dan spaties en komma's wilt omdat de waarden die tekens bevatten, kunt u een optionele derde parameter `search.in`. 

    Deze derde parameter is een tekenreeks waarin elk teken van de tekenreeks of een subset van deze tekenreeks wordt beschouwd als scheidingsteken bij het parseren van de lijst met waarden in de tweede parameter.

    > [!NOTE]   
    >  Sommige scenario's vereist een veld op basis van een groot aantal constante waarden vergelijken. Bijvoorbeeld: implementatie van security trimming wordt geregeld met filters mogelijk vergelijken van het document-ID-veld op basis van een lijst met id's waarvoor de aanvragende gebruiker leestoegang wordt verleend. In scenario's zoals dit is het raadzaam met behulp van de `search.in` functie in plaats van een meer complexe scheiding van gelijkheid expressies. Gebruik bijvoorbeeld `search.in(Id, '123, 456, ...')` in plaats van `Id eq 123 or Id eq 456 or ....`. 

>  Als u `search.in`, u krijgt dan een seconde reactietijd wanneer de tweede parameter een lijst met honderden of duizenden waarden bevat. Houd er rekening mee dat er geen expliciete limiet voor het aantal items dat u kunt doorgeven is aan `search.in`, hoewel u nog steeds worden beperkt door de grootte van de maximale aanvraag. Echter, de latentie zal toenemen naarmate het aantal waarden groeit.

-   De `search.ismatch` functie zoekopdracht wordt geëvalueerd als onderdeel van een filterexpressie. De documenten die overeenkomen met de zoekopdracht worden geretourneerd in de resultatenset. De volgende overloads van deze functie zijn beschikbaar:
    - `search.ismatch(search)`
    - `search.ismatch(search, searchFields)`
    - `search.ismatch(search, searchFields, queryType, searchMode)`

    Waar: 
  
    - `search`: de query (in een [eenvoudige](query-simple-syntax.md) of [volledige](query-lucene-syntax.md) querysyntaxis). 
    - `queryType`: 'eenvoudige' of 'volledig' standaard 'simple'. Hiermee geeft u op welke querytaal is gebruikt in de `search` parameter.
    - `searchFields`: door komma's gescheiden lijst met doorzoekbare velden om te zoeken, standaard ingesteld op alle doorzoekbare velden in de index.    
    - `searchMode`: 'een' of 'alle' standaard ingesteld op 'alle'. Geeft aan of een of meer van de zoektermen overeen moeten komen voor het tellen van het document als een overeenkomst.

    De bovenstaande parameters gelijk zijn aan de bijbehorende [zoeken aanvraagparameters](https://docs.microsoft.com/rest/api/searchservice/search-documents).

-   De `search.ismatchscoring` functioneren, zoals de `search.ismatch` functie, retourneert ' True ' voor documenten die overeenkomen met de query doorgegeven als parameter. Het verschil is dat de relevantie score van documenten die overeenkomt met de `search.ismatchscoring` query dragen bij aan de totale score document, terwijl in het geval van `search.ismatch`, de document-score niet meer gewijzigd. De volgende overloads van deze functie zijn beschikbaar met parameters die identiek zijn aan die van `search.ismatch`:
    - `search.ismatchscoring(search)`
    - `search.ismatchscoring(search, searchFields)`
    - `search.ismatchscoring(search, searchFields, queryType, searchMode)`

  De `search.ismatch` en `search.ismatchscoring` functies zijn volledig rechthoekige met elkaar en met de rest van de wiskundige filter. Dit betekent dat beide functies in de dezelfde filterexpressie kunnen worden gebruikt. 

### <a name="geospatial-queries-and-polygons-spanning-the-180th-meridian"></a>Georuimtelijke query's en de 180th kaartmeridiaan spanning veelhoeken  
 Voor veel georuimtelijke query bibliotheken formuleren van een query uitvoert met de 180th kaartmeridiaan (in de buurt van de datumgrens) is een off-limits of een tijdelijke oplossing, zoals het splitsen van de veelhoek in twee, een aan beide zijden van de kaartmeridiaan vereist.  

 In Azure Search, georuimtelijke query's die 180 graden lengtegraad zijn werkt zoals verwacht als de query-shape rechthoekige en uw coördinaten uitgelijnd met een rasterindeling langs de lengtegraad en breedtegraad (bijvoorbeeld `geo.intersects(location, geography'POLYGON((179 65,179 66,-179 66,-179 65,179 65))'`). Anders voor niet-rechthoekige of niet-uitgelijnd vormen, kunt u de benadering van de polygoon splitsen.  

<a name="bkmk_limits"></a>

## <a name="filter-size-limitations"></a>Beperkingen van de bestandsgrootte filteren 

 Er gelden beperkingen voor de grootte en complexiteit van filterexpressies die u naar Azure Search verzenden kunt. De limieten zijn ongeveer gebaseerd op het aantal in de filterexpressie van de EU. Een goede vuistregel is dat als u honderden componenten hebt, u risico van het uitvoeren van de limiet. Het is raadzaam om het ontwerpen van uw toepassing zodanig dat deze filters van niet-gebonden grootte wordt gegenereerd.  


## <a name="filter-examples"></a>Voorbeelden van filters  

 Zoek alle hotels met een basistarief kleiner is dan $100 die worden beoordeeld op of boven de 4:  

```  
$filter=baseRate lt 100.0 and rating ge 4  
```  

 Zoek alle hotels dan "Roach Motel" hebben is renovated sinds 2010:  

```  
$filter=hotelName ne 'Roach Motel' and lastRenovationDate ge 2010-01-01T00:00:00Z  
```  

 Zoek alle hotels met een basistarief kleiner is dan $200 die u hebt zijn renovated sinds 2012, met een letterlijke datetime-waarde die informatie over de tijdzone voor Pacific (standaardtijd bevat):  

```  
$filter=baseRate lt 200 and lastRenovationDate ge 2012-01-01T00:00:00-08:00  
```  

 Zoek alle hotels die van de vervangende domeinpagina opgenomen en soorten niet toestaan:  

```  
$filter=parkingIncluded and not smokingAllowed  
```  

 \- OR-  

```  
$filter=parkingIncluded eq true and smokingAllowed eq false  
```  

 Alle hotels die luxe of vervangende domeinpagina bevatten en hebben een classificatie van 5 zoeken:  

```  
$filter=(category eq 'Luxury' or parkingIncluded eq true) and rating eq 5  
```  

 Zoek alle hotels met het label 'Wi-Fi' (waarbij elke hotel heeft voor labels die zijn opgeslagen in een veld Collection(Edm.String)):  

```  
$filter=tags/any(t: t eq 'wifi')  
```  

 Zoek alle hotels zonder de tag "motel":  

```  
$filter=tags/all(t: t ne 'motel')  
```  

 Alle hotels met alle tags vinden:  

```  
$filter=tags/any()  
```  

 Zoek alle hotels binnen 10 kilometer van een opgegeven punt (locatie is een veld van het type Edm.GeographyPoint):  

```  
$filter=geo.distance(location, geography'POINT(-122.131577 47.678581)') le 10  
```  

 Zoek alle hotels binnen een bepaalde viewport beschreven als een veelhoek (locatie is een veld van het type Edm.GeographyPoint). Houd er rekening mee dat de veelhoek is gesloten (de eerste en laatste punt sets moet hetzelfde) en [de punten moeten worden weergegeven in tegen de klok in volgorde](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

```  
$filter=geo.intersects(location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))')  
```  

 Zoek alle hotels die geen waarde in het veld 'description' hebben of waarde expliciet is ingesteld op null:  

```  
$filter=description eq null  
```  

Zoek alle hotels met de naam gelijk is aan een van beide Roach motel' of 'Budget hotel'):  

```  
$filter=search.in(name, 'Roach motel,Budget hotel', ',') 
```

Zoek alle hotels met de naam gelijk is aan een van beide Roach motel' of 'Budget hotel' gescheiden door ' |'):  

```  
$filter=search.in(name, 'Roach motel|Budget hotel', '|') 
```

Zoek alle hotels met de tag 'Wi-Fi' of 'groep':  

```  
$filter=tags/any(t: search.in(t, 'wifi, pool'))  
```

Alle hotels zonder de tag 'motel' en 'handbagage' niet vinden:  

```  
$filter=tags/all(t: not search.in(t, 'motel, cabin'))  
```  

Documenten met het woord "afgebakend" zoeken. Dit filterquery is gelijk aan een [zoekaanvraag](https://docs.microsoft.com/rest/api/searchservice/search-documents) met `search=waterfront`.

```
$filter=search.ismatchscoring('waterfront')
```

Documenten met het woord 'hostel' en waardering groter of gelijk zijn aan 4 of documenten met het woord "motel" en waardering gelijk is aan 5 zoeken. Opmerking: deze aanvraag kan niet worden weergegeven zonder de `search.ismatchscoring` functie.

```
$filter=search.ismatchscoring('hostel') and rating ge 4 or search.ismatchscoring('motel') and rating eq 5
```

Documenten zonder het woord "luxe" zoeken.

```
$filter=not search.ismatch('luxury') 
```

Documenten met de woordgroep in de Indische Oceaan "view" of de classificatie die gelijk is aan 5 zoeken. De `search.ismatchscoring` query wordt uitgevoerd alleen op basis van velden `hotelName` en `description`.
Houd er rekening mee, worden documenten die overeenkomen met alleen de tweede component van de scheiding te worden geretourneerd: hotels met een classificatie gelijk is aan 5. Om deze documenten duidelijk te maken is niet overeen met een van de beoordeelde onderdelen van de expressie, wordt deze geretourneerd met score gelijk zijn aan nul.

```
$filter=search.ismatchscoring('"ocean view"', 'description,hotelName') or rating eq 5
```

Zoeken naar documenten, waarbij de termen "Hotels" en "luchthaven" zijn binnen 5 woorden uit elkaar in de beschrijving van het hotel en waar soorten is niet toegestaan. Deze query gebruikt de [volledige Lucene-querytaal](query-lucene-syntax.md).

```
$filter=search.ismatch('"hotel airport"~5', 'description', 'full', 'any') and not smokingAllowed 
```

## <a name="order-by-syntax"></a>Volgorde-by-syntaxis

De **$orderby** parameter accepteert een door komma's gescheiden lijst met maximaal 32 expressies van het formulier `sort-criteria [asc|desc]`. De sorteercriteria kunnen de naam van een `sortable` veld of een aanroep van hetzij de `geo.distance` of de `search.score` functies. U kunt een gebruiken `asc` of `desc` expliciet de sorteervolgorde opgeven. De standaard wordt oplopend.

Als meerdere documenten dezelfde criteria sorteren hebben en `search.score` functie wordt niet gebruikt (bijvoorbeeld, als u op een numerieke sorteren `rating` veld en drie documenten hebt u een classificatie van 4), ties zullen niet werken op document score in aflopende volgorde. Wanneer het document scores dezelfde (bijvoorbeeld wanneer er geen query zoeken in volledige tekst is opgegeven in de aanvraag) zijn, klikt u vervolgens is de relatieve positie van de gebonden documenten onbepaald.
 
U kunt meerdere criteria voor sorteren opgeven. De volgorde van de expressies bepaalt de volgorde van de laatste sorteren. Bijvoorbeeld, de syntaxis van de zou zijn als u wilt sorteren op Aflopend op score, gevolgd door classificatie `$orderby=search.score() desc,rating desc`.

De syntaxis voor `geo.distance` in **$orderby** is hetzelfde als het zich in **$filter**. Bij het gebruik van `geo.distance` in **$orderby**, het veld waarop deze van toepassing is moet van het type `Edm.GeographyPoint` en moet ook `sortable`.  

De syntaxis voor `search.score` in **$orderby** is `search.score()`. De functie `search.score` neemt geen parameters.  
 

## <a name="order-by-examples"></a>Volgorde van voorbeelden

Hotels door basistarief Oplopend sorteren:

```
$orderby=baseRate asc
```

Sorteren door te beoordelen en vervolgens basistarief oplopende aflopende hotels (Houd er rekening mee dat de standaardinstelling oplopend is):

```
$orderby=rating desc,baseRate
```

Hotels Aflopend op classificatie en klik vervolgens op afstand van de opgegeven coördinaten Oplopend sorteren:

```
$orderby=rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc
```

Sorteren hotels in aflopende volgorde door search.score en classificatie en klik vervolgens in oplopende volgorde op afstand van de opgegeven coördinaten zodat tussen twee hotels met identieke beoordeling, het beste een eerste wordt weergegeven:

```
$orderby=search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc
```
<a name="bkmk_unsupported"></a>

## <a name="unsupported-odata-syntax"></a>Niet-ondersteunde OData-syntaxis

-   Rekenkundige expressies  

-   Functies (met uitzondering van de afstand en snijdt georuimtelijke functies)  

-   `any/all` met willekeurige lambda-expressies  

## <a name="see-also"></a>Zie ook  

+ [Facetnavigatie in Azure Search](search-faceted-navigation.md) 
+ [Filters in Azure Search](search-filters.md) 
+ [Documenten zoeken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) 
+ [Lucene-querysyntaxis](query-lucene-syntax.md)
+ [Vereenvoudigde querysyntaxis in Azure Search](query-simple-syntax.md)   