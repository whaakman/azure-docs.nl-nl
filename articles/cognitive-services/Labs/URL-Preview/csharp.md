---
title: C# Quick Start voor het voorbeeld in de Project-URL - cognitieve Microsoft-Services | Microsoft Docs
description: Aan de slag met Project URL Preview in Microsoft cognitieve Services in Azure.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/16/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 17d44bd0c23d0a1e67da5a0e91248700d3166c1a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345289"
---
# <a name="url-preview-query-in-c"></a>Voorbeeld van de URL-query in C#

Het volgende C#-voorbeeld wordt een voorbeeld-Url voor de website van SwiftKey: https://swiftkey.com/en.

## <a name="prerequisites"></a>Vereisten

U moet [Visual Studio 2017](https://www.visualstudio.com/downloads/) naar deze code in Windows worden uitgevoerd. (De editie free Community blijft werken.)

Ophalen van een toegangssleutel voor de gratis proefversie [cognitieve Services Labs](https://aka.ms/answersearchsubscription)

## <a name="code-scenario"></a>Code-scenario

De volgende C#-code maakt een voorbeeld-URL van de website van SwiftKey: https://swiftkey.com/en. 

Deze is geïmplementeerd in de volgende stappen uit:
1. Declareer de variabelen opgeven van het eindpunt en een query-URL voor de preview.  
2. Maken van de aanvraag.
3. Voeg de *Ocp-Apim-Subscription-Key* header. 
4. De webaanvraag asynchroon uitgevoerd. 
5. Het antwoord lezen.
6. De kop- en JSON-resultaten naar de console afdrukken.

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

1. Maak een nieuwe Console-oplossing in Visual Studio.
2. Vervang `Program.cs` met de opgegeven code.
3. Vervang de `YOUR-ACCESS-KEY` waarde met een geldige toegangssleutel voor uw abonnement.
4. Voer het programma.

## <a name="next-steps"></a>Volgende stappen
- [Java-Quick Start](java-quickstart.md)
- [JavaScript Quick Start](javascript.md)
- [Knooppunt Quick Start](node-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)