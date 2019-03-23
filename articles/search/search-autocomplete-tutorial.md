---
title: Voorbeeld van automatisch aanvullen voor het typeahead toevoegen aan een zoekvak - Azure Search
description: Typeahead Queryacties in Azure Search inschakelen door het maken van suggesties en formuleren van aanvragen die in een zoekvak met voltooide termen of zinnen vullen.
manager: pablocas
author: mrcarter8
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: mcarter
ms.custom: seodec2018
ms.openlocfilehash: b78fdf0c493e4631e4cdd7e26b154570b6226d1f
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369567"
---
# <a name="example-add-autocomplete-to-partial-term-inputs-in-azure-search"></a>Voorbeeld: Automatisch aanvullen voor gedeeltelijke term invoer in Azure Search toevoegen

Deze preview-functie 'is voltooid' een gedeeltelijke term invoer door het opgeven van een voltooide term uit documenten in een Azure Search-index. Mogelijk hebt u deze mogelijkheid in commerciële zoekmachines opgemerkt. U kunt nu deze functie momenteel in openbare preview, toevoegen aan een Azure Search-oplossing voor het vereenvoudigen van het opnemen van een query.

In dit voorbeeld leert u hoe u [suggesties](https://docs.microsoft.com/rest/api/searchservice/suggestions), [automatisch aanvullen](https://docs.microsoft.com/rest/api/searchservice/autocomplete) en [facetten](search-faceted-navigation.md) in de [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/) en [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions?view=azure-dotnet) aan het bouwen van een krachtige zoekvak. 

+ *Suggesties* biedt aanbevelingen van werkelijke resultaten op basis van wat de gebruiker tot dusverre heeft getypt. 
+ *Automatisch aanvullen* is [een nieuwe preview-functie](search-api-preview.md) in Azure Search die termen aanbiedt die afkomstig zijn uit de index, om aan te vullen wat de gebruiker op dat moment typt. 

Er worden meerdere technieken met elkaar vergeleken die de productiviteit van gebruikers verbeteren door de geavanceerde functionaliteit van Search rechtstreeks ter beschikking te stellen aan gebruikers terwijl ze typen.

In dit voorbeeld ziet u hoe een ASP.NET MVC-toepassing die gebruikmaakt van C# om aan te roepen de [Azure Search .NET-clientbibliotheken](https://aka.ms/search-sdk), en JavaScript, kunnen de Azure Search REST-API rechtstreeks aanroepen. De toepassing voor dit voorbeeld is gericht op een index ingevuld de [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) sample van gegevens. U kunt de index gebruiken die eerder al is geconfigureerd in de demoversie van NYC Jobs, of u kunt uw eigen index van gegevens voorzien met behulp van een gegevenslader in de voorbeeldtoepassing NYCJobs. In het voorbeeld worden de [jQuery UI](https://jqueryui.com/autocomplete/) en [XDSoft](https://xdsoft.net/jqplugins/autocomplete/) JavaScript-bibliotheken gebruikt om een zoekvak te bouwen dat ondersteuning biedt voor automatisch aanvullen. Door deze onderdelen in combinatie met Azure Search te gebruiken, krijgt u meerdere voorbeelden te zien van hoe u ervoor kunt zorgen dat er via aangevulde suggesties in uw zoekbalk ondersteuning wordt geboden voor automatisch aanvullen.

U gaat de volgende taken uitvoeren:

> [!div class="checklist"]
> * De oplossing downloaden en configureren
> * Informatie over de zoekservice toevoegen aan toepassingsinstellingen
> * Een invoervak voor de zoekfunctie implementeren
> * Ondersteuning toevoegen voor een lijst voor automatisch aanvullen die gegevens uit een externe bron ophaalt 
> * Ophalen van suggesties en automatisch aanvullen met behulp van de SDK voor .NET en REST-API
> * Ondersteuning bieden voor caching aan clientzijde om de prestaties te verbeteren 

## <a name="prerequisites"></a>Vereisten

* Visual Studio 2017. U kunt de gratis [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) gebruiken. 

* Het voorbeeld downloaden [broncode](https://github.com/azure-samples/search-dotnet-getting-started) voor het voorbeeld.

* Een actief Azure-account en een Azure Search-service. (optioneel) Als u nog geen Azure-account hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/free/). Zie [Een zoekservice maken](search-create-service-portal.md) voor hulp bij het inrichten van de service. Het account en de service zijn optioneel, omdat in dit voorbeeld kan worden voltooid met een gehoste NYCJobs index al in plaats van een andere demo.

* Download de voorbeeldcode [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) om de gegevens van NYCJobs te importeren in een index van uw eigen Azure Search-service. (optioneel)

> [!Note]
> Als u de gratis Azure Search-service gebruikt, bent u beperkt tot drie indexen. De gegevenslader NYCJobs maakt twee indexen. Zorg ervoor dat uw service voldoende ruimte heeft voor de nieuwe indexen.

### <a name="set-up-azure-search-optional"></a>Azure Search instellen (optioneel)

Volg de stappen in deze sectie als u de gegevens voor de voorbeeldtoepassing NYCJobs in uw eigen index wilt importeren. Deze stap is optioneel.  Als u de meegeleverde voorbeeldindex wilt gebruiken, kunt u verdergaan met de volgende sectie waarin het voorbeeld wordt uitgevoerd.

1. Open in Visual Studio het oplossingsbestand DataLoader.sln in de map DataLoader van de NYCJobs-voorbeeldcode.

1. Werk de verbindingsgegevens voor uw Azure Search-service bij.  Open App.config in het project DataLoader en wijzig de app-instellingen TargetSearchServiceName en TargetSearchServiceApiKey zodat deze verwijzen naar uw Azure Search-service en Azure Search Service API-sleutel.  Deze vindt u in Azure Portal.

1. Druk op F5 om de toepassing starten.  Nu worden twee indexen gemaakt en de NYCJob-voorbeeldgegevens geïmporteerd.

1. Open het oplossingsbestand AutocompleteTutorial.sln in Visual Studio in de voorbeeldcode voorbeeld.  Open Web.config in het project AutocompleteTutorial en wijzig de waarden SearchServiceName en SearchServiceApiKey in dezelfde waarden als hierboven.

### <a name="running-the-sample"></a>Het voorbeeld uitvoeren

U bent nu klaar om uit te voeren van de voorbeeld-voorbeeldtoepassing.  Open het oplossingsbestand AutocompleteTutorial.sln in Visual Studio om uit te voeren in het voorbeeld.  De oplossing bevat een ASP.NET MVC-project.  Druk op F5 om het project uit te voeren en de pagina in de door u gewenste browser te laden.  Bovenaan ziet u een optie voor het selecteren van C# of JavaScript.  De C# optie aanroepen naar de HomeController vanuit de browser en de Azure Search .NET SDK gebruikt voor het ophalen van resultaten.  Met de JavaScript-optie wordt de Azure Search REST-API rechtstreeks vanuit de browser aangeroepen.  Met deze optie worden doorgaans opmerkelijk betere prestaties gehaald, omdat de controller ertussenuit wordt gehaald.  U kunt de optie kiezen die bij uw behoeften en taalvoorkeuren past.  Er bevinden zich diverse voorbeelden voor automatisch aanvullen op de pagina, met enkele richtlijnen voor elk voorbeeld.  Elk voorbeeld heeft een aanbevolen voorbeeldtekst die u kunt proberen.  Typ een paar letters in elk zoekvak om te zien wat er gebeurt.

## <a name="how-this-works-in-code"></a>Hoe dit met code werkt

Nu dat u de voorbeelden in de browser in werking hebt gezien, wordt stapsgewijs en in detail het eerste voorbeeld bekeken om te zien welke onderdelen hierbij een rol spelen en welke rol dit is.

### <a name="search-box"></a>Zoekvak

Voor elke taal is het zoekvak precies hetzelfde.  Open het bestand Index.cshtml in de map \Views\Home. Het zoekvak zelf is eenvoudig:

```html
<input class="searchBox" type="text" id="example1a" placeholder="search">
```

Dit is een eenvoudige invoertekstvak met een klasse voor stijl, een ID worden verwezen door JavaScript, en de tijdelijke aanduiding voor tekst.  Het geheim schuilt in het JavaScript.

### <a name="javascript-code-c"></a>JavaScript-code (C#)

In het voorbeeld in C# wordt van JavaScript in Index.cshtml gebruikgemaakt om de bibliotheek voor automatisch aanvullen van jQuery UI te kunnen gebruiken.  Deze bibliotheek voegt de ervaring voor automatisch aanvullen toe aan het zoekvak door asynchrone aanroepen naar de MVC-controller te verzenden om zo aanbevelingen op te halen.  Kijk eens naar de JavaScript-code uit het eerste voorbeeld:

```javascript
$(function () {
    $("#example1a").autocomplete({
        source: "/home/suggest?highlights=false&fuzzy=false&",
        minLength: 3,
        position: {
            my: "left top",
            at: "left-23 bottom+10"
        }
    });
});
```

Deze code wordt tijdens het laden van een pagina in de browser uitgevoerd om automatisch aanvullen te configureren voor het invoervak 'example1a'.  `minLength: 3` zorgt ervoor dat aanbevelingen alleen worden weergegeven wanneer er zich ten minste drie tekens in het zoekvak bevinden.  De bronwaarde is belangrijk:

```javascript
source: "/home/suggest?highlights=false&fuzzy=false&",
```

Met deze regel wordt aan de API voor automatisch aanvullen doorgegeven waar de lijst met items moet worden opgehaald die onder het zoekvak moet worden weergegeven.  Omdat dit een MVC-project is, wordt de functie Voorstellen in HomeController.cs aangeroepen.  Dit komt in de volgende sectie weer aan de orde.  Er worden ook een aantal parameters doorgegeven voor het beheer van markeringen, zoeken bij benadering en terminologie.  Door de JavaScript-API voor automatisch aanvullen wordt de parameter 'term' toegevoegd.

#### <a name="extending-the-sample-to-support-fuzzy-matching"></a>Het voorbeeld uitbreiden om ondersteuning te bieden voor zoeken bij benadering

Zoeken bij benadering zorgt ervoor dat u resultaten krijgt te zien op basis van treffers bij benadering, zelfs als de gebruiker een woord in het zoekvak onjuist heeft gespeld.  U gaat dit uitproberen door de bronregel zo te wijzigen dat zoeken bij benadering mogelijk is.

Wijzig de volgende regel van dit:

```javascript
source: "/home/suggest?highlights=false&fuzzy=false&",
```

in dit:

```javascript
source: "/home/suggest?highlights=false&fuzzy=true&",
```

Start de toepassing door op F5 te drukken.

Typ een woord als 'exclasief' en u ziet hoe resultaten worden weergegeven voor 'exclusief', zelfs als deze niet exact overeenkomen met de letters die u hebt getypt.

### <a name="homecontrollercs-c"></a>HomeController.cs (C#)

Nu u de JavaScript-code voor het voorbeeld hebt bekeken, gaat u de code van de C#-controller bekijken, waarmee de aanbevelingen daadwerkelijk worden opgehaald met behulp van de Azure Search .NET SDK.

1. Open het bestand HomeController.cs in de map Controllers. 

1. Het eerste wat u mogelijk zal opvallen is een methode met de naam InitSearch die zich boven in de klasse bevindt.  Hiermee wordt een geverifieerde HTTP-indexclient voor de Azure Search-service gemaakt.  Als u meer informatie wilt over hoe dit werkt, gaat u naar het volgende voorbeeld: [Weten hoe u Azure Search gebruikt via een .NET-toepassing](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

1. Ga naar de functie Voorstellen.

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

    DocumentSuggestResult resp = _indexClient.Documents.Suggest(term, "sg", sp);

    // Convert the suggest query results to a list that can be displayed in the client.
    List<string> suggestions = resp.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = suggestions
    };
}
```

De functie Voorstellen maakt gebruik van twee parameters die bepalen of er bij het invoeren van de zoekterm markeringen voor treffers worden geretourneerd of zoeken bij benadering wordt gebruikt.  Met de methode wordt een object SuggestParameters gemaakt, dat vervolgens aan de Voorstellen-API wordt doorgegeven. Het resultaat wordt vervolgens geconverteerd naar JSON, zodat deze in de client kan worden weergegeven.
Voeg een onderbrekingspunt toe aan het begin van de functie Voorstellen en doorloop de code. (optioneel)  Let op de reactie die door de SDK is geretourneerd en hoe deze is geconverteerd naar het resultaat dat door de methode is geretourneerd.

In de andere voorbeelden op de pagina wordt hetzelfde patroon gevolgd om de markering van treffers, aanbevelingen voor automatisch aanvullen bij typen en facetten toe te voegen ter ondersteuning van caching aan clientzijde van de resultaten voor automatisch aanvullen.  Loop door elk van deze voorbeelden heen zodat u begrijpt hoe ze werken en hoe u ze kunt gebruiken in uw zoekervaring.

### <a name="javascript-language-example"></a>Voorbeeld van de JavaScript-taal

In het voorbeeld van de taal JavaScript wordt door de JavaScript-code op de pagina IndexJavaScript.cshtml gebruikgemaakt van jQuery UI Autocomplete.  Dit is een bibliotheek waarmee het grootste deel van zware werk wordt gedaan dat nodig is om een goed ogend zoekvak te kunnen weergeven, en die het mogelijk maakt om eenvoudig asynchrone aanroepen naar Azure Search uit te voeren om aanbevelingen op te halen.  Kijk eens naar de JavaScript-code uit het eerste voorbeeld:

```javascript
$(function () {
    $("#example1a").autocomplete({
        source: function (request, response) {
        $.ajax({
            type: "POST",
            url: suggestUri,
            dataType: "json",
            headers: {
                "api-key": searchServiceApiKey,
                "Content-Type": "application/json"
            },
            data: JSON.stringify({
                top: 5,
                fuzzy: false,
                suggesterName: "sg",
                search: request.term
            }),
                success: function (data) {
                    if (data.value && data.value.length > 0) {
                        response(data.value.map(x => x["@@search.text"]));
                    }
                }
            });
        },
        minLength: 3,
        position: {
            my: "left top",
            at: "left-23 bottom+10"
        }
    });
});
```

Als u dit vergelijkt met het bovenstaande voorbeeld waarbij de begincontroller wordt aangeroepen, ziet u verschillende overeenkomsten.  De configuratie voor automatisch aanvullen voor `minLength` en `position` zijn identiek.  De belangrijke wijziging hier is de bron.  In plaats van de aanbevolen methode aanroept in de oorspronkelijke domeincontroller, een REST-aanvraag in een JavaScript-functie wordt gemaakt en uitgevoerd met behulp van Ajax.  De reactie wordt vervolgens verwerkt in 'geslaagd' en als bron gebruikt.

## <a name="takeaways"></a>Opgedane kennis

In dit voorbeeld ziet u de basisstappen voor het bouwen van een zoekvak die ondersteuning biedt voor automatisch aanvullen en suggesties.  Hebt u gezien hoe u kunt een ASP.NET MVC-toepassing bouwen en de Azure Search .NET SDK of REST-API gebruiken voor het ophalen van suggesties.

## <a name="next-steps"></a>Volgende stappen

Suggesties en automatisch aanvullen in uw zoekervaring integreren.  Houd rekening met hoe u met behulp van de .NET SDK of de REST-API rechtstreeks kunt vele mogelijkheden van Azure Search bij uw gebruikers, terwijl ze typt zodat ze nog productiever.

> [!div class="nextstepaction"]
> [Autocomplete REST API](https://docs.microsoft.com/rest/api/searchservice/autocomplete)
> [Suggestions REST API ](https://docs.microsoft.com/rest/api/searchservice/suggestions)
> [Facets index attribute on a Create Index REST API](https://docs.microsoft.com/rest/api/searchservice/create-index) (Indexattribuut Facetten in een Create Index REST API)

