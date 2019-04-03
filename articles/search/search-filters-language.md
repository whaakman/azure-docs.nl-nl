---
title: Taalfilters voor meerdere talen inhoud van een zoekindex - Azure Search
description: Filtercriteria ter ondersteuning van meerdere taal zoeken, scoping queryuitvoering taalspecifieke velden.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.workload: search
ms.topic: conceptual
ms.date: 10/23/2017
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 695fdfba1573ff97b05f8e8b50a05bef9dbf48de
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58846162"
---
# <a name="how-to-filter-by-language-in-azure-search"></a>Hoe kunt u filteren op taal in Azure Search 

Een basisvereiste in een zoektoepassing meertalig is de mogelijkheid om te zoeken en ophalen van resultaten in de taal van gebruiker. In Azure Search is een manier om te voldoen aan de taalvereisten van een multilingual app voor het maken van een reeks velden die zijn toegewezen voor het opslaan van tekenreeksen in een bepaalde taal en vervolgens zoeken in volledige tekst om alleen de velden te beperken bij het uitvoeren van query's.

Queryparameters op de aanvraag worden gebruikt voor zowel het bereik van de search-bewerking en verwijder vervolgens de resultaten van de velden die niet compatibel is met de zoekervaring die u wilt leveren inhoud bieden.

| Parameters | Doel |
|-----------|--------------|
| **searchFields** | Limieten voor volledige tekst zoeken aan de lijst met benoemde velden. |
| **$select** | Hiermee wist het antwoord zodanig dat alleen de velden die u opgeeft. Standaard zijn alle Ophaalbare velden worden geretourneerd. De **$select** parameter kunt u kiezen welke om terug te keren. |

Het succes van deze techniek afhangt van de integriteit van de inhoud van de velden. Azure Search niet omzetten van tekenreeksen of taaldetectie wordt uitgevoerd. Het is aan u om ervoor te zorgen dat velden bevatten de tekenreeksen die u verwacht.

## <a name="define-fields-for-content-in-different-languages"></a>Velden voor de inhoud in verschillende talen definiëren

Query's in Azure Search gericht op één index. Ontwikkelaars die willen taalspecifieke tekenreeksen in een enkele zoekervaring doorgaans bieden speciale velden voor het opslaan van de waarden definiëren: één veld voor Engels tekenreeksen, één voor Frans, enzovoort. 

In onze voorbeelden, met inbegrip van de [onroerend goed-voorbeeld](search-get-started-portal.md) hieronder wordt weergegeven, u mogelijk hebt gezien velddefinities die vergelijkbaar is met de volgende schermafbeelding. U ziet hoe dit voorbeeld ziet u de taal analyzer toewijzingen voor de velden in de index. Velden met tekenreeksen uitvoeren beter in zoeken in volledige tekst in combinatie met speciaal ontworpen voor het afhandelen van de linguïstische regels van de doeltaal analyzer.

  ![](./media/search-filters-language/lang-fields.png)

> [!Note]
> Zie voor voorbeelden van code met velddefinities met talen analysefuncties, [definiëren van een index (.NET)](https://docs.microsoft.com/azure/search/search-create-index-dotnet) en [definiëren van een index (REST)](search-create-index-rest-api.md).

## <a name="build-and-load-an-index"></a>Bouw en een index laden

Een stap tussenliggende (en mogelijk voor de hand liggende) is die u hebt voor [bouwen en de index te vullen](https://docs.microsoft.com/azure/search/search-create-index-dotnet) voordat u een query te formuleren. We deze stap voor de volledigheid worden vermeld. Een manier om te bepalen of de index beschikbaar is, is door het controleren van de lijst indexen de [portal](https://portal.azure.com).

## <a name="constrain-the-query-and-trim-results"></a>Beperken van de query en trim resultaten

Parameters voor de query worden gebruikt om de zoekopdracht beperken tot specifieke velden en verwijder vervolgens de resultaten van de velden niet nuttig is voor uw scenario. Een doel van de zoekopdracht beperken gegeven aan velden met Franse tekenreeksen, zou u ook **searchFields** om de query op de velden met tekenreeksen in die taal. 

Een zoekopdracht retourneert standaard alle velden die zijn gemarkeerd als ophaalbaar. Mogelijk wilt u als zodanig velden die niet voldoen aan de taalspecifieke zoekopdrachten ervaring die u wilt uitsluiten. Met name als u zoeken naar een veld met de Franse tekenreeksen beperkt, u waarschijnlijk velden met tekenreeksen met de Engelse uitsluiten van de resultaten. Met behulp van de **$select** query van de parameter-hebt u meer controle over welke velden worden geretourneerd naar de aanroepende toepassing.

```csharp
parameters =
    new SearchParameters()
    {
        searchFields = "description_fr" 
        Select = new[] { "description_fr"  }
    };
```
> [!Note]
> Hoewel er no $filter argument voor de query, is ten zeerste deze use case gekoppeld aan filter concepten, zodat we deze als een scenario voor het filteren.

## <a name="see-also"></a>Zie ook

+ [Filters in Azure Search](search-filters.md)
+ [Taalanalyse](https://docs.microsoft.com/rest/api/searchservice/language-support)
+ [Hoe vol tekstzoekopdrachten werkt in Azure Search](search-lucene-query-architecture.md)
+ [REST-API voor Search-documenten](https://docs.microsoft.com/rest/api/searchservice/search-documents)

