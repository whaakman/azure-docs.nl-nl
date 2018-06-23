---
title: Oproep- en antwoord - C#-Snelstartgids voor Azure cognitieve Services, zoekopdracht Bing-Web-API | Microsoft Docs
description: Get-informatie en codevoorbeelden om u te helpen snel aan de slag met de Bing Web Search-API in Microsoft cognitieve Services in Azure.
services: cognitive-services
documentationcenter: ''
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 9/18/2017
ms.author: v-jerkin
ms.openlocfilehash: 05ac1ec2bed4eb5ce658ba44688afcf50d18b41f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344758"
---
# <a name="call-and-response-your-first-bing-web-search-query-in-c"></a>Oproep- en -antwoord: uw eerste Bing Web-zoekopdracht in C#

De Bing Web zoeken-API biedt een vergelijkbaar met Bing.com/Search ervaring door te retourneren zoekresultaten die Bing bepaalt relevant zijn voor de query van de gebruiker. De resultaten kunnen webpagina's, afbeeldingen, video's, nieuws en entiteiten, samen met verwante zoekquery's, correcties tijdzones, eenheidconversie, vertalingen en berekeningen bevatten. Het soort u resultaten zijn gebaseerd op hun relevantie en de laag van de Bing zoeken-API's waarop u bent geabonneerd.

Dit artikel bevat een eenvoudige consoletoepassing die voert een query zoeken Bing-Web-API en de geretourneerde onbewerkte zoekresultaten in het JSON-indeling weergegeven. Terwijl deze toepassing is geschreven in C#, is de API een compatibel is met elke programmeertaal die kunt maken van HTTP-aanvragen en parseren van JSON RESTful-Web-service. 

Voorbeeld gebruikt alleen .NET Core klassen en wordt uitgevoerd op Windows met behulp van de .NET CLR- of Linux- of met behulp van Mac OS [Mono](http://www.mono-project.com/).

## <a name="prerequisites"></a>Vereisten

U moet [Visual Studio 2017](https://www.visualstudio.com/downloads/) deze code die wordt uitgevoerd op Windows ophalen. (De editie free Community blijft werken.)

U moet hebben een [cognitieve Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Bing zoeken-API's**. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) voldoende is voor deze snelstartgids. U moet de toegangssleutel die is opgegeven bij het activeren van uw gratis proefversie of u kunt de sleutel van een betaald abonnement van uw Azure-dashboard.

## <a name="running-the-application"></a>De toepassing uitvoeren

Volg deze stappen voor het uitvoeren van deze toepassing.

1. Maak een nieuwe Console-oplossing in Visual Studio.
2. Vervang `Program.cs` met de opgegeven code.
3. Vervang de `accessKey` waarde met een geldige toegangssleutel voor uw abonnement.
4. Voer het programma.

```csharp
using System;
using System.Text;
using System.Net;
using System.IO;
using System.Collections.Generic;

namespace BingSearchApisQuickstart
{

    class Program
    {
        // **********************************************
        // *** Update or verify the following values. ***
        // **********************************************

        // Replace the accessKey string value with your valid access key.
        const string accessKey = "enter key here";

        // Verify the endpoint URI.  At this writing, only one endpoint is used for Bing
        // search APIs.  In the future, regional endpoints may be available.  If you
        // encounter unexpected authorization errors, double-check this value against
        // the endpoint for your Bing Web search instance in your Azure dashboard.
        const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/search";

        const string searchTerm = "Microsoft Cognitive Services";

        // Used to return search results including relevant headers
        struct SearchResult
        {
            public String jsonResult;
            public Dictionary<String, String> relevantHeaders;
        }

        static void Main()
        {
            Console.OutputEncoding = System.Text.Encoding.UTF8;

            if (accessKey.Length == 32)
            {
                Console.WriteLine("Searching the Web for: " + searchTerm);
    
                SearchResult result = BingWebSearch(searchTerm);
    
                Console.WriteLine("\nRelevant HTTP Headers:\n");
                foreach (var header in result.relevantHeaders)
                    Console.WriteLine(header.Key + ": " + header.Value);
    
                Console.WriteLine("\nJSON Response:\n");
                Console.WriteLine(JsonPrettyPrint(result.jsonResult));
            }
            else
            {
                Console.WriteLine("Invalid Bing Search API subscription key!");
                Console.WriteLine("Please paste yours into the source code.");
            }
    
            Console.Write("\nPress Enter to exit ");
            Console.ReadLine();
        }

        /// <summary>
        /// Performs a Bing Web search and return the results as a SearchResult.
        /// </summary>
        static SearchResult BingWebSearch(string searchQuery)
        {
            // Construct the URI of the search request
            var uriQuery = uriBase + "?q=" + Uri.EscapeDataString(searchQuery);

            // Perform the Web request and get the response
            WebRequest request = HttpWebRequest.Create(uriQuery);
            request.Headers["Ocp-Apim-Subscription-Key"] = accessKey;
            HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
            string json = new StreamReader(response.GetResponseStream()).ReadToEnd();

            // Create result object for return
            var searchResult = new SearchResult()
            {
                jsonResult = json,
                relevantHeaders = new Dictionary<String, String>()
            };

            // Extract Bing HTTP headers
            foreach (String header in response.Headers)
            {
                if (header.StartsWith("BingAPIs-") || header.StartsWith("X-MSEdge-"))
                    searchResult.relevantHeaders[header] = response.Headers[header];
            }

            return searchResult;
        }

        /// <summary>
        /// Formats the given JSON string by adding line breaks and indents.
        /// </summary>
        /// <param name="json">The raw JSON string to format.</param>
        /// <returns>The formatted JSON string.</returns>
        static string JsonPrettyPrint(string json)
        {
            if (string.IsNullOrEmpty(json))
                return string.Empty;

            json = json.Replace(Environment.NewLine, "").Replace("\t", "");

            StringBuilder sb = new StringBuilder();
            bool quote = false;
            bool ignore = false;
            char last = ' ';
            int offset = 0;
            int indentLength = 2;

            foreach (char ch in json)
            {
                switch (ch)
                {
                    case '"':
                        if (!ignore) quote = !quote;
                        break;
                    case '\\':
                        if (quote && last != '\\') ignore = true;
                        break;
                }

                if (quote)
                {
                    sb.Append(ch);
                    if (last == '\\' && ignore) ignore = false;
                }
                else
                {
                    switch (ch)
                    {
                        case '{':
                        case '[':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', ++offset * indentLength));
                            break;
                        case '}':
                        case ']':
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', --offset * indentLength));
                            sb.Append(ch);
                            break;
                        case ',':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', offset * indentLength));
                            break;
                        case ':':
                            sb.Append(ch);
                            sb.Append(' ');
                            break;
                        default:
                            if (quote || ch != ' ') sb.Append(ch);
                            break;
                    }
                }
                last = ch;
            }

            return sb.ToString().Trim();
        }
    }
}
```

## <a name="json-response"></a>JSON-antwoord

Er volgt een voorbeeldantwoord. Als u wilt beperken de lengte van de JSON, alleen een enkelvoudig resultaat wordt verkregen wordt weergegeven en andere onderdelen van het antwoord is afgekapt. 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "Microsoft Cognitive Services"
  },
  "webPages": {
    "webSearchUrl": "https://www.bing.com/search?q=Microsoft+cognitive+services",
    "totalEstimatedMatches": 22300000,
    "value": [
      {
        "id": "https://api.cognitive.microsoft.com/api/v7/#WebPages.0",
        "name": "Microsoft Cognitive Services",
        "url": "https://www.microsoft.com/cognitive-services",
        "displayUrl": "https://www.microsoft.com/cognitive-services",
        "snippet": "Knock down barriers between you and your ideas. Enable natural and contextual interaction with tools that augment users' experiences via the power of machine-based AI. Plug them in and bring your ideas to life.",
        "deepLinks": [
          {
            "name": "Face API",
            "url": "https://azure.microsoft.com/services/cognitive-services/face/",
            "snippet": "Add facial recognition to your applications to detect, identify, and verify faces using a Face API from Microsoft Azure. ... Cognitive Services; Face API;"
          },
          {
            "name": "Text Analytics",
            "url": "https://azure.microsoft.com/services/cognitive-services/text-analytics/",
            "snippet": "Cognitive Services; Text Analytics API; Text Analytics API . Detect sentiment, ... you agree that Microsoft may store it and use it to improve Microsoft services, ..."
          },
          {
            "name": "Computer Vision API",
            "url": "https://azure.microsoft.com/services/cognitive-services/computer-vision/",
            "snippet": "Extract the data you need from images using optical character recognition and image analytics with Computer Vision APIs from Microsoft Azure."
          },
          {
            "name": "Emotion",
            "url": "https://www.microsoft.com/cognitive-services/en-us/emotion-api",
            "snippet": "Cognitive Services Emotion API - microsoft.com"
          },
          {
            "name": "Bing Speech API",
            "url": "https://azure.microsoft.com/services/cognitive-services/speech/",
            "snippet": "Add speech recognition to your applications, including text to speech, with a speech API from Microsoft Azure. ... Cognitive Services; Bing Speech API;"
          },
          {
            "name": "Get Started for Free",
            "url": "https://azure.microsoft.com/services/cognitive-services/",
            "snippet": "Add vision, speech, language, and knowledge capabilities to your applications using intelligence APIs and SDKs from Cognitive Services."
          }
        ]
      }
    ]
  },
  "relatedSearches": {
    "id": "https://api.cognitive.microsoft.com/api/v7/#RelatedSearches",
    "value": [
      {
        "text": "microsoft bot framework",
        "displayText": "microsoft bot framework",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+bot+framework"
      },
      {
        "text": "microsoft cognitive services youtube",
        "displayText": "microsoft cognitive services youtube",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+youtube"
      },
      {
        "text": "microsoft cognitive services search api",
        "displayText": "microsoft cognitive services search api",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+search+api"
      },
      {
        "text": "microsoft cognitive services news",
        "displayText": "microsoft cognitive services news",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+news"
      },
      {
        "text": "ms cognitive service",
        "displayText": "ms cognitive service",
        "webSearchUrl": "https://www.bing.com/search?q=ms+cognitive+service"
      },
      {
        "text": "microsoft cognitive services text analytics",
        "displayText": "microsoft cognitive services text analytics",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+text+analytics"
      },
      {
        "text": "microsoft cognitive services toolkit",
        "displayText": "microsoft cognitive services toolkit",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+toolkit"
      },
      {
        "text": "microsoft cognitive services api",
        "displayText": "microsoft cognitive services api",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+api"
      }
    ]
  },
  "rankingResponse": {
    "mainline": {
      "items": [
        {
          "answerType": "WebPages",
          "resultIndex": 0,
          "value": {
            "id": "https://api.cognitive.microsoft.com/api/v7/#WebPages.0"
          }
        }
      ]
    },
    "sidebar": {
      "items": [
        {
          "answerType": "RelatedSearches",
          "value": {
            "id": "https://api.cognitive.microsoft.com/api/v7/#RelatedSearches"
          }
        }
      ]
    }
  }
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Bing Web search één pagina app-zelfstudie](../tutorial-bing-web-search-single-page-app.md)

## <a name="see-also"></a>Zie ook 

[Overzicht van Bing webpagina's zoeken](../overview.md)  
[Probeer deze](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/)  
[Een gratis proefversie toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api)  
[Bing Web Search API-verwijzingen](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
