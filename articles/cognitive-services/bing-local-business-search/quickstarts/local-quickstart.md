---
title: Snelstartgids - verzenden een query naar de Bing lokale bedrijven zoeken-API in C# | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: In dit artikel gebruiken om te beginnen met de Bing lokale bedrijven zoeken-API in C#.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-local-business
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 3513ada8a911c36a31c5796214cfe35d088320b7
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52316035"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-in-c"></a>Snelstartgids: Een query verzenden naar de Bing lokale bedrijven zoeken-API inC#

Gebruik deze Quick Start om te beginnen met het verzenden van aanvragen naar de Bing lokale bedrijven zoeken-API, dit is een Cognitive Service van Azure. Terwijl deze eenvoudige toepassing is geschreven in C#, de API is een RESTful-Web-compatibel is met elke programmeertaal die HTTP-aanvragen en parseren van JSON.

In dit voorbeeld van de toepassing lokaal antwoordgegevens worden opgehaald uit de API voor de zoekquery `hotel in Bellevue`.

## <a name="prerequisites"></a>Vereisten

* Elke versie van [Visual Studio 2017](https://www.visualstudio.com/downloads/).
* Als u Linux/MacOS gebruikt, kan deze toepassing worden uitgevoerd met behulp van [Mono](http://www.mono-project.com/).

U moet beschikken over een [account voor de Cognitive Services-API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met Bing Zoeken-API's. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) is voldoende voor deze snelstartgids.  Zie ook [Cognitive Services prijsinformatie: Bing zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="create-the-request"></a>De aanvraag maken 

De volgende code maakt een `WebRequest`, stelt u de koptekst van de toegang en wordt een querytekenreeks voor 'restaurant in Bellevue' toegevoegd.  Vervolgens wordt de aanvraag verzonden en het antwoord toegewezen aan een tekenreeks die de JSON-tekst moet bevatten.

````
    // Replace the accessKey string value with your valid access key.
    const string accessKey = "enter key here";

    const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search";   

    const string searchTerm = "restaurant in Bellevue";
    // Construct the URI of the search request
    var uriQuery = uriBase + "?q=" + Uri.EscapeDataString(searchQuery) + mkt=en-us;

    // Run the Web request and get response.
    WebRequest request = HttpWebRequest.Create(uriQuery);
    request.Headers["Ocp-Apim-Subscription-Key"] = accessKey; 

    HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
    string json = new StreamReader(response.GetResponseStream()).ReadToEnd();
````

## <a name="run-the-complete-application"></a>De volledige toepassing uitvoeren

De Bing lokale bedrijven zoeken-API retourneert gelokaliseerde zoekresultaten van de Bing zoeken-engine.
1. Maak een nieuwe Console-oplossing in Visual Studio (de Community Edition volstaat).
2. Vervang Program.cs door de code hieronder.
3. De waarde accessKey vervangen door een geldige toegangssleutel voor uw abonnement.
4. Voer het programma uit.

````
using System;
using System.Collections.Generic;
using System.Text;
using System.Net;
using System.IO;

namespace localSearch
{
    class Program
    {
        // **********************************************
        // *** Update or verify the following values. ***
        // **********************************************

        // Replace the accessKey string value with your valid access key.
        const string accessKey = "enter key here";

        const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search";   

        const string searchTerm = "restaurant in Bellevue";

        // Returns search results including relevant headers
        struct SearchResult
        {
            public String jsonResult;
            public Dictionary<String, String> relevantHeaders;
        }

        static void Main()
        {
            Console.OutputEncoding = System.Text.Encoding.UTF8;
            Console.WriteLine("Searching locally for: " + searchTerm);

            SearchResult result = BingLocalSearch(searchTerm, accessKey);

            Console.WriteLine("\nRelevant HTTP Headers:\n");
            foreach (var header in result.relevantHeaders)
                Console.WriteLine(header.Key + ": " + header.Value);

            Console.WriteLine("\nJSON Response:\n");
            Console.WriteLine(JsonPrettyPrint(result.jsonResult));

            Console.Write("\nPress Enter to exit ");
            Console.ReadLine();
        }

        /// <summary>
        /// Performs a Bing Local business search and return the results as a SearchResult.
        /// </summary>
        static SearchResult BingLocalSearch(string searchQuery, string key)
        {
            // Construct the URI of the search request
            var uriQuery = uriBase + "?q=" + Uri.EscapeDataString(searchQuery) + "&mkt=en-us";

            // Perform the Web request and get the response
            WebRequest request = HttpWebRequest.Create(uriQuery);
            request.Headers["Ocp-Apim-Subscription-Key"] = key; 

            HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
            string json = new StreamReader(response.GetResponseStream()).ReadToEnd();

            // Create result object for return
            var searchResult = new SearchResult();
            searchResult.jsonResult = json;
            searchResult.relevantHeaders = new Dictionary<String, String>();

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
            int offset = 0;
            int indentLength = 3;

            foreach (char ch in json)
            {
                switch (ch)
                {
                    case '"':
                        if (!ignore) quote = !quote;
                        break;
                    case '\'':
                        if (quote) ignore = !ignore;
                        break;
                }

                if (quote)
                    sb.Append(ch);
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
                            if (ch != ' ') sb.Append(ch);
                            break;
                    }
                }
            }

            return sb.ToString().Trim();
        }
    }
}

````

## <a name="next-steps"></a>Volgende stappen
- [Lokale bedrijven zoeken Java-snelstartgids](local-search-java-quickstart.md)
- [Lokale bedrijven zoeken Node-quickstart](local-search-node-quickstart.md)
- [Lokale bedrijven zoeken Python-snelstartgids](local-search-python-quickstart.md)
