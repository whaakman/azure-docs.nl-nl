---
title: Facetfilters om de zoeknavigatie te in apps - Azure Search
description: Filtercriteria door gebruiker beveiligings-id, geo-locatie of numerieke waarden te verminderen van de zoekresultaten op query's in Azure Search, een gehoste cloud search-service op Microsoft Azure.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 10/13/2017
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 8793f6f4d135d6099541d24aa5f5cfc0b6c21b30
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58339428"
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

Facetten kunnen worden berekend voor één waardevelden, evenals de verzamelingen. Velden die het beste in facetnavigatie werken hebben lage kardinaliteit: een klein aantal afzonderlijke waarden die in de gehele documenten in uw zoekverzameling (bijvoorbeeld een lijst met kleuren, landen of merknamen) worden herhaald. 

Op meerdere niveaus op basis van de door veld is ingeschakeld wanneer u de index maken met de volgende kenmerken instellen op TRUE: `filterable`, `facetable`. Alleen filterbare velden kunnen als facet worden gebruikt.

Alle [veldtype](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) die mogelijk worden gebruikt in meervoudige navigatie is gemarkeerd als 'geschikt voor facetten':

+ Edm.String
+ Edm.DateTimeOffset
+ Edm.Boolean
+ Edm.Collections
+ Numeriek veldtypen: Edm.Int32, Edm.Int64, Edm.Double

U kunt Edm.GeographyPoint in meervoudige navigatie gebruiken. Facetten zijn samengesteld uit menselijke leesbare tekst of cijfers. Facetten zijn daarom niet ondersteund voor geo-coördinaten. U moet een stad of regio veld facet per locatie.

## <a name="set-attributes"></a>Kenmerken instellen

Indexkenmerken die bepalen hoe een veld wordt gebruikt, worden toegevoegd aan afzonderlijke velddefinities in de index. In het volgende voorbeeld wordt de velden met een lage kardinaliteit, nuttig voor facetten, bestaan uit: categorie (hotel, motel, hostel), faciliteiten en classificaties. 

Filterkenmerken moeten expliciet worden ingesteld in de .NET-API. In de REST-API, onderverdeling en filteren van standaard, wat betekent dat u hoeft alleen de kenmerken van expliciet instellen dat als u wilt uitschakelen ingeschakeld. Maar dit geen technisch vereist is, laten we zien de afschrijvingen in het volgende voorbeeld van de REST voor educatieve doeleinden. 

> [!Tip]
> Als een best practice voor prestaties en opslagoptimalisatie van uitschakelen op meerdere niveaus voor velden die moeten nooit worden gebruikt als een facet. In het bijzonder tekenreeksvelden voor singleton-waarden, zoals een naam-ID of product moeten worden ingesteld op "Geschikt voor facetten": false om te voorkomen dat hun per ongeluk niet effectief gebruiken () in meervoudige navigatie.


```http
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String", "filterable": true, "facetable": true},
        {"name": "tags", "type": "Collection(Edm.String)", "filterable": true, "facetable": true},
        {"name": "parkingIncluded", "type": "Edm.Boolean",  "filterable": true, "facetable": true, "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "filterable": true, "facetable": true, "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32", "filterable": true, "facetable": true},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
```

> [!Note]
> Deze definitie van de index wordt opgehaald uit [maken van een Azure Search-index met behulp van de REST-API](https://docs.microsoft.com/azure/search/search-create-index-rest-api). Het is vrijwel identiek, met uitzondering van minder belangrijke verschillen in de velddefinities. Kenmerken filteren mogelijk en geschikt voor facetten worden expliciet op categorie, labels, parkingIncluded, smokingAllowed en classificatievelden toegevoegd. U krijgt Filterbaar en geschikt voor facetten voor gratis op Edm.String Edm.Boolean en Edm.Int32 veldtypen in de praktijk. 

## <a name="build-and-load-an-index"></a>Bouw en een index laden

Een stap tussenliggende (en mogelijk voor de hand liggende) is die u hebt voor [bouwen en de index te vullen](https://docs.microsoft.com/azure/search/search-create-index-dotnet#3---construct-index) voordat u een query te formuleren. We deze stap voor de volledigheid worden vermeld. Een manier om te bepalen of de index beschikbaar is, is door het controleren van de lijst indexen de [portal](https://portal.azure.com).

## <a name="add-facet-filters-to-a-query"></a>Facetfilters toevoegen aan een query

In de code van de toepassing, een query waarmee alle onderdelen van een geldige query, met inbegrip van uitdrukkingen zoeken, facetten, filters, score profielen – iets gebruikt voor het formuleren van een aanvraag te maken. Het volgende voorbeeld wordt een aanvraag die facet navigatie op basis van het type accommodatie, beoordeling en andere faciliteiten worden gemaakt.

```csharp
SearchParameters sp = new SearchParameters()
{
  ...
  // Add facets
  Facets = new List<String>() { "category", "rating", "parkingIncluded", "smokingAllowed" },
};
```

### <a name="return-filtered-results-on-click-events"></a>Gefilterde resultaten op klikt u op gebeurtenissen

De filterexpressie verwerkt de gebeurtenis click van de facetwaarde. Een categorie-facet opgegeven, te klikken op de categorie "motel" wordt geïmplementeerd via een `$filter` expressie die faciliteiten van dat type selecteert. Wanneer een gebruiker op 'motels' om aan te geven dat alleen motels moeten worden weergegeven, de volgende query die de toepassing verzendt $filter bevat = categorie eq 'motels'.

Het volgende codefragment wordt categorie aan het filter toegevoegd als een gebruiker een waarde in de categorie-facet selecteert.

```csharp
if (categoryFacet != "")
  filter = "category eq '" + categoryFacet + "'";
```
Met behulp van de REST-API, de aanvraag zou worden gelede als `$filter=category eq 'c1'`. Categorie een veld meerdere waarden maken, gebruiken de volgende syntaxis: `$filter=category/any(c: c eq 'c1')`

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
