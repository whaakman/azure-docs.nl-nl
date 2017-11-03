---
title: Filters in Azure Search | Microsoft Docs
description: Filteren op beveiligings-id van gebruiker, taal, geografische locatie of numerieke waarden te verminderen zoekresultaten op query's in Azure Search, een gehoste cloud search-service op Microsoft Azure.
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: 
ms.service: search
ms.devlang: 
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/19/2017
ms.author: heidist
ms.openlocfilehash: 2e8721684b1d4ed0e7392d85ea1df0f595860a05
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="filters-in-azure-search"></a>Filters in Azure Search 

Een *filter* vindt u de criteria voor het selecteren van documenten die worden gebruikt in een Azure Search-query. Ongefilterde zoeken bevat alle documenten in de index. Een filter-query voor een zoekopdracht op een subset van documenten bereiken. Een filter kan bijvoorbeeld zoeken in volledige tekst beperken tot alleen de producten met een specifiek merk of de kleuren, op Prijspunten boven een bepaalde drempelwaarde komt.

Sommige ervaringen search filter vereisten afdwingen als onderdeel van de implementatie, maar u kunt filters gebruiken telkens wanneer u wilt beperken zoekactie met *waarde gebaseerde* criteria (bereik zoekt naar het producttype 'books' categorie' niet-fictie' gepubliceerd door 'Simon & Schuster').

Als u in plaats daarvan gerichte zoeken op specifieke gegevens *structuren* (bereik zoeken naar een veld mening van de klant), er zijn andere methoden, die hieronder worden beschreven.

## <a name="when-to-use-a-filter"></a>Wanneer u een filter te gebruiken

Filters zijn fundamentele naar de verschillende ervaringen van de zoekopdracht, inclusief "zoeken dichtbij', meervoudige navigatie en beveiliging filtert waarin alleen documenten een gebruiker is toegestaan om te zien. Als u een van deze ervaringen implementeert, is een filter vereist. Dit is het filter dat is gekoppeld aan de query die de coördinaten geolocatie biedt de facet categorie geselecteerd door de gebruiker of de beveiligings-ID van de aanvrager.

Voorbeeldscenario's omvatten het volgende:

1. Een filter gebruiken om te segmenteren van uw index op basis van gegevenswaarden in de index. U kunt een filter kunt u documenten die voldoen aan uw criteria (in Seattle, condos, kust) expliciet selecteren met een schema met de plaats waar type en faciliteiten maken. 

  Zoekopdracht in volledige tekst met de dezelfde invoer vaak biedt vergelijkbare resultaten, maar een filter is nauwkeurigere in dat een exacte overeenkomst met de filterterm op basis van inhoud in de index is vereist. 

2. Een filter gebruiken als de zoekfunctie wordt geleverd met een vereiste filter:

 * [Meervoudige navigatie](search-faceted-navigation.md) gebruikt een filter om door te geven weer de facet categorie geselecteerd door de gebruiker.
 * Geo-search gebruikt een filter om door te geven coördinaten van de huidige locatie in 'zoeken dichtbij' apps. 
 * Beveiligingsfilters doorgeven beveiligings-id's als filtercriteria, waarbij een overeenkomst in de index fungeert als proxy voor toegangsrechten tot het document.

3. Een filter gebruiken als u wilt dat de zoekcriteria op een numeriek veld. 

  Numerieke velden worden opgehaald in het document zijn en kunnen worden weergegeven in zoekresultaten, maar ze zijn niet afzonderlijk doorzoekbaar (afhankelijk van de zoekopdracht in volledige tekst). Als u selectiecriteria op basis van numerieke gegevens nodig, gebruikt u een filter.

### <a name="alternative-methods-for-reducing-scope"></a>Alternatieve methoden voor het bereik beperken

Als u een beperkende invloed in de zoekresultaten wilt, wordt filters zijn niet alleen. Deze alternatieven mogelijk beter geschikt zijn, afhankelijk van uw doelstelling:

 + `searchFields`queryparameter wasknijpers zoeken naar specifieke velden. Bijvoorbeeld, als uw index afzonderlijke velden voor de Engelse en Spaanse beschrijvingen bevat, kunt u searchFields toe te passen welke velden moeten worden gebruikt voor zoekopdrachten in volledige tekst. 

+ `$select`parameter wordt gebruikt om op te geven welke velden u wilt opnemen in een resultaat ingesteld, het antwoord effectief bijsnijden voordat deze naar de aanroepende toepassing verzonden. Deze parameter niet de query te verfijnen of beperken van de documentenverzameling, maar als een gedetailleerde antwoord het doel is, deze parameter is een optie om te overwegen. 

Zie voor meer informatie over de parameter [documenten zoeken > aanvragen > queryparameters](https://docs.microsoft.com/rest/api/searchservice/search-documents#request).


## <a name="filters-in-the-query-pipeline"></a>Filters in de query-pipeline

De expressie converteert naar atomic Booleaanse expressies en een filterstructuur, wordt vervolgens geëvalueerd via Filterbaar velden in een index is gebaseerd op het moment dat de query, een filter-parser criteria geaccepteerd als invoer.  

Filteren vindt plaats voordat de zoekopdracht, in aanmerking komende welke documenten worden opgenomen in de downstreamverwerking voor document ophalen en relevantie score berekenen. Wanneer gekoppeld aan een zoekopdracht, vermindert het filter effectief het oppervlak van de volgende search-bewerking. Wanneer u alleen wordt gebruikt (bijvoorbeeld wanneer de query-tekenreeks is leeg wanneer `search=*`), de filtercriteria is de enige invoer. 

## <a name="filter-definition"></a>Filterdefinitie

Filters zijn OData-expressies gelede met behulp van een [subset van de OData-V4-syntaxis ondersteund in Azure Search](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search). 

U kunt een filter opgeven voor elke **search** bewerking, maar het filter zelf kan bevatten meerdere velden, meerdere criteria en als u een **ismatch** functie, meerdere expressies. U kunt in een meerdelig filterexpressie predicaten in willekeurige volgorde opgeven. Er is geen noemenswaardig voordeel in prestaties als u probeert te predicaten in een bepaalde volgorde rangschikken.

De vaste limiet voor een filterexpressie is de maximale limiet voor de aanvraag. De gehele request, inclusief het filter mag uit maximaal 16 MB voor POST of 8 KB voor GET. Dynamische limieten correleert met het aantal componenten in de filterexpressie. Een goede vuistregel is dat als er honderden componenten, u het risico worden uitgevoerd in de limiet. Het is raadzaam om het ontwerpen van uw toepassing zodanig dat deze geen filters met een grootte van niet-gebonden genereert.

De volgende voorbeelden vertegenwoordigen prototypische filterdefinities in verschillende API's.

```http
# Option 1:  Use $filter for GET
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2016-09-01

# Option 2: Use filter for POST and pass it in the header
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

```csharp
    parameters =
        new SearchParameters()
        {
            Filter = "baseRate lt 150",
            Select = new[] { "hotelId", "description" }
        };

```

## <a name="filter-design-patterns"></a>Ontwerppatronen filteren

De volgende voorbeelden ziet enkele ontwerppatronen voor filter scenario's. Zie voor meer ideeën [OData-expressiesyntaxis > voorbeelden](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search#bkmk_examples).

+ Zelfstandige **$filter**, zonder een queryreeks bevat, is nuttig wanneer de filterexpressie is volledig kwalificeren documenten van belang. Zonder een query-tekenreeks is geen lexicale of taalkundige analyse, geen score berekenen en geen ranking. U ziet dat de zoekreeks is leeg.

   ```
   search=*&$filter=(baseRate ge 60 and baseRate lt 300) and accommodation eq 'Hotel' and city eq 'Nogales'
   ```

+ De combinatie van queryreeks en **$filter**, waarbij het filter de subset maakt en de queryreeks de term invoer voor zoeken in volledige tekst via de gefilterde subset bevat. Met een filter met een queryreeks bevat, is het meest voorkomende code-patroon.

   ```
   search=hotels ocean$filter=(baseRate ge 60 and baseRate lt 300) and city eq 'Los Angeles'
   ```

+ Samengestelde query's, gescheiden door 'of', elk met een eigen filtercriteria (bijvoorbeeld ' beagles' in 'aquaduct') of 'siamese' in 'cat'. OF was expressies worden afzonderlijk geëvalueerd met reacties van elkaar gecombineerd tot een reactie terug naar de aanroepende toepassing wordt verzonden. Dit ontwerppatroon wordt bereikt door middel van de functie search.ismatch. U kunt de versie niet score berekenen (search.ismatch) of de score versie (search.ismatchscoring) gebruiken.

   ```
   # Match on hostels rated higher than 4 OR 5-star motels.
   $filter=search.ismatchscoring('hostel') and rating ge 4 or search.ismatchscoring('motel') and rating eq 5

   # Match on 'luxury' or 'high-end' in the description field OR on category exactly equal to 'Luxury'.
   $filter=search.ismatchscoring('luxury | high-end', 'description') or category eq 'Luxury'
   ```

Opvolgen met deze artikelen voor uitgebreide richtlijnen specifieke gebruiksvoorbeelden:

+ [Facetfilters](search-filters-facets.md)
+ [Taalfilters](search-filters-language.md)
+ [Beveiligingsbeperkingen](search-security-trimming-for-azure-search.md) 

## <a name="field-requirements-for-filtering"></a>Vereisten voor het filteren van veld

In de REST API, Filterbaar is *op* standaard. Filterbaar velden vergroot index; Stel `filterable=FALSE` voor velden die u niet wilt gebruiken in een filter. Zie voor meer informatie over instellingen voor velddefinities [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index).

In de .NET SDK, de Filterbaar is *uit* standaard. De API voor het instellen van de eigenschap Filterbaar is [IsFilterable](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.isfilterableattribute). In het volgende voorbeeld wordt een set op de definitie van het veld BaseRate.

```csharp
    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }
```

### <a name="reindexing-requirements"></a>Vereisten voor indexeren

Als een veld niet Filterbaar is en u wilt maken Filterbaar, hebt u een nieuw veld toevoegt of opnieuw opbouwen van de bestaande velden. De velddefinitie van een wijzigen, wijzigt de fysieke structuur van de index. In Azure Search, worden alle paden die toegang hebben geïndexeerd voor snelle query snelheid, die noodzakelijk gegevensstructuren opnieuw opbouwen maakt wanneer velddefinities wijzigt. 

Opnieuw opbouwen van afzonderlijke velden mag een lage impact-bewerking, vereisen een merge-bewerking waarmee de bestaande documentsleutel en het bijbehorende waarden worden verzonden naar de index, blijven de rest van elk document behouden. Als u een vereiste rebuild ondervindt, raadpleegt u de volgende koppelingen voor instructies:

 + [Indexeren van acties met de .NET SDK](https://docs.microsoft.com/azure/search/search-import-data-dotnet#decide-which-indexing-action-to-use)
 + [Indexeren van acties met de REST API](https://docs.microsoft.com/azure/search/search-import-data-rest-api#decide-which-indexing-action-to-use)

## <a name="text-filter-fundamentals"></a>Grondbeginselen van tekst filter

Tekstfilters zijn geldig voor tekenreeksvelden, van waaruit u wilt voor het ophalen van een willekeurige verzameling van documenten op basis van waarden binnen corpus zoeken.

Voor tekstfilters bestaat uit tekenreeksen, is er geen lexicale analysis of woordafbreking, zodat vergelijkingen voor de exacte overeenkomsten zijn. Stel bijvoorbeeld dat een veld *f* 'mooi day', bevat `$filter=f eq 'Sunny'`komt niet overeen met, maar `$filter=f eq 'Sunny day'` wordt. 

Tekenreeksen zijn hoofdlettergevoelig. Er is geen kleine-hoofdlettergebruik van hoofdletters, woorden: `$filter=f eq 'Sunny day'` wordt niet gevonden ' mooi dag ''.


| Benadering | Beschrijving | 
|----------|-------------|
| [Search.in()](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) | Een functie die door komma's gescheiden lijst met tekenreeksen voor een bepaald veld. De tekenreeksen omvatten de filtercriteria op die worden toegepast op elk veld in het bereik voor de query. <br/><br/>`search.in(f, ‘a, b, c’)`semantisch gelijk is aan `f eq ‘a’ or f eq ‘b’ or f eq ‘c’`, behalve dat deze wordt veel sneller uitgevoerd wanneer de lijst met waarden groot is.<br/><br/>Het is raadzaam de **search.in** werken voor [beveiligingsfilters](search-security-trimming-for-azure-search.md) en voor de filters bestaat uit onbewerkte tekst die moet worden geplaatst op de waarden in een bepaald veld. Deze aanpak is ontworpen voor snelheid. U kunt verwachten subsecond reactietijd voor 500 en 5000 liggen van waarden. Er is geen expliciete limiet voor het aantal items die u aan de functie doorgeven kunt, verhoogt de latentie in verhouding tot het aantal tekenreeksen die u opgeeft. | 
| [Search.ismatch()](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) | Een functie waarmee u meng zoekopdracht in volledige tekst bewerkingen met bewerkingen in de filterexpressie dezelfde strikt Boolean-filter. Hiermee kunt meerdere combinaties van query-filter in een aanvraag. U kunt ook gebruiken voor een *bevat* om te filteren op een gedeeltelijke tekenreeks binnen een grotere tekenreeks. |  
| [$filter = veld operatortekenreeks](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) | De expressie voor een gebruiker gedefinieerde bestaan uit velden, operators en waarden. | 

## <a name="numeric-filter-fundamentals"></a>Grondbeginselen van numerieke filter

Numerieke velden zijn niet `searchable` in de context van een zoekopdracht in volledige tekst. Alleen tekenreeksen zijn onderworpen aan zoeken in volledige tekst. Bijvoorbeeld, als u 99,99 als een zoekterm invoert, won't u terughalen prijs 99,99 items. In plaats daarvan ziet u items waarvoor het aantal 99 in tekenreeksvelden van het document. Als u numerieke gegevens hebt, is de veronderstelling dus dat u deze voor filters gebruikt, inclusief adresbereiken, facetten, groepen, enzovoort. 

Documenten met numerieke velden (prijs, grootte, SKU, -ID) opgeven die waarden in de zoekresultaten als het veld is gemarkeerd `retrievable`. Hier is dat zoeken in volledige tekst zelf is niet van toepassing op numeriek veldtypen.

## <a name="next-steps"></a>Volgende stappen

Probeer eerst **Search explorer** in de portal voor het indienen van query's met **$filter** parameters. De [real-goed-sample index](search-get-started-portal.md) interessante resultaten bevat van de volgende query's worden gefilterd wanneer u ze in de zoekbalk plakken:

```
# Geo-filter returning documents within 5 kilometers of Redmond, Washington state
# Use $count=true to get a number of hits returned by the query
# Use $select to trim results, showing values for named fields only
# Use search=* for an empty query string. The filter is the sole input

search=*&$count=true&$select=description,city,postCode&$filter=geo.distance(location,geography'POINT(-122.121513 47.673988)') le 5

# Numeric filters use comparison like greater than (gt), less than (lt), not equal (ne)
# Include "and" to filter on multiple fields (baths and bed)
# Full text search is on John Leclerc, matching on John or Leclerc

search=John Leclerc&$count=true&$select=source,city,postCode,baths,beds&$filter=baths gt 3 and beds gt 4

# Text filters can also use comparison operators
# Wrap text in single or double quotes and use the correct case
# Full text search is on John Leclerc, matching on John or Leclerc

search=John Leclerc&$count=true&$select=source,city,postCode,baths,beds&$filter=city gt 'Seattle'
```

Om te werken met meer voorbeelden, Zie [OData-Filter-expressiesyntaxis > voorbeelden](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search#bkmk_examples).

## <a name="see-also"></a>Zie ook

+ [Hoe vol tekst zoeken werkt in Azure Search](search-lucene-query-architecture.md)
+ [REST-API documenten zoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents)
+ [Vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Lucene-querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)
+ [Ondersteunde gegevenstypen](https://docs.microsoft.com/rest/api/searchservice/supported-data-types)
