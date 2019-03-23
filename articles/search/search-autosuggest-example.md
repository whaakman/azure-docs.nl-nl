---
title: Automatische suggestie-voorbeeld voor het vervolgkeuzelijst voorwaarden toevoegen aan een zoekvak - Azure Search
description: Voorgestelde query invoer toevoegen door te maken van suggesties en formuleren van aanvragen die aanroepen van de voorgestelde query's in Azure Search.
manager: cgronlun
author: heidisteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: heidist
ms.openlocfilehash: 1db085d61c60d303aaff5c3b0d16998805fcda90
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58373073"
---
# <a name="example-add-autosuggest-for-dropdown-query-selections"></a>Voorbeeld: Voeg Automatische suggestie voor vervolgkeuzelijst queryselecties

Term zoekinvoergegevens kunnen een vervolgkeuzelijst met voorgestelde querytermen bevatten. U hebt deze mogelijkheid in commerciële zoekmachines gezien en u kunt een vergelijkbare ervaring implementeren in Azure Search met behulp van een [suggestie constructie](index-add-suggesters.md) en een bewerking suggesties voor een queryaanvraag. Dit artikel wordt gebruikgemaakt van voorbeelden ter illustratie van de aanmaak van een automatische suggesties-query met behulp van een suggestie die u al hebt gedefinieerd. 

## <a name="rest-api"></a>REST-API

U kunt [Postman](search-fiddler.md) of [PowerShell](search-create-index-rest-api.md) en de [REST-API](https://docs.microsoft.com/rest/api/searchservice/) voor het uitproberen van dit voorbeeld, maar de resultaten worden geretourneerd als JSON-documenten. Een realistische en meer visuele ervaring kan worden gevonden met de [voorbeeldcode voor automatisch aanvullen](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete).

### <a name="1---index-with-a-suggester-construct"></a>1 - index met een suggestie constructie

Automatische suggestie begint met een [suggestie constructie](index-add-suggesters.md) toegevoegd aan een index, die bestaat uit de velden die bijdragen van de waarden van de vervolgkeuzelijst. Opgegeven in het volgende schema, zal voorgestelde query's worden geformuleerd met behulp van waarden uit de velden hotelName en categorie.

Ervan uitgaande dat de minimale voorbeeldgegevenssets gevonden in de QuickStart, hotel namen bevatten "Decoratieve blijven" en 'Roach motel' en categorieën bevatten 'Luxe' en 'Budget'.

Als u de index hotels al hebt, moet u deze verwijderen en opnieuw maken met behulp van de volgende schema. Dit schema wordt toegevoegd een suggestie. Vergeet niet om de gegevens ook opnieuw laden.

```json
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ],
  "suggesters": [
    {
      "name": "sg",
      "searchMode": "analyzingInfixMatching",
      "sourceFields": ["hotelName", "category"]
    }
  ]
}

```

### <a name="2---query-with-suggestions-operator"></a>2 - query's uitvoeren met suggesties operator

Voor het gebruik van een suggestie en aanroepen autosuggest, moet u verzenden een [suggesties API](https://docs.microsoft.com/rest/api/searchservice/suggestions) vraag via GET of POST. Op verzoek scant de search-service voor mogelijke overeenkomsten nadat de eerste drie tekens worden ontvangen. 

Stel in de aanvraagheader **api-sleutel** aan een beheerder of query-sleutel en **Content-Type** naar application/json. 

```http
GET https://mydemo.search.windows.net/indexes/hotels/docs/suggest?search=fan&&suggesterName=sg
```

De aanvraag scant alle velden opgenomen in de suggestie (hotelName en categorie), het volgende antwoord geretourneerd:

```
{
    "@odata.context": "https://mydemo.search.windows.net/indexes('hotels')/$metadata#docs(*)",
    "value": [
        {
            "@search.text": "Fancy Stay",
            "hotelId": "1"
        }
    ]
}
```

Met het oog op het verwachte resultaat zou uw clientcode de resultaten weergegeven als de vervolgkeuzelijst onder een zoekbalk.

## <a name="net-sdk-c"></a>.NET-SDK (C#)

### <a name="1---index-with-a-suggester-construct"></a>1 - index met een suggestie constructie

In de .NET SDK, gebruikt u een [suggestie klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet). Suggestie is een verzameling, maar duurt slechts één item.

```csharp
private static void CreateHotelsIndex(SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = "hotels",
        Fields = FieldBuilder.BuildForType<Hotel>(),
        Suggesters = new List<Suggester>() {new Suggester()
            {
                Name = "sg",
                SourceFields = new string[] { "HotelId", "Category" }
            }}
    };

    serviceClient.Indexes.Create(definition);

}
```

### <a name="2---query-with-suggest-method"></a>2 - query's uitvoeren met de methode voorstellen

De [DocumentsOperationsExtensions.Suggest methode](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.suggest?view=azure-dotnet) wordt gebruikt om terug te keren voorgestelde query-tekenreeks.

```csharp
public ActionResult Suggest(bool highlights, bool fuzzy, string term)
{
    InitSearch();

    // Call suggest API and return results
    SuggestParameters sp = new SuggestParameters()
    {
        UseFuzzyMatching = fuzzy,
        Top = 5
    };

    if (highlights)
    {
        sp.HighlightPreTag = "<b>";
        sp.HighlightPostTag = "</b>";
    }

    DocumentSuggestResult suggestResult = _indexClient.Documents.Suggest(term, "sg",sp);

    // Convert the suggest query results to a list that can be displayed in the client.
    List<string> suggestions = suggestResult.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = suggestions
    };
}
```

## <a name="see-also"></a>Zie ook

+ [REST-API met behulp van Postman verkennen](search-fiddler.md)
+ [Voorbeeld: Automatisch aanvullen](search-autocomplete-tutorial.md)
+ [Suggesties aan een index toevoegen](index-add-suggesters.md)
+ [Voeg een suggesties-klasse met behulp van .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet)
+ [Aanroepen van suggesties met behulp van GET of POST (REST-API)](https://docs.microsoft.com/rest/api/searchservice/suggestions)
+ [Suggesties met behulp van SuggestWithHttpMessagesAsync (.NET) aanroepen](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet-preview) of 