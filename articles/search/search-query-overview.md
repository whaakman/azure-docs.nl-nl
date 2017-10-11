---
title: Een query uitvoeren in uw Azure Search-index | Microsoft Docs
description: Een zoekquery samenstellen in Azure Search en gebruikmaken van zoekparameters om zoekresultaten te filteren en te sorteren.
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 69205d7a-363f-4b92-a53f-6ca818a3d2c7
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 04/26/2017
ms.author: ashmaka
ms.openlocfilehash: a22b82829df4659681940267e64c98d345453958
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="query-your-azure-search-index"></a>Een query uitvoeren in uw Azure Search-index
> [!div class="op_single_selector"]
> * [Overzicht](search-query-overview.md)
> * [Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Als u zoekaanvragen verzendt naar Azure Search, kunt u naast de werkelijke woorden die in het zoekvak van uw toepassing worden getypt een aantal parameters opgeven. Deze queryparameters bieden een betere controle over de [functie voor het zoeken in volledige tekst](search-lucene-query-architecture.md).

Hieronder volgt een lijst met een korte beschrijving van het algemeen gebruik van de queryparameters in Azure Search. Zie de gedetailleerde pagina's over de [REST-API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) en [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters#microsoft_azure_search_models_searchparameters#properties_summary) voor een uitgebreid overzicht van queryparameters en de functie ervan.

## <a name="types-of-queries"></a>Typen query's
Azure Search biedt verschillende opties om krachtige query's te maken. De twee belangrijkste querytypen die u zult gebruiken zijn `search` en `filter`. Met een `search`-query zoekt u naar een of meerdere termen in alle *doorzoekbare* velden in uw index. Deze query werkt min of meer als een zoekmachine als Google of Bing. Een `filter`-query evalueert een Booleaanse expressie op alle *filterbare* velden in een index. In tegenstelling tot `search`-query 's vergelijken `filter`-query's de exacte inhoud van een veld. Dit betekent dat deze query's hoofdlettergevoelig zijn voor tekenreeksvelden.

U kunt zoekopdrachten en filters samen of afzonderlijk gebruiken. Als u zoekopdrachten en filters samen gebruikt, wordt het filter eerst op de gehele index toegepast. Vervolgens wordt de zoekopdracht uitgevoerd op het resultaat van het filter. Filters zijn dus nuttig om de resultaten van de zoekopdracht te verbeteren, doordat het aantal documenten dat moet worden doorzocht, wordt verminderd.

De syntaxis voor filterexpressies is een subset van de [OData-filtertaal](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search). Voor zoekquery's kunt u de [vereenvoudigde syntaxis](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) of de [Lucene-querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search) gebruiken. Deze worden hieronder behandeld.

### <a name="simple-query-syntax"></a>Vereenvoudigde querysyntaxis
De [vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) is de standaardtaal voor een query in Azure Search. De vereenvoudigde querysyntaxis ondersteunt een aantal algemene zoekoperators, zoals de operators EN, OF, NIET, woordgroep, achtervoegsel en prioriteit.

### <a name="lucene-query-syntax"></a>Lucene-querysyntaxis
Met de [Lucene-querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search) kunt u algemene en expressieve querytaal gebruiken die ontwikkeld is als onderdeel van [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html).

Met deze querysyntaxis beschikt u over de volgende mogelijkheden: [veldgerelateerde query's](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fields), [fuzzy zoeken](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fuzzy), [zoeken bij benadering](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_proximity), [versterking van termen](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_termboost), [zoeken op reguliere expressies](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_regex), [zoeken met jokertekens](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_wildcard), [syntaxisgrondbeginselen](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_syntax) en [query's waarbij gebruikt wordt gemaakt van Booleaanse operators](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_boolean).

## <a name="ordering-results"></a>Resultaten ordenen
De resultaten van een zoekopdracht kunnen door Azure Search worden geordend op de waarde in een bepaald veld. Standaard ordent Azure Search zoekresultaten op basis van de positie van de zoekscore van elk document, die is afgeleid van [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).

Als u de zoekresultaten op een andere waarde wilt ordenen, kunt u de `orderby`-zoekparameter gebruiken. U kunt opgeven dat in de waarde van de `orderby`-parameter de veldnamen en aanroepen naar de [`geo.distance()`-functie](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search) voor georuimtelijke waarden worden opgenomen. Elke expressie kan worden gevolgd door `asc` om aan te geven dat de resultaten zijn aangevraagd in oplopende volgorde of door `desc` om aan te geven dat de resultaten in aflopende volgorde worden aangevraagd. De standaard oplopende volgorde.

## <a name="paging"></a>Zoekresultaten oproepen
Met Azure Search kunt u gemakkelijk zoekresultaten oproepen. Met behulp van de `top`- en `skip`-parameter kunt u eenvoudig zoekaanvragen uitgeven waarmee u alle zoekresultaten in een beheersbare, geordende subset kunt weergeven om een goede zoekopdracht uit te voeren. Tijdens het ontvangen van deze kleinere subsets van resultaten kunt u ook het aantal documenten weergeven in het totale aantal zoekresultaten.

Zie het artikel [Zoekresultaten oproepen in Azure Search](search-pagination-page-layout.md) voor meer informatie over het oproepen van zoekresultaten.

## <a name="hit-highlighting"></a>Markeren
U kunt in Azure Search eenvoudig het exacte aantal zoekresultaten weergeven met behulp van de `highlight`-, `highlightPreTag`- en `highlightPostTag`-parameters. U kunt aangeven in welke *doorzoekbare* velden de tekst moet worden benadrukt. Ook kunt u de exacte tekenreekslabels opgeven die moeten worden toegevoegd aan het begin en einde van de overeenkomstige tekst die Azure Search retourneert.

## <a name="try-out-query-syntax"></a>Querysyntaxis uitproberen

De beste manier om syntaxisverschillen te begrijpen, is door query's uit te voeren en de resultaten te bekijken.

+ Gebruik [Search Explorer](search-explorer.md) in Azure Portal. Door het implementeren van [de voorbeeldindex](search-get-started-portal.md) kunt u de index binnen een paar bevragen met hulpprogramma's in de portal.

+ Gebruik [Fiddler](search-fiddler.md) of Chrome Postman om query's te verzenden naar een index die u hebt geüpload naar uw zoekservice. Beide hulpprogramma's ondersteunen REST-aanroepen naar een HTTP-eindpunt. 