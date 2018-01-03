---
title: Taal-filters in Azure Search | Microsoft Docs
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
ms.date: 10/23/2017
ms.author: heidist
ms.openlocfilehash: 2c09de74405394d4c385dbbd0535913cf2488744
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="how-to-filter-by-language-in-azure-search"></a>Hoe kunt u filteren op taal in Azure Search 

Een sleutel vereiste in een meertalige zoektoepassing is de mogelijkheid om te zoeken en ophalen van resultaten in de taal van gebruiker. In Azure Search is een manier om te voldoen aan de taalvereisten van een meertalige app voor het maken van een reeks velden die zijn toegewezen voor het opslaan van tekenreeksen in een bepaalde taal en vervolgens beperken tot alleen de velden zoeken in volledige tekst op het moment dat de query.

Queryparameters in de aanvraag worden gebruikt voor zowel het bereik van de zoekopdracht en vervolgens trim de resultaten van de velden die niet compatibel met de zoekfunctie die u wilt leveren van inhoud bieden.

| Parameters | Doel |
|-----------|--------------|
| **searchFields** | Limieten voor volledige tekst zoeken aan de lijst met benoemde velden. |
| **$select** | Verwijdert het antwoord zodanig dat alleen de velden die u opgeeft. Standaard zijn alle ophalen mogelijk velden worden geretourneerd. De **$select** parameter kunt u kiezen welke u wilt retourneren. |

Het succes van deze techniek staat of de integriteit van de inhoud van veld valt. Azure Search niet omzetten van tekenreeksen of taal detectieproces. Het is aan u om ervoor te zorgen dat velden bevatten de tekenreeksen die u verwacht.

## <a name="define-fields-for-content-in-different-languages"></a>Velden voor inhoud in verschillende talen definiëren

Query's in Azure Search gericht op een enkele index. Ontwikkelaars die wilt bieden taalspecifieke tekenreeksen in een enkel zoekfunctie meestal specifieke velden voor het opslaan van de waarden definiëren: één veld voor Engels tekenreeksen, één voor Frans, enzovoort. 

In onze voorbeelden, met inbegrip van de [vastgoed voorbeeld](search-get-started-portal.md) hieronder ziet u mogelijk hebt gezien velddefinities vergelijkbaar met de volgende schermafbeelding. U ziet hoe dit voorbeeld ziet u de taal analyzer toewijzingen voor de velden in de index. Velden met tekenreeksen sneller zoekopdracht in volledige tekst wanneer gekoppeld aan een analyzer ontworpen voor het afhandelen van de taalkundige regels van de doel-taal.

  ![](./media/search-filters-language/lang-fields.png)

> [!Note]
> Zie voor voorbeelden van programmacode met velddefinities met talen analyzers [(.NET) van een index definiëren](https://docs.microsoft.com/azure/search/search-create-index-dotnet#define-your-azure-search-index) en [(REST) van een index definiëren](https://docs.microsoft.com/azure/search/search-create-index-rest-api#define-your-azure-search-index-using-well-formed-json).

## <a name="build-and-load-an-index"></a>Opbouwen en laden van een index

Een stap tussenliggende (en mogelijk duidelijk) is die u hebt voor [bouwen en de index te vullen](https://docs.microsoft.com/azure/search/search-create-index-dotnet#create-the-index) voordat u een query formuleren. We deze stap voor de volledigheid worden vermeld. Een manier om te bepalen of de index beschikbaar is, is door het controleren van de lijst van de indexen de [portal](https://portal.azure.com).

## <a name="constrain-the-query-and-trim-results"></a>De query beperken en trim resultaten

Parameters voor de query worden gebruikt voor de zoekactie te beperken tot specifieke velden en vervolgens trim de resultaten van de velden die niet op uw scenario nuttig. Een doel van beperkingsfacet zoekopdracht gegeven aan velden met Franse tekenreeksen, gebruikt u **searchFields** toe te passen van de query op de velden met tekenreeksen in die taal. 

Een zoekopdracht retourneert standaard alle velden die zijn gemarkeerd als worden opgehaald. Als zodanig als u wilt uitsluiten van de velden die niet voldoen aan de taalspecifieke zoekervaring dat u wilt bieden. In het bijzonder als u zoeken naar een veld met Franse tekenreeksen beperkte, wilt u waarschijnlijk velden met Engelse tekenreeksen uitsluiten van de resultaten. Met behulp van de **$select** opvragen van de parameter-hebt u meer controle over welke velden u keert terug naar de aanroepende toepassing.

```csharp
parameters =
    new SearchParameters()
    {
        searchFields = "description_fr" 
        Select = new[] { "description_fr"  }
    };
```
> [!Note]
> Hoewel er no $filter argument van de query, wordt deze gebruiksvoorbeeld sterk gekoppeld aan filter concepten, zodat we als een filteren scenario geven.

## <a name="see-also"></a>Zie ook

+ [Filters in Azure Search](search-filters.md)
+ [Taalanalyse](https://docs.microsoft.com/rest/api/searchservice/language-support)
+ [Hoe vol tekst zoeken werkt in Azure Search](search-lucene-query-architecture.md)
+ [REST-API documenten zoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents)

