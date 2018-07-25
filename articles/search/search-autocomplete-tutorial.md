---
title: Zelfstudie voor het toevoegen van automatisch aanvullen aan het zoekvak met Azure Search | Microsoft Docs
description: Voorbeelden van hoe u voor het verbeteren van de eindgebruikerservaring van uw gegevensgerichte toepassingen met behulp van Azure Search-API's voor automatisch aanvullen en het bieden van suggesties.
manager: pablocas
author: mrcarter8
services: search
ms.service: search
ms.devlang: NA
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: mcarter
ms.openlocfilehash: 7120080bfdc188c150c7065e1c0639ab8c04f173
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989672"
---
# <a name="tutorial-add-auto-complete-to-your-search-box-using-azure-search"></a>Zelfstudie: Automatisch aanvullen aan het zoekvak toevoegen met Azure Search

Tijdens deze zelfstudie leert u hoe u met behulp van [suggesties](https://docs.microsoft.com/rest/api/searchservice/suggestions), [automatisch aanvullen](https://docs.microsoft.com/en-us/rest/api/searchservice/autocomplete) en [facetten](search-faceted-navigation.md) in de [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/) en [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions?view=azure-dotnet) een zeer efficiënt zoekvak kunt bouwen. *Suggesties* biedt aanbevelingen van werkelijke resultaten op basis van wat de gebruiker tot dusverre heeft getypt. *Automatisch aanvullen* is [een nieuwe preview-functie](search-api-preview.md) in Azure Search die termen aanbiedt die afkomstig zijn uit de index, om aan te vullen wat de gebruiker op dat moment typt. Er worden meerdere technieken met elkaar vergeleken die de productiviteit van gebruikers verbeteren, waardoor deze in staat zijn om snel en eenvoudig te vinden waar ze naar op zoek zijn. Om dit te bereiken wordt de geavanceerde functionaliteit van Search rechtstreeks ter beschikking gesteld aan gebruikers terwijl ze typen.

In deze zelfstudie wordt u stapsgewijs uitgelegd hoe een ASP.NET MVC-toepassing werkt die gebruikmaakt van C# om de [Azure Search .NET-clientbibliotheken](https://aka.ms/search-sdk) aan te roepen, en van JavaScript gebruikmaakt om de Azure Search REST-API rechtstreeks aan te roepen. De toepassing voor deze zelfstudie richt zich op een index met de [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs)-voorbeeldgegevens. U kunt de index gebruiken die eerder al is geconfigureerd in de demoversie van NYC Jobs, of u kunt uw eigen index van gegevens voorzien met behulp van een gegevenslader in de voorbeeldtoepassing NYCJobs. In het voorbeeld worden de [jQuery UI](https://jqueryui.com/autocomplete/) en [XDSoft](https://xdsoft.net/jqplugins/autocomplete/) JavaScript-bibliotheken gebruikt om een zoekvak te bouwen dat het gebruik van automatisch aanvullen ondersteunt. Door deze onderdelen in combinatie met Azure Search te gebruiken, krijgt u meerdere voorbeelden te zien van hoe u ervoor kunt zorgen dat er voor automatisch aanvullen ondersteuning wordt geboden via aangevulde suggesties.

U gaat de volgende taken uitvoeren:

> [!div class="checklist"]
> * De oplossing downloaden en configureren
> * Informatie over de zoekservice toevoegen aan toepassingsinstellingen
> * Een invoervak voor de zoekfunctie implementeren
> * Ondersteuning toevoegen voor het gebruik van een lijst voor automatisch aanvullen die gegevens uit een externe bron ophaalt 
> * Suggesties en automatisch aanvullen ophalen met de .Net SDK en REST-API
> * Ondersteuning bieden voor caching aan clientzijde om de prestaties te verbeteren 

## <a name="prerequisites"></a>Vereisten

* Visual Studio 2017. U kunt de gratis [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) gebruiken. 

* Download de voorbeeld[broncode](https://github.com/azure-samples/search-dotnet-getting-started) voor de zelfstudie.

* Een actief Azure-account en een Azure Search-service. (optioneel) Als u nog geen Azure-account hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/free/). Zie [Een zoekservice maken](search-create-service-portal.md) voor hulp bij het inrichten van de service. Het account en de service zijn optioneel omdat deze zelfstudie kan worden voltooid met een gehoste NYCJobs-index die al aanwezig is in verband met een andere demo.

* Download de voorbeeldcode [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) om de gegevens van NYCJobs te importeren in een index van uw eigen Azure Search-service. (optioneel)

> [!Note]
> Als u de gratis Azure Search-service gebruikt, bent u beperkt tot drie indexen. De gegevenslader NYCJobs maakt twee indexen. Zorg ervoor dat uw service voldoende ruimte heeft voor de nieuwe indexen.

### <a name="set-up-azure-search-optional"></a>Azure Search instellen (optioneel)

Volg de stappen in deze sectie als u de gegevens voor de voorbeeldtoepassing NYCJobs in uw eigen index wilt importeren. Deze stap is optioneel.  Als u de meegeleverde voorbeeldindex wilt gebruiken, kunt u verdergaan met de volgende sectie waarin het voorbeeld wordt uitgevoerd.

1. Open in Visual Studio het oplossingsbestand DataLoader.sln in de map DataLoader van de NYCJobs-voorbeeldcode.

1. Werk de verbindingsgegevens voor uw Azure Search-service bij.  Open App.config in het project DataLoader en wijzig de app-instellingen TargetSearchServiceName en TargetSearchServiceApiKey zodat deze verwijzen naar uw Azure Search-service en Azure Search Service API-sleutel.  Deze vindt u in Azure Portal.

1. Druk op F5 om de toepassing starten.  Nu worden twee indexen gemaakt en de NYCJob-voorbeeldgegevens geïmporteerd.

1. Open in de voorbeeldcode van de zelfstudie het oplossingsbestand AutocompleteTutorial.sln in Visual Studio.  Open Web.config in het project AutocompleteTutorial en wijzig de waarden SearchServiceName en SearchServiceApiKey in dezelfde waarden als hierboven.

### <a name="running-the-sample"></a>Het voorbeeld uitvoeren

U kunt nu de voorbeeldtoepassingen van de zelfstudie gaan uitvoeren.  Open het oplossingsbestand AutocompleteTutorial.sln in Visual Studio om de zelfstudie uit te voeren.  De oplossing bevat een ASP.NET MVC-project.  Druk op F5 om het project uit te voeren en de pagina in de door u gewenste browser te laden.  Bovenaan ziet u een optie voor het selecteren van C# of JavaScript.  De optie C# roept de HomeController aan vanuit de browser en gebruikt de Azure Search .Net SDK om resultaten op te halen.  Met de JavaScript-optie wordt de Azure Search REST-API rechtstreeks vanuit de browser aangeroepen.  Deze optie worden doorgaans opmerkelijk betere prestaties gehaald omdat controller ertussenuit wordt gehaald.  U kunt de optie kiezen die bij uw behoeften en taalvoorkeuren past.  Er bevinden zich voorbeelden voor automatisch aanvullen op de pagina, met enkele richtlijnen voor elk.  Elk voorbeeld heeft een aanbevolen voorbeeldtekst die u kunt proberen.  Typ een paar letters in elk zoekvak om te zien wat er gebeurt.

## <a name="how-this-works-in-code"></a>Hoe dit met code werkt

Nu dat u de voorbeelden in de browser in werking hebt gezien, wordt stapsgewijs en in detail het eerste voorbeeld bekeken om te zien welke onderdelen hierbij een rol spelen en welke rol dit is.

### <a name="search-box"></a>Zoekvak

Voor elke taal is het zoekvak precies hetzelfde.  Open het bestand Index.cshtml in de map \Views\Home. Het zoekvak zelf is eenvoudig:

```html
<input class="searchBox" type="text" id="example1a" placeholder="search">
```

Het is een eenvoudig vak voor het invoeren van tekst met een klasse voor stijlaanpassingen, een id waar door JavaScript naar wordt verwezen en een tijdelijke aanduiding voor tekst.  Het geheim schuilt in het JavaScript.

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

Deze code wordt tijdens het laden van een pagina in de browser uitgevoerd om automatisch aanvullen voor het invoervak 'example1a' te configureren.  `minLength: 3` zorgt ervoor dat aanbevelingen alleen worden weergegeven wanneer er zich ten minste drie tekens in het zoekvak bevinden.  De bronwaarde is belangrijk:

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

1. Het eerste wat u mogelijk zal opvallen is een methode met de naam InitSearch die zich boven in de klasse bevindt.  Hiermee wordt een geverifieerde HTTP-indexclient voor de Azure Search-service gemaakt.  Als u meer informatie wilt over hoe dit werkt, raadpleegt u de volgende zelfstudie:[Weten hoe u Azure Search gebruikt via een .NET-toepassing](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

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

In de andere voorbeelden op de pagina wordt hetzelfde patroon gevolgd om het markeren van treffers, het geven van aanbevelingen voor automatisch aanvullen bij typen en facetten toe te voegen ter ondersteuning van caching aan clientzijde van de resultaten voor automatisch aanvullen.  Loop door elk van deze voorbeelden heen zodat u begrijpt hoe ze werken en hoe u ze kunt gebruiken in uw zoekervaring.

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

Als u dit vergelijkt met het bovenstaande voorbeeld waarbij de begincontroller wordt aangeroepen, ziet u verschillende overeenkomsten.  De configuratie voor automatisch aanvullen voor `minLength` en `position` zijn identiek.  De belangrijke wijziging hier is de bron.  In plaats van dat de methode Suggest wordt aangeroepen in de begincontroller, wordt er een REST-aanvraag gemaakt in een JavaScript-functie en wordt deze uitgevoerd met behulp van AJAX.  De reactie wordt vervolgens verwerkt in 'geslaagd' en als bron gebruikt.

## <a name="takeaways"></a>Opgedane kennis

In deze zelfstudie worden basisstappen gedemonstreerd voor het bouwen van een zoekvak dat ondersteuning biedt voor automatisch aanvullen en suggesties.  U hebt gezien hoe u een ASP.NET MVC-toepassing kunt bouwen en de Azure Search .Net SDK of REST-API kunt gebruiken voor het ophalen van suggesties.

## <a name="next-steps"></a>Volgende stappen

Suggesties en automatisch aanvullen in uw zoekervaring integreren.  Bedenk hoe de .Net SDK of de REST-API u direct kan helpen om het potentieel van Azure Search ter beschikking te stellen aan uw gebruikers terwijl ze typen, zodat ze nog productiever worden.

> [!div class="nextstepaction"]
> [Autocomplete REST API](https://docs.microsoft.com/en-us/rest/api/searchservice/autocomplete)
> [Suggestions REST API ](https://docs.microsoft.com/en-us/rest/api/searchservice/suggestions)
> [Facets index attribute on a Create Index REST API](https://docs.microsoft.com/en-us/rest/api/searchservice/create-index) (Indexattribuut Facetten in een Create Index REST API)

