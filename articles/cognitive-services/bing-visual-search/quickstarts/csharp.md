---
title: C# Quick Start voor Bing Visual zoeken-API | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Laat zien hoe u een installatiekopie uploaden naar Bing Visual zoeken-API en terughalen van inzicht in de afbeelding.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: dd7531004759cdaeb59f4706dc2650d0db3c0cdb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345656"
---
# <a name="your-first-bing-visual-search-query-in-c"></a>Uw eerste Bing Visual zoekopdracht in C#

Bing Visual zoeken-API retourneert informatie over de installatiekopie die u opgeeft. Met behulp van de URL van de afbeelding, een insights token, of door een afbeelding te uploaden, kunt u de installatiekopie opgeven. Zie voor meer informatie over deze opties [wat Bing Visual zoeken-API is?](../overview.md) In dit artikel ziet u een afbeelding te uploaden. Een afbeelding te uploaden kan nuttig zijn in mobiele scenario's waarin u een afbeelding van een bekende kenmerkende en informatie over het terughalen. De insights kunnen bijvoorbeeld de algemene vragen over de kenmerkende bevatten. 

Als u een lokale installatiekopie uploadt, ziet hieronder u de gegevens dat moet u in de hoofdtekst van het bericht opnemen. De formuliergegevens moet de header Content-Disposition bevatten. De `name` parameter moet worden ingesteld op 'installatiekopie' en de `filename` parameter kan worden ingesteld op een willekeurige tekenreeks. De inhoud van het formulier is het binaire bestand van de afbeelding. De grootte van de maximale installatiekopie die u kan uploaden is 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Dit artikel bevat een eenvoudige consoletoepassing die verzendt een aanvraag Bing Visual zoeken-API en de JSON-zoekresultaten worden weergegeven. Terwijl deze toepassing is geschreven in C#, is de API een compatibel is met elke programmeertaal die kunt maken van HTTP-aanvragen en parseren van JSON RESTful-Web-service. 

Voorbeeld gebruikt alleen .NET Core klassen en wordt uitgevoerd op Windows met behulp van de .NET CLR- of Linux- of met behulp van Mac OS [Mono](http://www.mono-project.com/).


## <a name="prerequisites"></a>Vereisten

U moet [Visual Studio 2017](https://www.visualstudio.com/downloads/) deze code die wordt uitgevoerd op Windows ophalen. (De editie free Community blijft werken.)

U kunt gebruiken voor deze snelstartgids een [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonnementssleutel of een betaald abonnement-sleutel.

## <a name="running-the-application"></a>De toepassing uitvoeren

Hieronder ziet u hoe u het bericht met behulp van HttpWebRequest verzendt. Zie voor een voorbeeld waarin HttpClient HttpRequestMessage en MultipartFormDataContent [HttpClient met behulp van](#using-httpclient).

Voor het uitvoeren van deze toepassing, de volgende stappen uit:

1. Maak een nieuwe Console-oplossing in Visual Studio.
1. Vervang de inhoud van `Program.cs` met de code in deze snelstartgids wordt weergegeven.
2. Vervang de `accessKey` waarde met de abonnementssleutel van uw.
2. Vervang de `imagePath` waarde met het pad van de afbeelding te uploaden.
3. Voer het programma.


```csharp
using System;
using System.Text;
using System.Net;
using System.IO;
using System.Collections.Generic;

namespace VisualSearchUpload
{

    class Program
    {
        // **********************************************
        // *** Update and verify the following values. ***
        // **********************************************

        // Replace the accessKey string value with your valid subscription key.
        const string accessKey = "<yoursubscriptionkeygoeshere>";

        const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";

        // Set the path to the image that you want to get insights of. 
        static string imagePath = @"<pathtoimagegoeshere>";

        // Boundary strings for form data in body of POST.
        const string CRLF = "\r\n";
        static string BoundaryTemplate = "batch_{0}";
        static string StartBoundaryTemplate = "--{0}";
        static string EndBoundaryTemplate = "--{0}--";

        const string CONTENT_TYPE_HEADER_PARAMS = "multipart/form-data; boundary={0}";
        const string POST_BODY_DISPOSITION_HEADER = "Content-Disposition: form-data; name=\"image\"; filename=\"{0}\"" + CRLF +CRLF;


        static void Main()
        {
            try
            {
                Console.OutputEncoding = System.Text.Encoding.UTF8;

                if (accessKey.Length == 32)
                {
                    if (IsImagePathSet(imagePath))
                    {
                        var filename = GetImageFileName(imagePath);
                        Console.WriteLine("Getting image insights for image: " + filename);
                        var imageBinary = GetImageBinary(imagePath);

                        // Set up POST body.
                        var boundary = string.Format(BoundaryTemplate, Guid.NewGuid());
                        var startFormData = BuildFormDataStart(boundary, filename);
                        var endFormData = BuildFormDataEnd(boundary);
                        var contentTypeHdrValue = string.Format(CONTENT_TYPE_HEADER_PARAMS, boundary);

                        var json = BingImageSearch(startFormData, endFormData, imageBinary, contentTypeHdrValue);

                        Console.WriteLine("\nJSON Response:\n");
                        Console.WriteLine(JsonPrettyPrint(json));
                    }
                }
                else
                {
                    Console.WriteLine("Invalid Bing Visual Search API subscription key!");
                    Console.WriteLine("Please paste yours into the source code.");
                }

                Console.Write("\nPress Enter to exit ");
                Console.ReadLine();
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message);
            }
        }



        /// <summary>
        /// Verify that imagePath exists.
        /// </summary>
        static Boolean IsImagePathSet(string path)
        {
            if (string.IsNullOrEmpty(path))
                throw new ArgumentException("Image path is null or empty.");

            if (!File.Exists(path))
                throw new ArgumentException("Image path does not exist.");

            var size = new FileInfo(path).Length;

            if (size > 1000000)
                throw new ArgumentException("Image is greater than the 1 MB maximum size.");

            return true;
        }



        /// <summary>
        /// Get the binary characters of an image.
        /// </summary>
        static byte[] GetImageBinary(string path)
        {
            return File.ReadAllBytes(path);
        }


        /// <summary>
        /// Get the image's filename.
        /// </summary>
        static string GetImageFileName(string path)
        {
            return new FileInfo(path).Name;
        }


        /// <summary>
        /// Build the beginning part of the form data.
        /// </summary>
        static string BuildFormDataStart(string boundary, string filename)
        {
            var startBoundary = string.Format(StartBoundaryTemplate, boundary);

            var requestBody = startBoundary + CRLF;
            requestBody += string.Format(POST_BODY_DISPOSITION_HEADER, filename);

            return requestBody;
        }


        /// <summary>
        /// Build the ending part of the form data.
        /// </summary>
        static string BuildFormDataEnd(string boundary)
        {
            return CRLF + CRLF + string.Format(EndBoundaryTemplate, boundary) + CRLF;
        }



        /// <summary>
        /// Calls the Bing visual search endpoint and returns the JSON response.
        /// </summary>
        static string BingImageSearch(string startFormData, string endFormData, byte[] image, string contentTypeValue)
        {
            WebRequest request = HttpWebRequest.Create(uriBase);
            request.ContentType = contentTypeValue;
            request.Headers["Ocp-Apim-Subscription-Key"] = accessKey;
            request.Method = "POST";

            // Writes the boundary and Content-Disposition header, then writes
            // the image binary, and finishes by writing the closing boundary.
            using (Stream requestStream = request.GetRequestStream())
            {
                StreamWriter writer = new StreamWriter(requestStream);
                writer.Write(startFormData);
                writer.Flush();
                requestStream.Write(image, 0, image.Length);
                writer.Write(endFormData);
                writer.Flush();
                writer.Close();
            }


            HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
            string json = new StreamReader(response.GetResponseStream()).ReadToEnd();

            return json;
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


## <a name="using-httpclient"></a>Met behulp van HttpClient

Als u HttpClient gebruikt, kunt u MultipartFormDataContent gebruiken om de gegevens samen te stellen. Gebruik de volgende secties van de code alleen ter vervanging van de benoemde methodes die in het vorige voorbeeld.

Vervang de Main-methode met deze code:

```csharp
        static void Main()
        {
            try
            {
                Console.OutputEncoding = System.Text.Encoding.UTF8;

                if (accessKey.Length == 32)
                {
                    if (IsImagePathSet(imagePath))
                    {
                        var filename = GetImageFileName(imagePath);
                        Console.WriteLine("Getting image insights for image: " + filename);
                        var imageBinary = GetImageBinary(imagePath);

                        var boundary = string.Format(BoundaryTemplate, Guid.NewGuid());
                        var json = BingImageSearch(imageBinary, boundary, uriBase, accessKey);

                        Console.WriteLine("\nJSON Response:\n");
                        Console.WriteLine(JsonPrettyPrint(json));
                    }
                }
                else
                {
                    Console.WriteLine("Invalid Bing Visual Search API subscription key!");
                    Console.WriteLine("Please paste yours into the source code.");
                }

                Console.Write("\nPress Enter to exit ");
                Console.ReadLine();
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message);
            }
        }
```

Vervang de methode BingImageSearch met deze code:

```csharp
        /// <summary>
        /// Calls the Bing visual search endpoint and returns the JSON response.
        /// </summary>
        static string BingImageSearch(byte[] image, string boundary, string uri, string subscriptionKey)
        {
            var requestMessage = new HttpRequestMessage(HttpMethod.Post, uri);
            requestMessage.Headers.Add("Ocp-Apim-Subscription-Key", accessKey);

            var content = new MultipartFormDataContent(boundary);
            content.Add(new ByteArrayContent(image), "image", "myimage");
            requestMessage.Content = content;

            var httpClient = new HttpClient();

            Task<HttpResponseMessage> httpRequest = httpClient.SendAsync(requestMessage, HttpCompletionOption.ResponseContentRead, CancellationToken.None);
            HttpResponseMessage httpResponse = httpRequest.Result;
            HttpStatusCode statusCode = httpResponse.StatusCode;
            HttpContent responseContent = httpResponse.Content;

            string json = null;

            if (responseContent != null)
            {
                Task<String> stringContentsTask = responseContent.ReadAsStringAsync();
                json = stringContentsTask.Result;
            }

            return json;
        }
```




## <a name="next-steps"></a>Volgende stappen

[Inzichten over een afbeelding met behulp van een token inzichten verkrijgen](../use-insights-token.md)  
[Bing Visual Search één pagina app-zelfstudie](../tutorial-bing-visual-search-single-page-app.md)
[Visual zoeken Bing-overzicht](../overview.md)  
[Probeer deze](https://aka.ms/bingvisualsearchtryforfree)  
[Een gratis proefversie toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing Visual Search API-verwijzingen](https://aka.ms/bingvisualsearchreferencedoc)
