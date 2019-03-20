---
title: Over het werken met zoekresultaten - Azure Search
description: Structuur en zoekresultaten sorteren, een documentaantal ophalen en inhoud navigatie zoekresultaten in Azure Search toevoegen.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: ''
ms.topic: conceptual
ms.date: 02/14/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: ef29dafe32c3c5988cd33f59c8436eeef4b45886
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57849275"
---
# <a name="how-to-work-with-search-results-in-azure-search"></a>Over het werken met zoeken resulteert in Azure Search
Dit artikel bevat richtlijnen over het implementeren van standard elementen van een pagina met zoekresultaten, zoals het totale aantal, document ophalen, sorteervolgorde en navigatie. Opties voor pagina's gerelateerde die of gegevens naar uw zoekresultaten bijdragen worden opgegeven via de [Document doorzoeken](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) aanvragen naar uw Azure Search-Service verzonden. 

In de REST-API bevatten aanvragen een GET-opdracht, pad, en queryparameters die de service wat wordt aangevraagd informeren en hoe u het antwoord te formuleren. In de .NET SDK, de equivalente API is [DocumentSearchResult klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.documentsearchresult?view=azure-dotnet).

Enkele voorbeelden van code bevatten een web front-interface, dat u hier kunt vinden: [New York City taken demo-app](https://azjobsdemo.azurewebsites.net/) en [CognitiveSearchFrontEnd](https://github.com/LuisCabrer/CognitiveSearchFrontEnd).

> [!NOTE]
> Een geldige aanvraag bevat een aantal elementen, zoals een service-URL en het pad, HTTP-term `api-version`, enzovoort. Beknopt alternatief bijgesneden we in de voorbeelden om te markeren, alleen de syntaxis die relevant is voor paginering. Zie voor meer informatie over de syntaxis van de aanvraag [Azure Search Service REST API](https://docs.microsoft.com/rest/api/searchservice). > 
> 

## <a name="total-hits-and-page-counts"></a>Totaal aantal treffers en het aantal paginaweergaven
Met het totale aantal resultaten die door een query zijn geretourneerd, en vervolgens de resultaten te retourneren in kleinere chunks is van cruciaal belang op vrijwel alle pagina's voor zoeken.

![][1]

In Azure Search, gebruikt u de `$count`, `$top`, en `$skip` parameters om deze waarden te retourneren. Het volgende voorbeeld toont een voorbeeld van een aanvraag voor totaal aantal treffers op een index met de naam 'onlineCatalog", geretourneerd als `@OData.count`:

        GET /indexes/onlineCatalog/docs?$count=true

Documenten in groepen van 15 ophalen en ook het totaal aantal treffers, beginnend bij de eerste pagina worden weergegeven:

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

Resultaten pagineren vereist zowel `$top` en `$skip`, waarbij `$top` Hiermee geeft u op hoeveel items om te retourneren in een batch en `$skip` Hiermee geeft u op hoeveel items om over te slaan. In het volgende voorbeeld wordt elke pagina ziet u de volgende 15 items, aangegeven door de incrementele koppelingen in de `$skip` parameter.

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=15&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=30&$count=true

## <a name="layout"></a>Layout
Op een pagina met zoekresultaten, is het raadzaam om een miniatuurafbeelding, een subset van velden en een koppeling naar een product met volledig pagina weer te geven.

 ![][2]

In Azure Search, zou u `$select` en een lookup-opdracht voor het implementeren van deze ervaring.

Om terug te keren een subset van velden voor een naast elkaar lay-out:

        GET /indexes/ onlineCatalog/docs?search=*&$select=productName,imageFile,description,price,rating 

Installatiekopieën en media-bestanden zijn niet rechtstreeks kan worden doorzocht en moeten worden opgeslagen in een andere opslagplatform, zoals Azure Blob-opslag, om kosten te verlagen. In de index en documenten, definieert u een veld waarin de URL-adres van de externe inhoud. U kunt vervolgens het veld gebruiken als een verwijzing naar de installatiekopie. De URL naar de installatiekopie moet zich in het document.

Ophalen van een beschrijving van de productpagina voor een **onClick** gebeurtenis, gebruik [Lookup Document](https://docs.microsoft.com/rest/api/searchservice/Lookup-Document) om door te geven in de sleutel van het document om op te halen. Het gegevenstype van de sleutel is `Edm.String`. In dit voorbeeld is het *246810*. 

        GET /indexes/onlineCatalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a>Sorteren op relevantie, classificatie of prijs
Sorteervolgorde vaak standaard ingesteld op relevantie, maar het is gebruikelijk om alternatieve sorteren orders direct beschikbaar maken zodat klanten snel bestaande resultaten naar een andere positie volgorde verplaatsen kunnen.

 ![][3]

In Azure Search sorteren is gebaseerd op de `$orderby` expressie, voor alle velden die zijn geïndexeerd als `"Sortable": true.` een `$orderby` component is een OData-expressie. Zie voor meer informatie over de syntaxis van [syntaxis voor OData-expressie voor filters en order by-componenten](query-odata-filter-orderby-syntax.md).

Relevantie is sterk aan scoreprofielen gekoppeld. Kunt u de standaard score, die is afhankelijk van de analyse van tekst en statistieken rangschikken op volgorde alle resultaten met hogere scores gaan documenten met sterkere of meer overeenkomsten op een zoekterm.

Alternatieve sorteervolgorde zijn meestal gekoppeld aan **onClick** gebeurtenissen die aanroepen om een methode die de sorteervolgorde is gebaseerd. Bijvoorbeeld, vermeld dit paginaelement:

 ![][4]

Maakt u een methode die de geselecteerde sorteeroptie als invoer accepteert en retourneert een geordende lijst voor de criteria die zijn gekoppeld aan deze optie.

 ![][5]

> [!NOTE]
> Het standaard scoren is voldoende voor veel scenario's, wordt u aangeraden relevantie in plaats daarvan op een aangepaste scoringprofiel baseren. Een aangepaste scoringprofiel biedt een manier om meer nuttig zijn voor uw bedrijf boost-items. Zie [scoreprofielen toevoegen](index-add-scoring-profiles.md) voor meer informatie. 
> 
> 

## <a name="faceted-navigation"></a>Facetnavigatie
Zoeknavigatie is gebruikelijk is in een pagina met resultaten, vaak te vinden op de bovenkant van de pagina of. In Azure Search biedt facetnavigatie Self-directed search op basis van vooraf gedefinieerde filters. Zie [facetnavigatie in Azure Search](search-faceted-navigation.md) voor meer informatie.

## <a name="filters-at-the-page-level"></a>Filters op paginaniveau
Als uw ontwerp van de oplossing opgenomen toegewezen zoekpagina's voor specifieke typen inhoud (bijvoorbeeld een detailhandel online toepassing die diensten die aan de bovenkant van de pagina worden vermeld heeft), kunt u een [filterexpressie](search-filters.md) naast een **onClick** gebeurtenis aan een pagina openen in een vooraf gefilterde status. 

U kunt een filter met of zonder een zoekexpressie verzenden. De volgende aanvraag wordt bijvoorbeeld filteren op naam, alleen documenten die overeenkomen met het retourneren.

        GET /indexes/onlineCatalog/docs?$filter=brandname eq ‘Microsoft’ and category eq ‘Games’

Zie [documenten zoeken (Azure Search API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) voor meer informatie over `$filter` expressies.

## <a name="see-also"></a>Zie ook
* [Azure Search Service REST API](https://docs.microsoft.com/rest/api/searchservice)
* [-Indexbewerkingen](https://docs.microsoft.com/rest/api/searchservice/Index-operations)
* [Documentbewerkingen](https://docs.microsoft.com/rest/api/searchservice/Document-operations)
* [Facetnavigatie in Azure Search](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png 
