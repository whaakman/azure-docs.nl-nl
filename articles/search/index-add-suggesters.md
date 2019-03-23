---
title: Typeahead query's toevoegen aan een index - Azure Search
description: Automatisch aangevulde Queryacties in Azure Search inschakelen door het maken van suggesties en aanvragen die aanroepen van automatisch aanvullen of autosuggested querytermen formuleren.
ms.date: 03/22/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 877294e80d655ab75be78a5aa57854a03a5f267a
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370649"
---
# <a name="add-suggesters-to-an-index-for-typeahead-in-azure-search"></a>Suggesties toevoegen aan een index voor typeahead in Azure Search

Een **suggestie** is een constructie in een [Azure Search-index](search-what-is-an-index.md) die ondersteuning biedt voor een 'search-als-u-type'-ervaring. Het bevat een lijst met velden waarvoor u wilt om in te schakelen typeahead query invoer. Er zijn twee varianten van typeahead: [ *automatisch aanvullen* ](search-autocomplete-tutorial.md) is voltooid de term of woordgroep die u hebt getypt, [ *Automatische suggestie* ](search-autosuggest-example.md) biedt een korte lijst met termen of zinnen die u als een query invoeren selecteren kunt. U hebt deze problemen voordat in commerciële zoekmachines ongetwijfeld gezien.

![Visuele vergelijking van automatisch aanvullen en Automatische suggestie](./media/index-add-suggesters/visual-comparison-suggest-complete.png "visuele vergelijking van automatisch aanvullen en Automatische suggestie")

Voor het implementeren van deze problemen in Azure Search, is er een index en query-onderdeel. 

+ In een index, voegt u een suggestie toe. U kunt de portal, REST-API of .NET SDK te maken van een suggestie gebruiken. 

+ Geef een automatische suggesties- of automatisch aangevulde actie op een query. 

> [!Important]
> Automatisch aanvullen is momenteel in preview, beschikbaar in preview REST-API's en SDK voor .NET en niet ondersteund voor productie-Apps. 

Typeahead ondersteuning is ingeschakeld op basis van per veld. Als u een ervaring die vergelijkbaar is met de aangegeven in de schermafbeelding wilt, kunt u beide gedrag typeahead binnen de dezelfde search-oplossing implementeren. Het doel van beide aanvragen de *documenten* verzameling van specifieke index en -antwoorden worden geretourneerd nadat een gebruiker ten minste een invoerreeks drie tekens heeft geleverd.

## <a name="create-a-suggester"></a>Maken van een suggestie

Hoewel een suggestie verschillende eigenschappen heeft, is voornamelijk een verzameling van velden waarvoor u een typeahead ervaring wilt inschakelen. Bijvoorbeeld, een reis-app mogelijk wilt inschakelen typeahead zoeken op bestemmingen, plaatsen en bezienswaardigheden. Alle drie de velden zou daarom gaat u in de Veldenverzameling.

Voor het maken van een suggestie toevoegen aan een indexschema. U kunt een suggestie hebt in een index (met name een suggestie in de verzameling suggesties). 

In de REST-API, voegt u toe suggesties via [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) of [Index bijwerken](https://docs.microsoft.com/rest/api/searchservice/update-index). 

  ```json
  {
    "name": "hotels",
    "fields": [
      . . .
    ],
    "suggesters": [
      {
        "name": "sg",
        "searchMode": "analyzingInfixMatching",
        "sourceFields": ["hotelName", "category"]
      }
    ],
    "scoringProfiles": [
      . . .
    ]
  }
  ```

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

Sleutel verwijst naar een kennisgeving over suggesties is dat er is een naam (suggesties wordt verwezen door de naam op een aanvraag), een searchMode (momenteel slechts één, "analyzingInfixMatching") en de lijst met velden waarvoor typeahead is ingeschakeld. 

Nadat een suggestie is gemaakt, voegt u toe de [suggesties API](https://docs.microsoft.com/rest/api/searchservice/suggestions) in uw querylogica voor het aanroepen van de functie.

### <a name="property-reference"></a>Eigenschapverwijzing

Eigenschappen die een suggestie bepalen omvatten het volgende:

|Eigenschap      |Description      |
|--------------|-----------------|
|`name`        |De naam van de suggestie. U de naam van de suggestie gebruiken bij het aanroepen van de [suggesties REST-API](https://docs.microsoft.com/rest/api/searchservice/suggestions) of [automatisch aanvullen REST-API (preview)](https://docs.microsoft.com/rest/api/searchservice/autocomplete).|
|`searchMode`  |De strategie gebruikt om te zoeken naar kandidaat zinnen. De enige modus die momenteel niet ondersteund is `analyzingInfixMatching`, die wordt uitgevoerd met het flexibele overeenkomende woordgroepen aan het begin of in het midden van zinnen.|
|`sourceFields`|Een lijst met een of meer velden die de bron van de inhoud voor suggesties. Alleen de velden van het type `Edm.String` en `Collection(Edm.String)` mogelijk bronnen voor suggesties. Alleen velden waarvoor een aangepaste taalanalyse ingesteld niet kunnen worden gebruikt.<p/>Geef alleen de velden die voor een verwachte en het juiste antwoord lenen, of het is een voltooide tekenreeks in een zoekbalk of in een vervolgkeuzelijst weergegeven.<p/>De naam van een hotel is een goede kandidaat omdat er precisie. Er zijn uitgebreide velden, zoals beschrijvingen en opmerkingen te compacte. Op deze manier zijn terugkerende velden, zoals categorieën en -tags minder effectief. In de voorbeelden opnemen we 'categorie' toch om aan te tonen dat u meerdere velden kunt opnemen. |

### <a name="index-rebuilds-for-existing-fields"></a>Index wordt opnieuw gemaakt voor bestaande velden

Suggesties velden bevatten, en als u een suggestie toe te voegen aan een bestaande index of de samenstelling van het veld wijzigt, heeft u waarschijnlijk de index opnieuw opbouwen.

| Bewerking | Impact |
|--------|--------|
| Nieuwe velden maken en maak een nieuwe suggestie tegelijkertijd in dezelfde update | Minimale impact. Als een index velden eerder hebt toegevoegd bevat, heeft het toevoegen van nieuwe velden en een nieuwe suggestie geen invloed op bestaande velden. |
| Bestaande velden toevoegen aan een suggestie | Hoge impact. Toevoegen van een veld de definitie van het veld wijzigt, blijkt een [volledig opnieuw opbouwen](search-howto-reindex.md).|

## <a name="use-a-suggester"></a>Gebruik een suggestie

Zoals eerder vermeld, kunt u een suggestie voor automatische suggesties, automatisch aanvullen, of beide. 

Een suggestie wordt verwezen in de aanvraag, samen met de bewerking. Geef bijvoorbeeld op een GET REST-aanroep, een `suggest` of `autocomplete` op de verzameling documenten. Rest, nadat een suggestie is gemaakt, gebruiken de [suggesties API](https://docs.microsoft.com/rest/api/searchservice/suggestions) of de [automatisch aanvullen API (preview)](https://docs.microsoft.com/rest/api/searchservice/autocomplete) in uw querylogica.

Voor .NET, gebruiken [SuggestWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet-preview) of [AutocompleteWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.autocompletewithhttpmessagesasync?view=azure-dotnet-preview&viewFallbackFrom=azure-dotnet).

Voorbeelden demonstreren van elke aanvraag:

+ [Voeg Automatische suggestie voor vervolgkeuzelijst queryselecties](search-autosuggest-example.md)

+ [Automatisch aanvullen toevoegen aan gedeeltelijke term invoer in Azure Search](search-autocomplete-tutorial.md) (in Preview-versie) 

## <a name="sample-code"></a>Voorbeeldcode

De [DotNetHowToAutocomplete](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete) voorbeeld bevat zowel C# en Java-code, en ziet u een suggestie bouw, autosuggest, automatisch aanvullen en facet navigatie. 

Het maakt gebruik van een sandbox met Azure Search-service en een index vooraf geladen, zodat u alle hoeft te doen is druk op F5 uit te voeren. Er is geen abonnement of aanmelding nodig.

## <a name="next-steps"></a>Volgende stappen

Bekijk de volgende voorbeelden om te zien hoe de aanvragen worden geformuleerd:

+ [Voorbeeld van autosuggested](search-autosuggest-example.md) 
+ [Voorbeeld van Autocompleted (preview)](search-autocomplete-tutorial.md) 
