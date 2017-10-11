---
title: Hoe zoekresultaten in Azure Search | Microsoft Docs
description: Paginering in Azure Search, een gehoste cloud search-service op Microsoft Azure.
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: a0a1d315-8624-4cdf-b38e-ba12569c6fcc
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: heidist
ms.openlocfilehash: 1054e15a2751c53aad5dbc8054c4cec41102dee9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-page-search-results-in-azure-search"></a>Pagina's met zoekresultaten maken in Azure Search
In dit artikel biedt richtlijnen voor het gebruik van de Azure Search Service REST API voor het implementeren van standaardelementen van een pagina met zoekresultaten, zoals het totale aantal, document ophalen, sorteervolgorde en navigatie.

In elk geval hieronder vermelde opties voor pagina's gerelateerde bijdragen of gegevens naar de pagina met zoekresultaten worden opgegeven via de [zoekdocument](http://msdn.microsoft.com/library/azure/dn798927.aspx) aanvragen die naar uw Azure Search-Service verzonden. Aanvragen bevatten een GET-opdracht, pad, en queryparameters die informeren over de service wat wordt aangevraagd en hoe u het antwoord formuleren.

> [!NOTE]
> Een geldige aanvraag bevat een aantal elementen, zoals een service-URL en het pad, HTTP-term `api-version`, enzovoort. Als beknopt alternatief bevat, wordt de voorbeelden om uit te lichten alleen de syntaxis die relevant is voor paginering bijgesneden. Zie de [Azure Search Service REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) documentatie voor meer informatie over de syntaxis van de aanvraag.
> 
> 

## <a name="total-hits-and-page-counts"></a>Totaal aantal treffers en het aantal paginaweergaven
Het totale aantal resultaten geretourneerd door een query wordt weergegeven en vervolgens de resultaten te retourneren in kleinere reeksen is van cruciaal belang op vrijwel alle pagina's voor zoeken.

![][1]

In Azure Search, gebruikt u de `$count`, `$top`, en `$skip` parameters om deze waarden te retourneren. Het volgende voorbeeld toont een voorbeeld van een aanvraag voor het totaal aantal treffers geretourneerd als `@OData.count`:

        GET /indexes/onlineCatalog/docs?$count=true

Documenten in groepen van 15 ophalen en het totaal aantal treffers, beginnend bij de eerste pagina worden ook weergegeven:

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

Resultaten pagineren vereist zowel `$top` en `$skip`, waarbij `$top` Hiermee geeft u op hoeveel items om te retourneren in een batch en `$skip` Hiermee geeft u op hoeveel items om over te slaan. In het volgende voorbeeld wordt elke pagina bevat de volgende 15 items aangegeven door de incrementele jumps in de `$skip` parameter.

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=15&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=30&$count=true

## <a name="layout"></a>Lay-out
Op een pagina met zoekresultaten, is het raadzaam om een miniatuur, een subset van velden en een koppeling naar een product met volledig pagina weer te geven.

 ![][2]

U wilt gebruiken in Azure Search `$select` en een lookup-opdracht voor het implementeren van deze ervaring.

Een subset van velden voor een naast elkaar indeling geretourneerd:

        GET /indexes/ onlineCatalog/docs?search=*&$select=productName,imageFile,description,price,rating 

Installatiekopieën en media-bestanden zijn niet rechtstreeks kan worden doorzocht en moeten worden opgeslagen in een ander platform van de opslag, zoals Azure Blob-opslag om kosten te verlagen. Definieer een veld waarin de URL-adres van de externe inhoud opgeslagen in de index en documenten. Vervolgens kunt u het veld als een verwijzing naar afbeelding. De URL van de afbeelding moet in het document.

Voor het ophalen van een product beschrijvingspagina voor een **onClick** gebeurtenis, gebruik [Lookup Document](http://msdn.microsoft.com/library/azure/dn798929.aspx) om door te geven in de sleutel van het document om op te halen. Het gegevenstype van de sleutel is `Edm.String`. In dit voorbeeld is het *246810*. 

        GET /indexes/onlineCatalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a>Sorteren op relevantie, classificatie of prijs
Sorteervolgorde vaak standaard op relevantie, maar het is gebruikelijk om alternatieve sorteren orders gemakkelijk beschikbaar maken zodat klanten snel bestaande resultaten naar een andere volgorde van de waarden van positie verplaatsen kunnen.

 ![][3]

In Azure Search sorteren is gebaseerd op de `$orderby` expressie, voor alle velden die zijn geïndexeerd als`"Sortable": true.`

Relevantie is ten zeerste aan score berekenen voor profielen gekoppeld. Kunt u het standaard scoren die afhankelijk is van de analyse van tekst en statistieken rangschikken volgorde alle resultaten met hogere scores gebeurt met documenten met meer of sterker komt overeen met een zoekterm.

Alternatieve sorteervolgorde zijn meestal gekoppeld aan **onClick** gebeurtenissen die terugbellen naar een methode die de sorteervolgorde is gebaseerd. Bijvoorbeeld, krijgt deze pagina-element:

 ![][4]

Maakt u een methode die de geselecteerde sorteeroptie als invoer accepteert en retourneert een geordende lijst voor de criteria gekoppeld aan die optie.

 ![][5]

> [!NOTE]
> De standaard score berekenen is voldoende voor veel scenario's, wordt u aangeraden relevantie in plaats daarvan op een aangepaste scoreprofiel baseert. Een aangepaste scoreprofiel biedt u een manier versterking items meer nuttig zijn voor uw bedrijf. Zie [een scoreprofiel toevoegen](http://msdn.microsoft.com/library/azure/dn798928.aspx) voor meer informatie. 
> 
> 

## <a name="faceted-navigation"></a>Facetnavigatie
Zoeken navigatie is gebruikelijk dat op een pagina met zoekresultaten, vaak zich bevindt op de bovenkant van de pagina of. In Azure Search biedt facetnavigatie zelf gerichte zoeken op basis van vooraf gedefinieerde filters. Zie [facetnavigatie in Azure Search](search-faceted-navigation.md) voor meer informatie.

## <a name="filters-at-the-page-level"></a>Filters op paginaniveau
Als uw ontwerp van de oplossing opgenomen toegewezen zoekpagina's voor specifieke typen inhoud (bijvoorbeeld, een online retail toepassing met afdelingen weergegeven boven aan de pagina), kunt u een filterexpressie naast een **onClick**gebeurtenis om een pagina openen in een vooraf gefilterde staat. 

U kunt een filter met of zonder een zoekexpressie verzenden. De volgende aanvraag wordt bijvoorbeeld filteren op naam van het merk, alleen documenten die overeenkomen met het retourneren.

        GET /indexes/onlineCatalog/docs?$filter=brandname eq ‘Microsoft’ and category eq ‘Games’

Zie [documenten zoeken (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) voor meer informatie over `$filter` expressies.

## <a name="see-also"></a>Zie ook
* [Azure Search Service REST-API](http://msdn.microsoft.com/library/azure/dn798935.aspx)
* [Indexbewerkingen](http://msdn.microsoft.com/library/azure/dn798918.aspx)
* [Document bewerkingen](http://msdn.microsoft.com/library/azure/dn800962.aspx)
* [Video's en zelfstudies over het Azure Search](search-video-demo-tutorial-list.md)
* [Meervoudige navigatie in Azure Search](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png 
