---
title: Facetfilters om de zoeknavigatie te in apps - Azure Search
description: Filtercriteria door gebruiker beveiligings-id, geo-locatie of numerieke waarden te verminderen van de zoekresultaten op query's in Azure Search, een gehoste cloud search-service op Microsoft Azure.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 5/13/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 8dffc5b87aefe23953d3a74f1d96b5ee03e0315d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65597388"
---
# <a name="how-to-build-a-facet-filter-in-azure-search"></a>Over het bouwen van een filter facet in Azure Search 

Meervoudige navigatie wordt gebruikt voor zelfgestuurd filteren op de resultaten van de query in een app zoeken, waarbij uw toepassing biedt UI-besturingselementen voor scoping zoeken naar groepen van documenten (bijvoorbeeld, categorieën of merken) en Azure Search biedt de gegevensstructuur voor de back-ups maken de ervaring. In dit artikel de basisstappen voor het maken van een facetnavigatiestructuur back-ups maken van de zoekervaring die u wilt bieden snel te controleren. 

> [!div class="checklist"]
> * Kies de velden voor het filteren en op meerdere niveaus
> * Kenmerken in het veld instellen
> * Bouw de index en gegevens laden
> * Facetfilters toevoegen aan een query
> * Resultaten verwerken

Facetten zijn dynamisch en geretourneerd in een query. De reacties op webzoekopdrachten doen om met hen de facetcategorieën gebruikt om te navigeren van de resultaten. Als u niet bekend bent met de facetten, is het volgende voorbeeld een afbeelding van een facetnavigatiestructuur.

  ![](./media/search-filters-facets/facet-nav.png)

Nieuw voor meervoudige navigatie en u meer informatie? Zie [facetnavigatie implementeren in Azure Search](search-faceted-navigation.md).

## <a name="choose-fields"></a>Velden kiezen

Facetten kunnen worden berekend voor één waardevelden, evenals de verzamelingen. Velden die het beste in facetnavigatie werken hebben lage kardinaliteit: een klein aantal afzonderlijke waarden die in de gehele documenten in uw zoekverzameling (bijvoorbeeld een lijst met kleuren, landen/regio's of merknamen) worden herhaald. 

Op meerdere niveaus op basis van de door veld is ingeschakeld wanneer u de index door in te stellen maken de `facetable` kenmerk `true`. U moet over het algemeen ook ingesteld de `filterable` kenmerk `true` voor deze velden zodat uw zoektoepassing kunt filteren op de velden op basis van de facetten die door de eindgebruiker wordt geselecteerd. 

Bij het maken van de REST-API, elk met een index [veldtype](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) die mogelijk worden gebruikt in meervoudige navigatie is gemarkeerd als `facetable` standaard:

+ `Edm.String`
+ `Edm.DateTimeOffset`
+ `Edm.Boolean`
+ Numerieke veldtypen: `Edm.Int32`, `Edm.Int64`, `Edm.Double`
+ Verzamelingen van de bovenstaande typen (bijvoorbeeld `Collection(Edm.String)` of `Collection(Edm.Double)`)

U kunt geen gebruiken `Edm.GeographyPoint` of `Collection(Edm.GeographyPoint)` velden in meervoudige navigatie. Facetten werken het beste voor velden met een lage kardinaliteit. Vanwege de resolutie van geo-coördinaten is het zeldzaam dat een twee sets coördinaten gelijk in een bepaalde gegevensset zal zijn. Facetten zijn daarom niet ondersteund voor geo-coördinaten. U moet een stad of regio veld facet per locatie.

## <a name="set-attributes"></a>Kenmerken instellen

Indexkenmerken die bepalen hoe een veld wordt gebruikt, worden toegevoegd aan afzonderlijke velddefinities in de index. In het volgende voorbeeld wordt de velden met een lage kardinaliteit, nuttig voor facetten, bestaan uit: `category` (hotel, motel, hostel) `tags`, en `rating`. Deze velden hebben de `filterable` en `facetable` kenmerken set expliciet in het volgende voorbeeld ter illustratie. 

> [!Tip]
> Als een best practice voor prestaties en opslagoptimalisatie van uitschakelen op meerdere niveaus voor velden die moeten nooit worden gebruikt als een facet. In het bijzonder tekenreeksvelden unieke waarden, zoals een naam-ID of product moeten worden ingesteld op `"facetable": false` om te voorkomen dat onbedoelde (en inefficiënte) gebruik in meervoudige navigatie.


```json
{
  "name": "hotels",  
  "fields": [
    { "name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false },
    { "name": "baseRate", "type": "Edm.Double" },
    { "name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false },
    { "name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene" },
    { "name": "hotelName", "type": "Edm.String", "facetable": false },
    { "name": "category", "type": "Edm.String", "filterable": true, "facetable": true },
    { "name": "tags", "type": "Collection(Edm.String)", "filterable": true, "facetable": true },
    { "name": "parkingIncluded", "type": "Edm.Boolean",  "filterable": true, "facetable": true, "sortable": false },
    { "name": "smokingAllowed", "type": "Edm.Boolean", "filterable": true, "facetable": true, "sortable": false },
    { "name": "lastRenovationDate", "type": "Edm.DateTimeOffset" },
    { "name": "rating", "type": "Edm.Int32", "filterable": true, "facetable": true },
    { "name": "location", "type": "Edm.GeographyPoint" }
  ]
}
```

> [!Note]
> Deze definitie van de index wordt opgehaald uit [maken van een Azure Search-index met behulp van de REST-API](https://docs.microsoft.com/azure/search/search-create-index-rest-api). Het is vrijwel identiek, met uitzondering van minder belangrijke verschillen in de velddefinities. De `filterable` en `facetable` kenmerken expliciet worden toegevoegd op `category`, `tags`, `parkingIncluded`, `smokingAllowed`, en `rating` velden. In de praktijk `filterable` en `facetable` zou worden standaard ingeschakeld op deze velden bij het gebruik van de REST-API. Wanneer u de .NET SDK gebruikt, moeten deze kenmerken expliciet zijn ingeschakeld.

## <a name="build-and-load-an-index"></a>Bouw en een index laden

Een stap tussenliggende (en mogelijk voor de hand liggende) is die u hebt voor [bouwen en de index te vullen](https://docs.microsoft.com/azure/search/search-create-index-dotnet#3---construct-index) voordat u een query te formuleren. We deze stap voor de volledigheid worden vermeld. Een manier om te bepalen of de index beschikbaar is, is door het controleren van de lijst indexen de [portal](https://portal.azure.com).

## <a name="add-facet-filters-to-a-query"></a>Facetfilters toevoegen aan een query

In de code van de toepassing, een query waarmee alle onderdelen van een geldige query, met inbegrip van uitdrukkingen zoeken, facetten, filters, score profielen – iets gebruikt voor het formuleren van een aanvraag te maken. Het volgende voorbeeld wordt een aanvraag die facet navigatie op basis van het type accommodatie, beoordeling en andere faciliteiten worden gemaakt.

```csharp
var sp = new SearchParameters()
{
    ...
    // Add facets
    Facets = new[] { "category", "rating", "parkingIncluded", "smokingAllowed" }.ToList()
};
```

### <a name="return-filtered-results-on-click-events"></a>Gefilterde resultaten op klikt u op gebeurtenissen

Wanneer de eindgebruiker op een facetwaarde klikt, moet de handler voor de gebeurtenis click een filterexpressie gebruiken om te profiteren van de intentie van de gebruiker. Gegeven een `category` facet, te klikken op de categorie "motel" wordt geïmplementeerd met een `$filter` expressie die faciliteiten van dat type selecteert. Wanneer een gebruiker op 'motel' om aan te geven dat alleen motels moeten worden weergegeven, de volgende query die de toepassing verzendt bevat `$filter=category eq 'motel'`.

Het volgende codefragment wordt categorie aan het filter toegevoegd als een gebruiker een waarde in de categorie-facet selecteert.

```csharp
if (!String.IsNullOrEmpty(categoryFacet))
    filter = $"category eq '{categoryFacet}'";
```

Als de gebruiker op een facetwaarde voor een verzameling-veld, zoals klikt `tags`, bijvoorbeeld de waarde 'groep', moet uw toepassing de volgende filtersyntaxis gebruiken: `$filter=tags/any(t: t eq 'pool')`

## <a name="tips-and-workarounds"></a>Tips en tijdelijke oplossingen

### <a name="initialize-a-page-with-facets-in-place"></a>Een pagina met de facetten erin initialiseren

Als u een pagina met de facetten erin initialiseren wilt, kunt u een query als onderdeel van de initialisatie van de pagina seeding van de pagina met de structuur van een eerste facet te verzenden.

### <a name="preserve-a-facet-navigation-structure-asynchronously-of-filtered-results"></a>Een facetnavigatiestructuur asynchroon van gefilterde resultaten behouden

Een van de uitdagingen met facet navigatie in Azure Search is dat facetten voor de huidige resultaten alleen bestaat. In de praktijk is het gebruikelijk dat een vaste set facetten behouden, zodat de gebruiker in omgekeerde volgorde navigeren kan, nalopen stappen voor het alternatieve pad in zoeken in de inhoud te verkennen. 

Hoewel dit een gebruikelijk is, is het niet iets dat de facetnavigatiestructuur biedt momenteel out-of-the-box. Ontwikkelaars die statische facetten doorgaans willen de beperking omzeilen door uitgifte van twee gefilterde query's: één binnen het bereik van de resultaten, de andere gebruikt voor het maken van een statische lijst facetten voor navigatie doeleinden.

## <a name="see-also"></a>Zie ook

+ [Filters in Azure Search](search-filters.md)
+ [Index REST-API maken](https://docs.microsoft.com/rest/api/searchservice/create-index)
+ [REST-API voor Search-documenten](https://docs.microsoft.com/rest/api/searchservice/search-documents)
