---
title: Facet filters in Azure Search | Microsoft Docs
description: Filtercriteria door gebruiker beveiligings-id, taal, geografische locatie of numerieke waarden te verminderen zoekresultaten op query's in Azure Search, een gehoste cloud search-service op Microsoft Azure.
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
ms.date: 10/13/2017
ms.author: heidist
ms.openlocfilehash: 02a027845e56407bc8cc95f54a46d9534cb6de92
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="how-to-build-a-facet-filter-in-azure-search"></a>Het bouwen van een filter facet in Azure Search 

Meervoudige navigatie wordt gebruikt voor te filteren op de resultaten van de query in een zoekopdracht-app, waarbij de toepassing biedt UI-besturingselementen voor bereik zoeken op groepen van documenten (bijvoorbeeld categorieën of merken) en Azure Search biedt de structuur van de gegevens back-de ervaring. In dit artikel, controleert u snel de basisstappen voor het maken van een back-ups maken van de zoekfunctie die u wilt bieden meervoudige navigatie-structuur. 

> [!div class="checklist"]
> * Kies de velden voor het filteren en facetten
> * Kenmerken instellen op het veld
> * De index en gegevens laden bouwen
> * Facet filters toevoegen aan een query
> * Resultaten verwerken

Facetten zijn dynamisch en geretourneerde van een query. Zoekresultaten doen om met hen de facetcategorieën gebruikt om de resultaten te bladeren. Als u niet bekend bent met facetten, is het volgende voorbeeld een afbeelding van een facetnavigatiestructuur.

  ![](./media/search-filters-facets/facet-nav.png)

Nieuw voor meervoudige navigatie en wilt meer informatie? Zie [facetnavigatie implementeren in Azure Search](search-faceted-navigation.md).

## <a name="choose-fields"></a>Velden kiezen

Facetten kunnen via één waardevelden, alsmede verzamelingen worden berekend. Velden die het beste werkt voor meervoudige navigatie hebben lage kardinaliteit: een klein aantal afzonderlijke waarden die in de gehele documenten in uw zoekopdracht corpus (bijvoorbeeld, een lijst met kleuren, landen of merken) worden herhaald. 

Facetten is ingeschakeld op basis van veld wanneer u de index maken door de volgende kenmerken in te stellen op TRUE: `filterable`, `facetable`. Alleen filterbare velden kunnen als facet worden gebruikt.

Alle [veldtype](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) die mogelijk worden gebruikt in meervoudige navigatie is gemarkeerd als 'geschikt voor facetten':

+ Edm.String
+ Edm.DateTimeOffset
+ Edm.Boolean
+ Edm.Collections
+ Numerieke veldtypen: Edm.Int32, Edm.Int64, Edm.Double

U kunt Edm.GeographyPoint niet gebruiken in meervoudige navigatie. Facetten zijn samengesteld uit menselijke leesbare tekst of cijfers. Als zodanig worden facetten niet ondersteund voor de geo-coördinaten. U moet een veld of regio aan facet per locatie.

## <a name="set-attributes"></a>Kenmerken instellen

Indexkenmerken die bepalen hoe een veld wordt gebruikt, worden toegevoegd aan afzonderlijke velddefinities in de index. In het volgende voorbeeld velden met een lage kardinaliteit nuttig voor facetten, bestaan uit: categorie (hotel, motel, hostel), faciliteiten en classificaties. 

In de .NET API moeten filterkenmerken expliciet worden ingesteld. In de REST-API worden facetten en filteren ingeschakeld standaard, wat betekent dat u hoeft alleen de kenmerken expliciet zijn ingesteld dat als u deze wilt uit te schakelen. Hoewel dit niet technisch vereist is, weergegeven we de afschrijvingen in het volgende voorbeeld van de REST voor educatieve doeleinden. 

> [!Tip]
> Als een best practice voor prestaties en opslagoptimalisatie uitschakelen facetten voor velden die nooit mogen worden gebruikt als een beperkingsfacet. In het bijzonder tekenreeksvelden voor singleton-waarden, zoals een naam-ID of product moeten worden ingesteld op 'Geschikt voor facetten': ONWAAR om te voorkomen dat hun onbedoeld (en ongeschikt) gebruiken in meervoudige navigatie.


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
> De definitie van de index is gekopieerd uit [een Azure Search index maken met de REST-API](https://docs.microsoft.com/azure/search/search-create-index-rest-api). Het is identiek zijn, met uitzondering van minder belangrijke verschillen in de velddefinities. Kenmerken Filterbaar en geschikt voor facetten worden expliciet op categorie, tags, parkingIncluded, smokingAllowed en classificatievelden toegevoegd. In de praktijk, krijgt u Filterbaar en geschikt voor facetten voor op Edm.String Edm.Boolean en Edm.Int32 veldtypen vrij. 

## <a name="build-and-load-an-index"></a>Opbouwen en laden van een index

Een stap tussenliggende (en mogelijk duidelijk) is die u hebt voor [bouwen en de index te vullen](https://docs.microsoft.com/azure/search/search-create-index-dotnet#create-the-index) voordat u een query formuleren. We deze stap voor de volledigheid worden vermeld. Een manier om te bepalen of de index beschikbaar is, is door het controleren van de lijst van de indexen de [portal](https://portal.azure.com).

## <a name="add-facet-filters-to-a-query"></a>Facet filters toevoegen aan een query

In de toepassingscode, maakt u een query waarmee alle onderdelen van een geldige query, zoals zoeken expressies, facetten, filters, score berekenen profielen – iets gebruikt voor een aanvraag formuleren. Het volgende voorbeeld wordt een aanvraag die facet navigatie op basis van het type accommodatie, beoordeling en andere faciliteiten worden gemaakt.

```csharp
SearchParameters sp = new SearchParameters()
{
  ...
  // Add facets
  Facets = new List<String>() { "category", "rating", "parkingIncluded", "smokingAllowed" },
};
```

### <a name="return-filtered-results-on-click-events"></a>Gefilterde resultaten op klikt u op gebeurtenissen

De filterexpressie verwerkt de gebeurtenis click van de facetwaarde van. Een categorie-beperkingsfacet opgegeven, te klikken op de categorie 'motel' wordt geïmplementeerd via een `$filter` expressie die aanpassingen van dat type selecteert. Wanneer een gebruiker op 'motels' om aan te geven dat alleen motels moeten worden weergegeven, de volgende query de toepassing verzendt bevat een $filter = categorie eq 'motels'.

Als een gebruiker een waarde in de categorie-facet selecteert, het volgende codefragment categorie toegevoegd aan het filter.

```csharp
if (categoryFacet != "")
  filter = "category eq '" + categoryFacet + "'";
```
Met de REST API, zou de aanvraag worden gelede als `$filter=category eq 'c1'`. Als u een veld meerdere waarden maken categorie, gebruikt u de volgende syntaxis:`$filter=category/any(c: c eq 'c1')`

## <a name="tips-and-workarounds"></a>Tips en tijdelijke oplossingen

### <a name="initialize-a-page-with-facets-in-place"></a>Initialiseren van een pagina met facetten die zijn geïmplementeerd

Als u initialiseren van een pagina met facetten die zijn geïmplementeerd wilt, kunt u een query als onderdeel van de initialisatie van de pagina naar de pagina met de structuur van een initiële facet seed verzenden.

### <a name="preserve-a-facet-navigation-structure-asynchronously-of-filtered-results"></a>Een facetnavigatiestructuur asynchroon van gefilterde resultaten behouden

Een van de uitdagingen met facet navigatie in Azure Search is dat facetten bestaan voor de huidige resultaten alleen. In de praktijk wordt het meestal een vaste set van facetten behouden, zodat de gebruiker in omgekeerde volgorde navigeren kan, nalopen stappen uit om te verkennen alternatieve paden via zoeken in de inhoud. 

Hoewel dit een algemene gebruiksvoorbeeld is, is het niet iets de facetnavigatiestructuur biedt momenteel out-of-the-box. Ontwikkelaars willen statische facetten doorgaans de beperking omzeilen door uitgifte van twee gefilterde query's: één binnen het bereik van de resultaten, de andere gebruikt voor het maken van een statische lijst met facetten voor navigatie doeleinden.

## <a name="see-also"></a>Zie ook

+ [Filters in Azure Search](search-filters.md)
+ [Index REST-API maken](https://docs.microsoft.com/rest/api/searchservice/create-index)
+ [REST-API documenten zoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents)

