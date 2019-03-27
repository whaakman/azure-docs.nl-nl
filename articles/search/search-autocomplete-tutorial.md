---
title: Suggesties en automatisch aanvullen in een zoekvak - Azure Search toevoegen
description: Typeahead Queryacties in Azure Search inschakelen door het maken van suggesties en formuleren van aanvragen die in een zoekvak met voltooide termen of zinnen vullen.
manager: pablocas
author: mrcarter8
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: mcarter
ms.custom: seodec2018
ms.openlocfilehash: 9fb3cdd4b4b809e45180cd95b8fe930cce86826e
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58498805"
---
# <a name="example-add-suggestions-or-autocomplete-to-your-azure-search-application"></a>Voorbeeld: Suggesties of automatisch aangevulde toevoegen aan uw Azure Search-toepassing

In dit voorbeeld wordt informatie over het gebruik [suggesties](https://docs.microsoft.com/rest/api/searchservice/suggestions) en [automatisch aanvullen](https://docs.microsoft.com/rest/api/searchservice/autocomplete) aan het bouwen van een krachtige zoekvak die ondersteuning biedt voor search-as-u-type gedrag.

+ *Suggesties* is een lijst met voorgestelde resultaten die zijn gegenereerd tijdens het typen, waarbij elke voorstel is een enkelvoudig resultaat wordt verkregen uit de index die overeenkomt met wat u tot nu toe hebt getypt. 

+ *Automatisch aanvullen*, [een preview-functie](search-api-preview.md), "voltooid" het woord of woordgroep die een gebruiker is momenteel te typen. Net als bij suggesties voor is een voltooide woord of woordgroep echter afhankelijk van een overeenkomst in de index. 

U kunt downloaden en uitvoeren van de voorbeeldcode **DotNetHowToAutocomplete** om te evalueren van deze functies. De voorbeeldcode is gericht op een vooraf gedefinieerde index die is gevuld met [NYCJobs demo gegevens](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs). De index NYCJobs bevat een [suggestie constructie](index-add-suggesters.md), dit is een vereiste voor het gebruik van suggesties of automatisch aanvullen. U kunt de voorbereide index die wordt gehost in een sandboxservice of [uw eigen-index te vullen](#configure-app) met behulp van een laadprogramma voor inventarisatiegegevens in de Voorbeeldoplossing NYCJobs. 

De **DotNetHowToAutocomplete** voorbeeld laat zien zowel suggesties en automatisch aanvullen, in beide C# en taalversies van JavaScript. C#Ontwikkelaars kunnen doorloopt u een ASP.NET MVC-toepassing die gebruikmaakt van de [Azure Search .NET SDK](https://aka.ms/search-sdk). De logica voor het maken van automatisch aanvullen en aanroepen van de voorgestelde query kan worden gevonden in het bestand HomeController.cs. JavaScript-ontwikkelaars vindt gelijkwaardige querylogica in IndexJavaScript.cshtml, waaronder direct aanroepen naar de [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/). 

Voor beide taalversies van de front-end-gebruikerservaring is gebaseerd op de [jQuery UI](https://jqueryui.com/autocomplete/) en [XDSoft](https://xdsoft.net/jqplugins/autocomplete/) bibliotheken. We gebruiken deze bibliotheken aan het bouwen van het zoekvak ondersteunen zowel suggesties en automatisch aanvullen. Invoer verzameld in het zoekvak zijn gekoppeld met suggesties en acties voor automatisch aanvullen, zoals de zoals gedefinieerd in HomeController.cs of IndexJavaScript.cshtml.

In deze oefening leidt u door de volgende taken:

> [!div class="checklist"]
> * Een zoekvak in JavaScript en probleem aanvragen voor voorgestelde overeenkomsten of autocompleted voorwaarden implementeren
> * In C#, suggesties en acties voor automatisch aanvullen in HomeController.cs definiëren
> * Aanroepen in JavaScript, de REST-API's voor dezelfde functionaliteit

## <a name="prerequisites"></a>Vereisten

Een Azure Search-service is optioneel voor deze oefening omdat de oplossing maakt gebruik van een live sandboxservice die als host fungeert voor een voorbereide NYCJobs demo-index. Als u uitvoeren in dit voorbeeld op uw eigen search-service wilt, raadpleegt u [NYC-Jobs configureren index](#configure-app) voor instructies.

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/), alle edities. Voorbeeldcode en instructies zijn getest op de gratis Community-versie.

* Download de [DotNetHowToAutoComplete voorbeeld](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete).

Het voorbeeld is uitgebreid, dekking biedt suggesties, automatisch aanvullen, meervoudige navigatie en caching aan clientzijde. U wordt aangeraden de Leesmij-bestand en opmerkingen voor een volledige beschrijving van wat het voorbeeld te bieden.

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

1. Open **AutocompleteTutorial.sln** in Visual Studio. De oplossing bevat een ASP.NET MVC-project met een verbinding met de index van de demo NYC-Jobs.

2. Druk op F5 om het project uit te voeren en de pagina in de door u gewenste browser te laden.

Bovenaan ziet u een optie voor het selecteren van C# of JavaScript. De C# optie aanroepen naar de HomeController vanuit de browser en de Azure Search .NET SDK gebruikt voor het ophalen van resultaten. 

Met de JavaScript-optie wordt de Azure Search REST-API rechtstreeks vanuit de browser aangeroepen. Met deze optie worden doorgaans opmerkelijk betere prestaties gehaald, omdat de controller ertussenuit wordt gehaald. U kunt de optie kiezen die bij uw behoeften en taalvoorkeuren past. Er bevinden zich diverse voorbeelden voor automatisch aanvullen op de pagina, met enkele richtlijnen voor elk voorbeeld. Elk voorbeeld heeft een aanbevolen voorbeeldtekst die u kunt proberen.  

Typ een paar letters in elk zoekvak om te zien wat er gebeurt.

## <a name="search-box"></a>Zoekvak

Voor beide C# en JavaScript-versies, de implementatie van het vak Zoeken is precies hetzelfde. 

Open de **Index.cshtml** bestand onder de map \Views\Home om de code weer te geven:

```html
<input class="searchBox" type="text" id="example1a" placeholder="search">
```

Dit is een eenvoudige invoertekstvak met een klasse voor stijl, een ID worden verwezen door JavaScript, en de tijdelijke aanduiding voor tekst.  De Magic-pakket is in de ingesloten JavaScript.

De C# taal voorbeeld gebruikt JavaScript in Index.cshtml gebruikmaken van de [jQuery automatisch aanvullen van UI-bibliotheek](https://jqueryui.com/autocomplete/). Deze bibliotheek wordt de ervaring voor automatisch aanvullen toegevoegd aan het zoekvak door asynchrone aanroepen naar de MVC-controller om op te halen van suggesties. De versie van de JavaScript-taal is in IndexJavaScript.cshtml. Dit omvat het volgende script voor de zoekbalk typt, evenals de REST-API-aanroepen naar Azure Search.

Bekijk de JavaScript-code voor het eerste voorbeeld, waarin jQuery automatisch aanvullen van UI-functie roept, in een aanvraag wordt doorgegeven voor suggesties:

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

De bovenstaande code wordt uitgevoerd in de browser laden van de pagina jQuery gebruikersinterface automatisch aanvullen voor het invoervak "example1a" configureren.  `minLength: 3` zorgt ervoor dat aanbevelingen alleen worden weergegeven wanneer er zich ten minste drie tekens in het zoekvak bevinden.  De bronwaarde is belangrijk:

```javascript
source: "/home/suggest?highlights=false&fuzzy=false&",
```

De bovenstaande regel weet de functie van de gebruikersinterface automatisch aanvullen jQuery waar om op te halen van de lijst met items om weer te geven onder het zoekvak. Omdat dit een MVC-project, wordt de functie voorstellen in HomeController.cs die de logica bevat voor het retourneren van Querysuggesties (informatie over suggesties in de volgende sectie). Deze functie worden ook enkele parameters doorgegeven aan het besturingselement hoogtepunten, zoeken bij benadering en termijn. Door de JavaScript-API voor automatisch aanvullen wordt de parameter 'term' toegevoegd.

### <a name="extending-the-sample-to-support-fuzzy-matching"></a>Het voorbeeld uitbreiden om ondersteuning te bieden voor zoeken bij benadering

Zoeken bij benadering zorgt ervoor dat u resultaten krijgt te zien op basis van treffers bij benadering, zelfs als de gebruiker een woord in het zoekvak onjuist heeft gespeld. Hoewel het niet vereist, het aanzienlijk wordt verbeterd de robuustheid van een typeahead-ervaring. U gaat dit uitproberen door de bronregel zo te wijzigen dat zoeken bij benadering mogelijk is.

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

### <a name="jquery-autocomplete--backed-by-azure-search-autocomplete"></a>jQuery automatisch aanvullen ondersteund door Azure Search automatisch aanvullen

De zoekopdracht UX-code is tot nu toe is gericht op de suggesties. Het volgende codeblok ziet u de jQuery automatisch aanvullen van UI-functie (regel 91 in index.cshtml), wordt doorgegeven in een aanvraag voor Azure Search automatisch aanvullen:

```javascript
$(function () {
    // using modified jQuery Autocomplete plugin v1.2.6 http://xdsoft.net/jqplugins/autocomplete/
    // $.autocomplete -> $.autocompleteInline
    $("#example2").autocompleteInline({
        appendMethod: "replace",
        source: [
            function (text, add) {
                if (!text) {
                    return;
                }

                $.getJSON("/home/autocomplete?term=" + text, function (data) {
                    if (data && data.length > 0) {
                        currentSuggestion2 = data[0];
                        add(data);
                    }
                });
            }
        ]
    });

    // complete on TAB and clear on ESC
    $("#example2").keydown(function (evt) {
        if (evt.keyCode === 9 /* TAB */ && currentSuggestion2) {
            $("#example2").val(currentSuggestion2);
            return false;
        } else if (evt.keyCode === 27 /* ESC */) {
            currentSuggestion2 = "";
            $("#example2").val("");
        }
    });
});
```

## <a name="c-version"></a>C#Versie

Nu we de JavaScript-code voor de webpagina hebt doorgenomen, laten we bekijken de C# serverzijde controllercode die daadwerkelijk haalt de voorgestelde komt overeen met de Azure Search .NET SDK gebruiken.

Open de **HomeController.cs** bestand in de map domeincontrollers. 

Het eerste wat u ziet misschien ook is een methode aan de bovenkant van de klasse met de naam `InitSearch`. Hiermee wordt een geverifieerde HTTP-indexclient voor de Azure Search-service gemaakt. Zie voor meer informatie, [over het gebruik van Azure Search via een .NET-toepassing](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk).

U ziet de suggesties-functie op regel 41. Deze is gebaseerd op de [DocumentsOperationsExtensions.Suggest methode](https://docs.microsoft.com/dotnet/api/dotnet/api/microsoft.azure.search.documentsoperationsextensions.suggest?view=azure-dotnet-preview).

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

De functie Voorstellen maakt gebruik van twee parameters die bepalen of er bij het invoeren van de zoekterm markeringen voor treffers worden geretourneerd of zoeken bij benadering wordt gebruikt. De methode maakt u een [SuggestParameters object](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggestparameters?view=azure-dotnet), die vervolgens wordt doorgegeven aan de API voorstellen. Het resultaat wordt vervolgens geconverteerd naar JSON, zodat deze in de client kan worden weergegeven.

U ziet de functie automatisch aanvullen op regel 69. Deze is gebaseerd op de [DocumentsOperationsExtensions.Autocomplete methode](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.autocomplete?view=azure-dotnet-preview).

```csharp
public ActionResult AutoComplete(string term)
{
    InitSearch();
    //Call autocomplete API and return results
    AutocompleteParameters ap = new AutocompleteParameters()
    {
        AutocompleteMode = AutocompleteMode.OneTermWithContext,
        UseFuzzyMatching = false,
        Top = 5
    };
    AutocompleteResult autocompleteResult = _indexClient.Documents.Autocomplete(term, "sg", ap);

    // Conver the Suggest results to a list that can be displayed in the client.
    List<string> autocomplete = autocompleteResult.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = autocomplete
    };
}
```

De functie automatisch aanvullen haalt de invoer van de term zoeken. De methode maakt u een [AutoCompleteParameters object](https://docs.microsoft.com/rest/api/searchservice/autocomplete). Het resultaat wordt vervolgens geconverteerd naar JSON, zodat deze in de client kan worden weergegeven.

Voeg een onderbrekingspunt toe aan het begin van de functie Voorstellen en doorloop de code. (optioneel) U ziet het antwoord geretourneerd door de SDK en hoe deze wordt geconverteerd naar het resultaat geretourneerd door de methode.

De andere voorbeelden op de pagina gaat u als volgt de hetzelfde patroon om toe te voegen treffers markeren en facetten ter ondersteuning van caching aan clientzijde van de resultaten automatisch aanvullen. Loop door elk van deze voorbeelden heen zodat u begrijpt hoe ze werken en hoe u ze kunt gebruiken in uw zoekervaring.

## <a name="javascript-version-with-rest"></a>JavaScript-versie met REST

Open voor de implementatie van JavaScript **IndexJavaScript.cshtml**. U ziet dat de jQuery automatisch aanvullen van UI-functie ook voor het zoekvak gebruikt wordt, verzamelen van search term invoer en asynchrone aanroepen naar Azure Search om op te halen komt overeen met voorgestelde of voorwaarden voltooid. 

Kijk eens naar de JavaScript-code uit het eerste voorbeeld:

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

Als u dit vergelijkt met het bovenstaande voorbeeld waarbij de begincontroller wordt aangeroepen, ziet u verschillende overeenkomsten.  De configuratie voor automatisch aanvullen voor `minLength` en `position` zijn identiek. 

De belangrijke wijziging hier is de bron. In plaats van de aanbevolen methode aanroept in de oorspronkelijke domeincontroller, een REST-aanvraag in een JavaScript-functie wordt gemaakt en uitgevoerd met behulp van Ajax. De reactie wordt vervolgens verwerkt in 'geslaagd' en als bron gebruikt.

REST-aanroepen met URI's kunt u opgeven of een [automatisch aanvullen](https://docs.microsoft.com/rest/api/searchservice/autocomplete) of [suggesties](https://docs.microsoft.com/rest/api/searchservice/suggestions) API-aanroep wordt uitgevoerd. De volgende URI's op regels 9 en 10 zijn.

```javascript
var suggestUri = "https://" + searchServiceName + ".search.windows.net/indexes/" + indexName + "/docs/suggest?api-version=" + apiVersion;
var autocompleteUri = "https://" + searchServiceName + ".search.windows.net/indexes/" + indexName + "/docs/autocomplete?api-version=" + apiVersion;
```

Op regel 148, vindt u een script dat roept de `autocompleteUri`. De eerste aanroep `suggestUri` regel 39.

> [!Note]
> REST-aanroepen naar de service maken in JavaScript wordt aangeboden als een handige demonstratie van de REST-API, maar mogen niet worden opgevat als een best practice of een aanbeveling. Het opnemen van een API-sleutel en een eindpunt in een script wordt geopend uw service tot denial of service-aanvallen voor iedereen die deze waarden uit het script kan lezen. Terwijl de veilige met JavaScript voor educatieve doeleinden, bijvoorbeeld op indexen die worden gehost op de gratis service, wordt u aangeraden Java of C# voor indexeren en query-bewerkingen in productiecode. 

<a name="configure-app"></a>

## <a name="configure-nycjobs-to-run-on-your-service"></a>NYCJobs om uit te voeren op uw service configureren

Tot nu toe hebt u de gehoste NYCJobs demo index gebruikt. Als u wilt dat volledig inzicht in alle code, met inbegrip van de index, volgt u deze instructies voor het maken en te laden van de index in uw eigen search-service.

1. [Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. U kunt een gratis service voor dit voorbeeld gebruiken. 

   > [!Note]
   > Als u de gratis Azure Search-service gebruikt, bent u beperkt tot drie indexen. De gegevenslader NYCJobs maakt twee indexen. Zorg ervoor dat uw service voldoende ruimte heeft voor de nieuwe indexen.

1. Download [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) voorbeeldcode.

1. Open in de map DataLoader van de voorbeeldcode NYCJobs **DataLoader.sln** in Visual Studio.

1. De verbindingsgegevens voor uw Azure Search-service toevoegen. Open App.config in het project DataLoader en wijzig de app-instellingen TargetSearchServiceName en TargetSearchServiceApiKey zodat deze verwijzen naar uw Azure Search-service en Azure Search Service API-sleutel. Deze vindt u in Azure Portal.

1. Druk op F5 om de toepassing, twee indexen te maken en de voorbeeldgegevens NYCJob importeren te starten.

1. Open **AutocompleteTutorial.sln** en bewerken van het bestand Web.config in de **AutocompleteTutorial** project. Wijzig de waarden SearchServiceName en SearchServiceApiKey op waarden die geldig voor uw zoekservice zijn.

1. Druk op F5 om de toepassing uit te voeren. De voorbeeld-web-app wordt geopend in de standaardbrowser. De ervaring is identiek aan de sandbox-versie, alleen de index en de gegevens worden gehost op uw service.

## <a name="next-steps"></a>Volgende stappen

In dit voorbeeld ziet u de basisstappen voor het bouwen van een zoekvak die ondersteuning biedt voor automatisch aanvullen en suggesties. Hebt u gezien hoe u kunt een ASP.NET MVC-toepassing bouwen en de Azure Search .NET SDK of REST-API gebruiken voor het ophalen van suggesties.

De volgende stap, suggesties en automatisch aanvullen integreren in uw zoekervaring probeert. De volgende artikelen voor verwijzing kunnen helpen.

> [!div class="nextstepaction"]
> [Autocomplete REST API](https://docs.microsoft.com/rest/api/searchservice/autocomplete)
> [Suggestions REST API ](https://docs.microsoft.com/rest/api/searchservice/suggestions)
> [Facets index attribute on a Create Index REST API](https://docs.microsoft.com/rest/api/searchservice/create-index) (Indexattribuut Facetten in een Create Index REST API)

