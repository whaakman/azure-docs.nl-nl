---
title: "Snelstart: preview van project-URL's, C#"
titlesuffix: Azure Cognitive Services
description: Aan de slag met previews van Project-URL's met C#.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: url-preview
ms.topic: quickstart
ms.date: 03/16/2018
ms.author: rosh
ms.openlocfilehash: 93dd376d94065c0e99dffe53bbfa2763d0a25b2c
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49470622"
---
# <a name="quickstart-url-preview-query-in-c"></a>Snelstart: query voor URL-previews in C#

In het volgende C#-voorbeeld wordt een URL-preview gemaakt voor de SwiftKey-website: https://swiftkey.com/en.

## <a name="prerequisites"></a>Vereisten

U hebt [Visual Studio 2017](https://www.visualstudio.com/downloads/) nodig om deze code uit te voeren in Windows. (De gratis Community-editie volstaat.)

Vraag een toegangssleutel aan voor de gratis proefversie van [Cognitive Services Labs](https://aka.ms/answersearchsubscription)

## <a name="code-scenario"></a>Codescenario

Met de volgende C#-code wordt een URL-preview gemaakt voor de SwiftKey-website: https://swiftkey.com/en. 

De code wordt in de volgende stappen geïmplementeerd:
1. Declareer variabelen om het eindpunt op te geven en een query-URL om als voorbeeld weer te geven.  
2. Maak de aanvraag.
3. Voeg de header *Ocp-Apim-Subscription-Key* toe. 
4. Voer de webaanvraag asynchroon uit. 
5. Lees het antwoord.
6. Kopieer de headers en JSON-resultaten naar de console.

**Broncode**

```
using System;
using System.IO;
using System.Net;
using System.Text;

namespace UrlPrevCshp
{
    class Program
    {
        static void Main(string[] args)
        {
            String uriBase = "https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search";
            String searchQuery = "https://swiftkey.com/en"; 
            var uriQuery = uriBase + "?q=" + Uri.EscapeDataString(searchQuery);

            // Do the Web request and get response
            WebRequest request = HttpWebRequest.Create(uriQuery);
            request.Headers["Ocp-Apim-Subscription-Key"] = "YOUR-SUBSCRIPTION-KEY";
            HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
            string json = new StreamReader(response.GetResponseStream()).ReadToEnd();

            Console.WriteLine("\nHTTP Headers:\n");
            foreach (String header in response.Headers)
            {
                if (header.StartsWith("BingAPIs-") || header.StartsWith("X-MSEdge-"))
                    Console.WriteLine(header + ": " + response.Headers[header]);
            }

            Console.WriteLine(JsonPrettyPrint(json));
            

            Console.ReadKey();
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
## <a name="running-the-application"></a>De toepassing uitvoeren

De toepassing uitvoeren:

1. Maak een nieuwe console-oplossing in Visual Studio.
2. Vervang `Program.cs` door de opgegeven code.
3. Vervang de waarde `YOUR-ACCESS-KEY` door een geldige toegangssleutel voor uw abonnement.
4. Voer het programma uit.

## <a name="next-steps"></a>Volgende stappen
- [Snelstart voor Java](java-quickstart.md)
- [Snelstart voor JavaScript](javascript.md)
- [Snelstart voor Node](node-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)