---
title: Met behulp van de positie om weer te geven van zoekresultaten
titleSuffix: Azure Cognitive Services
description: Ziet u hoe u het antwoord Bing RankingResponse zoekresultaten in positie volgorde weergeven.
services: cognitive-services
author: bradumbaugh
manager: cgronlun
ms.assetid: 2575A80C-FC74-4631-AE5D-8101CF2591D3
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 05/08/2017
ms.author: bking
ms.openlocfilehash: 90ad2ff87e9541c892262edb2e48366826b2a388
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55188436"
---
# <a name="build-a-console-app-search-client-in-c"></a>Bouw een console-client-app zoeken in C#

In deze zelfstudie laat zien hoe een eenvoudige .NET Core-consoletoepassing waarmee gebruikers de Bing webzoekopdrachten-API opvragen en weergeven van gerangschikte resultaten kan bouwen.

Deze zelfstudie laat zien hoe u:

- Een eenvoudige query maken die naar de Bing webzoekopdrachten-API
- Queryresultaten in gerangschikte volgorde weergeven

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen, samen met de zelfstudie, hebt u het volgende nodig:

- Visual Studio. Als u geen, [download en installeer de gratis Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).
- De abonnementssleutel van een voor de Bing webzoekopdrachten-API. Als u geen sleutel hebt, kunt u zich [registreren voor een gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).

## <a name="create-a-new-console-app-project"></a>Een nieuw Console-App-project maken

Maak in Visual Studio een project met `Ctrl`+`Shift`+`N`.

In de **nieuw Project** dialoogvenster, klikt u op **Visual C# > Windows Classic Desktop > consoletoepassing (.NET Framework)**.

Noem de toepassing **MyConsoleSearchApp**, en klik vervolgens op **OK**.

## <a name="add-the-jsonnet-nuget-package-to-the-project"></a>JSON.net Nuget-pakket toevoegen aan het project

JSON.net kunt u werken met de JSON-antwoord geretourneerd door de API. Voeg de NuGet-pakket toe aan uw project:

- In **Solution Explorer** met de rechtermuisknop op het project en selecteer **NuGet-pakketten beheren...** .
- Op de **Bladeren** tabblad, zoek `Newtonsoft.Json`. Selecteer de meest recente versie en klik vervolgens op **installeren**.
- Klik op de **OK** knop op de **wijzigingen controleren** venster.
- Sluit de Visual Studio-tabblad met de titel **NuGet: MyConsoleSearchApp**.

## <a name="add-a-reference-to-systemweb"></a>Voeg een verwijzing naar System.Web

In deze zelfstudie is afhankelijk van de `System.Web` assembly. Voeg een verwijzing naar deze assembly toe aan uw project:

- In **Solution Explorer**, met de rechtermuisknop op **verwijzingen** en selecteer **verwijzing toevoegen...**
- Selecteer **assembly's > Framework**, schuif omlaag en controleren **System.Web**
- Selecteer **OK**

## <a name="add-some-necessary-using-statements"></a>Sommige vereiste using-instructies toevoegen

De code in deze zelfstudie vereist drie extra met behulp van instructies. Toevoegen van deze instructies onder de bestaande `using` instructies aan het begin van **Program.cs**:

```csharp
using System.Web;
using System.Net.Http;
```

## <a name="ask-the-user-for-a-query"></a>De gebruiker vragen voor een query

In **Solution Explorer**Open **Program.cs**. Update de `Main()` methode:

```csharp
static void Main()
{
    // Get the user's query
    Console.Write("Enter Bing query: ");
    string userQuery = Console.ReadLine();
    Console.WriteLine();

    // Run the query and display the results
    RunQueryAndDisplayResults(userQuery);

    // Prevent the console window from closing immediately
    Console.WriteLine("\nHit ENTER to exit...");
    Console.ReadLine();
}
```

Deze methode:

- De gebruiker vraagt om een query
- Aanroepen `RunQueryAndDisplayResults(userQuery)` uit te voeren van de query en de resultaten weergeven
- Wacht op invoer van de gebruiker om te voorkomen dat het consolevenster onmiddellijk wordt gesloten.

## <a name="search-for-query-results-using-the-bing-web-search-api"></a>Zoeken naar de resultaten van de query met behulp van de Bing webzoekopdrachten-API

Voeg vervolgens de methode waarmee query's van de API en de resultaten worden weergegeven:

```csharp
static void RunQueryAndDisplayResults(string userQuery)
{
    try
    {
        // Create a query
        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "<YOUR_SUBSCRIPTION_KEY_GOES_HERE>");
        var queryString = HttpUtility.ParseQueryString(string.Empty);
        queryString["q"] = userQuery;
        var query = "https://api.cognitive.microsoft.com/bing/v7.0/search?" + queryString;

        // Run the query
        HttpResponseMessage httpResponseMessage = client.GetAsync(query).Result;

        // Deserialize the response content
        var responseContentString = httpResponseMessage.Content.ReadAsStringAsync().Result;
        Newtonsoft.Json.Linq.JObject responseObjects = Newtonsoft.Json.Linq.JObject.Parse(responseContentString);

        // Handle success and error codes
        if (httpResponseMessage.IsSuccessStatusCode)
        {
            DisplayAllRankedResults(responseObjects);
        }
        else
        {
            Console.WriteLine($"HTTP error status code: {httpResponseMessage.StatusCode.ToString()}");
        }
    }
    catch (Exception e)
    {
        Console.WriteLine(e.Message);
    }
}
```

Deze methode:

- Hiermee maakt u een `HttpClient` query uitvoeren op de webzoekopdrachten-API
- Stelt de `Ocp-Apim-Subscription-Key` HTTP-header die Bing gebruikt voor het verifiëren van de aanvraag
- De aanvraag wordt uitgevoerd en maakt gebruik van JSON.net deserialiseren van de resultaten
- Aanroepen `DisplayAllRankedResults(responseObjects)` om alle resultaten in gerangschikte volgorde weer te geven

Zorg ervoor dat u de waarde van `Ocp-Apim-Subscription-Key` naar de abonnementssleutel van uw.

## <a name="display-ranked-results"></a>Gerangschikte resultaten weergeven

Voordat u die laat zien hoe u de resultaten weergeven in gerangschikte volgorde, kijk eens een voorbeeldantwoord voor web zoeken:

```json
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346...",
        "totalEstimatedMatches" : 982000,
        "value" : [{
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0",
            "name" : "Contoso Sailing Club - Seattle",
            "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FE...",
            "displayUrl" : "https:\/\/contososailingsea...",
            "snippet" : "Come sail with Contoso in Seattle...",
            "dateLastCrawled" : "2017-04-07T02:25:00"
        },
        {
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/7\/#WebPages.6",
            "name" : "Contoso Sailing Lessons - Official Site",
            "url" : "http:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FE...",
            "displayUrl" : "https:\/\/www.constososailinglessonsseat...",
            "snippet" : "Contoso sailing lessons in Seattle...",
            "dateLastCrawled" : "2017-04-09T14:30:00"
        },

        ...

        ],
        "someResultsRemoved" : true
    },
    "relatedSearches" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/7\/#RelatedSearches",
        "value" : [{
            "text" : "sailing lessons",
            "displayText" : "sailing lessons",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346E..."
        }

        ...

        ]
    },
    "rankingResponse" : {
        "mainline" : {
            "items" : [{
                "answerType" : "WebPages",
                "resultIndex" : 0,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 1,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.1"
                }
            }

            ...

            ]
        },
        "sidebar" : {
            "items" : [{
                "answerType" : "RelatedSearches",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#RelatedSearches"
                }
            }]
        }
    }
}
```

De `rankingResponse` JSON-object ([documentatie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse)) beschrijft de juiste volgorde voor zoekresultaten. Het bevat een of meer van de volgende, geprioriteerde groepen:

- `pole`: De lijst met zoekresultaten om op te halen van de meest zichtbaar behandeling (bijvoorbeeld weergegeven boven de hoofdlijnen en zijbalk).
- `mainline`: De lijst met zoekresultaten om weer te geven in de hoofdlijnen.
- `sidebar`: De lijst met zoekresultaten om weer te geven in de zijbalk. Als er geen zijbalk, moet u de resultaten hieronder de hoofdlijnen weergeven.

Het antwoord rangorde JSON, omvat mogelijk een of meer van de groepen.

In **Program.cs**, voegt u de volgende methode om resultaten in goed gerangschikte volgorde weer te geven:

```csharp
static void DisplayAllRankedResults(Newtonsoft.Json.Linq.JObject responseObjects)
{
    string[] rankingGroups = new string[] { "pole", "mainline", "sidebar" };

    // Loop through the ranking groups in priority order
    foreach (string rankingName in rankingGroups)
    {
        Newtonsoft.Json.Linq.JToken rankingResponseItems = responseObjects.SelectToken($"rankingResponse.{rankingName}.items");
        if (rankingResponseItems != null)
        {
            foreach (Newtonsoft.Json.Linq.JObject rankingResponseItem in rankingResponseItems)
            {
                Newtonsoft.Json.Linq.JToken resultIndex;
                rankingResponseItem.TryGetValue("resultIndex", out resultIndex);
                var answerType = rankingResponseItem.Value<string>("answerType");
                switch (answerType)
                {
                    case "WebPages":
                        DisplaySpecificResults(resultIndex, responseObjects.SelectToken("webPages.value"), "WebPage", "name", "url", "displayUrl", "snippet");
                        break;
                    case "News":
                        DisplaySpecificResults(resultIndex, responseObjects.SelectToken("news.value"), "News", "name", "url", "description");
                        break;
                    case "Images":
                        DisplaySpecificResults(resultIndex, responseObjects.SelectToken("images.value"), "Image", "thumbnailUrl");
                        break;
                    case "Videos":
                        DisplaySpecificResults(resultIndex, responseObjects.SelectToken("videos.value"), "Video", "embedHtml");
                        break;
                    case "RelatedSearches":
                        DisplaySpecificResults(resultIndex, responseObjects.SelectToken("relatedSearches.value"), "RelatedSearch", "displayText", "webSearchUrl");
                        break;
                }
            }
        }
    }
}
```

Deze methode:

- Wordt uitgevoerd via de `rankingResponse` groepen die het antwoord bevat
- De items in elke groep worden weergegeven door aan te roepen `DisplaySpecificResults(...)`

In **Program.cs**, voeg de volgende twee methoden toe:

```csharp
static void DisplaySpecificResults(Newtonsoft.Json.Linq.JToken resultIndex, Newtonsoft.Json.Linq.JToken items, string title, params string[] fields)
{
    if (resultIndex == null)
    {
        foreach (Newtonsoft.Json.Linq.JToken item in items)
        {
            DisplayItem(item, title, fields);
        }
    }
    else
    {
        DisplayItem(items.ElementAt((int)resultIndex), title, fields);
    }
}

static void DisplayItem(Newtonsoft.Json.Linq.JToken item, string title, string[] fields)
{
    Console.WriteLine($"{title}: ");
    foreach( string field in fields )
    {
        Console.WriteLine($"- {field}: {item[field]}");
    }
    Console.WriteLine();
}
```

Deze methoden werken samen om de zoekresultaten in de console-uitvoer.

## <a name="run-the-application"></a>De toepassing uitvoeren

Voer de toepassing uit. De uitvoer moet er als volgt uitzien:

```
Enter Bing query: sailing lessons seattle

WebPage:
- name: Contoso Sailing Club - Seattle
- url: https://www.bing.com/cr?IG=70BE289346ED4594874FE...
- displayUrl: https://contososailingsea....
- snippet: Come sail with Contoso in Seattle...

WebPage:
- name: Contoso Sailing Lessons Seattle - Official Site
- url: http://www.bing.com/cr?IG=70BE289346ED4594874FE...
- displayUrl: https://www.constososailinglessonsseat...
- snippet: Contoso sailing lessons in Seattle...

...
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [met behulp van trefwoordenrangschikking om weer te geven resultaten](rank-results.md).
